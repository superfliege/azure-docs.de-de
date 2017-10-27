---
title: ".NET SDK: Kontoverwaltungsvorgänge in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie das Azure Data Lake Store .NET SDK verwenden, um in Data Lake Store Kontoverwaltungsvorgänge durchzuführen."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: fb9be26d3affe898bbbb66ead242dbdb59436bb6
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="account-management-operations-on-azure-data-lake-store-using-net-sdk"></a>Kontoverwaltungsvorgänge in Azure Data Lake Store mit dem .NET SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

In diesem Artikel wird beschrieben, wie Sie Kontoverwaltungsvorgänge in Data Lake Store mit dem .NET SDK durchführen. Zu den Kontoverwaltungsvorgängen gehören das Erstellen eines Data Lake Store-Kontos, das Auflisten der Konten in einem Azure-Abonnement, das Löschen der Konten usw.

Eine Anleitung zum Durchführen von Datenverwaltungsvorgängen in Data Lake Store per .NET SDK finden Sie unter [Filesystem operations on Data Lake Store using .NET SDK](data-lake-store-data-operations-net-sdk.md) (Dateisystemvorgänge in Data Lake Store per .NET SDK).

## <a name="prerequisites"></a>Voraussetzungen
* **Visual Studio 2013, 2015 oder 2017** In der Anleitung unten wird Visual Studio 2017 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-net-application"></a>Erstellen einer .NET-Anwendung
1. Öffnen Sie Visual Studio, und erstellen Sie eine Konsolenanwendung.
2. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
3. Unter **Neues Projekt** können Sie die folgenden Werte eingeben bzw. auswählen:

   | Eigenschaft | Wert |
   | --- | --- |
   | Kategorie |Vorlagen/Visual C#/Windows |
   | Vorlage |Konsolenanwendung |
   | Name |CreateADLApplication |
4. Klicken Sie auf **OK** , um das Projekt zu erstellen.
5. Fügen Sie Ihrem Projekt die NuGet-Pakete hinzu.

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **NuGet-Pakete verwalten**.
   2. Stellen Sie auf der Registerkarte **NuGet-Paket-Manager** sicher, dass **Paketquelle** auf **nuget.org** festgelegt und das Kontrollkästchen **Vorabversion einbeziehen** aktiviert ist.
   3. Suchen und installieren Sie die folgenden NuGet-Pakete:

      * `Microsoft.Azure.Management.DataLake.Store`: In diesem Tutorial wird „v2.1.3-preview“ verwendet.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: In diesem Tutorial wird „v2.2.12“ verwendet.

        ![Hinzufügen einer NuGet-Quelle](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Erstellen eines neuen Azure Data Lake-Kontos")
   4. Schließen Sie den **NuGet-Paket-Manager**.
6. Öffnen Sie **Program.cs**, löschen Sie den vorhandenen Code, und fügen Sie dann die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

        using System;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Deklarieren Sie die Variablen, und geben Sie die Werte für Platzhalter an. Stellen Sie außerdem sicher, dass der angegebene lokale Pfad und der Dateiname auf dem Computer vorhanden sind.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; 
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; 
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";                    
                }
            }
        }

In den restlichen Abschnitten dieses Artikels erfahren Sie, wie Sie die verfügbaren .NET-Methoden verwenden, um Vorgänge wie Authentifizierung, Dateiupload usw. durchzuführen.

## <a name="authentication"></a>Authentifizierung

* Informationen zur Authentifizierung von Endbenutzern für Ihre Anwendung finden Sie unter [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Authentifizierung von Endbenutzern mit Data Lake Store per .NET SDK).
* Informationen zur Dienst-zu-Dienst-Authentifizierung für Ihre Anwendung finden Sie unter [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Dienst-zu-Dienst-Authentifizierung mit Data Lake Store per .NET SDK).

## <a name="create-client-objects"></a>Erstellen von Clientobjekten
Im folgenden Codeausschnitt wird das Data Lake Store-Kontoclientobjekt erstellt, das zum Senden von Kontoverwaltungsanforderungen an den Dienst verwendet wird, z.B. Konto erstellen, Konto löschen usw.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(armCreds) { SubscriptionId = _subId };
    
## <a name="create-a-data-lake-store-account"></a>Erstellen eines Data Lake-Speicherkontos
Im folgenden Codeausschnitt wird ein Data Lake Store-Konto in dem Azure-Abonnement erstellt, das Sie angegeben haben, als Sie das Data Lake Store-Kontoclientobjekt erstellt haben.

    // Create Data Lake Store account
    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Auflisten aller Data Lake Store-Konten in einem Abonnement
Fügen Sie Ihrer Klassendefinition die folgende Methode hinzu: Mit dem folgenden Codeausschnitt werden alle Data Lake Store-Konten innerhalb eines bestimmten Azure-Abonnements aufgelistet:

    // List all Data Lake Store accounts within the subscription
    public static List<DataLakeStoreAccountBasic> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccountBasic>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="delete-a-data-lake-store-account"></a>Löschen eines Data Lake-Speicherkontos
Im folgenden Codeausschnitt wird das zuvor erstellte Data Lake Store-Konto gelöscht.

    // Delete Data Lake Store account
    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);

## <a name="see-also"></a>Weitere Informationen
* [Filesystem operations on Data Lake Store using .NET SDK](data-lake-store-data-operations-net-sdk.md) (Dateisystemvorgänge in Data Lake Store per .NET SDK)
* [Data Lake Store .NET SDK Reference (Referenz zum Data Lake Store .NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)

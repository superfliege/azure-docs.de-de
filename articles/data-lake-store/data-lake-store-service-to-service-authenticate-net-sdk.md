---
title: 'Authentifizierung zwischen Diensten: .NET SDK mit Data Lake Store mit Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die Authentifizierung zwischen Diensten bei Data Lake Store mithilfe von Azure Active Directory und des .NET SDK implementieren.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: c336cda6f3af4e2a4647371458b2db3e97917105
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-net-sdk"></a>Dienst-zu-Dienst-Authentifizierung bei Data Lake Store mithilfe des .NET SDK
> [!div class="op_single_selector"]
> * [Verwenden von Java](data-lake-store-service-to-service-authenticate-java.md)
> * [Verwenden des .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Verwenden von Python](data-lake-store-service-to-service-authenticate-python.md)
> * [Verwenden der REST-API](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

In diesem Artikel erfahren Sie, wie Sie mithilfe des .NET SDK die Authentifizierung zwischen Diensten bei Azure Data Lake Store durchführen. Informationen zur Authentifizierung von Endbenutzern bei Data Lake Store mithilfe des .NET SDK finden Sie unter [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Authentifizierung von Endbenutzern bei Data Lake Store per .NET SDK).


## <a name="prerequisites"></a>Voraussetzungen
* **Visual Studio 2013, 2015 oder 2017** In der Anleitung unten wird Visual Studio 2017 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Erstellen einer Azure Active Directory-Webanwendung.** Sie müssen die Schritte unter [Dienst-zu-Dienst-Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md) ausgeführt haben.

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

## <a name="service-to-service-authentication-with-client-secret"></a>Dienst-zu-Dienst-Authentifizierung mit geheimem Clientschlüssel
Fügen Sie diesen Codeausschnitt der .NET-Clientanwendung hinzu. Ersetzen Sie die Platzhalterwerte mit den aus einer (als Voraussetzung angegebenen) Azure AD-Webanwendung abgerufenen Werten.  Mit diesem Codeausschnitt kann Ihre Anwendung mithilfe des geheimen Clientschlüssels/Schlüssels für die Azure AD-Webanwendung **nicht interaktiv** bei Data Lake Store authentifiziert werden. 

    private static void Main(string[] args)
    {    
        // Service principal / appplication authentication with client secret / key
        // Use the client ID of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
        var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
        var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
    }

Im vorangehenden Ausschnitt wird die Hilfsfunktion `GetCreds_SPI_SecretKey` verwendet. Den Code für diese Hilfsfunktion finden Sie [hier auf GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey).

## <a name="service-to-service-authentication-with-certificate"></a>Dienst-zu-Dienst-Authentifizierung mit Zertifikat

Fügen Sie diesen Codeausschnitt der .NET-Clientanwendung hinzu. Ersetzen Sie die Platzhalterwerte mit den aus einer (als Voraussetzung angegebenen) Azure AD-Webanwendung abgerufenen Werten. Mit diesem Codeausschnitt kann Ihre Anwendung mithilfe des Zertifikats für eine Azure AD-Webanwendung **nicht interaktiv** bei Data Lake Store authentifiziert werden. Anweisungen zum Erstellen einer Azure AD-Anwendung finden Sie unter [Erstellen eines Dienstprinzipals mit selbstsigniertem Zertifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

    
    private static void Main(string[] args)
    {
        // Service principal / application authentication with certificate
        // Use the client ID and certificate of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
        var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
        var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
    }

Im vorangehenden Ausschnitt wird die Hilfsfunktion `GetCreds_SPI_Cert` verwendet. Den Code für diese Hilfsfunktion finden Sie [hier auf GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie die Authentifizierung zwischen Diensten verwenden, um sich mit dem .NET SDK bei Azure Data Lake Store zu authentifizieren. In den folgenden Artikeln wird erörtert, wie Sie das .NET SDK mit Azure Data Lake Store verwenden.

* [Kontoverwaltungsvorgänge in Azure Data Lake Store mit dem .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Dateisystemvorgänge in Azure Data Lake Store per .NET SDK](data-lake-store-data-operations-net-sdk.md)



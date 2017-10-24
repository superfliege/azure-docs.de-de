---
title: 'Endbenutzerauthentifizierung: .NET SDK mit Data Lake Store mit Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory mit .NET SDK umsetzen.
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
ms.date: 09/29/2017
ms.author: nitinme
ms.openlocfilehash: a2a9754b462e39713e6580baab84c064237306a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-net-sdk"></a>Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von .NET SDK
> [!div class="op_single_selector"]
> * [Verwenden von Java](data-lake-store-end-user-authenticate-java-sdk.md)
> * [Verwenden des .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Verwenden von Python](data-lake-store-end-user-authenticate-python.md)
> * [Verwenden der REST-API](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

In diesem Artikel erfahren Sie, wie Sie mithilfe des .NET SDK die Authentifizierung von Endbenutzern bei Azure Data Lake Store durchführen. Informationen zur Dienst-zu-Dienst-Authentifizierung bei Data Lake Store mithilfe des .NET SDK finden Sie unter [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Dienst-zu-Dienst-Authentifizierung mit Data Lake Store per .NET SDK).

## <a name="prerequisites"></a>Voraussetzungen
* **Visual Studio 2013, 2015 oder 2017** In der Anleitung unten wird Visual Studio 2017 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Erstellen einer „nativen“ Azure Active Directory-Anwendung**. Sie müssen die Schritte unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md) ausgeführt haben.

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
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

## <a name="end-user-authentication"></a>Authentifizierung von Endbenutzern
Fügen Sie diesen Codeausschnitt der .NET-Clientanwendung hinzu. Ersetzen Sie die Platzhalterwerte mit den aus einer (als Voraussetzung angegebenen) nativen Azure AD-Anwendung abgerufenen Werten. Mit diesem Codeausschnitt können Sie Ihre Anwendung **interaktiv** bei Data Lake Store authentifizieren, d.h. Sie werden aufgefordert, Ihre Azure-Anmeldeinformationen einzugeben.

Zur Vereinfachung werden im folgenden Codeausschnitt Standardwerte für Client-ID und Umleitungs-URI verwendet, die für jedes Azure-Abonnement gültig sind. Im folgenden Codeausschnitt müssen Sie nur den Wert für Ihre Mandanten-ID bereitstellen. Sie können mithilfe der Anweisungen unter [Abrufen der Mandanten-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id) die Mandanten-ID abrufen.
    
    private static void Main(string[] args)
    {
        // User login via interactive popup
        // Use the client ID of an existing AAD native application.
        SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
        var tenant_id = "<AAD_tenant_id>"; // Replace this string with the user's Azure Active Directory tenant ID
        var nativeClientApp_applicationId = "1950a258-227b-4e31-a9cf-717495945fc2";
        var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_applicationId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
        var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;
    }

Im Anschluss folgen einige wissenswerte Informationen zu obigem Codeausschnitt:

* Zur Vereinfachung des Tutorials werden in dem Codeausschnitt eine Azure AD-Domäne und eine Client-ID verwendet, die standardmäßig für alle Azure-Abonnements verfügbar sind. Dadurch können Sie **diesen Codeausschnitt unverändert in Ihrer Anwendung verwenden**.
* Wenn Sie allerdings Ihre eigene Azure AD-Domäne und Anwendungsclient-ID verwenden möchten, müssen Sie eine native Azure AD-Anwendung erstellen und dann die Azure AD-Mandanten-ID, die Client-ID und den Umleitungs-URI für die erstellte Anwendung verwenden. Eine entsprechende Anleitung finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

  
## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie die Authentifizierung von Endbenutzern verwenden, um sich mithilfe des .NET SDK bei Azure Data Lake Store zu authentifizieren. In den folgenden Artikeln wird erörtert, wie Sie das .NET SDK mit Azure Data Lake Store verwenden.

* [Erste Schritte mit Azure Data Lake Store mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Dateisystemvorgänge in Data Lake Store per .NET SDK](data-lake-store-data-operations-net-sdk.md)


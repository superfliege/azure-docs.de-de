---
title: Authentifizieren mit Azure Active Directory für den Zugriff auf Blob- und Warteschlangendaten aus Ihren Anwendungen (Vorschauversion) | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Azure Active Directory zum Authentifizieren aus einer Anwendung verwenden und anschließend Anforderungen für Azure Storage-Ressourcen autorisieren (Vorschauversion).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/07/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 6a0b7139fd8d216397090154a4324c8e4305a939
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816377"
---
# <a name="authenticate-with-azure-active-directory-from-an-azure-storage-application-preview"></a>Authentifizieren mit Azure Active Directory über eine Azure Storage-Anwendung (Vorschauversion)

Ein wesentlicher Vorteil bei der Verwendung von Azure Active Directory (Azure AD) mit Azure Storage besteht darin, dass Ihre Anmeldeinformationen nicht mehr im Code gespeichert werden müssen. Stattdessen können Sie ein OAuth 2.0-Zugriffstoken von Azure AD anfordern. Azure AD übernimmt die Authentifizierung des Sicherheitsprinzipals (Benutzer, Gruppe oder Dienstprinzipal), der die Anwendung ausführt. Wenn die Authentifizierung erfolgreich ist, gibt Azure AD das Zugriffstoken an die Anwendung zurück, und die Anwendung kann dann das Zugriffstoken zum Autorisieren von Anforderungen an Azure Storage verwenden.

In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung für die Authentifizierung mit Azure AD konfigurieren. Im Codebeispiel wird .NET verwendet. Der Ansatz ist bei anderen Sprachen aber ähnlich.

Damit Sie einen Sicherheitsprinzipal aus Ihrer Azure Storage-Anwendung authentifizieren können, konfigurieren Sie die Einstellungen der rollenbasierten Zugriffskontrolle (Role-Based Access Control, RBAC) für diesen Sicherheitsprinzipal. Azure Storage definiert RBAC-Rollen, die Berechtigungen für Container und Warteschlangen beinhalten. Wird die RBAC-Rolle einem Sicherheitsprinzipal zugewiesen, wird diesem Sicherheitsprinzipal Zugriff auf die Ressource gewährt. Weitere Informationen finden Sie unter [Manage access rights to Azure Storage data with RBAC (Preview)](storage-auth-aad-rbac.md) (Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)).

Eine Übersicht über den Datenfluss für OAuth 2.0-Codeberechtigungen finden Sie unter [Autorisieren des Zugriffs auf Azure Active Directory-Webanwendungen mit dem Flow zum Erteilen des OAuth 2.0-Codes](../../active-directory/develop/v1-protocols-oauth-code.md).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registrieren der Anwendung bei einem Azure AD-Mandanten

Der erste Schritt bei der Verwendung von Azure AD zum Authentifizieren des Zugriffs auf Speicherressourcen ist die Registrierung Ihrer Anwendung bei einem Azure AD-Mandanten. Die Registrierung Ihrer Anwendung ermöglicht es Ihnen, die Azure [Active Directory-Authentifizierungsbibliothek](../../active-directory/active-directory-authentication-libraries.md) (Active Directory Authentication Library, ADAL) aus dem Code aufzurufen. Die ADAL stellt eine API für die Authentifizierung bei Azure AD über Ihre Anwendung bereit. Die Registrierung Ihrer Anwendung ermöglicht Ihnen darüber hinaus das Autorisieren von Aufrufen von Azure Storage-APIs aus dieser Anwendung mit einem Zugriffstoken.

Wenn Sie Ihre Anwendung registrieren, liefern Sie Azure AD Informationen über Ihre Anwendung. Azure AD stellt dann eine Client-ID (auch als *Anwendungs-ID* bezeichnet) bereit, mit der Sie Ihre Anwendung zur Laufzeit Azure AD zuordnen. Weitere Informationen zur Client-ID finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](../../active-directory/develop/app-objects-and-service-principals.md).

Führen Sie die Schritte im Abschnitt [Hinzufügen einer Anwendung](../../active-directory/develop/quickstart-v1-add-azure-ad-app.md) des Artikels [Integrieren von Anwendungen in Azure Active Directory](../../active-directory/active-directory-integrating-applications.md) aus, um Ihre Azure Storage-Anwendung zu registrieren. Wenn Sie Ihre Anwendung als native Anwendung registrieren, können Sie jeden gültigen URI als **Umleitungs-URI** angeben. Der Wert muss kein echter Endpunkt sein.

![Screenshot, der das Registrieren Ihrer Speicheranwendung bei Azure AD zeigt](./media/storage-auth-aad-app/app-registration.png)

Nachdem Sie Ihre Anwendung registriert haben, wird die Anwendungs-ID (oder Client-ID) unter **Einstellungen** angezeigt:

![Screenshot der Client-ID](./media/storage-auth-aad-app/app-registration-client-id.png)

Ausführlichere Informationen zum Registrieren einer Anwendung bei Azure AD finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Erteilen von Berechtigungen für die registrierte App für Azure Storage

Als Nächstes müssen Sie Ihrer Anwendung Berechtigungen zum Aufrufen von Azure Storage-APIs erteilen. Dieser Schritt ermöglicht es Ihrer Anwendung, Aufrufe von Azure Storage mit Azure AD zu autorisieren.

1. Wählen Sie im linken Navigationsbereich des Azure-Portals **Alle Dienste** aus, und suchen Sie nach **App-Registrierungen**.
2. Suchen Sie nach dem Namen der registrierten Anwendung, die Sie im vorherigen Schritt erstellt haben.
3. Wählen Sie Ihre registrierte App aus, und klicken Sie auf **Einstellungen**. Wählen Sie im Abschnitt **API-Zugriff** die Option **Erforderliche Berechtigungen**.
4. Klicken Sie auf dem Blatt **Erforderliche Berechtigungen** auf die Schaltfläche **Hinzufügen**.
5. Suchen Sie unter **API auswählen** nach „Azure Storage“, und wählen Sie in der Liste der Ergebnisse **Azure Storage** aus.

    ![Screenshot mit Berechtigungen für den Speicher](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Aktivieren Sie unter **Berechtigungen auswählen** das Kontrollkästchen neben **Access Azure Storage** (Auf Azure Storage zugreifen), und klicken Sie auf **Auswählen**.
7. Klicken Sie auf **Fertig**.

Im Fenster **Erforderliche Berechtigungen** wird jetzt angezeigt, dass Ihre Azure AD-Anwendung sowohl Zugriff auf Azure Active Directory als auch auf Azure Storage hat. Berechtigungen für Azure AD werden automatisch gewährt, wenn Sie Ihre App zum ersten Mal bei Azure AD registrieren.

![Screenshot mit Berechtigungen der registrierten App](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="net-code-example-create-a-block-blob"></a>.NET Codebeispiel: Erstellen eines Blockblobs

Das Codebeispiel veranschaulicht, wie Sie ein Zugriffstoken aus Azure AD abrufen. Mit dem Zugriffstoken wird der angegebene Benutzer authentifiziert und anschließend eine Anforderung zum Erstellen eines Blockblobs autorisiert. Damit dieses Beispiel funktioniert, führen Sie zunächst die in den vorherigen Abschnitten beschriebenen Schritte aus.

> [!NOTE]
> Als Besitzer Ihres Azure Storage-Kontos erhalten Sie nicht automatisch Berechtigungen für den Zugriff auf Daten. Sie müssen sich selbst explizit eine RBAC-Rolle für Azure Storage zuweisen. Sie können sie auf Ebene Ihres Abonnements, einer Ressourcengruppe, eines Speicherkontos oder eines Containers oder einer Warteschlange zuordnen. 
>
> Wenn Sie den Beispielcode also etwa für ein Speicherkonto, für das Sie als Besitzer fungieren, unter Ihrer eigenen Benutzeridentität ausführen möchten, müssen Sie sich selbst die RBAC-Rolle „Blob Data Contributor“ (Mitwirkender an Blobdaten) zuweisen. Andernfalls tritt beim Aufruf zum Erstellen des Blobs ein Fehler mit HTTP-Statuscode 403 (Verboten) auf. Weitere Informationen finden Sie unter [Manage access rights to Azure Storage data with RBAC (Preview)](storage-auth-aad-rbac.md) (Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Bekannte Werte für die Authentifizierung mit Azure AD

Um einen Sicherheitsprinzipal bei Azure AD zu authentifizieren, müssen Sie einige bekannte Werte in den Code aufnehmen.

#### <a name="azure-ad-oauth-endpoint"></a>Azure AD-OAuth-Endpunkt

Der Azure AD-Basisautoritätsendpunkt für OAuth 2.0 lautet wie unten angegeben. Dabei steht *tenant-id* für Ihre Active Directory-Mandanten-ID (oder Verzeichnis-ID):

`https://login.microsoftonline.com/<tenant-id>/oauth2/token`

Die Mandanten-ID identifiziert den zur Authentifizierung zu verwendenden Azure AD-Mandanten. Führen Sie zum Abrufen der Mandanten-ID die Schritte unter **Abrufen der Mandanten-ID für Ihr Azure Active Directory** aus.

#### <a name="storage-resource-id"></a>Storage-Ressourcen-ID

Verwenden Sie die Azure Storage-Ressourcen-ID, um ein Token zum Authentifizieren von Anforderungen an Azure Storage abzurufen:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>Abrufen der Mandanten-ID für Ihr Azure Active Directory

Um die Mandanten-ID abzurufen, gehen Sie folgendermaßen vor:

1. Wählen Sie im Azure-Portal Ihr Active Directory aus.
2. Klicken Sie auf **Eigenschaften**.
3. Kopieren Sie den GUID-Wert, der für die **Verzeichnis-ID** bereitgestellt wird. Dieser Wert wird auch als Mandanten-ID bezeichnet.

![Screenshot, der zeigt, wie Sie die Mandanten-ID kopieren](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Hinzufügen von Verweisen und using-Anweisungen  

Installieren Sie in Visual Studio die Vorschauversion der Azure Storage-Clientbibliothek. Wählen Sie im Menü **Extras** den Eintrag **NuGet-Paket-Manager** und danach **Paket-Manager-Konsole** aus. Geben Sie den folgenden Befehl in die Konsole ein, um die aktuelle Version der Clientbibliothek für .NET zu installieren:

```
Install-Package WindowsAzure.Storage
```

Installieren Sie außerdem die neueste Version der Active Directory-Authentifizierungsbibliothek (ADAL):

```
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

Fügen Sie anschließend Ihrem Code die folgenden using-Anweisungen hinzu:

```dotnet
using System.Globalization;
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Abrufen eines OAuth-Tokens von Azure AD

Fügen Sie als Nächstes eine Methode hinzu, die ein Token von Azure AD anfordert. Rufen Sie zum Anfordern des Tokens die Methode [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync) auf. Stellen Sie sicher, dass Sie über die folgenden Werte aus den zuvor ausgeführten Schritten verfügen:

- Mandanten-ID (Verzeichnis)
- Client-ID (Anwendung)
- URI für Clientumleitung

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/";
    const string AuthEndpoint = "https://login.microsoftonline.com/{0}/oauth2/token";
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthEndpoint, TenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    // Acquire an access token from Azure AD. 
    AuthenticationResult result = authContext.AcquireTokenAsync(ResourceId, 
                                                                "<client-id>", 
                                                                new Uri(@"<client-redirect-uri>"), 
                                                                new PlatformParameters(PromptBehavior.Auto)).Result;

    return result.AccessToken;
}
```

### <a name="create-the-block-blob"></a>Erstellen des Blockblobs

Verwenden Sie abschließend das Zugriffstoken zum Erstellen neuer Speicheranmeldeinformationen und diese Anmeldeinformationen wiederum zum Erstellen des Blobs:

```dotnet
// Get the access token.
string accessToken = GetUserOAuthToken();

// Use the access token to create the storage credentials.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using those credentials
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
```

> [!NOTE]
> Für die Azure AD-Integration mit Azure Storage müssen Sie HTTPS für Azure Storage-Vorgänge verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).
- Informationen zur Verwendung verwalteter Identitäten für Azure-Ressourcen mit Azure Storage finden Sie unter [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Azure-Identitäten für Azure-Ressourcen (Vorschau)](storage-auth-aad-msi.md).
- Informationen zum Anmelden bei der Azure-Befehlszeilenschnittstelle (CLI) und bei PowerShell mit einer Azure AD-Identität finden Sie unter [Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)](storage-auth-aad-script.md) (Verwenden einer Azure AD-Identität für den Zugriff auf Azure Storage über die Befehlszeilenschnittstelle oder PowerShell (Vorschau)).
- Weitere Informationen zur Azure AD-Integration für Azure-Blobs und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).




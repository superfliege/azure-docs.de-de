---
title: Tutorial zur Verwendung von Azure Key Vault aus einer Webanwendung – Azure Key Vault | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure Key Vault aus einer Webanwendung verwenden.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: barclayn
ms.openlocfilehash: 79bccbcbcf78de18504c5cb0235e29930d90ede8
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999304"
---
# <a name="tutorial-use-azure-key-vault-from-a-web-application"></a>Tutorial: Verwenden von Azure Key Vault aus einer Webanwendung

In diesem Tutorial erfahren Sie, wie Sie Azure Key Vault aus einer Webanwendung in Azure verwenden. Es zeigt den Prozess für den Zugriff auf einen geheimen Schlüssel aus Azure Key Vault für die Verwendung in einer Webanwendung. Anschließend baut das Tutorial auf dem Prozess auf und verwendet ein Zertifikat anstelle eines geheimen Clientschlüssels. Dieses Tutorial richtet sich an Webentwickler, die die Grundlagen zum Erstellen von Webanwendungen in Azure kennen.

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Hinzufügen von Anwendungseinstellungen zur Datei „web.config“
> * Hinzufügen einer Methode zum Abrufen eines Zugriffstokens
> * Abrufen des Tokens beim Anwendungsstart
> * Authentifizierung mit einem Zertifikat

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie folgende Elemente:

* Einen URI eines geheimen Schlüssels in Azure Key Vault
* Eine Client-ID und einen geheimen Schlüssel für den Client für eine Webanwendung, die in Azure Active Directory registriert ist und Zugriff auf Ihren Schlüsseltresor hat
* Eine Webanwendung. In diesem Tutorial werden die Schritte für eine ASP.NET MVC-Anwendung gezeigt, die in Azure als Web-App bereitgestellt wurde.

Führen Sie die Schritte unter [Erste Schritte mit Azure Key Vault](key-vault-get-started.md) aus, um den URI für einen geheimen Schlüssel, eine Client-ID, einen geheimen Clientschlüssel abzurufen und die Anwendung zu registrieren. Die Webanwendung hat Zugriff auf den Tresor und muss in Azure Active Directory registriert werden. Darüber hinaus benötigt sie Zugriffsrechte für Key Vault. Wenn dies nicht der Fall ist, kehren Sie zur Registrierung der Anwendung im Tutorial „Erste Schritte“ zurück, und wiederholen Sie die dort aufgeführten Schritte. Weitere Informationen zur Erstellung von Azure-Web-Apps finden Sie unter [Web-Apps – Übersicht](../app-service/overview.md).

Dieses Beispiel basiert auf der manuellen Bereitstellung von Azure Active Directory-Identitäten. Sie sollten aber stattdessen [verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) verwenden, da hierbei automatisch Azure AD-Identitäten bereitgestellt werden. Weitere Informationen finden Sie im [Beispiel auf GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) und im zugehörigen Tutorial zu [App Service und Functions](https://docs.microsoft.com/azure/app-service/overview-managed-identity). Sie können auch den für Key Vault spezifischen Artikel [Tutorial: Konfigurieren einer Azure-Webanwendung zum Lesen eines Geheimnisses aus Key Vault](tutorial-web-application-keyvault.md) verwenden.

## <a id="packages"></a>Hinzufügen von NuGet-Paketen

Für Ihre Webanwendung müssen zwei Pakete installiert sein.

* Active Directory-Authentifizierungsbibliothek: enthält Methoden für die Interaktion mit Azure Active Directory und die Verwaltung der Benutzeridentität
* Azure Key Vault-Bibliothek: enthält Methoden für die Interaktion mit Azure Key Vault

Beide Pakete können mithilfe der Paket-Manager-Konsole mit dem Befehl „Install-Package“ installiert werden.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory 
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>Ändern der Datei „web.config“

Es gibt drei Anwendungseinstellungen, die wie folgt zur Datei „web.config“ hinzugefügt werden müssen. Zur Erhöhung der Sicherheit fügen wir die tatsächlichen Werte im Azure-Portal hinzu.

```xml
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
    <!-- If you aren't hosting your app as an Azure Web App, then you should use the actual ClientId, Client Secret, and Secret URI values -->
```

## <a id="gettoken"></a>Hinzufügen einer Methode zum Abrufen eines Zugriffstokens

Sie benötigen für die Verwendung der Key Vault-API ein Zugriffstoken. Der Key Vault Client verarbeitet die Aufrufe in der Key Vault-API. Sie müssen dafür jedoch eine Funktion angeben, die das Zugriffstoken übernimmt. Im folgenden Beispiel ist Code für das Abrufen eines Zugriffstokens von Azure Active Directory enthalten. Dieser Code kann überall in Ihrer Anwendung eingefügt werden. Ich füge dafür i.d.R. die Klasse „Utils“ oder „EncryptionHelper“ hinzu.  

```cs
//add these using statements
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Threading.Tasks;
using System.Web.Configuration;

//this is an optional property to hold the secret after it is retrieved
public static string EncryptSecret { get; set; }

//the method that will be provided to the KeyVaultClient
public static async Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                WebConfigurationManager.AppSettings["ClientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
// Using Client ID and Client Secret is a way to authenticate an Azure AD application.
// Using it in your web application allows for a separation of duties and more control over your key management. 
// However, it does rely on putting the Client Secret in your configuration settings.
// For some people, this can be as risky as putting the secret in your configuration settings.
```

## <a id="appstart"></a>Abrufen des geheimen Schlüssel beim Anwendungsstart

Wir benötigen nun Code zum Aufrufen der Key Vault-API, um den geheimen Schlüssel abzurufen. Der folgende Code kann an einer beliebigen Stelle eingefügt werden, sofern er aufgerufen wird, bevor Sie ihn verwenden müssen. Ich habe diesen Code im Ereignis „Application Start“ (Anwendungsstart) in der Datei „Global.asax“ eingefügt, sodass es einmal beim Start ausgeführt wird und den geheimen Schlüssel für die Anwendung verfügbar macht.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>Hinzufügen von App-Einstellungen im Azure-Portal

In der Azure-Web-App können Sie jetzt die tatsächlichen Werte für die App-Einstellungen im Azure-Portal hinzufügen. Damit befinden sich die tatsächlichen Werte nicht in der Datei „web.config“, sondern sie werden über das Portal geschützt, für das Sie eigene Zugriffssteuerungsfunktionen verwenden können. Diese Werte ersetzen die Werte, die Sie in der Datei „web.config“ eingegeben haben. Stellen Sie sicher, dass die Namen übereinstimmen.

![Anwendungseinstellungen im Azure-Portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Authentifizieren mit einem Zertifikat anstelle eines geheimen Clientschlüssels

Nun, da Sie eine Azure AD-App mithilfe einer Client-ID und eines geheimen Clientschlüssels authentifizieren können, verwenden wir eine Client-ID und ein Zertifikat. Führen Sie die folgenden Schritte aus, um ein Zertifikat in einer Azure-Web-App zu verwenden:

1. Abrufen oder Erstellen eines Zertifikats
2. Zuordnen des Zertifikats zu einer Azure AD-App
3. Hinzufügen von Code zur Web-App für die Verwendung des Zertifikats
4. Hinzufügen eines Zertifikats zur Web-App

### <a name="get-or-create-a-certificate"></a>Abrufen oder Erstellen eines Zertifikats

 Wir erstellen für dieses Tutorial ein Testzertifikat. Im Folgenden finden Sie ein Skript zur Erstellung eines selbstsignierten Zertifikats. Passen Sie das Verzeichnis an, um den Speicherort für die Zertifikatdateien festzulegen.  Erhöhen Sie zur Angabe des Start- und Enddatums für das Zertifikat das aktuelle Datum jeweils um ein Jahr.

```powershell
#Create self-signed certificate and export pfx and cer files 
$PfxFilePath = 'KVWebApp.pfx'
$CerFilePath = 'KVWebApp.cer'
$DNSName = 'MyComputer.Contoso.com'
$Password = 'MyPassword"'

$StoreLocation = 'CurrentUser' #be aware that LocalMachine requires elevated privileges
$CertBeginDate = Get-Date
$CertExpiryDate = $CertBeginDate.AddYears(1)

$SecStringPw = ConvertTo-SecureString -String $Password -Force -AsPlainText 
$Cert = New-SelfSignedCertificate -DnsName $DNSName -CertStoreLocation "cert:\$StoreLocation\My" -NotBefore $CertBeginDate -NotAfter $CertExpiryDate -KeySpec Signature
Export-PfxCertificate -cert $Cert -FilePath $PFXFilePath -Password $SecStringPw 
Export-Certificate -cert $Cert -FilePath $CerFilePath 
```

Notieren Sie sich das Enddatum und das Kennwort für die PFX-Datei (in diesem Beispiel: 15. Mai 2019 und „MyPassword“). Sie benötigen diese für das nachfolgende Skript. 
### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Zuordnen des Zertifikats zu einer Azure AD-App

Nachdem Sie ein Zertifikat erstellt haben, müssen Sie es einer Azure AD-Anwendung zuordnen. Die Zuordnung kann über PowerShell ausgeführt werden. Führen Sie die folgenden Befehle aus, um das Zertifikat der Azure AD-Anwendung zuzuordnen:

```powershell
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Nachdem Sie diese Befehle ausgeführt haben, wird die Anwendung in Azure AD angezeigt. Beim Durchsuchen der App-Registrierungen müssen Sie sicherstellen, dass Sie im Dialogfeld für die Suche **Meine Apps** anstelle von „Alle Apps“ ausgewählt haben. 

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Hinzufügen von Code zur Web-App für die Verwendung des Zertifikats

Nun fügen wir Code zur Web-App hinzu, um auf das Zertifikat zuzugreifen und es für die Authentifizierung zu verwenden. 

Zunächst erstellen wir den Code für den Zugriff auf das Zertifikat. Der Speicherort (StoreLocation) ist „CurrentUser“ statt „LocalMachine“. Beachten Sie auch, dass wir „False“ für die Find-Methode bereitstellen, da wir ein Testzertifikat verwenden.

```cs
//Add this using statement
using System.Security.Cryptography.X509Certificates;  

public static class CertificateHelper
{
    public static X509Certificate2 FindCertificateByThumbprint(string findValue)
    {
        X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
        try
        {
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                findValue, false); // Don't validate certs, since the test root isn't installed.
            if (col == null || col.Count == 0)
                return null;
            return col[0];
        }
        finally
        {
            store.Close();
        }
    }
}
```



Als Nächstes folgt Code, der „CertificateHelper“ verwendet und das für die Authentifizierung benötigte „ClientAssertionCertificate“ erstellt.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Dies ist der neue Code zum Abrufen des Zugriffstokens. Dieser Code ersetzt die GetToken-Methode im vorherigen Beispiel. Ich habe ihr der Einfachheit halber einen anderen Namen gegeben. Zur einfacheren Handhabung habe ich all diesen Code in die Klasse „Utils“ meines Web-App-Projekts eingefügt.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```



Die letzte Codeänderung betrifft die Application_Start-Methode. Zunächst müssen wir die GetCert()-Methode aufrufen, um das „ClientAssertionCertificate“ zu laden. Anschließend ändern wir die Callback-Methode, die wir beim Erstellen eines neues „KeyVaultClient“ bereitstellen. Beachten Sie, dass dieser Code den Code im vorherigen Beispiel ersetzt.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Hinzufügen eines Zertifikats zu Ihrer Web-App über das Azure-Portal

Das Hinzufügen eines Zertifikats zu Ihrer Web-App ist ein einfacher Zwei-Schritte-Prozess. Navigieren Sie zunächst im Azure-Portal zu Ihrer Web-App. Klicken Sie in Ihrer Web-App unter „Einstellungen“ auf den Eintrag für **SSL-Einstellungen**. Wenn dieser geöffnet wird, laden Sie das im vorherigen Beispiel erstellte Zertifikat, „KVWebApp.pfx“ hoch. Stellen Sie sicher, dass Sie das Kennwort für die PFX-Datei merken.

![Hinzufügen eines Zertifikats zu einer Web-App im Azure-Portal][2]

Abschließend müssen Sie eine Anwendungseinstellung mit dem Namen „WEBSITE\_LOAD\_CERTIFICATES“ und dem Wert „*“ zu Ihrer Web-App hinzufügen. Dadurch wird sichergestellt, dass alle Zertifikate geladen werden. Wenn Sie nur die von Ihnen hochgeladenen Zertifikate laden möchten, können Sie eine durch Kommas getrennte Liste ihrer Fingerabdrücke eingeben.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie können den App-Dienst, den Schlüsseltresor und die Azure AD-Anwendung löschen, die Sie für das Tutorial verwendet haben, wenn Sie diese nicht mehr benötigen.  


## <a id="next"></a>Nächste Schritte
> [!div class="nextstepaction"]
>[API-Referenz für die Azure Key Vault-Verwaltung](/dotnet/api/overview/azure/keyvault/management).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
 
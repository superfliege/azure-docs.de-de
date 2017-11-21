---
title: Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET
description: "Verwenden Sie Bibliothek „Microsoft.Azure.Services.AppAuthentication“ zur Authentifizierung beim Azure Key Vault mithilfe von .NET."
keywords: "Lokale Anmeldeinformationen für die Authentifizierung bei Azure Key Vault"
author: lleonard-msft
manager: mbaldwin
services: key-vault
ms.author: alleonar
ms.date: 11/15/2017
ms.topic: article
ms.prod: 
ms.service: microsoft-keyvault
ms.technology: 
ms.assetid: 4be434c4-0c99-4800-b775-c9713c973ee9
ms.openlocfilehash: f67f81aeee0775ea8d90e4459f2c46266a774786
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET

Zum Authentifizieren in Azure Key Vault benötigen Sie die Anmeldeinformationen eines Azure Active Directory (AD), und zwar entweder einen gemeinsamen geheimen Schlüssel oder ein Zertifikat. Das Verwalten dieser Anmeldeinformationen kann schwierig sein, und es ist verführerisch, Anmeldeinformationen in Quell- oder Konfigurationsdateien einzuschließen um sie in eine App zu bündeln.

Die `Microsoft.Azure.Services.AppAuthentication` für .NET-Bibliothek vereinfacht dieses Problem. Die Anmeldeinformationen des Entwicklers werden für die Authentifizierung während der lokalen Entwicklung verwendet. Wenn die Lösung später in Azure bereitgestellt wird, wechselt die Bibliothek automatisch zu Anwendungsanmeldeinformationen.  

Während der lokalen Entwicklung sind Entwickleranmeldeinformationen sicherer, da Sie keine Azure AD-Anmeldeinformationen erstellen oder Anmeldeinformationen zwischen Entwicklern freigeben müssen.

Die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek verwaltet die Authentifizierung automatisch, wodurch Sie sich wiederum auf die Lösung anstatt auf Ihre Anmeldeinformationen konzentrieren können.

Die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek unterstützt die lokale Entwicklung mit Microsoft Visual Studio, Azure CLI oder der integrierten Azure AD-Authentifizierung. Wenn die Bereitstellung in Azure App Services oder einem virtuellen Azure-Computer (VM) erfolgt, verwendet die Bibliothek automatisch [verwaltete Dienstidentitäten](/azure/active-directory/msi-overview) (MSI). Es sind keine Änderungen von Code bzw. Konfiguration erforderlich. Die Bibliothek unterstützt auch die direkte Verwendung von Azure AD-[Clientanmeldeinformationen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal), wenn MSI nicht verfügbar ist oder wenn der Sicherheitskontext des Entwicklers bei der lokalen Entwicklung nicht bestimmt werden kann.

<a name="asal"></a>
## <a name="using-the-library"></a>Verwenden der Bibliothek

Für .NET-Anwendungen ist die einfachste Methode zum Verwenden einer verwalteten Dienstidentität (Managed Service Identity, MSI) ein `Microsoft.Azure.Services.AppAuthentication`-Paket. Erfahren Sie, wie Sie loslegen können:

1. Fügen Sie einen Verweis auf das NuGet-Paket [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) zu Ihrer Anwendung hinzu.

2. Fügen Sie den folgenden Code hinzu:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // ...

    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(
    azureServiceTokenProvider.KeyVaultTokenCallback));

    // or

    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync(
       "https://management.azure.com/").ConfigureAwait(false);
    ```

Die `AzureServiceTokenProvider`-Klasse speichert das Token im Arbeitsspeicher zwischen und ruft es kurz vor dem Ablaufdatum von Azure AD ab. Folglich brauchen Sie vor dem Aufruf der `GetAccessTokenAsync`-Methode das Ablaufdatum nicht mehr zu überprüfen. Rufen Sie einfach die Methode auf, wenn Sie das Token verwenden möchten. 

Die `GetAccessTokenAsync`-Methode erfordert einen Ressourcenbezeichner. Weitere Informationen finden Sie unter [Welche Azure-Dienste unterstützen verwaltete Dienstidentitäten?](https://docs.microsoft.com/en-us/azure/active-directory/msi-overview#which-azure-services-support-managed-service-identity).


<a name="samples"></a>
## <a name="samples"></a>Beispiele

Die folgenden Beispiele zeigen die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek in Aktion:

1. [Verwenden von verwalteten Dienstidentitäten (MSI) zum Abrufen eines geheimen Schlüssels aus Azure Key Vault zur Laufzeit](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Programmgesteuerte Bereitstellung einer Azure Resource Manager-Vorlage aus einer Azure-VM mit einer MSI](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Verwenden eines .NET Core-Beispiels und MSI zum Aufrufen der Azure-Dienste aus einer Azure Linux-VM ](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).


<a name="local"></a>
## <a name="local-development-authentication"></a>Authentifizierung für die lokale Entwicklung

Für die lokale Entwicklung gibt es zwei primäre Authentifizierungsszenarien:

- [Authentifizierung bei Azure-Diensten](#authenticating-to-azure-services)
- [Authentifizierung bei benutzerdefinierten Diensten](#authenticating-to-custom-services)

Hier lernen Sie die Anforderungen für jedes Szenario und die unterstützten Tools kennen.


### <a name="authenticating-to-azure-services"></a>Authentifizierung bei Azure-Diensten

Verwaltete Dienstidentitäten (MSI) werden von lokalen Computern nicht unterstützt.  Daher verwendet die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek Ihre Entwickleranmeldeinformationen für die Ausführung in Ihrer lokalen Entwicklungsumgebung. Wenn die Lösung in Azure bereitgestellt wird, verwendet die Bibliothek die verwaltete Dienstentität, um zu einem Flow zum Erteilen der OAuth 2.0-Clientanmeldeinformationen zu wechseln.  Dies bedeutet, dass Sie den gleichen Code ohne Sorge lokal und remote testen können.

Für die lokale Entwicklung ruft `AzureServiceTokenProvider` Token mithilfe von **Visual Studio**, der **Azure-Befehlszeilenschnittstelle** (CLI) oder der **integrierten Azure AD-Authentifizierung** ab. Jede Option wird nacheinander ausprobiert, und die Bibliothek verwendet die erste Option, die erfolgreich ausgeführt wird. Wenn keine Option funktioniert, wird eine `AzureServiceTokenProviderException`-Ausnahme mit detaillierten Informationen ausgelöst.

### <a name="authenticating-with-visual-studio"></a>Authentifizierung mit Visual Studio

Zum Verwenden von Visual Studio, überprüfen Sie Folgendes:

1. Sie haben [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) oder höher installiert.

2. Die [App-Authentifizierungserweiterung für Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354) ist installiert.
 
3. Sie haben sich bei Visual Studio angemeldet und ein Konto für die lokale Entwicklung ausgewählt. Verwenden Sie **Tools**&nbsp;>&nbsp;**Optionen**&nbsp;>&nbsp;**Azure Service-Authentifizierung**, um einen lokales Entwicklungskonto auszuwählen. 

Wenn Probleme bei der Verwendung von Visual Studio, z. B. Fehler im Zusammenhang mit der Tokenanbieterdatei, auftreten, überprüfen Sie sorgfältig die folgende Schritte. 

Es kann auch erforderlich sein, Ihr Entwicklertoken erneut zu authentifizieren.  Wechseln Sie zu diesem Zweck zu **Tools**&nbsp;>&nbsp;**Optionen**>**Azure&nbsp;Service&nbsp;Authentifizierung**, und suchen Sie unter dem ausgewählten Konto nach einem Link zum **erneuten Authentifizieren**.  Wählen Sie diesen aus, um eine erneute Authentifizierung durchzuführen. 

### <a name="authenticating-with-azure-cli"></a>Authentifizierung mit Azure CLI

So verwenden Sie Azure CLI für die lokale Entwicklung:

1. Installieren Sie [Azure CLI v2.0.12](/cli/azure/install-azure-cli) oder höher. Aktualisieren Sie frühere Versionen. 

2. Melden Sie sich mit **az login** bei Azure an.

Verwenden Sie `az account get-access-token`, um den Zugriff zu überprüfen.  Wenn Sie eine Fehlermeldung erhalten, vergewissern Sie sich, dass Schritt 1 erfolgreich abgeschlossen wurde. 

Wenn Azure CLI nicht im Standardverzeichnis installiert ist, erhalten Sie möglicherweise eine Fehlermeldung, die angibt, dass `AzureServiceTokenProvider` den Pfad zu Azure CLI nicht finden kann.  Verwenden Sie die Umgebungsvariable **AzureCLIPath**, um den Azure CLI-Installationsordner zu definieren. `AzureServiceTokenProvider` fügt das in der Umgebungsvariable **AzureCLIPath** angegebene Verzeichnis bei Bedarf zur Umgebungsvariable **Path** hinzu.

Wenn Sie mit mehreren Konten bei Azure CLI angemeldet sind, oder wenn Ihr Konto Zugriff auf mehrere Abonnements hat, müssen Sie das zu verwendende Abonnement angeben.  Verwenden Sie dazu:

```
az account set --subscription <subscription-id>
```

Mit diesem Befehl wird nur bei einem Fehler eine Ausgabe generiert.  Verwenden Sie zum Überprüfen der Einstellungen für das aktuelle Konto:

```
az account list
```

### <a name="authenticating-with-azure-ad-integrate-authentication"></a>Authentifizierung mit der integrierten Azure AD-Authentifizierung

Um die Azure AD-Authentifizierung zu verwenden, überprüfen Sie Folgendes:

- Ihr lokales Active Directory [wird mit Azure AD synchronisiert](/azure/active-directory/connect/active-directory-aadconnect).

- Ihr Code wird auf einem in die Domäne eingebundenen Computer ausgeführt.


### <a name="authenticating-to-custom-services"></a>Authentifizierung bei benutzerdefinierten Diensten

Wenn ein Dienst Azure-Dienste aufruft, wurden die vorherigen Schritte erfolgreich ausgeführt, da Azure-Dienste den Zugriff auf Benutzer und Anwendungen zuzulassen.  

Beim Erstellen eines Diensts, der einen benutzerdefinierten Dienst aufruft, verwenden Sie Azure AD-Clientanmeldeinformationen für die lokale Entwicklungsauthentifizierung.  Es gibt zwei Optionen: 

1.  Verwenden Sie einen Dienstprinzipal für die Anmeldung bei Azure:

    1.  [Erstellen Sie einen Dienstprinzipal](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Verwenden Sie Azure CLI zum Anmelden:

        ```
        az login --service-principal -u <principal-id> --password <password>
           --tenant <tenant-id> --allow-no-subscriptions
        ```

        Da der Dienstprinzipal möglicherweise keinen Zugriff auf ein Abonnement hat, verwenden Sie das Argument `--allow-no-subscriptions`.

2.  Verwenden Sie Umgebungsvariablen, um Details zum Dienstprinzipal anzugeben.  Weitere Informationen finden Sie unter [Ausführen der Anwendung mithilfe eines Dienstprinzipals](#running-the-application-using-a-service-principal).

Nachdem Sie sich bei Azure angemeldet haben, verwendet `AzureServiceTokenProvider` den Dienstprinzipal zum Abrufen eines Tokens für die lokale Entwicklung.

Dies gilt nur für lokale Entwicklung. Wenn Ihre Lösung in Azure bereitgestellt wird, wechselt die Bibliothek zur MSI-Authentifizierung.

<a name="msi"></a>
## <a name="running-the-application-using-a-managed-service-identity"></a>Ausführen der Anwendung mithilfe einer verwalteten Dienstidentität 

Wenn Sie Ihren Code auf einer Azure App Service- oder Azure-VM ausführen und MSI aktiviert ist, verwendet die Bibliothek automatisch verwaltete Dienstidentitäten (Managed Service Identity, MSI). Es sind keine Codeänderungen erforderlich. 


<a name="sp"></a>
## <a name="running-the-application-using-a-service-principal"></a>Ausführen der Anwendung mithilfe eines Dienstprinzipals 

Es ist möglicherweise erforderlich, Azure AD-Clientanmeldeinformationen zum Authentifizieren zu erstellen. Häufige Beispiele sind:

1. Der Code wird in einer lokalen Entwicklungsumgebung, aber nicht unter der Identität des Entwicklers ausgeführt.  Service Fabric verwendet zum Beispiel das [NetworkService-Konto](/azure/service-fabric/service-fabric-application-secret-management) für lokale Entwicklung.
 
2. Der Code wird in einer lokalen Entwicklungsumgebung ausgeführt, und Sie authentifizieren sich bei einem benutzerdefinierten Dienst, sodass Sie Ihre Entwickleridentität nicht nutzen können. 
 
3. Ihr Code wird für eine Azure Compute-Ressource ausgeführt, die die verwaltete Dienstidentität noch nicht unterstützt (z. B. Azure Batch).

So verwenden Sie ein Zertifikat für die Anmeldung bei Azure AD:

1. Erstellen Sie ein [Dienstprinzipalzertifikat](/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

2. Stellen Sie das Zertifikat entweder dem _LocalMachine_- oder _CurrentUser_-Store bereit. 

3. Legen Sie eine Umgebungsvariable namens **AzureServicesAuthConnectionString** folgendermaßen fest:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={LocalMachine or CurrentUser}.
    ```
 
    Ersetzen Sie _{AppId}_, _{TenantId}_ und _{Thumbprint}_ durch die in Schritt 1 generierten Werte.

    **CertificateStoreLocation** muss, je nach Ihrem Bereitstellungsplan, entweder _CurrentUser_ oder _LocalMachine_ sein.

4. Führen Sie die Anwendung aus. 

So melden Sie sich mit einem gemeinsamen geheimen Schlüssel als Azure AD-Anmeldeinformationen an:

1. Erstellen Sie einen [Dienstprinzipal mit einem Kennwort](/azure/azure-resource-manager/resource-group-authenticate-service-principal), und gewähren sie diesem Zugriff auf den Key Vault. 

2. Legen Sie eine Umgebungsvariable namens **AzureServicesAuthConnectionString** folgendermaßen fest:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}. 
    ```

    Ersetzen Sie _{AppId}_, _{TenantId}_ und _{ClientSecret}_ durch die in Schritt 1 generierten Werte.

3. Führen Sie die Anwendung aus. 

Nachdem alles ordnungsgemäß eingerichtet wurde, sind keine weiteren Codeänderungen mehr erforderlich.  `AzureServiceTokenProvider` verwendet die Umgebungsvariable und das Zertifikat zum Authentifizieren bei Azure AD. 

<a name="connectionstrings"></a>
## <a name="connection-string-support"></a>Unterstützung der Verbindungszeichenfolge

Standardmäßig verwendet `AzureServiceTokenProvider` mehrere Methoden zum Abrufen eines Tokens. 

Um den Prozess zu steuern, verwenden Sie eine Verbindungszeichenfolge, die an den `AzureServiceTokenProvider`-Konstruktor übergeben oder in der Umgebungsvariablen *AzureServicesAuthConnectionString* angegeben wurde. 

Die folgenden Optionen werden unterstützt:

| Verbindungs&nbsp;zeichenfolgen&nbsp;option | Szenario | Kommentare|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Lokale Entwicklung | AzureServiceTokenProvider verwendet Azure CLI zum Abrufen des Tokens. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Lokale Entwicklung | AzureServiceTokenProvider verwendet Visual Studio zum Abrufen des Tokens. |
| `RunAs=CurrentUser;` | Lokale Entwicklung | AzureServiceTokenProvider verwendet die integrierte Azure AD-Authentifizierung zum Abrufen des Tokens. |
| `RunAs=App;` | Verwaltete Dienstidentität | AzureServiceTokenProvider verwendet verwaltete Dienstidentitäten zum Abrufen des Tokens. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint`<br>`   ={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`  | Dienstprinzipal | `AzureServiceTokenProvider` verwendet ein Zertifikat zum Abrufen des Tokens von Azure AD. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};`<br>`   CertificateSubjectName={Subject};CertificateStoreLocation=`<br>`   {LocalMachine or CurrentUser}` | Dienstprinzipal | `AzureServiceTokenProvider` verwendet ein Zertifikat zum Abrufen des Tokens von Azure AD.|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Dienstprinzipal |`AzureServiceTokenProvider` verwendet einen geheimen Schlüssel zum Abrufen des Tokens von Azure AD. |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [verwaltete Dienstidentität](/azure/app-service/app-service-managed-service-identity).

- Lernen Sie verschiedene Möglichkeiten zum [Authentifizieren und Autorisieren von Apps](/azure/app-service/app-service-authentication-overview) kennen.

- Erfahren Sie mehr über Azure AD-[Authentifizierungsszenarien](/azure/active-directory/develop/active-directory-authentication-scenarios#web-browser-to-web-application).
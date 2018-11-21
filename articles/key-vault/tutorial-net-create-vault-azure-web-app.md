---
title: 'Tutorial: Verwenden von Azure Key Vault mit Azure-Web-App in .NET | Microsoft-Dokumentation'
description: 'Tutorial: Konfigurieren einer ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 6e30a181bc69678051e6d6469a2f1c533de811df
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686210"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-web-app-in-net"></a>Tutorial: Verwenden von Azure Key Vault mit Azure-Web-App in .NET

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial führen Sie die erforderlichen Schritte aus, um eine Azure-Webanwendung zu erhalten und Informationen unter Verwendung von verwalteten Identitäten für Azure-Ressourcen aus Azure Key Vault zu lesen. Dieses Tutorial basiert auf [Azure-Web-Apps](../app-service/app-service-web-overview.md). In diesem Artikel wird Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Speichern eines Geheimnisses im Schlüsseltresor
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen einer Azure-Webanwendung
> * Aktivieren einer [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) für die Web-App
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus dem Schlüsseltresor für die Webanwendung
> * Ausführen der Webanwendung in Azure

Lesen Sie die Informationen zu [grundlegenden Konzepten](key-vault-whatis.md#basic-concepts), bevor Sie fortfahren.

## <a name="prerequisites"></a>Voraussetzungen

* Unter Windows:
  * [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/download/windows)

* Auf einem Macintosh:
  * Siehe [Neues in Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/)

* Alle Plattformen:
  * Git ([Download](https://git-scm.com/downloads))
  * Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
  * [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), Version 2.0.4 oder höher. Diese Version ist für Windows, Mac und Linux verfügbar.
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Was ist eine verwaltete Dienstidentität, und wie funktioniert sie?
Gehen wir zunächst auf das Konzept der verwalteten Dienstidentität (Managed Service Identity, MSI) ein. Azure Key Vault kann Anmeldeinformationen sicher speichern. Dadurch befinden sie sich nicht in Ihrem Code. Zum Abrufen der Anmeldeinformationen ist jedoch eine Authentifizierung bei Azure Key Vault erforderlich. Und für die Authentifizierung bei Key Vault sind Anmeldeinformationen erforderlich. Ein klassisches Bootstrap-Problem. Dank Azure und Azure AD bietet MSI eine „Bootstrap-Identität“, die die ersten Schritte erheblich vereinfacht.

Das funktioniert so: Wenn Sie MSI für einen Azure-Dienst wie etwa Virtual Machines, App Service oder Functions aktivieren, erstellt Azure einen [Dienstprinzipal](key-vault-whatis.md#basic-concepts) für die Instanz des Diensts in Azure Active Directory und fügt die Anmeldeinformationen für den Dienstprinzipal in die Instanz des Diensts ein. 

![MSI](media/MSI.png)

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, auf ein Zugriffstoken zu erhalten.
Ihr Code verwendet das vom lokalen MSI-Endpunkt (MSI_ENDPOINT) erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst. 

Beginnen wir nun mit dem Tutorial.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein.
Im folgenden Beispiel wird eine Ressourcengruppe am Standort „USA, Westen“ erstellt:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Artikel verwendet.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie einen Schlüsseltresor in der Ressourcengruppe aus dem vorherigen Schritt. Geben Sie die folgenden Informationen ein:

* Name des Schlüsseltresors: Der Name muss zwischen drei und 24 Zeichen umfassen und darf nur folgende Zeichen enthalten: 0-9, a-z, A-Z und -.
* Ressourcengruppenname
* Standort: **USA, Westen**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Sie können eine SQL-Verbindungszeichenfolge oder beliebige andere Informationen speichern, die sicher aufbewahrt, aber für Ihre Anwendung verfügbar gemacht werden müssen.

Geben Sie die folgenden Befehle ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen. Dieses Geheimnis speichert den Wert **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Geben Sie Folgendes ein, um den Wert im Geheimnis als Nur-Text anzuzeigen:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Mit diesem Befehl werden die Geheimnisinformationen einschließlich des URI angezeigt. Nach der Ausführung dieser Schritte sollten Sie einen URI zu einem Geheimnis in einem Schlüsseltresor besitzen. Notieren Sie sich diese Informationen. Sie benötigen sie in einem späteren Schritt.

## <a name="create-a-net-core-web-app"></a>Erstellen einer .NET Core-Web-App

Gehen Sie wie in [diesem Tutorial](../app-service/app-service-web-get-started-dotnet.md) beschrieben vor, um eine .NET Core-Web-App zu erstellen und in Azure zu **veröffentlichen**. **ODER**: Sehen Sie sich das folgende Video an.
> [!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Öffnen und Bearbeiten der Lösung

1. Navigieren Sie unter „Seiten“ zur Datei „About.cshtml.cs“.
2. Installieren Sie die beiden folgenden NuGet-Pakete:
    - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
    - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importieren Sie Folgendes in die Datei „About.cshtml.cs-Datei“:

    ```
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
    ```
4. Der Code in der Klasse „AboutModel“ sollte wie folgt aussehen:
    ```
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;             

                /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
                do
                {
                    long waitTime = Math.Min(getWaitTime(retries), 2000000);
                    secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                    retry = false;
                } 
                while(retry && (retries++ < 10));
            }
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
                if((int)keyVaultException.Response.StatusCode == 429)
                    retry = true;
            }            
        }

        // This method implements exponential backoff incase of 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```


## <a name="run-the-app"></a>Ausführen der App

Wählen Sie im Hauptmenü von Visual Studio 2017 die Option **Debuggen** > **Starten** (mit/ohne Debugging) aus. Navigieren Sie im angezeigten Browser zur Seite **Info**. Der Wert für **AppSecret** wird angezeigt.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Aktivieren einer verwalteten Identität für die Web-App

Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mit den Informationen unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

1. Kehren Sie zur Azure-Befehlszeilenschnittstelle zurück.
2. Führen Sie den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen: 

   ```azurecli
   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
   ```
   Hinweis: <YourAppName> muss durch den Namen der veröffentlichten App in Azure ersetzt werden. Wenn der Name Ihrer veröffentlichten App also „MyAwesomeapp.azurewebsites.net“ lautet, ersetzen Sie <YourAppName> durch „MyAwesomeapp“.
 
 Die Ausgabe des obigen Befehls sieht wie folgt aus. Notieren Sie sich beim Veröffentlichen der Anwendung in Azure die Prinzipal-ID (PrincipalId). Sie sollte das folgende Format haben:
   ```
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
  ```
>[!NOTE]
>Der Befehl in dieser Vorgehensweise ist identisch mit dem Aufrufen des [Portals](https://portal.azure.com) und dem anschließenden Festlegen der Einstellung **Identität/Vom System zugewiesen** auf **Ein** (in den Webanwendungseigenschaften).

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Zuweisen von Berechtigungen zu Ihrer Anwendung für das Lesen von Geheimnissen aus Key Vault
        
Führen Sie anschließend diesen Befehl aus, indem Sie den Namen Ihres Schlüsseltresors und den Wert von **PrincipalId** verwenden:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

## <a name="publish-the-web-application-to-azure"></a>Veröffentlichen der Webanwendung in Azure

Veröffentlichen Sie diese App erneut in Azure, um sie live als Web-App zu sehen und zu überprüfen, ob Sie den Geheimniswert abrufen können.

1. Wählen Sie in Visual Studio das Projekt **key-vault-dotnet-core-quickstart** aus.
2. Klicken Sie auf **Veröffentlichen** > **Starten**.
3. Klicken Sie auf **Erstellen**.

Im obigen Befehl gewähren Sie der Identität (MSI) von App Service Berechtigungen zum Ausführen von **get**- und **list**-Vorgängen für Ihre Key Vault-Instanz. <br />
Wenn Sie die Anwendung nun ausführen, sollte der Wert des Geheimnisses abgerufen werden. 

Das ist auch schon alles. Sie haben nun erfolgreich eine Web-App in .NET erstellt, die ihre Geheimnisse in Key Vault speichert und daraus abruft.

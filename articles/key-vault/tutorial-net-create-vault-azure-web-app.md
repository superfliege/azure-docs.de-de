---
title: 'Tutorial: Verwenden von Azure Key Vault mit einer Azure-Web-App in .NET | Microsoft-Dokumentation'
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
ms.openlocfilehash: 50a7f3166d677fe1af961866ccae4445a3d810b8
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322140"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Tutorial: Verwenden von Azure Key Vault mit einer Azure-Web-App in .NET

Azure Key Vault unterstützt Sie beim Schützen von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen. Die Lösung ermöglicht Ihnen den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen.

In diesem Tutorial erfahren Sie, wie Sie eine Azure-Webanwendung erstellen, die Informationen aus einer Azure Key Vault-Instanz lesen kann. Dabei werden verwaltete Identitäten für Azure-Ressourcen verwendet. Weitere Informationen zu Azure-Webanwendungen finden Sie unter [Web-Apps – Übersicht](../app-service/app-service-web-overview.md).

In diesem Artikel wird Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Speichern eines Geheimnisses im Schlüsseltresor
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen einer Azure-Webanwendung
> * Aktivieren einer [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) für die Web-App
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus dem Schlüsseltresor für die Webanwendung
> * Ausführen der Webanwendung in Azure

Machen Sie sich zunächst mit den [grundlegenden Konzepten von Key Vault](key-vault-whatis.md#basic-concepts) vertraut, bevor Sie mit diesem Tutorial fortfahren.

## <a name="prerequisites"></a>Voraussetzungen

* Unter Windows:
  * [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/download/windows)

* Auf einem Macintosh:
  * [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/)

* Alle Plattformen:
  * [Git-Client](https://git-scm.com/downloads)
  * Ein Azure-Abonnement <br />(Falls Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.)
  * Mindestens Version 2.0.4 der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (verfügbar für Windows, Mac und Linux)
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="managed-service-identity-and-how-it-works"></a>Verwaltete Dienstidentität und ihre Funktionsweise

Azure Key Vault dient zum sicheren Speichern von Anmeldeinformationen, damit sich diese nicht in Ihrem Code befinden. Sie müssen sich jedoch bei Azure Key Vault authentifizieren, um Ihre Schlüssel abzurufen. Und für die Authentifizierung bei Key Vault benötigen Sie wiederum Anmeldeinformationen. Ein klassisches Bootstrap-Dilemma. Die verwaltete Dienstidentität (Managed Service Identity, MSI) löst dieses Problem, indem sie eine _Bootstrap-Identität_ bereitstellt, die den Prozess vereinfacht.

Wenn Sie MSI für einen Azure-Dienst aktivieren (z.B.: Virtual Machines, App Service oder Functions), erstellt Azure einen [Dienstprinzipal](key-vault-whatis.md#basic-concepts). MSI führt diesen Schritt für die Instanz des Diensts in Azure Active Directory (Azure AD) aus und fügt die Anmeldeinformationen für den Dienstprinzipal in diese Instanz ein.

![MSI-Diagramm](media/MSI.png)

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, um ein Zugriffstoken zu erhalten. Ihr Code verwendet das vom lokalen MSI-Endpunkt (MSI_ENDPOINT) erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

1. Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe.
1. Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein. Im folgenden Beispiel wird eine Ressourcengruppe am Standort „USA, Westen“ erstellt:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Diese Ressourcengruppe wird im gesamten Tutorial verwendet.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Geben Sie die folgenden Informationen an, um einen Schlüsseltresor in Ihrer Ressourcengruppe zu erstellen:

* Name des Schlüsseltresors: eine Zeichenfolge mit 3 bis 24 Zeichen, die nur Zahlen, Buchstaben und Bindestriche enthalten darf (z.B.: 0-9, a – Z, A-Z und -).
* Ressourcengruppenname
* Standort: **USA, Westen**

Geben Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl ein:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Nun können Sie ein Geheimnis hinzufügen. Dabei kann es sich beispielsweise um eine SQL-Verbindungszeichenfolge oder um beliebige andere Informationen handeln, die sowohl sicher aufbewahrt werden als auch für Ihre Anwendung verfügbar sein müssen.

Geben Sie den folgenden Befehl ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen. Dieses Geheimnis speichert den Wert **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Geben Sie den folgenden Befehl ein, um den im Geheimnis enthaltenen Wert als Nur-Text anzuzeigen:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Mit diesem Befehl werden die Geheimnisinformationen einschließlich des URI angezeigt. Nach der Ausführung dieser Schritte sollten Sie einen URI zu einem Geheimnis in einem Schlüsseltresor besitzen. Notieren Sie sich diese Informationen. Sie benötigen sie in einem späteren Schritt.

## <a name="create-a-net-core-web-app"></a>Erstellen einer .NET Core-Web-App

Gehen Sie wie in [diesem Tutorial](../app-service/app-service-web-get-started-dotnet.md) beschrieben vor, um eine .NET Core-Web-App zu erstellen und in Azure zu **veröffentlichen**. Sie können sich auch das folgende Video ansehen:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Öffnen und Bearbeiten der Lösung

1. Navigieren Sie zu **Seiten** > **About.cshtml.cs**.
2. Installieren Sie die folgenden NuGet-Pakete:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importieren Sie den folgenden Code in die Datei „About.cshtml.cs“:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

4. Ihr Code in der Klasse „AboutModel“ sollte wie folgt aussehen:

   ```csharp
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

1. Wählen Sie im Hauptmenü von Visual Studio 2017 **Debuggen** > **Starten** (mit oder ohne Debugging) aus. 
1. Navigieren Sie im angezeigten Browser zur Seite **Info**.
1. Der Wert für **AppSecret** wird angezeigt.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Aktivieren einer verwalteten Identität für die Web-App

Azure Key Vault ermöglicht das sichere Speichern von Anmeldeinformationen und anderen Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Dieses Problem können Sie anhand der Informationen unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md) lösen, indem Sie Azure-Diensten eine automatisch verwaltete Identität in Azure AD bereitstellen. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

1. Führen Sie über die Azure-Befehlszeilenschnittstelle den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen:

   ```azurecli

   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"

   ```

   >[!NOTE]
   >Ersetzen Sie \<YourAppName\> durch den Namen der veröffentlichten App in Azure. Wenn Ihre veröffentlichte App also beispielsweise **MyAwesomeapp.azurewebsites.net** heißt, ersetzen Sie \<YourAppName\> durch **MyAwesomeapp**.

1. Notieren Sie sich den Wert von `PrincipalId`, wenn Sie die Anwendung in Azure veröffentlichen. Die Ausgabe des Befehls in Schritt 1 hat folgendes Format:

   ```json
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
   ```

>[!NOTE]
>Der Befehl in dieser Vorgehensweise ist identisch mit dem Aufrufen des [Portals](https://portal.azure.com) und dem anschließenden Festlegen der Einstellung **Identität/Vom System zugewiesen** auf **Ein** (in den Webanwendungseigenschaften).

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Zuweisen von Berechtigungen zu Ihrer Anwendung für das Lesen von Geheimnissen aus Key Vault

Ersetzen Sie im folgenden Befehl \<YourKeyVaultName\> durch den Namen Ihres Schlüsseltresors und \<PrincipalId\> durch den Wert von **PrincipalId**:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Dieser Befehl erteilt der Identität (MSI) des App-Diensts Berechtigungen zum Ausführen von **get**- und **list**-Vorgängen für Ihren Schlüsseltresor.

## <a name="publish-the-web-application-to-azure"></a>Veröffentlichen der Webanwendung in Azure

Veröffentlichen Sie Ihre Web-App erneut in Azure, um sich zu vergewissern, dass Ihre Live-Web-App den Geheimniswert abrufen kann.

1. Wählen Sie in Visual Studio das Projekt **key-vault-dotnet-core-quickstart** aus.
2. Klicken Sie auf **Veröffentlichen** > **Starten**.
3. Klicken Sie auf **Erstellen**.

Wenn Sie die Anwendung ausführen, sollte sie den Wert des Geheimnisses abrufen können.

Sie haben nun erfolgreich eine Web-App in .NET erstellt, die ihre Geheimnisse in Key Vault speichert und daraus abruft.

## <a name="next-steps"></a>Nächste Schritte

>[!div class="nextstepaction"]
>[Entwicklerhandbuch zu Azure-Schlüsseltresor](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-developers-guide)

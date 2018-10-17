---
title: 'Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer Node-Web-App | Microsoft-Dokumentation'
description: 'Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer Node-Web-App'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.workload: identity
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 860294ebc7fbadd3eeefc4298ec740ca7f704587
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714393"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-node-web-app"></a>Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer Node-Web-App 

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Geheimnis in Key Vault speichern und es über eine Web-App abrufen. Um den Wert des Geheimnisses anzuzeigen, müssen Sie die folgenden Schritte in Azure ausführen. In dieser Schnellstartanleitung werden Node.js und verwaltete Identitäten für Azure-Ressourcen verwendet.

> [!div class="checklist"]
> * Erstellen einer Key Vault-Instanz
> * Speichern eines Geheimnisses in Key Vault
> * Abrufen eines Geheimnisses aus Key Vault
> * Erstellen einer Azure-Webanwendung
> * Aktivieren einer [verwalteten Identität](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) für die Web-App
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus Key Vault für die Webanwendung

Bevor Sie fortfahren, stellen Sie sicher, dass Sie mit den [grundlegenden Konzepte](key-vault-whatis.md#basic-concepts) vertraut sind.

>[!NOTE]
Sie müssen mit ein paar Konzepten vertraut sein, um zu verstehen, warum das folgende Tutorial die empfohlene Vorgehensweise ist. Key Vault ist ein zentrales Repository zum programmgesteuerten Speichern von Geheimnissen. Dafür müssen sich Anwendungen oder Benutzer jedoch zuerst bei Key Vault authentifizieren, d.h. sie müssen ein Geheimnis angeben. Aus Sicherheitsgründen muss das erste Geheimnis außerdem regelmäßig rotiert werden. Dank der [verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) erhalten in Azure ausgeführte Anwendungen jedoch eine Identität, die automatisch von Azure verwaltet wird. Das macht die **Einführung von Geheimnissen weniger problematisch**, da Benutzer/Anwendungen die empfohlene Vorgehensweise verwenden können und sich nicht um die Rotation des ersten Geheimnisses kümmern müssen.

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js](https://nodejs.org/en/)
* [Git-Client](https://www.git-scm.com/)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 oder höher
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="login-to-azure"></a>Anmelden an Azure

Geben Sie Folgendes ein, um sich über die Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein.
Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *<YourResourceGroupName>* am Standort *eastus* erstellt.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Tutorial verwendet.

## <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz

Als Nächstes erstellen Sie eine Key Vault-Instanz mithilfe der Ressourcengruppe, die Sie im vorherigen Schritt erstellt haben. In diesem Artikel lautet der Name für die Key Vault-Instanz zwar „ContosoKeyVault“, Sie müssen jedoch einen eindeutigen Namen verwenden. Geben Sie die folgenden Informationen ein:

* Tresorname: **Wählen Sie hier einen Key Vault-Namen aus**.
* Ressourcengruppenname: **Wählen Sie hier einen Ressourcengruppennamen aus**.
* Standort: Hier wird **East US** (USA, Osten) verwendet.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-key-vault"></a>Hinzufügen eines Geheimnisses zu Key Vault

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Sie können eine SQL-Verbindungszeichenfolge oder beliebige andere Informationen speichern, die sicher aufbewahrt, aber für Ihre Anwendung verfügbar gemacht werden müssen. In diesem Tutorial heißt das Kennwort **AppSecret**, und darin wird der Wert **MySecret** gespeichert.

Geben Sie die folgenden Befehle ein, um in Key Vault ein Geheimnis namens **AppSecret** mit dem Wert **Pa$$MySecret** zu erstellen:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Geben Sie Folgendes ein, um den Wert im Geheimnis als Nur-Text anzuzeigen:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Mit diesem Befehl werden die Geheimnisinformationen einschließlich des URI angezeigt. Nach der Ausführung dieser Schritte sollten Sie einen URI zu einem Geheimnis in einer Azure Key Vault-Instanz besitzen. Notieren Sie sich diese Informationen. Sie benötigen sie in einem späteren Schritt.

## <a name="clone-the-repo"></a>Klonen des Repositorys

Klonen Sie das Repository, um eine lokale Kopie zu erstellen, damit Sie die Quelle bearbeiten können. Führen Sie hierzu den folgenden Befehl aus:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Im Folgenden installieren wir die Abhängigkeiten. Führen Sie die folgenden Befehle aus: cd key-vault-node-quickstart npm install

Dieses Projekt verwendet zwei Node-Module:

* [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) 
* [azure-keyvault](https://www.npmjs.com/package/azure-keyvault)

## <a name="publish-the-web-application-to-azure"></a>Veröffentlichen der Webanwendung in Azure

Im Folgenden finden Sie die Schritte, die erforderlich sind.

- Der erste Schritt besteht darin, einen [Azure App Service](https://azure.microsoft.com/services/app-service/)-Plan zu erstellen. In diesem Plan können Sie mehrere Web-Apps speichern.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
- Als Nächstes erstellen Sie eine Web-App. Ersetzen Sie im folgenden Beispiel <app_name> durch einen global eindeutigen App-Namen (gültige Zeichen sind a–z, 0–9 und -). Die Runtime ist auf NODE|6.9 festgelegt. Führen Sie zum Anzeigen aller unterstützten Runtimes den folgenden Befehl aus: az webapp list-runtimes.
    ```
    # Bash
    az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
    ```
    Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:
    ```
    {
      "availabilityState": "Normal",
      "clientAffinityEnabled": true,
      "clientCertEnabled": false,
      "cloningInfo": null,
      "containerSize": 0,
      "dailyMemoryTimeQuota": 0,
      "defaultHostName": "<app_name>.azurewebsites.net",
      "enabled": true,
      "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
      < JSON data removed for brevity. >
    }
    ```
    Navigieren Sie zu Ihrer neu erstellten Web-App. Ihnen sollte nun eine funktionsfähige Web-App angezeigt werden. Ersetzen Sie <app_name> durch einen eindeutigen App-Namen.

    ```
    http://<app name>.azurewebsites.net
    ```
    Der o.g. Befehl erstellt auch eine Git-fähige App, mit der Sie Bereitstellungen in Azure aus Ihrem lokalen Git durchführen können. 
    Das lokale Git ist mit der folgenden URL konfiguriert: https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git

- Erstellen Sie einen Bereitstellungsbenutzer. Nach Abschluss des vorherigen Befehls können Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzufügen. Ersetzen Sie <url> durch die URL des Git-Remoterepositorys, die Sie beim Aktivieren von Git für die App erhalten haben.

    ```
    git remote add azure <url>
    ```

## <a name="enable-a-managed-identity-for-the-web-app"></a>Aktivieren einer verwalteten Identität für die Web-App

Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mit den Informationen unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

Führen Sie den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen:

```azurecli
az webapp identity assign --name <app_name> --resource-group "<YourResourceGroupName>"
```

Dieser Befehl entspricht dem Aufrufen des Portals und dem Festlegen der Einstellung **Identität/Vom System zugewiesen** auf **Ein** in den Webanwendungseigenschaften.

### <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Zuweisen von Berechtigungen zu Ihrer Anwendung für das Lesen von Geheimnissen aus Key Vault

Notieren Sie sich die Ausgabe des obigen Befehls, oder kopieren Sie sie. Sie sollte das folgende Format aufweisen:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Führen Sie anschließend diesen Befehl aus, indem Sie den Namen Ihrer Key Vault-Instanz und den Wert von PrincipalId verwenden, den Sie oben kopiert haben:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Bereitstellen der Node-App in Azure und Abrufen des Geheimniswerts

Nachdem Sie nun alles eingerichtet haben, führen Sie den folgenden Befehl aus, um die App in Azure bereitzustellen:

```
git push azure master
```

Wenn Sie anschließend zu https://<app_name>.azurewebsites.net navigieren, wird Ihnen der Geheimniswert angezeigt.
Stellen Sie sicher, dass Sie den Namen <YourKeyVaultName> durch Ihren Tresornamen ersetzt haben.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Key Vault – Startseite](https://azure.microsoft.com/services/key-vault/)
* [Azure Key Vault – Dokumentation](https://docs.microsoft.com/azure/key-vault/)
* [Azure SDK für Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST-API-Referenz](https://docs.microsoft.com/rest/api/keyvault/)

---
title: 'Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer Node-Web-App – Azure Key Vault | Microsoft-Dokumentation'
description: 'Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer .NET-Web-App'
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 20d47ecaea8ce393f60cba93c3dbcf7ca4a076c8
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "54002602"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer .NET-Web-App

In dieser Schnellstartanleitung führen Sie die erforderlichen Schritte zum Abrufen einer Azure-Webanwendung durch, um Informationen aus Azure Key Vault mithilfe von verwalteten Identitäten für Azure-Ressourcen zu lesen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Speichern eines Geheimnisses im Schlüsseltresor
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen einer Azure-Webanwendung
> * Aktivieren einer [verwalteten Dienstidentität](../active-directory/managed-identities-azure-resources/overview.md) für die Web-App
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus dem Schlüsseltresor für die Webanwendung

Lesen Sie die Informationen zu [grundlegenden Konzepten](key-vault-whatis.md#basic-concepts), bevor Sie fortfahren.

>[!NOTE]
>Key Vault ist ein zentrales Repository zum programmgesteuerten Speichern von Geheimnissen. Dafür müssen sich Anwendungen und Benutzer jedoch zuerst bei Key Vault authentifizieren, d.h. sie müssen ein Geheimnis angeben. Aus Sicherheitsgründen muss das erste Geheimnis regelmäßig rotiert werden. 
>
>Dank der [verwalteten Dienstidentitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) erhalten in Azure ausgeführte Anwendungen eine Identität, die automatisch von Azure verwaltet wird. Das macht die *Einführung von Geheimnissen weniger problematisch*, da Benutzer und Anwendungen die empfohlene Vorgehensweise verwenden können und sich nicht um die Rotation des ersten Geheimnisses kümmern müssen.

## <a name="prerequisites"></a>Voraussetzungen

* Unter Windows:
  * [Visual Studio 2017 Version 15.7.3 oder höher](https://www.microsoft.com/net/download/windows) mit den folgenden Workloads:
    * ASP.NET und Webentwicklung
    * Plattformübergreifende .NET Core-Entwicklung
  * [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/download/windows)

* Auf einem Macintosh:
  * Siehe [Neues in Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/)

* Alle Plattformen:
  * Git ([Download](https://git-scm.com/downloads))
  * Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
  * [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), Version 2.0.4 oder höher. Diese Version ist für Windows, Mac und Linux verfügbar.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein.
Im folgenden Beispiel wird eine Ressourcengruppe am Standort „USA, Osten“ erstellt.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Artikel verwendet.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie einen Schlüsseltresor in der Ressourcengruppe aus dem vorherigen Schritt. Geben Sie die folgenden Informationen ein:

* Name des Schlüsseltresors: Der Name muss zwischen 3 und 24 Zeichen umfassen und darf nur die Zeichen 0-9, a-z, A-Z und Bindestrich (-) enthalten.
* Ressourcengruppenname
* Standort: **USA, Osten**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
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

## <a name="clone-the-repo"></a>Klonen des Repositorys

Klonen Sie das Repository, um eine lokale Kopie zu erstellen, in der Sie die Quelle bearbeiten können. Führen Sie den folgenden Befehl aus:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Öffnen und Bearbeiten der Lösung

Bearbeiten Sie die Datei „program.cs“, um das Beispiel mit Ihrem spezifischen Schlüsseltresornamen auszuführen:

1. Navigieren Sie zum Ordner „key-vault-dotnet-core-quickstart“.
2. Öffnen Sie die Datei „key-vault-dotnet-core-quickstart.sln“ in Visual Studio 2017.
3. Öffnen Sie die Datei „Program.cs“, und aktualisieren Sie den Platzhalter *YourKeyVaultName* mit dem Namen Ihres Schlüsseltresors, den Sie weiter oben erstellt haben.

Für diese Lösung werden die NuGet-Bibliotheken [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) und [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) verwendet.

## <a name="run-the-app"></a>Ausführen der App

Wählen Sie im Hauptmenü von Visual Studio 2017 die Option **Debuggen** > **Ohne Debugger starten**. Navigieren Sie im angezeigten Browser zur Seite **Info**. Der Wert für **AppSecret** wird angezeigt.

## <a name="publish-the-web-application-to-azure"></a>Veröffentlichen der Webanwendung in Azure

Veröffentlichen Sie diese App in Azure, um sie live als Web-App zu verwenden und zu überprüfen, ob Sie den Geheimniswert abrufen können:

1. Wählen Sie in Visual Studio das Projekt **key-vault-dotnet-core-quickstart** aus.
2. Klicken Sie auf **Veröffentlichen** > **Starten**.
3. Erstellen Sie eine neue Instanz von **App Service**, und klicken Sie dann auf **Veröffentlichen**.
4. Ändern Sie den App-Namen in **keyvaultdotnetcorequickstart**.
5. Klicken Sie auf **Erstellen**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-a-managed-identity-for-the-web-app"></a>Aktivieren einer verwalteten Identität für die Web-App

Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mit den Informationen unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

1. Kehren Sie zur Azure-Befehlszeilenschnittstelle zurück.
2. Führen Sie den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>Dieser Befehl in dieser Vorgehensweise entspricht dem Aufrufen des Portals und dem Festlegen der Einstellung **Identität/Vom System zugewiesen** auf **Ein** in den Webanwendungseigenschaften.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Zuweisen von Berechtigungen zu Ihrer Anwendung für das Lesen von Geheimnissen aus Key Vault

Notieren Sie sich die Ausgabe, wenn Sie die Anwendung in Azure veröffentlichen. Sie sollte das folgende Format haben:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Führen Sie anschließend diesen Befehl aus, indem Sie den Namen Ihres Schlüsseltresors und den Wert von **PrincipalId** verwenden:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

Wenn Sie die Anwendung nun ausführen, sollte der Wert des Geheimnisses abgerufen werden. Im obigen Befehl gewähren Sie der Identität (MSI) von App Service Berechtigungen zum Ausführen von **get**- und **list**-Vorgängen für Ihren Schlüsseltresor.

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
* [Azure SDK für .NET](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST-API-Referenz](https://docs.microsoft.com/rest/api/keyvault/)

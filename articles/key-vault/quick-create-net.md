---
title: 'Azure-Schnellstart: Konfigurieren einer Azure-Webanwendung zum Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Key Vault | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine ASP.NET Core-Anwendung zum Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Key Vault konfigurieren.
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 8b5624ae3083d92213b4ee919dc0860bf5ff4ab7
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480201"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-net-web-app"></a>Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer .NET-Web-App

In dieser Schnellstartanleitung wird ausführlich erläutert, mit welchen Schritten Sie eine Azure-Webanwendung zum Lesen von Informationen aus Key Vault unter Verwendung von verwalteten Dienstidentitäten konfigurieren. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer Key Vault-Instanz
> * Speichern eines Geheimnisses in Key Vault
> * Abrufen eines Geheimnisses aus Key Vault
> * Erstellen einer Azure-Webanwendung
> * [Aktivieren von verwalteten Dienstidentitäten](../active-directory/managed-service-identity/overview.md)
> * Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus Key Vault für die Webanwendung

Lesen Sie sich vor dem Fortfahren den Abschnitt [Grundlegende Konzepte](key-vault-whatis.md#basic-concepts) durch, vor allem zum Thema [Verwaltete Dienstidentität](../active-directory/managed-service-identity/overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Unter Windows:
  * [Visual Studio 2017 Version 15.7.3 oder höher](https://www.microsoft.com/net/download/windows) mit den folgenden Workloads:
    * ASP.NET und Webentwicklung
    * Plattformübergreifende .NET Core-Entwicklung
  * [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/download/windows)

* Auf einem Macintosh:
  * https://visualstudio.microsoft.com/vs/mac/

* Alle Plattformen:
  * Laden Sie GIT [hier](https://git-scm.com/downloads) herunter.
  * Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest): Sie benötigen Azure CLI Version 2.0.4 oder höher. Diese Version ist für Windows, Macintosh und Linux verfügbar.

## <a name="login-to-azure"></a>Anmelden an Azure

   Geben Sie Folgendes ein, um sich über die Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein.
Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *<YourResourceGroupName>* am Standort *eastus* erstellt.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Artikel verwendet.

## <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz

Als Nächstes erstellen Sie eine Key Vault-Instanz in der Ressourcengruppe aus dem vorherigen Schritt. Geben Sie die folgenden Informationen ein:

* Tresorname: **Wählen Sie hier einen Key Vault-Namen aus**. Der Schlüsseltresornamen muss eine Zeichenfolge sein, die zwischen 3 und 24 Zeichen lang ist, und darf nur die Zeichen 0-9, a-z, A-Z und - enthalten.
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
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Öffnen und Bearbeiten der Lösung

Bearbeiten Sie die Datei „program.cs“, um das Beispiel mit Ihrem spezifischen Schlüsseltresornamen auszuführen.

1. Navigieren Sie zum Ordner „key-vault-dotnet-core-quickstart“.
2. Öffnen Sie die Datei „key-vault-dotnet-core-quickstart.sln“ in Visual Studio 2017.
3. Öffnen Sie die Datei „Program.cs“, und aktualisieren Sie den Platzhalter <YourKeyVaultName> mit dem Namen Ihrer Key Vault-Instanz, die Sie weiter oben erstellt haben.

Für diese Lösung werden die NuGet-Bibliotheken [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) und [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) verwendet.

## <a name="run-the-app"></a>Ausführen der App

Wählen Sie im Hauptmenü von Visual Studio 2017 die Option „Debuggen“ > „Ohne Debugger starten“. Navigieren Sie im angezeigten Browser zur Seite „Info“. Der Wert für „AppSecret“ wird angezeigt.

## <a name="publish-the-web-application-to-azure"></a>Veröffentlichen der Webanwendung in Azure

Wir veröffentlichen diese App in Azure, um sie live als Web-App zu verwenden und das Abrufen des Geheimniswerts zu verfolgen.

1. Wählen Sie in Visual Studio das Projekt **key-vault-dotnet-core-quickstart** aus.
2. Klicken Sie auf **Veröffentlichen** und dann auf **Starten**.
3. Erstellen Sie eine neue Instanz von **App Service**, und klicken Sie auf **Veröffentlichen**.
4. Ändern Sie den App-Namen in „keyvaultdotnetcorequickstart“.
5. Klicken Sie auf **Erstellen**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-managed-service-identities-msi"></a>Aktivieren von verwalteten Dienstidentitäten (MSI)

Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code aber bei Azure Key Vault authentifizieren. Mithilfe der verwalteten Dienstidentität (Managed Service Identity, MSI) kann dieser Vorgang vereinfacht werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

1. Kehren Sie zur Azure-Befehlszeilenschnittstelle zurück.
2. Führen Sie den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen:

```azurecli
az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
```

>[!NOTE]
>Dieser Befehl entspricht dem Aufrufen des Portals und dem Festlegen von **Verwaltete Dienstidentität** auf **Ein** in den Webanwendungseigenschaften.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Zuweisen von Berechtigungen zu Ihrer Anwendung für das Lesen von Geheimnissen aus Key Vault

Notieren Sie sich die Ausgabe, wenn Sie die [Anwendung in Azure veröffentlichen][]. Sie sollte das folgende Format haben:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Führen Sie anschließend diesen Befehl aus, indem Sie den Namen Ihrer Key Vault-Instanz und den Wert von PrincipalId verwenden, den Sie oben kopiert haben:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get

```

## <a name="next-steps"></a>Nächste Schritte

* [Azure Key Vault – Startseite](https://azure.microsoft.com/services/key-vault/)
* [Azure Key Vault – Dokumentation](https://docs.microsoft.com/azure/key-vault/)
* [Azure SDK für .NET](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST-API-Referenz](https://docs.microsoft.com/rest/api/keyvault/)

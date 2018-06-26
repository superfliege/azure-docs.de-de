---
title: Erstellen einer statischen HTML-Web-App in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Web-Apps in Azure App Service ausführen, indem Sie eine statische HTML-Beispiel-App erstellen.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 9002d0636a5abaf24cc2bcd1e531f38ec5c8d2eb
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294011"
---
# <a name="create-a-static-html-web-app-in-azure"></a>Erstellen einer statischen HTML-Web-App in Azure

[Azure-Web-Apps](app-service-web-overview.md) bietet einen hoch skalierbaren Webhostingdienst mit Self-Patching.  In diesem Schnellstart wird erläutert, wie Sie eine einfache Website mit HTML und CSS in Azure-Web-Apps bereitstellen. In dieser Schnellstartanleitung wird [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) verwendet. Die Befehle können aber auch lokal über die [Azure CLI](/cli/azure/install-azure-cli) ausgeführt werden.

![Startseite der Beispiel-App](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="install-web-app-extension-for-cloud-shell"></a>Installieren der Web-App-Erweiterung für Cloud Shell

Für diese Schnellstartanleitung müssen Sie die [az-Web-App-Erweiterung](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az-extension-add) hinzufügen. Falls die Erweiterung bereits installiert ist, sollten Sie sie auf die neueste Version aktualisieren. Geben Sie zum Aktualisieren der Web-App-Erweiterung Folgendes ein: `az extension update -n webapp`.

Führen Sie zum Installieren der Web-App-Erweiterung den folgenden Befehl aus:

```bash
az extension add -n webapp
```

Wenn die Erweiterung installiert wurde, zeigt Cloud Shell Informationen wie im folgenden Beispiel an:

```bash
The installed extension 'webapp' is in preview.
```

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Erstellen Sie in Cloud Shell ein Schnellstartverzeichnis, und wechseln Sie dorthin.

```bash
mkdir quickstart

cd quickstart
```

Führen Sie als Nächstes den folgenden Befehl aus, um das Beispiel-App-Repository in Ihrem Schnellstartverzeichnis zu klonen.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Wechseln Sie zum Verzeichnis mit dem Beispielcode, und führen Sie den Befehl `az webapp up` aus.

Ersetzen Sie im folgenden Beispiel „<app_name>“ durch einen eindeutigen App-Namen.

```bash
cd html-docs-hello-world

az webapp up -n <app_name>
```

Der Befehl `az webapp up` bewirkt Folgendes:

- Erstellen einer Standardressourcengruppe

- Erstellen eines standardmäßigen App Service-Plans

- Erstellen einer App mit dem angegebenen Namen

- [Bereitstellen von ZIP-Dateien](https://docs.microsoft.com/en-us/azure/app-service/app-service-deploy-zip) aus dem aktuellen Arbeitsverzeichnis für die Web-App

Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen. Während der Ausführung werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:

```json
{
  "app_url": "https://<app_name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app_name>",
  "os": "Windows",
  "resourcegroup": "appsvc_rg_Windows_CentralUS ",
  "serverfarm": "appsvc_asp_Windows_CentralUS",
  "sku": "FREE",
  "src_path": "/home/username/quickstart/html-docs-hello-world ",
  < JSON data removed for brevity. >
}
```

Notieren Sie sich den Wert für `resourceGroup`. Sie benötigen ihn im Abschnitt [Bereinigen von Ressourcen](#clean-up-resources).

## <a name="browse-to-the-app"></a>Navigieren zur App

Wechseln Sie in einem Browser zur URL der Azure-Web-App: `http://<app_name>.azurewebsites.net`.

Die Seite wird als Azure App Service-Web-App ausgeführt.

![Startseite der Beispiel-App](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Glückwunsch!** Sie haben Ihre erste HTML-App für App Service bereitgestellt.

## <a name="update-and-redeploy-the-app"></a>Aktualisieren und erneutes Bereitstellen der App

Geben Sie in Cloud Shell `nano index.html` ein, um den Nano-Text-Editor zu öffnen. Ändern Sie in der H1-Überschrift „Azure App Service – Statische HTML-Beispielsite“ in „Azure App Service“ wie nachfolgend gezeigt.

![Nano index.html](media/app-service-web-get-started-html/nano-index-html.png)

Speichern Sie Ihre Änderungen, und beenden Sie Nano. Verwenden Sie `^O` zum Speichern und `^X` zum Beenden.

Nun stellen Sie die App mit dem gleichen `az webapp up`-Befehl erneut bereit.

```bash
az webapp up -n <app_name>
```

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt **Navigieren zur App** geöffnet wurde, und aktualisieren Sie die Seite.

![Aktualisierte Startseite der Beispiel-App](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Verwalten Ihrer neuen Azure-Web-App

Wechseln Sie zum <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>, um die erstellte Web-App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-get-started-html/portal1.png)

Die Übersichtsseite Ihrer Web-App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen.

![App Service-Blatt im Azure-Portal](./media/app-service-web-get-started-html/portal2.png)

Im linken Menü werden verschiedene Seiten für die Konfiguration Ihrer App angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, löschen Sie die Ressourcengruppe, indem Sie den folgenden Befehl in Cloud Shell ausführen. Denken Sie daran, dass der Name der Ressourcengruppe im Schritt [Erstellen einer Web-App](#create-a-web-app) automatisch für Sie generiert wurde.

```bash
az group delete --name appsvc_rg_Windows_CentralUS
```

Die Ausführung dieses Befehls kann eine Minute in Anspruch nehmen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zuordnen einer benutzerdefinierten Domäne](app-service-web-tutorial-custom-domain.md)

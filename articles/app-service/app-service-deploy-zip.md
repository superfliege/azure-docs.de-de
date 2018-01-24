---
title: Bereitstellen der App in Azure App Service mit einer ZIP-Datei | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ihre App mit einer ZIP-Datei für Azure App Service bereitstellen."
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: cephalin;sisirap
ms.openlocfilehash: a0e4df0ef0a1c873f1efcac1d8dbfe3cada18218
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-file"></a>Bereitstellen der App in Azure App Service mit einer ZIP-Datei

In diesem Artikel erfahren Sie, wie Sie Ihre Web-App mithilfe einer ZIP-Datei in [Azure App Service](app-service-web-overview.md) bereitstellen. 

Bei dieser Bereitstellung per ZIP-Datei wird der gleiche Kudu-Dienst verwendet, der auch bei der Continuous Integration-basierten Bereitstellungen zum Einsatz kommt. Kudu unterstützt die folgenden Funktionen für die Bereitstellung per ZIP-Datei: 

- Löschen von Dateien aus einer vorherigen Bereitstellung
- Aktivieren des Standarderstellungsprozesses, der die Paketwiederherstellung umfasst
- [Anpassen der Bereitstellung](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), einschließlich der Ausführung von Bereitstellungsskripts  
- Bereitstellungsprotokolle 

Weitere Informationen finden Sie in der [Kudu-Dokumentation](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="prerequisites"></a>Voraussetzungen

Zur Durchführung der in diesem Artikel aufgeführten Schritte ist Folgendes erforderlich:

* [Erstellen Sie eine App Service-App](/azure/app-service/), oder verwenden Sie eine App, die Sie für ein anderes Tutorial erstellt haben.

## <a name="create-a-project-zip-file"></a>Erstellen einer ZIP-Datei für das Projekt

>[!NOTE]
> Wenn Sie die Dateien in einer ZIP-Datei heruntergeladen haben, extrahieren Sie diese zunächst. Wenn Sie z.B. eine ZIP-Datei von GitHub heruntergeladen haben, können Sie diese nicht ohne Anpassungen bereitstellen. GitHub fügt zusätzliche geschachtelte Verzeichnisse hinzu, die in App Service nicht funktionieren. 
>

Navigieren Sie in einem lokalen Terminalfenster zum Stammverzeichnis Ihres App-Projekts. 

Dieses Verzeichnis sollte die Eingabedatei für Ihre Web-App enthalten, z.B. _index.html_, _index.php_ oder _app.js_. Sie kann auch Dateien zur Paketverwaltung enthalten, z.B. _project.json_, _composer.json_, _package.json_, _bower.json_ oder _requirements.txt_.

Erstellen Sie ein ZIP-Archiv mit allen Elementen Ihres Projekts. Mit dem folgenden Befehl wird das Standardtool in Ihrem Terminal verwendet:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="upload-zip-file-to-cloud-shell"></a>Hochladen der ZIP-Datei in die Cloud-Shell

Wenn Sie die Azure-Befehlszeilenschnittstelle lieber von Ihrem lokalen Terminal ausführen möchten, überspringen Sie diesen Schritt.

Führen Sie die hier beschriebenen Schritte aus, um die ZIP-Datei in die Cloud-Shell hochzuladen. 

[!INCLUDE [app-service-web-upload-zip.md](../../includes/app-service-web-upload-zip-no-h.md)]

Weitere Informationen finden Sie unter [Beibehalten von Dateien in Azure Cloud Shell](../cloud-shell/persisting-shell-storage.md).

## <a name="deploy-zip-file"></a>Bereitstellen der ZIP-Datei

Stellen Sie die hochgeladene ZIP-Datei mit dem Befehl [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) für Ihre Web-App bereit. Wenn Sie nicht Cloud-Shell verwenden möchten, müssen Sie sicherzustellen, dass Sie mindestens Version 2.0.21 der Azure-Befehlszeilenschnittstelle verwenden. Sie ermitteln die ausgeführte Version, indem Sie den Befehl `az --version` im lokalen Terminalfenster ausführen. 

Im folgenden Beispiel wird die ZIP-Datei bereitgestellt, die Sie hochgeladen haben. Wenn Sie eine lokale Installation der Azure-Befehlszeilenschnittstelle verwenden, geben Sie den Pfad zu Ihrer lokalen ZIP-Datei als `--src` an.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Mit diesem Befehl werden die Dateien und Verzeichnisse aus der ZIP-Datei in Ihrem Standardordner der App Service-Anwendung (`\home\site\wwwroot`) bereitgestellt, und die App wird neu gestartet. Falls ein zusätzlicher benutzerdefinierter Buildprozess konfiguriert wurde, wird dieser ebenfalls ausgeführt. Weitere Informationen finden Sie in der [Kudu-Dokumentation](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

Zum Anzeigen der Liste der Bereitstellungen für diese App müssen Sie die REST-APIs verwenden (siehe nächster Abschnitt). 

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="next-steps"></a>Nächste Schritte

Für komplexere Bereitstellungsszenarien versuchen Sie die [Bereitstellung in Azure mit Git](app-service-deploy-local-git.md). Die Git-basierte Bereitstellung in Azure ermöglicht Versionskontrolle, Paketwiederherstellung, MSBuild und mehr.

## <a name="more-resources"></a>Weitere Ressourcen

* [Kudu: Bereitstellen aus einer ZIP-Datei](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Anmeldeinformationen für die Azure App Service-Bereitstellung](app-service-deploy-ftp.md)

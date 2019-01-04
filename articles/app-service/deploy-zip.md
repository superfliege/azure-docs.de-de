---
title: Bereitstellen von Code mit einer ZIP- oder WAR-Datei – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre App mit einer ZIP-Datei (oder für Java-Entwickler mit einer WAR-Datei) in Azure App Service bereitstellen.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: cephalin;sisirap
ms.custom: seodec18
ms.openlocfilehash: 011caaad2f9cb3b0b891df172002dcb6b6aa8206
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53607050"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Bereitstellen der App in Azure App Service mit einer ZIP- oder WAR-Datei

In diesem Artikel erfahren Sie, wie Sie Ihre Web-App mithilfe einer ZIP- oder WAR-Datei in [Azure App Service](overview.md) bereitstellen. 

Bei dieser Bereitstellung per ZIP-Datei wird der gleiche Kudu-Dienst verwendet, der auch bei der Continuous Integration-basierten Bereitstellungen zum Einsatz kommt. Kudu unterstützt die folgenden Funktionen für die Bereitstellung per ZIP-Datei: 

- Löschen von Dateien aus einer vorherigen Bereitstellung
- Aktivieren des Standarderstellungsprozesses, der die Paketwiederherstellung umfasst
- [Anpassen der Bereitstellung](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), einschließlich der Ausführung von Bereitstellungsskripts  
- Bereitstellungsprotokolle 

Weitere Informationen finden Sie in der [Kudu-Dokumentation](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

Bei der Bereitstellung mit einer WAR-Datei wird Ihre [WAR](https://wikipedia.org/wiki/WAR_(file_format))-Datei in App Service bereitgestellt, um dort Ihre Java-Web-App auszuführen. Weitere Informationen finden Sie unter [Bereitstellen einer WAR-Datei](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

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

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>Bereitstellen einer ZIP-Datei mit der Azure-Befehlszeilenschnittstelle

Vergewissern Sie sich, dass Sie mindestens Version 2.0.21 von Azure CLI verwenden. Sie ermitteln die ausgeführte Version, indem Sie den Befehl `az --version` im Terminalfenster ausführen.

Stellen Sie die hochgeladene ZIP-Datei mit dem Befehl [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) für Ihre Web-App bereit.  

Im folgenden Beispiel wird die ZIP-Datei bereitgestellt, die Sie hochgeladen haben. Wenn Sie eine lokale Installation der Azure-Befehlszeilenschnittstelle verwenden, geben Sie den Pfad zu Ihrer lokalen ZIP-Datei als `--src` an.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Mit diesem Befehl werden die Dateien und Verzeichnisse aus der ZIP-Datei in Ihrem Standardordner der App Service-Anwendung (`\home\site\wwwroot`) bereitgestellt, und die App wird neu gestartet. Falls ein zusätzlicher benutzerdefinierter Buildprozess konfiguriert wurde, wird dieser ebenfalls ausgeführt. Weitere Informationen finden Sie in der [Kudu-Dokumentation](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>Bereitstellen einer WAR-Datei

Senden Sie zum Bereitstellen einer WAR-Datei in App Service eine POST-Anforderung an https://<App_Name>.scm.azurewebsites.net/api/wardeploy. Die POST-Anforderung muss die WAR-Datei im Nachrichtentext enthalten. Die Anmeldeinformationen für die Bereitstellung für Ihre App werden in der Anforderung mithilfe von HTTP-Standardauthentifizierung bereitgestellt. 

Für die HTTP-Standardauthentifizierung benötigen Sie die Anmeldeinformationen für die App Service-Bereitstellung. Informationen zum Festlegen der Anmeldeinformationen für Ihre Bereitstellung finden Sie unter [Festlegen und Zurücksetzen von Anmeldeinformationen auf Benutzerebene](deploy-configure-credentials.md#userscope).

### <a name="with-curl"></a>Mit cURL

Im folgenden Beispiel wird das cURL-Tool verwendet, um eine WAR-Datei bereitzustellen. Ersetzen Sie die Platzhalter `<username>`, `<war_file_path>` und `<app_name>`. Wenn Sie von cURL dazu aufgefordert werden, geben Sie das Kennwort ein.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>Mit PowerShell

Das folgende Beispiel verwendet [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod), um eine Anforderung zu senden, die die WAR-Datei enthält. Ersetzen Sie die Platzhalter `<deployment_user>`, `<deployment_password>`, `<zip_file_path>` und `<app_name>`.

```PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Nächste Schritte

Für komplexere Bereitstellungsszenarien versuchen Sie die [Bereitstellung in Azure mit Git](deploy-local-git.md). Die Git-basierte Bereitstellung in Azure ermöglicht Versionskontrolle, Paketwiederherstellung, MSBuild und mehr.

## <a name="more-resources"></a>Weitere Ressourcen

* [Kudu: Bereitstellen aus einer ZIP-Datei](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Anmeldeinformationen für die Azure App Service-Bereitstellung](deploy-ftp.md)

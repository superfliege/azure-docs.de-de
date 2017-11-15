---
title: "Ausführen eines benutzerdefinierten Images von Docker-Hubs in Azure-Web-Apps für Container | Microsoft-Dokumentation"
description: "Informationen zum Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps für Container."
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: cephalin;wesmc
ms.custom: mvc
ms.openlocfilehash: 8e7afd89def170ce756aae9e76daf91d78cc20e0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Ausführen eines benutzerdefinierten Images von Docker-Hubs in Azure-Web-Apps für Container

App Service stellt vordefinierte Anwendungsstapel unter Linux mit Unterstützung für bestimmte Versionen bereit, z.B. PHP 7.0 und Node.js 4.5. Sie können auch ein benutzerdefiniertes Docker-Image verwenden, um Ihre Web-App in einem Anwendungsstapel auszuführen, der nicht bereits in Azure definiert ist. In diesem Schnellstartartikel wird erläutert, wie eine Web-App erstellt und das [offizielle Nginx-Docker-Image](https://hub.docker.com/r/_/nginx/) für diese bereitgestellt wird. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![In Azure ausgeführte Beispiel-App](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Erstellen einer Web-App für Container

Erstellen Sie eine [Web-App](../app-service-web-overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create). Vergessen Sie nicht, `<app name>` durch einen eindeutigen App-Namen zu ersetzen.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

Im vorhergehenden Befehl verweist `--deployment-container-image-name` auf das öffentliche Image des Docker-Hubs [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Navigieren zur App

Rufen Sie folgende URL mit Ihrem Webbrowser auf:

```bash
http://<app_name>.azurewebsites.net
```

![In Azure ausgeführte Beispiel-App](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Glückwunsch!** Sie haben ein benutzerdefiniertes Docker-Image für Web-Apps für Container bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden eines benutzerdefinierten Docker-Images](tutorial-custom-docker-image.md)

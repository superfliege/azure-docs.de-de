---
title: "Ausführen eines benutzerdefinierten Docker Hub-Images in Web-App für Container | Microsoft Docs"
description: "Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container."
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/05/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: eadc0f7eb20b9e8d1cacc79b2907559e2b2535a2
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="run-a-custom-docker-hub-image-in-web-app-for-containers"></a>Ausführen eines benutzerdefinierten Docker Hub-Images in Web-App für Container

App Service stellt vordefinierte Anwendungsstapel unter Linux mit Unterstützung für bestimmte Versionen bereit, z.B. PHP 7.0 und Node.js 4.5. Sie können auch ein benutzerdefiniertes Docker-Image verwenden, um Ihre Web-App in einem Anwendungsstapel bereitzustellen, der nicht bereits in Azure definiert ist. In diesem Schnellstartartikel wird erläutert, wie eine Web-App erstellt und ein auf Python basierendes Docker-Image für diese bereitgestellt wird. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Erstellen einer Web-App für Container

Erstellen Sie eine [Web-App](../app-service-web-overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create). Vergessen Sie nicht, `<app name>` durch einen eindeutigen App-Namen zu ersetzen.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name elnably/dockerimagetest
```

Im vorhergehenden Befehl verweist `--deployment-container-image-name` auf das öffentliche Image des Docker-Hubs [https://hub.docker.com/r/elnably/dockerimagetest/](https://hub.docker.com/r/elnably/dockerimagetest/). Den jeweiligen Inhalt können Sie unter [https://github.com/rachelappel/docker-image](https://github.com/rachelappel/docker-image) prüfen.

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
> [Erstellen einer Docker Python- und PostgreSQL-Web-App in Azure](tutorial-docker-python-postgresql-app.md)

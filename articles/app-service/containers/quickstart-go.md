---
title: "Bereitstellen einer Go-App über die Azure-Web-App für Container | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie ein Docker-Image zum Ausführen einer Go-Anwendung für Azure-Web-Apps für Container bereitstellen."
keywords: Azure App Service, Web-App, Go, Docker, Container
services: app-service
author: sptramer
manager: cfowler
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.devlang: go
ms.topic: quickstart
ms.date: 01/17/2018
ms.author: sttramer
ms.custom: mvc
ms.openlocfilehash: dbe4d7bc6f5f1d83a079993c9616206fd82a1b9d
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="deploy-a-go-app-in-azure-web-app-for-containers"></a>Bereitstellen einer Go-App über die Azure-Web-App für Container

App Service stellt vordefinierte Anwendungsstapel unter Linux mit Unterstützung für bestimmte Versionen bereit, z.B. PHP 7.0 und Node.js 4.5. Sie können auch ein benutzerdefiniertes Docker-Image verwenden, um Ihre Web-App in einem Anwendungsstapel auszuführen, der nicht bereits in Azure definiert ist. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie einen vorhandenen Docker-Container mit einer Go-Anwendung verwenden und unter Azure App Service ausführen. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-the-container"></a>Erstellen einer Web-App für den Container

Erstellen Sie eine [Web-App](../app-service-web-overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Vergessen Sie nicht, `<app name>` durch einen global eindeutigen App-Namen zu ersetzen.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name microsoft/appservice-go-quickstart
```

Im obigen Befehl zeigt `--deployment-container-image-name` auf das öffentliche Docker Hub-Image [microsoft/appservice-go-quickstart](https://hub.docker.com/r/microsoft/appservice-go-quickstart).

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

## <a name="get-data-from-the-apps-endpoint"></a>Abrufen von Daten vom Endpunkt der App

Nehmen Sie mit dem Befehl `curl` Kontakt mit dem REST-API-Endpunkt auf, der von der Anwendung des Containers bereitgestellt wird.

```bash
curl -X GET http://<app_name>.azurewebsites.net:8080/hello
```

```output
Hello world!
```

**Glückwunsch!** Sie haben ein benutzerdefiniertes Docker-Image zur Ausführung einer Go-Anwendung für Web-Apps für Container bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden eines benutzerdefinierten Docker-Images](tutorial-custom-docker-image.md)

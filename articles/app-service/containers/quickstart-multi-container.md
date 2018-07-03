---
title: Erstellen einer App mit mehreren Containern (Vorschauversion) in Azure-Web-App für Container per Docker Compose-Konfiguration
description: Stellen Sie in wenigen Minuten Ihre erste App mit mehreren Containern in Azure-Web-App für Container bereit.
keywords: Azure App Service, Web-App, Linux, Docker, Compose, mehrere Container, Container, Kubernetes
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: ec5c92415668c925fe360c0c8887fd792a121842
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753712"
---
# <a name="create-a-multicontainer-preview-app-using-web-app-for-containers"></a>Erstellen einer App mit mehreren Containern (Vorschauversion) mit Web-App für Container

[Web-App für Container](app-service-linux-intro.md) umfasst eine flexible Möglichkeit zum Verwenden von Docker-Images. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie eine App mit mehreren Containern für Web-App für Container in [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) per Docker Compose-Konfiguration bereitstellen. Befolgen Sie für Kubernetes die Kubernetes-Schritte unter [Tutorial: Erstellen einer App mit mehreren Containern (Vorschauversion) über Web-App für Container](tutorial-multi-container-app.md).

In dieser Schnellstartanleitung wird Cloud Shell verwendet. Die Befehle können aber auch lokal über die [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 oder höher) ausgeführt werden. In dieser Schnellstartanleitung wird eine Docker Compose-Konfigurationsdatei verwendet.

![Beispiel-App mit mehreren Containern für Web-App für Container][1]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Für diese Schnellstartanleitung verwenden Sie die Compose-Datei von [Docker](https://docs.docker.com/compose/wordpress/#define-the-project), aber Sie ändern sie, um Azure Database for MySQL, beständigen Speicher und Redis einzubinden. Die Konfigurationsdatei finden Sie in den [Beispielen für Azure](https://github.com/Azure-Samples/multicontainerwordpress).

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

Erstellen Sie in Cloud Shell ein Schnellstartverzeichnis, und wechseln Sie dorthin.

```bash
mkdir quickstart

cd quickstart
```

Führen Sie als Nächstes den folgenden Befehl aus, um das Beispiel-App-Repository in Ihrem Schnellstartverzeichnis zu klonen.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Erstellen Sie in Cloud Shell mit dem Befehl [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) eine Ressourcengruppe. Im folgenden Beispiel wird am Standort *South Central US* (USA, Süden-Mitte) die Ressourcengruppe *myResourceGroup* erstellt. Wenn Sie alle unterstützten Standorte für App Service unter Linux im **Standard**-Tarif anzeigen möchten, führen Sie den Befehl [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) aus.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Im Allgemeinen erstellen Sie Ressourcengruppen und Ressourcen in einer Region in Ihrer Nähe.

Nach Ausführung dieses Befehls werden die Ressourcengruppeneigenschaften in einer JSON-Ausgabe angezeigt.

## <a name="create-an-azure-app-service-plan"></a>Erstellen eines Azure App Service-Plans

Erstellen Sie in Cloud Shell mit dem Befehl [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) einen App Service-Plan in der Ressourcengruppe.

Im folgenden Beispiel wird ein App Service-Plan mit dem Namen `myAppServicePlan` in einem Linux-Container (`--is-linux`) für den Tarif **Standard** (`--sku S1`) erstellt.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

## <a name="create-a-docker-compose-app"></a>Erstellen einer Docker Compose-App

Wechseln Sie in Ihrem Cloud Shell-Terminal zum Verzeichnis `multicontainerwordpress`. Erstellen Sie eine [Web-App](app-service-linux-intro.md) mit mehreren Containern im App Service-Plan `myAppServicePlan` mithilfe des Befehls [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Vergessen Sie nicht, _\<app_name>_ durch einen eindeutigen App-Namen zu ersetzen.

```bash
cd multicontainerwordpress

az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie zur bereitgestellten App unter `http://<app_name>.azurewebsites.net`. Das Laden der App kann einige Minuten dauern. Wenn Sie einen Fehler erhalten, sollten Sie einige Minuten warten und dann den Browser aktualisieren.

![Beispiel-App mit mehreren Containern für Web-App für Container][1]

**Glückwunsch!** Sie haben in Web-App für Container eine App mit mehreren Containern erstellt.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer App mit mehreren Containern (Vorschauversion) über Web-App für Container](tutorial-multi-container-app.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
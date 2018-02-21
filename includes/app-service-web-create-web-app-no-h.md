---
title: Includedatei
description: Includedatei
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 70548db9ef98cc8fa59ebc11c1371325e63e02fc
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
Erstellen Sie in der Cloud Shell im App Service-Plan `myAppServicePlan` eine [Web-App](../articles/app-service/app-service-web-overview.md). Verwenden Sie hierfür den Befehl [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Ersetzen Sie im folgenden Beispiel *\<App_Name>* durch einen global eindeutigen App-Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Nach dem Erstellen der Web-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Sie haben eine leere Web-App mit aktivierter Git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>

Wechseln Sie zu Ihrer neu erstellten Web-App.

```
http://<app_name>.azurewebsites.net
```

Ihre neue Web-App sollte nun wie folgt aussehen:

---
title: Erstellen einer Funktions-App unter Linux in einem Azure App Service-Plan
description: Erfahren Sie, wie Sie eine Funktions-App erstellen, die unter Linux in einem App Service-Plan mithilfe der Azure CLI ausgeführt wird.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: ec7b71c7da19ecefc14696c029e63a074b498ec8
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696734"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan-preview"></a>Erstellen einer Funktions-App in Linux in einem Azure App Service-Plan (Vorschau)

Mit Azure Functions können Sie Funktionen unter Linux in einem Azure App Service-Standardcontainer hosten. Dieser Artikel führt Sie durch die Verwendung der Azure CLI zum Erstellen einer Linux-gehosteten Funktions-App in Azure, die in einem [App Service-Plan](functions-scale.md#app-service-plan) ausgeführt wird. Sie können auch [einen eigenen benutzerdefinierten Container nutzen](functions-create-function-linux-custom-image.md). Das Hosten unter Linux befindet sich derzeit in der Vorschau.

In einem App Service-Plan sind Sie für die Skalierung Ihrer Funktions-App verantwortlich. Um die serverlosen Funktionen von Azure Functions zu nutzen, können Sie Ihre Funktionen unter Linux auch in einem [Verbrauchsplan](functions-scale.md#consumption-plan) hosten.

Die folgenden Schritte können auf einem Mac oder auf einem Computer unter Windows oder Linux ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieses Thema die Azure CLI-Version 2.0.21 oder höher. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Erstellen einer Funktions-App unter Linux

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktionen unter Linux hostet. Die Funktions-App bietet eine Umgebung für die Ausführung des Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie eine Funktions-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create) mit einem Linux App Service-Plan.

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen der Funktionen-App und den Platzhalter `<storage_name>` durch den Speicherkontonamen. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Darüber hinaus sollten Sie die `<language>`-Runtime für Ihre Funktions-App auf `dotnet` (C#), `node` (JavaScript) oder `python` festlegen.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Nach Erstellung und Bereitstellung der Funktions-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Da es sich bei `myAppServicePlan` um einen Linux-Plan handelt, wird mit dem integrierten Docker-Image der Container erstellt, aus dem die Funktions-App unter Linux ausgeführt wird.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources-simple.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird beschrieben, wie Sie eine unter Linux gehostete Funktions-App in Azure erstellen. Sie können jetzt ein [Funktionsprojekt](https://docs.microsoft.com/cli/azure/functionapp/deployment/source?view=azure-cli-latest) für diese Funktions-App bereitstellen. Mithilfe der Azure Functions Core Tools können Sie ein [Functions-Projekt](functions-run-local.md) auf dem lokalen Computer erstellen und für Ihre neue Linux-Funktions-App bereitstellen.  

> [!div class="nextstepaction"] 
> [Lokales Codieren und Testen von Azure Functions](functions-run-local.md)

---
title: Erstellen Ihrer ersten Funktion unter Linux in der Azure-Befehlszeilenschnittstelle (Vorschauversion) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Ihre erste Azure-Funktion für ein Linux-Image mit der Azure-Befehlszeilenschnittstelle erstellen."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/07/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4adc25dbca06271382dc76690c75d3198d59d4be
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Erstellen Ihrer ersten Funktion unter Linux in der Azure-Befehlszeilenschnittstelle (Vorschauversion)

Mit Azure Functions können Sie Funktionen unter Linux in einem Azure App Service-Standardcontainer hosten. Diese Funktion befindet sich derzeit in der Vorschauphase. Sie können auch [einen eigenen benutzerdefinierten Container nutzen](functions-create-function-linux-custom-image.md). 

In diesem Schnellstartthema wird schrittweise erläutert, wie Sie mithilfe von Azure Functions und der Azure CLI die erste eigene Funktions-App unter Linux erstellen, die im App Service-Standardcontainer gehostet wird. Der Funktionscode selbst wird für das Image über ein GitHub-Beispielrepository bereitgestellt.    

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen 

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

+ Ein aktives [GitHub](https://github.com)-Konto. 
+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieses Thema die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

Linux-Hosting für Functions wird derzeit lediglich für einen App Service-Plan unterstützt. Das Hosting für einen Verbrauchsplan wird noch nicht unterstützt. Weitere Informationen zum Hosting finden Sie unter [Vergleich von Hostingplänen für Azure Functions](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Erstellen einer Funktions-App unter Linux

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktionen unter Linux hostet. Die Funktions-App bietet eine Umgebung für die Ausführung des Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie eine Funktions-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#create) mit einem Linux App Service-Plan. 

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen der Funktionen-App und den Platzhalter `<storage_name>` durch den Speicherkontonamen. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Bei dem Parameter _deployment-source-url_ handelt es sich um ein Beispielrepository in GitHub, das eine Funktion vom Typ „Hallo Welt“ mit HTTP-Trigger enthält.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
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

>[!NOTE]  
>Beispielrepository enthält derzeit die zwei Skriptdateien [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) und [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). Die Datei „.deployment“ weist den Bereitstellungsprozess an, „deploy.sh“ als [benutzerdefiniertes Bereitstellungsskript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) zu verwenden. In der aktuellen Vorschauversion werden Skripts benötigt, um die Funktions-App für ein Linux-Image bereitzustellen.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

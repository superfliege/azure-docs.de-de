---
title: Erstellen Sie Ihre erste Funktion mit der Azure-CLI
description: Erfahren Sie, wie Sie Ihre erste Azure-Funktion für die serverlose Ausführung mit der Azure CLI und Azure Functions Core Tools erstellen.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 09/10/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 07a079e00963f1f5aff96369649e2e4fb248aae0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985997"
---
# <a name="create-your-first-function-from-the-command-line"></a>Erstellen Ihrer ersten Funktion über die Befehlszeile

In diesem Schnellstartthema erfahren Sie Schritt für Schritt, wie Sie Ihre erste Funktion über Befehlszeile oder Terminal erstellen. Sie erstellen über die Azure-Befehlszeilenschnittstelle eine Funktionen-App. Hierbei handelt es sich um die [serverlose](https://azure.microsoft.com/solutions/serverless/) Infrastruktur, die Ihre Funktion hostet. Das Funktionscodeprojekt wird mithilfe der [Azure Functions Core Tools](functions-run-local.md), mit denen auch das Funktions-App-Projekt in Azure bereitgestellt wird, aus einer Vorlage generiert.

Die folgenden Schritte können auf einem Mac oder auf einem Computer unter Windows oder Linux ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

+ Installieren Sie [Azure Core Tools-Version 2.x](functions-run-local.md#v2).

+ Installieren Sie die [Azure CLI]( /cli/azure/install-azure-cli). Für diesen Artikel ist die Azure CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Sie können ebenso [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.

+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Erstellen des lokalen Funktions-App-Projekts

Führen Sie den folgenden Befehl zum Erstellen eines Funktions-App-Projekts im `MyFunctionProj`-Ordner des aktuellen lokalen Verzeichnisses über die Befehlszeile aus. Ein GitHub-Repository wird ebenfalls in `MyFunctionProj` erstellt.

```bash
func init MyFunctionProj
```

Wählen Sie bei entsprechender Aufforderung eine Workerruntime aus den folgenden Sprachoptionen aus:

+ `dotnet`: Erstellt ein .NET-Klassenbibliotheksprojekt (.csproj).
+ `node`: Erstellt ein JavaScript-Projekt.

Wenn der Befehl ausgeführt wird, sehen Sie etwa folgende Ausgabe:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Verwenden Sie den folgenden Befehl, um zu dem neuen Projektordner `MyFunctionProj` zu navigieren.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen der Funktionen-App und den Platzhalter `<storage_name>` durch den Speicherkontonamen. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Bei dem Parameter _deployment-source-url_ handelt es sich um ein Beispielrepository in GitHub, das eine Funktion vom Typ „Hallo Welt“ mit HTTP-Trigger enthält.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name>  
```

Wenn Sie den Parameter _consumption-plan-location_ festlegen, wird die Funktions-App im Rahmen eines Hostingplans vom Typ „Verbrauch“ gehostet. Bei diesem serverlosen Plan werden Ressourcen dynamisch nach Bedarf für Ihre Funktionen hinzugefügt, und Sie bezahlen nur, wenn Funktionen ausgeführt werden. Weitere Informationen finden Sie unter [Auswählen des richtigen Serviceplans](functions-scale.md).

Nach Erstellung der Funktionen-App zeigt die Azure-CLI Informationen wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
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

## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Core Tools-Version 2.x erstellt Projekte mithilfe von Vorlagen für die Azure Functions 2.x-Runtime. Aus diesem Grund müssen Sie sicherstellen, dass die Version 2.x-Runtime in Azure verwendet wird. Durch das Festlegen der Anwendungseinstellung `FUNCTIONS_WORKER_RUNTIME` auf `~2` wird für die Funktions-App die aktuelle 2.x-Version festgelegt. Legen Sie Anwendungseinstellungen mit dem Befehl [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) fest.

Im folgenden Azure CLI-Befehl steht „<app_name>“ für den Namen Ihrer Funktions-App.

```azurecli-interactive
az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings FUNCTIONS_WORKER_RUNTIME=~2
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

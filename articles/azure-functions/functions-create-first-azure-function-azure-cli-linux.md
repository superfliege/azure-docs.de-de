---
title: Erstellen Ihrer ersten Funktion unter Linux in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Functions Core Tools und der Azure-Befehlszeilenschnittstelle Ihre erste Funktion erstellen, die unter Linux in Azure gehostet wird.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 09/12/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: multiple
manager: jeconnoc
ms.openlocfilehash: 1045e0cc0d114bb8b35e6136a2054b3642eac7e8
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249869"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Erstellen Ihrer ersten unter Linux gehosteten Funktion unter Verwendung von Core Tools und der Azure-Befehlszeilenschnittstelle (Vorschauversion)

Mit Azure Functions können Sie Code in einer [serverlosen](https://azure.microsoft.com/overview/serverless-computing/) Linux-Umgebung ausführen, ohne vorher einen virtuellen Computer erstellen oder eine Webanwendung veröffentlichen zu müssen. Das Hosten unter Linux ist derzeit als Vorschauversion verfügbar und erfordert [die Functions 2.0-Runtime](functions-versions.md).

In diesem Schnellstartartikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle Ihre erste unter Linux ausgeführte Funktions-App erstellen. Der Funktionscode wird lokal erstellt und dann über [Azure Functions Core Tools](functions-run-local.md) in Azure bereitgestellt.

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt. In diesem Artikel wird gezeigt, wie Sie Funktionen in JavaScript oder C# erstellen.

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

Wenn Sie aufgefordert werden, wählen Sie mit den Pfeiltasten eine Workerruntime aus den folgenden Sprachauswahlmöglichkeiten aus:

+ `dotnet`: Erstellt ein .NET-Klassenbibliotheksprojekt (.csproj).
+ `node`: Erstellt ein JavaScript-Projekt.

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Erstellen einer Linux-Funktions-App in Azure

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktionen unter Linux hostet. Die Funktions-App bietet eine serverlose Umgebung für die Ausführung Ihres Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az_functionapp_create) eine unter Linux ausführbare Funktions-App.

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen für die Funktions-App und `<storage_name>` durch den Speicherkontonamen. `<app_name>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein.

```azurecli
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--location "westus" --is-linux
```

> [!NOTE]
> Sollte bei Ihnen bereits eine Ressourcengruppe namens `myResourceGroup` mit Linux-fremden App Service-Apps vorhanden sein, müssen Sie eine andere Ressourcengruppe verwenden. In einer Ressourcengruppe können nicht gleichzeitig Windows- und Linux-Apps gehostet werden.  

Nach Erstellung der Funktions-App wird die folgende Meldung angezeigt:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Nun können Sie Ihr Projekt in der neuen Funktions-App in Azure veröffentlichen.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie Ihre Funktions-App in einem Azure App Service-Standardcontainer hosten. Funktionen unter Linux können auch in einem eigenen benutzerdefinierten Container gehostet werden.

> [!div class="nextstepaction"] 
> [Erstellen einer Funktion in Linux mit einem benutzerdefinierten Image (Vorschau)](functions-create-function-linux-custom-image.md)

---
title: Erstellen Ihrer ersten Funktion unter Linux in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Functions Core Tools und der Azure-Befehlszeilenschnittstelle Ihre erste Funktion erstellen, die unter Linux in Azure gehostet wird.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: javascript
manager: jeconnoc
ms.openlocfilehash: 6597e0058176eaa819170a494e4908ab44456360
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850532"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Erstellen Ihrer ersten unter Linux gehosteten Funktion unter Verwendung von Core Tools und der Azure-Befehlszeilenschnittstelle (Vorschauversion)

Mit Azure Functions können Sie Code in einer [serverlosen](https://azure.microsoft.com/overview/serverless-computing/) Linux-Umgebung ausführen, ohne vorher einen virtuellen Computer erstellen oder eine Webanwendung veröffentlichen zu müssen. Das Hosten unter Linux ist derzeit als Vorschauversion verfügbar und erfordert [die Functions 2.0-Runtime](functions-versions.md). Weitere Informationen zu den besonderen Aspekten in Bezug auf die Vorschauversion, was die Ausführung Ihrer Funktions-Apps unter Linux betrifft, finden Sie im Artikel zu [Functions unter Linux](https://aka.ms/funclinux).

In diesem Schnellstartartikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle Ihre erste unter Linux ausgeführte Funktions-App erstellen. Der Funktionscode wird lokal erstellt und dann über [Azure Functions Core Tools](functions-run-local.md) in Azure bereitgestellt.

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt. In diesem Artikel wird gezeigt, wie Sie Funktionen in JavaScript oder C# erstellen. Informationen zum Erstellen von Python-Funktionen finden Sie unter [Erstellen Ihrer ersten Python-Funktion in Azure (Vorschauversion)](functions-create-first-function-python.md).

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
+ `python`: Erstellt ein Python-Projekt. Informationen zu Python-Funktionen finden Sie in der [Schnellstartanleitung für Python](functions-create-first-function-python.md).

Wenn der Befehl ausgeführt wird, sehen Sie etwa folgende Ausgabe:

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

## <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Erstellen einer Linux-Funktions-App in Azure

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktionen unter Linux hostet. Die Funktions-App bietet eine serverlose Umgebung für die Ausführung Ihres Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create) eine unter Linux ausführbare Funktions-App.

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen für die Funktions-App und `<storage_name>` durch den Speicherkontonamen. `<app_name>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein. Darüber hinaus sollten Sie die `<language>`-Runtime für Ihre Funktions-App auf `dotnet` (C#), `node` (JavaScript) oder `python` festlegen.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
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

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
ms.openlocfilehash: f65ff3e38e3768dccf04e9eaa4345ffcea4daa9e
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451119"
---
# <a name="create-your-first-function-from-the-command-line"></a>Erstellen Ihrer ersten Funktion über die Befehlszeile

In diesem Schnellstartthema erfahren Sie Schritt für Schritt, wie Sie Ihre erste Funktion über Befehlszeile oder Terminal erstellen. Sie erstellen über die Azure-Befehlszeilenschnittstelle eine Funktionen-App. Hierbei handelt es sich um die [serverlose](https://azure.microsoft.com/overview/serverless-computing/) Infrastruktur, die Ihre Funktion hostet. Das Funktionscodeprojekt wird mithilfe der [Azure Functions Core Tools](functions-run-local.md), mit denen auch das Funktions-App-Projekt in Azure bereitgestellt wird, aus einer Vorlage generiert.

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

Wenn Sie aufgefordert werden, wählen Sie mit den Pfeiltasten eine Workerruntime aus den folgenden Sprachauswahlmöglichkeiten aus:

+ `dotnet`: Erstellt ein .NET-Klassenbibliotheksprojekt (.csproj).
+ `node`: Erstellt ein JavaScript-Projekt.

Wenn der Befehl ausgeführt wird, sehen Sie etwa folgende Ausgabe:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

## <a name="create-a-function"></a>Erstellen einer Funktion

Der folgende Befehl navigiert zu dem neuen Projekt und erstellt eine per HTTP ausgelöste Funktion namens `MyHtpTrigger`.

```bash
cd MyFunctionProj
func new --name MyHttpTrigger --template "HttpTrigger"
```

Wenn der Befehl ausgeführt wird, sehen Sie etwa folgende Ausgabe, die eine JavaScript-Funktion ist:

```output
Writing C:\functions\MyFunctionProj\MyHttpTrigger\index.js
Writing C:\functions\MyFunctionProj\MyHttpTrigger\sample.dat
Writing C:\functions\MyFunctionProj\MyHttpTrigger\function.json
```

## <a name="edit-the-function"></a>Bearbeiten der Funktion

Standardmäßig erstellt die Vorlage eine Funktion, die bei Anforderungen einen Funktionsschlüssel benötigt. Um das Testen der Funktion in Azure zu vereinfachen, müssen Sie die Funktion aktualisieren, um anonymen Zugriff zuzulassen. Wie Sie diese Änderung vornehmen, hängt von Ihrer Funktionenprojektsprache ab.

### <a name="c"></a>C\#

Öffnen Sie die MyHttpTrigger.cs-Codedatei, die Ihre neue Funktion ist, aktualisieren Sie das **AuthorizationLevel**-Attribut in der Funktionsdefinition mit dem Wert `anonymous`, und speichern Sie die Änderungen.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>JavaScript

Öffnen Sie die Datei „function.json“ für Ihre neue Funktion, öffnen Sie sie in einem Text-Editor, aktualisieren Sie die **authLevel**-Eigenschaft in **bindings.httpTrigger** in `anonymous`, und speichern Sie die Änderungen.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Jetzt können Sie die Funktion in Azure aufrufen, ohne den Funktionsschlüssel anzugeben. Der Funktionsschlüssel ist bei lokaler Ausführung nie erforderlich.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Führen Sie den folgenden Befehl aus, um die Funktions-App zu starten. Die App wird mit der gleichen Azure Functions-Runtime ausgeführt, die in Azure vorliegt.

```bash
func host start --build
```

Die `--build`-Option ist erforderlich, um C#-Projekte zu kompilieren. Für ein JavaScript-Projekt benötigen Sie diese Option nicht.

Wenn der Functions-Host startet, schreibt er eine Ausgabe ähnlich der folgenden, die zur besseren Lesbarkeit beschnitten wurde:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieren Sie die URL Ihrer `HTTPTrigger`-Funktion aus der Runtimeausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Hier ist die Antwort des Browsers auf die von der lokalen Funktion zurückgegebenen GET-Anforderung abgebildet:

![Lokales Testen im Browser](./media/functions-create-first-azure-function-azure-cli/functions-test-local-browser.png)

Jetzt haben Sie die Funktion lokal ausgeführt und können die Funktions-App und andere erforderliche Ressourcen in Azure erstellen.

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

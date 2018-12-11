---
title: Erstellen Ihrer ersten Python-Funktion in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Functions Core Tools und der Azure CLI Ihre erste Python-Funktion in Azure erstellen.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 08/29/2018
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 1ab6bb341c0b21db1e50f7dcada5b0a6b4267a49
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52853365"
---
# <a name="create-your-first-python-function-in-azure-preview"></a>Erstellen Ihrer ersten Python-Funktion in Azure (Vorschauversion)

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

In diesem Schnellstartartikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) Ihre erste [serverlose](https://azure.microsoft.com/overview/serverless-computing/) Python-Funktions-App erstellen, die unter Linux ausgeführt wird. Der Funktionscode wird lokal erstellt und dann über [Azure Functions Core Tools](functions-run-local.md) in Azure bereitgestellt. Weitere Informationen zu den besonderen Aspekten in Bezug auf die Vorschauversion, was die Ausführung Ihrer Funktions-Apps unter Linux betrifft, finden Sie im Artikel zu [Functions unter Linux](https://aka.ms/funclinux).

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Für das lokale Erstellen und Testen müssen Sie Folgendes durchführen:

+ Installieren von [Python 3.6](https://www.python.org/downloads/)

+ Installieren von [Azure Functions Core Tools](functions-run-local.md#v2), Version 2.2.70 oder höher

Gehen Sie wie folgt vor, um die Veröffentlichung und Ausführung in Azure durchzuführen:

+ Installieren Sie die [Azure CLI]( /cli/azure/install-azure-cli) (Version 2.x oder höher).

+ Sie benötigen ein aktives Azure-Abonnement.
  [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Erstellen und Aktivieren einer virtuellen Umgebung

Für die Erstellung eines Functions-Projekts müssen Sie in einer virtuellen Python 3.6-Umgebung arbeiten. Führen Sie die folgenden Befehle aus, um eine virtuelle Umgebung mit dem Namen `.env` zu erstellen und zu aktivieren.

```bash
# In Bash
python3.6 -m venv .env
source .env/bin/activate

# In PowerShell
py -3.6 -m venv .env
.env\scripts\activate
```

## <a name="create-a-local-functions-project"></a>Erstellen eines lokalen Functions-Projekts

Sie können jetzt ein lokales Functions-Projekt erstellen. Dieses Verzeichnis ist das Äquivalent zu einer Funktions-App in Azure. Es kann mehrere Funktionen enthalten, die über die gleiche lokale Konfiguration und Hostingkonfiguration verfügen.

Führen Sie im Terminalfenster oder an einer Eingabeaufforderung den folgenden Befehl aus:

```bash
func init MyFunctionProj
```

Wählen Sie **python** als gewünschte Runtime aus.

```output
Select a worker runtime:
1. dotnet
2. node
3. python
```

Die Anzeige sollte in etwa wie folgt aussehen:

```output
Installing wheel package
Installing azure-functions package
Installing azure-functions-worker package
Running pip freeze
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing /MyFunctionProj/.vscode/extensions.json
```

Ein neuer Ordner mit dem Namen _MyFunctionProj_ wird erstellt. Wechseln Sie in diesen Ordner, um fortzufahren.

```bash
cd MyFunctionProj
```

## <a name="create-a-function"></a>Erstellen einer Funktion

Um eine Funktion zu erstellen, führen Sie den folgenden Befehl aus:

```bash
func new
```

Wählen Sie `HTTP Trigger` als Vorlage, und geben Sie als **Funktionsnamen** den Namen `HttpTrigger` an.

```output
Select a template:
1. Blob trigger
2. Cosmos DB trigger
3. Event Grid trigger
4. Event Hub trigger
5. HTTP trigger
6. Queue trigger
7. Service Bus Queue trigger
8. Service Bus Topic trigger
9. Timer trigger

Choose option: 5
Function name: HttpTrigger
```

Die Anzeige sollte in etwa wie folgt aussehen:

```output
Writing /MyFunctionProj/HttpTrigger/sample.dat
Writing /MyFunctionProj/HttpTrigger/__init__.py
Writing /MyFunctionProj/HttpTrigger/function.json
The function "HttpTrigger" was created successfully from the "HTTP trigger" template.
```

Ein Unterordner mit dem Namen _HttpTrigger_ wird erstellt. Er enthält die Datei `__init__.py` als primäre Skriptdatei und `function.json` als die Datei, mit der der Trigger und die Bindungen für die Funktion beschrieben werden. Weitere Informationen zum Programmiermodell finden Sie im [Entwicklerhandbuch zu Azure Functions Python](functions-reference-python.md).

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Verwenden Sie den folgenden Befehl, um den Functions-Host lokal auszuführen.

```bash
func host start
```

Wenn der Functions-Host startet, gibt er die URL Ihrer per HTTP ausgelösten Funktion aus: (Hinweis: Die Ausgabe wurde aus Gründen der besseren Lesbarkeit gekürzt.)

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
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.
...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger
```

Kopieren Sie die URL Ihrer Funktion aus der Ausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus.

    http://localhost:7071/api/HttpTrigger?name=<yourname>

Im folgenden Screenshot ist die Antwort der Funktion dargestellt, die zurückgegeben wird, wenn sie über den Browser ausgelöst wird:

![test](./media/functions-create-first-function-python/function-test-local-browser.png)

Sie können jetzt eine Funktions-App und andere erforderliche Ressourcen zur Veröffentlichung in Azure erstellen.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Erstellen einer Linux-Funktions-App in Azure

Die Funktions-App stellt eine Umgebung für die Ausführung des Funktionscodes bereit. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie mit dem Befehl [az functionapp create](/cli/azure/functionapp#az_functionapp_create) eine **unter Linux ausgeführte Python-Funktions-App**.

Führen Sie den folgenden Befehl aus, indem Sie den Platzhalter `<app_name>` durch einen eindeutigen Namen für die Funktions-App und `<storage_name>` durch den Speicherkontonamen ersetzen. `<app_name>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westus  --runtime python \
--name <app_name> --storage-account  <storage_name>
```

> [!NOTE]
> Sollte bei Ihnen bereits eine Ressourcengruppe namens `myResourceGroup` mit Linux-fremden App Service-Apps vorhanden sein, müssen Sie eine andere Ressourcengruppe verwenden. In einer Ressourcengruppe können nicht gleichzeitig Windows- und Linux-Apps gehostet werden.  

Nach der Erstellung der Funktions-App wird die folgende Meldung angezeigt:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Nun können Sie Ihr lokales Funktions-Projekt in der Funktions-App in Azure veröffentlichen.

## <a name="deploy-the-function-app-project-to-azure"></a>Bereitstellen des Funktions-App-Projekts in Azure

Führen Sie über die Azure Functions Core Tools den folgenden Befehl aus. Ersetzen Sie `<app_name>` durch den Namen Ihrer App aus dem vorherigen Schritt.

```bash
func azure functionapp publish <app_name>
```

Es wird in etwa die folgende Ausgabe angezeigt (zur besseren Lesbarkeit gekürzt):

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die Entwicklung von Azure Functions mit Python.

> [!div class="nextstepaction"]
> [Entwicklerhandbuch zu Azure Functions Python](functions-reference-python.md)
> [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

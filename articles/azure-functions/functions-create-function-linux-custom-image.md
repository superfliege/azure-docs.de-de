---
title: Erstellen einer Funktion in Linux mit einem benutzerdefinierten Image (Vorschau) | Microsoft Docs
description: "Hier erfahren Sie, wie Sie Azure Functions erstellen, die auf einem benutzerdefinierten Linux-Image ausgeführt werden."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/08/2017
ms.topic: article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 40fbbd0ec020320b26ed343aec8ac31d60b646dc
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Erstellen einer Funktion in Linux mit einem benutzerdefinierten Image (Vorschau)

Mit Azure Functions können Sie Funktionen unter Linux in einem eigenen benutzerdefinierten Container hosten. Diese Funktion befindet sich derzeit in der Vorschauphase. Das [Hosten in einem Azure App Service-Standardcontainer](functions-create-first-azure-function-azure-cli-linux.md) ist ebenfalls möglich.  

In diesem Lernprogramm erfahren Sie, wie Sie eine Funktions-App als benutzerdefiniertes Docker-Image bereitstellen. Dieses Muster ist hilfreich, wenn Sie das integrierte App Service-Containerimage anpassen müssen. Sie sollten ein benutzerdefiniertes Image verwenden, wenn Sie für Ihre Funktionen eine bestimmte Sprachversion benötigen oder eine bestimmte Abhängigkeit oder Konfiguration erforderlich ist, die nicht mit dem integrierten Image bereitgestellt wird.

Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen und Pushen eines benutzerdefinierten Images an Docker Hub mit Azure Functions. Sie verwenden dieses Image anschließend als Bereitstellungsquelle für eine Funktions-App, die unter Linux ausgeführt wird. Mit Docker erstellen und pushen Sie das Image. Mit Azure CLI erstellen Sie eine Funktions-App und stellen das Image über Docker Hub bereit. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Images mit Docker.
> * Veröffentlichen eines benutzerdefinierten Images in einer Containerregistrierung. 
> * Erstellen eines Azure-Speicherkontos. 
> * Erstellen eines Linux-App Service-Plans. 
> * Bereitstellen einer Funktions-App über Docker Hub.
> * Hinzufügen von Anwendungseinstellungen für die Funktions-App. 

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt.  

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Git](https://git-scm.com/downloads)
* Ein aktives [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* Ein [Docker-Hub-Konto](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen, und navigieren Sie dann zum Verzeichnis mit dem Beispielcode.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Erstellen des Images aus der Docker-Datei

Betrachten Sie in diesem Git-Repository _Dockerfile_. Diese Datei beschreibt die Umgebung, die zur Ausführung der Funktions-App erforderlich ist. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Wenn Sie ein Image in einer privaten Containerregistrierung hosten, sollten Sie die Verbindungseinstellungen mithilfe der **ENV**-Variablen in der Dockerfile zur Funktions-App hinzufügen. Da in diesem Lernprogramm nicht sichergestellt werden kann, dass Sie einen privaten Registrierungsschlüssel verwenden, werden die Verbindungseinstellungen aus Sicherheitsgründen [nach der Bereitstellung mit Azure CLI hinzugefügt](#configure-the-function-app).   

### <a name="run-the-build-command"></a>Ausführen des Build-Befehls
Um das Docker-Image zu erstellen, führen Sie den `docker build`-Befehl aus, und geben Sie einen Namen (`mydockerimage`) und das Tag (`v1.0.0`) ein. Ersetzen Sie `<docker-id>` durch Ihre Docker Hub-Konto-ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Der Befehl erzeugt eine Ausgabe ähnlich der Folgenden:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Lokales Testen des Images
Stellen Sie sicher, dass das erstellte Image funktioniert, indem Sie das Docker-Image in einem lokalen Container ausführen. Rufen Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) auf, und übergeben Sie ihm Namen und Tag des Images. Stellen Sie sicher, dass Sie auch den Port mit dem `-p`-Argument angeben.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Wenn das benutzerdefinierte Image in einem lokalen Docker-Container ausgeführt wird, überprüfen Sie, dass die Funktions-App und der Container korrekt ausgeführt werden, indem Sie zu <http://localhost:8080> navigieren.

![Funktions-App lokal testen.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Nachdem Sie die Funktions-App im Container überprüft haben, beenden Sie die Ausführung. Jetzt können Sie das benutzerdefinierte Image per Push zu Ihrem Docker Hub-Konto übertragen.

## <a name="push-the-custom-image-to-docker-hub"></a>Pushübertragung des benutzerdefinierten Images zu Docker Hub

Eine Registrierung ist eine Anwendung, die Images hostet und Image- sowie Container-Dienste für Dienste bereitstellt. Damit Ihr Image gemeinsam verwendet werden kann, müssen Sie es per Pushübertragung an eine Registrierung senden. Docker Hub ist eine Registrierung für Docker-Images, die Ihnen ermöglicht, Ihre eigenen Repositorys – entweder öffentlich oder privat – zu hosten. 

Bevor Sie ein Image mithilfe von Push übertragen können, müssen Sie sich mit dem Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) in Docker Hub anmelden. Ersetzen Sie `<docker-id>` durch den Namen Ihres Kontos, und geben Sie Ihr Kennwort in der Konsole an der Eingabeaufforderung ein. Weitere Kennwortoptionen für Docker Hub finden Sie in der [Dokumentation zum Docker Login-Befehl](https://docs.docker.com/engine/reference/commandline/login/) (in englischer Sprache).

```bash
docker login --username <docker-id> 
```

Eine „login succeeded“-Meldung bestätigt Ihre erfolgreiche Anmeldung. Nach der Anmeldung können Sie das Image mit dem [docker push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl per Push an Docker-Hub übertragen.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Überprüfen Sie anhand der Befehlsausgabe, ob der Pushvorgang erfolgreich war.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Jetzt können Sie dieses Image als Bereitstellungsquelle für eine neue Funktions-App in Azure verwenden. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieses Thema die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

Linux-Hosting für Funktionen wird bei Verbrauchstarifen derzeit nicht unterstützt. Sie müssen einen Linux-App Service-Plan verwenden. Weitere Informationen zum Hosting finden Sie unter [Vergleich von Hostingplänen für Azure Functions](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Erstellen und Bereitstellen des benutzerdefinierten Images

Die Funktions-App hostet die Ausführung Ihrer Funktionen. Erstellen Sie mit dem Befehl [az functionapp create](/cli/azure/functionapp#create) eine Funktions-App aus einem Docker Hub-Image. 

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen der Funktionen-App und den Platzhalter `<storage_name>` durch den Speicherkontonamen. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Wie zuvor steht `<docker-id>` für den Namen Ihres Docker-Kontos.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
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

Der Parameter _deployment-container-image-name_ gibt das in Docker Hub gehostete Image an, das zum Erstellen der Funktions-App verwendet werden soll. 


## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Die Funktion benötigt die Verbindungszeichenfolge zum Herstellen der Verbindung mit dem Standardspeicherkonto. Beim Veröffentlichen des benutzerdefinierten Images in einem privaten Containerkonto sollten Sie diese Anwendungseinstellungen als Umgebungsvariablen in der Dockerfile festlegen, indem Sie die [ENV-Anweisung](https://docs.docker.com/engine/reference/builder/#env) o.ä. verwenden. 

In diesem Fall ist `<storage_account>` der Name des Speicherkontos, das Sie erstellt haben. Rufen Sie die Verbindungszeichenfolge mit dem Befehl [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) ab. Fügen Sie diese Anwendungseinstellungen in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) hinzu.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Sie können jetzt Ihre Funktionen unter Linux in Azure testen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Images mit Docker.
> * Veröffentlichen eines benutzerdefinierten Images in einer Containerregistrierung. 
> * Erstellen eines Azure-Speicherkontos. 
> * Erstellen eines Linux-App Service-Plans. 
> * Bereitstellen einer Funktions-App über Docker Hub.
> * Hinzufügen von Anwendungseinstellungen für die Funktions-App.

Weitere Informationen zur lokalen Azure Functions-Entwicklung mit Azure Functions Core Tools.

> [!div class="nextstepaction"] 
> [Lokales Codieren und Testen von Azure Functions](functions-run-local.md)

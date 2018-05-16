---
title: Tutorial für die Integration von Azure-Funktionen in den Azure SignalR Service | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure Functions mit dem Azure SignalR Service nutzen.
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/24/2018
ms.author: wesmc
ms.openlocfilehash: b1bb6b3fe545d5a42fa985ab85bd7a914128e56b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-integrate-azure-functions-with-azure-signalr-service"></a>Tutorial: Integrieren von Azure Functions in den Azure SignalR Service

Dieses Tutorial baut auf der Chatraumanwendung auf, die in vorherigen Tutorials erstellt wurde. Falls Sie [Erstellen eines Chatraums per SignalR Service](signalr-quickstart-dotnet-core.md) und [Azure SignalR Service-Authentifizierung](./signalr-authenticate-oauth.md) nicht durchgearbeitet haben, sollten Sie dies zuerst nachholen. 

Ein häufiges Szenario für Echtzeitanwendungen ist, dass von einem Server stammende Inhaltsupdates auf Webclients veröffentlicht werden sollen. [Azure Functions](../azure-functions/functions-overview.md) ist eine hervorragende Option zum Generieren dieser Inhaltsupdates. Ein wichtiger Vorteil der Nutzung von Azure Functions ist, dass Sie Ihren Code bedarfsgesteuert ausführen können, ohne sich um die Architektur einer gesamten Anwendung oder die Infrastruktur für die Ausführung kümmern zu müssen. Außerdem zahlen Sie nur für die Zeit, in der Ihr Code auch wirklich ausgeführt wird.  

Normalerweise würde dieses Szenario ein Problem darstellen, wenn der Versuch unternommen wird, SignalR zu verwenden. Der Grund ist, dass SignalR versucht, eine Verbindung zwischen Client und Server für die Übertragung von Inhaltsupdates per Pushvorgang herzustellen. Da der Code nur bedarfsgesteuert ausgeführt wird, kann keine dauerhafte Verbindung hergestellt werden. Der Azure SignalR Service kann dieses Szenario aber unterstützen, da er die Verbindungen für Sie zur Laufzeit verwaltet.

In diesem Tutorial verwenden Sie Azure Functions zum Generieren von Nachrichten, indem Sie zu Beginn jeder Minute eine Timer-Funktion verwenden. Mit der Funktion werden die Nachrichten auf allen Clients des Chatraums veröffentlicht, der in den vorherigen Tutorials erstellt wurde. Weitere Informationen zu Timer-Funktionen finden Sie unter [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](../azure-functions/functions-create-scheduled-function.md).

Sie können einen beliebigen Code-Editor nutzen, um die Schritte dieser Schnellstartanleitung auszuführen. [Visual Studio Code](https://code.visualstudio.com/) ist aber eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

Der Code für dieses Tutorial kann aus dem [GitHub-Repository „AzureSignalR-samples“](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer) heruntergeladen werden.

![Chat-App mit Servernachrichten](./media/signalr-integrate-functions/signalr-functions-complete.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Timer-Funktion mit Azure Functions per Azure CLI
> * Konfigurieren der Timer-Funktion für die Bereitstellung des lokalen Git-Repositorys
> * Herstellen einer Verbindung für den Timer mit Ihrem SignalR Service für Pushupdates jede Minute

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Git-Client](https://git-scm.com/)
* [.NET Core SDK](https://www.microsoft.com/net/download/windows) 
* [Konfigurierte Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)
* Laden Sie das GitHub-Repository [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples) herunter, oder klonen Sie es.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen eine Funktionen-App erstellen, um die Ausführungsumgebung für Ihre Funktionen zu definieren. Außerdem können Sie mit der Funktionen-App mehrere Funktionen als logische Einheit gruppieren, um die Verwaltung, Bereitstellung und Ressourcenfreigabe zu vereinfachen. Weitere Informationen finden Sie unter [Erstellen Sie Ihre erste Funktion mit der Azure-CLI](../azure-functions/functions-create-first-azure-function-azure-cli.md).

In diesem Abschnitt verwenden Sie die Azure Cloud Shell zum Erstellen einer neuen Azure-Funktionen-App, die für die Bereitstellung aus einem lokalen Git-Repository konfiguriert ist. 

Erstellen Sie die Funktionen-App-Ressourcen in derselben Ressourcengruppe, die Sie in den vorherigen Tutorials verwendet haben. Dieser Ansatz vereinfacht die Verwaltung aller Tutorialressourcen.

Kopieren Sie das unten angegebene Skript in Ihren Text-Editor, und ersetzen Sie die Werte der Variablenparameter durch die Werte für Ihre Ressourcen. Kopieren Sie das aktualisierte Skript, und fügen Sie es in Ihre Azure Cloud Shell ein. Drücken Sie die **EINGABETASTE**, um ein Speicherkonto und eine Funktionen-App zu erstellen.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your values from previous    === 
#=== tutorials.                                                   ===
#====================================================================
ResourceGroupName=SignalRTestResources
location=eastus

#====================================================================
#=== Update these variables for the new function app and storage  ===
#=== account.                                                     ===
#====================================================================
functionappName=mysignalfunctionapp
storageAccountName=mystorageaccount

# Create a storage account to hold function app code and settings
az storage account create --resource-group $ResourceGroupName \
--name $storageAccountName \
--location $location --sku Standard_LRS

# Create the function app
az functionapp create --resource-group $ResourceGroupName \
--name $functionappName \
--consumption-plan-location $location \
--storage-account $storageAccountName

```

| Parameter | BESCHREIBUNG |
| -------------------- | --------------- |
| ResourceGroupName | Dieser Ressourcengruppenname wurde in vorherigen Tutorials vorgeschlagen. Es ist ratsam, alle Tutorialressourcen zu gruppieren. Verwenden Sie dieselbe Ressourcengruppe wie in den vorherigen Tutorials. | 
| location | Aktualisieren Sie diese Variable auf denselben Speicherort, den Sie in den vorherigen Tutorials zum Erstellen der Ressourcengruppe verwendet haben. | 
| functionappName | Aktualisieren Sie diese Variable mit einem eindeutigen Namen für Ihre neue Funktionen-App. Beispiel: signalrfunctionapp22665120. | 
| storageAccountName | Geben Sie einen Namen für das neue Speicherkonto ein, das den Code und die Einstellungen für die Funktionen-App enthalten soll. | 



## <a name="configure-the-function-app"></a>Konfigurieren der Funktionen-App

In diesem Abschnitt konfigurieren Sie die Funktionen-App mit einer App-Einstellung, die die Verbindungszeichenfolge für Ihre Azure SignalR Service-Ressource enthält. In Ihrem Funktionscode wird diese Einstellung verwendet, um eine Verbindung herzustellen und Nachrichten im Chatraum zu veröffentlichen. Außerdem konfigurieren Sie die Funktionen-App für die Bereitstellung aus einem lokalen Git-Repository.

Kopieren Sie das unten angegebene Skript, und ersetzen Sie die Werte der Parameter. Fügen Sie das aktualisierte Skript in Ihre Azure Cloud Shell ein, und drücken Sie die **EINGABETASTE**.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your resources.              === 
#====================================================================
ResourceGroupName=SignalRTestResources
functionappName=mysignalfunctionapp

#========================================================================
#=== Replace this value with the connection string for your           ===
#=== SignalR Service resource.                                        ===
#========================================================================
connstring="Endpoint=<service_endpoint>;AccessKey=<access_key>;"

# Add the SignalR Service connection string app setting
az functionapp config appsettings set --resource-group $ResourceGroupName \
    --name $functionappName \
    --setting "Azure:SignalR:ConnectionString=$connstring"

# configure for deployment from a local Git repository
az functionapp deployment source config-local-git --name $functionappName \
    --resource-group $ResourceGroupName

```

| Parameter | BESCHREIBUNG |
| -------------------- | --------------- |
| ResourceGroupName | Dieser Ressourcengruppenname wurde in vorherigen Tutorials vorgeschlagen. Es ist ratsam, alle Tutorialressourcen zu gruppieren. Verwenden Sie dieselbe Ressourcengruppe wie in den vorherigen Tutorials. | 
| functionappName | Aktualisieren Sie diese Variable mit einem eindeutigen Namen für Ihre neue Funktionen-App. Beispiel: signalrfunctionapp22665120. | 
| connstring | Geben Sie die Verbindungszeichenfolge für Ihre SignalR Service-Ressource ein. Sie können diese Verbindungszeichenfolge im Azure-Portal über die Seite mit den SignalR Service-Ressourcen abrufen, indem Sie unter **EINSTELLUNGEN** auf **Schlüssel** klicken. | 



Notieren Sie sich die URL für die Git-Bereitstellung, die mit dem letzten Befehl zurückgegeben wurde. Sie verwenden diese URL zum Bereitstellen des Funktionscodes.


## <a name="the-timer-function"></a>Timer-Funktion

Das Beispiel für die Timer-Funktion befindet sich im Verzeichnis */samples/Timer* Ihres Downloads. Alternativ hierzu können Sie auch das GitHub-Repository [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer) klonen. Der Code der Timer-Funktion befindet sich in *TimerFunction.cs*. In diesem Code wird die mit dem Standardkonfigurationsschlüssel gespeicherte Verbindungszeichenfolge (*Azure:SignalR:ConnectionString*) verwendet, um einen Proxy für den Hub zu erstellen. Da der Funktionscode serverseitig ausgeführt wird, gibt es keinen Grund, die Authentifizierung als regulärer Client zu erzwingen. Es wird darauf vertraut, dass der Code die Verbindungszeichenfolge verwendet. Mit diesem Hub-Proxy kann der Funktionscode alle Methoden aufrufen, die Sie auf Ihrem Hub definiert haben. Der Code ruft die `BroadcastMessage`-Methode auf, um eine Nachricht zu veröffentlichen, die die aktuelle Zeit bei Auslösung des Triggers enthält.

Der Trigger für den Funktionscode ist ein *timerTrigger*, der in den Bindungen unter *TimerFunction/function.json* definiert ist. Er enthält einen [CRON-Ausdruck](https://wikipedia.org/wiki/Cron#CRON_expression), mit dem der Timer-Trigger so festgelegt wird, dass er am Anfang jeder Minute ausgelöst wird.

```json
{
  "bindings": [
    {
      "type": "timerTrigger",
      "schedule": "0 * * * * *",
      "useMonitor": true,
      "runOnStartup": false,
      "name": "myTimer"
    }
  ],
  "disabled": false,
  "scriptFile": "../Timer.dll",
  "entryPoint": "Timer.TimerFunction.Run"
}
```


## <a name="building-the-timer-function"></a>Erstellen der Timer-Funktion

Verwenden Sie die [.NET Core-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/dotnet/core/tools/) in den folgenden Schritten, um die Funktion zu erstellen und für die Bereitstellung vorzubereiten:

1. Navigieren Sie zum Unterverzeichnis */samples/Timer* Ihres Downloads, oder klonen Sie das GitHub-Repository [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples).

2. Stellen Sie die NuGet-Pakete mit dem folgenden Befehl wieder her:

        dotnet restore

3. Erstellen Sie die *Timer*-Funktionen-App, indem Sie den folgenden Befehl verwenden:

        msbuild /p:Configuration=Release


## <a name="create-and-deploy-the-local-git-repo"></a>Erstellen und Bereitstellen des lokalen Git-Repositorys

1. Navigieren Sie in einer Git-Shell zum Unterverzeichnis */samples/Timer/bin/Release/net461* für den Buildvorgang.

        cd ./AzureSignalR-samples/samples/Timer/bin/Release/net461

2. Initialisieren Sie das Verzeichnis als ein neues Git-Repository, indem Sie den folgenden Befehl verwenden:

        git init

3. Fügen Sie einen neuen Commitvorgang für alle Dateien im Buildverzeichnis hinzu.

        git add -A
        git commit -v -a -m "Initial Timer function commit"        

4. Fügen Sie für die URL für die Git-Bereitstellung, die Sie sich während der Konfiguration Ihrer Funktionen-App notiert haben, einen Remoteendpunkt hinzu:

        git remote add Azure <enter your Git deployment URL>

5. Bereitstellen der Funktionen-App

        git push Azure master

   Nachdem der Code bereitgestellt wurde, wird der Timer unmittelbar jede Minute ausgelöst, um Ihren Code auszuführen.

## <a name="test-the-chat-app"></a>Testen der Chat-App

Navigieren Sie zur Chatanwendung. Die eben erstellte Timer-Funktion meldet jetzt die Zeit am Anfang jeder Minute.

![Chat-App mit Servernachrichten](./media/signalr-integrate-functions/signalr-functions-complete.png)



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit dem Testen der Anwendung fortfahren möchten, können Sie die von Ihnen erstellten Ressourcen beibehalten.

Wenn Sie die Beispielanwendung nicht mehr benötigen, können Sie die Azure-Ressourcen löschen, um das Anfallen von Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden also unwiederbringlich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Falls Sie die Ressourcen zum Hosten dieses Beispiels in einer vorhandenen Ressourcengruppe erstellt haben, die beizubehaltende Ressourcen enthält, können Sie die Ressourcen einzeln über das jeweilige Blatt löschen, anstatt die gesamte Ressourcengruppe.
> 
> 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

Geben Sie im Textfeld **Nach Name filtern...** den Namen Ihrer Ressourcengruppe ein. In diesem Artikel wurde eine Ressourcengruppe mit dem Namen *SignalRTestResources* verwendet. Klicken Sie in Ihrer Ressourcengruppe in der Ergebnisliste auf **...** und dann auf **Ressourcengruppe löschen**.

   
![Löschen](./media/signalr-integrate-functions/signalr-delete-resource-group.png)


Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie den Namen der entsprechenden Ressourcengruppe ein, und klicken Sie auf **Löschen**.
   
Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie die Integration in Azure Functions durchführen, um Updates anhand von Azure Functions-Triggern per Pushvorgang an Clients zu übertragen. Weitere Informationen zur Verwendung von Azure SignalR Server erhalten Sie, wenn Sie mit den Azure CLI-Beispielen für SignalR Service fortfahren.

> [!div class="nextstepaction"]
> [Azure SignalR-CLI-Beispiele](./signalr-cli-samples.md)




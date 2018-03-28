---
title: "Azure-Funktionen für Government | Microsoft Docs"
description: "Dies bietet eine Reihe von Schnellstarts für die Verwendung von Funktionen mit Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: yujhongmicrosoft
manager: zakramer
ms.assetid: fb11f60c-5a70-46a9-82a0-abb2a4f4239b
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 9/6/2017
ms.author: yujhong
ms.openlocfilehash: 1000c686e1c0bd400d81400de698df7645178216
ms.sourcegitcommit: 09a2485ce249c3ec8204615ab759e3b58c81d8cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="azure-functions-on-azure-government"></a>Azure-Funktionen auf Azure Government
Die Reihen des Schnellstarts unten können Sie erste Schritte mit Azure-Funktionen auf Azure Government. Verwenden von Azure mit Azure Government ähnelt der mit der Azure-kommerziellen Plattform mit einem [bis auf einige Ausnahmen](documentation-government-compute.md#azure-functions).

Weitere Informationen zum Azure-Funktionen, klicken Sie auf [hier](../azure-functions/functions-overview.md). 
## <a name="create-function---azure-cli"></a>Erstellen von Funktionen – Azure-CLI

### <a name="prerequisites"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

+ Ein aktives [GitHub](https://github.com)-Konto. 
+ Ein aktives Azure Government-Abonnement.
Wenn Sie kein Azure Government-Abonnement besitzen, erstellen Sie eine [Konto frei](https://azure.microsoft.com/overview/clouds/government/) Vorbereitungen.

+ Installiert [CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

> [!NOTE]
> In diesem Thema ist der Azure-CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Informationen dazu. 
>
>

### <a name="launch-azure-cli-20"></a>Launch Azure CLI 2.0

Sie müssen zunächst eine Verbindung mit Azure Government mit Azure-Befehlszeilenschnittstelle (CLI) gemäß [Schritte](documentation-government-get-started-connect-with-cli.md). 

Zur Verbindung mit Azure Government über CLI legen Sie die Cloud:

```azurecli-interactive
az cloud set --name AzureUSGovernment
```

Nachdem die Cloud festgelegt wurde, können Sie weiterhin anmelden:

```azurecli-interactive
az login
```
### <a name="create-a-resource-group-with-cli"></a>Erstellen Sie eine Ressourcengruppe mit CLI

> [!NOTE]
> Wenn Sie eine bereits vorhandene Ressourcengruppe verwenden möchten, können Sie diesen Abschnitt überspringen. Sie können auch über eine Ressourcengruppe erstellen die [Azure Government-Portal](https://portal.azure.us) anstatt CLI. 
> 
> 

Erstellen Sie mit dem Befehl [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az_group_create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Funktionen-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden.

Das folgende Beispiel erstellt eine Ressourcengruppe namens `myResourceGroup` mit dem Speicherort festgelegt `usgovvirginia`.
Wenn Sie CLI nicht verwenden, melden Sie sich zuerst mit der `az login` oben.

```azurecli-interactive
az group create --name myResourceGroup --location usgovvirginia
```
### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

> [!NOTE]
> Wenn Sie ein bereits vorhandenes Speicherkonto verwenden möchten, können Sie diesen Abschnitt überspringen. Sie können auch über ein Speicherkonto erstellen die [Azure Government-Portal](https://portal.azure.us) anstatt CLI. 
> 
> 

Functions verwenden ein Azure Storage-Konto, um den Status und andere Informationen über Ihre Funktionen zu verwalten. Erstellen Sie ein Speicherkonto in der Ressourcengruppe, die Sie erstellt haben, indem Sie den Befehl [az storage account create](../storage/common/storage-azure-cli.md#create-a-new-storage-account) verwenden.

Ersetzen Sie im folgenden Befehl den Platzhalter `<storage_name>` durch einen eindeutigen Speicherkontonamen. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

```azurecli-interactive
az storage account create --name <storage_name> --location usgovvirginia --resource-group myResourceGroup --sku Standard_LRS
```

Nach dem Erstellen des Speicherkontos zeigt die Azure-CLI ähnliche Informationen wie im folgenden Beispiel an:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "usgovvirginia",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.usgovcloudapi.net/",
    "file": "https://myfunctionappstorage.file.core.usgovcloudapi.net/",
    "queue": "https://myfunctionappstorage.queue.core.usgovcloudapi.net/",
    "table": "https://myfunctionappstorage.table.core.usgovcloudapi.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```
### <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Sie können Funktionen als logische Einheit für die einfachere Verwaltung, Bereitstellung und Freigeben von Ressourcen zu gruppieren. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp?view=azure-cli-latest#az_functionapp_create).

Bevor Sie Ihre Funktion-app erstellen, müssen Sie erstellen eine [App Service-Plan](../azure-functions/functions-scale.md#app-service-plan) , die Ihre app Funktion hostet. Dies kann mithilfe des folgenden Befehls erfolgen, die einen Plan mit dem Namen "ein TestPlan" erstellt hat.

```azurecli-interactive
az appservice plan create --resource-group myResourceGroup --name testPlan
```

Ersetzen Sie in den folgenden Befehl ein eindeutiger app Funktionsname, in dem Sie finden Sie unter, der `<app_name>` Platzhalter und das Speicherkonto für Namen `<storage_name>`. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan testPlan
```
>[!NOTE] 
>Standardmäßig wird eine Funktion app erstellt, mit der App Service-Plan der bedeutet, die virtuellen Computer reserviert, die Ihre App Service-apps zugewiesen werden und der Host Funktionen immer ausgeführt.
Weitere Informationen zu den App Service-Plan [klicken Sie hier](../azure-functions/functions-scale.md#app-service-plan). 
>
>

Nach Erstellung der Funktionen-App zeigt die Azure-CLI Informationen wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.us",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.us",
    "quickstart.scm.azurewebsites.us"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Nun, da Sie eine Funktion app haben, können Sie die eigentliche Funktionscode aus Bereitstellen der [GitHub-beispielrepository](https://github.com/Azure-Samples/functions-quickstart).

### <a name="deploy-your-function-code"></a>Bereitstellen Ihres Funktionscodes
 Dieser Schnellstart eine Verbindung mit einer [beispielrepository in GitHub](https://github.com/Azure-Samples/functions-quickstart) , eine Beispielfunktion enthält. Ersetzen Sie wie zuvor im folgenden Code den Platzhalter `<app_name>` durch den Namen der Funktionen-App, die Sie erstellt haben. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Nachdem die Bereitstellungsquelle festgelegt wurde, zeigt die Azure-CLI Informationen ähnlich wie im folgenden Beispiel (zwecks besserer Lesbarkeit wurden NULL-Werte entfernt):
```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "USGov Virginia",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```
### <a name="test-the-function"></a>Testen der Funktion

CURL So testen Sie die bereitgestellte Funktion auf einem Mac oder Linux-Computer verwenden oder mithilfe von Bash unter Windows, die Sie [installieren](https://msdn.microsoft.com/commandline/wsl/install_guide).  

Führen Sie den folgenden cURL-Befehl aus, und ersetzen Sie dabei den Platzhalter `<app_name>` mit dem Namen der Funktionen-App. Fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an.

```bash
curl http://<app_name>.azurewebsites.us/api/HttpTriggerJS1?name=<yourname>
```
 
![In einem Browser angezeigte Funktion-Antwort](./media/documentation-government-function2.png)  

Wenn cURL in der Befehlszeile nicht zur Verfügung steht, geben Sie dieselbe URL in die Adresse des Webbrowsers ein. Ersetzen Sie erneut den Platzhalter `<app_name>` durch den Namen der Funktionen-App, fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an, und führen Sie die Anforderung aus. 

    http://<app_name>.azurewebsites.us/api/HttpTriggerJS1?name=<yourname>

![Die Funktionsantwort wird in einem Browser angezeigt.](./media/documentation-government-function3.png)  

## <a name="create-function---visual-studio"></a>Erstellen von Funktionen – Visual Studio 

Vor dem Start: Überprüfen Sie zunächst sicherstellen, dass Ihre Visual Studio ist [mit der Azure Government-Umgebung verbunden](documentation-government-get-started-connect-with-vs.md). 

Nachdem, dass sichergestellt ist die Schritte zum Erstellen einer Funktion, die mit Visual Studio auf Azure Government sind im Wesentlichen identisch mit der Erstellung einer in Azure kommerziellen, befinden sich [hier](../azure-functions/functions-create-your-first-function-visual-studio.md). 

### <a name="test-your-function-in-azure-government"></a>Testen Sie die Funktion in Azure Government 

Nachdem die Funktion aus dem vorherigen Schritt oben bereitgestellt wurde, können wir die Funktion testen. Die URL, über die Ihre per HTTP ausgelöste Funktion aufgerufen wird, sieht wie folgt aus:

        http://<functionappname>.azurewebsites.us/api/<functionname>?name=<yourname> 

Fügen Sie diese neue URL für die HTTP-Anforderung in die Adresszeile des Browsers ein. Die folgende Abbildung zeigt die Antwort im Browser, um die remote GET-Anforderung, die von der Funktion zurückgegebenen: 

   ![Funktionsantwort im Browser](./media/documentation-government-functions-createvs.png)

## <a name="create-trigger-functions---visual-studio"></a>Erstellen Sie Triggerfunktionen - Visual Studio
Erfahren Sie, wie eine Funktion Trigger in Azure Government mithilfe von Visual Studio zu erstellen. 

### <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Visual Studio installiert wurde:
    -   [Visual Studio 2017, Version 15.3](https://www.visualstudio.com/vs/preview/), einschließlich der Workload **Azure-Entwicklung**
    
    >[!NOTE] 
    > Nach der Installation oder upgrade auf Visual Studio 2017 Version 15.3, müssen Sie auch die 2017 von Visual Studio-Tools für Azure-Funktionen manuell aktualisieren. Sie können die Tools im Menü **Extras** aktualisieren. Navigieren Sie zu **Erweiterungen und Updates...** >          **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**. 
    >
    >
* In Visual Studio ausgeführte Funktion-app
    - Zum Erstellen, führen Sie den Schnellstart weiter oben [erstellen mit Visual Studio-Funktion](documentation-government-functions.md#create-function-using-visual-studio). 
* Installiert [CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

> [!NOTE]
> In diesem Thema ist der Azure-CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Informationen dazu. 
>
>

### <a name="create-trigger-function"></a>Erstellen Sie Trigger-Funktion

1. Öffnen Sie die Funktion-app in Visual Studio, und klicken Sie mit der rechten Maustaste auf die app selbst nicht die Projektmappe. Wechseln Sie nach unten auf die Schaltfläche "Hinzufügen", und klicken Sie auf "Neues Element" wie unten dargestellt.
    
    ![triggerfunctioncreate](./media/documentation-government-function4.png)
2. Wird dieser Bildschirm wird angezeigt, und klicken Sie auf "Azure-Funktion":
    
    ![triggerfunctioncreate1](./media/documentation-government-function5.png)
    
    Wie Sie sehen können, sind mehrere Typen von Trigger-Funktionen, die Sie erstellt haben, z. B. generisch, Timer und GitHub Webhook, Blob, und in die Warteschlange. Es wird eine Zeitgeber-Trigger-Funktion in diesem Lernprogramm erstellt. 
    
3. Klicken Sie auf den Timer als Auslöser und zu erstellen, und Sie sollten die neue Funktion in Ihrer Visual Studio angezeigt werden. 
4. Klicken Sie im Projektmappen-Explorer auf der rechten Seite werden finden Sie unter, und öffnen Sie die Datei "local.settings.json" angezeigt.

    ![triggerfunctioncreate2](./media/documentation-government-function6.png)

    Um die Trigger-Funktion mit Azure Government zu verbinden, müssen wir die Appeinstellungen definieren, die in dieser Datei definiert sind. Dieser Wert wird im nächsten Schritt gefunden werden. 
5. Wechseln Sie zu Ihrer [Azure Government-Portal](https://portal.azure.us) , und klicken Sie auf "Speicherkonten" aus der linken Seite-Leiste. 

    ![triggerfunctioncreate3](./media/documentation-government-function7.png)
    
    Klicken Sie auf das Speicherkonto, das diese Funktion app entspricht, und sehen Sie einen Abschnitt "Zugriffstasten" unter "Einstellungen".
    Sobald Sie navigieren Sie zu diesem Abschnitt werden Sie zwei Standard-Schlüsseln finden Sie unter sein.

    ![triggerfunctioncreate4](./media/documentation-government-function8.png)
6. Kopieren Sie die Verbindungszeichenfolge für Ihre Key1, wechseln Sie zurück zur Datei "local.settings.json", und fügen Sie die Zeichenfolge für jedes der 3 Werte im Parameters "Values". Außerdem stellen Sie sicher, dass "AccountName" den Namen des Speicherkontos =. 

    ![triggerfunctioncreate5](./media/documentation-government-function9.png)
    
7. Wir müssen jetzt die Verbindungszeichenfolge auf Ihr Azure-Speicherkonto in unserer app-Einstellungen, die über CLI ausgeführt festgelegt. 
    >[!NOTE] 
    > Stellen Sie sicher, dass die CLI mit Azure Government verbunden ist, anhand der weiter oben im Abschnitt "Starten Azure-CLI 2.0". 
    >
    >

    Ersetzen Sie in den folgenden Befehl "ResourceGroupName" und "FunctionAppname" RFID-Transponder mit Ihren app-Informationen, und fügen Sie die Verbindungszeichenfolge aus Schritt 6 oben anstelle von "ConnectionString" Tags. Führen Sie den Befehl unter Verwendung der Azure-CLI an.

```azurecli-interactive
az functionapp config appsettings set --resource-group <resourceGroupName> --name <FunctionAppname> --settings AzureWebJobsmyconnection= "<connectionString>"

```

Sobald dies festgelegt wurde, können Sie Ihre Timer Trigger Funktion lokal ausführen. Verwenden Sie das gleiche Verfahren zum Bereitstellen von Azure-Regierung gemäß [oben](documentation-government-functions.md#create-function-using-visual-studio).

## <a name="use-azure-queues-for-output-bindings"></a>Verwenden von Azure-Warteschlangen für Ausgabebindungen

Dieses Lernprogramm führt durch die Schritte zum Aktualisieren einer vorhandenen Funktion durch das Hinzufügen von ausgabebindungen, die eine Verbindung herzustellen, und Senden von Nachrichten an den Azure-Warteschlangenspeicher von Ihrer Funktion in Visual Studio. 

### <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Visual Studio installiert wurde:

   -   [Visual Studio 2017, Version 15.3](https://www.visualstudio.com/vs/preview/), einschließlich der Workload **Azure-Entwicklung**
    
    >[!NOTE] 
    > Nach der Installation oder upgrade auf Visual Studio 2017 Version 15.3, müssen Sie auch die 2017 von Visual Studio-Tools für Azure-Funktionen manuell aktualisieren. Sie können die Tools im Menü **Extras** aktualisieren. Navigieren Sie zu **Erweiterungen und Updates...** >          **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**. 
    >
    >

* Dieses Lernprogramm erfordert eine ausgeführte app für die Funktion. Wenn Sie eine nicht verfügen, können Sie im Schnellstart Abschnitt "Erstellen der Funktion Visual Studio" folgen. 
* Vollständige **die Schritte 4 bis 7 im Abschnitt "Erstellen eines Triggers Function".** 
* Dieses Lernprogramm erfordert auch eine Azure-Warteschlange, wenn Sie nicht einen können Sie dies tun erstellt haben, indem Sie die folgenden [Schritte](../storage/queues/storage-dotnet-how-to-use-queues.md).

### <a name="update-the-function-code"></a>Aktualisieren des Funktionscodes 
Um die Verbindung mit der Ausgabe der Funktion Warteschlange, müssen Sie eine ausgabebindung erstellen. 
1. Öffnen Sie die Funktion in Visual Studio
2.  Aktualisieren Sie bei einer C#-Funktion wie folgt, um die Ausgabe in der Definition der Funktion Warteschlange Speicher bindenden Parameter (ersetzen Sie die <QueueName> -Platzhalter durch den Namen der Ihre Ausgabe in die Warteschlange). Überspringen Sie diesen Schritt bei einer JavaScript-Funktion.
    
    ```csharp
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req,TraceWriter log,[Queue("<QueueName>", Connection = "myconnection")] ICollector<string> <QueueName>)
    {
    …
    }
            
    ```
3. Fügen Sie den folgenden Code an die Funktion nur verwendet werden, bevor die Methode zurückgegeben wird, um in die Ausgabe geschrieben Warteschlange. Verwenden Sie den Ausschnitt, der der Sprache der Funktion entspricht.

    ```javascript
    context.bindings.<QueueName> = "Name passed to the function: " + (req.query.name || req.body.name);
    ```
    
    ```cs
    <QueueName>.Add("Name passed to the function: " + name);     
    ```
    
### <a name="test-your-function"></a>Testen der Funktion

1. Führen Sie die Funktion auf Visual Studio.
2. Navigieren Sie zu dem Azure Government [Portal](https://portal.azure.us) , und klicken Sie auf die Schaltfläche "Speicherkonten" im Menü auf der linken Seite auf. 
3. Übersicht über die Seite klicken Sie auf die Schaltfläche "Warteschlangen" um die verschiedenen Warteschlangen finden Sie unter. 

    ![triggerfunctioncreate6](./media/documentation-government-function10.png)
4. Klicken Sie auf die Warteschlange, und Sie sollten die Ausgabe der Funktion angezeigt werden.

    ![triggerfunctioncreate7](./media/documentation-government-function11.png)
5. Nun, dass Sie die Ausgabe, die lokal Bindung getestet haben, wechseln Sie über die gleichen Schritte für die Bereitstellung, die Sie in der [obigen Abschnitt](documentation-government-functions.md#create-function-using-visual-studio). 

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie den folgenden Befehl aus, um alle Ressourcen, die von diesem Schnellstart erstellt löschen:

```azurecli-interactive
az group delete --name myResourceGroup
```
Geben Sie `y` ein, wenn Sie dazu aufgefordert werden.

## <a name="next-steps"></a>Nächste Schritte
* Abonnieren Sie die [Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/)
* Abrufen von Hilfe Stapelüberlauf mithilfe der "[Azure Gov](https://stackoverflow.com/questions/tagged/azure-gov)" Tag
* Senden Sie uns Feedback, oder bitten Sie die neue Funktionen über die [Azure Government-Feedback-Forum](https://feedback.azure.com/forums/558487-azure-government)

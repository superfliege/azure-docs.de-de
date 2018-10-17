---
title: Verwenden von Azure Event Grid zum Automatisieren der Größenänderung hochgeladener Bilder | Microsoft Docs
description: Azure Event Grid kann bei Blob-Uploads in Azure Storage ausgelöst werden. Sie können diese Funktion verwenden, um in Azure Storage hochgeladene Bilddateien an andere Dienste (z. B. Azure Functions) für die Größenänderung und andere Optimierungen zu senden.
services: event-grid, functions
author: ggailey777
manager: cfowler
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/29/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 2d94389ade02cb6e61f192e9b9e8adb8f8ceec31
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585576"
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Automatisieren der Größenänderung von hochgeladenen Bildern mit Event Grid

[Azure Event Grid](overview.md) ist ein Ereignisdienst für die Cloud. Event Grid ermöglicht Ihnen das Erstellen von Abonnements für Ereignisse, die von Azure-Diensten oder Drittanbieterressourcen ausgelöst werden.  

Dieses Tutorial stellt Teil zwei einer Reihe von Tutorials zu Storage dar. Es erweitert das [vorherige Tutorial zu Storage][previous-tutorial], um serverlose automatische Miniaturansichtengenerierung mithilfe von Azure Event Grid und Azure Functions hinzuzufügen. Event Grid ermöglicht, dass [Azure Functions](..\azure-functions\functions-overview.md) auf Ereignisse von [Azure Blob Storage](..\storage\blobs\storage-blobs-introduction.md) reagieren kann und Miniaturansichten hochgeladener Bilder generiert. Ein Ereignisabonnement wird für das Blob Storage-Erstellungsereignis erstellt. Wenn ein Blob einem bestimmten Blob Storage-Container hinzugefügt wird, wird ein Funktionsendpunkt aufgerufen. Aus Event Grid an die Funktionsbindung übergebene Daten werden für den Zugriff auf das Blob und zum Generieren der Miniaturansicht verwendet.

Sie verwenden die Azure-CLI und das Azure-Portal, um die Größenänderungsfunktionalität einer vorhandenen Bildupload-App hinzuzufügen.

![Veröffentlichte Web-App im Microsoft Edge-Browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines neuen Azure Storage-Kontos
> * Bereitstellen von serverlosem Code mit Azure Functions
> * Erstellen eines Blob Storage-Ereignisabonnements in Event Grid

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

Sie müssen das vorherige Blob Storage-Tutorial abgeschlossen haben: [Hochladen von Bilddaten in die Cloud mit Azure Storage][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Falls Sie den Event Grid-Ressourcenanbieter noch nicht in Ihrem Abonnement registriert haben, stellen Sie sicher, dass er registriert ist.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieses Tutorial die Azure CLI-Version 2.0.14 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli). 

Falls Sie nicht Cloud Shell verwenden, müssen Sie sich erst mithilfe von `az login` anmelden.

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Für Azure Functions ist ein allgemeines Speicherkonto erforderlich. Erstellen Sie ein separates allgemeines Speicherkonto in der Ressourcengruppe, indem Sie den Befehl [az storage account create](/cli/azure/storage/account#az-storage-account-create) verwenden.

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. 

Ersetzen Sie im folgenden Befehl den Platzhalter `<general_storage_account>` durch Ihren eigenen global eindeutigen Namen für das allgemeine Speicherkonto. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App  

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktion in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `<function_app>` durch den eigenen eindeutigen Namen Ihrer Funktions-App. Da der Name der Funktions-App als DNS-Standarddomäne für die Funktions-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Ersetzen Sie `<general_storage_account>` durch den Namen des allgemeinen Speicherkontos, das Sie erstellt haben.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Jetzt müssen Sie die Funktions-App für die Verbindung mit dem Blob Storage-Konto konfigurieren,das Sie im [vorhergehenden Tutorial][previous-tutorial] erstellt haben.

## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Die Funktion benötigt die Verbindungszeichenfolge zum Herstellen der Verbindung mit dem Blob Storage-Konto. Der Funktionscode, den Sie im folgenden Schritt in Azure bereitstellen, sucht in der App-Einstellung „myblobstorage_STORAGE“ nach der Verbindungszeichenfolge und in der App-Einstellung „myContainerName“ nach dem Namen des Miniaturansichtcontainers. Rufen Sie die Verbindungszeichenfolge mit dem Befehl [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) ab. Legen Sie Anwendungseinstellungen mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) fest.

In den folgenden CLI-Befehlen ist `<blob_storage_account>` der Name des Blob Storage-Kontos, das Sie im vorherigen Tutorial erstellt haben.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbnails FUNCTIONS_EXTENSION_VERSION=~2
```

Die Einstellung `FUNCTIONS_EXTENSION_VERSION=~2` legt fest, dass die Funktions-App unter der Version 2.x der Azure Functions-Runtime ausgeführt wird.

Sie können nun ein Funktionscodeprojekt für diese Funktions-App bereitstellen.

## <a name="deploy-the-function-code"></a>Bereitstellen des Funktionscodes 

# <a name="nettabnet"></a>[\.NET](#tab/net)

Das C#-Beispielskript (.csx) für die Größenänderung ist auf [GitHub](https://github.com/Azure-Samples/function-image-upload-resize) verfügbar. Stellen Sie dieses Funktionscodeprojekt für die Funktions-App mithilfe des Befehls [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) bereit. 

Im folgenden Befehl ist `<function_app>` der Name der zuvor erstellten Funktions-App.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
Die Node.js-Beispielfunktion für die Größenänderung ist auf [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node) verfügbar. Stellen Sie dieses Funktionscodeprojekt für die Funktions-App mithilfe des Befehls [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) bereit.

Im folgenden Befehl ist `<function_app>` der Name der zuvor erstellten Funktions-App.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```
---

Die Funktion für die Bildgrößenänderung wird durch HTTP-Anforderungen ausgelöst, die sie vom Event Grid-Dienst empfängt. Erstellen Sie ein Ereignisabonnement, um Event Grid zu informieren, dass Sie diese Benachrichtigungen unter der URL Ihrer Funktion erhalten möchten. Für dieses Tutorial abonnieren Sie durch Blobs erstellte Ereignisse.

Die von der Event Grid-Benachrichtigung an die Funktion übergebenen Daten enthalten die URL des Blobs. Diese URL wird wiederum an die Eingabebindung übergeben, um das hochgeladene Bild aus dem Blob-Speicher abzurufen. Die Funktion generiert eine Miniaturansicht und schreibt den sich ergebenden Datenstrom in einen separaten Container in Blob Storage. 

In diesem Projekt wird `EventGridTrigger` als Triggertyp verwendet. Es wird empfohlen, anstelle generischer HTTP-Trigger den Event Grid-Trigger zu verwenden. Event Grid überprüft automatisch Event Grid-Funktionstrigger. Bei generischen HTTP-Triggern müssen Sie die [Überprüfungsantwort](security-authentication.md#webhook-event-delivery) implementieren.

Weitere Informationen zu dieser Funktion finden Sie unter in den [Dateien „function.json“ und „run.csx“](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
Der Funktionsprojektcode wird direkt aus dem öffentlichen Beispielrepository bereitgestellt. Weitere Informationen zu den Bereitstellungsoptionen für Azure Functions finden Sie unter [Continuous Deployment für Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Erstellen eines Ereignisabonnements

Ein Ereignisabonnement gibt an, welche vom Anbieter generierten Ereignisse an einen bestimmten Endpunkt gesendet werden sollen. In diesem Fall wird der Endpunkt durch Ihre Funktion zur Verfügung gestellt. Führen Sie die folgenden Schritte durch, um ein Ereignisabonnement zu erstellen, dass Benachrichtigungen an Ihre Funktion im Azure-Portal sendet: 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) unten links auf den Pfeil, um alle Dienste zu erweitern, geben Sie *Funktionen* in das Feld **Filter** ein, und wählen Sie dann **Funktionen-Apps** aus. 

    ![Navigieren zu Funktions-Apps im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Erweitern Sie Ihre Funktions-App, wählen Sie die Funktion **imageresizerfunc** aus, und wählen Sie dann **Event Grid-Abonnement hinzufügen** aus.

    ![Navigieren zu Funktions-Apps im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Verwenden Sie die in der Tabelle angegebenen Einstellungen für das Ereignisabonnement.
    
    ![Erstellen eines Ereignisabonnements aus der Funktion im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | imageresizersub | Der Name, der Ihr neues Ereignisabonnement angibt. | 
    | **Thementyp** |  Speicherkonten | Wählen Sie den Speicherkonto-Ereignisanbieter aus. | 
    | **Abonnement** | Ihr Azure-Abonnement | Ihr aktuelles Azure-Abonnement ist standardmäßig ausgewählt.   |
    | **Ressourcengruppe** | myResourceGroup | Wählen Sie **Vorhandene verwenden** und anschließend die Ressourcengruppe aus, die Sie in diesem Tutorial verwendet haben.  |
    | **Instanz** |  Ihr Blob Storage-Konto |  Wählen Sie das Blob Storage-Konto aus, das Sie erstellt haben. |
    | **Ereignistypen** | Blob erstellt | Deaktivieren Sie alle Typen außer **Blob erstellt**. Nur Ereignistypen mit `Microsoft.Storage.BlobCreated` werden an die Funktion übergeben.| 
    | **Abonnententyp** |  Webhook |  „Webhook“ oder „Event Hubs“ stehen zur Auswahl. |
    | **Abonnentenendpunkt** | Automatisch generiert | Verwenden Sie die Endpunkt-URL, die für Sie generiert wird. | 
    | **Präfixfilter** | /blobServices/default/containers/images/blobs/ | Filtert Speicherereignisse nur für den Container **images**.| 

4. Klicken Sie auf **Erstellen**, um das Ereignisabonnement hinzuzufügen. Dadurch wird ein Ereignisabonnement erstellt, das `imageresizerfunc` auslöst, wenn ein Blob dem Container *images* hinzugefügt wird. Die Funktion passt die Größe der Bilder an und fügt sie dem Container *thumbnails* hinzu.

Nachdem die Back-End-Dienste konfiguriert wurden, testen Sie die Funktion für die Größenänderung von Bildern in der Beispiel-Web-App. 

## <a name="test-the-sample-app"></a>Testen der Beispiel-App

Navigieren Sie zum Testen der Größenänderung von Bildern in der Web-App zur URL Ihrer veröffentlichten App. Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`.

Klicken Sie auf den Bereich **Fotos hochladen**, um eine Datei auszuwählen und hochzuladen. Sie können auch ein Foto in diese Region ziehen. 

Beachten Sie, dass eine Kopie des hochgeladenen Bilds im Karussell **Generierte Miniaturansichten** angezeigt wird, nachdem das hochgeladene Bild nicht mehr angezeigt wird. Die Größe dieses Bilds wurde von der Funktion angepasst, und das Bild wurde dem Container *thumbnails* hinzugefügt und vom Webclient heruntergeladen.

![Veröffentlichte Web-App im Microsoft Edge-Browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines neuen Azure Storage-Kontos
> * Bereitstellen von serverlosem Code mit Azure Functions
> * Erstellen eines Blob Storage-Ereignisabonnements in Event Grid

Fahren Sie mit Teil 3 der Tutorialreihe zu Storage fort, um zu erfahren, wie der Zugriff auf das Speicherkonto gesichert wird.

> [!div class="nextstepaction"]
> [Sichern des Zugriffs auf die Daten einer Anwendung in der Cloud](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Weitere Informationen zu Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md). 
+ Ein weiteres Tutorial zu Azure Functions finden Sie unter [Erstellen einer Funktion, die in Azure Logic Apps integriert ist](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md

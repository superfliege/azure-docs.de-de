---
title: "Verwenden von Azure Event Grid zum Automatisieren der Größenänderung hochgeladener Bilder | Microsoft Docs"
description: "Azure Event Grid kann bei Blob-Uploads in Azure Storage ausgelöst werden. Sie können diese Funktion verwenden, um in Azure Storage hochgeladene Bilddateien an andere Dienste (z. B. Azure Functions) für die Größenänderung und andere Optimierungen zu senden."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 358015d6cfd9961508b209f628b2d648a75e3c2c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Automatisieren der Größenänderung von hochgeladenen Bildern mit Event Grid

[Azure Event Grid](overview.md) ist ein Ereignisdienst für die Cloud. Event Grid ermöglicht Ihnen das Erstellen von Abonnements für Ereignisse, die von Azure-Diensten oder Drittanbieterressourcen ausgelöst werden.  

Dieses Tutorial stellt Teil zwei einer Reihe von Tutorials zu Storage dar. Es erweitert das [vorherige Tutorial zu Storage][previous-tutorial], um serverlose automatische Miniaturansichtengenerierung mithilfe von Azure Event Grid und Azure Functions hinzuzufügen. Event Grid ermöglicht, dass [Azure Functions](..\azure-functions\functions-overview.md) auf Ereignisse von [Azure Blob Storage](..\storage\blobs\storage-blobs-introduction.md) reagieren kann und Miniaturansichten hochgeladener Bilder generiert. Ein Ereignisabonnement wird für das Blob Storage-Erstellungsereignis erstellt. Wenn ein Blob einem bestimmten Blob Storage-Container hinzugefügt wird, wird ein Funktionsendpunkt aufgerufen. Aus Event Grid an die Funktionsbindung übergebene Daten werden für den Zugriff auf das Blob und zum Generieren der Miniaturansicht verwendet. 

Sie verwenden die Azure-CLI und das Azure-Portal, um die Größenänderungsfunktionalität einer vorhandenen Bildupload-App hinzuzufügen.

[!INCLUDE [storage-events-note.md](../../includes/storage-events-note.md)]

![Veröffentlichte Web-App im Edge-Browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines neuen Azure Storage-Kontos
> * Bereitstellen von serverlosem Code mit Azure Functions
> * Erstellen eines Blob Storage-Ereignisabonnements in Event Grid

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ Sie müssen das vorherige Blob Storage-Tutorial abgeschlossen haben: [Hochladen von Bilddaten in die Cloud mit Azure Storage][previous-tutorial]. 
+ Sie müssen sich für die Blob Storage-Ereignisfunktionalität angemeldet und Zugriff darauf erhalten haben. [Fordern Sie Zugriff auf Blob Storage-Ereignisse](#request-storage-access) an, bevor Sie mit den weiteren Schritten in diesem Thema fortfahren.  

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, die Azure CLI-Version 2.0.14 oder höher auszuführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

Falls Sie nicht Cloud Shell verwenden, müssen Sie sich erst mithilfe von `az login` anmelden.

## <a name="enable-blob-storage-events"></a>Aktivieren von Blob Storage-Ereignissen

Zu diesem Zeitpunkt müssen Sie den Zugriff auf das Blob Storage-Ereignisfeature anfordern.  

### <a name="request-storage-access"></a>Anfordern des Zugriffs auf Blob Storage-Ereignisse

Sie fordern den Zugriff mit dem Befehl `az feature register` an.

> [!IMPORTANT]  
> Wir akzeptieren Teilnehmer an der Preview von Blob Storage-Ereignissen in der Reihenfolge ihrer Teilnahmeanforderung. Sie müssen ggf. mit einer Verzögerung von 1 bis 2 Werktagen rechnen, bis Sie Zugriff auf dieses Feature erhalten. 

```azurecli-interactive
az feature register --name storageEventSubscriptions --namespace Microsoft.EventGrid
```

### <a name="check-access-status"></a>Überprüfen des Genehmigungsstatus

Sie erhalten eine E-Mail von Microsoft, in der Sie benachrichtigt werden, dass Ihnen der Zugriff auf Blob Storage-Ereignisse gewährt wurde. Sie können den Status Ihrer Zugriffsanforderung jederzeit mit dem Befehl `az feature show` überprüfen.

```azurecli-interactive
az feature show --name storageEventSubscriptions --namespace Microsoft.EventGrid --query properties.state
```
Nachdem Ihnen der Zugriff auf das Feature Blob Storage-Ereignisse gewährt wurde, gibt dieser Befehl einen Wert `"Registered"` zurück. 
 
Nachdem Sie registriert wurden, können Sie dieses Tutorial fortsetzen.

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Für Azure Functions ist ein allgemeines Speicherkonto erforderlich. Erstellen Sie ein separates allgemeines Speicherkonto in der Ressourcengruppe, indem Sie den Befehl [az storage account create](/cli/azure/storage/account#create) verwenden.

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. 

Ersetzen Sie im folgenden Befehl den Platzhalter `<general_storage_account>` durch Ihren eigenen global eindeutigen Namen für das allgemeine Speicherkonto. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App  

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktion in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `<function_app>` durch den eigenen eindeutigen Namen Ihrer Funktions-App. Da `<function_app>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. In diesem Fall ist „`<general_storage_account>`“ der Name des allgemeinen Speicherkontos, das Sie erstellt haben.  

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Jetzt müssen Sie die Funktions-App für die Verbindung mit Blob Storage konfigurieren. 

## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Die Funktion benötigt die Verbindungszeichenfolge zum Herstellen der Verbindung mit dem Blob Storage-Konto. In diesem Fall ist „`<blob_storage_account>`“ der Name des Blob Storage-Kontos, das Sie im vorherigen Tutorial erstellt haben. Rufen Sie die Verbindungszeichenfolge mit dem Befehl [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) ab. Der Name des Miniaturansichtcontainers muss ebenfalls auf „`thumbs`“ festgelegt werden. Fügen Sie diese Anwendungseinstellungen in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) hinzu.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

Sie können nun ein Funktionscodeprojekt für diese Funktions-App bereitstellen.

## <a name="deploy-the-function-code"></a>Bereitstellen des Funktionscodes 

Die C#-Funktion, die die Änderung der Bildgröße ausführt, steht in diesem [GitHub-Beispielrepository](https://github.com/Azure-Samples/function-image-upload-resize) zur Verfügung. Stellen Sie dieses Funktionscodeprojekt für die Funktions-App mithilfe des Befehls [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) bereit. 

Im folgenden Befehl ist `<function_app>` dieselbe Funktions-App, die Sie im vorherigen Skript erstellt haben.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Die Funktion für die Bildgrößenänderung wird durch ein Ereignisabonnement für ein Ereignis „Blob erstellt“ ausgelöst. Die an den Trigger übergebenen Daten umfassen die URL des Blobs, die ihrerseits an die Eingabebindung zum Abrufen des hochgeladenen Bilds aus Blob Storage übergeben wird. Die Funktion generiert eine Miniaturansicht und schreibt den sich ergebenden Datenstrom in einen separaten Container in Blob Storage. Weitere Informationen zu dieser Funktion finden Sie in der [Infodatei im Beispielrepository](https://github.com/Azure-Samples/function-image-upload-resize/blob/master/README.md).
 
Der Funktionsprojektcode wird direkt aus dem öffentlichen Beispielrepository bereitgestellt. Weitere Informationen zu den Bereitstellungsoptionen für Azure Functions finden Sie unter [Continuous Deployment für Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-your-event-subscription"></a>Erstellen eines Ereignisabonnements

Ein Ereignisabonnement gibt an, welche vom Anbieter generierten Ereignisse an einen bestimmten Endpunkt gesendet werden sollen. In diesem Fall wird der Endpunkt durch Ihre Funktion zur Verfügung gestellt. Verwenden Sie die folgenden Schritte, um ein Ereignisabonnement aus Ihrer Funktion im Azure-Portal zu erstellen: 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf den Pfeil unten links, um alle Dienste zu erweitern, geben Sie `functions` in das Feld **Filter** ein, und wählen Sie dann **Funktions-Apps** aus. 

    ![Navigieren zu Funktions-Apps im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Erweitern Sie Ihre Funktions-App, wählen Sie die Funktion **imageresizerfunc** aus, und wählen Sie dann **Event Grid-Abonnement hinzufügen** aus.

    ![Navigieren zu Funktions-Apps im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Verwenden Sie die in der Tabelle angegebenen Einstellungen für das Ereignisabonnement.

    ![Erstellen eines Ereignisabonnements aus der Funktion im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | imageresizersub | Der Name, der Ihr neues Ereignisabonnement angibt. | 
    | **Thementyp** |  Speicherkonten | Wählen Sie den Speicherkonto-Ereignisanbieter aus. | 
    | **Abonnement** | Ihr Abonnement | Standardmäßig sollte Ihr aktuelles Abonnement ausgewählt sein.   |
    | **Ressourcengruppe** | myResourceGroup | Wählen Sie **Vorhandene verwenden** aus, und wählen Sie die Ressourcengruppe aus, die Sie in diesem Thema verwendet haben.  |
    | **Instanz** |  `<blob_storage_account>` |  Wählen Sie das Blob Storage-Konto aus, das Sie erstellt haben. |
    | **Ereignistypen** | Blob erstellt | Deaktivieren Sie alle Typen außer **Blob erstellt**. Nur Ereignistypen mit `Microsoft.Storage.BlobCreated` werden an die Funktion übergeben.| 
    | **Abonnentenendpunkt** | Automatisch generiert | Verwenden Sie die Endpunkt-URL, die für Sie generiert wird. | 
    | **Präfixfilter** | /blobServices/default/containers/images/blobs/ | Filtert Speicherereignisse nur für den Container **images**.| 

4. Klicken Sie auf **Erstellen**, um das Ereignisabonnement hinzuzufügen. Es wird ein Ereignisabonnement erstellt, das **imageresizerfunc** auslöst, wenn ein Blob dem Container **images** hinzugefügt wird. In der Größe geänderte Bilder werden dem Container **thumbs** hinzugefügt.

Nachdem die Back-End-Dienste konfiguriert wurden, testen Sie die Funktion für die Größenänderung von Bildern in der Beispiel-Web-App. 

## <a name="test-the-sample-app"></a>Testen der Beispiel-App

Navigieren Sie zum Testen der Größenänderung von Bildern in der Web-App zur URL Ihrer veröffentlichten App. Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`.

Klicken Sie auf den Bereich **Fotos hochladen**, um eine Datei auszuwählen und hochzuladen. Sie können auch ein Foto in diese Region ziehen. 

Beachten Sie, dass eine Kopie des hochgeladenen Bilds im Karussell **Generierte Miniaturansichten** angezeigt wird, nachdem das hochgeladene Bild nicht mehr angezeigt wird. Die Größe dieses Bilds wurde von der Funktion geändert, dem Container „thumbs“ hinzugefügt und vom Webclient heruntergeladen. 

![Veröffentlichte Web-App im Edge-Browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

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
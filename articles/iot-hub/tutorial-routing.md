---
title: Konfigurieren der Nachrichtenweiterleitung mit Azure IoT Hub (.NET) | Microsoft-Dokumentation
description: Konfigurieren der Nachrichtenweiterleitung mit Azure IoT Hub
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 6e421aa630dc121589dece789e2e0d7f9a56bbe6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434838"
---
# <a name="tutorial-configure-message-routing-with-iot-hub"></a>Tutorial: Konfigurieren der Nachrichtenweiterleitung mit IoT Hub

Die Nachrichtenweiterleitung ermöglicht das Senden von Telemetriedaten von Ihren IoT-Geräten an integrierte, Event Hub-kompatible Endpunkte oder benutzerdefinierte Endpunkte wie Blob Storage-Instanzen, Service Bus-Warteschlangen, Service Bus-Themen und Event Hubs. Beim Konfigurieren der Nachrichtenweiterleitung können Sie Routingregeln erstellen, um die Route anzupassen, die einer bestimmten Regel entspricht. Nach der Einrichtung werden die eingehenden Daten von IoT Hub automatisch an die Endpunkte weitergeleitet. 

In diesem Tutorial erfahren Sie, wie Sie Routingregeln mit IoT Hub einrichten und verwenden. Sie leiten Nachrichten von einem IoT-Gerät an einen von mehreren Diensten weiter, wie z.B. Blob Storage oder eine Service Bus-Warteschlange. Nachrichten an die Service Bus-Warteschlange werden von einer Logik-App aufgenommen und über E-Mail gesendet. Nachrichten ohne speziell eingerichtete Weiterleitung werden an den Standardendpunkt gesendet und in einer Power BI-Visualisierung angezeigt.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Sie richten über die Azure-Befehlszeilenschnittstelle oder mithilfe von PowerShell die grundlegenden Ressourcen ein – eine IoT Hub-Instanz, ein Speicherkonto, eine Service Bus-Warteschlange und ein simuliertes Gerät.
> * Sie konfigurieren Endpunkte und Routen in IoT Hub für das Speicherkonto und die Service Bus-Warteschlange.
> * Sie erstellen eine Logik-App, die ausgelöst wird und eine E-Mail sendet, wenn der Service Bus-Warteschlange eine Nachricht hinzugefügt wird.
> * Sie laden eine App herunter und führen sie aus, die ein IoT-Gerät simuliert, das für die verschiedenen Weiterleitungsoptionen Nachrichten an den Hub sendet.
> * Sie erstellen eine Power BI-Visualisierung für die Daten, die an den Standardendpunkt gesendet werden.
> * Sie zeigen die Ergebnisse an:
> * – in der Service Bus-Warteschlange und in E-Mails
> * – im Speicherkonto
> * – in der Power BI-Visualisierung

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Installieren Sie [Visual Studio für Windows](https://www.visualstudio.com/). 

- Ein Power BI-Konto zum Ausführen von Stream Analytics-Funktionen des Standardendpunkts. ([Power BI kostenlos testen](https://app.powerbi.com/signupredirect?pbi_source=web))

- Ein Office 365-Konto zum Senden von Benachrichtigungs-E-Mails. 

Sie benötigen entweder die Azure-Befehlszeilenschnittstelle oder Azure PowerShell, um die Einrichtungsschritte für dieses Tutorial auszuführen. 

Sie können zwar die Azure-Befehlszeilenschnittstelle lokal installieren, wir empfehlen jedoch die Verwendung von Azure Cloud Shell. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie Skripts der Azure-Befehlszeilenschnittstelle ausführen können. Häufig verwendete Azure-Tools sind in Cloud Shell vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Daher müssen Sie die Tools nicht lokal installieren. 

Wenn Sie PowerShell verwenden möchten, installieren Sie das Tool lokal gemäß der unten stehenden Anleitung. 

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

Cloud Shell kann auf mehrere Arten geöffnet werden:

|  |   |
|-----------------------------------------------|---|
| Klicken Sie in der rechten oberen Ecke eines Codeblocks auf **Ausprobieren**. | ![Cloud Shell in diesem Artikel](./media/tutorial-routing/cli-try-it.png) |
| Öffnen Sie Cloud Shell in Ihrem Browser. | [![https://shell.azure.com/bash](./media/tutorial-routing/launchcloudshell.png)](https://shell.azure.com) |
| Klicken Sie im [Azure-Portal](https://portal.azure.com) rechts oben im Menü auf die Schaltfläche **Cloud Shell**: |    ![Cloud Shell im Portal](./media/tutorial-routing/cloud-shell-menu.png) |
|  |  |

### <a name="using-azure-cli-locally"></a>Lokale Verwendung der Azure-Befehlszeilenschnittstelle

Wenn Sie lieber eine Befehlszeilenschnittstelle verwenden als Cloud Shell, benötigen Sie die Azure CLI-Modulversion 2.0.30.0 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

### <a name="using-powershell-locally"></a>Lokale Verwendung von PowerShell

Für dieses Tutorial ist das Azure PowerShell-Modul Version 5.7 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="set-up-resources"></a>Einrichten von Ressourcen

Für dieses Tutorial benötigen Sie eine IoT Hub-Instanz, ein Speicherkonto und eine Service Bus-Warteschlange. All diese Ressourcen können mithilfe der Azure-Befehlszeilenschnittstelle oder über Azure PowerShell erstellt werden. Verwenden Sie für alle Ressourcen die gleiche Ressourcengruppe und den gleichen Speicherort. Am Ende dieses Tutorials können Sie dann sämtliche Elemente in einem Schritt entfernen, indem Sie die Ressourcengruppe löschen.

In den folgenden Abschnitten wird beschrieben, wie Sie diese Schritte ausführen. Führen Sie die Anweisungen zur Installation der Befehlszeilenschnittstelle *oder* von PowerShell aus.

1. Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). 

    <!-- When they add the Basic tier, change this to use Basic instead of Standard. -->

1. Erstellen Sie eine IoT Hub-Instanz im Tarif S1. Fügen Sie Ihrer IoT Hub-Instanz eine Consumergruppe hinzu. Die Consumergruppe wird beim Abrufen von Daten von Azure Stream Analytics verwendet.

1. Erstellen Sie ein Speicherkonto des Typs „Standard V1“ mit Standard_LRS-Replikation.

1. Erstellen Sie einen Service Bus-Namespace und eine Service Bus-Warteschlange. 

1. Erstellen Sie eine Geräteidentität für das simulierte Gerät, das Nachrichten an Ihren Hub sendet. Speichern Sie den Schlüssel für die Testphase.

### <a name="azure-cli-instructions"></a>Anleitung für die Azure-Befehlszeilenschnittstelle

Am einfachsten lässt sich dieses Skript verwenden, indem Sie es kopieren und in Cloud Shell einfügen. Sofern Sie bereits angemeldet sind, wird das Skript Zeile für Zeile ausgeführt. 

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostorage$RANDOM
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "$storageAccountKey"

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off 

# The Service Bus namespace must be globally unique, so add a random number to the end.
sbNameSpace=ContosoSBNamespace$RANDOM
echo "Service Bus namespace = " $sbNameSpace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNameSpace \
    --location $location
    
# The Service Bus queue name must be globally unique, so add a random number to the end.
sbQueueName=ContosoSBQueue$RANDOM
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNameSpace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

### <a name="powershell-instructions"></a>Anleitung für PowerShell

Am einfachsten lässt sich dieses Skript verwenden, indem Sie [PowerShell ISE](/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise.md) öffnen, das Skript in die Zwischenablage kopieren und dann das gesamte Skript in das Skriptfenster einfügen. Danach ändern Sie die Werte für die Ressourcennamen (sofern gewünscht), und führen das gesamte Skript aus. 

```azurepowershell-interactive
# Log into Azure account.
Login-AzureRMAccount

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"
$iotDeviceName = "Contoso-Test-Device"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, so add a random number to the end.
$iotHubName = "ContosoTestHub$(Get-Random)"
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzureRmIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzureRmIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random number to the end.
$storageAccountName = "contosostorage$(Get-Random)"
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
$storageContext = $storageAccount.Context 

# Create the container in the storage account.
New-AzureStorageContainer -Name $containerName `
    -Context $storageContext

# The Service Bus namespace must be globally unique,
#   so add a random number to the end.
$serviceBusNamespace = "ContosoSBNamespace$(Get-Random)"
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzureRmServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random number to the end.
$serviceBusQueueName  = "ContosoSBQueue$(Get-Random)"
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzureRmServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName 

```

Als Nächstes erstellen Sie eine Geräteidentität und speichern den zugehörigen Schlüssel für die spätere Verwendung. Diese Geräteidentität wird von der Simulationsanwendung verwendet, um Nachrichten an die IoT Hub-Instanz zu senden. Diese Funktion steht in PowerShell nicht zur Verfügung, aber Sie können das Gerät im [Azure-Portal](https://portal.azure.com) erstellen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich bei Ihrem Azure-Konto an.

1. Klicken Sie auf **Ressourcengruppen**, und wählen Sie Ihre Ressourcengruppe aus. In diesem Tutorial wird **ContosoResources** verwendet.

1. Klicken Sie in der Liste der Ressourcen auf Ihre IoT Hub-Instanz. In diesem Tutorial wird **ContosoTestHub** verwendet. Wählen Sie im Bereich „Hub“ die Option **IoT-Geräte** aus.

1. Klicken Sie auf **+ Hinzufügen**. Geben Sie im Bereich „Gerät hinzufügen“ die Geräte-ID an. In diesem Tutorial wird **Contoso-Test-Device** verwendet. Lassen Sie die Felder für die Schlüssel leer, und aktivieren Sie das Kontrollkästchen **Schlüssel automatisch generieren**. Stellen Sie sicher, dass die Option **Gerät mit IoT Hub verbinden** aktiviert ist. Klicken Sie auf **Speichern**.

   ![Screenshot: Bildschirm zum Hinzufügen von Geräten](./media/tutorial-routing/add-device.png)

1. Nachdem das Gerät erstellt wurde, klicken Sie darauf, um die generierten Schlüssel anzuzeigen. Klicken Sie beim primären Schlüssel auf das Symbol „Kopieren“, und speichern Sie den Schlüssel für die Testphase dieses Tutorials an einem Ort wie beispielsweise dem Editor.

   ![Screenshot: Gerätedetails, einschließlich Schlüssel](./media/tutorial-routing/device-details.png)

## <a name="set-up-message-routing"></a>Einrichten der Nachrichtenweiterleitung

Sie leiten Nachrichten an verschiedene Ressourcen weiter, abhängig von Eigenschaften, die vom simulierten Gerät an die Nachricht angefügt werden. Nachrichten ohne benutzerdefinierte Weiterleitung, werden an den Standardendpunkt gesendet (Nachrichten/Ereignisse). 

|value |Ergebnis|
|------|------|
|level="storage" |Schreibvorgang in Azure Storage.|
|level="critical" |Schreibvorgang in eine Service Bus-Warteschlange. Eine Logik-App ruft die Nachricht aus der Warteschlange ab und sendet sie mithilfe von Office 365.|
|die Standardeinstellung |Anzeigen dieser Daten mithilfe von Power BI.|

### <a name="routing-to-a-storage-account"></a>Weiterleiten an ein Speicherkonto 

Richten Sie jetzt die Weiterleitung für das Speicherkonto ein. Definieren Sie einen Endpunkt, und richten Sie dann eine Route für diesen Endpunkt ein. Nachrichten, deren **level**Eigenschaft auf **storage** festgelegt wurde, werden automatisch in ein Speicherkonto geschrieben.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressourcengruppen**, und wählen Sie dann Ihre Ressourcengruppe aus. In diesem Tutorial wird **ContosoResources** verwendet. Klicken Sie in der Liste der Ressourcen auf Ihre IoT Hub-Instanz. In diesem Tutorial wird **ContosoTestHub** verwendet. Klicken Sie auf **Endpunkte**. Klicken Sie im Bereich **Endpunkte** auf **+ Hinzufügen**. Geben Sie Folgendes ein:

   **Name**: Geben Sie einen Namen für den Endpunkt an. In diesem Tutorial wird **StorageContainer** verwendet.
   
   **Endpunkttyp**: Wählen Sie **Azure Storage-Container** aus der Dropdownliste aus.

   Klicken Sie auf **Container auswählen**, um die Liste der Speicherkonten anzuzeigen. Wählen Sie dann Ihr Speicherkonto aus. In diesem Tutorial wird **contosostorage** verwendet. Wählen Sie als Nächstes den Container aus. In diesem Tutorial wird **contosoresults** verwendet. Klicken Sie auf **Auswählen**. Dadurch gelangen Sie wieder zum Bereich **Endpunkt hinzufügen**. 
   
   ![Screenshot: Hinzufügen eines Endpunkts](./media/tutorial-routing/add-endpoint-storage-account.png)
   
   Klicken Sie auf **OK**, um das Hinzufügen des Endpunkts abzuschließen.
   
1. Klicken Sie in Ihrer IoT Hub-Instanz auf **Routen**. Sie erstellen eine Routingregel, die Nachrichten an den Speichercontainer weiterleitet, den Sie gerade als Endpunkt hinzugefügt haben. Klicken Sie oben im Bereich „Routen“ auf **+ Hinzufügen**. Füllen Sie die Felder auf dem Bildschirm aus. 

   **Name**: Geben Sie einen Namen ein Sie einen Namen für Ihre Routingregel ein. In diesem Tutorial wird **StorageRule** verwendet.

   **Datenquelle**: Wählen Sie **Gerätemeldungen** aus der Dropdownliste aus.

   **Endpunkt**: Wählen Sie den Endpunkt aus, den Sie gerade eingerichtet haben. In diesem Tutorial wird **StorageContainer** verwendet. 
   
   **Abfragezeichenfolge**: Geben Sie `level="storage"` als Abfragezeichenfolge ein. 

   ![Screenshot: Erstellen einer Routingregel für das Speicherkonto](./media/tutorial-routing/create-a-new-routing-rule-storage.png)
   
   Klicken Sie auf **Speichern**. Wenn der Speichervorgang abgeschlossen ist, werden Sie wieder zum Bereich „Routen“ geleitet, in dem Sie Ihre neue Routingregel für die Speicherung anzeigen können. Schließen Sie den Bereich „Routen“. Daraufhin werden Sie wieder zur Seite der Ressourcengruppe geleitet.

### <a name="routing-to-a-service-bus-queue"></a>Weiterleitung in eine Service Bus-Warteschlange 

Richten Sie jetzt die Weiterleitung für die Service Bus-Warteschlange ein. Definieren Sie einen Endpunkt, und richten Sie dann eine Route für diesen Endpunkt ein. Nachrichten, bei denen die **level**-Eigenschaft auf **critical** festgelegt ist, werden in die Service Bus-Warteschlange geschrieben. Diese löst eine Logik-App aus, die dann eine E-Mail mit den Informationen sendet. 

1. Klicken Sie auf der Seite der Ressourcengruppe auf Ihre IoT Hub-Instanz und dann auf **Endpunkte**. Klicken Sie im Bereich **Endpunkte** auf **+ Hinzufügen**. Geben Sie Folgendes ein:

   **Name**: Geben Sie einen Namen für den Endpunkt an. In diesem Tutorial wird **CriticalQueue** verwendet. 

   **Endpunkttyp**: Wählen Sie **Service Bus-Warteschlange** aus der Dropdownliste aus.

   **Service Bus-Namespace**: Wählen Sie den Service Bus-Namespace für dieses Tutorial aus der Dropdownliste aus. In diesem Tutorial wird **ContosoSBNamespace** verwendet.

   **Service Bus-Warteschlange**: Wählen Sie die Service Bus-Warteschlange aus der Dropdownliste aus. In diesem Tutorial wird **contososbqueue** verwendet.

   ![Screenshot: Hinzufügen eines Endpunkts für die Service Bus-Warteschlange](./media/tutorial-routing/add-endpoint-sb-queue.png)

   Klicken Sie auf **OK**, um den Endpunkt zu speichern. Wenn der Speichervorgang abgeschlossen ist, schließen Sie den Bereich „Endpunkte“. 
    
1. Klicken Sie in Ihrer IoT Hub-Instanz auf **Routen**. Sie erstellen eine Routingregel, die Nachrichten an die Service Bus-Warteschlange weiterleitet, die Sie gerade als Endpunkt hinzugefügt haben. Klicken Sie oben im Bereich „Routen“ auf **+ Hinzufügen**. Füllen Sie die Felder auf dem Bildschirm aus. 

   **Name**: Geben Sie einen Namen ein Sie einen Namen für Ihre Routingregel ein. In diesem Tutorial wird **SBQueueRule** verwendet. 

   **Datenquelle**: Wählen Sie **Gerätemeldungen** aus der Dropdownliste aus.

   **Endpunkt**: Wählen Sie den Endpunkt aus, den Sie gerade eingerichtet haben – **CriticalQueue**.

   **Abfragezeichenfolge**: Geben Sie `level="critical"` als Abfragezeichenfolge ein. 

   ![Screenshot: Erstellen einer Routingregel für die Service Bus-Warteschlange](./media/tutorial-routing/create-a-new-routing-rule-sbqueue.png)
   
   Klicken Sie auf **Speichern**. Wenn Sie zum Bereich „Routen“ zurückkehren, sehen Sie Ihre beiden neuen Routingregeln, wie hier gezeigt.

   ![Screenshot: die von Ihnen eingerichteten Routen](./media/tutorial-routing/show-routing-rules-for-hub.png)

   Schließen Sie den Bereich „Routen“. Daraufhin werden Sie wieder zur Seite der Ressourcengruppe geleitet.

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App  

Die Service Bus-Warteschlange sollte zum Empfangen von als kritisch eingestuften Nachrichten verwendet werden. Sie richten eine Logik-App ein, um die Service Bus-Warteschlange zu überwachen und eine E-Mail zu senden, wenn der Warteschlange eine Nachricht hinzugefügt wird. 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **+ Ressource erstellen**. Geben Sie **Logik-App** in das Suchfeld ein, und drücken Sie die EINGABETASTE. Wählen Sie aus den angezeigten Ergebnissen „Logik-App“ aus, und klicken Sie dann auf **Erstellen**, um zum Bereich **Logik-App erstellen** zu gelangen. Füllen Sie die Felder aus. 

   **Name**: Dieses Feld enthält den Namen der Logik-App. In diesem Tutorial wird **ContosoLogicApp** verwendet. 

   **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus.

   **Ressourcengruppe**: Klicken Sie auf **Vorhandene verwenden**, und wählen Sie Ihre Ressourcengruppe aus. In diesem Tutorial wird **ContosoResources** verwendet. 

   **Standort**: Verwenden Sie Ihren Standort. In diesem Tutorial wird **USA, Westen** verwendet. 

   **Log Analytics**: Diese Option sollte deaktiviert werden. 

   ![Screenshot: Bildschirm „Logik-App erstellen“](./media/tutorial-routing/create-logic-app.png)

   Klicken Sie auf **Create**.

1. Wechseln Sie jetzt zur Logik-App. Am einfachsten wechseln Sie zur Logik-App, indem Sie auf **Ressourcengruppen** klicken, Ihre Ressourcengruppe auswählen (in diesem Tutorial wird **ContosoResources** verwendet) und dann die Logik-App aus der Liste der Ressourcen auswählen. Die Seite „Designer für Logik-Apps“ wird angezeigt (möglicherweise müssen Sie nach rechts scrollen, um die ganze Seite zu sehen). Scrollen Sie auf der Seite „Designer für Logik-Apps“ nach unten, bis Sie die Kachel **Leere Logik-App +** sehen. Klicken Sie darauf. 

1. Eine Liste von Connectors wird angezeigt. Wählen Sie **Service Bus** aus. 

   ![Screenshot: Liste der Connectors](./media/tutorial-routing/logic-app-connectors.png)

1. Eine Liste von Triggern wird angezeigt. Wählen Sie **Service Bus – Wenn eine Nachricht in einer Warteschlange empfangen wird (automatisch abschließen)** aus. 

   ![Screenshot: Liste der Trigger für die Service Bus-Warteschlange](./media/tutorial-routing/logic-app-triggers.png)

1. Geben Sie im nächsten Bildschirm den Namen für die Verbindung ein. In diesem Tutorial wird **ContosoConnection** verwendet. 

   ![Screenshot: Einrichten der Verbindung für die Service Bus-Warteschlange](./media/tutorial-routing/logic-app-define-connection.png)

   Klicken Sie auf den Service Bus-Namespace. In diesem Tutorial wird **ContosoSBNamespace** verwendet. Wenn Sie den Namespace auswählen, fragt das Portal den Service Bus-Namespace ab, um die Schlüssel zu erhalten. Wählen Sie **RootManageSharedAccessKey** aus, und klicken Sie auf **Erstellen**. 
   
   ![Screenshot: Abschließen der Verbindungseinrichtung](./media/tutorial-routing/logic-app-finish-connection.png)

1. Wählen Sie im nächsten Bildschirm aus der Dropdownliste den Namen der Warteschlange (in diesem Tutorial wird **contososbqueue** verwendet). Für die restlichen Felder können Sie die Standardwerte verwenden. 

   ![Screenshot: Warteschlangenoptionen](./media/tutorial-routing/logic-app-queue-options.png)

1. Richten jetzt die Aktion ein, die eine E-Mail sendet, wenn in der Warteschlange eine Nachricht empfangen wird. Klicken Sie im Logik-App-Designer auf **+ Neuer Schritt**, um einen Schritt hinzufügen, und klicken Sie dann auf **Aktion hinzufügen**. Suchen Sie im Bereich **Aktion auswählen** nach **Office 365 Outlook**, und klicken Sie darauf. Wählen Sie im Bildschirm mit den Triggern die Option **Office 365 Outlook – E-Mail senden** aus.  

   ![Screenshot: Office 365-Optionen](./media/tutorial-routing/logic-app-select-outlook.png)

1. Melden Sie sich als Nächstes bei Ihrem Office 365-Konto an, um die Verbindung einzurichten. Geben Sie die E-Mail-Adressen für die E-Mail-Empfänger ein. Geben Sie auch einen Betreff an sowie eine Nachricht ein, die den Empfängern im Nachrichtentext angezeigt werden soll. Geben Sie zu Testzwecken Ihre eigene E-Mail-Adresse als Empfänger ein.

   Klicken Sie auf **Dynamischen Inhalt hinzufügen**, um Inhalte aus der Nachricht anzuzeigen, die Sie hinzufügen können. Wählen Sie **Inhalt** aus – damit wird die Nachricht in die E-Mail einbezogen. 

   ![Screenshot: E-Mail-Optionen für die Logik-App](./media/tutorial-routing/logic-app-send-email.png)

1. Klicken Sie auf **Speichern**. Schließen Sie dann den Logik-App-Designer.

## <a name="set-up-azure-stream-analytics"></a>Einrichten von Azure Stream Analytics

Um die Daten in einer Power BI-Visualisierung anzuzeigen, richten Sie zuerst einen Stream Analytics-Auftrag zum Abrufen der Daten ein. Denken Sie daran, dass nur Nachrichten mit „**level** = **normal**“ an den Standardendpunkt gesendet und vom Stream Analytics-Auftrag für die Power BI-Visualisierung abgerufen werden.

### <a name="create-the-stream-analytics-job"></a>Erstellen des Stream Analytics-Auftrags

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressource erstellen** > **Internet der Dinge** > **Stream Analytics-Auftrag**.

1. Geben Sie die folgenden Informationen für den Auftrag ein.

   **Auftragsname**: Der Name des Auftrags. Der Name muss global eindeutig sein. In diesem Tutorial wird **contosoJob** verwendet.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihre IoT Hub-Instanz. In diesem Tutorial wird **ContosoResources** verwendet. 

   **Standort**: Verwenden Sie den gleichen Standort, der im Einrichtungsskript verwendet wurde. In diesem Tutorial wird **USA, Westen** verwendet. 

   ![Screenshot: Erstellen des Stream Analytics-Auftrags](./media/tutorial-routing/stream-analytics-create-job.png)

1. Klicken Sie auf **Erstellen**, um den Auftrag zu erstellen. Klicken Sie auf **Ressourcengruppen**, um wieder zum Auftrag zu gelangen. In diesem Tutorial wird **ContosoResources** verwendet. Wählen Sie die Ressourcengruppe aus, und klicken Sie anschließend in der Ressourcenliste auf den Stream Analytics-Auftrag. 

### <a name="add-an-input-to-the-stream-analytics-job"></a>Hinzufügen einer Eingabe zum Stream Analytics-Auftrag

1. Klicken Sie unter **Auftragstopologie** auf **Eingaben**.

1. Klicken Sie im Bereich **Eingaben** auf **Datenstromeingabe hinzufügen**, und wählen Sie „IoT Hub“ aus. Füllen Sie im daraufhin angezeigten Bildschirm die folgenden Felder aus:

   **Eingabealias**: In diesem Tutorial wird **contosoinputs** verwendet.

   **Abonnement**: Wählen Sie Ihr Abonnement aus.

   **IoT Hub**: Wählen Sie die IoT Hub-Instanz aus. In diesem Tutorial wird **ContosoTestHub** verwendet.

   **Endpunkt**: Wählen Sie **Messaging** aus. (Wenn Sie die Vorgangsüberwachung auswählen, erhalten Sie nicht die Daten, die Sie senden, sondern die Telemetriedaten zu Ihrer IoT Hub-Instanz.) 

   **Name der SAS-Richtlinie**: Wählen Sie **iothubowner** aus. Das Portal füllt den Schlüssel für die Richtlinie für den gemeinsamen Zugriff für Sie aus.

   **Consumergruppe**: Wählen Sie die Consumergruppe aus, die Sie zuvor erstellt haben. In diesem Tutorial wird **contosoconsumers** verwendet.
   
   Übernehmen Sie in den restlichen Feldern die Standardwerte. 

   ![Screenshot: Einrichten der Eingaben für den Stream Analytics-Auftrag](./media/tutorial-routing/stream-analytics-job-inputs.png)

1. Klicken Sie auf **Speichern**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag

1. Klicken Sie unter **Auftragstopologie** auf **Ausgaben**.

1. Klicken Sie im Bereich **Ausgaben** auf **Hinzufügen**, und wählen Sie dann **Power BI** aus. Füllen Sie im daraufhin angezeigten Bildschirm die folgenden Felder aus:

   **Ausgabealias**: Der eindeutige Alias für die Ausgabe. In diesem Tutorial wird **contosooutputs** verwendet. 

   **Datasetname**: Name des Datasets, das in Power BI verwendet werden soll. In diesem Tutorial wird **contosodataset** verwendet. 

   **Tabellenname**: Name der Tabelle, die in Power BI verwendet werden soll. In diesem Tutorial wird **contosotable** verwendet.

   Übernehmen Sie in den restlichen Feldern die Standardwerte.

1. Klicken Sie auf **Autorisieren**, und melden Sie sich bei Ihrem Power BI-Konto an.

   ![Screenshot: Einrichten der Ausgaben für den Stream Analytics-Auftrag](./media/tutorial-routing/stream-analytics-job-outputs.png)

1. Klicken Sie auf **Speichern**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Konfigurieren der Abfrage des Stream Analytics-Auftrags

1. Klicken Sie unter **Auftragstopologie** auf **Abfrage**.

1. Ersetzen Sie `[YourInputAlias]` durch den Eingabealias des Auftrags. In diesem Tutorial wird **contosoinputs** verwendet.

1. Ersetzen Sie `[YourOutputAlias]` durch den Ausgabealias des Auftrags. In diesem Tutorial wird **contosooutputs** verwendet.

   ![Screenshot: Einrichten der Abfrage für den Stream Analytics-Auftrag](./media/tutorial-routing/stream-analytics-job-query.png)

1. Klicken Sie auf **Speichern**.

1. Schließen Sie den Abfragebereich. Daraufhin gelangen Sie wieder zur Ansicht mit den Ressourcen in der Ressourcengruppe. Klicken Sie auf den Stream Analytics-Auftrag. In diesem Tutorial hat er den Namen **contosoJob**.

### <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

Klicken Sie im Stream Analytics-Auftrag auf **Starten** > **Jetzt** > **Starten**. Sobald der Auftrag erfolgreich gestartet wurde, ändert sich der Status des Auftrags von **Beendet** in **Wird ausgeführt**.

Um den Power BI-Bericht einzurichten, benötigen Sie Daten. Richten Sie Power BI daher erst ein, wenn Sie das Gerät erstellt haben und die Gerätesimulationsanwendung ausgeführt wird.

## <a name="run-simulated-device-app"></a>Ausführen der App zur Simulation von Geräten

Weiter oben im Abschnitt zum Skriptsetup haben Sie ein Gerät für die Simulation mithilfe eines IoT-Geräts eingerichtet. In diesem Abschnitt laden Sie eine .NET-Konsolen-App herunter, die ein Gerät simuliert, das Gerät-zu-Cloud-Nachrichten an eine IoT Hub-Instanz sendet. Diese Anwendung sendet Nachrichten für jede der verschiedenen Routingmethoden. 

Laden Sie die Lösung für die [IoT-Gerätesimulation](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) herunter. Dieser Download enthält ein Repository mit zahlreichen Anwendungen. Die von Ihnen benötigte Lösung befindet sich in „iot-hub/Tutorials/Routing/SimulatedDevice/“.

Doppelklicken Sie auf die Lösungsdatei (SimulatedDevice.sln), um den Code in Visual Studio zu öffnen. Öffnen Sie anschließend „Program.cs“. Ersetzen Sie `{iot hub hostname}` durch den IoT Hub-Hostnamen. Das Format des IoT Hub-Hostnamens ist **{iot-hub-name}.azure-devices.net**. Für dieses Tutorial lautet der Hubhostname **ContosoTestHub.azure-devices.net**. Ersetzen Sie jetzt `{device key}` durch den Geräteschlüssel, den Sie zuvor beim Einrichten des simulierten Geräts gespeichert haben. 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Ausführen und Testen 

Führen Sie die Konsolenanwendung aus. Warten Sie ein paar Minuten. Sie sehen die Nachrichten, die an den Konsolenbildschirm der Anwendung gesendet werden.

Die App sendet jede Sekunde eine neue Gerät-zu-Cloud-Nachricht an die IoT Hub-Instanz. Die Nachricht enthält ein JSON-serialisiertes Objekt mit Geräte-ID, Temperatur und Luftfeuchtigkeit sowie der Nachrichtenebene, die standardmäßig `normal` lautet. Nach dem Zufallsprinzip wird die Ebene `critical` oder `storage` zugewiesen, sodass die Nachricht an das Speicherkonto oder die Service Bus-Warteschlange weitergeleitet wird (diese löst das Senden einer E-Mail durch Ihre Logik-App aus). Die Ablesewerte für die Standardeinstellung (`normal`) werden in dem BI-Bericht angezeigt, den Sie als Nächstes einrichten.

Wenn alles ordnungsgemäß eingerichtet wurde, sollten Sie jetzt die folgenden Ergebnisse sehen:

1. Ab jetzt erhalten Sie E-Mails mit kritischen Nachrichten. 

   ![Screenshot: resultierende E-Mail](./media/tutorial-routing/results-in-email.png)

   Dies bedeutet Folgendes:

   * Die Weiterleitung an die Service Bus-Warteschlange funktioniert ordnungsgemäß.
   * Die Logik-App, die die Nachricht aus der Service Bus-Warteschlange abruft, funktioniert ordnungsgemäß.
   * Der Logik-App-Connector zu Outlook funktioniert ordnungsgemäß. 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressourcengruppen**, und wählen Sie Ihre Ressourcengruppe aus. In diesem Tutorial wird **ContosoResources** verwendet. Wählen Sie das Speicherkonto aus, klicken Sie auf **Blobs**, und wählen Sie dann den Container aus. In diesem Tutorial wird **contosoresults** verwendet. Es wird ein Ordner angezeigt, und Sie können ein Drilldown durch die Verzeichnisse ausführen, bis Dateien angezeigt werden. Öffnen Sie eine dieser Dateien. Sie enthalten die Einträge, die an das Speicherkonto weitergeleitet wurden. 

   ![Screenshot: Ergebnisdateien im Speicher](./media/tutorial-routing/results-in-storage.png)

Dies bedeutet Folgendes:

   * Die Weiterleitung an das Speicherkonto funktioniert ordnungsgemäß.

Richten Sie die Power BI-Visualisierung ein, während die Anwendung weiter ausgeführt wird, um die Nachrichten anzuzeigen, die per Standardrouting weitergeleitet werden. 

## <a name="set-up-the-power-bi-visualizations"></a>Einrichten der Power BI-Visualisierungen

1. Melden Sie sich bei Ihrem [Power BI](https://powerbi.microsoft.com/)-Konto an.

1. Wechseln Sie zum **Arbeitsbereiche**, und wählen Sie den Arbeitsbereich aus, den Sie festgelegt haben, als Sie die Ausgabe für den Stream Analytics-Auftrag erstellt haben. In diesem Tutorial wird **My Workspace** verwendet. 

1. Klicken Sie auf **Datasets**.

   Es sollte das Dataset aufgelistet sein, das Sie beim Erstellen der Ausgabe für den Stream Analytics-Auftrag angegeben haben. In diesem Tutorial wird **contosodataset** verwendet. (Es kann 5-10 Minuten dauern, bis das Dataset zum ersten Mal angezeigt wird.)

1. Klicken Sie unter **AKTIONEN** auf das erste Symbol, um einen Bericht zu erstellen.

   ![Screenshot: Power BI-Arbeitsbereich mit Aktionen und Berichtsymbol](./media/tutorial-routing/power-bi-actions.png)

1. Erstellen Sie ein Liniendiagramm, um die Temperatur in Echtzeit im Zeitverlauf anzuzeigen.

   a. Fügen Sie auf der Seite zum Erstellen von Berichten ein Liniendiagramm hinzu, indem Sie auf das Symbol für Liniendiagramme klicken.

   ![Screenshot: Visualisierungen und Felder](./media/tutorial-routing/power-bi-visualizations-and-fields.png)

   b. Erweitern Sie im Bereich **Felder** die Tabelle, die Sie beim Erstellen der Ausgabe für den Stream Analytics-Auftrag angegeben haben. In diesem Tutorial wird **contosotable** verwendet.

   c. Ziehen Sie **EventEnqueuedUtcTime** im Bereich **Visualisierungen** auf **Achse**.

   d. Ziehen Sie **Temperatur** auf **Werte**.

   Ein Liniendiagramm wird erstellt. Die x-Achse zeigt Datum und Uhrzeit in der Zeitzone UTC. Die y-Achse zeigt vom Sensor empfangene Temperatur an.

1. Erstellen Sie ein weiteres Liniendiagramm, um die Luftfeuchtigkeit in Echtzeit im Zeitverlauf anzuzeigen. Um das zweite Diagramm zu erstellen, gehen Sie wie oben beschrieben vor. Platzieren Sie **EventEnqueuedUtcTime** auf der X-Achse und **humidity** auf der Y-Achse.

   ![Screenshot: endgültiger Power BI-Bericht mit den beiden Diagrammen](./media/tutorial-routing/power-bi-report.png)

1. Klicken Sie auf **Speichern**, um den Bericht zu speichern.

In beiden Diagrammen sollten Daten angezeigt werden. Dies bedeutet Folgendes:

   * Die Weiterleitung an den Standardendpunkt funktioniert ordnungsgemäß.
   * Das Streaming des Azure Stream Analytics-Auftrags funktioniert ordnungsgemäß.
   * Die Power BI-Visualisierung ist ordnungsgemäß eingerichtet.

Sie können die Diagramme aktualisieren, um die neuesten Daten anzuzeigen. Klicken Sie zu diesem Zweck oben im Power BI-Fenster auf die Schaltfläche zum Aktualisieren. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Wenn Sie alle erstellten Ressourcen entfernen möchten, löschen Sie die Ressourcengruppe. Diese Aktion löscht alle in der Gruppe enthaltenen Ressourcen. In diesem Fall werden folgende Elemente entfernt: die IoT Hub-Instanz, der Service Bus-Namespace und die Service Bus-Warteschlange, die Logik-App, das Speicherkonto und die Ressourcengruppe selbst. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Bereinigen von Ressourcen in der Power BI-Visualisierung

Melden Sie sich bei Ihrem [Power BI-Konto](https://powerbi.microsoft.com/) an. Wechseln Sie zu Ihrem Arbeitsbereich. In diesem Tutorial wird **My Workspace** verwendet. Um die Power BI-Visualisierung zu entfernen, wechseln Sie zu „Datasets“, und klicken Sie auf das Papierkorbsymbol, um das Dataset zu löschen. In diesem Tutorial wird **contosodataset** verwendet. Wenn Sie das Dataset entfernen, wird auch der Bericht entfernt.

### <a name="clean-up-resources-using-azure-cli"></a>Bereinigen von Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle

Um die Ressourcengruppe zu entfernen, verwenden Sie den Befehl [az group delete](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-delete).

```azurecli-interactive
az group delete --name $resourceGroup
```
### <a name="clean-up-resources-using-powershell"></a>Bereinigen von Ressourcen über PowerShell

Um die Ressourcengruppe zu entfernen, verwenden Sie den Befehl [Remove-AzureRmResourceGroup](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup). $resourceGroup wurde zu Beginn des Tutorials auf **ContosoIoTRG1** festgelegt.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $resourceGroup
```


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie die Nachrichtenweiterleitung verwenden, um IoT Hub-Nachrichten an verschiedene Ziele weiterzuleiten. Dazu haben Sie folgende Aufgaben ausgeführt.  

> [!div class="checklist"]
> * Sie richten über die Azure-Befehlszeilenschnittstelle oder mithilfe von PowerShell die grundlegenden Ressourcen ein – eine IoT Hub-Instanz, ein Speicherkonto, eine Service Bus-Warteschlange und ein simuliertes Gerät.
> * Sie konfigurieren Endpunkte und Routen in IoT Hub für das Speicherkonto und die Service Bus-Warteschlange.
> * Sie erstellen eine Logik-App, die ausgelöst wird und eine E-Mail sendet, wenn der Service Bus-Warteschlange eine Nachricht hinzugefügt wird.
> * Sie laden eine App herunter und führen sie aus, die ein IoT-Gerät simuliert, das für die verschiedenen Weiterleitungsoptionen Nachrichten an den Hub sendet.
> * Sie erstellen eine Power BI-Visualisierung für die Daten, die an den Standardendpunkt gesendet werden.
> * Sie zeigen die Ergebnisse an:
> * – in der Service Bus-Warteschlange und in E-Mails
> * – im Speicherkonto
> * – in der Power BI-Visualisierung

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Status eines IoT-Geräts verwalten. 

> [!div class="nextstepaction"]
[Konfigurieren Ihrer Geräte über einen Back-End-Dienst](tutorial-device-twins.md)

 <!--  [Manage the state of a device](./tutorial-manage-state.md) -->
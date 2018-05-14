---
title: Erstellen eines Stream Analytics-Auftrags mithilfe von Azure PowerShell
description: In dieser Schnellstartanleitung wird ausführlich die Bereitstellung und Ausführung eines Azure Stream Analytics-Auftrags mithilfe des Azure PowerShell-Moduls beschrieben.
services: stream-analytics
author: SnehaGunda
ms.author: sngun
ms.date: 03/16/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 0be8cee9e6c7874282f4e8f43f75fa7f2490c14e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe von Azure PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über PowerShell-Cmdlets oder -Skripts. In dieser Schnellstartanleitung wird ausführlich die Bereitstellung und Ausführung eines Azure Stream Analytics-Auftrags mithilfe des Azure PowerShell-Moduls beschrieben.

## <a name="before-you-begin"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.  

* Für diese Schnellstartanleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu ermitteln, die auf Ihrem lokalen Computer installiert ist. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, lesen Sie den Artikel [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Das Szenario in diesem Artikel erläutert das Lesen von Daten aus dem Blobspeicher, das Transformieren der Daten und das Zurückschreiben in einen anderen Container im gleichen Blobspeicher.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und geben Sie im Popup-Browserfenster Ihre Azure-Anmeldeinformationen ein. Wählen Sie bei mehreren Abonnements nach der Anmeldung das Abonnement aus, das Sie für diesen Schnellstart verwenden möchten. Führen Sie dazu die folgenden Cmdlets aus. Ersetzen Sie <your subscription> durch den Namen Ihres Abonnements:  

```powershell
# Log in to your Azure account
Connect-AzureRmAccount

# List all available subscriptions.
Get-AzureRmSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzureRmSubscription -SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

```powershell
$resourceGroup = "ASAPSRG"
$location = "WestUS2"
New-AzureRMResourceGroup `
  -Name $resourceGroup `
  -Location $location 
```

## <a name="prepare-the-input-data"></a>Vorbereiten der Eingabedaten

Vor dem Definieren des Stream Analytics-Auftrags sollten Sie die Daten vorbereiten, die als Eingabe für den Auftrag konfiguriert werden. Führen Sie die folgenden Schritte aus, um die für den Auftrag erforderlichen Eingabedaten vorzubereiten: 

1. Laden Sie die [Sensorbeispieldaten](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) von GitHub herunter.  

2. Erstellen Sie mit dem Cmdlet [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) ein allgemeines Standardspeicherkonto mit LRS-Replikation.  

3. Rufen Sie den Kontext des Speicherkontos ab, der das zu verwendende Speicherkonto definiert. Beim Arbeiten mit Speicherkonten verweisen Sie auf den Kontext, statt die Anmeldeinformationen wiederholt anzugeben. In diesem Beispiel wird ein Speicherkonto namens „mystorageaccount“ mit lokal redundantem Speicher (Locally Redundant Storage, LRS) und Blobverschlüsselung (standardmäßig akiviert) erstellt.  

4. Erstellen Sie als Nächstes mit [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) einen Container, legen Sie als Berechtigung „Blob“ fest, um den öffentlichen Zugriff auf die Dateien zuzulassen, und laden Sie die zuvor heruntergeladenen [Sensorbeispieldaten](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) hoch. 

Führen Sie dazu das folgende PowerShell-Skript aus:

```powershell
$storageAccountName = "mystorageaccount"
$storageAccount = New-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
$containerName = "myinputcontainer"

New-AzureStorageContainer `
  -Name $containerName `
  -Context $ctx `
  -Permission blob

Set-AzureStorageBlobContent `
  -File "C:\HelloWorldASA-InputStream.json" `
  -Container $containerName `
  -Context $ctx  

$storageAccountKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName).Value[0]
```

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

Erstellen Sie mit dem Cmdlet [New-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0) einen Stream Analytics-Auftrag. Dieses Cmdlet übernimmt den Auftragsnamen, den Ressourcengruppennamen und die Auftragsdefinition als Parameter. Der Auftragsname kann ein beliebiger Anzeigename sein, der Ihren Auftrag identifiziert. Der Stream Analytics-Auftragsname darf nur alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und muss zwischen 3 und 63 Zeichen lang sein. Bei der Auftragsdefinition handelt es sich um eine JSON-Datei mit den Eigenschaften, die zum Erstellen eines Auftrags erforderlich sind. Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen „JobDefinition.json“, und fügen Sie ihr die folgenden JSON-Daten hinzu:

```json
{    
   "location":"Central US",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"drop",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
}
}
```

Führen Sie als Nächstes das Cmdlet „New-AzureRMStreamAnalyticsJob“ aus. Ersetzen Sie dabei den Wert der Variablen „jobDefinitionFile“ durch den Pfad, unter dem Sie die JSON-Datei mit der Auftragsdefinition gespeichert haben. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  –File $jobDefinitionFile `
  –Name $jobName -Force 
```

## <a name="configure-input-to-the-job"></a>Konfigurieren einer Eingabe für den Auftrag

Fügen Sie mit dem Cmdlet [New-AzureRMStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0) eine Eingabe zu Ihrem Auftrag hinzu. Dieses Cmdlet übernimmt den Auftragsnamen, den Auftragseingabenamen, den Ressourcengruppennamen und die Auftragseingabedefinition als Parameter. Bei der Auftragseingabedefinition handelt es sich um eine JSON-Datei mit den Eigenschaften, die zum Konfigurieren der Auftragseingabe erforderlich sind. In diesem Beispiel erstellen Sie einen Blobspeicher als Eingabe. Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen „JobInputDefinition.json“, und fügen Sie ihr die folgenden JSON-Daten hinzu. Ersetzen Sie dabei den Wert für **accountKey** durch den Zugriffsschlüssel Ihres Speicherkontos. Dies ist der in „$storageAccountKey“ gespeicherte Wert. 

```json
{
    "properties": {
        "type": "Stream",
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                {
                   "accountName": "mystorageaccount",
                   "accountKey":"<Your storage account key>"
                }],
                "container": "myinputcontainer",
                "pathPattern": "",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8"
            }
        }
    },
    "name": "MyBlobInput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/inputs"
}
```

Führen Sie als Nächstes das Cmdlet „New-AzureRMStreamAnalyticsInput“ aus. Ersetzen Sie dabei den Wert der Variablen „jobDefinitionFile“ durch den Pfad, unter dem Sie die JSON-Datei mit der Auftragseingabedefinition gespeichert haben. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRMStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>Konfigurieren einer Ausgabe für den Auftrag

Fügen Sie mit dem Cmdlet [New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0) eine Ausgabe zu Ihrem Auftrag hinzu. Dieses Cmdlet übernimmt den Auftragsnamen, den Auftragsausgabenamen, den Ressourcengruppennamen und die Auftragsausgabedefinition als Parameter. Bei der Auftragsausgabedefinition handelt es sich um eine JSON-Datei mit den Eigenschaften, die zum Konfigurieren der Auftragsausgabe erforderlich sind. In diesem Beispiel erstellen Sie einen Blobspeicher als Ausgabe. Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen „JobOutputDefinition.json“, und fügen Sie ihr die folgenden JSON-Daten hinzu. Ersetzen Sie dabei den Wert für **accountKey** durch den Zugriffsschlüssel Ihres Speicherkontos. Dies ist der in „$storageAccountKey“ gespeicherte Wert. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<Your storage account key>"
                    }],
                "container": "myoutputcontainer",
                "pathPattern": "",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8",
                "format": "LineSeparated"
            }
        }
    },
    "name": "MyBlobOutput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/outputs"
}
```

Führen Sie als Nächstes das Cmdlet „New-AzureRMStreamAnalyticsOutput“ aus. Ersetzen Sie dabei den Wert der Variablen „jobOutputDefinitionFile“ durch den Pfad, unter dem Sie die JSON-Datei mit der Auftragsausgabedefinition gespeichert haben. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRMStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobOutputDefinitionFile `
  –Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>Definieren der Transformationsabfrage

Fügen Sie Ihrem Auftrag mithilfe des Cmdlets [New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0) eine Transformation hinzu. Dieses Cmdlet übernimmt den Auftragsnamen, den Auftragstransformationsnamen, den Ressourcengruppennamen und die Auftragstransformationsdefinition als Parameter. Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen „JobTransformationDefinition.json“, und fügen Sie ihr die folgenden JSON-Daten hinzu. Die JSON-Datei enthält einen Abfrageparameter, der die Transformationsabfrage definiert:

```json
{     
   "name":"MyTransformation",  
   "type":"Microsoft.StreamAnalytics/streamingjobs/transformations",  
   "properties":{    
      "streamingUnits":1,  
      "script":null,  
      "query":" SELECT System.Timestamp AS OutputTime, dspl AS SensorName, Avg(temp) AS AvgTemperature INTO MyBlobOutput FROM MyBlobInput TIMESTAMP BY time GROUP BY TumblingWindow(second,30),dspl HAVING Avg(temp)>100"  
   }  
}
```

Führen Sie als Nächstes das Cmdlet „New-AzureRMStreamAnalyticsTransformation“ aus. Ersetzen Sie dabei den Wert der Variablen „jobTransformationDefinitionFile“ durch den Pfad, unter dem Sie die JSON-Datei mit der Auftragstransformationsdefinition gespeichert haben. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRMStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobTransformationDefinitionFile `
  –Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Starten des Stream Analytics-Auftrags und Überprüfen der Ausgabe

Starten Sie den Auftrag mit dem Cmdlet [Start-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Dieses Cmdlet übernimmt den Auftragsnamen, den Ressourcengruppennamen, den Ausgabestartmodus und die Startzeit als Parameter. „OutputStartMode“ akzeptiert zwei Werte: „JobStartTime“ und „CustomTime“ (LastOutputEventTime). Informationen zu diesen Werten finden Sie in der PowerShell-Dokumentation im Abschnitt [Parameter](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). In diesem Beispiel können Sie als Modus „CustomTime“ festlegen und einen Wert für „OutputStartTime“ angeben. 

Da die Datei vor dem aktuellen Datum in den Blobspeicher hochgeladen wurde, muss für den Zeitwert das Datum auf den Tag vor dem Upload festgelegt werden. Nach der Ausführung des folgenden Cmdlets wird als Ausgabe „True“ zurückgegeben, wenn der Auftrag gestartet wird. Im Speicherkonto wird der Container „myoutputcontainer“ mit den transformierten Daten erstellt. 

```powershell
Start-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-03-11T14:45:12Z 
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Streamingauftrag und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Durch das Löschen des Auftrags verhindern Sie, dass Kosten für die vom Auftrag verbrauchten Streamingeinheiten anfallen. Wenn Sie den Auftrag in Zukunft verwenden möchten, können Sie ihn beenden und später bei Bedarf neu starten. Wenn Sie diesen Auftrag nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen. Führen Sie dazu das folgende Cmdlet aus:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen einfachen Stream Analytics-Auftrag bereitgestellt. Wenn Sie Informationen zum Konfigurieren anderer Eingabequellen sowie zum Ausführen der Echtzeiterkennung benötigen, lesen Sie den folgenden Artikel:

> [!div class="nextstepaction"]
> [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md)

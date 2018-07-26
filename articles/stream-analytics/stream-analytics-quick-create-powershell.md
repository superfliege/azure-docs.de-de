---
title: Erstellen eines Stream Analytics-Auftrags mithilfe von Azure PowerShell
description: In dieser Schnellstartanleitung wird ausführlich die Bereitstellung und Ausführung eines Azure Stream Analytics-Auftrags mithilfe des Azure PowerShell-Moduls beschrieben.
services: stream-analytics
author: sidramadoss
ms.author: sidram
ms.date: 05/14/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 18903dfbe187de73a6edb14196fa29e02d35dbca
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185379"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe von Azure PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über PowerShell-Cmdlets oder -Skripts. In dieser Schnellstartanleitung wird ausführlich die Bereitstellung und Ausführung eines Azure Stream Analytics-Auftrags mithilfe des Azure PowerShell-Moduls beschrieben. 
 
Der Beispielauftrag liest Streamingdaten aus einem Blob in Azure Blob Storage. Die in diesem Schnellstart verwendete Eingabedatendatei enthält statische Daten, die lediglich der Veranschaulichung dienen. In der Praxis verwenden Sie später Streamingeingabedaten für einen Stream Analytics-Auftrag. Im nächsten Schritt transformiert der Auftrag die Daten unter Verwendung der Stream Analytics-Abfragesprache, um bei Überschreitung von 100° die Durchschnittstemperatur zu berechnen. Abschließend werden die entsprechenden Ausgabeereignisse in eine andere Datei geschrieben. 

## <a name="before-you-begin"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.  

* Für diese Schnellstartanleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu ermitteln, die auf Ihrem lokalen Computer installiert ist. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, lesen Sie den Artikel [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). 


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und geben Sie im Popup-Browserfenster Ihre Azure-Anmeldeinformationen ein.

```powershell
# Log in to your Azure account
Connect-AzureRmAccount
```

Wählen Sie bei mehreren Abonnements nach der Anmeldung das Abonnement aus, das Sie für diesen Schnellstart verwenden möchten. Führen Sie dazu die folgenden Cmdlets aus. Ersetzen Sie <your subscription name> durch den Namen Ihres Abonnements:  

```powershell
# List all available subscriptions.
Get-AzureRmSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzureRmSubscription -SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

```powershell
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzureRmResourceGroup `
   -Name $resourceGroup `
   -Location $location 
```

## <a name="prepare-the-input-data"></a>Vorbereiten der Eingabedaten

Bereiten Sie vor dem Definieren des Stream Analytics-Auftrags die Daten vor, die als Eingabe für den Auftrag konfiguriert werden.

1. Laden Sie die [Sensorbeispieldaten](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) von GitHub herunter. Klicken Sie mit der rechten Maustaste auf den Link, und wählen Sie **Save Link As...** (Link speichern unter...) oder **Save target as** (Ziel speichern unter...).

2. Der folgende PowerShell-Codeblock führt mehrere Befehle aus, um die vom Eintrag benötigten Eingabedaten vorzubereiten. Sehen Sie sich die Abschnitte an, um den Code zu verstehen. 

   1. Erstellen Sie mit dem Cmdlet [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) ein allgemeines Standardspeicherkonto.  In diesem Beispiel wird ein Speicherkonto namens „mystorageaccount“ mit lokal redundantem Speicher (Locally Redundant Storage, LRS) und Blobverschlüsselung (standardmäßig akiviert) erstellt.  

   2. Rufen Sie den Kontext des Speicherkontos (`$storageAccount.Context`) ab, der das zu verwendende Speicherkonto definiert. Beim Arbeiten mit Speicherkonten verweisen Sie auf den Kontext, statt die Anmeldeinformationen wiederholt anzugeben. 

   3. Erstellen Sie mit [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) einen Speichercontainer, und laden Sie die zuvor heruntergeladenen [Sensorbeispieldaten](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) hoch. 

   4. Kopieren Sie den vom Code ausgegebenen Speicherschlüssel, und fügen Sie diesen Schlüssel in die JSON-Dateien für die spätere Erstellung der Ein- und Ausgaben des Streamingauftrags ein.

   ```powershell
   $storageAccountName = "mystorageaccount"
   $storageAccount = New-AzureRmStorageAccount `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName `
     -Location $location `
     -SkuName Standard_LRS `
     -Kind Storage
   
   $ctx = $storageAccount.Context
   $containerName = "streamanalytics"
   
   New-AzureStorageContainer `
     -Name $containerName `
     -Context $ctx
   
   Set-AzureStorageBlobContent `
     -File "c:\HelloWorldASA-InputStream.json" `
     -Blob "input/HelloWorldASA-InputStream.json" `
     -Container $containerName `
     -Context $ctx  
   
   $storageAccountKey = (Get-AzureRmStorageAccountKey `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName).Value[0]
   
   Write-Host "The <storage account key> placeholder needs to be replaced in your input and output json files with this key value:" 
   Write-Host $storageAccountKey -ForegroundColor Cyan
   ```

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

Erstellen Sie mit dem Cmdlet [New-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0) einen Stream Analytics-Auftrag. Dieses Cmdlet übernimmt den Auftragsnamen, den Ressourcengruppennamen und die Auftragsdefinition als Parameter. Beim Auftragsnamen kann es sich um einen beliebigen Anzeigenamen handeln, der Ihren Auftrag identifiziert. Er darf nur alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und muss zwischen 3 und 63 Zeichen lang sein. Bei der Auftragsdefinition handelt es sich um eine JSON-Datei mit den Eigenschaften, die zum Erstellen eines Auftrags erforderlich sind. Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen `JobDefinition.json`, und fügen Sie ihr die folgenden JSON-Daten hinzu:

```json
{    
   "location":"WestUS2",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"adjust",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
   }
}
```

Führen Sie anschließend das Cmdlet `New-AzureRmStreamAnalyticsJob` aus. Ersetzen Sie den Wert der Variablen `jobDefinitionFile` durch den Pfad, an dem Sie die JSON-Datei der Auftragsdefinition gespeichert haben. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -File $jobDefinitionFile `
  -Name $jobName `
  -Force 
```

## <a name="configure-input-to-the-job"></a>Konfigurieren einer Eingabe für den Auftrag

Fügen Sie mit dem Cmdlet [New-AzureRmStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0) eine Eingabe zu Ihrem Auftrag hinzu. Dieses Cmdlet übernimmt den Auftragsnamen, den Auftragseingabenamen, den Ressourcengruppennamen und die Auftragseingabedefinition als Parameter. Bei der Auftragseingabedefinition handelt es sich um eine JSON-Datei mit den Eigenschaften, die zum Konfigurieren der Eingabe eines Auftrags erforderlich sind. In diesem Beispiel erstellen Sie einen Blobspeicher als Eingabe. 

Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen `JobInputDefinition.json`, und fügen Sie ihr die folgenden JSON-Daten hinzu. Ersetzen Sie den Wert für `accountKey` durch den Zugriffsschlüssel Ihres Speicherkontos. Dieser Wert ist in „$storageAccountKey“ gespeichert. 

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
                   "accountKey":"<storage account key>"
                }],
                "container": "streamanalytics",
                "pathPattern": "input/",
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

Führen Sie als Nächstes das Cmdlet `New-AzureRmStreamAnalyticsInput` aus. Ersetzen Sie den Wert der Variablen `jobDefinitionFile` durch den Pfad, an dem Sie die JSON-Datei der Auftragseingabedefinition gespeichert haben. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRmStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>Konfigurieren einer Ausgabe für den Auftrag

Fügen Sie mit dem Cmdlet [New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0) eine Ausgabe zu Ihrem Auftrag hinzu. Dieses Cmdlet übernimmt den Auftragsnamen, den Auftragsausgabenamen, den Ressourcengruppennamen und die Auftragsausgabedefinition als Parameter. Bei der Auftragsausgabedefinition handelt es sich um eine JSON-Datei mit den Eigenschaften, die zum Konfigurieren der Ausgabe eines Auftrags erforderlich sind. In diesem Beispiel wird der Blobspeicher als Ausgabe verwendet. 

Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen `JobOutputDefinition.json`, und fügen Sie ihr die folgenden JSON-Daten hinzu. Ersetzen Sie den Wert für `accountKey` durch den Zugriffsschlüssel Ihres Speicherkontos. Dieser Wert ist in „$storageAccountKey“ gespeichert. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<storage account key>"
                    }],
                "container": "streamanalytics",
                "pathPattern": "output/",
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

Führen Sie anschließend das Cmdlet `New-AzureRmStreamAnalyticsOutput` aus. Ersetzen Sie den Wert der Variablen `jobOutputDefinitionFile` durch den Pfad, an dem Sie die JSON-Datei der Auftragsausgabedefinition gespeichert haben. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRmStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobOutputDefinitionFile `
  -Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>Definieren der Transformationsabfrage

Fügen Sie Ihrem Auftrag mithilfe des Cmdlets [New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0) eine Transformation hinzu. Dieses Cmdlet übernimmt den Auftragsnamen, den Auftragstransformationsnamen, den Ressourcengruppennamen und die Auftragstransformationsdefinition als Parameter. Erstellen Sie auf Ihrem lokalen Computer eine Datei mit dem Namen `JobTransformationDefinition.json`, und fügen Sie ihr die folgenden JSON-Daten hinzu. Die JSON-Datei enthält einen Abfrageparameter, der die Transformationsabfrage definiert:

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

Führen Sie anschließend das Cmdlet `New-AzureRmStreamAnalyticsTransformation` aus. Ersetzen Sie den Wert der Variablen `jobTransformationDefinitionFile` durch den Pfad, an dem Sie die JSON-Datei der Auftragstransformationsdefinition gespeichert haben. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRmStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobTransformationDefinitionFile `
  -Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Starten des Stream Analytics-Auftrags und Überprüfen der Ausgabe

Starten Sie den Auftrag mit dem Cmdlet [Start-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Dieses Cmdlet übernimmt den Auftragsnamen, den Ressourcengruppennamen, den Ausgabestartmodus und die Startzeit als Parameter. `OutputStartMode` akzeptiert die Werte `JobStartTime`, `CustomTime` und `LastOutputEventTime`. Weitere Informationen zur Bedeutung dieser Werte finden Sie in der PowerShell-Dokumentation im Abschnitt [Parameter](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Geben Sie in diesem Beispiel als Modus `CustomTime` und einen Wert für `OutputStartTime` an. 

Wählen Sie als Zeitwert `2018-01-01` aus. Dieses Startdatum wird gewählt, weil es dem Ereigniszeitstempel aus den Beispieldaten vorangestellt ist. Nach der Ausführung des folgenden Cmdlets wird als Ausgabe `True` zurückgegeben, wenn der Auftrag gestartet wird. Im Speichercontainer wird ein Ausgabeordner mit den transformierten Daten erstellt. 

```powershell
Start-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-01-01T00:00:00Z 
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Streamingauftrag und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Durch das Löschen des Auftrags verhindern Sie, dass Kosten für die vom Auftrag verbrauchten Streamingeinheiten anfallen. Wenn Sie den Auftrag in Zukunft verwenden möchten, können Sie den Löschvorgang überspringen und Auftrag vorläufig beenden. Wenn Sie diesen Auftrag nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen. Führen Sie dazu das folgende Cmdlet aus:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen einfachen Stream Analytics-Auftrag bereitgestellt. Wenn Sie Informationen zum Konfigurieren anderer Eingabequellen sowie zum Ausführen der Echtzeiterkennung benötigen, lesen Sie den folgenden Artikel:

> [!div class="nextstepaction"]
> [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md)

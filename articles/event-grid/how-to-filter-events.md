---
title: Filtern von Ereignissen für Azure Event Grid
description: Erläutert das Erstellen von Azure Event Grid-Abonnements, die Ereignisse filtern.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: tomfitz
ms.openlocfilehash: 8bf7ac9daf928c35a3d6efcac528d3372fa87c8a
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252042"
---
# <a name="filter-events-for-event-grid"></a>Filtern von Ereignissen für Event Grid

Dieser Artikel zeigt, wie Sie Ereignisse beim Erstellen eines Event Grid-Abonnements filtern können. Weitere Informationen zu den Optionen für die Ereignisfilterung finden Sie unter [Grundlegendes zur Ereignisfilterung für Event Grid-Abonnements](event-filtering.md).

## <a name="filter-by-event-type"></a>Filtern nach Ereignistyp

Beim Erstellen eines Event Grid-Abonnements können Sie angeben, welche [Ereignistypen](event-schema.md) an den Endpunkt gesendet werden sollen. Die Beispiele in diesem Abschnitt erstellen Ereignisabonnements für eine Ressourcengruppe, beschränken jedoch die Ereignisse, die an `Microsoft.Resources.ResourceWriteFailure` und `Microsoft.Resources.ResourceWriteSuccess` gesendet werden. Wenn Sie mehr Flexibilität beim Filtern von Ereignissen nach Ereignistypen benötigen, lesen Sie [Filtern nach erweiterten Operatoren und Datenfeldern](#filter-by-advanced-operators-and-data-fields).

Verwenden Sie für PowerShell den Parameter `-IncludedEventType` beim Erstellen des Abonnements.

```powershell
$includedEventTypes = "Microsoft.Resources.ResourceWriteFailure", "Microsoft.Resources.ResourceWriteSuccess"

New-AzureRmEventGridSubscription `
  -EventSubscriptionName demoSubToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Endpoint <endpoint-URL> `
  -IncludedEventType $includedEventTypes
```

Verwenden Sie für die Azure CLI den Parameter `--included-event-types`. Im folgenden Beispiel wird die Azure-CLI in einer Bash-Shell verwendet:

```azurecli
includedEventTypes="Microsoft.Resources.ResourceWriteFailure Microsoft.Resources.ResourceWriteSuccess"

az eventgrid event-subscription create \
  --name demoSubToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --included-event-types $includedEventTypes
```

Verwenden Sie für eine Resource Manager-Vorlage, die Eigenschaft `includedEventTypes`.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [
          "Microsoft.Resources.ResourceWriteFailure",
          "Microsoft.Resources.ResourceWriteSuccess"
        ]
      }
    }
  }
]
```

## <a name="filter-by-subject"></a>Filtern nach Betreff

Sie können Ereignisse nach dem Betreff in den Ereignisdaten filtern. Sie können einen Wert angeben, der dem Anfang oder Ende des Betreffs entspricht. Wenn Sie mehr Flexibilität beim Filtern von Ereignissen nach Betreff benötigen, lesen Sie [Filtern nach erweiterten Operatoren und Datenfeldern](#filter-by-advanced-operators-and-data-fields).

Im folgenden PowerShell-Beispiel erstellen Sie ein Ereignisabonnement, das nach dem Anfang des Betreffs filtert. Mit dem Parameter `-SubjectBeginsWith` können Sie Ereignisse auf solche für eine bestimmte Ressource beschränken. Sie übergeben die Ressourcen-ID einer Netzwerksicherheitsgruppe.

```powershell
$resourceId = (Get-AzureRmResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

New-AzureRmEventGridSubscription `
  -Endpoint <endpoint-URL> `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

Im nächste PowerShell-Beispiel wird ein Abonnement für einen Blob Storage erstellt. Es begrenzt die Ereignisse auf die, deren Betreff auf `.jpg` endet.

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName myResourceGroup -AccountName $storageName).Id

New-AzureRmEventGridSubscription `
  -EventSubscriptionName demoSubToStorage `
  -Endpoint <endpoint-URL> `
  -ResourceId $storageId `
  -SubjectEndsWith ".jpg"
```

Im folgenden Azure CLI-Beispiel erstellen Sie ein Ereignisabonnement, das nach dem Anfang des Betreffs filtert. Mit dem Parameter `--subject-begins-with` können Sie Ereignisse auf solche für eine bestimmte Ressource beschränken. Sie übergeben die Ressourcen-ID einer Netzwerksicherheitsgruppe.

```azurecli
resourceId=$(az resource show --name demoSecurityGroup --resource-group myResourceGroup --resource-type Microsoft.Network/networkSecurityGroups --query id --output tsv)

az eventgrid event-subscription create \
  --name demoSubscriptionToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --subject-begins-with $resourceId
```

Das nächste Azure CLI-Beispiel erstellt ein Abonnement für einen Blob Storage. Es begrenzt die Ereignisse auf die, deren Betreff auf `.jpg` endet.

```azurecli
storageid=$(az storage account show --name $storageName --resource-group myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name demoSubToStorage \
  --endpoint <endpoint-URL> \
  --subject-ends-with ".jpg"
```

Im folgenden Beispiel für eine Resource Manager-Vorlage erstellen Sie ein Ereignisabonnement, das nach dem Anfang des Betreffs filtert. Mit der Eigenschaft `subjectBeginsWith` können Sie Ereignisse auf solche für eine bestimmte Ressource beschränken. Sie übergeben die Ressourcen-ID einer Netzwerksicherheitsgruppe.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "[resourceId('Microsoft.Network/networkSecurityGroups','demoSecurityGroup')]",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

Das nächste Beispiel für eine Resource Manager-Vorlage erstellt ein Abonnement für einen Blob Storage. Es begrenzt die Ereignisse auf die, deren Betreff auf `.jpg` endet.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
    "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/', parameters('eventSubName'))]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectEndsWith": ".jpg",
        "subjectBeginsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

## <a name="filter-by-operators-and-data"></a>Filtern nach Operatoren und Daten

Um die erweiterte Filterung nutzen zu können, müssen Sie eine Vorschauerweiterung für Azure CLI installieren. Sie können [CloudShell](/azure/cloud-shell/quickstart) verwenden oder Azure CLI lokal installieren.

### <a name="install-extension"></a>Installieren der Erweiterung

In CloudShell:

* Wenn Sie die Erweiterung bereits installiert haben, aktualisieren Sie sie mit `az extension update -n eventgrid`.
* Wenn Sie die Erweiterung noch nicht installiert haben, installieren Sie sie mit `az extension add -n eventgrid`.

Für eine lokale Installation:

1. Deinstallieren Sie die Azure CLI lokal.
1. Installieren Sie die [neueste Version](/cli/azure/install-azure-cli) der Azure CLI.
1. Starten Sie das Befehlsfenster.
1. Deinstallieren Sie frühere Versionen der Erweiterung `az extension remove -n eventgrid`.
1. Installieren Sie die Erweiterung `az extension add -n eventgrid`.

Sie können jetzt die erweiterte Filterung verwenden.

### <a name="subscribe-with-advanced-filters"></a>Abonnieren mit erweiterten Filtern

Weitere Informationen zu den Operatoren und den Schlüsseln, die Sie für die erweiterte Filterung verwenden können, finden Sie unter [Erweiterte Filterung](event-filtering.md#advanced-filtering).

Das folgende Beispiel erstellt ein benutzerdefiniertes Thema. Es abonniert das benutzerdefinierte Thema und filtert nach einem Wert im Datenobjekt. Ereignisse, bei denen die Farbeigenschaft auf „Blau“, „Rot“ oder „Grün“ eingestellt ist, werden an das Abonnement gesendet.

```azurecli-interactive
topicName=<your-topic-name>
endpointURL=<endpoint-URL>

az group create -n gridResourceGroup -l eastus2
az eventgrid topic create --name $topicName -l eastus2 -g gridResourceGroup

topicid=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  -n demoAdvancedSub \
  --advanced-filter data.color stringin blue red green \
  --endpoint $endpointURL \
  --expiration-date "2018-11-30"
```

Beachten Sie, dass für das Abonnement ein Ablaufdatum festgelegt ist. Das Ereignisabonnement läuft nach diesem Datum automatisch ab. Legen Sie ein Ablaufdatum für Ereignisabonnements fest, die nur für einen begrenzten Zeitraum benötigt werden.

### <a name="test-filter"></a>Testen der Filter

Um den Filter zu testen, senden Sie ein Ereignis, bei dem im Farbfeld „Grün“ festgelegt ist.

```azurecli-interactive
topicEndpoint=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name $topicName -g gridResourceGroup --query "key1" --output tsv)

event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "green"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

Das Ereignis wird an Ihren Endpunkt gesendet.

Um ein Szenario zu testen, in dem das Ereignis nicht gesendet wird, senden Sie ein Ereignis, bei dem das Farbfeld auf „Gelb“ gesetzt ist.

```azurecli-interactive
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "yellow"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

Gelb ist nicht einer der im Abonnement angegebenen Werte, sodass das Ereignis nicht an Ihr Abonnement gesendet wird.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Überwachung von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Weitere Informationen zum Authentifizierungsschlüssel finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

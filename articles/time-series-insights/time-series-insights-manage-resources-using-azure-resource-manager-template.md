---
title: Verwalten Ihrer Azure Time Series Insights-Umgebung mit Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre Azure Time Series Insights-Umgebung mit dem Azure Resource Manager programmgesteuert verwalten.
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 12/08/2017
ms.openlocfilehash: 8355248f28a019ef4712f542c8eac731362330ce
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Erstellen von Time Series Insights-Ressourcen mit Azure Resource Manager-Vorlagen

In diesem Artikel wird das Erstellen und Bereitstellen von Time Series Insights-Ressourcen mithilfe von Azure Resource Manager-Vorlagen, PowerShell und des Time Series Insights-Ressourcenanbieters beschrieben.

Time Series Insights unterstützt die folgenden Ressourcen:
   | Ressource | BESCHREIBUNG |
   | --- | --- |
   | Environment | Eine Time Series Insights-Umgebung ist eine logische Gruppierung von Ereignissen, die aus Ereignisbrokern gelesen, gespeichert und für Abfragen verfügbar gemacht werden. Weitere Informationen finden Sie unter [Planen Ihrer Azure Time Series Insights-Umgebung](time-series-insights-environment-planning.md). |
   | Ereignisquelle | Eine Ereignisquelle ist eine Verbindung mit einem Ereignisbroker, von dem Time Series Insights Daten liest und Ereignisse für die Umgebung erfasst. Derzeit werden IoT Hub und Event Hub als Ereignisquellen unterstützt. |
   | Verweisdataset | Verweisdatasets stellen Metadaten zu den Ereignissen der Umgebung bereit. Metadaten in den Verweisdaten werden während der Erfassung mit Ereignissen verknüpft. Verweisdatasets werden über die dazugehörigen Ereignisschlüsseleigenschaften als Ressourcen definiert. Die eigentlichen Metadaten, aus denen das Verweisdataset besteht, wird über Datenebenen-APIs hochgeladen oder geändert. |
   | Zugriffsrichtlinie | Zugriffsrichtlinien gewähren Berechtigungen für die Erstellung von Datenabfragen, für die Bearbeitung von Verweisdaten in der Umgebung und für die Freigabe gespeicherter Abfragen und Perspektiven, die der Umgebung zugeordnet sind. Weitere Informationen finden Sie unter [Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal](time-series-insights-data-access.md). |

Eine Resource Manager-Vorlage ist eine JSON-Datei, mit der die Infrastruktur und Konfiguration von Ressourcen in einer Ressourcengruppe definiert wird. Weitere Informationen finden Sie in den folgenden Dokumenten:

- [Übersicht über den Azure Resource Manager – Bereitstellung von Vorlagen](../azure-resource-manager/resource-group-overview.md#template-deployment)
- [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)

Die Schnellstartvorlage [201-timeseriesinsights-environment-with-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) ist auf GitHub verfügbar. Mit dieser Vorlage werden eine Time Series Insights-Umgebung, eine untergeordnete Ereignisquelle, die für die Nutzung von Ereignissen von einem Event Hub konfiguriert ist, und Zugriffsrichtlinien erstellt, mit denen Zugriff auf die Daten der Umgebung gewährt wird. Wenn Sie keinen vorhandenen Event Hub angeben, wird einer für die Bereitstellung erstellt.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>Lokales Bereitstellen der Schnellstartvorlage mit PowerShell

Im folgenden Verfahren wird beschrieben, wie Sie PowerShell zum Bereitstellen einer Azure Resource Manager-Vorlage verwenden, mit der eine Time Series Insights-Umgebung, eine untergeordnete Ereignisquelle, die für die Nutzung von Ereignissen von einem Event Hub konfiguriert ist, und Zugriffsrichtlinien erstellen, mit denen Zugriff auf die Daten der Umgebung gewährt wird. Wenn Sie keinen vorhandenen Event Hub angeben, wird einer für die Bereitstellung erstellt.

Der Workflow sieht in etwa folgendermaßen aus:

1. Installieren Sie PowerShell.
1. Erstellen Sie die Vorlage und eine Parameterdatei.
1. Melden Sie sich in PowerShell bei Ihrem Azure-Konto an.
1. Erstellen Sie eine neue Ressourcengruppe, falls noch keine vorhanden ist.
1. Testen Sie die Bereitstellung.
1. Stellen Sie die Vorlage bereit.

### <a name="install-powershell"></a>Installieren von PowerShell

Installieren Sie Azure PowerShell gemäß den Anweisungen unter [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Erstellen einer Vorlage

Klonen oder kopieren Sie die Vorlage [201-timeseriesinsights-environment-with-eventhub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) von GitHub.

### <a name="create-a-parameters-file"></a>Erstellen einer Parameterdatei

Kopieren Sie die Datei [201-timeseriesinsights-environment-with-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json), um eine Parameterdatei zu erstellen.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "eventHubNamespaceName": {
          "value": "GEN-UNIQUE"
      },
      "eventHubName": {
          "value": "GEN-UNIQUE"
      },
      "consumerGroupName": {
          "value": "GEN-UNIQUE"
      },
      "environmentName": {
        "value": "GEN-UNIQUE"
      },
      "eventSourceName": {
        "value": "GEN-UNIQUE"
      }
  }
}
```

#### <a name="required-parameters"></a>Erforderliche Parameter

   | Parameter | BESCHREIBUNG |
   | --- | --- |
   | eventHubNamespaceName | Der Namespace der Quelle (Event Hub). |
   | eventHubName | Der Name der Quelle (Event Hub). |
   | consumerGroupName | Der Name der Consumergruppe, die vom Time Series Insights-Dienst verwendet wird, um die Daten vom Event Hub zu lesen. **HINWEIS:** Zur Vermeidung von Ressourcenkonflikten muss diese Consumergruppe dediziert für den Time Series Insights-Dienst verwendet werden und sollte nicht für andere Leser freigegeben werden. |
   | environmentName | Der Name der Umgebung. Der Name darf folgende Zeichen nicht enthalten: „<“, „>“, „%“, „&“, „:“, „\\“, „?“, „/“ oder beliebige Steuerzeichen. Alle anderen Zeichen sind zulässig.|
   | eventSourceName | Der Name der untergeordneten Ressource der Ereignisquelle. Der Name darf folgende Zeichen nicht enthalten: „<“, „>“, „%“, „&“, „:“, „\\“, „?“, „/“ oder beliebige Steuerzeichen. Alle anderen Zeichen sind zulässig. |

#### <a name="optional-parameters"></a>Optionale Parameter

   | Parameter | BESCHREIBUNG |
   | --- | --- |
   | existingEventHubResourceId | Eine optionale Ressourcen-ID eines vorhandenen Event Hub, der über die Ereignisquelle mit der Time Series Insights-Umgebung verbunden wird. **HINWEIS:** Der Benutzer, der die Vorlage bereitstellt, muss über Berechtigungen zum Durchführen des Vorgangs „listkeys“ auf dem Event Hub verfügen. Wenn kein Wert übergeben wird, wird von der Vorlage ein neuer Event Hub erstellt. |
   | environmentDisplayName | Ein optionaler Anzeigenamen, der in Tools oder Benutzeroberflächen anstelle des Umgebungsnamens angezeigt wird. |
   | environmentSkuName | Der Name der SKU. Weitere Informationen finden Sie auf der Seite [Time Series Insights – Preise](https://azure.microsoft.com/pricing/details/time-series-insights/).  |
   | environmentSkuCapacity | Die Einheitenkapazität der SKU. Weitere Informationen finden Sie auf der Seite [Time Series Insights – Preise](https://azure.microsoft.com/pricing/details/time-series-insights/).|
   | environmentDataRetentionTime | Die Mindestzeitspanne der Ereignisse einer Umgebung ist für Abfragen verfügbar. Der Wert muss im Format ISO 8601 angegeben werden, z.B. „P30D“ für eine Aufbewahrungsrichtlinie von 30 Tagen. |
   | eventSourceDisplayName | Ein optionaler Anzeigename, der in Tools oder Benutzeroberflächen anstelle des Ereignisquellennamens angezeigt wird. |
   | eventSourceTimestampPropertyName | Die Ereigniseigenschaft, die als Zeitstempel der Ereignisquelle verwendet wird. Wenn für timestampPropertyName kein Name oder wenn Null oder eine leere Zeichenfolge angegeben wird, wird der Zeitpunkt der Ereigniserstellung verwendet. |
   | eventSourceKeyName | Der Name des freigegebenen Zugriffsschlüssels, der vom Time Series Insights-Dienst verwendet wird, um eine Verbindung mit dem Event Hub herzustellen. |
   | accessPolicyReaderObjectIds | Eine Liste mit Objekt-IDs der Benutzer oder Anwendungen in Azure AD, die über Lesezugriff auf die Umgebung verfügen sollen. Die objectId des Dienstprinzipals kann abgerufen werden, indem das Cmdlet **Get-AzureRMADUser** oder **Get-AzureRMADServicePrincipal** aufgerufen wird. Die Erstellung einer Zugriffsrichtlinie für Azure AD-Gruppen wird noch nicht unterstützt. |
   | accessPolicyContributorObjectIds | Eine Liste mit Objekt-IDs der Benutzer oder Anwendungen in Azure AD, die über den Zugriff „Mitwirkender“ auf die Umgebung verfügen sollen. Die objectId des Dienstprinzipals kann abgerufen werden, indem das Cmdlet **Get-AzureRMADUser** oder **Get-AzureRMADServicePrincipal** aufgerufen wird. Die Erstellung einer Zugriffsrichtlinie für Azure AD-Gruppen wird noch nicht unterstützt. |

Beispielsweise wird die folgende Parameterdatei verwendet, um eine Umgebung und eine Ereignisquelle zu erstellen, über die Ereignisse von einem vorhandenen Event Hub gelesen werden. Außerdem werden hiermit zwei Zugriffsrichtlinien erstellt, mit denen der Zugriff „Mitwirkender“ auf die Umgebung gewährt wird.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventHubNamespaceName": {
            "value": "tsiTemplateTestNamespace"
        },
        "eventHubName": {
            "value": "tsiTemplateTestEventHub"
        },
        "consumerGroupName": {
            "value": "tsiTemplateTestConsumerGroup"
        },
        "environmentName": {
          "value": "tsiTemplateTestEnvironment"
        },
        "eventSourceName": {
          "value": "tsiTemplateTestEventSource"
        },
        "existingEventHubResourceId": {
          "value": "/subscriptions/{yourSubscription}/resourceGroups/MyDemoRG/providers/Microsoft.EventHub/namespaces/tsiTemplateTestNamespace/eventhubs/tsiTemplateTestEventHub"
        },
        "accessPolicyContributorObjectIds": {
            "value": [
                "AGUID001-0000-0000-0000-000000000000",
                "AGUID002-0000-0000-0000-000000000000"
            ]
        }
    }
  }
```

Weitere Informationen finden Sie im Artikel [Parameter](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Anmelden bei Azure und Festlegen des Azure-Abonnements

Führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus:

```powershell
Connect-AzureRmAccount
```

Sie werden aufgefordert, sich bei Ihrem Azure-Konto anzumelden. Führen Sie nach der Anmeldung den folgenden Befehl aus, um Ihre verfügbaren Abonnements anzuzeigen:

```powershell
Get-AzureRMSubscription
```

Dieser Befehl gibt eine Liste der verfügbaren Azure-Abonnements zurück. Wählen Sie ein Abonnement für die aktuelle Sitzung aus, indem Sie folgenden Befehl ausführen. Ersetzen Sie `<YourSubscriptionId>` durch die GUID des Azure-Abonnements, das Sie verwenden möchten:

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Festlegen der Ressourcengruppe

Wenn noch keine Ressourcengruppe vorhanden ist, können Sie mit dem Befehl **New-AzureRmResourceGroup** eine neue Ressourcengruppe erstellen. Geben Sie den Namen der gewünschten Ressourcengruppe und den gewünschten Speicherort ein. Beispiel: 

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Nach erfolgreicher Ausführung wird eine Zusammenfassung der neuen Ressourcengruppe angezeigt:

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Testen der Bereitstellung

Überprüfen Sie Ihre Bereitstellung, indem Sie das Cmdlet `Test-AzureRmResourceGroupDeployment` ausführen. Geben Sie beim Testen der Bereitstellung die Parameter exakt so an wie beim Ausführen der Bereitstellung.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

### <a name="create-the-deployment"></a>Erstellen der Bereitstellung

Um die neue Bereitstellung zu erstellen, führen Sie das Cmdlet `New-AzureRmResourceGroupDeployment` aus, und geben Sie bei entsprechender Aufforderung die erforderlichen Parameter an. Die Parameter enthalten einen Namen für Ihre Bereitstellung, den Namen Ihrer Ressourcengruppe und den Pfad oder die URL zur Vorlagendatei. Wenn der Parameter **Mode** nicht angegeben wurde, wird der Standardwert **Incremental** verwendet. Weitere Informationen finden Sie unter [Inkrementelle und vollständige Bereitstellungen](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

Mit dem folgenden Befehl werden Sie aufgefordert, die fünf erforderlichen Parameter im PowerShell-Fenster einzugeben:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

Um stattdessen eine Parameterdatei zu verwenden, geben Sie folgenden Befehl ein:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Sie können beim Ausführen des Bereitstellungs-Cmdlets auch Inlineparameter verwenden. Der Befehl lautet wie folgt:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Legen Sie zum Ausführen einer [vollständigen](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) Bereitstellung den Parameter **Mode** auf **Complete** fest:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Wenn die Ressourcen erfolgreich bereitgestellt wurden, wird im PowerShell-Fenster eine Zusammenfassung der Bereitstellung angezeigt:

```powershell
DeploymentName          : azuredeploy
ResourceGroupName       : MyDemoRG
ProvisioningState       : Succeeded
Timestamp               : 12/9/2017 01:06:54
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          eventHubNamespaceName  String               <eventHubNamespaceName>
                          eventHubName     String                     <eventHubName>
                          consumerGroupName  String                   <consumerGroupName>
                          existingEventHubResourceId  String
                          environmentName  String                     <environmentName>
                          environmentDisplayName  String
                          environmentSkuName  String                  S1
                          environmentSkuCapacity  Int                 1
                          environmentDataRetentionTime  String        P30D
                          eventSourceName  String                     <eventSourceName>
                          eventSourceDisplayName  String
                          eventSourceTimestampPropertyName  String
                          eventSourceKeyName  String                  manage
                          accessPolicyReaderObjectIds  Array          []
                          accessPolicyContributorObjectIds  Array     [
                            "AGUID001-0000-0000-0000-000000000000",
                            "AGUID002-0000-0000-0000-000000000000"
                          ]

Outputs                 :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          dataAccessFQDN   String
                          <guid>.env.timeseries.azure.com
```

## <a name="deploy-the-quickstart-template-through-the-azure-portal"></a>Bereitstellen der Schnellstartvorlage über das Azure-Portal

Die Startseite der Schnellstartvorlage auf GitHub enthält auch die Schaltfläche **Deploy to Azure** (In Azure bereitstellen). Wenn Sie darauf klicken, wird im Azure-Portal die Seite „Benutzerdefinierte Bereitstellung“ geöffnet. Auf dieser Seite können Sie Werte für jeden Parameter aus der Tabelle mit den [erforderlichen Parametern](time-series-insights-manage-resources-using-azure-resource-manager-template.md#required-parameters) oder den [optionalen Parametern](time-series-insights-manage-resources-using-azure-resource-manager-template.md#optional-parameters) eingeben oder auswählen. Wenn Sie nach dem Angeben der Einstellungen auf die Schaltfläche **Kauf** klicken, wird die Vorlagenbereitstellung initiiert.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur programmgesteuerten Verwaltung von Time Series Insights-Ressourcen mit REST-APIs finden Sie unter [Time Series Insights Management](https://docs.microsoft.com/rest/api/time-series-insights-management/) (Time Series Insights-Verwaltung).

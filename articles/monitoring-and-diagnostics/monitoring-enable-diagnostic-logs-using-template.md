---
title: Automatisches Aktivieren von Diagnoseeinstellungen mithilfe einer Resource Manager-Vorlage | Microsoft Do
description: Hier erfahren Sie, wie Sie mithilfe einer Resource Manager-Vorlage Diagnoseeinstellungen erstellen, mit denen Sie Ihre Diagnoseprotokolle an Event Hubs streamen oder in einem Speicherkonto speichern können.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a8a88a8c-4a48-4df6-8f7e-d90634d39c57
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2018
ms.author: johnkem
ms.openlocfilehash: 1963c4d7b2e895032862fae896c6d9ff7cf80806
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automatisches Aktivieren von Diagnoseeinstellungen bei der Ressourcenerstellung mithilfe einer Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie mithilfe einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md) Diagnoseeinstellungen für eine Ressource konfigurieren, wenn die Ressource erstellt wird. Dadurch können Sie automatisch mit dem Streamen Ihrer Diagnoseprotokolle und Metriken an Event Hubs beginnen, sie in einem Speicherkonto archivieren oder sie bei der Erstellung einer Ressource an Log Analytics senden.

Die Methode zum Aktivieren von Diagnoseprotokollen mithilfe einer Resource Manager-Vorlage hängt vom Ressourcentyp ab.

* **computefremden Ressourcen** (Netzwerksicherheitsgruppen, Logik-Apps, Automation oder Ähnliches) verwendeten Diagnoseeinstellungen finden Sie in [diesem Artikel](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **Computeressourcenressourcen** (WAD-LAD-basierte Ressourcen) verwendete WAD-/LAD-Konfigurationsdatei wird in [diesem Artikel](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

In diesem Artikel erfahren Sie, wie Sie die Diagnose mit diesen Methoden konfigurieren.

Die grundlegenden Schritte lauten wie folgt:

1. Erstellen Sie eine Vorlage als JSON-Datei, die die Ressourcenerstellung und die Diagnoseaktivierung beschreibt.
2. [Stellen Sie die Vorlage mithilfe einer beliebigen Bereitstellungsmethode bereit.](../azure-resource-manager/resource-group-template-deploy.md)

Im Anschluss finden Sie ein Beispiel für die JSON-Vorlagendatei, die für computefremde Ressourcen und für Computeressourcen generiert werden muss.

## <a name="non-compute-resource-template"></a>Vorlage für computefremde Ressourcen
Für computefremde Ressourcen müssen zwei Schritte ausgeführt werden:

1. Fügen Sie dem Parameterblob Parameter für den Speicherkontonamen, die Event Hub-Autorisierungsregel-ID und/oder die Log Analytics-Arbeitsbereichs-ID hinzu (um die Archivierung von Diagnoseprotokollen in einem Speicherkonto, das Streamen von Protokollen an Event Hubs und/oder das Senden von Protokollen an Log Analytics zu ermöglichen).
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Azure Resource ID of the Log Analytics workspace for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Fügen Sie im Ressourcenarray der Ressource, für die Sie Diagnoseprotokolle aktivieren möchten, eine Ressource vom Typ `[resource namespace]/providers/diagnosticSettings`hinzu.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/[parameters('settingName')]",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2017-05-01-preview",
        "properties": {
          "name": "[parameters('settingName')]",
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
          "eventHubName": "[parameters('eventHubName')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "category": "AllMetrics",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

Das Eigenschaftenblob für die Diagnoseeinstellung verwendet das in [diesem Artikel](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings/createorupdate)beschriebene Format. Das Hinzufügen der `metrics`-Eigenschaft ermöglicht Ihnen, auch Ressourcenmetriken an diese gleichen Ausgaben zu senden, vorausgesetzt dass [Azure Monitor-Metriken von der Ressource unterstützt werden](monitoring-supported-metrics.md).

Im folgenden vollständigen Beispiel wird eine Logik-App erstellt und das Streamen an Event Hubs sowie das Speichern in einem Speicherkonto aktiviert.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/[parameters('settingName')]",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2017-05-01-preview",
          "properties": {
            "name": "[parameters('settingName')]",
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Vorlage für Computeressourcen
Führen Sie die folgenden Schritte aus, um die Diagnose für eine Computeressource (beispielsweise für einen virtuellen Computer oder für einen Service Fabric-Cluster) zu aktivieren:

1. Fügen Sie die Azure-Diagnoseerweiterung der VM-Ressourcendefinition hinzu.
2. Geben Sie ein Speicherkonto und/oder eine Event Hub-Instanz als Parameter an.
3. Fügen Sie den Inhalt der WADCfg-XML-Datei in die XMLCfg-Eigenschaft ein, und achten Sie dabei darauf, dass alle XML-Zeichen ordnungsgemäß mit Escapezeichen versehen sind.

> [!WARNING]
> Der letzte Schritt ist unter Umständen nicht ganz einfach. [diesem Artikel](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) finden Sie ein Beispiel, in dem das Diagnosekonfigurationsschema in Variablen aufgeteilt wird, die korrekt mit Escapezeichen versehen und ordnungsgemäß formatiert sind.
> 
> 

Der gesamte Prozess wird in [diesem Dokument](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)beschrieben (einschließlich Beispielen).

## <a name="next-steps"></a>Nächste Schritte
* [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle.](monitoring-overview-of-diagnostic-logs.md)
* [Streamen Sie Azure-Diagnoseprotokolle an Event Hubs.](monitoring-stream-diagnostic-logs-to-event-hubs.md)


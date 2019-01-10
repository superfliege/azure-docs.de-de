---
title: Erstellen eines Log Analytics-Arbeitsbereichs mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit der Azure-Befehlszeilenschnittstelle einen Log Analytics-Arbeitsbereich erstellen, um Verwaltungslösungen und die Datensammlung in Ihren cloudbasierten und lokalen Umgebungen zu aktivieren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: magoedte
ms.openlocfilehash: a8fc517ba233c16fbe98ed42498b272cf4422107
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105687"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Erstellen eines Log Analytics-Arbeitsbereichs mit Azure CLI 2.0

Azure CLI 2.0 dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In diesem Schnellstart erfahren Sie, wie Sie mit Azure CLI 2.0 einen Log Analytics-Arbeitsbereich in Azure einrichten. Dabei handelt es sich um eine einzigartige Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen.  Die in diesem Artikel beschriebenen Schritte sind erforderlich, wenn Sie Daten aus den folgenden Quellen sammeln möchten:

* Azure-Ressourcen im Abonnement  
* Lokale Computer, die von System Center Operations Manager überwacht werden  
* Gerätesammlungen aus System Center Configuration Manager  
* Diagnose- oder Protokolldaten aus dem Azure-Speicher  
 
Informationen zu anderen Quellen in Ihrer Umgebung (etwa virtuelle Azure-Computer und virtuelle Windows- oder Linux-Computer) finden Sie in folgenden Themen:

* [Sammeln von Daten von virtuellen Azure-Computern](../../azure-monitor/learn/quick-collect-azurevm.md)
* [Sammeln von Daten von Linux-Hybridcomputern](../../azure-monitor/learn/quick-collect-linux-computer.md)
* [Sammeln von Daten von Windows-Hybridcomputern](quick-collect-windows-computer.md)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.30 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Informationen dazu.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs
Erstellen Sie mit [az group deployment create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create) einen Arbeitsbereich. Das folgende Beispiel erstellt den Arbeitsbereich *TestWorkspace* in der Ressourcengruppe *Lab* am Standort *eastus* (USA, Osten) mithilfe einer Resource Manager-Vorlage von Ihrem lokalen Computer. Die JSON-Vorlage ist so konfiguriert, dass Sie nur zur Eingabe des Namens für den Arbeitsbereich aufgefordert werden und Standardwerte für die anderen Parameter angegeben werden, die wahrscheinlich als Standardkonfiguration in Ihrer Umgebung verwendet werden. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie die Vorlage auch in einem Azure Storage-Konto speichern. Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und der Azure-Befehlszeilenschnittstelle](../../azure-resource-manager/resource-group-template-deploy-cli.md).

Für folgende Parameter wird ein Standardwert festgelegt:

* Standort: Als Standardwert wird „USA, Osten“ verwendet.
* SKU: Als Standardwert wird der neue, im Preismodell von April 2018 veröffentlichte Tarif pro GB verwendet.

>[!WARNING]
>Wenn Sie einen Log Analytics-Arbeitsbereich in einem Abonnement mit dem neuen Preismodell von April 2018 erstellen oder konfigurieren, ist **PerGB2018** als einziger gültiger Log Analytics-Tarif verfügbar. 
>

### <a name="create-and-deploy-template"></a>Erstellen und Bereitstellen der Vorlage

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Bearbeiten Sie die Vorlage entsprechend Ihren Anforderungen.  Informationen zu den unterstützten Eigenschaften und Werten finden Sie in der Referenz [Microsoft.OperationalInsights/workspaces template](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces). 
3. Speichern Sie diese Datei unter dem Namen **deploylaworkspacetemplate.json** in einem lokalen Ordner.   
4. Nun können Sie die Vorlage bereitstellen. Führen Sie die folgenden Befehle in dem Ordner mit der Vorlage aus:

    ```azurecli
    azure group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem Ergebnis angezeigt:

![Beispielergebnis nach abgeschlossener Bereitstellung](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Nächste Schritte
Sie besitzen einen verfügbaren Arbeitsbereich und können nun die Erfassung von Überwachungstelemetrie konfigurieren, Protokollsuchen zum Analysieren dieser Daten ausführen und eine Verwaltungslösung hinzufügen, um weitere daten- und analysebasierte Einblicke zu gewinnen.  

* Informationen zum Aktivieren der Datensammlung aus Azure-Ressourcen mit Azure-Diagnose oder Azure Storage finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md).  
* [Fügen Sie System Center Operations Manager als Datenquelle hinzu](../../azure-monitor/platform/om-agents.md), um Daten von Agents zu sammeln, die an die Operations Manager-Verwaltungsgruppe berichten, und sie im Log Analytics-Arbeitsbereich zu speichern.  
* Stellen Sie eine Verbindung mit [Configuration Manager](../../azure-monitor/platform/collect-sccm.md) her, um Computer zu importieren, die Sammlungen in der Hierarchie angehören.  
* Sehen Sie sich die verfügbaren [Verwaltungslösungen](../../azure-monitor/insights/solutions.md) sowie Informationen dazu an, wie Sie eine Lösung zu Ihrem Arbeitsbereich hinzufügen bzw. daraus entfernen.

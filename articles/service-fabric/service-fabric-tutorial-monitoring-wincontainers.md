---
title: "Überwachung und Diagnose für Windows-Container in Azure Service Fabric | Microsoft-Dokumentation"
description: "Einrichten der Überwachung und Diagnose für Windows-Container, die unter Azure Service Fabric orchestriert sind."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 16c9926d44f972d2b38028cb6ab1420de6b60533
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="monitor-windows-containers-on-service-fabric-using-oms"></a>Überwachen von Windows-Containern unter Service Fabric mit OMS

Dieser dritte Teil eines Tutorials führt Sie schrittweise durch die OMS-Einrichtung zum Überwachen Ihrer Windows-Container, die unter Service Fabric orchestriert sind.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren von OMS für Ihren Service Fabric-Cluster
> * Verwenden eines OMS-Arbeitsbereichs zum Anzeigen und Abfragen von Protokollen Ihrer Container und Knoten
> * Konfigurieren des OMS-Agents zum Erfassen von Container- und Knotenmetriken

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:
- Sie besitzen einen Cluster in Azure oder [erstellen ihn mit diesem Tutorial](service-fabric-tutorial-create-cluster-azure-ps.md)
- [Sie stellen eine Containeranwendung dafür bereit](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-oms-with-your-cluster-in-the-resource-manager-template"></a>Einrichten von OMS mit Ihrem Cluster in der Resource Manager-Vorlage

Falls Sie die im ersten Teil dieses Tutorials [bereitgestellte Vorlage](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) verwendet haben, sollte sie die folgenden Ergänzungen einer generischen Service Fabric Azure Resource Manager-Vorlage enthalten. Falls Sie einen eigenen Cluster besitzen, den Sie zur Überwachung von Containern mit OMS einrichten möchten:
* Nehmen Sie die folgenden Änderungen an Ihrer Resource Manager-Vorlage vor.
* Stellen Sie sie mithilfe von PowerShell bereit, um Ihren Cluster durch [Bereitstellen der Vorlage](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) zu aktualisieren. Azure Resource Manager erkennt, dass die Ressource vorhanden ist, und gibt sie als Upgrade aus.

### <a name="adding-oms-to-your-cluster-template"></a>Hinzufügen von OMS zu Ihrer Clustervorlage

Nehmen Sie die folgenden Änderungen in Ihrer *template.json*-Datei vor:

1. Fügen Sie Speicherort und Name des OMS-Arbeitsbereichs dem Abschnitt *parameters* hinzu:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your OMS Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your OMS workspace"
      }
    }
    ```

    Um einen beliebigen der beiden Werte zu ändern, fügen Sie dieselben Parameter Ihrer *template.parameters.json*-Datei hinzu, und ändern Sie die dort verwendeten Werte.

2. Fügen Sie den Namen der Lösung und die Lösung Ihren *Variablen* hinzu: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. Fügen Sie den OMS Microsoft Monitoring Agent als Erweiterung eines virtuellen Computers hinzu. Suchen Sie die Ressource für VM-Skalierungsgruppen: *resources* > *"apiVersion": "[variables('vmssApiVersion')]"*. Fügen Sie unter *properties* > *virtualMachineProfile* > *extensionProfile* > *extensions* die folgende Erweiterungsbeschreibung unter der Erweiterung *ServiceFabricNode* hinzu: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Fügen Sie den OMS-Arbeitsbereich als einzelne Ressource hinzu. Fügen Sie in *resources* nach der VM-Skalierungsgruppenressource Folgendes hinzu:
    
    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[Hier](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) finden Sie eine (in Teil 1 dieses Tutorials verwendete) Beispielvorlage, die alle diese Änderungen enthält und Ihnen bei Bedarf als Referenz zur Verfügung steht. Diese Änderungen fügen Ihrer Ressourcengruppe einen OMS-Log Analytics-Arbeitsbereich hinzu. Der Arbeitsbereich wird zum Erfassen von Service Fabric-Plattformereignissen aus den Speichertabellen konfiguriert, die mit dem [Microsoft Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md)-Agent konfiguriert wurden. Der OMS-Agent (Microsoft Monitoring Agent) wurde auch jedem Knoten in Ihrem Cluster als eine Erweiterung des virtuellen Computers hinzugefügt – dies bedeutet: Wenn Sie Ihren Cluster skalieren, wird der Agent automatisch auf jedem Computer konfiguriert und mit demselben Arbeitsbereich verknüpft.

Stellen Sie die Vorlage mit den neuen Änderungen zum Aktualisieren Ihres aktuellen Clusters bereit. Nach Abschluss des Vorgangs sollten die OMS-Ressourcen in Ihrer Ressourcengruppe angezeigt werden. Wenn der Cluster bereit ist, stellen Sie die Containeranwendung darauf bereit. Im nächsten Schritt richten wir die Containerüberwachung ein.

## <a name="add-the-container-monitoring-solution-to-your-oms-workspace"></a>Hinzufügen der Containerüberwachungslösung zu Ihrem OMS-Arbeitsbereich

Um Informationen zum Einrichten der Containerlösung in Ihrem Arbeitsbereich zu erhalten, suchen Sie nach *Container Monitoring Solution*, und erstellen Sie eine Ressource „Container“ (unter der Kategorie „Überwachung + Verwaltung“).

![Hinzufügen der Containerlösung](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

Wählen Sie bei Aufforderung zur Eingabe des *OMS-Arbeitsbereichs* den Arbeitsbereich aus, der in der Ressourcengruppe erstellt wurde, und klicken Sie auf **Erstellen**. Dadurch wird eine *Container Monitoring Solution* Ihrem Arbeitsbereich hinzugefügt, sodass der von der Vorlage bereitgestellte OMS-Agent automatisch veranlasst wird, mit dem Sammeln von Docker-Protokollen und -Statistiken zu beginnen. 

Navigieren Sie zurück zu *Ihrer Ressourcengruppe*, wo Sie jetzt die neu hinzugefügte Überwachungslösung finden sollten. Wenn Sie darauf klicken, sollte auf der Landing Page die Anzahl der von Ihnen ausgeführten Containerimages angezeigt werden. 

*Beachten Sie, dass 5 Instanzen des fabrikam-Containers aus [Teil 2](service-fabric-host-app-in-a-container.md) des Tutorials* ausgeführt werden.

![Landing Page für Containerlösung](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

Wenn Sie auf **Container Monitor Solution** klicken, gelangen Sie zu einem ausführlicheren Dashboard, auf dem Sie sowohl durch mehrere Bereiche als auch Abfragen in Log Analytics scrollen können. 

*Beachten Sie, dass ab September 2017 einige Updates der Lösung durchgeführt werden – ignorieren Sie alle Fehlermeldungen, die Sie vielleicht über Kubernetes-Ereignisse erhalten, da wir an der Integration mehrerer Orchestratoren in derselben Lösung arbeiten.*

Da der Agent Docker-Protokolle erfasst, werden standardmäßig *stdout* und *stderr* angezeigt. Wenn Sie nach rechts scrollen, sehen Sie Containerimagebestand, Status, Metriken und Beispielabfragen, die Sie ausführen könnten, um weitere nützliche Daten zu erhalten. 

![Containerlösungsdashboard](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Durch Klicken auf einen der folgenden Bereiche gelangen Sie zu der Log Analytics-Abfrage, die den angezeigten Wert generiert. Ändern Sie die Abfrage in *\**, um die verschiedenen Arten von Protokollen anzuzeigen, die erfasst werden. Von hier aus können Sie nach Containerleistung und Protokollen abfragen oder filtern bzw. Service Fabric-Plattformereignisse betrachten. Die Agents geben auch ständig einen Heartbeat von jedem Knoten aus, den Sie sich ansehen können, um sicherzustellen, dass immer noch Daten von allen Computern gesammelt werden, wenn sich Ihre Clusterkonfiguration ändert.   

![Containerabfrage](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>Konfigurieren des OMS-Agent zum Erfassen von Leistungsindikatoren

Ein weiterer Vorteil der Verwendung des OMS-Agent ist die Fähigkeit, die Leistungsindikatoren ändern zu können, die Sie über die OMS-Benutzeroberfläche erfassen möchten, anstatt den Azure-Diagnose-Agent konfigurieren und jedes Mal ein Upgrade auf Basis der Resource Manager-Vorlage durchführen zu müssen. Klicken Sie hierzu auf der Landing Page Ihrer Containerüberwachungslösung (oder Service Fabric-Lösung) auf **OMS-Portal**.

![OMS-Portal](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

So gelangen Sie zu Ihrem Arbeitsbereich im OMS-Portal, in dem Sie Ihre Lösungen anzeigen, benutzerdefinierte Dashboards erstellen sowie den OMS-Agent konfigurieren können. 
* Klicken Sie in der oberen rechten Ecke des Bildschirms auf das **Zahnrad**, um das Menü *Einstellungen* zu öffnen.
* Klicken Sie auf **Verbundene Quellen** > **Windows-Server**, um sicherzustellen: *5 Windows-Computer sind verbunden*.
* Klicken Sie auf **Daten** > **Windows-Leistungsindikatoren**, um neue Leistungsindikatoren zu suchen und hinzuzufügen. Hier sehen Sie eine Liste der OMS-Empfehlungen für Leistungsindikatoren, die Sie erfassen könnten, sowie die Option zum Suchen nach anderen Indikatoren. Klicken Sie auf **Ausgewählte Leistungsindikatoren hinzufügen**, um mit dem Sammeln der vorgeschlagenen Metriken zu beginnen.

    ![Perf counters](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

Zurück im Azure-Portal, **aktualisieren** Sie Ihre Containerüberwachungslösung in ein paar Minuten, und Sie sollten zunächst den Eingang der Daten für *Computerleistung* sehen. Dies hilft Ihnen zu verstehen, wie Ihre Ressourcen verwendet werden. Sie können diese Metriken auch für entsprechende Entscheidungen zur Skalierung Ihrer Cluster verwenden, oder um zu überprüfen, ob ein Cluster Ihre Last wie erwartet ausgleicht.

*Hinweis: Stellen Sie sicher, dass Ihre Zeitfilter gemäß Ihrer Nutzung dieser Metriken festgelegt sind.* 

![Leistungsindikatoren 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren von OMS für Ihren Service Fabric-Cluster
> * Verwenden eines OMS-Arbeitsbereichs zum Anzeigen und Abfragen von Protokollen Ihrer Container und Knoten
> * Konfigurieren des OMS-Agent zum Erfassen von Container- und Knotenmetriken

Nun haben Sie die Überwachung für Ihre Containeranwendung eingerichtet und können Folgendes versuchen:

* Richten Sie die OMS für einen Linux-Cluster ein, und befolgen Sie dabei ähnliche Schritte wie oben beschrieben. Nutzen Sie [diese Vorlage](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux), um Änderungen in der Resource Manager-Vorlage vorzunehmen.
* Konfigurieren Sie die OMS für die Einrichtung von [automatisierten Warnungen](../log-analytics/log-analytics-alerts.md) bei der Erkennung und Diagnose.
* Untersuchen Sie die Service Fabric-Liste der [empfohlen Leistungsindikatoren](service-fabric-diagnostics-event-generation-perf.md), um Ihre Cluster zu konfigurieren.
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.

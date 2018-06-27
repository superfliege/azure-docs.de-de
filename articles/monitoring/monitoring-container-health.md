---
title: Überwachen der Integrität von Azure Kubernetes Service (AKS) (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ganz einfach die Leistung Ihres AKS-Containers überprüfen können, um sich schnell einen Eindruck von der Auslastung Ihrer gehosteten Kubernetes-Umgebung zu verschaffen.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2018
ms.author: magoedte
ms.openlocfilehash: 7c4294947cba72b1638e77c2dd8de1f5ee37b62a
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285989"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Überwachen der Integrität von Azure Kubernetes Service-Containern (AKS) (Vorschauversion)

In diesem Artikel wird beschrieben, wie die Azure Monitor-Containerintegrität eingerichtet und verwendet wird, um die Leistung von Workloads zu überwachen, die in von Azure Kubernetes Service (AKS) gehosteten Kubernetes-Umgebungen bereitgestellt werden.  Die Überwachung des Kubernetes-Clusters und der Kubernetes-Container ist vor allem dann entscheidend, wenn Sie einen umfangreichen Produktionscluster mit mehreren Anwendungen verwalten.

Die Containerintegrität bietet Ihnen eine Möglichkeit zur Leistungsüberwachung, bei der anhand der Metriken-API die in Kubernetes verfügbaren Speicher- und Prozessormetriken von Controllern, Knoten und Containern erfasst werden.  Nach der Aktivierung der Containerintegrität werden diese Metriken automatisch für Sie mittels einer Containerversion des OMS-Agents für Linux erfasst und in Ihrem [Log Analytics](../log-analytics/log-analytics-overview.md)-Arbeitsbereich gespeichert.  Die darin enthaltenen vordefinierten Ansichten zeigen die Containerworkloads sowie deren Auswirkungen auf die Leistungsintegrität des Kubernetes-Clusters, um Ihnen einen Überblick über folgende Informationen zu bieten:  

* Im Knoten ausgeführte Container sowie deren durchschnittliche Prozessor- und Speicherauslastung zur Ermittlung von Ressourcenengpässen
* Ermittlung der Container in einem Controller und/oder in Pods, um die allgemeine Leistung für einen Controller oder Pod anzuzeigen 
* Überprüfung der Ressourcenauslastung von Workloads, die unabhängig von den Standardprozessen, die den Pod unterstützen, im Host ausgeführt werden
* Informationen über das Verhalten des Clusters bei durchschnittlicher und maximaler Last, um Kapazitätsanforderungen und die maximal tolerierbare Last zu ermitteln 

Wenn Sie mehr über die Überwachung und Verwaltung Ihrer Docker- und Windows-Containerhosts für die Anzeige von Informationen zur Konfiguration, Überwachung und Ressourcenverwendung erfahren möchten, lesen Sie den Artikel zur [Containerüberwachungslösung](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Requirements (Anforderungen) 
Bevor Sie beginnen, überprüfen Sie die folgenden Details, um sich einen Überblick über die geforderten Voraussetzungen zu verschaffen.

- Die folgenden AKS-Clusterversionen werden unterstützt: 1.7.7 bis 1.9.6.
- Ein containerbasierter OMS-Agent für Linux-Version microsoft/oms:ciprod04202018 und höher. Dieser Agent wird automatisch beim Onboardingvorgang der Containerintegrität installiert.  
- Einen Log Analytics-Arbeitsbereich  Sie können diesen bei der Aktivierung der Überwachung des neuen AKS-Clusters erstellen oder über den [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md) erstellen.
- Ein Mitglied der Mitwirkendenrolle in Log Analytics, um Containerüberwachung zu aktivieren.  Weitere Informationen zum Steuern des Zugriffs auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Komponenten 

Für diese Funktion muss ein containerbasierter OMS-Agent für Linux Leistungs- und Ereignisdaten von allen Knoten im Cluster sammeln.  Der Agent wird automatisch bereitgestellt und beim angegebenen Log Analytics-Arbeitsbereich registriert, nachdem Sie die Containerüberwachung aktiviert haben. 

>[!NOTE] 
>Wenn Sie bereits einen AKS-Cluster bereitgestellt haben, können Sie die Überwachung mithilfe einer bereitgestellten Azure Resource Manager-Vorlage aktivieren, wie weiter unten in diesem Artikel erläutert wird. Sie können mit `kubectl` kein Upgrade für den Agent durchführen oder diesen löschen, bereitstellen oder erneut bereitstellen.  
>

## <a name="sign-in-to-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an. 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Aktivieren der Containerintegritätsüberwachung für einen neuen Cluster
Sie können die Überwachung Ihres AKS-Clusters nur dann aktivieren, wenn Sie diesen über das Azure-Portal bereitstellen.  Führen Sie die Schritte im Schnellstartartikel [Schnellstart: Bereitstellen eines Azure Container Service-Clusters (AKS)](../aks/kubernetes-walkthrough-portal.md) durch.  Legen Sie auf der Seite **Überwachung** für die Option **Überwachung aktivieren** **Ja** fest, und wählen Sie dann einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen neuen.  

Nach der Aktivierung der Überwachung werden alle Konfigurationsaufgaben erfolgreich abgeschlossen. Sie können die Leistung Ihres Clusters über eine der beiden folgenden Methoden überwachen:

1. Direkt über das AKS-Cluster durch Auswahl von **Integrität** im linken Bereich.<br><br> 
2. Durch Klicken auf die Kachel **Containerintegrität überwachen** auf der AKS-Clusterseite für den ausgewählten Cluster.  Wählen Sie in Azure Monitor im linken Bereich **Integrität** aus.  

![Optionen zur Auswahl der Containerintegrität in AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Nachdem die Überwachung aktiviert wurde, dauert es ungefähr 15 Minuten, bis operative Daten für den Cluster angezeigt werden.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Aktivieren der Containerintegritätsüberwachung für vorhandene verwaltete Cluster
Die Aktivierung der Überwachung eines bereits bereitgestellten AKS-Containers kann entweder über das Azure-Portal oder mithilfe der bereitgestellten Azure Resource Manager-Vorlage mit dem PowerShell-Cmdlet **New-AzureRmResourceGroupDeployment** oder der Azure CLI vorgenommen werden.  


### <a name="enable-from-azure-portal"></a>Aktivieren im Azure-Portal
Führen Sie die folgenden Schritte aus, um die Überwachung des AKS-Containers vom Azure-Portal aus zu ermöglichen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Containers** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Kubernetes-Dienste** aus.<br><br> ![Azure-Portal](./media/monitoring-container-health/azure-portal-01.png)<br><br>  
2. Wählen Sie in der Liste mit den Containern einen Container aus.
3. Wählen Sie auf der Containerübersichtsseite **Containerintegrität überwachen** aus, sodass die Seite **Onboarding zu Containerintegrität und Protokollen** angezeigt wird.
4. Wenn Sie im selben Abonnement wie der Cluster über einen Log Analytics-Arbeitsbereich verfügen, wählen Sie ihn auf der Seite **Onboarding zu Containerintegrität und Protokollen** aus der Dropdownliste aus.  Die Liste wählt Standardarbeitsbereich und Speicherort vorab aus, wo der AKS-Container im Abonnement bereitgestellt wird. Alternativ können Sie **Neu erstellen** auswählen und einen neuen Arbeitsbereich in demselben Abonnement angeben.<br><br> ![Integritätsüberwachung für AKS-Container aktivieren](./media/monitoring-container-health/container-health-enable-brownfield.png) 

    Bei Auswahl von **Neu erstellen** wird der Bereich **Neuen Arbeitsbereich erstellen** angezeigt. **Region** ist standardmäßig die Region, in der Ihre Containerressource erstellt worden ist, und Sie können den Standardwert übernehmen oder eine andere Region auswählen und dann einen Namen für den Arbeitsbereich angeben.  Klicken Sie auf **Erstellen**, um Ihre Auswahl zu bestätigen.<br><br> ![Arbeitsbereich für Containerüberwachung definieren](./media/monitoring-container-health/create-new-workspace-01.png)  

    >[!NOTE]
    >Zu diesem Zeitpunkt können Sie keinen neuen Arbeitsbereich in der Region „USA, Westen-Mitte“ erstellen, sondern nur einen bereits vorhandenen Arbeitsbereich in dieser Region auswählen.  Obwohl Sie diese Region in der Liste auswählen können, wird die Bereitstellung zwar gestartet, jedoch kurz danach mit einem Fehler abgebrochen.  
    >
 
Nachdem die Überwachung aktiviert wurde, dauert es ungefähr 15 Minuten, bis operative Daten für den Cluster angezeigt werden. 

### <a name="enable-using-azure-resource-manager-template"></a>Aktivieren mithilfe der Azure Resource Manager-Vorlag
Diese Methode umfasst zwei JSON-Vorlagen – eine gibt die Konfiguration zum Aktivieren der Überwachung an, während die andere JSON-Vorlage von Ihnen zu konfigurierende Parameterwerte enthält, mit denen Folgendes festgelegt wird:

* Ressourcen-ID des AKS-Containers 
* Ressourcengruppe, in der der Cluster bereitgestellt wird 
* Log Analytics-Arbeitsbereich und -Region, in dem bzw. in der der Arbeitsbereichs erstellt werden soll 

Der Log Analytics-Arbeitsbereich muss manuell erstellt werden.  Arbeitsbereiche können über den [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md) eingerichtet werden.

Wenn Sie nicht mit den Prinzipien der Bereitstellung von Ressourcen mittels einer Vorlage über PowerShell vertraut sind, lesen Sie den Artikel [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) oder in Bezug auf die Azure CLI den Artikel [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und der Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Wenn Sie die Azure CLI verwenden möchten, müssen Sie zuerst die CLI installieren und lokal verwenden.  Sie benötigen mindestens Version 2.0.27 der Azure CLI oder höher. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-template"></a>Erstellen und Ausführen von Vorlagen

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. Speichern Sie diese Datei unter den Namen **existingClusterOnboarding.json** in einem lokalen Ordner.
3. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscroptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "East US"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "eastus"
       }
     }
    }
    ```

4. Aktualisieren Sie den Wert für **aksResourceId** und **aksResourceLocation** mit den Werten, die auf der Seite **AKS – Übersicht** für den AKS-Cluster angegeben werden.  Der Wert für **workspaceResourceId** ist die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs, darunter der Name des Arbeitsbereichs.  Geben Sie den Speicherort des Arbeitsbereichs auch für **workspaceRegion** an.    
5. Speichern Sie diese Datei unter den Namen **existingClusterParam.json** in einem lokalen Ordner.
6. Nun können Sie die Vorlage bereitstellen. 

    * Verwenden Sie die folgenden PowerShell-Befehle in dem Ordner mit der Vorlage:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem Ergebnis angezeigt:

        ```powershell
        provisioningState       : Succeeded
        ```

    * So führen Sie folgenden Befehl mit der Azure CLI unter Linux aus
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem Ergebnis angezeigt:

        ```azurecli
        provisioningState       : Succeeded
        ```
Nachdem die Überwachung aktiviert wurde, dauert es ungefähr 15 Minuten, bis operative Daten für den Cluster angezeigt werden.  

## <a name="verify-agent-deployed-successfully"></a>Überprüfen der erfolgreichen Bereitstellung des Agents
Um zu überprüfen, ob der OMS-Agent ordnungsgemäß bereitgestellt wurde, führen Sie den folgenden Befehl aus: ` kubectl get ds omsagent --namespace=kube-system`.

Die Ausgabe sollte ungefähr wie im folgenden Beispiel aussehen, um sicherzustellen, dass er ordnungsgemäß bereitgestellt wurde:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Anzeigen der Leistungsauslastung
Wenn Sie die Containerintegrität öffnen, wird auf der Seite sofort die Leistungsauslastung Ihrer Clusterknoten angezeigt.  Die Darstellung der Informationen zu Ihrem AKS-Cluster ist in drei Perspektiven unterteilt:

- Nodes 
- Controllers  
- Container

Die Zeilenhierarchie folgt dem Kubernetes-Objektmodell, das mit einem Knoten in Ihrem Cluster beginnt.  Wenn Sie den Knoten erweitern, wird mindestens ein im Knoten ausgeführter Pod angezeigt. Ist mehr als ein Container in einem Pod gruppiert, wird dieser als letzte Zeile in der Hierarchie angezeigt.<br><br> ![Beispiel für eine Kubernetes-Knotenhierarchie in der Leistungsansicht](./media/monitoring-container-health/container-performance-and-health-view-03.png)

Im oberen Bereich der Seite können Sie Controller oder Container auswählen und den Zustand sowie die Ressourcenauslastung für diese Objekte überprüfen.  Mit den Dropdownfeldern am oberen Bildschirmrand können Sie die Anzeige nach Namespace, Dienst und Knoten filtern. Wenn Sie stattdessen die Speicherauslastung überprüfen möchten, wählen Sie aus der Dropdownliste **Metrik** die Option **Arbeitsspeicher RSS** oder **Speicherarbeitssatz** aus.  **Arbeitsspeicher RSS** wird nur für Kubernetes-Version 1.8 und höher unterstützt. Andernfalls werden Werte für **MITTELW. %** als *NaN%* angezeigt, ein numerischer Datentypwert, der einen nicht definierten oder nicht darstellbaren Wert darstellt. 

![Leistungsansicht zu den Containerknoten](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Standardmäßig beziehen sich Leistungsdaten auf die letzten sechs Stunden, Sie können das Fenster jedoch über die Dropdownliste **Zeitraum** in der oberen rechten Ecke der Seite ändern. Gegenwärtig wird die Seite nicht automatisch aktualisiert, sodass sie sie manuell aktualisieren müssen. 

Beachten Sie im folgenden Beispiel, dass beim Knoten *aks-agentpool-3402399-0* der Wert für **Container** 10 lautet. Hierbei handelt es sich um ein Rollup der Gesamtzahl der bereitgestellten Container.<br><br> ![Beispiel für ein Rollup der Container pro Knoten](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Hierdurch können Sie schnell herausfinden, ob Sie über das richtige Verhältnis an Containern und Knoten im Cluster verfügen.  

In der folgenden Tabelle werden die Informationen beschrieben, die bei der Anzeige von Knoten dargestellt werden.

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Hosts |
| Status | Kubernetes-Ansicht zum Knotenstatus |
| MITTELW. % | Durchschnittlicher Prozentsatz von Knoten basierend auf der ausgewählten Metrik für den ausgewählten Zeitraum. |
| MITTELWERT | Tatsächlicher Durchschnittswert der Knoten basierend auf der ausgewählten Metrik für den ausgewählten Zeitraum.  Der Mittelwert wird anhand des festgelegten CPU-/Speichergrenzwert für einen Knoten gemessen. Bei Pods und Containern ist dies der vom Host gemeldete Mittelwert. |
| Container | Anzahl von Containern |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start oder Neustart eines Knotens verstrichen ist. |
| Pod | Nur für Container. Diese Spalte zeigt an, welche Pods enthalten sind. |
| Controllers | Nur für Container und Pods. Diese Spalte zeigt an, welcher Controller enthalten ist. Nicht alle Pods sind in einem Controller enthalten, weshalb einige Spalten eventuell keine Angaben enthalten. | 
| Trend MITTELW. % | Balkendiagrammtrend basierend auf der Metrik „MITTELW. %“ von Containern und Knoten |


Wählen Sie aus dem Selektor **Controller** aus.<br><br> ![Ansicht über Controllerauswahl](./media/monitoring-container-health/container-performance-and-health-view-08.png)

In dieser Ansicht können Sie die Leistungsintegrität Ihrer Controller sehen.<br><br> ![Leistungsansicht des Controllers <Name>](./media/monitoring-container-health/container-performance-and-health-view-05.png)

Die Zeilenhierarchie beginnt mit einem Controller und erweitert den Controller. Sie sehen mindestens einen Pod oder Container.  Wenn Sie einen Pod erweitern, wird in der letzten Zeile der Container angezeigt, der im Pod gruppiert ist.  

In der folgenden Tabelle werden die Informationen beschrieben, die bei der Anzeige von Controllern dargestellt werden.

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Controllers|
| Status | Status der Container, wenn die Ausführung für diesen mit einem Status wie den Folgenden abgeschlossen wurde, z.B. *Beendet*, *Fehlerhaft*, *Beendet* oder *Angehalten*. Wenn der Container ausgeführt wird, der Status jedoch entweder nicht richtig angezeigt oder nicht vom Agent übernommen wurde und seit mehr als 30 Minuten nicht antwortet, wird der Status auf *Unbekannt* gesetzt. |
| MITTELW. % | Durchschnittliches Rollup des Mittelwerts (in %) der einzelnen Entitäten für die ausgewählte Metrik. |
| MITTELWERT | Rollup der durchschnittlichen CPU-Millicore oder Speicherleistung des Containers.  Der Mittelwert wird ausgehend vom festgelegten CPU-/Speichergrenzwert für einen Pod gemessen. |
| Container | Gesamtanzahl der Container für den Controller oder Pod. |
| Neustarts | Rollup der Anzahl von Containerneustarts. |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start eines Containers verstrichen ist. |
| Pod | Nur für Container. Diese Spalte zeigt an, welche Pods enthalten sind. |
| Knoten | Nur für Container und Pods. Diese Spalte zeigt an, welcher Controller enthalten ist. | 
| Trend MITTELW. % | Balkendiagrammtrend zur Metrik „Mittelw. %“des Containers |

Wählen Sie aus dem Selektor **Container** aus.<br><br> ![Auswählen der Containeransicht](./media/monitoring-container-health/container-performance-and-health-view-09.png)

In dieser Ansicht können Sie die Leistungsintegrität Ihrer Container sehen.<br><br> ![Leistungsansicht des Controllers <Name>](./media/monitoring-container-health/container-performance-and-health-view-06.png)

In der folgenden Tabelle werden die Informationen beschrieben, die bei der Anzeige von Containern dargestellt werden.

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Controllers|
| Status | Rollup des Zustands der Container, sofern vorhanden. |
| MITTELW. % | Durchschnittliches Rollup des Mittelwerts (in %) der einzelnen Entitäten für die ausgewählte Metrik. |
| MITTELWERT | Rollup der durchschnittlichen CPU-Millicore oder Speicherleistung des Containers. Der Mittelwert wird ausgehend vom festgelegten CPU-/Speichergrenzwert für einen Pod gemessen. |
| Container | Gesamtanzahl der Container für den Controller.|
| Neustarts | Stellt den Zeitraum dar, der seit dem Start eines Containers verstrichen ist. |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start oder Neustart eines Containers verstrichen ist. |
| Pod | Informationen zum Speicherort von Pods |
| Knoten |  Der Knoten, in dem sich der Container befindet.  | 
| Trend MITTELW. % | Balkendiagrammtrend zur Metrik „Mittelw. %“des Containers |

## <a name="container-data-collection-details"></a>Details zur Datensammlung in Containern
Die Containerintegrität sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern. Alle drei Minuten werden Daten gesammelt.

### <a name="container-records"></a>Containerdatensätze

Die folgende Tabelle enthält Beispiele für die von der Containerintegrität gesammelten Datensätze und den Datentypen, die in Protokollsuchergebnissen angezeigt werden.

| Datentyp | Datentyp in der Protokollsuche | Felder |
| --- | --- | --- |
| Leistung von Hosts und Containern | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | `ContainerInventory` | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerimageinhalt | `ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Containerprotokoll | `ContainerLog` | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerdienstprotokoll | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |
| Containerknotenbestand | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Containerprozess | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Bestand der Pods in einem Kubernetes-Cluster | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Bestand der Knoten als Teil eines Kubernetes-Clusters | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes-Ereignisse | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message,  SourceSystem | 
| Dienste im Kubernetes-Cluster | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Leistungsmetriken für die zum Kubernetes-Cluster zugehörigen Knoten | Perf &#124; where ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;cpuUsageNanoCorescpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Leistungsmetriken für die zum Kubernetes-Cluster zugehörigen Container | Perf &#124; where ObjectName == “K8SContainer” | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Suchen von Protokollen zur Datenanalyse
Mit Log Analytics können Sie nach Trends suchen, Engpässe diagnostizieren, Prognosen erstellen oder Daten korrelieren, die Ihnen die Einschätzung ermöglichen, ob die aktuelle Clusterkonfiguration optimal funktioniert.  Es werden vordefinierte Protokollsuchen für die sofortige Verwendung bereitgestellt. Alternativ können diese so angepasst werden, dass die Informationen auf die von Ihnen bevorzugte Weise zurückgegeben werden. 

Im Arbeitsbereich können Sie interaktive Datenanalysen durchführen, indem Sie beim Erweitern eines Containers am äußersten rechten Rand die Option **Protokoll anzeigen** auswählen.  Direkt oberhalb der Seite, die Sie im Portal gesucht haben, wird die Seite **Protokollsuche** angezeigt.<br><br> ![Analysieren von Daten in Log Analytics](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

Die Containerprotokolle, die an Log Analytics weitergeleitet wurden, geben STDOUT und STDERR aus. Da die Containerintegrität die von Azure verwalteten Kubernetes-Cluster (AKS) überwacht, wird „kube-system“ aufgrund der umfangreichen generierten Datenmenge gegenwärtig nicht gesammelt.     

### <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche
Es ist oft hilfreich, die Erstellung von Abfragen ausgehend von einem oder zwei Beispielen zu beginnen und diese dann Ihren Anforderungen entsprechend anzupassen. Sie können mit den folgenden Beispielabfragen experimentieren, um komplexere Abfragen zu erstellen.

| Abfragen | BESCHREIBUNG | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Liste von Lebenszyklusinformationen aller Container| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Kubernetes-Ereignisse|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Imagebestand | 
| **Wählen Sie in Advanced Analytics Liniendiagramme aus**:<br> Perf<br> &#124; where ObjectName == "Container" and CounterName == "% Processor Time"<br> &#124; summarize AvgCPUPercent = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Container-CPU | 
| **Wählen Sie in Advanced Analytics Liniendiagramme aus**:<br> Perf &#124; where ObjectName == "Container" and CounterName == "Memory Usage MB"<br> &#124; summarize AvgUsedMemory = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Containerspeicher |

## <a name="how-to-stop-monitoring-with-container-health"></a>Gewusst wie: Anhalten der Überwachung mit der Containerintegrität
Einen AKS-Container mit aktivierter Überwachung, den Sie nicht mehr überwachen möchten, können Sie mit den bereitgestellten Azure Resource Manager-Vorlagen, dem PowerShell-Cmdlet **New-AzureRmResourceGroupDeployment** oder der Azure CLI *deaktivieren*.  Eine JSON-Vorlage gibt die Konfiguration zum *Deaktivieren* an, während die andere JSON-Vorlage die von Ihnen zu konfigurierenden Parameterwerte enthält, mit denen Sie die Ressourcen-ID des AKS-Clusters und die Ressourcengruppe, in der der Cluster bereitgestellt wird, festlegen.  Wenn Sie nicht mit den Prinzipien der Bereitstellung von Ressourcen mittels einer Vorlage über PowerShell vertraut sind, lesen Sie den Artikel [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) oder in Bezug auf die Azure CLI den Artikel [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und der Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Wenn Sie die Azure CLI verwenden möchten, müssen Sie zuerst die CLI installieren und lokal verwenden.  Sie benötigen mindestens Version 2.0.27 der Azure CLI oder höher. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-template"></a>Erstellen und Ausführen von Vorlagen

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       }
    },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": {
                "logAnalyticsWorkspaceResourceID": null
              }
            }
           }
         }
       }
      ]
    }
    ```

2. Speichern Sie diese Datei als **OptOutTemplate.json** in einem lokalen Ordner.
3. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Aktualisieren Sie den Wert für **aksResourceId** und **aksResourceLocation** mit den Werten des AKS-Clusters, die auf der Seite **Eigenschaften** für den ausgewählten Cluster angegeben werden.<br><br> ![Seite mit den Containereigenschaften](./media/monitoring-container-health/container-properties-page.png)<br>

    Kopieren Sie auf der Seite **Eigenschaften** außerdem die **Arbeitsbereichsressourcen-ID**.  Dieser Wert ist erforderlich, wenn Sie den Log Analytics-Arbeitsbereich später löschen möchten, was nicht im Rahmen dieses Prozesses erfolgt.  

5. Speichern Sie diese Datei als **OptOutParam.json** in einem lokalen Ordner.
6. Nun können Sie die Vorlage bereitstellen. 

    * So werden die folgenden PowerShell-Befehle in dem Ordner mit der Vorlage verwendet

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem zurückgegebenen Ergebnis angezeigt:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * So führen Sie folgenden Befehl mit der Azure CLI unter Linux aus

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem zurückgegebenen Ergebnis angezeigt:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Wenn der Arbeitsbereich nur als Unterstützung für die Clusterüberwachung erstellt wurde und nicht mehr benötigt wird, müssen Sie ihn manuell löschen. Wie ein Arbeitsbereich gelöscht wird, erfahren Sie unter [Löschen eines Log Analytics-Arbeitsbereichs mit dem Azure-Portal](../log-analytics/log-analytics-manage-del-workspace.md).  Denken Sie an die **Arbeitsbereichsressourcen-ID**, die Sie weiter oben in Schritt 4 kopiert haben. Diese werden Sie später benötigen.  

## <a name="troubleshooting"></a>Problembehandlung
Dieser Abschnitt enthält Informationen zum Durchführen der Problembehandlung mit der Containerintegrität.

Wenn die Containerintegrität erfolgreich aktiviert und konfiguriert wurde, beim Ausführen einer Protokollsuche jedoch keine Statusinformationen oder -ergebnisse in Log Analytics angezeigt werden, können Sie zum Diagnostizieren des Problems die folgenden Schritte ausführen.   

1. Überprüfen Sie den Zustand des Agents, indem Sie den folgenden Befehl ausführen: `kubectl get ds omsagent --namespace=kube-system`

    Die Ausgabe sollte in etwa wie im folgenden Beispiel aussehen, was darauf hinweist, dass der Agent in allen Knoten im Cluster ausgeführt wird.  Dieser Cluster enthält beispielsweise zwei Knoten. Sie sollten daher mit einem Wert rechnen, der mit der Anzahl der Knoten identisch ist.  

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```
2. Überprüfen Sie den Status des Pods, um sicherzustellen, dass er ausgeführt wird oder nicht durch den folgenden Befehl ausgeführt wird: `kubectl get pods --namespace=kube-system`

    Die Ausgabe sollte etwa dem folgenden Beispiel mit dem Status *Ausgeführt* für „omsagent“ ähneln:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

3. Überprüfen Sie die Agent-Protokolle. Bei der Bereitstellung des Container-Agents, wird durch OMI-Befehle eine Schnellüberprüfung ausgeführt und die Version des Agents und Docker-Anbieters angezeigt. Um anzuzeigen, ob der Agent erfolgreich eingebunden ist, führen Sie den folgenden Befehl aus: `kubectl logs omsagent-484hw --namespace=kube-system`

    Der Status sollte wie folgt aussehen:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Nächste Schritte

[Suchen Sie nach Protokollen](../log-analytics/log-analytics-log-search.md), um ausführliche Informationen zur Containerintegrität und Anwendungsleistung anzuzeigen.  

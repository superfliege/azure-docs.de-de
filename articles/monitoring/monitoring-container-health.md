---
title: Überwachen der Integrität von Azure Kubernetes Service (AKS) (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ganz einfach die Leistung Ihres AKS-Containers überprüfen können, um sich schnell einen Eindruck von der Auslastung Ihrer gehosteten Kubernetes-Umgebung zu verschaffen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: magoedte
ms.openlocfilehash: 8027149f3e5ace163bf380bc5362fcb101397986
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42144098"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Überwachen der Integrität von Azure Kubernetes Service-Containern (AKS) (Vorschauversion)

In diesem Artikel wird beschrieben, wie die Azure Monitor-Containerintegrität eingerichtet und verwendet wird, um die Leistung von Workloads zu überwachen, die in Kubernetes-Umgebungen bereitgestellt und von Azure Kubernetes Service (AKS) gehostet werden. Die Überwachung des Kubernetes-Clusters und der Kubernetes-Container ist vor allem dann entscheidend, wenn Sie einen umfangreichen Produktionscluster mit mehreren Anwendungen ausführen.

Die Containerintegrität bietet Ihnen eine Möglichkeit zur Leistungsüberwachung, indem anhand der Metrik-API die in Kubernetes verfügbaren Speicher- und Prozessormetriken von Controllern, Knoten und Containern erfasst werden. Nach der Aktivierung der Containerintegrität werden diese Metriken automatisch für Sie mittels einer Containerversion des Log Analytics-Agents für Linux erfasst und in Ihrem [Log Analytics](../log-analytics/log-analytics-overview.md)-Arbeitsbereich gespeichert. Die darin enthaltenen vordefinierten Ansichten zeigen die Containerworkloads sowie deren Auswirkungen auf die Leistungsintegrität des Kubernetes-Clusters. Dadurch haben Sie folgende Möglichkeiten:  

* Sie können auf dem Knoten ausgeführte Container sowie deren durchschnittliche Prozessor- und Speicherauslastung ermitteln. Mit diesem Wissen können Sie Ressourcenengpässe erkennen.
* Sie können feststellen, wo im Controller bzw. im Pod sich der Container befindet. Dadurch können Sie die Gesamtleistung des Controllers bzw. Pods anzeigen. 
* Sie können die Ressourcenauslastung von Workloads überprüfen, die unabhängig von den Standardprozessen, die den Pod unterstützen, im Host ausgeführt werden.
* Sie bekommen Einblicke in das Verhalten des Clusters bei durchschnittlichen und schwersten Lasten. So können Sie benötigte Kapazitäten ermitteln und die maximale Last bestimmen, die der Cluster toleriert. 

Wenn Sie mehr über die Überwachung und Verwaltung Ihrer Docker- und Windows-Containerhosts für die Anzeige von Informationen zur Konfiguration, Überwachung und Ressourcenverwendung erfahren möchten, lesen Sie den Artikel zur [Containerüberwachungslösung](../log-analytics/log-analytics-containers.md).

## <a name="prerequisites"></a>Voraussetzungen 
Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

- Ein neuer oder vorhandener AKS-Cluster.
- Ein containerbasierter Log Analytics-Agent für Linux-Version microsoft/oms:ciprod04202018 und höher. Die Versionsnummer wird durch eine Datumsangabe im folgenden Format dargestellt: *mmttjjjj*. Der Agent wird automatisch beim Onboardingvorgang der Containerintegrität installiert. 
- Einen Log Analytics-Arbeitsbereich Sie können ihn beim Aktivieren der Überwachung des neuen AKS-Clusters erstellen oder beim Onboarding einen Standardarbeitsbereich in der Standardressourcengruppe des AKS-Clusterabonnements erstellen lassen. Wenn Sie ihn selbst erstellen möchten, können Sie dies über den [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md) erledigen.
- Die Log Analytics-Mitwirkendenrolle, um Containerüberwachung zu aktivieren. Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Komponenten 

Die Leistungsüberwachung beruht auf einem containerbasierten Log Analytics-Agent für Linux, der Leistungs- und Ereignisdaten von allen Knoten im Cluster sammelt. Der Agent wird automatisch bereitgestellt und beim angegebenen Log Analytics-Arbeitsbereich registriert, nachdem Sie die Containerüberwachung aktiviert haben. 

>[!NOTE] 
>Wenn Sie bereits einen AKS-Cluster bereitgestellt haben, können Sie die Überwachung mithilfe der Azure-Befehlszeilenschnittstelle oder einer bereitgestellten Azure Resource Manager-Vorlage aktivieren (siehe weiter unten in diesem Artikel). Sie können mit `kubectl` kein Upgrade für den Agent durchführen oder diesen löschen, bereitstellen oder erneut bereitstellen. 
>

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Aktivieren der Containerintegritätsüberwachung für einen neuen Cluster
Sie können die Überwachung eines neuen AKS-Clusters bei der Bereitstellung im Azure-Portal oder über die Azure-Befehlszeilenschnittstelle aktivieren. Wenn Sie ihn über das Portal aktivieren möchten, führen Sie die Schritte im Schnellstartartikel [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](../aks/kubernetes-walkthrough-portal.md) durch. Legen Sie auf der Seite **Überwachung** für die Option **Überwachung aktivieren** **Ja** fest, und wählen Sie dann einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen neuen. 

Um die Überwachung eines mit der Azure-Befehlszeilenschnittstelle erstellten neuen AKS-Clusters zu aktivieren, führen Sie die Schritte im Schnellstartartikel im Abschnitt [Erstellen eines AKS-Clusters](../aks/kubernetes-walkthrough.md#create-aks-cluster) durch.  

>[!NOTE]
>Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.43 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

Nachdem Sie die Überwachung aktiviert haben, und alle Konfigurationsaufgaben erfolgreich abgeschlossen wurden, haben Sie zwei Möglichkeiten, die Leistung Ihres Clusters zu überwachen:

* Direkt über das AKS-Cluster durch Auswählen von **Integrität** im linken Bereich.
* Durch Auswählen der Kachel **Containerintegrität überwachen** auf der AKS-Clusterseite für den ausgewählten Cluster. Wählen Sie in Azure Monitor im linken Bereich **Integrität** aus. 

  ![Optionen zum Auswählen der Containerintegrität in AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Nachdem Sie die Überwachung aktiviert haben, kann es ca. 15 Minuten dauern, bis Sie die operativen Daten für den Cluster ansehen können. 

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Aktivieren der Containerintegritätsüberwachung für vorhandene verwaltete Cluster
Sie können die Überwachung eines bereits bereitgestellten AKS-Clusters entweder über die Azure-Befehlszeilenschnittstelle, das Portal oder die bereitgestellte Azure Resource Manager-Vorlage mit dem PowerShell-Cmdlet `New-AzureRmResourceGroupDeployment` aktivieren. 

### <a name="enable-monitoring-using-azure-cli"></a>Aktivieren der Überwachung mithilfe der Azure-Befehlszeilenschnittstelle
Der folgende Schritt aktiviert die Überwachung Ihres AKS-Clusters mithilfe der Azure-Befehlszeilenschnittstelle. In diesem Beispiel müssen Sie keinen Arbeitsbereich erstellen oder einen vorhandenen angeben. Dieser Befehl vereinfacht den Prozess durch Erstellen eines Standardarbeitsbereichs in der Standardressourcengruppe des AKS-Cluster-Abonnements, wenn noch keiner in der Region vorhanden ist.  Das Format des Standardarbeitsbereichs ähnelt *DefaultWorkspace-<GUID>-<Region>*.  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

Die Ausgabe ähnelt der folgenden:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-in-the-azure-portal"></a>Aktivieren der Überwachung im Azure-Portal
Führen Sie die folgenden Schritte aus, um die Überwachung des AKS-Containers über das Azure-Portal zu aktivieren:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. 
2. Geben Sie in die Ressourcenliste **Containers** ein.  
    Die Liste wird anhand Ihrer Eingabe gefiltert. 
3. Wählen Sie **Kubernetes-Dienste** aus.  

    ![Link zu Kubernetes-Diensten](./media/monitoring-container-health/azure-portal-01.png)

4. Wählen Sie in der Containerliste einen Container aus.
5. Wählen Sie auf der Containerübersichtsseite **Containerintegrität überwachen** aus.  
6. Wenn Sie im selben Abonnement wie der Cluster einen Log Analytics-Arbeitsbereich haben, wählen Sie ihn auf der Seite **Onboarding in Containerintegrität und Protokolle** aus der Dropdownliste aus.  
    Die Liste wählt Standardarbeitsbereich und Speicherort vorab aus, in dem der AKS-Container im Abonnement bereitgestellt wird. 

    ![Aktivieren der AKS-Containerintegritätsüberwachung](./media/monitoring-container-health/container-health-enable-brownfield-02.png)

>[!NOTE]
>Wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus dem Cluster erstellen möchten, befolgen Sie die Anweisungen in [Erstellen eines Log Analytics-Arbeitsbereichs](../log-analytics/log-analytics-quick-create-workspace.md). Achten Sie darauf, dass Sie den Arbeitsbereich in demselben Abonnement erstellen, in dem der AKS-Container bereitgestellt wird. 
 
Nachdem Sie die Überwachung aktiviert haben, kann es ca. 15 Minuten dauern, bis Sie die operativen Daten für den Cluster ansehen können. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Aktivieren der Überwachung mit einer Azure Resource Manager-Vorlage
Diese Methode umfasst zwei JSON-Vorlagen. Eine Vorlage gibt die Konfiguration zur Aktivierung der Überwachung an, während die andere Vorlage die zu konfigurierenden Parameterwerte enthält, mit denen Folgendes festgelegt wird:

* Ressourcen-ID des AKS-Containers 
* Ressourcengruppe, in der der Cluster bereitgestellt wird
* Log Analytics-Arbeitsbereich und -Region, in der der Arbeitsbereich erstellt wird 

Der Log Analytics-Arbeitsbereich muss manuell erstellt werden. Arbeitsbereiche können über den [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md) eingerichtet werden.

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Erstellen und Ausführen von Vorlagen

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
3. Fügen Sie die folgende JSON-Syntax in Ihre Datei ein:

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

4. Aktualisieren Sie die Werte für **aksResourceId** und **aksResourceLocation** mit den Werten, die auf der Seite **AKS – Übersicht** für den AKS-Cluster angegeben werden. Der Wert für **workspaceResourceId** ist die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs, darunter der Name des Arbeitsbereichs. Geben Sie den Speicherort des Arbeitsbereichs auch für **workspaceRegion** an. 
5. Speichern Sie diese Datei unter den Namen **existingClusterParam.json** in einem lokalen Ordner.
6. Nun können Sie die Vorlage bereitstellen. 

    * Verwenden Sie die folgenden PowerShell-Befehle in dem Ordner mit der Vorlage:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Führen Sie den folgenden Befehl mithilfe der Azure CLI für Linux aus:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

        ```azurecli
        provisioningState       : Succeeded
        ```
Nachdem Sie die Überwachung aktiviert haben, kann es ca. 15 Minuten dauern, bis Sie die operativen Daten für den Cluster ansehen können. 

## <a name="verify-agent-and-solution-deployment"></a>Überprüfen der Agent- und Lösungsbereitstellung
Ab Agent-Version *06072018* können Sie überprüfen, ob Agent und Lösung erfolgreich bereitgestellt wurden. In früheren Agent-Versionen können Sie nur die Agent-Bereitstellung überprüfen.

### <a name="agent-version-06072018-or-later"></a>Agent-Version 06072018 oder höher
Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Agent erfolgreich bereitgestellt wurde. 

```
kubectl get ds omsagent --namespace=kube-system
```

Die Ausgabe sollte wie folgt aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Um die Bereitstellung der Lösung zu überprüfen, führen Sie den folgenden Befehl aus:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Die Ausgabe sollte wie folgt aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Agent-Version vor 06072018

Um zu überprüfen, ob die Log Analytics-Agent-Version vor *06072018* ordnungsgemäß bereitgestellt wurde, führen Sie den folgenden Befehl aus:  

```
kubectl get ds omsagent --namespace=kube-system
```

Die Ausgabe sollte wie folgt aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Anzeigen der Konfiguration mit der Befehlszeilenschnittstelle
Verwenden Sie den Befehl `aks show`, um Details wie den Aktivierungsstatus der Lösung, die Ressourcen-ID für den Log Analytics-Arbeitsbereich und zusammenfassende Details zum Cluster abzurufen.  

```azurecli
az aks show -g <resoourceGroupofAKSCluster> -n <nameofAksCluster>
```

Nach wenigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zur Lösung im JSON-Format zurückgegeben.  In den Ergebnissen des Befehls sollte das Profil des Überwachungs-Add-Ons angezeigt werden, ähnlich der folgenden Beispielausgabe:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="view-performance-utilization"></a>Anzeigen der Leistungsauslastung
Wenn Sie die Containerintegrität öffnen, wird auf der Seite sofort die Leistungsauslastung Ihres gesamten Clusters angezeigt. Die Darstellung der Informationen zu Ihrem AKS-Cluster ist in vier Perspektiven unterteilt:

- Cluster
- Nodes 
- Controllers  
- Container

Die vier Leistungsliniendiagramme auf Registerkarte **Cluster** zeigen wichtige Leistungsmetriken Ihres Clusters an. 

![Beispielleistungsdiagramme auf der Registerkarte „Cluster“](./media/monitoring-container-health/container-health-cluster-perfview.png)

Das Leistungsdiagramm zeigt vier Leistungsmetriken an:

- **Knoten-CPU-Auslastung&nbsp;%**: eine aggregierte Ansicht der CPU-Auslastung für den gesamten Cluster. Sie können die Ergebnisse für den Zeitbereich filtern, indem Sie **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Perzentilselektor oberhalb des Diagramms auswählen, entweder einzeln oder kombiniert. 
- **Knoten-Arbeitsspeicherauslastung&nbsp;%**: Eine aggregierte Ansicht der Arbeitsspeicherauslastung für den gesamten Cluster. Sie können die Ergebnisse für den Zeitbereich filtern, indem Sie **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Perzentilselektor oberhalb des Diagramms auswählen, entweder einzeln oder kombiniert. 
- **Knotenanzahl**: Die Anzahl von Knoten und der Status von Kubernetes. Die Status der dargestellten Clusterknoten sind *Alle*, *Bereit* und *Nicht bereit*. Der Status kann im Selektor oberhalb des Diagramms einzeln oder kombiniert gefiltert werden. 
- **Aktivitätspodanzahl**: Die Anzahl von Pods und der Status von Kubernetes. Die Status der dargestellten Pods sind *Alle*, *Ausstehend*, *Wird ausgeführt* und *Unbekannt*. Der Status kann im Selektor oberhalb des Diagramms einzeln oder kombiniert gefiltert werden. 

Wenn Sie zur Registerkarte **Knoten**, **Controller** und **Container** wechseln, wird der Eigenschaftenbereich automatisch rechts auf der Seite angezeigt.  Dort werden die Eigenschaften des ausgewählten Elements einschließlich der Bezeichnungen angezeigt, die Sie definieren, um die Kubernetes-Objekte zu organisieren.  Klicken Sie auf den **>>**-Link im Bereich, um ihn anzuzeigen\auszublenden.  

![Beispiel für den Eigenschaftenbereich von Kubernetes-Perspektiven](./media/monitoring-container-health/perspectives-preview-pane-01.png)

Wenn Sie die Objekte in der Hierarchie erweitern, wird der Eigenschaftenbereich auf Basis des ausgewählten Objekts aktualisiert. Im Bereich können Sie auch Kubernetes-Ereignisse mit vordefinierten Protokollsuchen anzeigen, indem Sie am oberen Rand des Bereichs auf **Kubernetes-Ereignisprotokolle anzeigen** klicken. Weitere Informationen zum Anzeigen von Kubernetes-Protokolldaten finden Sie unter [Suchen von Protokollen zur Datenanalyse](#search-logs-to-analyze-data).

Auf der Registerkarte **Knoten** folgt die Zeilenhierarchie dem Kubernetes-Objektmodell, das mit einem Knoten in Ihrem Cluster beginnt. Erweitern Sie den Knoten, und Sie können mindestens einen Pod ansehen, der auf dem Knoten ausgeführt wird. Wenn mehrere Container zu einem Pod zusammengefasst sind, werden sie als letzte Zeile in der Hierarchie angezeigt. Sie können auch anzeigen, wie viele nicht auf Pods bezogene Workloads auf dem Host ausgeführt werden, falls Prozessor oder Arbeitsspeicher des Hosts überlastet sind.

![Beispiel für eine Kubernetes-Knotenhierarchie in der Leistungsansicht](./media/monitoring-container-health/container-health-nodes-view.png)

Im oberen Bereich der Seite können Sie Controller oder Container auswählen und den Status sowie die Ressourcenauslastung für diese Objekte überprüfen. Mit den Dropdownfeldern oben können Sie nach Namespace, Dienst und Knoten filtern. Wenn Sie stattdessen die Arbeitsspeicherauslastung überprüfen möchten, wählen Sie aus der Dropdownliste **Metrik** die Option **Arbeitsspeicher RSS** oder **Arbeitssatz für Arbeitsspeicher** aus. **Arbeitsspeicher RSS** wird nur für die Kubernetes-Version 1.8 und höher unterstützt. Andernfalls werden Werte für **Min&nbsp;%** als *NaN&nbsp;%* angezeigt. Dieser numerische Datentypwert stellt einen nicht definierten oder nicht darstellbaren Wert dar. 

![Leistungsansicht zu den Containerknoten](./media/monitoring-container-health/container-health-node-metric-dropdown.png)

Standardmäßig beziehen sich Leistungsdaten auf die letzten sechs Stunden. Sie können das Fenster jedoch oben rechts über die Dropdownliste **Zeitbereich** ändern. Gegenwärtig wird die Seite nicht automatisch aktualisiert, sodass sie sie manuell aktualisieren müssen. Außerdem haben Sie die Möglichkeit, die Ergebnisse im Zeitbereich durch Auswahl von **Mittelw.**, **Min.**, **Max.**, **50.**, **90.** und **95.** im Perzentilselektor zu filtern. 

![Perzentilauswahl für die Datenfilterung](./media/monitoring-container-health/container-health-metric-percentile-filter.png)

Beachten Sie im folgenden Beispiel, dass für Knoten *aks-nodepool-3977305* der Wert für **Container** 5 lautet. Hierbei handelt es sich um ein Rollup der Gesamtzahl der bereitgestellten Container.

![Beispiel für Rollup der Container pro Knoten](./media/monitoring-container-health/container-health-nodes-containerstotal.png)

So können Sie schnell feststellen, ob Sie das richtige Verhältnis zwischen Containern und Knoten im Cluster haben. 

Die folgende Tabelle beschreibt die Informationen, die bei der Anzeige von Knoten erscheinen:

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Hosts. |
| Status | Kubernetes-Ansicht des Knotenstatus. |
| Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Durchschnittlicher Prozentsatz von Knoten basierend auf dem Perzentil für die ausgewählte Dauer. |
| Mittelw., Min., Max., 50., 90. | Tatsächlicher Durchschnittswert der Knoten basierend auf dem Perzentil für den ausgewählten Zeitraum. Der Mittelwert wird anhand des festgelegten CPU-/Arbeitsspeichergrenzwerts für einen Knoten gemessen. Bei Pods und Containern ist dies der vom Host gemeldete Mittelwert. |
| Container | Anzahl von Containern |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start oder Neustart eines Knotens verstrichen ist. |
| Controllers | Nur für Container und Pods. Diese Spalte zeigt an, welcher Controller enthalten ist. Nicht alle Pods befinden sind in einem Controller, sodass einige Spalten **N/V** anzeigen. | 
| Trend Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Balkendiagrammtrend, der die Perzentilmetrik des Controllers in Prozent anzeigt. |


Wählen Sie im Selektor **Controller** aus.

![Auswählen der Controlleransicht](./media/monitoring-container-health/container-health-controllers-tab.png)

In dieser Ansicht können Sie die Leistungsintegrität Ihrer Controller ansehen.

![Leistungsansicht des Controllers <Name>](./media/monitoring-container-health/container-health-controllers-view.png)

Die Zeilenhierarchie beginnt mit einem Controller und erweitert den Controller. Sie sehen mindestens einen Container. Wenn Sie einen Pod erweitern, wird in der letzten Zeile der Container angezeigt, der im Pod gruppiert ist. 

Die folgende Tabelle beschreibt die Informationen, die bei der Anzeige von Controllern erscheinen:

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Controllers.|
| Status | Rollupstatus der Container, wenn deren Ausführung mit einem Status abgeschlossen wurde, z. B. *OK*, *Beendet*, *Fehler*, *Gestoppt* oder *Angehalten*. Wenn der Container ausgeführt wird, der Status jedoch entweder nicht richtig angezeigt oder nicht vom Agent übernommen wurde und seit über 30 Minuten nicht antwortet, ist der Status *Unbekannt*. Zusätzliche Details zu dem Symbol „Status“ werden in der folgenden Tabelle angegeben.|
| Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Durchschnittliches Rollup des durchschnittlichen Prozentsatzes jeder Entität für die ausgewählte Metrik und das ausgewählte Perzentil. |
| Mittelw., Min., Max., 50., 90.  | Rollup der durchschnittlichen CPU-Millicore oder Speicherleistung des Containers für das ausgewählte Perzentil. Der Mittelwert wird ausgehend vom festgelegten CPU-/Speichergrenzwert für einen Pod gemessen. |
| Container | Gesamtanzahl der Container für den Controller oder Pod. |
| Neustarts | Rollup der Anzahl von Containerneustarts. |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start eines Containers verstrichen ist. |
| Knoten | Nur für Container und Pods. Diese Spalte zeigt an, welcher Controller enthalten ist. | 
| Trend Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;%| Balkendiagrammtrend, der die Perzentilmetrik des Controllers anzeigt. |

Die Symbole im Statusfeld zeigen den Onlinestatus von Containern an:
 
| Symbol | Status | 
|--------|-------------|
| ![Statussymbol für bereit und ausgeführt](./media/monitoring-container-health/container-health-ready-icon.png) | Wird ausgeführt (bereit)|
| ![Statussymbol für wartend oder angehalten](./media/monitoring-container-health/container-health-waiting-icon.png) | Wartend oder angehalten|
| ![Symbol für zuletzt gemeldeten Ausführungsstatus](./media/monitoring-container-health/container-health-grey-icon.png) | Zuletzt als ausgeführt gemeldet, hat aber seit mehr als 30 Minuten nicht geantwortet|
| ![Statussymbol für erfolgreiches Anhalten/Stoppen](./media/monitoring-container-health/container-health-green-icon.png) | Erfolgreich beendet oder Fehler beim Beenden|

Das Statussymbol zeigt basierend auf dem, was der Pod bereitstellt, eine Anzahl an. Es werden die beiden schlechtesten Status angezeigt. Wenn Sie den Mauszeiger über den Status bewegen, wird ein Rollupstatus aller Pods im Container angezeigt. Wenn der Status „Bereit“ nicht vorliegt, wird der Statuswert **(0)** angezeigt. 

Wählen Sie im Selektor **Container** aus.

![Auswählen der Containeransicht](./media/monitoring-container-health/container-health-containers-tab.png)

In dieser Ansicht können Sie die Leistungsintegrität Ihrer Container ansehen.

![Leistungsansicht des Controllers <Name>](./media/monitoring-container-health/container-health-containers-view.png)

Die folgende Tabelle beschreibt die Informationen, die bei der Anzeige von Containern erscheinen:

| Column | BESCHREIBUNG | 
|--------|-------------|
| NAME | Der Name des Controllers.|
| Status | Status der Container, sofern vorhanden. Zusätzliche Informationen zum Statussymbol finden Sie in der folgenden Tabelle.|
| Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Rollup des durchschnittlichen Prozentsatzes jeder Entität für die ausgewählte Metrik und das ausgewählte Perzentil. |
| Mittelw., Min., Max., 50., 90.  | Rollup der durchschnittlichen CPU-Millicore oder Speicherleistung des Containers für das ausgewählte Perzentil. Der Mittelwert wird ausgehend vom festgelegten CPU-/Speichergrenzwert für einen Pod gemessen. |
| Pod | Container, in dem sich der Pod befindet.| 
| Knoten |  Der Knoten, in dem sich der Container befindet. | 
| Neustarts | Stellt den Zeitraum dar, der seit dem Start eines Containers verstrichen ist. |
| Betriebszeit | Stellt den Zeitraum dar, der seit dem Start oder Neustart eines Containers verstrichen ist. |
| Trend Mittelw.&nbsp;%, Min.&nbsp;%, Max.&nbsp;%, 50.&nbsp;%, 90.&nbsp;% | Balkendiagrammtrend, der die durchschnittliche Metrik des Containers in Prozent anzeigt. |

Die Symbole im Statusfeld zeigen die Onlinestatus der Pods an, wie in der folgenden Tabelle beschrieben:
 
| Symbol | Status | 
|--------|-------------|
| ![Statussymbol für bereit und ausgeführt](./media/monitoring-container-health/container-health-ready-icon.png) | Wird ausgeführt (bereit)|
| ![Statussymbol für wartend oder angehalten](./media/monitoring-container-health/container-health-waiting-icon.png) | Wartend oder angehalten|
| ![Symbol für zuletzt gemeldeten Ausführungsstatus](./media/monitoring-container-health/container-health-grey-icon.png) | Zuletzt als ausgeführt gemeldet, hat aber seit mehr als 30 Minuten nicht geantwortet|
| ![Statussymbol für „Beendet“](./media/monitoring-container-health/container-health-terminated-icon.png) | Erfolgreich beendet oder Fehler beim Beenden|
| ![Symbol für den Status „Fehler“](./media/monitoring-container-health/container-health-failed-icon.png) | Status „Fehler“ |

## <a name="container-data-collection-details"></a>Details zur Datensammlung in Containern
Die Containerintegrität sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern. Alle drei Minuten werden Daten gesammelt.

### <a name="container-records"></a>Containerdatensätze

Die folgende Tabelle zeigt Beispiele für Datensätze, die von der Containerintegrität gesammelt werden, und Datentypen, die in Protokollsuchergebnissen angezeigt werden:

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
Mit Log Analytics können Sie nach Trends suchen, Engpässe diagnostizieren, Prognosen erstellen oder Daten korrelieren, die Ihnen die Einschätzung ermöglichen, ob die aktuelle Clusterkonfiguration optimal funktioniert. Ihnen werden vordefinierte Protokollsuchen für die sofortige Verwendung bereitgestellt. Alternativ können Sie diese so anpassen, dass die Informationen auf die gewünschte Weise zurückgegeben werden. 

Im Arbeitsbereich können Sie interaktive Datenanalysen durchführen, indem Sie im Vorschaubereich die Option **Kubernetes-Ereignisprotokolle anzeigen** oder **Containerprotokolle anzeigen** auswählen. Die Seite **Protokollsuche** wird rechts von der Azure-Portalseite angezeigt, die Sie besucht haben.

![Analysieren von Daten in Log Analytics](./media/monitoring-container-health/container-health-log-search-example.png)   

Die Containerprotokolle, die an Log Analytics weitergeleitet wurden, geben STDOUT und STDERR aus. Da die Containerintegrität Azure Kubernetes Service (AKS) überwacht, wird „kube-system“ aufgrund der umfangreichen generierten Datenmenge zurzeit nicht gesammelt. 

### <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche
Es ist oft hilfreich, die Abfrageerstellung ausgehend von einem oder zwei Beispielen zu beginnen und diese dann den Anforderungen entsprechend anzupassen. Sie können mit den folgenden Beispielabfragen experimentieren, um komplexere Abfragen zu erstellen:

| Abfragen | BESCHREIBUNG | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Liste mit Lebenszyklusinformationen aller Container| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Kubernetes-Ereignisse|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Imagebestand | 
| **Wählen Sie in Advanced Analytics Liniendiagramme aus**:<br> Perf<br> &#124; where ObjectName == "Container" and CounterName == "% Processor Time"<br> &#124; summarize AvgCPUPercent = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Container-CPU | 
| **Wählen Sie in Advanced Analytics Liniendiagramme aus**:<br> Perf &#124; where ObjectName == "Container" and CounterName == "Memory Usage MB"<br> &#124; summarize AvgUsedMemory = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Containerspeicher |

## <a name="how-to-stop-monitoring-with-container-health"></a>Gewusst wie: Anhalten der Überwachung mit der Containerintegrität
Wenn Sie einen AKS-Container mit aktivierter Überwachung nicht mehr überwachen möchten, können Sie die Option über die bereitgestellten Azure Resource Manager-Vorlagen, den PowerShell-Cmdlet **New-AzureRmResourceGroupDeployment** oder die Azure CLI *deaktivieren*. Eine JSON-Vorlage *deaktiviert* die Konfiguration. Die andere enthält Parameterwerte, die Sie konfigurieren, um die Ressourcen-ID des AKS-Clusters und die Ressourcengruppe anzugeben, in der der Cluster bereitgestellt wird. 

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Speichern Sie diese Datei als **OptOutTemplate.json** in einem lokalen Ordner.
3. Fügen Sie die folgende JSON-Syntax in Ihre Datei ein:

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

4. Aktualisieren Sie den Wert für **aksResourceId** und **aksResourceLocation** mit den Werten des AKS-Clusters, die auf der Seite **Eigenschaften** für den ausgewählten Cluster angegeben werden.

    ![Seite mit den Containereigenschaften](./media/monitoring-container-health/container-properties-page.png)

    Kopieren Sie auf der Seite **Eigenschaften** außerdem die **Arbeitsbereichsressourcen-ID**. Sie benötigen diesen Wert, falls Sie den Log Analytics-Arbeitsbereich später löschen möchten. Das Löschen des Log Analytics-Arbeitsbereichs wird nicht im Rahmen dieses Prozesses ausgeführt. 

5. Speichern Sie diese Datei als **OptOutParam.json** in einem lokalen Ordner.
6. Nun können Sie die Vorlage bereitstellen. 

    * So werden die folgenden PowerShell-Befehle im Ordner mit der Vorlage verwendet:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das zurückgegebene Ergebnis anzeigt:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * So führen Sie folgenden Befehl mit der Azure CLI unter Linux aus

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das zurückgegebene Ergebnis anzeigt:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Wenn der Arbeitsbereich nur als Unterstützung für die Clusterüberwachung erstellt wurde und nicht mehr benötigt wird, müssen Sie ihn manuell löschen. Wie ein Arbeitsbereich gelöscht wird, erfahren Sie unter [Löschen eines Log Analytics-Arbeitsbereichs mit dem Azure-Portal](../log-analytics/log-analytics-manage-del-workspace.md). Denken Sie an die **Arbeitsbereichsressourcen-ID**, die Sie weiter oben in Schritt 4 kopiert haben. Diese werden Sie später benötigen. 

## <a name="troubleshooting"></a>Problembehandlung
Dieser Abschnitt enthält Informationen zum Durchführen der Problembehandlung mit der Containerintegrität.

Wenn die Containerintegrität erfolgreich aktiviert und konfiguriert wurde, beim Ausführen einer Protokollsuche jedoch keine Statusinformationen oder -ergebnisse in Log Analytics angezeigt werden, können Sie das Problem folgendermaßen diagnostizieren: 

1. Überprüfen Sie den Zustand des Agent, indem Sie den folgenden Befehl ausführen: 

    `kubectl get ds omsagent --namespace=kube-system`

    Die Ausgabe sollte wie folgt aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Überprüfen Sie den Status der Lösungsbereitstellung mit Agent-Version *06072018* oder höher, indem Sie den folgenden Befehl ausführen:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Die Ausgabe sollte wie folgt aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Überprüfen Sie den Podstatus mit dem folgenden Befehl, um festzustellen, ob der Pod ausgeführt wird: `kubectl get pods --namespace=kube-system`.

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

4. Überprüfen Sie die Agent-Protokolle. Beim Bereitstellen des Container-Agents wird durch OMI-Befehle eine Schnellüberprüfung ausgeführt und die Agent- sowie Anbieterversion angezeigt. 

5. Um zu überprüfen, ob der Agent erfolgreich eingebunden ist, führen Sie den folgenden Befehl aus: `kubectl logs omsagent-484hw --namespace=kube-system`.

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

Weitere Informationen zur detaillierten Containerintegrität und Anwendungsleistung finden Sie im Artikel zu [Protokollsuchvorgängen](../log-analytics/log-analytics-log-search.md). 

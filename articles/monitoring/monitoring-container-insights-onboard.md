---
title: Onboardingmethoden für Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie das Onboarding und die Konfiguration von Azure Monitor für Container durchführen, damit Sie erfahren können, wie gut die Leistung Ihrer Container ist und welche leistungsbezogenen Probleme erkannt wurden.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 6b1de03814017ca4eb811362b2aaf5ac24f5ea86
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46999799"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Onboardingmethoden für Azure Monitor für Container
In diesem Artikel wird beschrieben, wie Azure Monitor für Container eingerichtet und verwendet wird, um die Leistung von Workloads zu überwachen, die in Kubernetes-Umgebungen bereitgestellt und von [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) gehostet werden.

## <a name="prerequisites"></a>Voraussetzungen 
Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

- Ein neuer oder vorhandener AKS-Cluster.
- Ein containerbasierter Log Analytics-Agent für Linux-Version microsoft/oms:ciprod04202018 und höher. Die Versionsnummer wird durch eine Datumsangabe im folgenden Format dargestellt: *mmttjjjj*. Der Agent wird automatisch beim Onboardingvorgang dieses Features installiert. 
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

## <a name="enable-monitoring-for-a-new-cluster"></a>Aktivieren der Überwachung für einen neuen Cluster
Sie können die Überwachung eines neuen AKS-Clusters bei der Bereitstellung im Azure-Portal oder über die Azure-Befehlszeilenschnittstelle aktivieren. Wenn Sie ihn über das Portal aktivieren möchten, führen Sie die Schritte im Schnellstartartikel [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](../aks/kubernetes-walkthrough-portal.md) durch. Legen Sie auf der Seite **Überwachung** für die Option **Überwachung aktivieren** **Ja** fest, und wählen Sie dann einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen neuen. 

Um die Überwachung eines mit der Azure-Befehlszeilenschnittstelle erstellten neuen AKS-Clusters zu aktivieren, führen Sie die Schritte im Schnellstartartikel im Abschnitt [Erstellen eines AKS-Clusters](../aks/kubernetes-walkthrough.md#create-aks-cluster) durch.  

>[!NOTE]
>Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.43 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

Nachdem Sie die Überwachung aktiviert haben, und alle Konfigurationsaufgaben erfolgreich abgeschlossen wurden, haben Sie zwei Möglichkeiten, die Leistung Ihres Clusters zu überwachen:

* Direkt über das AKS-Cluster durch Auswählen von **Integrität** im linken Bereich.
* Durch Auswählen der Kachel **Monitor Container insights** (Containereinblicke überwachen) auf der AKS-Clusterseite für den ausgewählten Cluster. Wählen Sie in Azure Monitor im linken Bereich **Integrität** aus. 

  ![Optionen zum Auswählen von Azure Monitor für Container in AKS](./media/monitoring-container-insights-onboard/kubernetes-select-monitoring-01.png)

Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Aktivieren der Überwachung für vorhandene verwaltete Cluster
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

### <a name="enable-monitoring-from-azure-monitor"></a>Aktivieren der Überwachung aus Azure Monitor
Führen Sie die folgenden Schritte aus, um die Überwachung Ihres AKS-Clusters im Azure-Portal aus Azure Monitor zu aktivieren:

1. Wählen Sie im Azure-Portal die Option **Überwachen** aus. 
2. Wählen Sie in der Liste **Container (Vorschau)** aus.
3. Wählen Sie auf der Seite **Überwachung – Container (Vorschau)** **Nicht überwachte Cluster** aus.
4. Suchen Sie in der Liste der nicht überwachten Cluster den Container, und klicken Sie auf **Aktivieren**.   
5. Wenn Sie im selben Abonnement wie der Cluster über einen Log Analytics-Arbeitsbereich verfügen, wählen Sie ihn auf der Seite **Onboarding zu Containerintegrität und Protokollen** aus der Dropdownliste aus.  
    Die Liste wählt Standardarbeitsbereich und Speicherort vorab aus, in dem der AKS-Container im Abonnement bereitgestellt wird. 

    ![Aktivieren der AKS-Containereinblicküberwachung](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus dem Cluster erstellen möchten, befolgen Sie die Anweisungen in [Erstellen eines Log Analytics-Arbeitsbereichs](../log-analytics/log-analytics-quick-create-workspace.md). Achten Sie darauf, dass Sie den Arbeitsbereich in demselben Abonnement erstellen, in dem der AKS-Container bereitgestellt wird. 
 
Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>Aktivieren der Überwachung von einem AKS-Cluster im Portal aus
Führen Sie die folgenden Schritte aus, um die Überwachung des AKS-Containers über das Azure-Portal zu aktivieren:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. 
2. Geben Sie in die Ressourcenliste **Containers** ein.  
    Die Liste wird anhand Ihrer Eingabe gefiltert. 
3. Wählen Sie **Kubernetes-Dienste** aus.  

    ![Link zu Kubernetes-Diensten](./media/monitoring-container-insights-onboard/portal-search-containers-01.png)

4. Wählen Sie in der Containerliste einen Container aus.
5. Wählen Sie auf der Containerübersichtsseite **Containerintegrität überwachen** aus.  
6. Wenn Sie im selben Abonnement wie der Cluster einen Log Analytics-Arbeitsbereich haben, wählen Sie ihn auf der Seite **Onboarding in Containerintegrität und Protokolle** aus der Dropdownliste aus.  
    Die Liste wählt Standardarbeitsbereich und Speicherort vorab aus, in dem der AKS-Container im Abonnement bereitgestellt wird. 

    ![Aktivieren der AKS-Containerintegritätsüberwachung](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-02.png)

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
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
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
                        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]",
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
           "value": "/subscriptions/<SubscriptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. Aktualisieren Sie die Werte für **aksResourceId** und **aksResourceLocation** mit den Werten, die auf der Seite **AKS – Übersicht** für den AKS-Cluster angegeben werden. Der Wert für **workspaceResourceId** ist die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs, darunter der Name des Arbeitsbereichs. Geben Sie den Speicherort des Arbeitsbereichs auch für **workspaceRegion** an. 
5. Speichern Sie diese Datei unter den Namen **existingClusterParam.json** in einem lokalen Ordner.
6. Nun können Sie die Vorlage bereitstellen. 

    * Verwenden Sie die folgenden PowerShell-Befehle in dem Ordner mit der Vorlage:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ClusterResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Führen Sie den folgenden Befehl mithilfe der Azure CLI aus:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

        ```azurecli
        provisioningState       : Succeeded
        ```
Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden. 

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
## <a name="next-steps"></a>Nächste Schritte

* Wenn beim Onboarding der Lösung Probleme auftreten, lesen Sie den [Leitfaden zur Problembehandlung](monitoring-container-insights-troubleshoot.md)

* Bei aktivierter Überwachung zum Erfassen der Integritätsmetriken für AKS-Clusterknoten wie auch für Pods stehen diese Integritätsmetriken im Azure-Portal zur Verfügung. Informationen zum Erlernen der Verwendung von Azure Monitor für Container finden Sie unter [Anzeigen der Integrität von Azure Kubernetes Service](monitoring-container-insights-analyze.md).
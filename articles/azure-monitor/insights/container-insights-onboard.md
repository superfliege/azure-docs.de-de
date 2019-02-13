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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2018
ms.author: magoedte
ms.openlocfilehash: 13da9e0d731e87b6cdd5830c9295847511c301ef
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567297"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Onboardingmethoden für Azure Monitor für Container  
In diesem Artikel wird beschrieben, wie Azure Monitor für Container eingerichtet und verwendet wird, um die Leistung von Workloads zu überwachen, die in Kubernetes-Umgebungen bereitgestellt und von [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) gehostet werden.

Azure Monitor für Container kann für neue oder mindestens eine vorhandene Bereitstellung von AKS mit den folgenden unterstützten Methoden aktiviert werden:

* Über das Azure-Portal oder mit der Azure-Befehlszeilenschnittstelle
* Verwenden von [Terraform und AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

## <a name="prerequisites"></a>Voraussetzungen 
Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

- Einen Log Analytics-Arbeitsbereich Sie können ihn beim Aktivieren der Überwachung des neuen AKS-Clusters erstellen oder beim Onboarding einen Standardarbeitsbereich in der Standardressourcengruppe des AKS-Clusterabonnements erstellen lassen. Wenn Sie ihn selbst erstellen möchten, können Sie dies über den [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md) erledigen.
- Sie müssen der Rolle „Log Analytics-Mitwirkender“ angehören, um die Containerüberwachung aktivieren zu können. Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../../azure-monitor/platform/manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Komponenten 

Ihre Möglichkeit zum Überwachen der Leistung hängt von einem containerbasierten Log Analytics-Agent für Linux ab, der speziell für Azure Monitor für Container entwickelt wurde. Dieser spezialisierte Agent sammelt Leistungs- und Ereignisdaten von allen Knoten im Cluster, und der Agent wird während der Bereitstellung automatisch bereitgestellt und mit dem angegebenen Log Analytics-Arbeitsbereich registriert. Dabei wird die Agent-Version „microsoft/oms:ciprod04202018“ oder eine höhere Version bereitgestellt (dargestellt im Format *mmttjjjj*). 

Wenn eine neue Version des Agents veröffentlicht wird, wird er in Ihren Managed Kubernetes-Clustern, die unter Azure Kubernetes Service (AKS) gehostet werden, automatisch aktualisiert. Informationen zu den freigegebenen Versionen finden Sie unter [Agent-Versionsankündigungen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). 

>[!NOTE] 
>Wenn Sie bereits einen AKS-Cluster bereitgestellt haben, können Sie die Überwachung mithilfe der Azure-Befehlszeilenschnittstelle oder einer bereitgestellten Azure Resource Manager-Vorlage aktivieren (siehe weiter unten in diesem Artikel). Sie können mit `kubectl` kein Upgrade für den Agent durchführen oder diesen löschen, bereitstellen oder erneut bereitstellen. Die Vorlage muss in derselben Ressourcengruppe wie der Cluster bereitgestellt werden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

## <a name="enable-monitoring-for-a-new-cluster"></a>Aktivieren der Überwachung für einen neuen Cluster
Sie können die Überwachung eines neuen AKS-Clusters im Rahmen der Bereitstellung über das Azure-Portal, mit der Azure-Befehlszeilenschnittstelle oder mit Terraform aktivieren.  Wenn Sie ihn über das Portal aktivieren möchten, führen Sie die Schritte im Schnellstartartikel [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](../../aks/kubernetes-walkthrough-portal.md) durch. Legen Sie auf der Seite **Überwachung** für die Option **Überwachung aktivieren** **Ja** fest, und wählen Sie dann einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen neuen. 

### <a name="enable-using-azure-cli"></a>Aktivieren mithilfe der Azure-Befehlszeilenschnittstelle
Um die Überwachung eines mit der Azure-Befehlszeilenschnittstelle erstellten neuen AKS-Clusters zu aktivieren, führen Sie die Schritte im Schnellstartartikel im Abschnitt [Erstellen eines AKS-Clusters](../../aks/kubernetes-walkthrough.md#create-aks-cluster) durch.  

>[!NOTE]
>Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.43 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

### <a name="enable-using-terraform"></a>Aktivieren mithilfe von Terraform
Wenn Sie [einen neuen AKS-Cluster mit Terraform](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md) bereitstellen, geben Sie die Argumente an, die im Profil [zum Erstellen eines Log Analytics-Arbeitsbereichs](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) erforderlich sind, wenn Sie kein vorhandenes Argument angeben. 

>[!NOTE]
>Wenn Sie sich für Terraform entscheiden, müssen Sie den Terraform Azure RM-Anbieter, Version 1.17.0 oder höher, verwenden.

Informationen zum Hinzufügen von Azure Monitor für Container zum Arbeitsbereich finden Sie unter [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html), und vervollständigen Sie das Profil, indem Sie [**addon_profile**](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) einbeziehen und **oms_agent** angeben. 

Nachdem Sie die Überwachung aktiviert haben, und alle Konfigurationsaufgaben erfolgreich abgeschlossen wurden, haben Sie zwei Möglichkeiten, die Leistung Ihres Clusters zu überwachen:

* Direkt über das AKS-Cluster durch Auswählen von **Integrität** im linken Bereich.
* Durch Auswählen der Kachel **Monitor Container insights** (Containereinblicke überwachen) auf der AKS-Clusterseite für den ausgewählten Cluster. Wählen Sie in Azure Monitor im linken Bereich **Integrität** aus. 

  ![Optionen zum Auswählen von Azure Monitor für Container in AKS](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Aktivieren der Überwachung für vorhandene verwaltete Cluster
Sie können die Überwachung eines bereits bereitgestellten AKS-Clusters entweder über die Azure-Befehlszeilenschnittstelle, das Portal oder die bereitgestellte Azure Resource Manager-Vorlage mit dem PowerShell-Cmdlet `New-AzureRmResourceGroupDeployment` aktivieren. 

### <a name="enable-monitoring-using-azure-cli"></a>Aktivieren der Überwachung mithilfe der Azure-Befehlszeilenschnittstelle
Der folgende Schritt aktiviert die Überwachung Ihres AKS-Clusters mithilfe der Azure-Befehlszeilenschnittstelle. In diesem Beispiel müssen Sie keinen Arbeitsbereich erstellen oder einen vorhandenen angeben. Dieser Befehl vereinfacht den Prozess durch Erstellen eines Standardarbeitsbereichs in der Standardressourcengruppe des AKS-Cluster-Abonnements, wenn noch keiner in der Region vorhanden ist.  Das Format des Standardarbeitsbereichs ähnelt *DefaultWorkspace-\<GUID>-\<Region>*.  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

Die Ausgabe ähnelt der folgenden:

```azurecli
provisioningState       : Succeeded
```

Wenn Sie die Integration in einen vorhandenen Arbeitsbereich vorziehen, geben Sie mit dem folgenden Befehl den entsprechenden Arbeitsbereich an:

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG --workspace-resource-id <ExistingWorkspaceResourceID> 
```

Die Ausgabe ähnelt der folgenden:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-using-terraform"></a>Aktivieren der Überwachung mithilfe von Terraform
1. Fügen Sie das Add-On-Profil **oms_agent** zur vorhandenen [azurerm_kubernetes_cluster resource](https://www.terraform.io/docs/providers/azurerm/d/kubernetes_cluster.html#addon_profile) hinzu.

   ```
   addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
     }
   }
   ```

2. Fügen Sie die [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) hinzu, wie in der Terraform-Dokumentation beschrieben.

### <a name="enable-monitoring-from-azure-monitor"></a>Aktivieren der Überwachung aus Azure Monitor
Führen Sie die folgenden Schritte aus, um die Überwachung Ihres AKS-Clusters im Azure-Portal aus Azure Monitor zu aktivieren:

1. Wählen Sie im Azure-Portal die Option **Überwachen** aus. 
2. Wählen Sie in der Liste **Container** aus.
3. Wählen Sie auf der Seite **Überwachung – Container** die Option **Nicht überwachte Cluster** aus.
4. Suchen Sie in der Liste der nicht überwachten Cluster den Container, und klicken Sie auf **Aktivieren**.   
5. Wenn Sie im selben Abonnement wie der Cluster über einen Log Analytics-Arbeitsbereich verfügen, wählen Sie ihn auf der Seite **Onboarding zu Azure Monitor für Container** aus der Dropdownliste aus.  
    Die Liste wählt Standardarbeitsbereich und Speicherort vorab aus, in dem der AKS-Container im Abonnement bereitgestellt wird. 

    ![Aktivieren der AKS-Containereinblicküberwachung](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus dem Cluster erstellen möchten, befolgen Sie die Anweisungen in [Erstellen eines Log Analytics-Arbeitsbereichs](../../azure-monitor/learn/quick-create-workspace.md). Achten Sie darauf, dass Sie den Arbeitsbereich in demselben Abonnement erstellen, in dem der AKS-Container bereitgestellt wird. 
 
Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>Aktivieren der Überwachung von einem AKS-Cluster im Portal aus
Führen Sie die folgenden Schritte aus, um die Überwachung des AKS-Containers über das Azure-Portal zu aktivieren:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. 
2. Geben Sie in die Ressourcenliste **Containers** ein.  
    Die Liste wird anhand Ihrer Eingabe gefiltert. 
3. Wählen Sie **Kubernetes-Dienste** aus.  

    ![Link zu Kubernetes-Diensten](./media/container-insights-onboard/portal-search-containers-01.png)

4. Wählen Sie in der Containerliste einen Container aus.
5. Wählen Sie auf der Containerübersichtsseite **Container überwachen** aus.  
6. Wenn Sie im selben Abonnement wie der Cluster über einen Log Analytics-Arbeitsbereich verfügen, wählen Sie ihn auf der Seite **Onboarding zu Azure Monitor für Container** aus der Dropdownliste aus.  
    Die Liste wählt Standardarbeitsbereich und Speicherort vorab aus, in dem der AKS-Container im Abonnement bereitgestellt wird. 

    ![Aktivieren der AKS-Containerintegritätsüberwachung](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus dem Cluster erstellen möchten, befolgen Sie die Anweisungen in [Erstellen eines Log Analytics-Arbeitsbereichs](../../azure-monitor/learn/quick-create-workspace.md). Achten Sie darauf, dass Sie den Arbeitsbereich in demselben Abonnement erstellen, in dem der AKS-Container bereitgestellt wird. 
 
Nachdem Sie die Überwachung aktiviert haben, kann es ca. 15 Minuten dauern, bis Sie die operativen Daten für den Cluster ansehen können. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Aktivieren der Überwachung mit einer Azure Resource Manager-Vorlage
Diese Methode umfasst zwei JSON-Vorlagen. Eine Vorlage gibt die Konfiguration zur Aktivierung der Überwachung an, während die andere Vorlage die zu konfigurierenden Parameterwerte enthält, mit denen Folgendes festgelegt wird:

* Ressourcen-ID des AKS-Containers 
* Ressourcengruppe, in der der Cluster bereitgestellt wird
* Log Analytics-Arbeitsbereich und -Region, in der der Arbeitsbereich erstellt wird 

>[!NOTE]
>Die Vorlage muss in derselben Ressourcengruppe wie der Cluster bereitgestellt werden.
>

Der Log Analytics-Arbeitsbereich muss manuell erstellt werden. Arbeitsbereiche können über den [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md) eingerichtet werden.

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

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
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
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
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
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

* Wenn beim Onboarding der Lösung Probleme auftreten, lesen Sie den [Leitfaden zur Problembehandlung](container-insights-troubleshoot.md)

* Bei aktivierter Überwachung zum Erfassen der Integritätsmetriken für AKS-Clusterknoten wie auch für Pods stehen diese Integritätsmetriken im Azure-Portal zur Verfügung. Informationen zum Erlernen der Verwendung von Azure Monitor für Container finden Sie unter [Anzeigen der Integrität von Azure Kubernetes Service](container-insights-analyze.md).

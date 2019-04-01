---
title: Problembehandlung für Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Probleme mit Azure Monitor für Container behandeln und lösen können.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2018
ms.author: magoedte
ms.openlocfilehash: abf833cc054bfac0581506f75259e357f0ab1b38
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985749"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Problembehandlung für Azure Monitor für Container

Wenn Sie die Überwachung Ihres AKS-Clusters (Azure Kubernetes Service) mit Azure Monitor für Container konfigurieren, treffen Sie möglicherweise auf ein Problem, das die Sammlung von Daten oder die Meldung des Status verhindert. In diesem Artikel werden einige allgemeine Probleme und Schritte zur Problembehandlung ausführlich erläutert.

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Azure Monitor für Container ist aktiviert, meldet aber keinerlei Informationen
Wenn Azure Monitor für Container erfolgreich aktiviert und konfiguriert wurde, Sie aber keine Statusinformationen anzeigen können oder eine Protokollabfrage keine Ergebnisse zurückgibt, diagnostizieren Sie das Problem, indem Sie die folgenden Schritte ausführen: 

1. Überprüfen Sie den Zustand des Agents, indem Sie den folgenden Befehl ausführen: 

    `kubectl get ds omsagent --namespace=kube-system`

    Die Ausgabe sollte wie folgt aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Überprüfen Sie den Bereitstellungsstatus in Agent-Version *06072018* oder höher mit dem Befehl:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Die Ausgabe sollte wie im folgenden Beispiel aussehen, was auf eine ordnungsgemäße Bereitstellung hinweist:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Überprüfen Sie den Status des Pods, um festzustellen, ob er ausgeführt wird, mit dem folgenden Befehl: `kubectl get pods --namespace=kube-system`

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

5. Um zu überprüfen, ob der Agent erfolgreich eingebunden ist, führen Sie diesen Befehl aus: `kubectl logs omsagent-484hw --namespace=kube-system`.

    Der Status sollte wie im folgenden Beispiel aussehen:

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

## <a name="error-messages"></a>Fehlermeldungen

In der unten stehenden Tabelle sind bekannte Fehler aufgeführt, die bei der Verwendung von Azure Monitor für Container auftreten können.

| Fehlermeldungen  | Aktion |  
| ---- | --- |  
| Fehlermeldung `No data for selected filters`  | Die Einrichtung der Überwachung des Datenflusses für neu erstellte Cluster kann einige Zeit in Anspruch nehmen. Es dauert mindestens 10 bis 15 Minuten, bis Daten für Ihren Cluster angezeigt werden. |   
| Fehlermeldung `Error retrieving data` | Während der Einrichtung eines Azure Kubernetes-Dienstclusters für die Überwachung von Integrität und Leistung wird zwischen dem Cluster und dem Azure Log Analytics-Arbeitsbereich eine Verbindung hergestellt. Zum Speichern sämtlicher Überwachungsdaten für Ihren Cluster wird ein Log Analytics-Arbeitsbereich verwendet. Dieser Fehler kann auftreten, wenn ein Log Analytics-Arbeitsbereich gelöscht wurde oder verloren gegangen ist. Überprüfen Sie anhand der Informationen unter [Verwalten des Zugriffs](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json#view-workspace-details), ob Ihr Arbeitsbereich verfügbar ist. Wenn der Arbeitsbereich fehlt, müssen Sie Ihren Cluster mit Azure Monitor für Container erneut integrieren. Zum erneuten Integrieren müssen Sie die Überwachung für den Cluster [deaktivieren](container-insights-optout.md) und dann mit Azure Monitor für Container [erneut aktivieren](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-for-a-new-cluster). |  
| `Error retrieving data` nach dem Hinzufügen von Azure Monitor für Container mithilfe von „az aks cli“ | Beim Integrieren mithilfe von `az aks cli` funktioniert der Prozess für Azure Monitor für Container in seltenen Fällen nicht ordnungsgemäß. Überprüfen Sie, ob die Lösung integriert ist. Wechseln Sie zu diesem Zweck zu Ihrem Log Analytics-Arbeitsbereich, und wählen Sie im linken Bereich **Lösungen** aus, um zu überprüfen, ob die Lösung verfügbar ist. Um dieses Problem zu lösen, müssen Sie die Lösung erneut bereitstellen. Befolgen Sie dazu die Anweisungen zum [Bereitstellen von Azure Monitor für Container](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json). |  

Um Sie bei der Diagnose zu unterstützen, haben wir [hier](https://github.com/Microsoft/OMS-docker/tree/ci_feature_prod/Troubleshoot#troubleshooting-script) ein Skript zur Problembehandlung bereitgestellt.  

## <a name="next-steps"></a>Nächste Schritte
Bei aktivierter Überwachung zum Erfassen der Integritätsmetriken für AKS-Clusterknoten wie auch für Pods stehen diese Integritätsmetriken im Azure-Portal zur Verfügung. Informationen zum Erlernen der Verwendung von Azure Monitor für Container finden Sie unter [Anzeigen der Integrität von Azure Kubernetes Service](container-insights-analyze.md).
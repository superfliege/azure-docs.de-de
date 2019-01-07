---
title: 'Gewusst wie: Durchführen eines Upgrades für den Agent für Azure Monitor für Container (Vorschauversion) | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie für den Log Analytics-Agent, der von Azure Monitor für Container verwendet wird, ein Upgrade durchführen.
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
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 08206b9de4fca3b422c5b076d7e0c1c180f82fbd
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184082"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Gewusst wie: Durchführen eines Upgrades für den Agent für Azure Monitor für Container (Vorschauversion)
Für Azure Monitor für Container wird eine Containerversion des Log Analytics-Agents für Linux verwendet. Wenn eine neue Version des Agents veröffentlicht wird, wird automatisch ein Upgrade des Agents in Ihren Managed Kubernetes-Clustern ausgeführt, die im Azure Kubernetes Service (AKS) gehostet werden.  

In diesem Artikel wird der Prozess für das manuelle Upgrade des Agent beschrieben, falls beim Agent-Upgrade ein Fehler auftritt. Informationen zu den freigegebenen Versionen finden Sie unter [Agent-Versionsankündigungen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Aktualisieren des Agents im überwachten Kubernetes-Cluster
Der Prozess für das Upgrade des Agents besteht aus zwei einfachen Schritten. Der erste Schritt umfasst das Deaktivieren der Überwachung mit Azure Monitor für Container per Azure CLI.  Führen Sie die Schritte aus, die im Artikel [Deaktivieren der Überwachung](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) beschrieben sind. Indem wir die Azure CLI verwenden, können wir den Agent von den Knoten im Cluster entfernen, ohne dass sich Auswirkungen auf die Lösung und die entsprechenden Daten ergeben, die im Arbeitsbereich gespeichert sind. 

>[!NOTE]
>Während der Durchführung dieser Wartungsaktivität leiten die Knoten im Cluster keine gesammelten Daten weiter, und die Leistungsansichten zeigen keine Daten für den Zeitraum an, in dem Sie den Agent entfernen und die neue Version installieren. 
>

Führen Sie zum Installieren der neuen Agent-Version die Schritte aus, die im Artikel zum [Onboarding für die Überwachung](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) mit der Azure CLI beschrieben sind, um diesen Vorgang abzuschließen.  

Nach dem erneuten Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis aktualisierte Integritätsmetriken für den Cluster angezeigt werden. Um zu überprüfen, ob das Agent-Upgrade erfolgreich war, führen Sie den folgenden Befehl aus: `kubectl logs omsagent-484hw --namespace=kube-system`

Der Status sollte dem folgenden Beispiel ähneln, wobei der Wert für *omi* und *omsagent* mit der neuesten Version übereinstimmen sollte, die im [Agent-Versionsverlauf](https://github.com/microsoft/docker-provider/tree/ci_feature_prod) angegeben ist.  

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
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="next-steps"></a>Nächste Schritte
Falls beim Aktualisieren des Agents Probleme auftreten, hilft Ihnen der [Leitfaden zur Problembehandlung](container-insights-troubleshoot.md) weiter.
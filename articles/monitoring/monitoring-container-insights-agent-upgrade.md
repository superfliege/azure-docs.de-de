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
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: fb45002d284bc28dfb7093f69cfac7aae0681e8d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628512"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Gewusst wie: Durchführen eines Upgrades für den Agent für Azure Monitor für Container (Vorschauversion)
Für Azure Monitor für Container wird eine Containerversion des Log Analytics-Agents für Linux verwendet. Wenn eine neue Version des Agents veröffentlicht wird, wird der Agent in Ihren verwalteten Kubernetes-Clustern, die unter Azure Kubernetes Service (AKS) gehostet werden, nicht automatisch aktualisiert.

In diesem Artikel wird der Prozess beschrieben, mit dem für den Agent ein Upgrade durchgeführt werden kann.

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Aktualisieren des Agents im überwachten Kubernetes-Cluster
Der Prozess für das Upgrade des Agents besteht aus zwei einfachen Schritten. Der erste Schritt umfasst das Deaktivieren der Überwachung mit Azure Monitor für Container per Azure CLI.  Führen Sie die Schritte aus, die im Artikel [Deaktivieren der Überwachung](monitoring-container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) beschrieben sind. Indem wir die Azure CLI verwenden, können wir den Agent von den Knoten im Cluster entfernen, ohne dass sich Auswirkungen auf die Lösung und die entsprechenden Daten ergeben, die im Arbeitsbereich gespeichert sind. 

>[!NOTE]
>Während der Durchführung dieser Wartungsaktivität leiten die Knoten im Cluster keine gesammelten Daten weiter, und die Leistungsansichten zeigen keine Daten für den Zeitraum an, in dem Sie den Agent entfernen und die neue Version installieren. 
>

Führen Sie zum Installieren der neuen Agent-Version die Schritte aus, die im Artikel zum [Onboarding für die Überwachung](monitoring-container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) mit der Azure CLI beschrieben sind, um diesen Vorgang abzuschließen.  

Nach dem erneuten Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis aktualisierte Integritätsmetriken für den Cluster angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte
Falls beim Aktualisieren des Agents Probleme auftreten, hilft Ihnen der [Leitfaden zur Problembehandlung](monitoring-container-insights-troubleshoot.md) weiter.
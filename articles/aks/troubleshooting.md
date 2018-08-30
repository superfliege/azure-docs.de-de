---
title: Problembehandlung bei allgemeinen Problemen mit Azure Kubernetes Service
description: Erfahren Sie, wie Sie allgemeine Probleme bei der Verwendung von Azure Kubernetes Service (AKS) beheben und lösen können
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 614930fb11e65404416e604c94351e2754b8e941
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190981"
---
# <a name="aks-troubleshooting"></a>AKS-Problembehandlung
Beim Erstellen oder Verwalten von AKS-Clustern können gelegentlich Probleme auftreten. In diesem Artikel werden einige allgemeine Probleme und Schritte zur Problembehandlung ausführlich erläutert.

### <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-issues"></a>Wo finde ich im Allgemeinen Informationen zum Debuggen von Problemen mit Kubernetes?

[Hier] (https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/) ist ein offizieller Link zur Problembehandlung von Kubernetes-Clustern.)
[Hier](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md) finden Sie einen Link zu einem Leitfaden zur Problembehandlung, das von einem Microsoft-Techniker veröffentlicht wurde und sich mit der Problembehandlung von Pods, Knoten, Clustern usw. befasst.

### <a name="i-am-getting-a-quota-exceeded-error-during-create-or-upgrade-what-should-i-do"></a>Während der Erstellung oder Durchführung eines Upgrades erhalte ich die Fehlermeldung, dass das Kontingent überschritten wurde. Wie sollte ich vorgehen? 

Sie müssen [hier](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) Kerne anfordern

### <a name="what-is-the-max-pods-per-node-setting-for-aks"></a>Wie lautet die maximale Anzahl von Pods pro Knoteneinstellung für AKS?

Die maximale Anzahl von Pods pro Knoten ist standardmäßig auf 30 festgelegt, wenn Sie einen AKS-Cluster im Azure-Portal bereitstellen.
Die maximale Anzahl von Pods pro Knoten ist standardmäßig auf 110 festgelegt, wenn Sie einen AKS-Cluster in der Azure CLI bereitstellen. (Stellen Sie sicher, dass Sie die neueste Version der Azure CLI verwenden.) Diese Standardeinstellung kann mithilfe von „–max-nodes-per-pod flag“ im Befehl „az aks create“ geändert werden.

### <a name="i-am-getting--insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Während der Bereitstellung eines AKS-Clusters mit erweitertem Netzwerk erhalte ich die Fehlermeldung „InsufficientSubnetSize“. Wie sollte ich vorgehen?

In der für das Netzwerk während Erstellungen des AKS ausgewählten Option „Benutzerdefiniertes VNET“ wird Azure CNI für IPAM verwendet. Die Anzahl der Knoten in einem AKS-Cluster kann einen beliebigen Wert zwischen 1 und 100 aufweisen. Basierend auf 2) sollte die Subnetzgröße größer als das Produkt der Anzahl von Knoten und der maximalen Anzahl von Pods pro Subnetzgröße eines Knotens sein > Anzahl von Knoten im Cluster * maximale Anzahl von Pods pro Knoten

### <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mein Pod ist im Modus „CrashLoopBackOff“ hängen geblieben. Wie sollte ich vorgehen?

Es kann verschiedene Gründe dafür geben, dass der Pod in diesem Modus hängen bleibt. Sie sollten Folgendes überprüfen 
* Den Pod selbst mithilfe von `kubectl describe pod <pod-name>`
* Die Protokolle mithilfe von `kubectl log <pod-name>`

### <a name="i-am-trying-to-enable-rbac-on-an-existing-cluster-can-you-tell-me-how-i-can-do-that"></a>Ich versuche, RBAC in einem vorhandenen Cluster zu aktivieren. Können Sie mir sagen, wie ich dafür vorgehen muss?

Eine Aktivierung von RBAC in vorhandenen Clustern wird derzeit leider nicht unterstützt. Sie müssen explizit neue Cluster erstellen. Wenn Sie die CLI verwenden, ist RBAC standardmäßig aktiviert. Eine Umschaltfläche zur RBAC-Aktivierung hingegen ist im Workflow zur Erstellung des AKS-Portals verfügbar.

### <a name="i-created-a-cluster-using-the-azure-cli-with-defaults-or-the-azure-portal-with-rbac-enabled-and-numerous-warnings-in-the-kubernetes-dashboard-the-dashboard-used-to-work-before-without-any-warnings-what-should-i-do"></a>Ich habe über die Azure CLI mit Standardwerten oder über das Azure-Portal mit aktiviertem RBAC einen Cluster erstellt und im Kubernetes-Dashboard werden zahlreiche Warnungen angezeigt. Das Dashboard hat vorher ohne Warnungen funktioniert. Wie sollte ich vorgehen?

Der Grund für die Warnungen auf dem Dashboard ist, dass dieses nun mit RBAC aktiviert wurde und der Zugriff darauf standardmäßig deaktiviert wurde. Dieser Ansatz gilt im Allgemeinen als bewährte Methode, da die standardmäßige Offenlegung des Dashboards für alle Benutzer des Clusters zu Sicherheitsrisiken führen kann. Wenn Sie das Dashboard weiterhin aktivieren möchten, lesen Sie diesen [Blogbeitrag](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

### <a name="i-cant-seem-to-connect-to-the-dashboard-what-should-i-do"></a>Offenbar kann ich keine Verbindung mit dem Dashboard herstellen. Wie sollte ich vorgehen?

Der einfachste Weg, um außerhalb des Clusters auf Ihren Dienst zuzugreifen, besteht in der Ausführung des kubectl-Proxys, der Proxyanforderungen für Ihren localhost-Port 8001 an den Kubernetes-API-Server sendet. Von dort aus kann der API-Server eine Proxyanforderung für Ihren Dienst ausführen: http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default

Wenn Ihnen das Kubernetes-Dashboard nicht angezeigt wird, überprüfen Sie, ob der Pod „kube-proxy“ im Namespace „kube-system“ ausgeführt wird. Wenn der Pod keinen Ausführungsstatus aufweist, müssen Sie diesen löschen. Er wird anschließend neu gestartet.

### <a name="i-could-not-get-logs-using-kubectl-logs-or-cannot-connect-to-the-api-server-getting-the-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Ich konnte über die Kubectl-Protokolle keine Protokolle abrufen bzw. ich kann keine Verbindung mit dem API-Server herstellen, ohne dass mir folgende Fehlermeldung angezeigt wird: „Error from server: error dialing backend: dial tcp…“ Wie sollte ich vorgehen?

Stellen Sie sicher, dass die Standard-NSG nicht geändert wurde und Port 22 für die Verbindung mit dem API-Server geöffnet ist. Überprüfen Sie, ob der Pod Tunnelfront im Namespace „kube-system“ ausgeführt wird. Ist dies nicht der Fall, erzwingen Sie eine Löschung. Anschließend erfolgt ein Neustart.

### <a name="i-am-trying-to-upgrade-or-scale-and-am-getting-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-issue"></a>Ich versuche, ein Upgrade oder eine Skalierung durchzuführen und erhalte folgende Nachricht: „Das Ändern der Eigenschaft "imageReference" ist unzulässig.“ Fehler.  Wie wird dieses Problem behoben?

Möglicherweise wird Ihnen diese Fehlermeldung angezeigt, da Sie die Tags in den Agent-Knoten innerhalb des AKS-Clusters geändert haben. Das Ändern und Löschen von Tags und anderen Eigenschaften von Ressourcen in der Ressourcengruppe MC_* kann zu unerwarteten Ergebnissen führen. Durch das Ändern der Ressourcen unter der Ressourcengruppe MC_* im AKS-Cluster wird das SLO unterbrochen.



---
title: Problembehandlung bei allgemeinen Problemen mit Azure Kubernetes Service
description: Erfahren Sie, wie Sie allgemeine Probleme bei der Verwendung von Azure Kubernetes Service (AKS) beheben und lösen können
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: fd3d1c464c6f2d4cbecd715db0689581ca141769
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654069"
---
# <a name="aks-troubleshooting"></a>AKS-Problembehandlung

Beim Erstellen oder Verwalten von Clustern mit Azure Kubernetes Service (AKS) können gelegentlich Probleme auftreten. In diesem Artikel werden einige allgemeine Probleme und Schritte zur Problembehandlung ausführlich erläutert.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Wo finde ich grundsätzlich Informationen zum Debuggen von Problemen mit Kubernetes?

Lesen Sie den [offiziellen Leitfaden zur Problembehandlung von Kubernetes-Clustern](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
Es gibt auch einen [Leitfaden zur Problembehandlung](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md), der von einem Microsoft-Techniker veröffentlicht wurde und sich mit der Problembehandlung von Pods, Knoten, Clustern usw. befasst.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Während der Erstellung oder Aktualisierung erhalte ich die Fehlermeldung, dass das Kontingent überschritten wurde. Wie sollte ich vorgehen? 

Sie müssen [Kerne anfordern](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>Was ist die maximale Einstellung für die Anzahl von Pods pro Knoten für AKS?

Die maximale Anzahl von Pods pro Knoten ist standardmäßig auf 30 festgelegt, wenn Sie einen AKS-Cluster im Azure-Portal bereitstellen.
Die maximale Anzahl von Pods pro Knoten ist standardmäßig auf 110 festgelegt, wenn Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle bereitstellen. (Stellen Sie sicher, dass Sie die aktuelle Version der Azure-Befehlszeilenschnittstelle verwenden.) Diese Standardeinstellung kann mithilfe des Flags `–-max-pods` im Befehl `az aks create` geändert werden.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Bei der Bereitstellung eines AKS-Clusters mit erweitertem Netzwerk erhalte ich einen insufficientSubnetSize-Fehler. Wie sollte ich vorgehen?

In der Netzwerkoption für ein benutzerdefiniertes virtuelles Azure-Netzwerk bei der AKS-Erstellung wird die Azure-Containernetzwerkschnittstelle (Container Network Interface, CNI) für die IP-Adressverwaltung (IPAM) verwendet. Die Anzahl der Knoten in einem AKS-Cluster kann einen beliebigen Wert zwischen 1 und 100 aufweisen. Basierend auf dem vorherigen Abschnitt sollte das Subnetz größer als das Produkt der Anzahl von Knoten und der maximalen Pods pro Knoten sein. Die Beziehung kann folgendermaßen ausgedrückt werden: Subnetzgröße > Anzahl der Knoten im Cluster × maximale Pods pro Knoten.

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mein Pod ist im Modus CrashLoopBackOff hängen geblieben. Wie sollte ich vorgehen?

Es kann verschiedene Gründe dafür geben, dass der Pod in diesem Modus hängen bleibt. Untersuchen Sie Folgendes:

* Den Pod selbst mithilfe von `kubectl describe pod <pod-name>`
* Die Protokolle mit `kubectl log <pod-name>`

Weitere Informationen zur Behandlung von Podproblemen finden Sie unter [Debuggen von Anwendungen](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Ich versuche, RBAC in einem vorhandenen Cluster zu aktivieren. Wie gehe ich dazu vor?

Eine Aktivierung der rollenbasierten Zugriffssteuerung (RBAC) in vorhandenen Clustern wird derzeit nicht unterstützt. Sie müssen explizit neue Cluster erstellen. Wenn Sie die Befehlszeilenschnittstelle verwenden, ist RBAC standardmäßig aktiviert. Wenn Sie das AKS-Portal verwenden, ist eine Umschaltfläche zum Aktivieren von RBAC im Erstellungsworkflow verfügbar.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Ich habe über die Azure-Befehlszeilenschnittstelle mit Standardwerten oder über das Azure-Portal einen Cluster mit aktivierter RBAC erstellt, und nun werden im Kubernetes-Dashboard zahlreiche Warnungen angezeigt. Das Dashboard hat vorher ohne Warnungen funktioniert. Wie sollte ich vorgehen?

Der Grund für die Warnungen auf dem Dashboard ist, dass der Cluster nun mit RBAC aktiviert wurde und der Zugriff darauf standardmäßig deaktiviert wurde. Dieser Ansatz gilt allgemein als bewährte Methode, da die standardmäßige Offenlegung des Dashboards für alle Benutzer des Clusters zu Sicherheitsrisiken führen kann. Wenn Sie das Dashboard weiterhin aktivieren möchten, befolgen Sie die Schritte in [diesem Blogbeitrag](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Ich kann keine Verbindung mit dem Dashboard herstellen. Wie sollte ich vorgehen?

Der einfachste Weg, außerhalb des Clusters auf Ihren Dienst zuzugreifen, besteht im Ausführen von `kubectl proxy`, der Proxyanforderungen für Ihren localhost-Port 8001 an den Kubernetes-API-Server sendet. Von dort kann der API-Server eine Proxyverbindung mit Ihrem Dienst herstellen: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Wenn das Kubernetes-Dashboard nicht angezeigt wird, überprüfen Sie, ob der Pod `kube-proxy` im Namespace `kube-system` ausgeführt wird. Wenn der Pod keinen Ausführungsstatus aufweist, müssen Sie ihn löschen. Er wird anschließend neu gestartet.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Ich kann Protokolle nicht mithilfe von kubectl-Protokollen abrufen, oder ich kann keine Verbindung mit dem API-Server herstellen. Ich erhalte „Fehler vom Server: Fehler beim Anwählen des Back-Ends: Wählen Sie tcp …“. Wie sollte ich vorgehen?

Stellen Sie sicher, dass die Standard-NSG (Netzwerksicherheitsgruppe) nicht geändert wurde und dass Port 22 für die Verbindung mit dem API-Server geöffnet ist. Überprüfen Sie, ob der Pod `tunnelfront` im Namespace `kube-system` ausgeführt wird. Falls nicht, erzwingen Sie das Löschen des Pods. Er wird anschließend neu gestartet.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-problem"></a>Ich möchte ein Upgrade oder eine Skalierung ausführen und erhalte die folgende Fehlermeldung: „Das Ändern der Eigenschaft ‚imageReference‘ ist nicht zulässig.“  Wie kann ich dieses Problem beheben?

Möglicherweise erhalten Sie diese Fehlermeldung, da Sie die Tags in den Agent-Knoten innerhalb des AKS-Clusters geändert haben. Das Ändern und Löschen von Tags und anderen Eigenschaften von Ressourcen in der Ressourcengruppe MC_* kann zu unerwarteten Ergebnissen führen. Durch das Ändern der Ressourcen unter der Gruppe „MC_*“ im AKS-Cluster wird das Servicelevelziel (SLO) unterbrochen.

## <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>Wie verlängere ich den geheimen Dienstprinzipalschlüssel in meinem AKS-Cluster?

Standardmäßig werden AKS-Cluster mit einem Dienstprinzipal mit einer Ablaufzeit von einem Jahr erstellt. Wenn Sie sich dem Ablaufdatum nähern, können Sie die Anmeldeinformationen zurücksetzen, um den Dienstprinzipal um einen zusätzlichen Zeitraum zu verlängern.

Im folgenden Beispiel werden diese Schritte ausgeführt:

1. Abrufen der Dienstprinzipal-ID des Clusters mit dem Befehl [az aks show](/cli/azure/aks#az-aks-show).
1. Auflisten des geheimen Clientschlüssels des Dienstprinzipals mit dem Befehl [az ad sp credential list](/cli/azure/ad/sp/credential#az-ad-sp-credential-list)
1. Verlängern des Dienstprinzipals um ein weiteres Jahr mit dem Befehl [az ad sp credential-reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset). Der geheime Clientschlüssel des Dienstprinzipals muss gleich bleiben, damit der AKS-Cluster ordnungsgemäß ausgeführt werden kann.

```azurecli
# Get the service principal ID of your AKS cluster.
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret.
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for one year.
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```

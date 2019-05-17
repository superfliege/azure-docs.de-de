---
title: Häufig gestellte Fragen zu Azure Kubernetes Service (AKS)
description: Enthält Antworten auf einige der häufig gestellten Fragen zu Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2019
ms.author: iainfou
ms.openlocfilehash: 17bc1d2b7a08314f19f1bf8f87d0c774afc37500
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508175"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Häufig gestellte Fragen zu Azure Kubernetes Service (AKS)

Dieser Artikel behandelt häufig gestellte Fragen zu Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welche Azure-Regionen stellen Azure Kubernetes Service (AKS) zurzeit zur Verfügung?

Eine vollständige Liste der verfügbaren Regionen finden Sie unter [AKS-Regionen und Verfügbarkeit][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>Unterstützt AKS die automatische Skalierung von Knoten?

Ja, die automatische Skalierung ist ab Kubernetes 1.10 über den [Kubernetes Autoscaler][auto-scaler] verfügbar. Weitere Informationen zum Konfigurieren und Verwenden der automatischen Skalierungsfunktion für Cluster finden Sie unter [Cluster Autoscaler in AKS][aks-cluster-autoscale].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Unterstützt AKS die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Kubernetes?

Ja, Kubernetes RBAC ist standardmäßig aktiviert, wenn Cluster mit der Azure CLI erstellt werden. RBAC kann für Cluster aktiviert werden, die über das Azure-Portal oder mithilfe von Vorlagen erstellt wurden.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kann ich AKS in meinem vorhandenen virtuellen Netzwerk bereitstellen?

Ja, Sie können einen AKS-Cluster in einem vorhandenen virtuellen Netzwerk mithilfe des [erweiterten Netzwerkfeatures][aks-advanced-networking] bereitstellen.

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>Kann ich einschränken, dass auf den Kubernetes-API-Server nur über meinem virtuellen Netzwerk zugegriffen werden kann?

Derzeit leider nicht. Der Kubernetes-API-Server wird als öffentlicher vollqualifizierter Domänenname (FQDN) verfügbar gemacht. Den Zugriff auf Ihren Cluster können Sie mit der [rollenbasierten Kubernetes-Zugriffssteuerung (Role-Based Access Control, RBAC) und mit Azure Active Directory (AAD)][aks-rbac-aad] steuern.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Werden Sicherheitsupdates auf AKS-Agent-Knoten angewendet?

Ja, Azure wendet automatisch Sicherheitspatches auf die Knoten in Ihrem Cluster an, und zwar gemäß einem nächtlichen Zeitplan. Sie sind jedoch dafür verantwortlich, dass die Knoten bei Bedarf neu gestartet werden. Sie haben mehrere Möglichkeiten, einen Neustart von Knoten auszuführen:

- Manuell über das Azure-Portal oder die Azure-CLI.
- Durch ein Upgrade des AKS-Clusters. Clusterupgrades [sperren Knoten automatisch ab und gleichen sie aus][cordon-drain] und starten sie anschließend mit dem neuesten Ubuntu-Image und einer neuen Patchversion oder einer Kubernetes-Nebenversion neu. Weitere Informationen finden Sie unter [Aktualisieren eines AKS-Clusters][aks-upgrade].
- Mit [Kured](https://github.com/weaveworks/kured), einem Open Source-Neustartdaemon für Kubernetes. Kured wird als ein [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ausgeführt und überwacht jeden Knoten auf das Vorhandensein einer Datei, die angibt, dass ein Neustart erforderlich ist. Neustarts des Betriebssystems werden clusterübergreifend verwaltet, wobei derselbe [Vorgang des Absperrens und Ausgleichens][cordon-drain] wie bei einem Clusterupgrade angewendet wird.

Weitere Informationen zur Verwendung von kured finden Sie unter [Apply security and kernel updates to nodes in Azure Kubernetes Service (AKS)][node-updates-kured] (Anwenden von Sicherheits- und Kernelupdates auf Knoten in Azure Kubernetes Service (AKS)).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Warum werden zwei Ressourcengruppen mit AKS erstellt?

Jede AKS-Bereitstellung umfasst zwei Ressourcengruppen:

- Die erste Ressourcengruppe wird von Ihnen erstellt und enthält nur die Kubernetes-Dienstressource. Der AKS-Ressourcenanbieter erstellt während der Bereitstellung automatisch die zweite Ressourcengruppe, z.B. *MC_myResourceGroup_myAKSCluster_eastus*. Weitere Informationen dazu, wie Sie den Namen dieser zweiten Ressourcengruppe angeben können, finden Sie im nächsten Abschnitt.
- Diese zweite Ressourcengruppe (z.B. *MC_myResourceGroup_myAKSCluster_eastus*) enthält alle Infrastrukturressourcen für den Cluster. Diese Ressourcen umfassen die virtuellen Computer des Kubernetes-Knotens, virtuelle Netzwerke und Speicher. Diese separate Ressourcengruppe wird erstellt, um die Ressourcenbereinigung zu vereinfachen.

Wenn Sie Ressourcen erstellen, die mit Ihrem AKS-Cluster verwendet werden sollen (z.B. Speicherkonten oder reservierte öffentliche IP-Adressen), platzieren Sie diese in der automatisch generierten Ressourcengruppe.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>Kann ich meinen eigenen Namen für die AKS-Infrastrukturressourcengruppe angeben?

Ja. Standardmäßig erstellt der AKS-Ressourcenanbieter während der Bereitstellung automatisch eine sekundäre Ressourcengruppe, z. B. *MC_myResourceGroup_myAKSCluster_eastus*. Sie können Ihren eigenen Namen für diese verwaltete (*MC_*-)Clusterressourcengruppe angeben, um die Unternehmensrichtlinie zu erfüllen.

Installieren Sie die [aks-preview][aks-preview-cli]-Erweiterungsversion *0.3.2* oder höher für die Azure CLI, um Ihren eigenen Ressourcengruppennamen anzugeben. Verwenden Sie bei der Erstellung eines AKS-Clusters mit dem Befehl [az aks create][az-aks-create] den Parameter *--node-resource-group*, und geben Sie einen Namen für die Ressourcengruppe an. Wenn Sie [eine Azure Resource Manager-Vorlage verwenden][aks-rm-template], um einen AKS-Cluster bereitzustellen, können Sie die *nodeResourceGroup*-Eigenschaft verwenden, um den Namen der Ressourcengruppe zu definieren.

* Diese Ressourcengruppe wird automatisch von dem Azure-Ressourcenanbieter in Ihrem eigenen Abonnement erstellt.
* Sie können nur einen benutzerdefinierten Namen für die Ressourcengruppe angeben, wenn der Cluster erstellt wird.

Folgende Szenarios werden nicht unterstützt:

* Sie können keine vorhandene Ressourcengruppe für *MC_*-Gruppe angeben.
* Sie können kein anderes Abonnement für die *MC_*-Ressourcengruppe angeben.
* Sie können den Namen der *MC_*-Ressourcengruppe nicht mehr ändern, nachdem der Cluster erstellt wurde.
* Sie können keine Namen für die verwalteten Ressourcen innerhalb der *MC_*-Ressourcengruppe angeben.
* Sie können keine Tags von verwalteten Ressourcen innerhalb der *MC_*-Ressourcengruppe ändern oder löschen (weitere Information hierzu finden Sie im nächsten Abschnitt).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Kann ich Tags und anderen Eigenschaften der AKS-Ressourcen in der Ressourcengruppe MC_* ändern?

Das Ändern und Löschen der von Azure erstellten Tags und anderen Eigenschaften von Ressourcen in der Ressourcengruppe *MC_** kann zu unerwarteten Ergebnissen wie Skalierungs- und Aktualisierungsfehlern führen. Unterstützt wird das Erstellen und Ändern zusätzlicher benutzerdefinierter Tags, z.B. um einen Geschäftsbereich oder eine Kostenstelle zuzuweisen. Durch das Ändern der Ressourcen unter der Gruppe *MC_** im AKS-Cluster wird das Servicelevelziel (SLO) unterbrochen. Weitere Informationen finden Sie unter [Bietet AKS eine Vereinbarung zum Servicelevel?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Welche Kubernetes-Zugangssteuerungen werden von AKS unterstützt? Können Zulassungscontroller hinzugefügt oder entfernt werden?

AKS unterstützt die folgenden [Zugangssteuerungen][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

Derzeit ist es nicht möglich, die Liste der Zugriffssteuerungen in AKS zu ändern.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Ist Azure Key Vault in AKS integriert?

AKS ist derzeit nicht nativ in Azure Key Vault integriert. Das Projekt [Azure Key Vault FlexVolume für Kubernetes][keyvault-flexvolume] ermöglicht die direkte Integration von Kubernetes-Pods in KeyVault-Geheimnisse.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kann ich Windows Server-Container unter AKS ausführen?

Um Windows Server-Container auszuführen, müssen Sie Windows Server-basierte Knoten ausführen. Windows Server-basierte Knoten sind derzeit in AKS nicht verfügbar. Mit Virtual Kubelet können Sie jedoch Windows-Container in Azure Container Instances planen und sie im Rahmen Ihres AKS-Clusters verwalten. Weitere Informationen finden Sie unter [Virtuelles Kubelet mit AKS][virtual-kubelet].

## <a name="does-aks-offer-a-service-level-agreement"></a>Bietet AKS eine Vereinbarung zum Servicelevel?

In einer Vereinbarung zum Servicelevel (Service Level Agreement, SLA) sichert der Anbieter zu, dem Kunden die Kosten des Diensts zu erstatten, falls das veröffentliche Servicelevel nicht erfüllt wird. AKS ist kostenlos. Da keine Kosten erstattet werden können, gibt es auch keine formale SLA. AKS ist jedoch bestrebt, für den Kubernetes-API-Server eine Verfügbarkeit von mindestens 99,5 % zu bieten.

## <a name="why-can-i-not-set-maxpods-below-30"></a>Warum kann ich für `maxPods` keinen Wert festlegen, der kleiner als 30 ist?

AKS unterstützt das Festlegen des `maxPods`-Werts zur Erstellungszeit des Clusters über die Azure CLI und Azure Resource Manager-Vorlagen. Es gibt jedoch einen *Mindestwert* (zum Erstellungszeitpunkt überprüft) für Kubenet und Azure CNI, wie unten gezeigt:

| Netzwerk | Minimum | Maximum |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

Da es sich bei AKS um einen verwalteten Dienst handelt, stellen wir Add-Ons und Pods zur Verfügung, die wir als Teil des Clusters bereitstellen und verwalten. In der Vergangenheit konnten Benutzer einen `maxPods`-Wert definieren, der kleiner ist als der Wert, der für den Betrieb der verwalteten Pods erforderlich ist (Beispiel: 30), AKS berechnet jetzt die Mindestanzahl der Pods über: ((maxPods or (maxPods * vm_count)) > managed add-on pods minimum.

Benutzer dürfen die minimale `maxPods`-Validierung nicht außer Kraft setzen.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol

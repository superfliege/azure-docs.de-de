---
title: Häufig gestellte Fragen zum Azure Kubernetes Service
description: Enthält Antworten auf einige der häufig gestellten Fragen zu Azure Kubernetes Service.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/11/2018
ms.author: iainfou
ms.openlocfilehash: 915f74df69596b1677a0e03770e076ae50efc609
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001244"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Häufig gestellte Fragen zu Azure Kubernetes Service (AKS)

Dieser Artikel behandelt häufig gestellte Fragen zu Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welche Azure-Regionen stellen Azure Kubernetes Service (AKS) zurzeit zur Verfügung?

Eine vollständige Liste finden Sie in der Azure Kubernetes Service-Dokumentation unter [Kontingente und Verfügbarkeit von Azure Kubernetes Service (AKS) in den Regionen][aks-regions].

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Werden Sicherheitsupdates auf AKS-Agent-Knoten angewendet?

Azure wendet automatisch Sicherheitspatches auf die Knoten in Ihrem Cluster an, und zwar gemäß einem nächtlichen Zeitplan. Sie sind jedoch dafür verantwortlich, dass die Knoten bei Bedarf neu gestartet werden. Sie haben mehrere Möglichkeiten, einen Neustart von Knoten auszuführen:

- Manuell über das Azure-Portal oder die Azure-CLI.
- Durch ein Upgrade des AKS-Clusters. Clusterupgrades [sperren Knoten automatisch ab und gleichen sie aus](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) und starten sie anschließend mit dem neuesten Ubuntu-Image neu. Aktualisieren Sie das Betriebssystemimage auf Ihren Knoten, ohne Änderungen an den Kubernetes-Versionen vorzunehmen, indem Sie die aktuelle Clusterversion in `az aks upgrade` angeben.
- Mit [Kured](https://github.com/weaveworks/kured), einem Open Source-Neustartdaemon für Kubernetes. Kured wird als ein [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ausgeführt und überwacht jeden Knoten auf das Vorhandensein einer Datei, die angibt, dass ein Neustart erforderlich ist. Dann werden Neustarts über den Cluster orchestriert und folgen dem zuvor beschriebenen Vorgang des Absperrens und Ausgleichens.

## <a name="does-aks-support-node-autoscaling"></a>Unterstützt AKS die automatische Skalierung von Knoten?

Ja, die automatische Skalierung ist ab Kubernetes 1.10 über den [Kubernetes Autoscaler][auto-scaler] verfügbar.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Unterstützt AKS die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Kubernetes?

Ja, RBAC kann bei der Bereitstellung eines AKS-Clusters über die Azure CLI oder eine Azure Resource Manager-Vorlage aktiviert werden. Diese Funktion ist bald im Azure-Portal verfügbar.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>Welche Kubernetes-Zugangssteuerungen werden von AKS unterstützt? Kann das konfiguriert werden?

AKS unterstützt die folgenden [Zugangssteuerungen][admission-controllers]:

* NamespaceLifecycle
* LimitRanger
* ServiceAccount
* DefaultStorageClass
* DefaultTolerationSeconds
* MutatingAdmissionWebhook
* MutatingAdmissionWebhook
* ResourceQuota
* DenyEscalatingExec
* AlwaysPullImages

Derzeit ist es nicht möglich, die Liste der Zugriffssteuerungen in AKS zu ändern.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kann ich AKS in meinem vorhandenen virtuellen Netzwerk bereitstellen?

Ja, Sie können einen AKS-Cluster in einem vorhandenen Netzwerk mithilfe dem [erweiterten Netzwerkfeature](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md) bereitstellen.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Ist Azure Key Vault in AKS integriert?

AKS ist derzeit nicht nativ in Azure Key Vault integriert. Es gibt jedoch Communitylösungen wie [den acs-keyvault-agent von Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kann ich Windows Server-Container unter AKS ausführen?

Um Windows Server-Container auszuführen, müssen Sie Windows Server-basierte Knoten ausführen. Windows Server-basierte Knoten sind derzeit in AKS nicht verfügbar. Mit Virtual Kubelet können Sie jedoch Windows-Container in Azure Container Instances planen und sie im Rahmen Ihres AKS-Clusters verwalten. Weitere Informationen finden Sie unter [Virtuelles Kubelet mit AKS][virtual-kubelet].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Warum werden zwei Ressourcengruppen mit AKS erstellt?

Jede AKS-Bereitstellung umfasst zwei Ressourcengruppen. Die erste Ressourcengruppe wird von Ihnen erstellt und enthält nur die Kubernetes-Dienstressource. Der AKS-Ressourcenanbieter erstellt während der Bereitstellung automatisch die zweite Ressourcengruppe mit einem Namen wie z.B. *MC_myResourceGroup_myAKSCluster_eastus*. Die zweite Ressourcengruppe enthält alle Infrastrukturressourcen für den Cluster, etwa virtuelle Computer, Netzwerke und Speicherressourcen. Sie wird erstellt, um die Ressourcenbereinigung zu vereinfachen.

Wenn Sie Ressourcen erstellen, die mit Ihrem AKS-Cluster verwendet werden sollen (z.B. Speicherkonten oder eine reservierte öffentliche IP-Adresse), sollten Sie diese in der automatisch generierten Ressourcengruppe platzieren.

## <a name="does-aks-offer-a-service-level-agreement"></a>Bietet AKS eine Vereinbarung zum Servicelevel?

In einer Vereinbarung zum Servicelevel (service level agreement, SLA) sichert der Anbieter zu, dem Kunden die Kosten des Diensts zu erstatten, falls das veröffentliche Servicelevel nicht erfüllt wird. AKS ist kostenlos. Da keine Kosten erstattet werden können, gibt es auch keine formale SLA. Wir sind jedoch bestrebt, für den Kubernetes-API-Server eine Verfügbarkeit von mindestens 99,5 Prozent zu bieten.

<!-- LINKS - internal -->

[aks-regions]: ./container-service-quotas.md
[virtual-kubelet]: virtual-kubelet.md

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/

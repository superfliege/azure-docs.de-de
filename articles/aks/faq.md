---
title: Häufig gestellte Fragen zum Azure Kubernetes Service
description: Enthält Antworten auf einige der häufig gestellten Fragen zu Azure Kubernetes Service.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/09/2018
ms.author: nepeters
ms.openlocfilehash: 3152dc69bc8fb9a94111f85976e5d999c4b18261
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Häufig gestellte Fragen zu Azure Kubernetes Service (AKS)

Dieser Artikel behandelt häufig gestellte Fragen zu Azure Kubernetes Service (AKS).

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) befindet sich derzeit in der **Vorschauversion**. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welche Azure-Regionen stellen Azure Kubernetes Service (AKS) zurzeit zur Verfügung?

- Kanada, Mitte
- Kanada, Osten
- USA (Mitte)
- USA (Ost)
- Europa, Westen

## <a name="when-will-additional-regions-be-added"></a>Wann werden weitere Regionen hinzugefügt?

Zusätzliche Regionen werden bei steigendem Bedarf hinzugefügt.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Werden Sicherheitsupdates auf AKS-Agent-Knoten angewendet?

Azure wendet automatisch Sicherheitspatches auf die Knoten in Ihrem Cluster an, und zwar gemäß einem nächtlichen Zeitplan. Sie sind jedoch dafür verantwortlich, dass die Knoten bei Bedarf neu gestartet werden. Sie haben mehrere Möglichkeiten, einen Neustart von Knoten auszuführen:

- Manuell über das Azure-Portal oder die Azure-CLI.
- Durch ein Upgrade des AKS-Clusters. Clusterupgrades [sperren Knoten automatisch ab und gleichen sie aus](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) und starten sie anschließend mit dem neuesten Ubuntu-Image neu. Aktualisieren Sie das Betriebssystemimage auf Ihren Knoten, ohne Änderungen an den Kubernetes-Versionen vorzunehmen, indem Sie die aktuelle Clusterversion in `az aks upgrade` angeben.
- Mit [Kured](https://github.com/weaveworks/kured), einem Open Source-Neustartdaemon für Kubernetes. Kured wird als ein [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ausgeführt und überwacht jeden Knoten auf das Vorhandensein einer Datei, die angibt, dass ein Neustart erforderlich ist. Dann werden diese Neustarts über den Cluster orchestriert und folgen dem zuvor beschriebenen Vorgang des Absperrens und Ausgleichens.

## <a name="do-you-recommend-customers-use-acs-or-aks"></a>Empfehlen Sie Kunden die Verwendung von ACS oder AKS?

Während AKS in der Vorschau bleibt, empfehlen wir, Produktionscluster mit ACS-Kubernetes oder [acs-engine](https://github.com/azure/acs-engine) zu erstellen. Verwenden Sie AKS für Proof of Concept-Implementierungen und Entwicklungs-/Testumgebungen.

## <a name="when-will-acs-be-deprecated"></a>Wenn wird ACS veraltet sein?

ACS wird etwa dann veraltet sein, wenn AKS allgemein verfügbar sein wird. Ab diesem Datum haben Sie 12 Monate Zeit, Cluster zu AKS zu migrieren. Während des Zeitraums von 12 Monaten können Sie alle ACS-Vorgänge ausführen.

## <a name="does-aks-support-node-autoscaling"></a>Unterstützt AKS die automatische Skalierung von Knoten?

Automatische Knotenskalierung wird nicht unterstützt, ist aber geplant. Werfen Sie einen Blick auf diese [Implementierung der automatischen Skalierung ][auto-scaler] in Open Source.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Unterstützt AKS die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Kubernetes?

Nein, RBAC wird zurzeit nicht in AKS unterstützt, wird jedoch in Kürze verfügbar sein.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kann ich AKS in meinem vorhandenen virtuellen Netzwerk bereitstellen?

Ja, dies wird durch die [erweiterte Netzwerkfunktion](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md) unterstützt.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Ist Azure Key Vault in AKS integriert?

Nein, diese Integration ist aber geplant. In der Zwischenzeit sollten Sie die folgende Lösung von [Hexadite][hexadite] ausprobieren.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kann ich Windows Server-Container unter AKS ausführen?

Um Windows Server-Container auszuführen, müssen Sie Windows Server-basierte Knoten ausführen. Windows Server-basierte Knoten befinden sich derzeit in der [privaten Vorschau](https://azure.microsoft.com/en-us/blog/kubernetes-on-azure/). Wenn Sie Windows Server-Container unter Kubernetes in Azure außerhalb der Vorschau ausführen müssen, lesen Sie die [Dokumentation zu acs-engine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Warum werden zwei Ressourcengruppen mit AKS erstellt?

Jede AKS-Bereitstellung umfasst zwei Ressourcengruppen. Die erste Ressourcengruppe wird von Ihnen erstellt und enthält nur die AKS-Ressource. Der AKS-Ressourcenanbieter erstellt während der Bereitstellung automatisch die zweite Ressourcengruppe mit einem Namen wie z.B. *MC_myResourceGRoup_myAKSCluster_eastus*. Die zweite Ressourcengruppe enthält alle Infrastrukturressourcen für den Cluster, etwa virtuelle Computer, Netzwerke und Speicherressourcen. Sie wird erstellt, um die Ressourcenbereinigung zu vereinfachen.

Wenn Sie Ressourcen erstellen, die mit Ihrem AKS-Cluster verwendet werden sollen (z.B. Speicherkonten oder eine reservierte öffentliche IP-Adresse), sollten Sie diese in der automatisch generierten Ressourcengruppe platzieren.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent

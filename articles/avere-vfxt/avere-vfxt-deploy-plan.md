---
title: Planen des Avere vFXT-Systems – Azure
description: Erläutert die Planung vor der Bereitstellung von Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f0e5523565dc561ed457dbc340835ad1889cb876
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669867"
---
# <a name="plan-your-avere-vfxt-system"></a>Planen des Avere vFXT-Systems

In diesem Artikel wird erläutert, wie Sie einen neuen Avere vFXT für Azure-Cluster planen, um sicherzustellen, dass der von Ihnen erstellte Cluster entsprechend Ihren Anforderungen positioniert und dimensioniert ist. 

Bevor Sie zum Azure Marketplace wechseln oder virtuelle Computer erstellen, überlegen Sie, wie der Cluster mit anderen Elementen in Azure interagieren wird. Planen Sie, wo sich die Clusterressourcen in Ihrem privaten Netzwerk und Ihren Subnetzen befinden werden, und entscheiden Sie, wo sich Ihr Back-End-Speicher befinden soll. Stellen Sie sicher, dass die von Ihnen erstellten Clusterknoten leistungsfähig genug sind, um Ihren Workflow zu unterstützen. 

Weitere Informationen finden Sie im Folgenden.

## <a name="resource-group-and-network-infrastructure"></a>Ressourcengruppe und Netzwerkinfrastruktur

Überlegen Sie, wo sich die Elemente Ihrer Avere vFXT für Azure-Bereitstellung befinden werden. Das folgende Diagramm zeigt eine mögliche Anordnung der Avere vFXT für Azure-Komponenten:

![Diagramm, das den Clustercontroller und die virtuellen Clustercomputer innerhalb eines Subnetzes darstellt. Die Subnetzgrenze ist von einer VNet-Begrenzung umgeben. Innerhalb des VNets befindet sich ein Sechseck, das den Endpunkt des Speicherdiensts darstellt. Es ist mit einem gestrichelten Pfeil mit einem Blob-Speicher außerhalb des VNets verbunden.](media/avere-vfxt-components-option.png)

Befolgen Sie die folgenden Richtlinien bei der Planung der Netzwerkinfrastruktur Ihres Avere vFXT-Systems:

* Alle Elemente sollten mit einem neuen Abonnement verwaltet werden, das für die Avere vFXT-Bereitstellung erstellt wurde. Diese Strategie vereinfacht die Kostennachverfolgung und -bereinigung und hilft auch bei der Aufteilung von Ressourcenkontingenten. Da der Avere vFXT mit einer großen Anzahl von Clients verwendet wird, schützt die Isolierung der Clients und des Clusters in einem einzigen Abonnement andere kritische Workloads vor möglicher Ressourcenbegrenzung während der Clientbereitstellung.

* Platzieren Sie Ihre Clientcomputersysteme in der Nähe des vFXT-Clusters. Der Back-End-Speicher kann weiter entfernt sein.  

* Der Einfachheit halber positionieren Sie den vFXT-Cluster und die Clustercontroller-VM in demselben virtuellen Netzwerk (VNet) und in derselben Ressourcengruppe. Sie sollten zudem dasselbe Speicherkonto verwenden. 

* Der Cluster muss sich in einem eigenen Subnetz befinden, um IP-Adressenkonflikte mit Clients oder Computeressourcen zu vermeiden. 

## <a name="ip-address-requirements"></a>Anforderungen an die IP-Adresse 

Stellen Sie sicher, dass das Subnetz Ihres Clusters einen ausreichend großen IP-Adressbereich aufweist, um den Cluster zu unterstützen. 

Der Avere vFXT-Cluster verwendet die folgenden IP-Adressen:

* Eine IP-Adresse zur Verwaltung des Clusters. Diese Adresse kann im Cluster von Knoten zu Knoten verschoben werden, ist aber immer verfügbar, sodass Sie sich mit dem Konfigurationstool der Avere-Systemsteuerung verbinden können.
* Für jeden Clusterknoten:
  * Mindestens eine clientseitige IP-Adresse. (Alle clientseitigen Adressen werden vom *vserver* des Clusters verwaltet, der sie bei Bedarf zwischen den Knoten verschieben kann.)
  * Eine IP-Adresse für die Clusterkommunikation
  * Eine Instanz-IP-Adresse (dem virtuellen Computer zugeordnet)

Wenn Sie Azure-BLOB-Speicher verwenden, erfordert dieser möglicherweise auch IP-Adressen aus dem VNet Ihres Clusters:  

* Ein Azure-BLOB-Speicherkonto erfordert mindestens fünf IP-Adressen. Beachten Sie diese Anforderung, wenn Sie Blob-Speicher im gleichen VNet wie Ihren Cluster positionieren.
* Wenn Sie Azure-BLOB-Speicher verwenden, der sich außerhalb des virtuellen Netzwerks für Ihren Cluster befindet, sollten Sie einen Speicherdienstendpunkt innerhalb des VNets erstellen. Dieser Endpunkt verwendet keine IP-Adresse.

Sie haben die Möglichkeit, Netzwerkressourcen und Blob-Speicher (falls verwendet) in verschiedenen Ressourcengruppen des Clusters zu positionieren.

## <a name="vfxt-node-sizes"></a>vFXT-Knotengrößen 

Die virtuellen Computer, die als Clusterknoten dienen, bestimmen den Anforderungsdurchsatz und die Speicherkapazität Ihres Cache. Sie haben die Wahl zwischen zwei Instanztypen mit unterschiedlichen Eigenschaften für Arbeitsspeicher, Prozessor und lokalen Speicher. 

Die einzelnen vFXT-Knoten sind identisch. Das bedeutet, dass Sie beim Erstellen eines Clusters mit drei Knoten über drei virtuelle Computer des gleichen Typs und mit identischer Größe verfügen. 

| Instanztyp | vCPUs | Arbeitsspeicher  | Lokaler SSD-Speicher  | Max. Anzahl Datenträger | Durchsatz des Datenträgers ohne Cache | NIC (Anzahl) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D16s_v3 | 16  | 64 GiB  | 128 GB  | 32 | 25.600 IOPS <br/> 384 MBit/s | 8.000 MBit/s (8) |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GB  | 32 | 51.200 IOPS <br/> 768 MBit/s | 16.000 MBit/s (8)  |

Der Datenträgercache pro Knoten ist konfigurierbar und kann zwischen 1000 GB und 8000 GB liegen. 1 TB pro Knoten ist die empfohlene Cachegröße für Standard_D16s_v3-Knoten und 4 TB pro Knoten für Standard_E32s_v3-Knoten.

Weitere Informationen zu diesen virtuellen Computern finden Sie in den folgenden Microsoft Azure-Dokumenten:

* [Universelle VM-Größen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general)
* [Arbeitsspeicheroptimierte Größen virtueller Computer](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Kontokontingent

Stellen Sie sicher, dass Ihr Abonnement über die Kapazität verfügt, den Avere vFXT-Cluster sowie alle verwendeten Computer- oder Clientsysteme auszuführen. Weitere Informationen finden Sie unter [Kontingent für den vFXT-Cluster](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster).

## <a name="back-end-data-storage"></a>Back-End-Datenspeicher

Wenn es sich nicht im Cache befindet, wird der Arbeitssatz in einem neuen Blobcontainer oder in einem bestehenden Cloud- oder Hardwarespeichersystem gespeichert?

Wenn Sie den Azure-BLOB-Speicher für das Back-End verwenden möchten, sollten Sie im Rahmen der Erstellung des vFXT-Clusters einen neuen Container erstellen. Verwenden Sie das Bereitstellungsskript ``create-cloud-backed-container``, und geben Sie das Speicherkonto für den neuen Blobcontainer an. Diese Option erstellt und konfiguriert den neuen Container so, dass er sofort nach der Fertigstellung des Clusters einsatzbereit ist. Weitere Informationen finden Sie unter [Erstellen von Knoten und Konfigurieren des Clusters](avere-vfxt-deploy.md#create-nodes-and-configure-the-cluster).

> [!NOTE]
> Nur leere Blob-Speichercontainer können als Kernspeichereinheiten für das Avere vFXT-System verwendet werden. Der vFXT muss in der Lage sein, seinen Objektspeicher zu verwalten, ohne dass bestehende Daten erhalten bleiben müssen. 
>
> Lesen Sie [Verschieben von Daten in den vFXT-Cluster](avere-vfxt-data-ingest.md), um zu erfahren, wie Sie Daten effizient in den neuen Container des Clusters kopieren können, indem Sie Clientcomputer und den Avere vFXT-Cache verwenden.

Wenn Sie ein vorhandenes lokales Speichersystem verwenden möchten, müssen Sie es nach der Erstellung dem vFXT-Cluster hinzufügen. Das ``create-minimal-cluster``-Bereitstellungsskript erstellt einen vFXT-Cluster ohne Back-End-Speicher. Ausführliche Anweisungen zum Hinzufügen eines vorhandenen Speichersystems zum Avere vFXT-Cluster finden Sie unter [Konfigurieren von Speicher](avere-vfxt-add-storage.md). 

## <a name="next-step-understand-the-deployment-process"></a>Nächster Schritt: Verstehen des Bereitstellungsprozesses

[Übersicht über die Bereitstellung](avere-vfxt-deploy-overview.md) bietet eine Übersicht über alle Schritte, die erforderlich sind, um ein Avere vFXT für Azure-System zu erstellen und es für die Bereitstellung von Daten vorzubereiten.  
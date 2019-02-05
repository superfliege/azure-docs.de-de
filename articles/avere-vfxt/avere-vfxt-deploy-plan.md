---
title: Planen des Avere vFXT-Systems – Azure
description: Erläutert die Planung vor der Bereitstellung von Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: e60c92c22382112558307062afdeb87e08075765
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55298924"
---
# <a name="plan-your-avere-vfxt-system"></a>Planen des Avere vFXT-Systems

In diesem Artikel wird erläutert, wie Sie einen neuen Avere vFXT für Azure-Cluster planen, um sicherzustellen, dass der von Ihnen erstellte Cluster entsprechend Ihren Anforderungen positioniert und dimensioniert ist. 

Bevor Sie zum Azure Marketplace wechseln oder virtuelle Computer erstellen, überlegen Sie, wie der Cluster mit anderen Elementen in Azure interagieren wird. Planen Sie, wo sich die Clusterressourcen in Ihrem privaten Netzwerk und Ihren Subnetzen befinden werden, und entscheiden Sie, wo sich Ihr Back-End-Speicher befinden soll. Stellen Sie sicher, dass die von Ihnen erstellten Clusterknoten leistungsfähig genug sind, um Ihren Workflow zu unterstützen. 

Weitere Informationen finden Sie im Folgenden.

## <a name="resource-group-and-network-infrastructure"></a>Ressourcengruppe und Netzwerkinfrastruktur

Überlegen Sie, wo sich die Elemente Ihrer Avere vFXT für Azure-Bereitstellung befinden werden. Das folgende Diagramm zeigt eine mögliche Anordnung der Avere vFXT für Azure-Komponenten:

![Diagramm, das den Clustercontroller und die virtuellen Clustercomputer innerhalb eines Subnetzes darstellt. Die Subnetzgrenze ist von einer VNet-Begrenzung umgeben. Innerhalb des VNets befindet sich ein Sechseck, das den Endpunkt des Speicherdiensts darstellt. Es ist mit einem gestrichelten Pfeil mit einem Blob-Speicher außerhalb des VNets verbunden.](media/avere-vfxt-components-option.png)

Befolgen Sie die folgenden Richtlinien bei der Planung der Netzwerkinfrastruktur Ihres Avere vFXT-Systems:

* Alle Elemente sollten mit einem neuen Abonnement verwaltet werden, das für die Avere vFXT-Bereitstellung erstellt wurde. Dies hat unter anderem folgende Vorteile: 
  * Einfachere Nachverfolgung der Kosten: Zeigen Sie alle Kosten aus Ressourcen, Infrastruktur und Computezyklen in einem Abonnement an, und überprüfen Sie sie.
  * Einfachere Bereinigung: Sie können das gesamte Abonnement entfernen, wenn Sie das Projekt abgeschlossen haben.
  * Bequeme Partitionierung von Ressourcenkontingenten: Schützen Sie andere kritische Workloads vor möglicher Ressourcendrosselung, wenn Sie die große Anzahl von Clients für Ihren Hochleistungscomputingworkflow nutzen, indem Sie die Avere vFXT-Clients und -Cluster in einem einzigen Abonnement isolieren.

* Platzieren Sie Ihre Clientcomputersysteme in der Nähe des vFXT-Clusters. Der Back-End-Speicher kann weiter entfernt sein.  

* Der Einfachheit halber positionieren Sie den vFXT-Cluster und die Clustercontroller-VM in demselben virtuellen Netzwerk (VNet) und in derselben Ressourcengruppe. Sie sollten zudem dasselbe Speicherkonto verwenden. (Der Clustercontroller erstellt den Cluster und kann auch für die Clusterverwaltung über die Befehlszeile verwendet werden.)  

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

Wo sollte der Avere vFXT-Cluster Ihre Daten speichern, wenn sie sich nicht im Cache befinden? Entscheiden Sie, ob Ihr Arbeitssatz langfristig in einem neuen Blobcontainer oder in einem vorhandenen Cloud- oder Hardwarespeichersystem gespeichert wird. 

Wenn Sie den Azure-BLOB-Speicher für das Back-End verwenden möchten, sollten Sie im Rahmen der Erstellung des vFXT-Clusters einen neuen Container erstellen. Diese Option erstellt und konfiguriert den neuen Container so, dass er sofort nach der Fertigstellung des Clusters einsatzbereit ist. 

Details finden Sie unter [Erstellen des Avere vFXT-Systems für Azure](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure).

> [!NOTE]
> Nur leere Blob-Speichercontainer können als Kernspeichereinheiten für das Avere vFXT-System verwendet werden. Der vFXT muss in der Lage sein, seinen Objektspeicher zu verwalten, ohne dass bestehende Daten erhalten bleiben müssen. 
>
> Lesen Sie [Verschieben von Daten in den vFXT-Cluster](avere-vfxt-data-ingest.md), um zu erfahren, wie Sie Daten effizient in den neuen Container des Clusters kopieren können, indem Sie Clientcomputer und den Avere vFXT-Cache verwenden.

Wenn Sie ein vorhandenes lokales Speichersystem verwenden möchten, müssen Sie es nach der Erstellung dem vFXT-Cluster hinzufügen. Ausführliche Anweisungen zum Hinzufügen eines vorhandenen Speichersystems zum Avere vFXT-Cluster finden Sie unter [Konfigurieren von Speicher](avere-vfxt-add-storage.md).

## <a name="cluster-access"></a>Clusterzugriff 

Der Avere vFXT-Cluster für Azure befindet sich in einem privaten Subnetz und besitzt keine öffentliche IP-Adresse. Sie müssen über eine Methode verfügen, um auf das private Subnetz für die Clusterverwaltung und Clientverbindungen zuzugreifen. 

Folgende Zugriffsoptionen sind verfügbar:

* Jump Host: Weisen Sie einer separaten VM im privaten Netzwerk eine öffentliche IP-Adresse zu, und erstellen Sie damit einen SSL-Tunnel zu den Clusterknoten. 

  > [!TIP]
  > Wenn Sie eine öffentliche IP-Adresse für den Clustercontroller festlegen, können Sie ihn als Jump Host verwenden. Weitere Informationen finden Sie unter [Clustercontroller als Jump Host](#cluster-controller-as-jump-host).

* Virtuelles privates Netzwerk (VPN): Konfigurieren Sie ein Point-to-Site- oder Site-to-Site-VPN mit Ihrem privaten Netzwerk.

* Azure ExpressRoute: Konfigurieren Sie eine private Verbindung über einen ExpressRoute-Partner. 

Ausführliche Informationen zu diesen Optionen finden Sie in der [Dokumentation zu Azure Virtual Network unter „Internetkommunikation“](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet).

### <a name="cluster-controller-as-jump-host"></a>Clustercontroller als Jump Host

Wenn Sie eine öffentliche IP-Adresse für den Clustercontroller festlegen, können Sie diesen als Jump Host verwenden, um Kontakt mit dem Avere vFXT-Cluster von außerhalb des privaten Subnetzes herzustellen. Da der Controller jedoch Zugriffsrechte zum Ändern von Clusterknoten besitzt, entsteht ein kleines Sicherheitsrisiko.  

Verwenden Sie für verbesserte Sicherheit mit einer öffentlichen IP-Adresse eine Netzwerksicherheitsgruppe, um eingehenden Zugriff nur über Port 22 zuzulassen.

Beim Erstellen des Clusters können Sie auswählen, ob Sie eine öffentliche IP-Adresse für den Clustercontroller erstellen möchten. 

* Wenn Sie ein neues VNET oder ein neues Subnetz erstellen, wird dem Clustercontroller eine öffentliche IP-Adresse zugewiesen.
* Wenn Sie ein vorhandenes VNET und Subnetz auswählen, verfügt der Clustercontroller nur über private IP-Adressen. 

## <a name="next-step-understand-the-deployment-process"></a>Nächster Schritt: Verstehen des Bereitstellungsprozesses

[Übersicht über die Bereitstellung](avere-vfxt-deploy-overview.md) bietet eine Übersicht über alle Schritte, die erforderlich sind, um ein Avere vFXT für Azure-System zu erstellen und es für die Bereitstellung von Daten vorzubereiten.  
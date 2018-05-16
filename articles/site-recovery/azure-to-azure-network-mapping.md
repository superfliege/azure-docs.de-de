---
title: Zuordnung virtueller Netzwerke zwischen zwei Azure-Regionen in Azure Site Recovery | Microsoft-Dokumentation
description: Azure Site Recovery koordiniert Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server. Informieren Sie sich über das Failover zu Azure oder zu einem sekundären Rechenzentrum.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 9294940785deb0834a419de8320286783635d68e
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Zuordnen virtueller Netzwerke in verschiedenen Azure-Regionen


In diesem Artikel wird beschrieben, wie Sie zwei Instanzen von Azure Virtual Network in verschiedenen Azure-Regionen einander zuordnen. Durch die Netzwerkzuordnung wird sichergestellt, dass beim Erstellen eines replizierten virtuellen Computers in der Azure-Zielregion dieser auch im virtuellen Netzwerk erstellt wird, das dem virtuellen Netzwerk des virtuellen Quellcomputers zugeordnet ist.  

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie vor dem Zuordnen von Netzwerken sicher, dass Sie sowohl in der Azure-Quellregion als auch in der Azure-Zielregion ein [virtuelles Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) erstellt haben.

## <a name="map-virtual-networks"></a>Zuordnen von virtuellen Netzwerken

Zum Zuordnen eines virtuellen Azure-Netzwerks, das sich in einer Azure-Region (Quellnetzwerk) befindet, zu einer anderen Region (Zielnetzwerk) wechseln Sie für virtuelle Azure-Computer zu **Site Recovery-Infrastruktur** > **Netzwerkzuordnung**. Erstellen Sie eine Netzwerkzuordnung.

![Fenster „Netzwerkzuordnungen“ – Erstellen einer Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Im folgenden Beispiel wird der virtuelle Computer in der Region „Asien, Osten“ ausgeführt. Der virtueller Maschine wird zur Region „Asien, Südosten“ repliziert.

Zum Erstellen einer Netzwerkzuordnung aus der Region „Asien, Osten“ zur Region „Asien, Südosten“ wählen Sie den Standort des Quellnetzwerks und den Standort des Zielnetzwerks aus. Wählen Sie anschließend **OK** aus.

![Fenster „Netzwerkzuordnung hinzufügen“ – Auswählen der Quell- und Zielstandorte für das Quellnetzwerk](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Wiederholen Sie den vorhergehenden Vorgang, um eine Netzwerkzuordnung aus der Region „Asien, Südosten“ zur Region „Asien, Osten“ zu erstellen.

![Bereich „Netzwerkzuordnung hinzufügen“ – Auswählen der Quell- und Zielstandorte für das Zielnetzwerk](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Zuordnen eines Netzwerk beim Aktivieren der Replikation

Bei der ersten Replikation eines virtuellen Computers aus einer Azure-Region in eine andere Region können Sie beim Einrichten der Replikation das Zielnetzwerk festlegen, sofern keine Netzwerkzuordnung vorhanden ist. Basierend auf dieser Einstellung erstellt Azure Site Recovery Netzwerkzuordnungen von der Quellregion an die Zielregion und von der Zielregion an die Quellregion.   

![Bereich „Konfigurieren von Einstellungen“ – Wählen des Zielstandorts](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Site Recovery erstellt standardmäßig ein Netzwerk in der Zielregion, das identisch mit dem Quellnetzwerk ist. Site Recovery erstellt ein Netzwerk durch Anfügen des Suffixes **-asr** an den Namen des Quellnetzwerks. Zum Auswählen eines bereits erstellten Netzwerks wählen Sie **Anpassen**.

![Bereich zum Anpassen von Zieleinstellungen – Festlegen der Namen der Zielressourcengruppe und des virtuellen Zielnetzwerks](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Wenn die Netzwerkzuordnung bereits erfolgt ist, können Sie das virtuelle Zielnetzwerk beim Aktivieren der Replikation nicht ändern. In diesem Fall müssen Sie zum Ändern des virtuellen Zielnetzwerks die vorhandene Netzwerkzuordnung ändern.  

![Bereich zum Anpassen von Zieleinstellungen – Festlegen des Namens der Zielressourcengruppe](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Bereich zum Ändern der Netzwerkzuordnung – Ändern des Namens eines vorhandenen virtuellen Zielnetzwerks](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Achten Sie beim Ändern einer Netzwerkzuordnung von Region A zu Region B darauf, auch die Netzwerkzuordnung von Region B zu Region A zu ändern.
>
>


## <a name="subnet-selection"></a>Subnetzauswahl
Das Subnetz des virtuellen Zielcomputers wird basierend auf dem Namen des Subnetzes des virtuellen Quellcomputers ausgewählt. Wenn im Zielnetzwerk ein Subnetz mit demselben Namen wie der virtuelle Quellcomputer verfügbar ist, wird dieses Subnetz für den virtuellen Zielcomputer ausgewählt. Wenn im Zielnetzwerk kein Subnetz mit demselben Namen vorhanden ist, wird das Subnetz als Zielsubnetz ausgewählt, das in der alphabetischen Reihenfolge an erster Stelle steht.

Zum Ändern des Subnetzes rufen Sie die **Compute- und Netzwerkeinstellungen** für den virtuellen Computer auf.

![Fenster „Eigenschaften für Compute und Netzwerk“](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-Adresse

Die IP-Adresse für jede Netzwerkschnittstelle des virtuellen Zielcomputers wird gemäß der Beschreibung in den folgenden Abschnitten ausgewählt.

### <a name="dhcp"></a>DHCP
Wenn die Netzwerkschnittstelle des virtuellen Quellcomputers DHCP verwendet, wird die Netzwerkschnittstelle des virtuellen Zielcomputers ebenfalls als DHCP festgelegt.

### <a name="static-ip-address"></a>Statische IP-Adresse
Wenn die Netzwerkschnittstelle des virtuellen Quellcomputers eine statische IP-Adresse verwendet, wird die Netzwerkschnittstelle des virtuellen Zielcomputers ebenfalls als statische IP-Adresse festgelegt. In den folgenden Abschnitten wird das Festlegen einer statischen IP-Adresse beschrieben.

#### <a name="same-address-space"></a>Gleicher Adressraum

Wenn das Quellsubnetz und das Zielsubnetz über denselben Adressraum verfügen, wird die IP-Adresse der Netzwerkschnittstelle des virtuellen Quellcomputers als Ziel-IP-Adresse festgelegt. Wenn dieselbe IP-Adresse nicht verfügbar ist, wird die nächste verfügbare IP-Adresse als Ziel-IP-Adresse festgelegt.

#### <a name="different-address-spaces"></a>Verschiedene Adressräume

Wenn das Quellsubnetz und das Zielsubnetz über unterschiedliche Adressräume verfügen, wird die nächste verfügbare IP-Adresse im Zielsubnetz als Ziel-IP-Adresse festgelegt.

Zum Ändern der Ziel-IP an den einzelnen Netzwerkschnittstellen rufen Sie die Einstellungen für **Compute und Netzwerk** des virtuellen Computers auf.

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie den [Netzwerkleitfaden zum Replizieren virtueller Azure-Computer](site-recovery-azure-to-azure-networking-guidance.md).

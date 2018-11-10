---
title: Konfigurieren und Verwalten von Replikationsrichtlinien für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Replikationseinstellungen für die VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery konfigurieren.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: fd987097c2ca7b1e7509a1a0e63905c36ec8fec8
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212895"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery-to-azure"></a>Konfigurieren und Verwalten von Replikationsrichtlinien für die VMware-Notfallwiederherstellung in Azure
In diesem Artikel wird beschrieben, wie Sie mithilfe von [Azure Site Recovery](site-recovery-overview.md) eine Replikationsrichtlinie für die Replikation von VMware-VMs in Azure konfigurieren.


## <a name="create-a-policy"></a>Erstellen einer Richtlinie

1. Wählen Sie **Verwalten** > **Site Recovery-Infrastruktur**.
1. Wählen Sie unter **For VMware and Physical machines** (Für VMware und physische Computer) die Option **Replikationsrichtlinien** aus. 
1. Klicken Sie auf **+ Replikationsrichtlinie**, und geben Sie den Richtliniennamen an.
1. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Wenn die fortlaufende Replikation diesen Grenzwert überschreitet, werden Warnungen generiert.
1. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** die Dauer des Aufbewahrungszeitfensters für die einzelnen Wiederherstellungspunkte (in Stunden) an. Geschützte Computer können innerhalb eines Aufbewahrungszeitfensters an einem beliebigen Punkt wiederhergestellt werden. Für in Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt. Bei Storage Standard werden bis zu 72 Stunden unterstützt.
1. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.
1. Klicken Sie auf **OK**. Die Richtlinie sollte innerhalb von 30 bis 60 Sekunden erstellt werden.

Wenn Sie eine Replikationsrichtlinie erstellen, wird automatisch eine entsprechende Replikationsrichtlinie für das Failback erstellt, die das Suffix „failback“ enthält. Nach dem Erstellen der Richtlinie können Sie sie bearbeiten, indem Sie **Einstellungen bearbeiten** auswählen.

## <a name="associate-a-configuration-server"></a>Zuordnen eines Konfigurationsservers 

Ordnen Sie Ihrem lokalen Konfigurationsserver die Replikationsrichtlinie zu.

1. Klicken Sie auf **Zuordnen**, und wählen Sie den Konfigurationsserver aus.

    ![Zuordnen des Konfigurationsservers](./media/vmware-azure-set-up-replication/associate1.png)

1. Klicken Sie auf **OK**. Die Zuordnung des Konfigurationsservers sollte innerhalb von ein bis zwei Minuten abgeschlossen sein.

    ![Zuordnung des Konfigurationsservers](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Aufheben der Zuordnung oder Löschen einer Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie aus.
    a. Um die Zuordnung der Richtlinie zum Konfigurationsserver aufzuheben, stellen Sie sicher, dass die Richtlinie von keinen replizierten Computern verwendet wird. Klicken Sie anschließend auf **Zuordnung aufheben**.
    b. Um die Richtlinie zu löschen, stellen Sie sicher, dass diese keinem Konfigurationsserver zugeordnet ist. Klicken Sie anschließend auf **Löschen**. Das Löschen dauert 30 bis 60 Sekunden.
1. Klicken Sie auf **OK**.

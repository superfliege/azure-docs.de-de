---
title: "Richten Sie die Einstellungen für die Replikation für Azure Site Recovery | Microsoft Docs"
description: Beschreibt, wie zum Site Recovery zum orchestrieren der Replikation, Failover und Wiederherstellung von Hyper-V-VMs in der VMM-Clouds in Azure bereitstellen.
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 73a1f19177f23441f5f7165cf2bc92ba85e62aa5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Verwalten von Replikationsrichtlinie für VMware in Azure


## <a name="create-a-replication-policy"></a>Erstellen Sie eine Replikationsrichtlinie

1. Wählen Sie **verwalten** > **Site Recovery-Infrastruktur**.
2. Wählen Sie **Replikationsrichtlinien** unter **für VMware und physischen Computern**.
3. Wählen Sie **+ Replikationsrichtlinie**.

    ![Erstellen der Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Geben Sie den Namen der Richtlinie ein.

5. In **RPO-Schwellenwert**, geben Sie die RPO-Höchstgrenze. Bei der fortlaufenden Replikation über diesen Grenzwert überschreitet, werden Warnungen generiert werden.
6. In **Aufbewahrungszeitraum des Wiederherstellungspunkts Punkt**, geben Sie die Dauer der die Beibehaltungsdauer für alle Wiederherstellungspunkte (in Stunden). Geschützte Computer können zu jedem beliebigen Punkt in einem aufbewahrungsfenster wiederhergestellt werden.

    > [!NOTE]
    > Bis zu 24 Stunden der Beibehaltung der wird für Computer angegeben, die auf Premium-Speicher unterstützt. Bis zu 72 Stunden der Beibehaltung der wird für Computer, die in Standardspeicher repliziert unterstützt.

    > [!NOTE]
    > Eine Failback-Replikationsrichtlinie wird automatisch erstellt.

7. In **Häufigkeit App-konsistenter Momentaufnahmen**, geben Sie Häufigkeit (in Minuten), die mit der anwendungskonsistente Momentaufnahmen enthalten Wiederherstellungspunkte erstellt werden.

8. Klicken Sie auf **OK**. Die Richtlinie sollte in 30 bis 60 Sekunden erstellt werden.

![Replikation richtliniengenerierung](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Ordnen Sie einem Konfigurationsserver eine Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie Konfigurationsserver zugeordnet werden sollen.
2. Klicken Sie auf **zuordnen**.
![Konfigurationsserver zuordnen](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Wählen Sie aus der Liste der Server den Konfigurationsserver.
4. Klicken Sie auf **OK**. Der Konfigurationsserver muss ein bis zwei Minuten zugeordnet werden.

![Konfiguration-Server-Zuordnung](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Bearbeiten Sie eine Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie für die Sie die Einstellungen für die Replikation bearbeiten möchten.
![Bearbeiten der Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klicken Sie auf **Einstellungen bearbeiten**.
![Bearbeiten Sie die Einstellungen für Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Ändern Sie die Einstellungen, die basierend auf Ihren Bedürfnissen an.
4. Klicken Sie auf **Speichern**. Die Richtlinie in zwei bis fünf Minuten gespeichert werden soll, je nachdem wie viele virtuelle Computer verwenden diese Replikationsrichtlinie.

![Speichern der Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>Trennen Sie einen Konfigurationsserver aus einer Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie Konfigurationsserver zugeordnet werden sollen.
2. Klicken Sie auf **trennen**.
3. Wählen Sie aus der Liste der Server den Konfigurationsserver.
4. Klicken Sie auf **OK**. Der Konfigurationsserver sollte in ein bis zwei Minuten getrennt werden.

    > [!NOTE]
    > Sie können keinem Konfigurationsserver nicht trennen, wenn mindestens eine replizierte Element mithilfe der Richtlinie vorhanden ist. Stellen Sie sicher, dass es sind keine replizierten Elemente mithilfe der Richtlinie, bevor Sie den Konfigurationsserver trennen.

## <a name="delete-a-replication-policy"></a>Löschen Sie eine Replikationsrichtlinie

1. Wählen Sie die Replikationsrichtlinie aus, der Sie löschen möchten.
2. Klicken Sie auf **löschen**. Die Richtlinie sollte in 30 bis 60 Sekunden gelöscht werden.

    > [!NOTE]
    > Eine Replikationsrichtlinie kann nicht gelöscht werden, verfügt über mindestens ein Konfigurationsserver zugeordnet ist. Stellen Sie sicher, es sind keine replizierten Elemente mithilfe der Richtlinie, und löschen Sie alle zugeordneten Konfigurationsserver aus, bevor Sie die Richtlinie löschen.

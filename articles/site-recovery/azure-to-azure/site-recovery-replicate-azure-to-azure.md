---
title: "Replizieren virtueller Azure-Computer in eine sekundäre Region mit Azure Site Recovery | Microsoft-Dokumentation"
description: In diesem Artikel erfahren Sie, wie Sie mit dem Azure Site Recovery-Dienst Azure-VMs zwischen Azure-Regionen replizieren.
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/29/2017
ms.author: asgang
ms.openlocfilehash: 114390712f5e5bf62ec515db62469e09361b8f85
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replizieren von virtuellen Azure-Computern in einer anderen Azure-Region


In diesem Artikel erfahren Sie, wie Sie mit dem Azure Site Recovery-Dienst virtuelle Azure-Computer (VMs) aus einer Azure-Region in eine sekundäre Azure-Region replizieren.

>[!NOTE]
>
> Die Replikation von Azure-VMs mit Site Recovery befindet sich derzeit in der Vorschau.

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen einen Recovery Services-Tresor. Es wird empfohlen, den Tresor in der Zielregion zu erstellen, in die Sie die VMs replizieren möchten.
* Wenn Sie NSG-Regeln (Netzwerksicherheitsgruppen) oder einen Firewallproxy verwenden, um den Zugriff für ausgehende Internetverbindungen auf den Azure-VMs zu steuern, stellen Sie sicher, dass die erforderlichen URLs oder IP-Adressen zugelassen sind. [Weitere Informationen](./site-recovery-azure-to-azure-networking-guidance.md).
* Wenn eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen und dem Quellstandort in Azure besteht, [informieren Sie sich über deren Einrichtung](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration).
* Ihr Azure-Benutzerkonto muss über bestimmte [Berechtigungen](../site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation einer Azure-VM verfügen.
Ihr Azure-Abonnement sollte zum Erstellen von VMs am Zielstandort aktiviert sein, den Sie als Region für die Notfallwiederherstellung verwenden möchten. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

## <a name="enable-replication"></a>Replikation aktivieren

In diesem Verfahren werden als Quellstandort „Asien, Osten“ und als Ziel „Asien, Südosten“ verwendet.

1. Klicken Sie im Tresor auf **+Replizieren**, um die Replikation für die virtuellen Computer zu aktivieren.
2. Vergewissern Sie sich, dass **Quelle** auf **Azure** festgelegt ist.
3. Legen Sie **Quellstandort** auf „Asien, Osten“ fest.
4. Wählen Sie als **Bereitstellungsmodell** entweder **Klassisch** oder **Resource Manager** aus.
5. Wählen Sie in **Ressourcengruppe** die Gruppe aus, zu der die Quell-VMs gehören. Es werden alle virtuellen Computer unter der ausgewählten Ressourcengruppe aufgeführt.

    ![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

6. Klicken Sie auf **Virtuelle Computer > Virtuelle Computer auswählen**, und wählen Sie die VMs aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)
    
7. Geben Sie unter **Einstellungen** > **Zielstandort** an, wohin die Daten der Quell-VM repliziert werden. Site Recovery stellt eine Liste geeigneter Zielregionen bereit, die auf der Region der ausgewählten virtuellen Computer basiert.
8. Site Recovery legt Standardeinstellungen für das Ziel fest. Diese können bei Bedarf geändert werden.

    - **Zielressourcengruppe:** Site Recovery erstellt standardmäßig in der Zielregion eine neue Ressourcengruppe mit dem Suffix „asr“. Wenn die erstellte Ressourcengruppe bereits vorhanden ist, wird sie wiederverwendet.
    - **Virtuelles Zielnetzwerk:** Site Recovery erstellt standardmäßig in der Zielregion ein neues virtuelles Netzwerk mit dem Suffix „asr“. Dieses Netzwerk wird Ihrem Quellnetzwerk zugeordnet. Informationen zur Netzwerkzuordnung finden Sie [hier](site-recovery-network-mapping-azure-to-azure.md).
    - **Zielspeicherkonten:** Site Recovery erstellt standardmäßig ein neues Zielspeicherkonto, das der Speicherkonfiguration der Quell-VM entspricht. Wenn das erstellte Konto bereits vorhanden ist, wird es wiederverwendet.
    - **Cachespeicherkonten:** Azure Site Recovery erstellt ein zusätzliches Cachespeicherkonto in der Quellregion. Alle Änderungen an den virtuellen Quellcomputern werden nachverfolgt und vor der Replikation dieser Computer am Zielstandort an das Cachespeicherkonto gesendet.
    - **Verfügbarkeitsgruppe**. Site Recovery erstellt standardmäßig in der Zielregion eine neue Verfügbarkeitsgruppe mit dem Suffix „asr“. Wenn die erstellte Gruppe bereits vorhanden ist, wird sie wiederverwendet.
    - **Replikationsrichtlinie:** Site Recovery definiert die Einstellungen für den Aufbewahrungsverlauf des Wiederherstellungspunkts und die App-konsistente Momentaufnahmenhäufigkeit. Standardmäßig erstellt Site Recovery eine neue Replikationsrichtlinie mit der Standardeinstellung „24 Stunden“ für den Aufbewahrungszeitraum des Wiederherstellungspunkts und „60 Minuten“ für die App-konsistente Momentaufnahmenhäufigkeit.

    ![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
9. Klicken Sie auf **Replikation aktivieren**, um mit dem Schützen der virtuellen Computer zu beginnen.

## <a name="customize-target-resources"></a>Anpassen der Zielressourcen

1. Ändern Sie diese Standardeinstellungen für das Ziel:

    - **Zielressourcengruppe:** Wählen Sie die Ressourcengruppe in der Liste aller Ressourcengruppen aus, die innerhalb des Abonnements am Zielspeicherort vorhanden sind.
    - **Virtuelles Zielnetzwerk:** Treffen Sie Ihre Auswahl in der Liste aller virtuellen Netzwerke am Zielspeicherort.
    - **Verfügbarkeitsgruppe:** Sie können nur Einstellungen für Verfügbarkeitsgruppen von VMs hinzufügen, die sich in einer Gruppe in der Quellregion befinden.
    - **Zielspeicherkonten:** Fügen Sie alle Konten hinzu, die verfügbar sind.

        ![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Klicken Sie auf **Zielressource erstellen** > **Replikation aktivieren**. Die Aktualisierung des VM-Status kann während der Erstreplikation einige Zeit dauern. Klicken Sie zum Abrufen des aktuellen Status auf **Aktualisieren**.

    ![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)
    
3. Nachdem die virtuellen Computer geschützt wurden, überprüfen Sie die VM-Integrität unter **Replizierte Elemente**.



## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über die [Durchführung eines Testfailovers](../azure-to-azure-tutorial-dr-drill.md).


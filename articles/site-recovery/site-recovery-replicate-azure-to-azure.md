---
title: Replizieren von Anwendungen (Azure, Azure) | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie beim Einrichten der Replikation von virtuellen Computern in einer Azure-Region in einer anderen Region in Azure ausgeführt wird."
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
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: f9f97cf840b722c8cfee169dd1640e0682f287ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replizieren Sie virtuelle Computer in Azure zu einem anderen Azure-region



>[!NOTE]
>
> Site Recovery-Replikation für Azure Virtual Machines ist derzeit als Vorschau verfügbar.

In diesem Artikel wird beschrieben, wie beim Einrichten der Replikation von virtuellen Computern in einer Azure-Region zu einem anderen Azure-Region ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

* Der Artikel wird davon ausgegangen, dass Sie bereits über Site Recovery und Recovery Services-Tresor bekannt. Sie müssen einen "Recovery Services-Tresor" vorab erstellt haben.

    >[!NOTE]
    >
    > Es wird empfohlen, dass Sie die "Recovery Services-Tresor" am Speicherort zu erstellen, in dem Ihre virtuellen Computer repliziert werden soll. Ist das Ziel "USA Mitte", erstellen Sie z. B. Tresor in "USA".

* Wenn Sie Netzwerksicherheitsgruppe Gruppen (NSG)-Regeln oder Firewallproxy für die Zugriffssteuerung für ausgehende Internetverbindung für den Azure-VMs verwenden, stellen Sie sicher, Sie-Positivliste erforderlichen URLs oder IP-Adressen. Verweisen auf [Networking Guidance-Dokument](./site-recovery-azure-to-azure-networking-guidance.md) Weitere Details.

* Wenn Sie eine Expressroute- oder eine VPN-Verbindung zwischen lokalen und den Speicherort der Quelle in Azure haben, führen Sie die [Website Wiederherstellungsaspekte für Azure an ExpressRoute lokal / VPN-Konfiguration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) Dokument.

* Ihr Azure-Benutzerkonto benötigt bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation des virtuellen Azure-Computer.

* Zum Erstellen von virtuellen Computern am Zielspeicherort an, die Sie als DR-Region verwenden möchten, sollte Ihr Azure-Abonnement aktiviert werden. Erhalten Sie Unterstützung dafür, dass das Kontingent erforderliche.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Aktivieren der Replikation von Azure Site Recovery-Tresor
Für diese Abbildung repliziert wir ausgeführter virtueller Maschinen in der "East Asia" Azure-Speicherort zu dem Speicherort "Südostasien". Die Schritte sind wie folgt aus:

 Klicken Sie auf **+ replizieren** im Tresor zum Aktivieren der Replikation für die virtuellen Computer.

1. **Quelle:** bezieht sich auf den Ausgangspunkt des der Computer in diesem Fall also **Azure**.

2. **Quellspeicherort:** ist die Azure-Region aus, wo Sie Ihre virtuellen Computer schützen möchten. Für diese Abbildung wird der Quellspeicherort "East Asia" sein.

3. **Bereitstellungsmodell:** bezieht sich auf das Azure-Bereitstellungsmodell Quellcomputer. Sie können entweder klassischen auswählen oder Ressourcen-Manager und der Computer, die zu dem bestimmten Modell gehören, werden für den Schutz im nächsten Schritt aufgeführt.

      >[!NOTE]
      >
      > Nur können Sie eine klassische virtuelle Maschine repliziert und stellen Sie es als eine klassische virtuelle Maschine wieder her. Sie können ihn nicht als Ressourcen-Manager für virtuelle Computer wiederherstellen.

4. **Ressourcengruppe:** ist die Ressourcengruppe, zu denen Ihre virtuellen Quellcomputern gehören. Für den Schutz im nächsten Schritt werden alle virtuellen Computer unter der ausgewählten Ressourcengruppe aufgeführt.

    ![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

In **virtuelle Maschinen > Wählen Sie virtuelle Computer**auf, und wählen Sie jeden Computer, die Sie replizieren möchten. Sie können nur Computer auswählen, für die Replikation aktiviert werden kann. Klicken Sie anschließend auf %%amp;quot;OK%%amp;quot;.
    ![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


Einstellungen im Abschnitt können Sie die Ziel-Standorteigenschaften konfigurieren.

1. **Zielspeicherort:** Dies ist der Speicherort, in denen die Quelldaten für die virtuelle Maschine werden repliziert. Abhängig von den ausgewählten Computern Speicherort, Site Recovery Geben Sie die Liste der geeigneten Zielregionen an.

    > [!TIP]
    > Es wird empfohlen, behalten Sie den Zielspeicherort an identisch zum Zeitpunkt des Wiederherstellungspunkts services-Tresor.

2. **Zielressourcengruppe:** ist die Ressourcengruppe, zu dem alle Ihre replizierte virtuelle Computer gehören wird. Standardmäßig erstellt ASR eine neue Ressourcengruppe in der Zielregion mit Namen "asr" Suffix. Für den Fall, dass Sie bereits von ASR erstellte Ressourcengruppe vorhanden sind, wird er wiederverwendet werden. Sie könne auch so anzupassen, wie im Abschnitt weiter unten dargestellt.    
3. **Ziel virtuelles Netzwerk:** standardmäßig ASR erstellt ein neues virtuelles Netzwerk in der Zielregion mit Namen "asr" Suffix. Dadurch wird Ihr Quellnetzwerk zugeordnet werden und wird für alle zukünftigen Schutz verwendet werden.

    > [!NOTE]
    > [Überprüfen Sie die Netzwerkdetails](site-recovery-network-mapping-azure-to-azure.md) um weitere Informationen zur Netzwerkzuordnung.

4. **Speicherkonten als Ziel:** standardmäßig ASR neue zielspeicherkonto, wobei Ihre Quell-VM-Speicherkonfiguration erstellt wird. Für den Fall, dass Sie bereits von ASR erstellte Speicherkonto vorhanden, wird er wiederverwendet werden.

5. **Zwischenspeichern von Speicherkonten:** ASR benötigt zusätzlichen Speicherplatz Konto mit dem Namen der Cachespeicher in den Quellbereich. Alle Änderungen, die in der Quell-VMs werden vor der Replikation, die an den Zielspeicherort Cache Speicherkonto gesendet und nachverfolgt.

6. **Verfügbarkeitsgruppe:** ASR erstellt standardmäßig eine neue verfügbarkeitsgruppe, die in der Zielregion mit Namen "asr" Suffix. Für den Fall, dass ASR bereits erstellte Verfügbarkeit Menge vorhanden sind, wird er wiederverwendet werden.

7.  **Replikationsrichtlinie:** definiert die Einstellungen für die Wiederherstellung Punkt Aufbewahrung Verlauf und der app konsistent Häufigkeit von Momentaufnahmen. Standardmäßig wird eine neue Replikationsrichtlinie ASR mit den Standardeinstellungen von 24 Stunden für Aufbewahrungszeitraum des Wiederherstellungspunkts Punkt und "60 Minuten für die Häufigkeit von app-konsistente Momentaufnahmen erstellt werden.

    ![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Anpassen der Zielressourcen

Für den Fall, dass Sie die von ASR verwendeten Standardeinstellungen ändern möchten, können Sie die Einstellungen, die je nach Ihren Anforderungen ändern.

1. **Anpassen:** klicken sie zum Ändern der Standardeinstellungen von ASR verwendet.

2. **Zielressourcengruppe:** können Sie die Ressourcengruppe aus der Liste aller Ressourcengruppen, die innerhalb des Abonnements am Zielspeicherort vorhandene auswählen.

3. **Ziel virtuelles Netzwerk:** Sie finden die Liste der im virtuellen Netzwerk am Zielspeicherort an.

4. **Verfügbarkeitsgruppe:** können Sie die virtuellen Computer, die Verfügbarkeit in Quellbereich gehören nur Verfügbarkeit legt Einstellungen hinzufügen.

5. **Ziel-Speicherkonten:**

![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klicken Sie auf **erstellen Zielressource** und Aktivieren der Replikation


Nachdem der virtuelle Computer geschützt werden können, überprüfen Sie den Status der Integrität von virtuellen Computern unter **replizierten Elemente**

>[!NOTE]
>Konnten Sie während der Zeit der ersten Replikation gibt es eine Möglichkeit, dass Status dauert es bis zu aktualisieren und für einige Zeit nicht Status angezeigt werden. Sie können die Schaltfläche "Aktualisieren" oben auf dem Blatt ", um den neuesten Status abzurufen klicken.
>

![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) zum Ausführen eines Test-Failovers.
- [Erfahren Sie mehr](site-recovery-failover.md) zu verschiedenen Typen von Failover und wie Sie diese ausführen.
- Erfahren Sie mehr über [mit Wiederherstellungsplänen](site-recovery-create-recovery-plans.md) RTO zu reduzieren.
- Erfahren Sie mehr über [erneut zu Azure-VMs schützen](site-recovery-how-to-reprotect.md) nach einem Failover.

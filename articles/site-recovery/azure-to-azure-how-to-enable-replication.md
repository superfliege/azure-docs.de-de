---
title: Konfigurieren der Replikation für virtuelle Azure-Computer in Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Replikation virtueller Azure-Computer zwischen Azure-Regionen mithilfe von Site Recovery konfigurieren.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 79ad4b3598c227ad2c2e3b76562cf95a30e82ad3
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101546"
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replizieren von virtuellen Azure-Computern in einer anderen Azure-Region



In diesem Artikel erfahren Sie, wie Sie die Replikation virtueller Azure-Computer zwischen Azure-Regionen aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie Site Recovery bereits für dieses Szenario eingerichtet haben, wie im Tutorial [Einrichten einer Notfallwiederherstellung für Azure-VMs in eine sekundäre Azure-Region (Vorschau)](azure-to-azure-tutorial-enable-replication.md) beschrieben. Vergewissern Sie sich, dass Sie die erforderlichen Komponenten vorbereitet und den Recovery Services-Tresor erstellt haben.



## <a name="enable-replication"></a>Aktivieren der Replikation

Aktivieren Sie die Replikation. In diesem Verfahren wird davon ausgegangen, dass als primäre Azure-Region „Asien, Osten“ und als sekundäre Region „Asien, Südosten“ verwendet wird.

1. Klicken Sie im Tresor auf **+Replizieren**.
2. Beachten Sie die folgenden Felder:
    - **Quelle**: Der Ursprung der virtuellen Computer, in diesem Fall **Azure**.
    - **Quellstandort**: Die Azure-Region, in der Sie Ihre virtuellen Computer schützen möchten. In diesem Beispiel ist der Quellspeicherort „Asien, Osten“.
    - **Bereitstellungsmodell**: Das Azure-Bereitstellungsmodell der Quellcomputer.
    - **Quellabonnement**: Das Abonnement, dem Ihre virtuellen Quellcomputer angehören. Dies kann ein beliebiges Abonnement im gleichen Azure Active Directory-Mandanten sein, in dem sich auch Ihr Recovery Services-Tresor befindet.
    - **Ressourcengruppe**: Die Ressourcengruppe, der Ihre virtuellen Quellcomputer angehören. Alle virtuellen Computer der ausgewählten Ressourcengruppe werden im nächsten Schritt für den Schutz aufgeführt.

    ![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. Klicken Sie auf **Virtuelle Computer > Virtuelle Computer auswählen**, und wählen Sie die virtuellen Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.
    ![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. Unter **Einstellungen** können Sie optional Einstellungen für den Zielstandort konfigurieren:

    - **Zielstandort**: Der Standort, an den die Daten Ihrer virtuellen Quellcomputer repliziert werden. Abhängig vom ausgewählten Computerstandort, stellt Site Recovery eine Liste der geeigneten Zielregionen bereit. Es empfiehlt sich, als Zielstandort den gleichen Standort zu verwenden wie für den Recovery Services-Tresor.
    - **Zielabonnement**: Das Zielabonnement für die Notfallwiederherstellung. Zielabonnement und Quellabonnement sind standardmäßig identisch.
    - **Zielressourcengruppe**: Die Ressourcengruppe, der all Ihre replizierten virtuellen Computer angehören. Azure Site Recovery erstellt standardmäßig in der Zielregion eine neue Ressourcengruppe, deren Name das Suffix „asr“ aufweist. Falls bereits eine von Azure Site Recovery erstellte Ressourcengruppe vorhanden ist, wird diese wiederverwendet. Sie können die Gruppe auch anpassen, wie im Abschnitt unten gezeigt. Der Speicherort der Zielressourcengruppe kann eine beliebige Azure-Region sein, mit Ausnahme der Region, in der die virtuellen Quellcomputer gehostet werden.
    - **Virtuelles Zielnetzwerk**: Site Recovery erstellt standardmäßig in der Zielregion ein neues virtuelles Netzwerk mit dem Namenssuffix „asr“. Dieses wird Ihrem Quellnetzwerk zugeordnet und für alle zukünftigen Schutzaktivitäten verwendet werden. Informationen zur Netzwerkzuordnung finden Sie [hier](site-recovery-network-mapping-azure-to-azure.md).
    - **Zielspeicherkonten (wenn die Quell-VM keine verwalteten Datenträger verwendet)**: Standardmäßig erstellt Site Recovery ein neues Zielspeicherkonto und übernimmt dabei die Speicherkonfiguration Ihrer Quell-VM. Sollte bereits ein Speicherkonto vorhanden sein, wird dieses wiederverwendet.
    - **Verwaltete Replikatdatenträger (wenn die Quell-VM verwaltete Datenträger verwendet)**: Site Recovery erstellt neue verwaltete Replikatdatenträger in der Zielregion, um die verwalteten Datenträger der Quell-VM zu spiegeln. Dabei wird der gleiche Speichertyp (Standard oder Premium) verwendet wie für die verwalteten Datenträger der Quell-VM.
    - **Cachespeicherkonten**: Site Recovery benötigt als zusätzliches Speicherkonto in der Quellregion ein so genanntes Cachespeicherkonto. Alle Änderungen an den virtuellen Quellcomputern werden nachverfolgt und vor der Replikation dieser Computer am Zielspeicherort an das Cachespeicherkonto gesendet.
    - **Zielverfügbarkeitsgruppen**: Standardmäßig erstellt Azure Site Recovery in der Zielregion eine neue Verfügbarkeitsgruppe und verwendet das Namenssuffix „asr“ für die virtuellen Computer, die zu einer Verfügbarkeitsgruppe in der Quellregion gehören. Falls bereits eine von Azure Site Recovery erstellte Verfügbarkeitsgruppe vorhanden ist, wird diese wiederverwendet.
    - **Zielverfügbarkeitszonen**: Site Recovery weist in der Zielregion standardmäßig die gleiche Anzahl von Zonen zu wie in der Quellregion, sofern die Zielregion Verfügbarkeitszonen unterstützt.

    Unterstützt die Zielregion keine Verfügbarkeitszonen, werden die virtuellen Zielcomputer standardmäßig als einzelne Instanzen konfiguriert. Sie können solche virtuellen Computer bei Bedarf auch als Teil von Verfügbarkeitsgruppen in der Zielregion konfigurieren. Klicken Sie dazu auf „Anpassen“.

    >[!NOTE]
    >Nach der Aktivierung der Replikation können Sie den Verfügbarkeitstyp (einzelne Instanz, Verfügbarkeitsgruppe oder Verfügbarkeitszone) nicht mehr ändern. Wenn Sie den Verfügbarkeitstyp ändern möchten, müssen Sie die Replikation deaktivieren und wieder aktivieren.
    >
    
    - **Replikationsrichtlinie**: Site Recovery definiert die Einstellungen für den Aufbewahrungsverlauf des Wiederherstellungspunkts und die Häufigkeit von App-konsistenten Momentaufnahmen. Standardmäßig erstellt Azure Site Recovery eine neue Replikationsrichtlinie mit der Standardeinstellung „24 Stunden“ für den Aufbewahrungszeitraum des Wiederherstellungspunkts und „60 Minuten“ für die App-konsistente Momentaufnahmenhäufigkeit.

    ![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
  
## <a name="customize-target-resources"></a>Anpassen der Zielressourcen

Sie können die von Site Recovery verwendeten Standardzieleinstellungen ändern.

1. Klicken Sie neben „Zielabonnement“ auf **Anpassen:**, um das Standardzielabonnement zu ändern. Wählen Sie das Abonnement aus der Liste aller Abonnements aus, die im gleichen Azure Active Directory-Mandanten (AAD) verfügbar sind.

2. Klicken Sie zum Ändern der Standardeinstellungen auf **Anpassen**:
    - Wählen Sie unter **Zielressourcengruppe** aus der Liste mit allen Ressourcengruppen, die innerhalb des Abonnements am Zielspeicherort vorhanden sind, die gewünschte Ressourcengruppe aus.
    - Wählen Sie unter **Virtuelles Zielnetzwerk** in der Liste mit allen virtuellen Netzwerken am Zielspeicherort das gewünschte Netzwerk aus.
    - Unter **Verfügbarkeitsgruppe** können Sie dem virtuellen Computer Verfügbarkeitsgruppeneinstellungen hinzufügen, sofern dieser einer Verfügbarkeitsgruppe in der Quellregion angehört.
    - Wählen Sie unter **Zielspeicherkonten** das Konto aus, das Sie verwenden möchten.

        ![Aktivieren der Replikation](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
1. Klicken Sie zum Ändern der Replikationseinstellungen auf **Anpassen**.
   - Wählen Sie in **Konsistenz mehrerer virtueller Computer** die VMs aus, die Sie zusammen replizieren möchten. 
   - Alle Computer in einer Replikationsgruppe verfügen beim Failover über absturz- und anwendungskonsistente Wiederherstellungspunkte. Das Aktivieren der Konsistenz mehrerer virtueller Computer kann sich auf die Leistung der Workload auswirken (da der Vorgang CPU-intensiv ist) und sollte nur verwendet werden, wenn Computer dieselbe Workload ausführen und eine Konsistenz erforderlich ist. Wenn eine Anwendung z.B. über 2 SQL-VMs und 2 Webserver verfügt, sollten Sie nur SQL-VMs als Teil der Replikationsgruppe hinzufügen.
   - Eine Replikationsgruppe kann maximal 16 virtuelle Computer enthalten.
   - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über den Port 20004 miteinander. Stellen Sie sicher, dass die interne Kommunikation zwischen den VMs über Port 20004 nicht durch eine Firewallappliance blockiert wird. Wenn Sie Linux-VMs in eine Replikationsgruppe einschließen möchten, stellen Sie sicher, dass der ausgehende Datenverkehr auf Port 20004 gemäß den Anweisungen für die jeweilige Linux-Version manuell geöffnet wird.
![Replikation aktivieren](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
2. Klicken Sie auf **Zielressource erstellen** > **Replikation aktivieren**.
3. Nachdem Sie die Replikation für die virtuellen Computer aktiviert haben, können Sie unter **Replizierte Elemente** den VM-Integritätsstatus überprüfen.

>[!NOTE]
>Die Aktualisierung des Status kann während der Erstreplikation einige Zeit dauern, und zu Beginn wird unter Umständen kein Fortschritt angezeigt. Klicken Sie auf die Schaltfläche **Aktualisieren**, um den aktuellen Status abzurufen.
>

# <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über die Ausführung von Testfailovervorgängen.

---
title: 'Häufige Fragen: Azure-zu-Azure-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation'
description: In diesem Artikel werden häufig gestellte Fragen zum Einrichten der Notfallwiederherstellung von Azure-VMs in eine andere Azure-Region mithilfe von Azure Site Recovery zusammengefasst.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: 6fe7152e43640a809ab9f4de39b1c6b599975a20
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969946"
---
# <a name="common-questions---azure-to-azure-replication"></a>Häufige Fragen: Azure-zu-Azure-Replikation

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zum Bereitstellen der Notfallwiederherstellung von Azure-VMs in eine andere Azure-Region. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Allgemein
### <a name="how-is-site-recovery-priced"></a>Wie werden Site Recovery-Preise kalkuliert?
Nähere Informationen finden Sie unter [Site Recovery – Preise](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).

### <a name="how-to-configure-site-recovery-on-azure-vms-what-are-the-best-practices"></a>Vorgehensweise: Konfigurieren von Site Recovery auf Azure-VMs. Welche bewährten Methoden gibt es?
1. [Understand Azure to Azure architecture (Grundlegendes zur Azure-zu-Azure-Architektur)](azure-to-azure-architecture.md)
2. [Review the supported and not-supported configurations (Überprüfen der unterstützten und nicht unterstützten Konfigurationen)](azure-to-azure-support-matrix.md)
3. [Set up disaster recovery for Azure VMs (Einrichten der Notfallwiederherstellung für Azure-VMs)](azure-to-azure-how-to-enable-replication.md)
4. [Ausführen eines Testfailovers](azure-to-azure-tutorial-dr-drill.md)
5. [Failover and failback to primary region (Failover und Failback auf die primäre Region)](azure-to-azure-tutorial-failover-failback.md)

## <a name="replication"></a>Replikation

### <a name="can-i-replicate-azure-disk-encryption-enabled-vms"></a>Kann ich VMs replizieren, für die Azure Disk Encryption aktiviert wurde?
Ja, Sie können diese replizieren. Weitere Informationen zum Aktivieren der Replikation von VMs, für die Azure Disk Encryption (ADE) aktiviert wurde, finden Sie in diesem [Artikel](azure-to-azure-how-to-enable-replication-ade-vms.md). Beachten Sie, dass von Azure Site Recovery derzeit nur Azure-VMs unterstützt werden, auf denen das Windows-Betriebssystem ausgeführt wird und die für die Verschlüsselung mit der Azure AD-App aktiviert sind.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Kann ich VMs in ein anderes Abonnement replizieren?
Ja, Sie können Azure-VMs in ein anderes Abonnement im selben Azure Active Directory-Mandanten replizieren.
Das [abonnementübergreifende](https://azure.microsoft.com/blog/cross-subscription-dr) Konfigurieren der Notfallwiederherstellung ist einfach. Sie können zum Zeitpunkt der Replikation ein anderes Abonnement auswählen.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Kann ich an Zonen geheftete Azure-VMs in eine andere Region replizieren?
Ja, Sie können [an Zonen geheftete VMs](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) in eine andere Region replizieren.

### <a name="can-i-exclude-disks"></a>Kann ich Datenträger ausschließen?

Ja, Sie können Datenträger zur Zeit des Schutzes mit PowerShell ausschließen. Wie Sie dabei ein Risiko ausschließen, erfahren Sie im [PowerShell-Leitfaden](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine).

###<a name="how-often-can-i-replicate-to-azure"></a>Wie oft kann ich zu Azure replizieren?
Die Replikation ist fortlaufend, wenn Azure-VMs in eine andere Azure-Region repliziert werden. Weitere Informationen finden Sie unter [Azure-zu-Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process)-Replikationsarchitektur.

### <a name="can-i-replicate-virtual-machines-within-a-same-region-i-need-this-to-migrate-vms"></a>Kann ich virtuelle Computer in derselben Region replizieren? Ist das zum Migrieren von VMs erforderlich?
Sie können die Lösung für die Azure-zu-Azure-Notfallwiederherstellung nicht verwenden, um VMs innerhalb derselben Region zu replizieren.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Kann ich VMs in jede Azure-Region replizieren?
Mit Site Recovery können Sie VMs zwischen zwei beliebigen Regionen im gleichen geografischen Cluster replizieren und wiederherstellen. Geographische Cluster werden unter Berücksichtigung von Datenlatenz und Souveränität definiert. Weitere Informationen finden Sie unter [Unterstützung für Regionen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) für Site Recovery.

### <a name="does-site-recovery-require-internet-connectivity"></a>Ist für Site Recovery eine Internetverbindung erforderlich?

Nein, für Site Recovery ist keine Internetverbindung erforderlich, aber Zugriff auf Site Recovery-URLs und -IP-Adressbereiche wie in diesem [Artikel](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) beschrieben.

## <a name="replication-policy"></a>Replikationsrichtlinie

### <a name="what-is-a-replication-policy"></a>Was ist eine Replikationsrichtlinie?
Eine Replikationsrichtlinie definiert die Einstellungen für den Aufbewahrungsverlauf des Wiederherstellungspunkts und die Häufigkeit von anwendungskonsistenten Momentaufnahmen. Standardmäßig erstellt Azure Site Recovery eine neue Replikationsrichtlinie mit der Standardeinstellung „24 Stunden“ für den Aufbewahrungszeitraum des Wiederherstellungspunkts und „60 Minuten“ für die App-konsistente Momentaufnahmenhäufigkeit.

### <a name="what-is-crash-consistent-recovery-point"></a>Was ist ein absturzkonsistenter Wiederherstellungspunkt?
Ein absturzkonsistenter Wiederherstellungspunkt repräsentiert die Daten auf einem Datenträger so, als ob zum Zeitpunkt der Momentaufnahme die VM abgestürzt wäre oder das Stromkabel aus dem Server gezogen worden wäre. Er enthält nichts, was sich zum Zeitpunkt der Momentaufnahme im Arbeitsspeicher befand. Heutzutage können die meisten Anwendungen aus absturzkonsistenten Momentaufnahmen gut wiederhergestellt werden. Ein absturzkonsistenter Wiederherstellungspunkt ist normalerweise ausreichend für Betriebssysteme ohne Datenbank und Anwendungen wie Dateiserver, DHCP-Server, Druckerserver usw.

### <a name="what-is-application-consistent-recovery-point"></a>Was ist ein anwendungskonsistenter Wiederherstellungspunkt? 
Anwendungskonsistente Wiederherstellungspunkte werden aus anwendungskonsistenten Momentaufnahmen erstellt, die dieselben Daten erfassen wie absturzkonsistente Wiederherstellungspunkte. Zudem werden alle Daten im Arbeitsspeicher und alle laufenden Transaktionen erfasst. Durch diesen zusätzlichen Inhalt sind anwendungskonsistente Momentaufnahmen am stärksten involviert, und ihre Ausführung dauert am längsten. Anwendungskonsistente Wiederherstellungspunkte werden für Betriebssysteme mit Datenbank und Anwendungen wie SQL empfohlen.

### <a name="how-are-recovery-points-generated-and-saved"></a>Wie werden Wiederherstellungspunkte generiert und gespeichert?
Um zu verstehen, wie Site Recovery Wiederherstellungspunkte generiert, nehmen wir als Beispiel eine Replikationsrichtlinie, die einen Wiederherstellungspunkt 24 Stunden lang aufbewahrt und jede Stunde eine anwendungskonsistente Momentaufnahme anfertigt.
Site Recovery erstellt alle 5 Minuten einen absturzkonsistenten Wiederherstellungspunkt, und der Benutzer kann diese Häufigkeit nicht ändern. Somit verfügt der Benutzer für die vergangene Stunde über eine Auswahl von 12 absturzkonsistenten Wiederherstellungspunkten und 1 anwendungskonsistenten Wiederherstellungspunkt. Mit der Zeit löscht Site Recovery alle Wiederherstellungspunkte, die älter als 1 Stunde sind, und speichert nur einen Wiederherstellungspunkt pro Stunde.
Der Screenshot unten veranschaulicht dies:


1. Für die Zeit innerhalb der letzten Stunde sind Wiederherstellungspunkte im Intervall von 5 Minuten vorhanden.
2. Für die Zeit vor der letzten Stunde wird nur ein Wiederherstellungspunkt pro Stunde aufbewahrt.

  ![Generierung von Wiederherstellungspunkten](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>Wie weit kann ich bei der Wiederherstellung zurückgehen?
Der älteste Wiederherstellungspunkt, den Sie verwenden können, ist 72 Stunden alt.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-there-is-no-recovery-points-generation-due-to-some-issue-for-more-than-24-hours-does-my-previous-recovery-points-will-be-pruned"></a>Was passiert, wenn ich über eine Replikationsrichtlinie von 24 Stunden verfüge und durch ein Problem für mehr als 24 Stunden kein Wiederherstellungspunkt generiert wird? Werden dann meine vorherigen Wiederherstellungspunkte gelöscht?
Nein, in diesem Fall bewahrt Site Recovery alle ihre vorherigen Wiederherstellungspunkte auf. 

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Wie ändere ich die Replikationsrichtlinie, nachdem die Replikation auf einer VM aktiviert wurde? 
Navigieren Sie zu „Site Recovery-Tresor“ > „Site Recovery-Infrastruktur“ > „Replikationsrichtlinien“. Wählen Sie die Richtlinie aus, die Sie bearbeiten möchten, und speichern Sie die Änderungen. Alle Änderungen werden auch auf alle vorhandenen Replikationen angewendet. 

### <a name="are-all-the-recovery-points-complete-copy-of-the-vm-or-differential"></a>Sind alle Wiederherstellungspunkte vollständige Kopien der VM oder gibt es Unterschiede?
Bei der ersten Replikation enthält der erste Wiederherstellungspunkt, der generiert wird, eine vollständige Kopie und alle nachfolgenden Wiederherstellungspunkte die Deltaänderungen.

### <a name="does-increasing-recovery-points-retention-windows-increases-the-storage-cost"></a>Erhöhen sich mit längeren Aufbewahrungszeitfenstern der Wiederherstellungspunkte die Speicherkosten?
Ja, wenn Sie den Aufbewahrungszeitraum von 24 auf 72 Stunden erhöhen, speichert Site Recovery die Wiederherstellungspunkte für weitere 48 Stunden, wodurch für Sie Speichergebühren anfallen. Wenn zum Beispiel ein einzelner Wiederherstellungspunkt Deltaänderungen von 10 GB enthält und jedes GB pro Monat 0,16 $ kostet, wären es zusätzliche Gebühren von 1,6 $ × 48 pro Monat.

## <a name="failover"></a>Failover

### <a name="is-failover-automatic"></a>Erfolgt ein Failover automatisch?

Ein Failover erfolgt nicht automatisch. Sie initiieren Failover mit einem Mausklick im Portal, oder Sie können Site Recovery-[PowerShell](azure-to-azure-powershell.md) verwenden, um ein Failover auszulösen. 

### <a name="can-i-retain-public-ip-address-after-failover"></a>Kann ich öffentliche IP-Adressen nach dem Failover beibehalten?

Öffentliche IP-Adressen einer Produktionsanwendung **können bei einem Failover nicht beibehalten werden**. Arbeitsauslastungen, die als Teil eines Failoverprozesses entstehen, müssen einer in der Zielregion verfügbaren öffentlichen Azure-IP-Ressource zugewiesen werden. Dieser Schritt kann entweder manuell erfolgen oder wird mithilfe von Wiederherstellungsplänen automatisiert. Weitere Informationen zum Zuweisen von öffentlichen IP-Adressen mit Wiederherstellungsplan finden Sie in diesem [Artikel](https://docs.microsoft.com/azure/site-recovery/concepts-public-ip-address-with-site-recovery#public-ip-address-assignment-using-recovery-plan).  

### <a name="can-i-retain-private-ip-address-during-failover"></a>Kann ich eine private IP-Adresse während des Failovers beibehalten?
Ja, Sie können eine private IP-Adresse beibehalten. Wenn Sie die Notfallwiederherstellung für virtuelle Azure-Computer aktivieren, werden die Zielressourcen in Site Recovery basierend auf den Einstellungen für die Quellressourcen erstellt. Für virtuelle Azure-Computer, die mit statischen IP-Adressen konfiguriert sind, wird in Site Recovery nach Möglichkeit die gleiche IP-Adresse für den virtuellen Zielcomputer bereitgestellt, sofern sie nicht bereits verwendet wird. Weitere Informationen zum Beibehalten von privaten IP-Adressen unter verschiedenen Bedingungen finden Sie in diesem [Artikel](site-recovery-retain-ip-azure-vm-failover.md).

### <a name="after-failover-the-server-does-not-have-the-same-ip-address-as-the-source-vm-why-is-it-assigned-with-a-new-ip-address"></a>Nach dem Failover weist der Server nicht dieselbe IP-Adresse auf wie die Quell-VM. Warum wurde eine neue IP-Adresse zugewiesen?

Site Recovery versucht, die IP-Adresse nach Möglichkeit zum Zeitpunkt des Failovers bereitzustellen. Falls diese von einem anderen virtuellen Computer übernommen wird, wird die nächste verfügbare IP-Adresse als Ziel festgelegt. Eine vollständige Erklärung der Verarbeitung der Adressierung in Site Recovery finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms).

### <a name="what-does-latestlowest-rpo-recovery-points-means"></a>Was bedeutet „Letzte (niedrigste RPO)“ bei Wiederherstellungspunkten?
Diese Option verarbeitet zuerst alle Daten, die an einen Site Recovery-Dienst gesendet wurden, um vor dem Failover einen Wiederherstellungspunkt für jede VM zu erstellen. Diese Option bietet die niedrigste RPO (Recovery Point Objective), da die nach dem Failover erstellte VM über alle Daten verfügt, die bei Auslösung des Failovers zu Site Recovery repliziert wurden.

### <a name="does-latest-lowest-rpo-recovery-points-have-impact-on-failover-rto"></a>Wirken sich letzte (niedrigste RPO)-Wiederherstellungspunkte auf die Failover-RTO aus?
Ja, da Site Recovery alle ausstehenden Daten vor dem Ausführen des Failovers verarbeitet, verfügt diese Option im Vergleich zu anderen über eine höhere RTO.

### <a name="what-does-latest-processed-option-in-recovery-points-mean"></a>Was bedeutet die Option „Letzte Verarbeitung“ bei Wiederherstellungspunkten?
Mit dieser Option wird ein Failover aller VMs im Plan auf den letzten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde. Den letzten Wiederherstellungspunkt für eine bestimmte VM finden Sie in den Einstellungen der VM unter „Letzte Wiederherstellungspunkte“. Diese Option bietet eine niedrige Recovery Time Objective (RTO), da keine Zeit für die Verarbeitung unverarbeiteter Daten aufgewendet wird.

### <a name="if-im-replicating-between-two-azure-regions-what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Was geschieht, wenn ich eine Replikation zwischen zwei Azure-Regionen ausführe und in meiner primären Region ein unerwarteter Ausfall auftritt?
Sie können nach dem Ausfall ein Failover auslösen. Site Recovery benötigt keine Verbindung mit der primären Region, um das Failover auszuführen.

## <a name="recovery-plan"></a>Wiederherstellungsplan

### <a name="what-is-a-recovery-plan-"></a>Was ist ein Wiederherstellungsplan?
Wiederherstellungspläne in Site Recovery orchestrieren die Failoverwiederherstellung von VMs. Sie tragen zu einer durchgängig exakten, wiederholbaren und automatisierten Wiederherstellung bei. Ein Wiederherstellungsplan kümmert sich für den Benutzer um Folgendes:

- Definieren einer Gruppe virtueller Computer, für die das Failover gemeinsam ausgeführt wird
- Definieren der Abhängigkeiten zwischen virtuellen Computern, damit die Anwendung korrekt funktioniert
- Automatisieren der Wiederherstellung zusammen mit benutzerdefinierten manuellen Aktionen, damit auch andere Vorgänge als das Failover der virtuellen Computer erreicht werden können

[hier](site-recovery-create-recovery-plans.md) .

### <a name="how-does-sequencing-is-achieved-in-a-recovery-plan"></a>Wie wird Sequenzierung in einem Wiederherstellungsplan erreicht?

In einem Wiederherstellungsplan können Sie mehrere Gruppen für die Sequenzierung erstellen. Für eine Gruppe findet das Failover zu einem Zeitpunkt statt, sodass für VMs derselben Gruppe das Failover gleichzeitig durchgeführt wird, gefolgt von einer anderen Gruppe. Hier erfahren Sie, wie Sie eine [Anwendung mit dem Wiederherstellungsplan modellieren.](https://review.docs.microsoft.com/azure/site-recovery/recovery-plan-overview?branch=pr-en-us-61681#model-apps) 

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Wie kann ich die RTO eines Wiederherstellungsplans finden?
Führen Sie für den Wiederherstellungsplan ein Testfailover durch, und navigieren Sie zu „Site Recovery-Aufträge“, um die RTO eines Wiederherstellungsplans zu finden.
Der unten gezeigte SAP-Testwiederherstellungsplan hat zum Beispiel 8 Minuten und 59 Sekunden benötigt, um das Failover aller virtuellen Computer und die angegebenen Aktionen durchzuführen.

  ![com-error](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kann ich Automation-Runbooks zum Wiederherstellungsplan hinzufügen?
Ja, Sie können Azure Automation-Runbooks in Ihren Wiederherstellungsplan integrieren. [Weitere Informationen](site-recovery-runbook-automation.md)

## <a name="reprotect-and-failback"></a>Erneutes Schützen und Failback 

### <a name="after-doing-a-failover-from-primary-region-to-disaster-recovery-region-does-vms-in-a-dr-region-gets-protected-automatically"></a>Werden VMs in einer Notfallwiederherstellungsregion nach einem Failover aus der primären Region in die Notfallwiederherstellungsregion automatisch geschützt?
Nein, wenn Sie ein [Failover](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) für Azure-VMs von einer Region in eine andere durchführen, werden die VMs in der Notfallwiederherstellungsregion in einem ungeschützten Zustand gestartet. Für ein Failback der VMs in die primäre Region müssen Sie die VMs in der sekundären Region [erneut schützen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect).

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-secondary-region-to-primary-region"></a>Repliziert Site Recovery zum Zeitpunkt des erneuten Schutzes alle Daten aus der sekundären Region in die primäre Region?
Dies hängt von der Situation ab. Wenn zum Beispiel die VM der Quellregion vorhanden ist, werden nur Änderungen zwischen dem Quelldatenträger und dem Zieldatenträger synchronisiert. Die Unterschiede werden durch einen Vergleich der beiden Datenträger ermittelt und dann übertragen. Dieser Vorgang dauert meist einige Stunden. Weitere Informationen zum erneuten Schutz finden Sie in diesem [Artikel]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).

### <a name="how-much-time-does-it-take-to-failback"></a>Wie lange dauert ein Failback?
Nach Abschluss des erneuten Schutzes ähnelt die verstrichene Zeitspanne meist der eines Failovers von der primären Region in die sekundäre Region. 

## <a name="security"></a>Sicherheit
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Werden Replikationsdaten an den Site Recovery-Dienst gesendet?
Nein, Site Recovery fängt replizierte Daten nicht ab und besitzt keine Informationen dazu, was auf Ihren virtuellen Computern ausgeführt wird. Nur die Metadaten, die zum Orchestrieren von Replikation und Failover erforderlich sind, werden an den Site Recovery-Dienst gesendet.  
Site Recovery ist nach ISO 27001:2013, 27018, HIPAA und DPA zertifiziert und durchläuft gerade die Prüfungen für SOC2 und FedRAMP JAB.

### <a name="does-site-recovery-encrypt-replication"></a>Verschlüsselt Site Recovery die Replikation?
Ja, sowohl Verschlüsselung bei der Übertragung als auch [Verschlüsselung in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) wird unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* [Informieren](azure-to-azure-support-matrix.md) Sie sich über die Supportanforderungen.
* [Einrichten](azure-to-azure-tutorial-enable-replication.md) der Azure-zu-Azure-Replikation.

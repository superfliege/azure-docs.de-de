---
title: Allgemeine Fragen – VMware-zu-Azure-Replikation mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel werden häufig gestellte Fragen zum Replizieren lokaler VMware-VMs zu Azure mithilfe von Azure Site Recovery zusammengefasst.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.date: 07/19/2018
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 487e0c763ca4b247f1818b2beaf3282734fc4e27
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388441"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Allgemeine Fragen – VMware-zu-Azure-Replikation

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zum Replizieren lokaler VMware-VMs zu Azure. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Allgemein
### <a name="how-is-site-recovery-priced"></a>Wie werden Site Recovery-Preise kalkuliert?
Nähere Informationen finden Sie unter [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).

### <a name="how-do-i-pay-for-azure-vms"></a>Wie zahle ich für Azure-VMs?
Während der Replikation werden Daten zu Azure-Speicher repliziert, und Sie bezahlen keine VM-Änderungen. Wenn Sie einen Failover zu Azure ausführen, erstellt Site Recovery automatisch Azure-IaaS-VMs. Danach werden Ihnen die Computeressourcen in Rechnung gestellt, die Sie in Azure nutzen.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>Was kann ich mit der VMware-zu-Azure-Replikation tun?
- **Wiederherstellung im Notfall**: Sie können die vollständige Notfallwiederherstellung einrichten. In diesem Szenario replizieren Sie lokale VMware-VMs zu Azure-Speicher. Dann können Sie, wenn Ihre lokale Infrastruktur nicht verfügbar ist, ein Failover zu Azure ausführen. Wenn Sie ein Failover ausführen, werden Azure-VMs aus replizierten Daten erstellt. Sie können auf Apps und Arbeitsauslastungen auf den virtuellen Azure-Computern zugreifen, bis Ihr lokales Rechenzentrum wieder verfügbar ist. Dann können Sie ein Failback aus Azure zu Ihrem lokalen Standort ausführen.
- **Migration**: Sie können mit Site Recovery lokale VMware-VMs zu Azure migrieren. In diesem Szenario replizieren Sie lokale VMware-VMs zu Azure-Speicher. Dann führen Sie ein Failover vom lokalen Standort zu Azure aus. Nach dem Failover sind Ihre Apps und Workloads verfügbar und werden auf virtuellen Azure-Computern ausgeführt.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Was muss ich in Azure tun?
Sie benötigen ein Azure-Abonnement, einen Recovery Services-Tresor, ein Speicherkonto und ein virtuelles Netzwerk. Tresor, Speicherkonto und Netzwerk müssen sich in derselben Region befinden.

### <a name="what-azure-storage-account-do-i-need"></a>Welches Azure-Speicherkonto benötige ich?
Sie benötigen ein LRS- oder GRS-Speicherkonto. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. Storage Premium wird unterstützt.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Benötigt mein Azure-Konto Berechtigungen zum Erstellen von virtuellen Computern?
Wenn Sie ein Abonnementadminstrator sind, besitzen Sie die erforderlichen Replikationsberechtigungen. Wenn nicht, benötigen Sie Berechtigungen zum Erstellen einer Azure-VM in der Ressourcengruppe und dem virtuellen Netzwerk, die Sie beim Konfigurieren von Site Reocvery angeben, und Schreibberechtigungen für das ausgewählte Speicherkonto. [Weitere Informationen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>Lokal

### <a name="what-do-i-need-on-premises"></a>Was benötige ich lokal?
Lokal benötigen Sie Site Recovery-Komponenten, die auf einer einzelnen VMware-VM installiert sind. Sie benötigen auch eine VMware-Infrastruktur mit mindestens einem ESXi-Host, und wir empfehlen einen vCenter-Server. Darüber hinaus benötigen Sie mindestens eine zu replizierende VMware-VM. [Erfahren Sie mehr](vmware-azure-architecture.md) über die VMware-zu-Azure-Architektur.

Der lokale Konfigurationsserver kann mit einer der beiden folgenden Methoden bereitgestellt werden.

1. Stellen Sie ihn mithilfe einer VM-Vorlage bereit, in der der Konfigurationsserver vorinstalliert ist. Weitere Informationen finden Sie [hier](vmware-azure-tutorial.md#download-the-vm-template).
2. Stellen Sie ihn über das Setup auf einem Windows Server 2016-Computer Ihrer Wahl bereit. Weitere Informationen finden Sie [hier](physical-azure-disaster-recovery.md#set-up-the-source-environment).

Wählen Sie zum Ermitteln der ersten Schritte zum Bereitstellen des Konfigurationsservers auf Ihren eigenen Windows Server-Computern im Schutzziel der Schutzaktivierung **In Azure > Nicht virtualisiert/Andere** aus.

### <a name="where-do-on-premises-vms-replicate-to"></a>Zu welchem Speicher werden lokale virtuelle Computer repliziert?
Daten werden zu Azure-Speicher repliziert. Wenn Sie ein Failover zu Azure ausführen, erstellt Site Recovery automatisch Azure-VMs aus dem Speicherkonto.

### <a name="what-apps-can-i-replicate"></a>Welche Apps kann ich replizieren?
Sie können jede App oder Arbeitsauslastung, auf einer VMware-VM ausführen, die mit den [Replikationsanforderungen](vmware-physical-azure-support-matrix.md##replicated-machines) kompatibel ist. Site Recovery bietet Unterstützung für die anwendungsorientierte Replikation, sodass für Apps ein Failover und ein Failback zum „intelligenten Zustand“ ausgeführt werden kann. Site Recovery kann in Microsoft-Anwendungen wie SharePoint, Exchange, Dynamics, SQL Server und Active Directory integriert werden. Zudem kann Site Recovery eng in die Produkte führender Anbieter wie Oracle, SAP, IBM und Red Hat eingebunden werden. [Erfahren Sie mehr](site-recovery-workload.md) über den Schutz von Workloads.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Kann ich über ein Site-to-Site-VPN zu Azure replizieren?
Site Recovery repliziert Daten über einen öffentlichen Endpunkt oder mittels öffentlichem ExpressRoute-Peering aus einem lokalen Speicher zu Azure-Speicher. Replikation über ein Site-to-Site-VPN-Netzwerk wird nicht unterstützt.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Kann ich mit ExpressRoute zu Azure replizieren?
Ja, mit ExpressRoute können VMs zu Azure repliziert werden. Site Recovery repliziert die Daten über einen öffentlichen Endpunkt zu einem Azure Storage-Konto, und Sie müssen [öffentliches Peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) für die Site Recovery-Replikation einrichten. Nach dem Failover virtueller Computer zu einem virtuellen Azure-Netzwerk können Sie mit [privatem Peering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) auf sie zugreifen.


### <a name="why-cant-i-replicate-over-vpn"></a>Warum kann ich nicht über VPN replizieren?

Wenn Sie zu Azure replizieren, erreicht der Replikationsdatenverkehr die öffentlichen Endpunkte eines Azure Storage-Kontos. Das bedeutet Sie können nur über das öffentliche Internet mit ExpressRoute replizieren (öffentliches Peering). VPN ist nicht verfügbar.



## <a name="what-are-the-replicated-vm-requirements"></a>Welche Anforderungen stellt die Replikation an virtuelle Computer?

Für die Replikation muss auf einer VMware-VM ein unterstütztes Betriebssystem ausgeführt werden. Darüber hinaus muss der virtuelle Computer die Anforderungen für Azure-VMs erfüllen. [Erfahren Sie mehr](vmware-physical-azure-support-matrix.md##replicated-machines) in der Unterstützungsmatrix.

## <a name="how-often-can-i-replicate-to-azure"></a>Wie oft kann ich zu Azure replizieren?
Beim Replizieren von VMware-VMs zu Azure ist die Replikation fortlaufend.

## <a name="can-i-extend-replication"></a>Kann ich die Replikation erweitern?
Eine erweiterte oder verkettete Replikation wird nicht unterstützt. Fordern Sie dieses Feature im [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

## <a name="can-i-do-an-offline-initial-replication"></a>Kann ich eine erste Offlinereplikation durchführen?
Dies wird nicht unterstützt. Fordern Sie dieses Feature im [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Kann ich Datenträger ausschließen?
Ja, sie können Datenträger von der Replikation ausschließen.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Kann ich virtuelle Computer mit dynamischen Datenträgern replizieren?
Dynamische Datenträger können repliziert werden. Der Betriebssystem-Datenträger muss ein Basisdatenträger sein.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Wenn ich Replikationsgruppen für die Konsistenz mehrerer VMs verwende, kann ich einer vorhandenen Replikationsgruppe eine neue VM hinzufügen?
Ja, Sie können einer vorhandenen Replikationsgruppe neue VMs hinzufügen, wenn Sie die Replikation für sie aktivieren. Einer bestehenden Replikationsgruppe lassen sich keine VMs hinzufügen, nachdem die Replikation gestartet wurde. Außerdem können Sie keine Replikationsgruppe für vorhandene VMs erstellen.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Kann ich replizierende VMs durch Hinzufügen oder Ändern der Datenträgergröße ändern?

Für die VMware-Replikation in Azure können Sie die Datenträgergröße ändern. Wenn Sie neue Datenträger hinzufügen möchten, müssen Sie den Datenträger hinzufügen und den Schutz für die VM erneut aktivieren.

## <a name="configuration-server"></a>Konfigurationsserver

### <a name="what-does-the-configuration-server-do"></a>Was macht der Konfigurationsserver?
Der Konfigurationsserver führt die lokalen Site Recovery-Komponenten aus, einschließlich:
- Der Konfigurationsserver, der die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeichern, Komprimieren und Verschlüsseln und sendet sie an Azure-Speicher. Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt die automatische Ermittlung von lokalen VMware-VMs durch.
- Der Masterzielserver, der die Replikationsdaten während des Failbacks von Azure verarbeitet.

### <a name="where-do-i-set-up-the-configuration-server"></a>Wo richte ich den Konfigurationsserver ein?
Für den Konfigurationsserver benötigen Sie eine einzelne hoch verfügbare lokale VMware-VM.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Welche Anforderungen gelten für den Konfigurationsserver?

Überprüfen Sie die [Voraussetzungen](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Kann ich den Konfigurationsserver manuell einrichten, statt eine Vorlage zu verwenden?
Sie sollten die neueste Version der OVF-Vorlage verwenden, um [die Konfigurationsserver-VM zu erstellen](vmware-azure-deploy-configuration-server.md). Wenn das aus irgendeinem Grund nicht möglich ist, Sie z.B. keinen Zugriff auf den VMware-Server haben, können Sie aus dem Portal die [einheitliche Setupdatei](physical-azure-set-up-source.md) herunterladen und einem virtuellen Computer ausführen.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Kann ein Konfigurationsserver zu mehreren Regionen replizieren?
Nein. Zu diesem Zweck müssen Sie in jeder Region eine Konfigurationsserver einrichten.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Kann ich einen Konfigurationsserver in Azure hosten?
Dies ist zwar möglich, aber die Azure-VM, die den Konfigurationsserver ausführt, müsste mit Ihrer lokalen VMware-Infrastruktur und den virtuellen Computern kommunizieren. Dies kann zu höherer Latenz führen und sich auf die laufende Replikation auswirken.


### <a name="where-can-i-get-the-latest-version-of-the-configuration-server-template"></a>Wo erhalte ich die neueste Version der Konfigurationsservervorlage?
Laden Sie die neueste Version aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunter.

### <a name="how-do-i-update-the-configuration-server"></a>Wie aktualisiere ich den Konfigurationsserver?
Sie installieren Updaterollups. Die neuesten Updateinformationen finden Sie auf der [Wiki-Updatesseite](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

### <a name="should-i-backup-the-deployed-configuration-server"></a>Soll ich den bereitgestellten Konfigurationsserver sichern?
Es wird empfohlen, regelmäßige geplante Sicherungen des Konfigurationsservers durchzuführen. Für ein erfolgreiches Failback muss der betreffende virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, und der Konfigurationsserver muss ausgeführt werden und verbunden sein. Weitere Informationen zu allgemeinen Aufgaben für die Verwaltung des Konfigurationsservers finden Sie [hier](vmware-azure-manage-configuration-server.md).

## <a name="mobility-service"></a>Mobilitätsdienst

### <a name="where-can-i-find-the-mobility-service-installers"></a>Wo finde ich die Mobility Service-Installationsprogramme?
Die Installationsprogramme befinden sich im **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**-Ordner auf dem Konfigurationsserver.

## <a name="how-do-i-install-the-mobility-service"></a>Wie installiere ich den Mobility Service?
Sie installieren ihn auf jedem virtuellen Computer, den Sie replizieren möchten, mithilfe einer [Pushinstallation](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery), manuellen Installation in [der Benutzeroberfläche](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui) oder [PowerShell](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt). Alternativ können Sie ein Bereitstellungstool wie [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md) oder [Azure Automation und DSC](vmware-azure-mobility-deploy-automation-dsc.md) verwenden.



## <a name="security"></a>Sicherheit

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Welchen Zugriff auf VMware-Server benötigt Site Recovery?
Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Richten Sie eine VMware-VM ein, die den Konfigurationsserver und andere lokale Site Recovery-Komponenten ausführt. [Weitere Informationen](vmware-azure-deploy-configuration-server.md)
- Ermitteln Sie automatisch virtuelle Computer für die Replikation. Informieren Sie sich über die Vorbereitung eines Kontos für die automatische Ermittlung. [Weitere Informationen](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Welchen Zugriff auf VMware-VMs benötigt Site Recovery?

- Zum Replizieren muss auf einer VMware-VM der Site Recovery Mobility Service installiert sein und ausgeführt werden. Sie können das Tool manuell bereitstellen oder angeben, dass Site Recovery eine Pushinstallation des Diensts ausführen soll, wenn Sie die Replikation für einen virtuellen Computer aktivieren. Für die Pushinstallation benötigt Site Recovery ein Konto, das zum Installieren der Dienstkomponente verwendet werden kann. [Weitere Informationen](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). Der (standardmäßig auf dem Konfigurationsserver ausgeführte) Prozessserver führt diese Installation durch. [Erfahren Sie mehr](vmware-azure-install-mobility-service.md) über die Mobility Service-Installation.
- Während der Replikation kommunizieren virtuelle Computer wie folgt mit Site Recovery:
    - VMs kommunizieren mit dem Konfigurationsserver über Port HTTPS 443 für die Replikationsverwaltung.
    - VMs senden Replikationsdaten über Port HTTPS 9443 an den Prozessserver (Konfiguration möglich).
    - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren VMs über den Port 20004 miteinander.


### <a name="is-replication-data-sent-to-site-recovery"></a>Werden Replikationsdaten an Site Recovery gesendet?
Nein. Site Recovery fängt replizierte Daten nicht ab und besitzt keine Informationen dazu, was auf Ihren virtuellen Computern ausgeführt wird. Replikationsdaten werden zwischen VMware-Hypervisoren und Azure-Speicher ausgetauscht. Site Recovery hat keine Möglichkeit, diese Daten abzufangen. Nur die Metadaten, die zum Orchestrieren von Replikation und Failover erforderlich sind, werden an den Site Recovery-Dienst gesendet.  

Site Recovery ist nach ISO 27001:2013, 27018, HIPAA und DPA zertifiziert und durchläuft gerade die Prüfungen für SOC2 und FedRAMP JAB.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Können wir lokale Metadaten innerhalb geografischer Regionen halten?
Ja. Wenn Sie einen Tresor in einer Region erstellen, stellen wir sicher, dass alle von Site Recovery verwendeten Metadaten innerhalb der geografischen Begrenzung der Region bleiben.

### <a name="does-site-recovery-encrypt-replication"></a>Verschlüsselt Site Recovery die Replikation?
Ja, sowohl Verschlüsselung bei der Übertragung als auch [Verschlüsselung in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) wird unterstützt.


## <a name="failover-and-failback"></a>Failover und Failback
### <a name="how-far-back-can-i-recover"></a>Wie weit kann ich bei der Wiederherstellung zurückgehen?
Für VMware zu Azure beträgt der älteste Wiederherstellungspunkt, den Sie verwenden können, 72 Stunden.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Wie greife ich nach einem Failover auf virtuelle Azure-Computer zu?
Nach einem Failover können Sie über eine sichere Internetverbindung, eine Site-to-Site-VPN-Verbindung oder über Azure ExpressRoute auf die Azure-VMs zugreifen. Sie müssen eine Reihe von Vorbereitungen treffen, um eine Verbindung herzustellen. [Weitere Informationen](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Sind Daten, mit denen ein Failover ausgeführt wurde, stabil?
Azure ist auf Resilienz ausgelegt. Site Recovery ist für ein Failover zu einem sekundären Azure-Rechenzentrum (gemäß Azure-SLA) konzipiert. Wenn ein Failover auftritt, stellen wir sicher, dass Ihre Metadaten und Tresore innerhalb der gleichen geografischen Region bleiben, die Sie für Ihren Tresor ausgewählt haben.

### <a name="is-failover-automatic"></a>Erfolgt ein Failover automatisch?
Ein [Failover](site-recovery-failover.md) erfolgt nicht automatisch. Sie initiieren Failover mit einem Mausklick im Portal, oder Sie können [PowerShell](/powershell/module/azurerm.siterecovery) verwenden, um ein Failover auslösen.

### <a name="can-i-fail-back-to-a-different-location"></a>Kann ich ein Failback zu einem anderen Speicherort ausführen?
Ja, wenn Sie ein Failover zu Azure ausgeführt haben, können Sie ein Failback zu einem anderen Speicherort ausführen, wenn der ursprüngliche nicht verfügbar ist. [Weitere Informationen](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Warum benötige ich ein VPN oder ExpressRoute für ein Failback?

Wenn Sie ein Failback von Azure ausführen, werden Daten aus Azure auf Ihren lokalen virtuellen Computer zurückkopiert und privater Zugriff ist erforderlich.



## <a name="automation-and-scripting"></a>Automatisierung und Skripterstellung

### <a name="can-i-set-up-replication-with-scripting"></a>Kann ich die Replikation mit Skripts einrichten?
Ja. Sie können Site Recovery-Workflows mithilfe von REST-API, PowerShell oder Azure SDK automatisieren. [Hier erfahren Sie mehr](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Leistung und Kapazität
### <a name="can-i-throttle-replication-bandwidth"></a>Kann ich die Replikationsbandbreite drosseln?
Ja. [Weitere Informationen](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Nächste Schritte
* [Informieren](vmware-physical-azure-support-matrix.md) Sie sich über die Supportanforderungen.
* [Richten Sie](vmware-azure-tutorial.md) die VMware-zu-Azure-Replikation ein.

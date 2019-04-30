---
title: 'Allgemeine Fragen: VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery | Microsoft-Dokumentation'
description: In diesem Artikel werden häufig gestellte Fragen zur Notfallwiederherstellung von lokalen virtuellen VMware-Computern in Azure mithilfe von Azure Site Recovery zusammengefasst.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 04/18/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: d0e39f9e24b3c486eccd71eb1c19823cfd33391a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60004770"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Allgemeine Fragen – VMware-zu-Azure-Replikation

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zum Bereitstellen der Notfallwiederherstellung von lokalen virtuellen VMware-Computern in Azure. 

## <a name="general"></a>Allgemein 
### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>Was benötige ich für die Notfallwiederherstellung von virtuellen VMware-Computern?

[Hier](vmware-azure-architecture.md) finden Sie Informationen zu den Komponenten der Notfallwiederherstellung von virtuellen VMware-Computern. 

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>Kann ich Site Recovery für die Migration von virtuellen VMware-Computern zu Azure verwenden?

Ja, neben der Verwendung von Site Recovery zum Einrichten der vollständigen Notfallwiederherstellung für virtuelle VMware-Computer können Sie mit Site Recovery die Migration von lokalen virtuellen VMware-Computern zu Azure durchführen. In diesem Szenario replizieren Sie lokale VMware-VMs zu Azure-Speicher. Dann führen Sie ein Failover vom lokalen Standort zu Azure aus. Nach dem Failover sind Ihre Apps und Workloads verfügbar und werden auf virtuellen Azure-Computern ausgeführt. Der Vorgang ähnelt dem Einrichten der vollständigen Notfallwiederherstellung, mit der Ausnahme, dass in einer Migration kein Failback von Azure ausgeführt werden kann.


### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Benötigt mein Azure-Konto Berechtigungen zum Erstellen von virtuellen Computern?
Wenn Sie ein Abonnementadminstrator sind, besitzen Sie die erforderlichen Replikationsberechtigungen. Wenn nicht, benötigen Sie Berechtigungen zum Erstellen einer Azure-VM in der Ressourcengruppe und dem virtuellen Netzwerk, die Sie beim Konfigurieren von Site Recovery angeben, und je nach Konfiguration Schreibberechtigungen für das ausgewählte Speicherkonto oder den verwalteten Datenträger. [Weitere Informationen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)

### <a name="what-applications-can-i-replicate"></a>Welche Anwendungen kann ich replizieren?
Sie können jede App oder Arbeitsauslastung, auf einer VMware-VM ausführen, die mit den [Replikationsanforderungen](vmware-physical-azure-support-matrix.md##replicated-machines) kompatibel ist.
- Site Recovery bietet Unterstützung für die anwendungsorientierte Replikation, sodass für Apps ein Failover und ein Failback zum „intelligenten Zustand“ ausgeführt werden kann.
- Site Recovery kann in Microsoft-Anwendungen wie SharePoint, Exchange, Dynamics, SQL Server und Active Directory integriert werden. Zudem kann Site Recovery eng in die Produkte führender Anbieter wie Oracle, SAP, IBM und Red Hat eingebunden werden.
- [Erfahren Sie mehr](site-recovery-workload.md) über den Schutz von Workloads.

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>Kann ich eine Gastbetriebssystem-Serverlizenz in Azure verwenden?
Ja, Microsoft Software Assurance-Kunden können den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) nutzen, um Lizenzierungskosten für **Windows Server-Computer** zu sparen, die zu Azure migriert werden, oder um Azure für die Notfallwiederherstellung zu verwenden.

## <a name="security"></a>Sicherheit

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Welchen Zugriff auf VMware-Server benötigt Site Recovery?
Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Einrichten eines virtuellen VMware-Computers, auf dem der Site Recovery-Konfigurationsserver ausgeführt wird
- Ermitteln Sie automatisch virtuelle Computer für die Replikation. 


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Welchen Zugriff auf VMware-VMs benötigt Site Recovery?

- Zum Replizieren muss auf einer VMware-VM der Site Recovery Mobility Service installiert sein und ausgeführt werden. Sie können das Tool manuell bereitstellen oder angeben, dass Site Recovery eine Pushinstallation des Diensts ausführen soll, wenn Sie die Replikation für einen virtuellen Computer aktivieren. 
- Während der Replikation kommunizieren virtuelle Computer wie folgt mit Site Recovery:
    - VMs kommunizieren mit dem Konfigurationsserver über Port HTTPS 443 für die Replikationsverwaltung.
    - VMs senden Replikationsdaten über Port HTTPS 9443 an den Prozessserver (Konfiguration möglich).
    - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren VMs über den Port 20004 miteinander.


### <a name="is-replication-data-sent-to-site-recovery"></a>Werden Replikationsdaten an Site Recovery gesendet?
Nein. Site Recovery fängt replizierte Daten nicht ab und besitzt keine Informationen dazu, was auf Ihren virtuellen Computern ausgeführt wird. Replikationsdaten werden zwischen VMware-Hypervisoren und Azure-Speicher ausgetauscht. Site Recovery hat keine Möglichkeit, diese Daten abzufangen. Nur die Metadaten, die zum Orchestrieren von Replikation und Failover erforderlich sind, werden an den Site Recovery-Dienst gesendet.  

Site Recovery ist nach ISO 27001:2013, 27018, HIPAA und DPA zertifiziert und durchläuft gerade die Prüfungen für SOC2 und FedRAMP JAB.


## <a name="pricing"></a>Preise
### <a name="how-can-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>Wie kann ich die ungefähren Kosten für die VMware-Notfallwiederherstellung berechnen?

Sie können den [Preisrechner](https://aka.ms/asr_pricing_calculator) verwenden, um die Kosten für die Verwendung von Site Recovery zu schätzen.

Für eine detaillierte Kostenschätzung können Sie das Bereitstellungsplanertool für [VMware](https://aka.ms/siterecovery_deployment_planner) ausführen und den [Kostenschätzungsbericht](https://aka.ms/asr_DP_costreport) verwenden.

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>Besteht ein Unterschied zwischen der Replikation im Speicher und direkt in verwalteten Datenträgern?

Die Berechnung der Kosten für verwaltete Datenträger unterscheidet sich etwas von Speicherkonten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/managed-disks/) zu den Preisen für verwaltete Datenträger.

## <a name="mobility-service"></a>Mobilitätsdienst

### <a name="where-can-i-find-the-mobility-service-installers"></a>Wo finde ich die Mobility Service-Installationsprogramme?
Die Installationsprogramme befinden sich im Ordner **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** auf dem Konfigurationsserver.

## <a name="how-do-i-install-the-mobility-service"></a>Wie installiere ich den Mobility Service?
Sie installieren den Dienst auf jedem virtuellen Computer, den Sie replizieren möchten, mithilfe verschiedener Methoden:
- [Pushinstallation](vmware-physical-mobility-service-overview.md#push-installation)
- [Manuelle Installation](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) über die Benutzeroberfläche oder PowerShell.
- Bereitstellung mithilfe eines Bereitstellungstools wie z. B. [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)

## <a name="managed-disks"></a>Verwaltete Datenträger

### <a name="where-does-site-recovery-replicate-data-to"></a>Wo werden Daten mithilfe von Site Recovery repliziert?

Site Recovery repliziert lokale virtuelle VMware-Computer und physische Server in verwalteten Datenträgern in Azure.
- Der Site Recovery-Prozessserver schreibt Replikationsprotokolle in ein Cachespeicherkonto in der Zielregion.
- Diese Protokolle werden verwendet, um Wiederherstellungspunkte in den verwalteten Datenträgern zu erstellen.
- Bei einem Failover wird der ausgewählte Wiederherstellungspunkt verwendet, um den verwalteten Zieldatenträger zu erstellen.
- Virtuelle Computer, die zuvor in einem Speicherkonto repliziert wurden (vor März 2019), sind nicht betroffen.


### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>Kann ich neue Computer in Speicherkonten replizieren?

Nein, ab März 2019 können Sie im Portal die Replikation nur in verwalteten Azure-Datenträgern durchführen. 

Die Replikation von neuen virtuellen Computern in einem Speicherkonto ist nur über PowerShell oder die REST-API (Version 2018-01-10 oder 2016-08-10) verfügbar.

### <a name="what-are-the-benefits-in-replicating-to-managed-disks"></a>Welche Vorteile hat die Replikation auf verwalteten Datenträgern?

[Hier](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) finden Sie Informationen dazu, wie die Notfallwiederherstellung mit verwalteten Datenträgern mithilfe von Site Recovery vereinfacht wird.


### <a name="can-i-change-the-managed-disk-type-after-machine-is-protected"></a>Kann ich den Typ der verwalteten Datenträger ändern, nachdem der Computer geschützt wird?

Ja, Sie können [den Typ des verwalteten Datenträgers mühelos ändern](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage). Bevor Sie den Typ ändern, stellen Sie sicher, dass Sie die SAS-URL für den Datenträger widerrufen, indem Sie im Azure-Portal zur Managed Disk-Ressource navigieren. Brechen Sie fortlaufenden Export ggf. im Übersichtsblatt ab. Nachdem Sie die SAS-URL widerrufen haben, ändern Sie den Typ des Datenträgers innerhalb der nächsten paar Minuten. Wenn Sie den Typ des verwalteten Datenträgers ändern, warten Sie jedoch, bis Azure Site Recovery neue Wiederherstellungspunkte generiert hat. Verwenden Sie die neuen Wiederherstellungspunkte für zukünftige Testfailover oder Failover.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>Kann ich die Replikation von verwalteten Datenträgern in nicht verwaltete Datenträger ändern?

Nein, das Wechseln von verwalteten zu nicht verwalteten Datenträgern wird nicht unterstützt.

## <a name="replication"></a>Replikation


### <a name="what-are-the-replicated-vm-requirements"></a>Welche Anforderungen stellt die Replikation an virtuelle Computer?

[Hier](vmware-physical-azure-support-matrix.md##replicated-machines) finden Sie Informationen zu Anforderungen für virtuelle VMware-Computer und physische Server und zur Unterstützung von diesen.

### <a name="how-often-can-i-replicate-to-azure"></a>Wie oft kann ich zu Azure replizieren?
Beim Replizieren von VMware-VMs zu Azure ist die Replikation fortlaufend.


### <a name="can-i-extend-replication"></a>Kann ich die Replikation erweitern?
Eine erweiterte oder verkettete Replikation wird nicht unterstützt. Fordern Sie dieses Feature im [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>Kann ich eine erste Offlinereplikation durchführen?
Dies wird nicht unterstützt. Fordern Sie dieses Feature im [Feedbackforum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks-from-replication"></a>Kann ich Datenträger von der Replikation ausschließen?
Ja, Sie können Datenträger ausschließen.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Kann ich virtuelle Computer mit dynamischen Datenträgern replizieren?
Dynamische Datenträger können repliziert werden. Der Betriebssystem-Datenträger muss ein Basisdatenträger sein.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Wenn ich Replikationsgruppen für die Konsistenz mehrerer VMs verwende, kann ich einer vorhandenen Replikationsgruppe eine neue VM hinzufügen?
Ja, Sie können einer vorhandenen Replikationsgruppe neue VMs hinzufügen, wenn Sie die Replikation für sie aktivieren.
- Nach dem Initiieren der Replikation können Sie einer vorhandenen Replikationsgruppe keinen virtuellen Computer mehr hinzufügen.
- Sie können keine Replikationsgruppe für vorhandene virtuelle Computer erstellen.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Kann ich replizierende VMs durch Hinzufügen oder Ändern der Datenträgergröße ändern?

Für die VMware-Replikation in Azure können Sie die Datenträgergröße ändern. Wenn Sie neue Datenträger hinzufügen möchten, müssen Sie den Datenträger hinzufügen und den Schutz für die VM erneut aktivieren.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>Kann ich lokale Computer zu einem neuen vCenter-Server migrieren, ohne die laufende Replikation zu beeinträchtigen?
Nein, eine Änderung des Vcenter oder der Migration beeinträchtigt die laufende Replikation. Sie müssen Site Recovery mit dem neuen vCenter-Server einrichten und die Replikation für Computer erneut aktivieren.

### <a name="can-i-replicate-to-a-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>Kann ich die Replikation in einem Cache oder einem Zielspeicherkonto durchführen, für den bzw. das ein VNET (mit Azure Storage-Firewalls) konfiguriert ist?
Nein, Site Recovery unterstützt keine Replikation im Speicher im VNET.





## <a name="component-upgrade"></a>Upgrade von Komponenten

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>Meine Version des Mobility Services-Agent oder des Konfigurationsservers ist alt, und beim Upgrade sind Fehler aufgetreten. Wie gehe ich vor?  

Für Site Recovery gilt das Supportmodell N-4. [Hier](https://aka.ms/asr_support_statement) finden Sie Informationen zum Upgrade von sehr alten Versionen.

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>Wo finde ich die Versionshinweise/Updaterollups von Azure Site Recovery?

[Hier](site-recovery-whats-new.md) finden Sie Informationen zu neuen Updates und [hier](service-updates-how-to.md) zum Rollup.

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>Wo finde ich Informationen zum Upgrade für die Notfallwiederherstellung in Azure?

Informationen zum Durchführen von Upgrades finden Sie [hier](https://aka.ms/asr_vmware_upgrades).

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>Muss ich die Quellcomputer für jedes Upgrade neu starten?

Ein Neustart wird zwar empfohlen, ist jedoch nicht für jedes Upgrade erforderlich. [Weitere Informationen](https://aka.ms/asr_vmware_upgrades)


## <a name="configuration-server"></a>Konfigurationsserver

### <a name="what-does-the-configuration-server-do"></a>Was macht der Konfigurationsserver?

Der Konfigurationsserver führt die lokalen Site Recovery-Komponenten aus, einschließlich:
- Den Konfigurationsserver selbst, der die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Der Prozessserver führt auch eine Pushinstallation des Mobility Service auf virtuellen Computern sowie eine automatische Ermittlung auf lokalen virtuellen VMware-Computern durch.
- Der Masterzielserver, der die Replikationsdaten während des Failbacks von Azure verarbeitet.

[Erfahren Sie mehr](vmware-azure-architecture.md) zu den Komponenten und Prozessen von Konfigurationsservern.

### <a name="where-do-i-set-up-the-configuration-server"></a>Wo richte ich den Konfigurationsserver ein?
Für den Konfigurationsserver benötigen Sie eine einzelne hoch verfügbare lokale VMware-VM. Für die Notfallwiederherstellung von physischen Servern können Sie den Konfigurationsserver auf einem physischen Computer installieren.

### <a name="what-do-i-need-for-the-configuration-server"></a>Was benötige ich für den Konfigurationsserver?

Überprüfen Sie die [Voraussetzungen](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Kann ich den Konfigurationsserver manuell einrichten, statt eine Vorlage zu verwenden?
Es empfiehlt sich, [den virtuellen Computer des Konfigurationsservers](vmware-azure-deploy-configuration-server.md) mit der neuesten Version der OVF-Vorlage zu erstellen. Wenn das aus einem bestimmten Grund nicht möglich ist, Sie z. B. keinen Zugriff auf den VMware-Server haben, können Sie die Setupdatei über das Portal [herunterladen](physical-azure-set-up-source.md) und den Konfigurationsserver einrichten.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Kann ein Konfigurationsserver zu mehreren Regionen replizieren?
 Nein. Zu diesem Zweck benötigen Sie einen Konfigurationsserver in jeder Region.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Kann ich einen Konfigurationsserver in Azure hosten?
Dies ist zwar möglich, aber die Azure-VM, die den Konfigurationsserver ausführt, müsste mit Ihrer lokalen VMware-Infrastruktur und den virtuellen Computern kommunizieren. Dies führt zu höherer Latenz und wirkt sich auf die laufende Replikation aus.

### <a name="how-do-i-update-the-configuration-server"></a>Wie aktualisiere ich den Konfigurationsserver?

[Hier](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) finden Sie Informationen zum Aktualisieren des Konfigurationsservers.
- Die neuesten Updateinformationen finden Sie auf der [Seite mit den Azure-Updates](https://azure.microsoft.com/updates/?product=site-recovery).
- Sie können die neueste Version über das Portal herunterladen. Alternativ können Sie die neueste Version des Konfigurationsservers direkt vom [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.
- Wenn zwischen Ihrer Version und der aktuellen Version mehr als vier Versionen liegen, finden Sie Upgradeanleitungen in unserem [Supporthinweis](https://aka.ms/asr_support_statement).

### <a name="should-i-back-up-the-configuration-server"></a>Sollte ich den Konfigurationsserver sichern?
Es wird empfohlen, regelmäßige geplante Sicherungen des Konfigurationsservers durchzuführen.
- Für ein erfolgreiches Failback muss der betreffende virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein.
- Der Konfigurationsserver muss ausgeführt werden und verbunden sein.
- Weitere Informationen zu allgemeinen Aufgaben für die Verwaltung des Konfigurationsservers finden Sie [hier](vmware-azure-manage-configuration-server.md).

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Kann ich beim Einrichten des Konfigurationsservers MySQL herunterladen und manuell installieren?

Ja. Laden Sie MySQL herunter, und speichern Sie die Daten im Ordner **C:\Temp\ASRSetup**. Führen Sie die Installation anschließend manuell durch. Wenn Sie die Konfigurationsserver-VM einrichten und die Bedingungen akzeptieren, wird MySQL unter **Herunterladen und installieren** als **Bereits installiert** aufgeführt.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Kann ich das Herunterladen von MySQL vermeiden und Site Recovery die Installation überlassen?

Ja. Laden Sie das MySQL-Installationsprogramm herunter, und speichern Sie es im Ordner **C:\Temp\ASRSetup**.  Akzeptieren Sie beim Einrichten der Konfigurationsserver-VM die Bedingungen, und klicken Sie auf **Herunterladen und installieren**. Im Portal wird dann das von Ihnen hinzugefügte Installationsprogramm verwendet, um MySQL zu installieren.
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Kann ich die Konfigurationsserver-VM für andere Zwecke nutzen?
Nein. Sie sollten die VM nur für den Konfigurationsserver verwenden. 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Kann ich einen Konfigurationsserver klonen und für die Orchestrierung verwenden?
Nein, Sie sollten einen neuen Konfigurationsserver einrichten, um Registrierungsprobleme zu vermeiden.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>Kann ich den Tresor ändern, in dem der Konfigurationsserver registriert ist?
 Nein. Nachdem dem Konfigurationsserver ein Tresor zugeordnet wurde, sind daran keine Änderungen mehr möglich. [In diesem Artikel](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) finden Sie Informationen zur erneuten Registrierung.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Kann ich denselben Konfigurationsserver für die Notfallwiederherstellung von VMware-VMs und von physischen Servern nutzen?
Ja. Beachten Sie aber, dass für den physischen Computer nur ein Failback auf einen virtuellen VMware-Computer durchgeführt werden kann.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Wo kann ich die Passphrase für den Konfigurationsserver herunterladen?
[Hier](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) finden Sie Informationen zum Herunterladen der Passphrase.

### <a name="where-can-i-download-vault-registration-keys"></a>Wo kann ich die Tresorregistrierungsschlüssel herunterladen?

Klicken Sie im Recovery Services-Tresor unter **Verwalten** > **Site Recovery-Infrastruktur** auf **Konfigurationsserver**. Wählen Sie dann unter **Server** die Option **Registrierungsschlüssel herunterladen** aus, um die Datei mit den Tresoranmeldeinformationen herunterzuladen.







## <a name="failover-and-failback"></a>Failover und Failback
### <a name="can-i-use-the-process-server-at-on-premises-for-failback"></a>Kann ich den lokalen Verarbeitungsserver für Failbacks verwenden?
Es wird dringend empfohlen, einen Prozessserver für Failbacks in Azure zu erstellen, um Wartezeiten bei der Datenübertragung zu vermeiden. Es ist außerdem wichtig, dass Sie den in Azure erstellten Prozessserver für Failbacks verwenden, wenn Sie das Quellnetzwerk der virtuellen Computer vom mit Azure verbundenen Netzwerk auf dem Konfigurationsserver getrennt haben.

### <a name="can-i-retain-the-ip-address-on-failover"></a>Kann ich die IP-Adresse beim Failover behalten?
Ja, Sie können die IP-Adresse beim Failover behalten. Stellen Sie sicher, dass Sie die Ziel-IP-Adresse vor dem Failover in den Einstellungen für „Compute und Netzwerk“ für den virtuellen Computer angeben. Fahren Sie außerdem die Computer zum Zeitpunkt des Failovers herunter, um Konflikte der IP-Adressen beim Failback zu vermeiden.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Kann ich die Größe oder den Typ der Ziel-VM vor dem Failover ändern?
Ja, Sie können den Typ oder die Größe des virtuellen Computers jederzeit vor dem Failover in den Compute- und Netzwerkeinstellungen des replizierten virtuellen Computers im Portal ändern.

### <a name="how-far-back-can-i-recover"></a>Wie weit kann ich bei der Wiederherstellung zurückgehen?
Für VMware zu Azure beträgt der älteste Wiederherstellungspunkt, den Sie verwenden können, 72 Stunden.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Wie greife ich nach einem Failover auf virtuelle Azure-Computer zu?
Nach einem Failover können Sie über eine sichere Internetverbindung, eine Site-to-Site-VPN-Verbindung oder über Azure ExpressRoute auf die Azure-VMs zugreifen. Sie müssen eine Reihe von Vorbereitungen treffen, um eine Verbindung herzustellen. [Weitere Informationen](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Sind Daten, mit denen ein Failover ausgeführt wurde, stabil?

Azure ist auf Resilienz ausgelegt. Site Recovery ist für ein Failover zu einem sekundären Azure-Rechenzentrum (gemäß Azure-SLA) konzipiert. Wenn ein Failover auftritt, stellen wir sicher, dass Ihre Metadaten und Tresore innerhalb der gleichen geografischen Region bleiben, die Sie für Ihren Tresor ausgewählt haben.

### <a name="is-failover-automatic"></a>Erfolgt ein Failover automatisch?
Ein [Failover](site-recovery-failover.md) erfolgt nicht automatisch. Sie initiieren Failover mit einem Mausklick im Portal, oder Sie können [PowerShell](/powershell/module/az.recoveryservices) verwenden, um ein Failover auszulösen.

### <a name="can-i-fail-back-to-a-different-location"></a>Kann ich ein Failback zu einem anderen Speicherort ausführen?
Ja, wenn Sie ein Failover zu Azure ausgeführt haben, können Sie ein Failback zu einem anderen Speicherort ausführen, wenn der ursprüngliche nicht verfügbar ist. [Weitere Informationen](concepts-types-of-failback.md#alternate-location-recovery-alr)

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Warum benötige ich ein VPN oder ExpressRoute für ein Failback?
Wenn Sie ein Failback von Azure ausführen, werden Daten aus Azure auf Ihren lokalen virtuellen Computer zurückkopiert und privater Zugriff ist erforderlich.

### <a name="can-i-resize-the-azure-vm-after-failover"></a>Kann ich die Größe der Azure-VM nach einem Failover ändern?
Nein, Sie können die Größe oder den Typ der Ziel-VM nach dem Failover nicht ändern.


## <a name="automation-and-scripting"></a>Automatisierung und Skripterstellung

### <a name="can-i-set-up-replication-with-scripting"></a>Kann ich die Replikation mit Skripts einrichten?
Ja. Sie können Site Recovery-Workflows mithilfe von REST-API, PowerShell oder Azure SDK automatisieren. [Hier erfahren Sie mehr](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Leistung und Kapazität
### <a name="can-i-throttle-replication-bandwidth"></a>Kann ich die Replikationsbandbreite drosseln?
Ja. [Weitere Informationen](site-recovery-plan-capacity-vmware.md)


## <a name="next-steps"></a>Nächste Schritte
* [Informieren](vmware-physical-azure-support-matrix.md) Sie sich über die Supportanforderungen.
* [Richten Sie](vmware-azure-tutorial.md) die VMware-zu-Azure-Replikation ein.

---
title: Rehost-Migrate und Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Zuweisen eines neuen Hosts für eine lokale Linux-App durch Migration zu Azure-VMs.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 8f039884ca0ea46c128078984d6cab6fd29ac9af
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220549"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Contoso-Migration: Zuweisen eines neuen Hosts für eine lokale Linux-App zu Azure-VMs

Dieser Artikel enthält Informationen zum Zuweisen eines neuen Hosts für eine lokale, auf Linux basierende Service Desk-App (**osTicket**) zu Azure-IaaS-VMs.

Dieses Dokument ist das siebte in einer Reihe von Artikeln, die dokumentieren, wie das fiktive Unternehmen Contoso seine lokalen Ressourcen zur Microsoft Azure-Cloud migriert. Die Reihe enthält Hintergrundinformationen und einige Szenarios, die veranschaulichen, wie die Infrastruktur einer Migration eingerichtet wird und wie verschiedene Migrationstypen ausgeführt werden. Die Szenarios werden an Komplexität zunehmen, und mit der Zeit werden zusätzliche Artikel hinzugefügt.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Übersicht](contoso-migration-overview.md) | Bietet eine Übersicht über die Migrationsstrategie von Contoso, die Artikelreihe und die Beispiel-Apps, die wir verwenden. | Verfügbar
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](contoso-migration-infrastructure.md) | Beschreibt, wie Contoso die lokale Infrastruktur und die Azure-Infrastruktur für die Migration vorbereitet. Für sämtliche Contoso-Migrationsszenarien wird dieselbe Infrastruktur verwendet. | Verfügbar
[Artikel 3: Bewerten von lokalen Ressourcen](contoso-migration-assessment.md)  | Zeigt, wie Contoso eine Bewertung der lokalen, zweischichtigen, unter VMware ausgeführten App SmartHotel durchführt. Sie bewerten App-VMs mit dem Dienst [Azure Migrate](migrate-overview.md) und die App SQL Server-Datenbank mit dem [Datenbankmigrations-Assistent von Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Verfügbar
[Artikel 4: Zuweisen eines neuen Hosts zu Azure-VMs und einer verwalteten SQL-Instanz](contoso-migration-rehost-vm-sql-managed-instance.md) | Veranschaulicht, wie Contoso die App SmartHotel zu Azure migriert. Das Unternehmen migriert die Front-End-VM der App mithilfe von [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), und verwendet die App-Datenbank und den Dienst [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview), um zu einer verwalteten SQL-Instanz zu migrieren. | Verfügbar
[Artikel 5: Zuweisen eines neuen Hosts zu Azure-VMs](contoso-migration-rehost-vm.md) | Veranschaulicht, wie Contoso die VMs der App SmartHotel nur mit SiteRecovery migriert.
[Artikel 6: Zuweisen von Azure-VMs und SQL Server-Verfügbarkeitsgruppen als neue Hosts](contoso-migration-rehost-vm-sql-ag.md) | Veranschaulicht, wie Contoso die App SmartHotel migriert. Das Unternehmen verwendet Site Recovery zum Migrieren der App-VMs und den Database Migration Service zum Migrieren der App-Datenbank zu einer SQL Server-Verfügbarkeitsgruppe. | Verfügbar
Artikel 7: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs (in diesem Artikel) | Dieser Artikel enthält Informationen darüber, wie Contoso die Linux-App osService mithilfe von Azure Site Recovery migriert.
[Artikel 8: Zuweisen eines neuen Hosts für eine Linux-App zu Azure-VMs und Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Veranschaulicht, wie Contoso die Linux-App osService mithilfe von Site Recovery und MySQL Workbench für die Migration (zu einer Azure MySQL Server-Instanz) migriert. | Verfügbar

In diesem Artikel migriert Contoso die zweischichtige App **osTicket** zu Azure, die unter Linux Apache MySQL PHP (LAMP) ausgeführt wird. Die App-VMs werden mithilfe des Diensts Azure Site Recovery migriert. Wenn Sie diese Open Source-App verwenden möchten, können Sie sie von [GitHub](https://github.com/osTicket/osTicket) herunterladen.

## <a name="business-drivers"></a>Business-Treiber

Das IT Leadership-Team hat eng mit seinen Geschäftspartnern zusammengearbeitet, um zu verstehen, was diese mit dieser Migration erreichen möchten:

- **Geschäftswachstum**: Contoso wächst und daher geraten die lokalen Systeme und die Infrastruktur unter Druck.
- **Risikobegrenzung**: Die Service Desk-App ist wichtig für das Geschäft von Contoso. Sie soll ohne jedes Risiko in Azure verschoben werden können.
- **Erweitern**: die App soll derzeit nicht geändert werden. Es soll lediglich sichergestellt werden, dass sie stabil ist.


## <a name="migration-goals"></a>Migrationsziele

Für die Bestimmung der besten Migrationsmethode hat das Contoso-Cloudteam Ziele für diese Migration festgelegt:

- Nach der Migration sollte die App in Azure die gleichen Leistungsmöglichkeiten aufweisen wie die lokale VMWare-Umgebung.  Die App bleibt lokal und in der Cloud gleichermaßen wichtig. 
- Contoso möchte nicht in diese App investieren.  Die Investition ist wichtig für das Geschäft, in ihrer derzeitigen Form soll die App jedoch lediglich sicher in die Cloud verschoben werden.
- Contoso möchte das Vorgangsmodell für diese App nicht ändern. Das Unternehmen möchte in der Cloud wie aktuell damit interagieren können.
- Contoso möchte die App-Funktionalität nicht ändern. Nur der Speicherort der App ändert sich.
- Nach Abschluss einer Reihe von Windows-App-Migrationen möchte Contoso mehr über die Verwendung einer Linux-basierten Infrastruktur in Azure erfahren.

## <a name="proposed-architecture"></a>Vorgeschlagene Architektur

Szenario:

- Die App wird über zwei VM-Schichten (OSTICKETWEB und OSTICKETMYSQL) zur Verfügung gestellt.
- Die VMs befinden sich auf dem VMware ESXi-Host **contosohost1.contoso.com** (Version 6.5).
- Die VMware-Umgebung wird von der vCenter Server 6.5-Software (**vcenter.contoso.com**) auf einer VM verwaltet.
- Contoso verfügt über ein lokales Rechenzentrum (**contoso-datacenter**) mit einem lokalen Domänencontroller (**contosodc1**).
- Da es sich bei der App um eine Produktionsworkload handelt, befinden sich die VMs in Azure in der Ressourcengruppe **ContosoRG** für die Produktion.
- Die VMs werden zur primären Region (USA, Osten 2) migriert und im Produktionsnetzwerk (VNET-PROD-EUS2) platziert:
    - Die Web-VM befindet sich im Front-End-Subnetz (PROD-FE-EUS2).
    - Die Datenbank-VM befindet sich im Subnetz der Datenbank (PROD-DB-EUS2).
- Die lokalen VMs im Rechenzentrum von Contoso werden nach Abschluss der Migration außer Betrieb gesetzt.

![Szenarioarchitektur](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>Migrationsprozess

Contoso geht bei der Migration wie folgt vor:

1. In einem ersten Schritt richtet Contoso die Azure-Infrastruktur und die lokale Infrastruktur ein, die für die Bereitstellung von Site Recovery erforderlich sind.
2. Nach der Vorbereitung der Azure-Komponenten und der lokalen Komponenten richtet das Unternehmen die Replikation für die VMs ein und aktiviert diese.
3. Wenn die Replikation funktioniert, migriert Contoso die VMs via Failover zu Azure.

![Migrationsprozess](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure-Dienste

**Service** | **Beschreibung** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.

 
## <a name="prerequisites"></a>Voraussetzungen

Für dieses Szenario benötigen Sie (und Contoso) Folgendes.

**Anforderungen** | **Details**
--- | ---
**Azure-Abonnement** | Sie sollten bereits im Zuge früherer Artikel dieser Reihe ein Abonnement erstellt haben. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-Infrastruktur** | Contoso richtet die Azure-Infrastruktur ein, wie in [Azure infrastructure for migration (Azure-Infrastruktur für die Migration)](contoso-migration-infrastructure.md) beschrieben.<br/><br/> Erfahren Sie mehr zu spezifischen [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network)- und [Speicheranforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) für Site Recovery.
**Lokale Server** | Ihr lokaler Server von vCenter Server sollte als Version 5.5, 6.0 oder 6.5 ausgeführt werden.<br/><br/> Einen ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.
**Lokale VMs** | [Überprüfen Sie die Linux-Computer](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines), die für die Migration mit Site Recovery unterstützt werden.<br/><br/> Überprüfen Sie unterstützte [Linux-Datei- und -Speichersysteme](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.


## <a name="scenario-steps"></a>Szenarioschritte

Azure geht bei der Migration wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Vorbereiten von Azure für Site Recovery**: Es werden ein Azure-Speicherkonto zum Speichern replizierter Daten und ein Recovery Services-Tresor erstellt.
> * **Schritt 2: Vorbereiten der lokalen VMware für Site Recovery**: Es werden Konten für die VM-Ermittlung und Agentinstallation sowie eine Verbindung mit Azure-VMs nach dem Failover vorbereitet.
> * **Schritt 3: Replizieren von VMs**: Die Quell- und Zielmigrationsumgebung wird eingerichtet, eine Replikationsrichtlinie erstellt und die Replikation von VMs im Azure-Speicher gestartet.
> * **Schritt 4: Migrieren der VMs mit Site Recovery**: Es wird ein Testfailover durchgeführt, um sicherzustellen, dass alles funktioniert, und anschließend wird ein vollständiges Failover für die Migration der VMs zu Azure ausgeführt.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Schritt 1: Vorbereiten von Azure für den Site Recovery-Dienst

Contoso benötigt eine Reihe von Azure-Komponenten für Site Recovery:

- Ein VNet, in dem sich Ressourcen befinden, für die ein Failover ausgeführt wurde (Contoso verwendet das bereits bereitgestellte Produktions-VNet)
- Ein neues Azure-Speicherkonto für die Speicherung replizierter Daten. 
- Ein Recovery Services-Tresor in Azure.

Contoso hat das VNet bereits während der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md) erstellt und muss daher nur ein Speicherkonto und einen Tresor erstellen.

1. Contoso erstellt ein Azure-Speicherkonto (contosovmsacc20180528) in der Region „USA, Osten 2“.

    - Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.
    - Das Unternehmen verwendet hierzu ein Konto für allgemeine Zwecke mit Standardspeicher und LRS-Replikation.

    ![Site Recovery-Speicher](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Wenn das Netzwerk und das Speicherkonto vorhanden sind, erstellt Contoso einen Tresor (ContosoMigrationVault) und platziert diesen in der Ressourcengruppe **ContosoFailoverRG**, in der primären Region „USA, Osten 2“.

    ![Recovery Services-Tresor](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Benötigen Sie weitere Hilfe?**

[Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) zum Einrichten von Azure für Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 2: Vorbereiten der lokalen VMware für Site Recovery

Contoso bereitet die lokale VMware-Infrastruktur wie folgt vor:

- Erstellen eines Kontos auf dem Server von vCenter Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung.
- Erstellen eines Kontos für die automatische Installation von Mobility Service auf VMware-VMs, die Sie replizieren möchten.
- Vorbereiten lokaler VMs, damit diese mit Azure-VMs verbunden werden können, wenn diese nach der Migration erstellt werden.


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Contoso richtet das Konto wie folgt ein:

1. Contoso erstellt eine Rolle auf der vCenter-Ebene.
2. Anschließend weist Contoso dieser Rolle die erforderlichen Berechtigungen zu.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Mobility Service muss auf den von Contoso migrierten Linux-VMs installiert werden:

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn Sie die Replikation für die VMs aktivieren.
- Für die automatische Pushinstallation müssen Sie ein Konto vorbereiten, mit dem Site Recovery auf die VMs zugreift.
- Während der Einrichtung der Replikation werden Kontodetails eingegeben. 
- Bei dem Konto kann es sich um ein Domänenkonto oder ein lokales Konto mit Berechtigungen für die Installation auf VMs handeln.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach dem Failover in Azure möchte Contoso eine Verbindung mit den replizierten VMs in Azure herstellen können. Hierzu muss das Unternehmen einige Schritte durchführen:

- Für den Zugriff über das Internet muss Contoso vor der Migration SSH auf der lokalen Linux-VM aktivieren.  Bei Ubuntu kann dieser Vorgang mithilfe des folgenden Befehls durchgeführt werden: **Sudo apt-get ssh install -y**.
- Nach der Durchführung der Migration (Failover) sollte das Unternehmen die **Startdiagnose** überprüfen, um einen Screenshot der VM anzuzeigen.
- Falls dies nicht funktioniert, sollte das Unternehmen überprüfen, ob die VM ausgeführt wird, und folgende [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) lesen.


**Benötigen Sie weitere Hilfe?**

- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) zum Erstellen und Zuweisen von Rollen für die automatische Ermittlung.
- [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) zum Erstellen eines Kontos für die Pushinstallation von Mobility Service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Schritt 3: Replizieren der lokalen VMs

Bevor Contoso die Web-VM zu Azure migrieren kann, muss das Unternehmen die Replikation einrichten und aktivieren.

### <a name="set-a-protection-goal"></a>Festlegen eines Schutzziels

1. Im Tresor wird unter dem Tresornamen (ContosoVMVault) ein Replikationsziel festgelegt (**Erste Schritte** > **Site Recovery** > **Infrastruktur vorbereiten**).
2. Das Unternehmen gibt an, dass seine Computer lokal sind, dass es sich um VMware-VMs handelt und diese in Azure repliziert werden sollen.
    ![Replikationsziel](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Damit das Unternehmen fortfahren kann, bestätigt es durch Auswahl von **Ja, ist abgeschlossen**, dass es die Bereitstellungsplanung abgeschlossen hat. Contoso migriert in diesem Szenario nur eine VM. Daher ist keine Bereitstellungsplanung erforderlich.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Contoso muss seine Quellumgebung konfigurieren. Hierzu wird eine OVF-Vorlage heruntergeladen, die für die Bereitstellung des Site Recovery-Konfigurationsservers als hoch verfügbare, lokale VMware-VM verwendet wird. Nachdem der Konfigurationsserver eingerichtet wurde und ausgeführt wird, wird er im Tresor registriert.

Auf dem Konfigurationsserver werden einige Komponenten ausgeführt:

- Die Konfigurationsserverkomponente, die die Kommunikation zwischen der lokalen Umgebung und Azure koordiniert und die Datenreplikation verwaltet.
- Der Prozessserver, der als Replikationsgateway fungiert. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.

Contoso führt diese Schritte wie folgt aus:

1. Das Unternehmen lädt die OVF-Vorlage von **Infrastruktur vorbereiten** > **Quelle** > **Konfigurationsserver** herunter.
    
    ![Herunterladen der OVF-Vorlage](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Es importiert die Vorlage in VMware, um die VM zu erstellen und bereitzustellen.

    ![OVF-Vorlage](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Wenn das Unternehmen die VM zum ersten Mal aktiviert, wird sie in einem Windows Server 2016-Installationsvorgang hochgefahren. Das Unternehmen akzeptiert den Lizenzvertrag und gibt ein Administratorkennwort ein.
4. Nach Abschluss der Installation meldet es sich als Administrator bei der VM an. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool standardmäßig ausgeführt.
5. Das Unternehmen gibt im Tool einen Namen an, der für die Registrierung des Konfigurationsservers im Tresor verwendet werden soll.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Nachdem die Verbindung hergestellt wurde, meldet sich das Unternehmen bei seinem Azure-Abonnement an. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.

    ![Konfigurationsserver registrieren](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Contoso meldet sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.
9. Im Assistenten wählt das Unternehmen die NIC zum Empfangen von Replikationsdatenverkehr aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
10. Das Unternehmen wählt das Abonnement, die Ressourcengruppe und den Tresor aus, in dem der Konfigurationsserver registriert werden soll.

    ![Tresor](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Anschließend werden MySQL Server und VMWare PowerCLI heruntergeladen und installiert. 
12. Nach der Überprüfung gibt Contoso den FQDN oder die IP-Adresse des Servers von vCenter Server oder des vSphere-Hosts an. Der Standardport wird beibehalten, und für den Server von vCenter Server wird ein Anzeigename angegeben.
13. Contoso gibt das für die automatische Ermittlung erstellte Konto und die Anmeldeinformationen für die automatische Installation von Mobility Service an.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Nach Abschluss der Registrierung im Azure-Portal überprüft Contoso, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Die Ermittlung kann mindestens 15 Minuten dauern. 
15. Anschließend stellt Site Recovery eine Verbindung mit VMware-Servern her und ermittelt VMs.

### <a name="set-up-the-target"></a>Einrichten des Ziels

Contoso konfiguriert nun die Zielreplikationseinstellungen.

1. Das Unternehmen wählt unter **Infrastruktur vorbereiten** > **Ziel** die Zieleinstellungen aus.
2. Site Recovery überprüft, ob das angegebene Ziel ein Azure-Speicherkonto und -Netzwerk enthält.

### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

Nach der Einrichtung der Quelle und des Ziels kann Contoso nun eine Replikationsrichtlinie erstellen.

1. Unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** >  **Erstellen und zuordnen** erstellt das Unternehmen die Richtlinie **ContosoMigrationPolicy**.
2. Es werden die Standardeinstellungen verwendet:
    - **RPO-Schwellenwert**: Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
    - **Aufbewahrung des Wiederherstellungspunkts**. Standardwert von 24 Stunden. Dieser Wert gibt den Aufbewahrungszeitraum für die einzelnen Wiederherstellungspunkte an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
    - **Häufigkeit von Momentaufnahmen für App-Konsistenz**. Der Standardwert ist eine Stunde. Dieser Wert gibt die Häufigkeit an, mit der anwendungskonsistente Momentaufnahmen erstellt werden.
 
        ![Erstellen einer Replikationsrichtlinie](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Zuordnen der Replikationsrichtlinie](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) zum Azure-Gast-Agent für Linux.

**Benötigen Sie weitere Hilfe?**

- Unter [Einrichten der Notfallwiederherstellung für lokale VMware-VMs](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial) finden Sie eine vollständige exemplarische Vorgehensweise all dieser Schritte.
- Ausführliche Anweisungen dienen Ihnen beim [Einrichten der Quellumgebung](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [Bereitstellen des Konfigurationsservers](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) und beim [Konfigurieren von Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication) als Unterstützung.
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) zum Azure-Gast-Agent für Linux.



### <a name="enable-replication-for-osticketweb"></a>Aktivieren der Replikation für OSTICKETWEB

Contoso kann nun mit der Replikation der VM **OSTICKETWEB** beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quelleneinstellungen aus.
2. Es wählt aus, dass virtuelle Computer aktiviert werden sollen. Darüber hinaus wählt es Quelleneinstellungen einschließlich der Einstellungen für den Server von vCenter Server und den Konfigurationsserver aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Contoso gibt die Zieleinstellungen, einschließlich der Ressourcengruppe und des VNet als Speicherort für die Azure-VM nach dem Failover, und das Speicherkonto an, in dem die replizierten Daten gespeichert werden.

     ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Contoso wählt **OSTICKETWEB** für die Replikation aus. 

    - In dieser Phase wählt Contoso nur **OSTICKETWEB** aus, da das VNet und das Subnetz ausgewählt werden müssen und sich die VMs nicht in demselben Subnetz befinden.
    - Wenn die Replikation für die VM aktiviert ist, wird Mobility Service von Site Recovery automatisch installiert.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. In den VM-Eigenschaften wählt Contoso das von dem Prozessserver verwendete Konto aus, damit Mobility Service automatisch auf dem Computer installiert wird.

     ![Mobilitätsdienst](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. Unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** überprüft das Unternehmen, ob die richtige Replikationsrichtlinie angewendet wird, und wählt **Replikation aktivieren** aus.
6.  Der Replikationsfortschritt wird unter **Aufträge** nachverfolgt. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.



### <a name="enable-replication-for-osticketmysql"></a>Aktivieren der Replikation für OSTICKETMYSQL

Contoso kann nun mit der Replikation von **OSTICKETMYSQL** beginnen.

1. Unter **Anwendung replizieren** > **Quelle** > **+ Replizieren** wählt das Unternehmen die Quellen- und Zieleinstellungen aus.
2. Contoso wählt **OSTICKETMYSQL** für die Replikation und das Konto für die Installation von Mobility Service aus.

    ![Aktivieren der Replikation](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso wendet die gleiche Replikationsrichtlinie an, die auch für OSTICKETWEB verwendet wurde, und aktiviert die Replikation.  

**Benötigen Sie weitere Hilfe?**

Unter [Aktivieren der Replikation](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication) finden Sie eine vollständige exemplarische Vorgehensweise mit all diesen Schritten.


## <a name="step-4-migrate-the-vms"></a>Schritt 4: Migrieren der VMs 

Contoso führt ein schnelles Testfailover aus und migriert die VMs anschließend.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Das Ausführen eines Testfailovers dient zur Sicherstellung, dass vor der Migration alles wie erwartet funktioniert. 

1. Contoso führt ein Testfailover zum letzten verfügbaren Zeitpunkt aus (**Zuletzt verarbeitet**).
2. **Computer vor Starten des Failovers herunterfahren** wird ausgewählt, damit Site Recovery versucht, die Quell-VM vor dem Auslösen des Failovers herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
3. Testfailover durchführen: 
    - Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
    - Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn der letzte Wiederherstellungspunkt ausgewählt wird, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
    - Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.
3. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal angezeigt. Das Unternehmen überprüft, ob die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird. 
4. Nach der Überprüfung bereinigt es das Failover. Darüber hinaus werden Beobachtungen aufgezeichnet und gespeichert.

### <a name="create-and-customize-a-recovery-plan"></a>Erstellen und Anpassen eines Wiederherstellungsplans

 Nachdem Contoso überprüft hat, ob das Testfailover wie erwartet funktioniert, erstellt das Unternehmen einen Wiederherstellungsplan für die Migration. 

- Ein Wiederherstellungsplan gibt die Reihenfolge an, in der ein Failover auftritt, wie Azure-VMs in Azure hervorgebracht werden.
- Da das Unternehmen eine zweischichtige App migrieren möchte, passt es den Wiederherstellungsplan so an, dass die Daten-VM (SQLVM) vor der Front-End-VM (WEBVM) gestartet wird.


1. Unter **Wiederherstellungspläne (Site Recovery)** > **+Wiederherstellungsplan** erstellt das Unternehmen einen Plan und fügt die VMs dem Plan hinzu.

    ![Wiederherstellungsplan](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Nachdem der Plan erstellt wurde, wählt das Unternehmen diesen zur Anpassung aus (**Wiederherstellungspläne** > **OsTicketMigrationPlan** > **Anpassen**).
3.  Es entfernt **OSTICKETWEB** aus **Gruppe 1: Start**.  Dadurch wird sichergestellt, dass die erste Startaktion sich nur auf **OSTICKETMYSQL** auswirkt.

    ![Wiederherstellungsgruppe](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  Unter **+ Gruppe** > **Geschützte Elemente hinzufügen** fügt Contoso **OSTICKETWEB** zu **Gruppe 2: Start** hinzu.  Contoso benötigt diese in zwei unterschiedlichen Gruppen.

    ![Wiederherstellungsgruppe](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Migrieren der VMs


Contoso kann nun ein Failover für den Wiederherstellungsplan ausführen, um die VMs zu migrieren.

1. Es wählt den Plan > **Failover** aus.
2.  Es entscheidet, ein Failover für den letzten Wiederherstellungspunkt auszuführen. Zudem gibt das Unternehmen an, dass Site Recovery versuchen sollte, die lokale VM vor dem Auslösen des Failovers herunterzufahren. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)

3. Während des Failovers gibt der vCenter Server Befehle zum Beenden der beiden VMs aus, die auf dem ESXi-Host ausgeführt werden.

    ![Failover](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Nach dem Failover überprüft Contoso, ob die Azure-VM wie erwartet im Azure-Portal angezeigt wird.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Nach der Überprüfung der VM in Azure schließt das Unternehmen die Migration ab, um den Migrationsprozess für die einzelnen VMs zu beenden. Dadurch werden die Replikation für die VM und die Site Recovery-Abrechnung für die VM beendet.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>Verbinden der VM mit der Datenbank

Der letzte Schritt im Migrationsprozess besteht in der Aktualisierung der Verbindungszeichenfolge der Anwendung, um auf die App-Datenbank zu verweisen, die auf der VM **OSTICKETMYSQL** ausgeführt wird. 

1. Das Unternehmen stellt über Putty oder einen anderen SSH-Client eine SSH-Verbindung mit der VM **OSTICKETWEB** her. Die VM ist privat, daher stellt Contoso eine Verbindung über die private IP-Adresse her.

    ![Herstellen einer Verbindung mit der Datenbank](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Herstellen einer Verbindung mit der Datenbank](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Das Unternehmen muss sicherstellen, dass die VM **OSTICKETWEB** mit der VM **OSTICKETMYSQL** kommunizieren kann. Die Konfiguration ist mit der lokalen IP-Adresse 172.16.0.43 derzeit hartcodiert.

    **Vor dem Update**
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Nach dem Update**
    
    ![Update-IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Das Unternehmen startet den Dienst mit dem Befehl **systemctl restart apache2** neu.

    ![Neu starten](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Schließlich aktualisiert Contoso die DNS-Datensätze für **OSTICKETWEB** und **OSTICKETMYSQL** auf einem der Contoso-Domänencontroller.

    ![DNS-Aktualisierung](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS-Aktualisierung](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Benötigen Sie weitere Hilfe?**

- [Informationen](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) zum Ausführen eines Testfailovers. 
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) zum Erstellen eines Wiederherstellungsplans.
- [Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) zum Ausführen eines Failovers für Azure.

## <a name="clean-up-after-migration"></a>Bereinigung nach der Migration

Nach Abschluss der Migration werden die Schichten der App osTicket jetzt auf Azure-VMs ausgeführt.

Contoso muss nun einige Bereinigungsmaßnahmen vornehmen:  

- Das Unternehmen entfernt die lokalen VMs aus dem vCenter-Bestand.
- Es entfernt die lokalen VMs aus lokalen Sicherungsaufträgen.
- Es aktualisiert die interne Dokumentation, damit der neue Speicherort und die IP-Adressen für OSTICKETWEB und OSTICKETMYSQL angezeigt werden.
- Das Unternehmen überprüft sämtliche Ressourcen, die mit den VMs interagieren, und aktualisiert sämtliche relevanten Einstellungen oder Dokumentationen, um die neue Konfiguration widerzuspiegeln.
- Contoso hat mithilfe des Azure Migrate-Diensts durch die Zuordnung von Abhängigkeiten die VMs für die Migration bewertet. Der Microsoft Monitoring Agent und der Dependency-Agent, die für diesen Zweck installiert wurden, sollten von der VM entfernt werden.

## <a name="review-the-deployment"></a>Überprüfen der Bereitstellung

Da die App jetzt ausgeführt wird, muss Contoso seine neue Infrastruktur vollständig operationalisieren und sichern.

### <a name="security"></a>Sicherheit

Das Sicherheitsteam von Contoso überprüft die VMs OSTICKETWEB und OSTICKETMYSQL, um mögliche Sicherheitsprobleme zu ermitteln.

- Es überprüft die Netzwerksicherheitsgruppen der VMs zur Steuerung des Zugriffs. Mithilfe von Netzwerksicherheitsgruppen wird sichergestellt, dass nur für die App zulässiger Datenverkehr übergeben werden kann.
- Das Sicherheitsteam zieht darüber hinaus in Erwägung, die Daten auf den VM-Datenträgern über Datenträgerverschlüsselung und Azure KeyVault zu sichern.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) über Sicherheitsverfahren für VMs.

### <a name="backups"></a>Backups

Contoso sichert die Daten auf den VMs mithilfe des Azure Backup-Diensts. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="licensing-and-cost-optimization"></a>Lizenzierung und Kostenoptimierung

- Nach der Bereitstellung von Ressourcen, weist der Contoso Azure Tags zu, die während der [Bereitstellung der Azure-Infrastruktur](contoso-migration-infrastructure.md#set-up-tagging) definiert wurden.
- Contoso hat mit seinen Ubuntu-Servern keine Lizenzierungsprobleme.
- Contoso aktiviert Azure Cost Management. Es ist durch Cloudyn lizenziert, ein Tochterunternehmen von Microsoft. Dabei handelt es sich um eine Kostenverwaltungslösung mit mehreren Clouds, die Ihnen das Verwenden und Verwalten von Azure und anderen Cloudressourcen erleichtert.  [Erfahren Sie mehr](https://docs.microsoft.com/azure/cost-management/overview) über die Azure Cost Management. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Contoso eine lokale mehrschichtige Service Desk-App mit zwei Linux-VMs mithilfe von Azure Site Recovery zu Azure-IaaS-VMs migriert hat.

Im nächsten Artikel der Reihe erfahren Sie, wie Contoso die gleiche Service Desk-App zu Azure migriert. Dieses Mal migriert Contoso die Front-End-VM für die App mithilfe von Site Recovery. Das Unternehmen migriert die App-Datenbank mit dem Tool MySQL Workbench über die Sicherung und Wiederherstellung in Azure Database for MySQL. [Erste Schritte](contoso-migration-rehost-linux-vm-mysql.md).

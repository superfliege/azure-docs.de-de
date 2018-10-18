---
title: Installieren von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Vorgehensweise zum Installieren von SAP HANA in Azure (große Instanzen).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dad088138fea2dd4fadc0cc9eed71245c32a8e0b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162660"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)

Machen Sie sich vor dem Lesen dieses Artikels mit [allgemeinen Begriffen zu HANA (große Instanzen)](hana-know-terms.md) und [SKUs für HANA (große Instanzen)](hana-available-skus.md) vertraut.

Die Installation von SAP HANA liegt in Ihrer Verantwortung, und Sie können die Aktivität nach der Übergabe eines Servers für SAP HANA in Azure (große Instanzen) starten. (Und nachdem die Konnektivität zwischen Ihren Azure VNets und der bzw. den Einheiten von HANA (große Instanzen) hergestellt wurde.) 

> [!Note]
> Gemäß SAP-Richtlinie muss die Installation von SAP HANA von einer Person durchgeführt werden, die für die Durchführung von SAP HANA-Installationen zertifiziert ist. Dies ist eine Person, die die Certified SAP Technology Associate-Prüfung, die SAP HANA Installation-Zertifizierungsprüfung oder die Prüfung durch einen SAP-zertifizierten Systemintegrator (SI) bestanden hat.

Führen Sie unter [SAP Support Note #2235581 - SAP HANA: Supported Operating Systems](https://launchpad.support.sap.com/#/notes/2235581/E) (SAP-Supporthinweis #2235581 – SAP HANA: Unterstützte Betriebssysteme) erneut eine Prüfung durch, vor allem bei einer geplanten Installation von HANA 2.0, um sicherzustellen, dass das Betriebssystem von der SAP HANA-Version unterstützt wird, die Sie installiert haben. Sie sehen, dass das unterstützte Betriebssystem für HANA 2.0 stärker eingeschränkt als das Betriebssystem ist, das für HANA 1.0 unterstützt wird. 

> [!IMPORTANT] 
> Derzeit wird für Einheiten vom Typ II nur die Betriebssystemversion SLES 12 SP2 unterstützt. 

Sie müssen Folgendes überprüfen, bevor Sie mit der Installation von HANA beginnen:
- [Überprüfen der HLI Einheiten](#validate-the-hana-large-instance-units)
- [Betriebssystemkonfiguration](#operating-system)
- [Network Configuration](#networking)
- [Speicherkonfiguration](#storage)


## <a name="validate-the-hana-large-instance-units"></a>Validieren der HANA-Einheiten (große Instanzen)

Nachdem Sie die HANA-Einheiten (große Instanzen) von Microsoft erhalten haben, überprüfen Sie die folgenden Einstellungen und passen sie nach Bedarf an.

Der **erste Schritt** nach dem Empfang der Einheiten von SAP HANA (große Instanzen) und der Einrichtung des entsprechenden Zugriffs und der Verbindung besteht darin, das Betriebssystem der Instanz bei Ihrem Betriebssystemanbieter zu registrieren. Zu diesem Schritt gehört das Registrieren Ihres SUSE Linux-Betriebssystems in einer Instanz von SUSE SMT, die Sie auf einer VM in Azure bereitstellen müssen. Für die HANA-Einheit (große Instanzen) kann eine Verbindung mit dieser SMT-Instanz hergestellt werden (siehe [Einrichten eines SMT-Servers für SUSE-Linux](hana-setup-smt.md)). Alternativ muss Ihr Red Hat-Betriebssystem beim Red Hat Subscription Manager registriert werden, mit dem Sie eine Verbindung herstellen müssen. Lesen Sie auch die Anmerkungen in diesem [Dokument](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dieser Schritt ist auch erforderlich, damit das Betriebssystem gepatcht werden kann. Eine Aufgabe, die in der Verantwortung des Kunden liegt. Die Dokumentation zum Installieren und Konfigurieren von SMT für SUSE finden Sie [hier](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

Der **zweite Schritt** besteht darin, nach neuen Patches und Fixes der speziellen Betriebssystemversion zu suchen. Überprüfen Sie, ob die Patchebene der SAP HANA (große Instanz) den neuesten Stand hat. Je nach Zeitplanung der Betriebssystempatches/-versionen und Änderungen am Image, das Microsoft bereitstellen kann, gibt es unter Umständen Fälle, in denen die neuesten Patches nicht enthalten sind. Daher muss, nachdem eine Einheit von SAP HANA (große Instanzen) übernommen wurde, geprüft werden, ob der jeweilige Linux-Anbieter in der Zwischenzeit Patches freigegeben hat, die für die Sicherheit, Funktionalität, Verfügbarkeit und Leistung relevant sind und angewendet werden müssen.

Der **dritte Schritt** besteht darin, die zutreffenden SAP-Hinweise (SAP Notes) zum Installieren und Konfigurieren von SAP HANA unter der speziellen Betriebssystemversion zu lesen. Wegen sich ändernden Empfehlungen oder Änderungen an SAP-Hinweisen oder -Konfigurationen, die von einzelnen Installationsszenarien abhängen, ist Microsoft nicht immer in der Lage, eine HANA (große Instanzen)-Einheit perfekt zu konfigurieren. Für Sie als Kunde ist es daher obligatorisch, dass Sie die SAP-Hinweise zu SAP HANA lesen, die exakt für Ihre Linux-Version gelten. Prüfen Sie außerdem die Konfigurationen der erforderlichen Betriebssystemversion, und wenden Sie die Konfigurationseinstellungen an, falls dies nicht bereits geschehen ist.

Prüfen Sie insbesondere die folgenden Parameter, und passen Sie diese ggf. an:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- net.core.wmem_default = 16777216
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

Seit SLES12 SP1 und RHEL 7.2 müssen diese Parameter in einer Konfigurationsdatei festgelegt sein, die sich im Verzeichnis „/etc/sysctl.d“ befindet. Beispielsweise muss eine Konfigurationsdatei namens „91-NetApp-HANA.conf“ erstellt werden. Für ältere SLES- und RHEL-Versionen müssen diese Parameter in „/etc/sysctl.conf“ festgelegt sein.

Für alle RHEL-Versionen und ab SLES12 muss der Parameter 
- sunrpc.tcp_slot_table_entries = 128

in „/etc/modprobe.d/sunrpc-local.conf“ festgelegt sein. Wenn die Datei nicht vorhanden ist, muss sie zunächst erstellt werden, indem der folgende Eintrag hinzugefügt wird: 
- options sunrpc tcp_max_slot_table_entries=128

Der **vierte Schritt** besteht darin, die Systemzeit Ihrer HANA (große Instanzen)-Einheit zu prüfen. Die Instanzen werden mit einer Systemzeitzone bereitgestellt, die dem Standort der Azure-Region entspricht, in der sich der Stempel für HANA (große Instanzen) befindet. Sie können die Systemzeit oder Zeitzone der Instanzen ändern, deren Besitzer Sie sind. Wenn Sie dies tun und weitere Instanzen in Ihrem Mandanten bestellen, müssen Sie auch die Zeitzone der neu bereitgestellten Instanzen anpassen. Microsoft-Vorgänge haben keinen Einblick in die Systemzeitzone, die Sie nach der Übergabe für die Instanzen eingerichtet haben. Daher kann es sein, dass neu bereitgestellte Instanzen nicht mit der Zeitzone eingerichtet sind, zu der Sie gewechselt haben. Folglich liegt es in Ihrer Verantwortung als Kunde, die Zeitzone der übergebenen Instanzen zu prüfen und ggf. anzupassen. 

Der **fünfte Schritt** besteht darin, „etc/hosts“ zu prüfen. Wenn die Blades übergeben werden, haben sie unterschiedliche IP-Adressen, die für unterschiedliche Zwecke zugeordnet sind (siehe nächster Abschnitt). Überprüfen Sie die Datei „etc/hosts“. In Fällen, in denen Einheiten in einem vorhandenen Mandanten hinzugefügt werden, dürfen Sie nicht erwarten, dass „etc/hosts“ der neu bereitgestellten Systeme ordnungsgemäß mit den IP-Adressen von früher bereitgestellten Systemen gewartet wird. Daher müssen Sie als Kunde die richtigen Einstellungen vornehmen, damit eine neu bereitgestellte Instanz die Namen von früher bereitgestellten Einheiten in Ihrem Mandanten auflösen kann. 

## <a name="operating-system"></a>Betriebssystem

> [!IMPORTANT] 
> Derzeit wird für Einheiten vom Typ II nur die Betriebssystemversion SLES 12 SP2 unterstützt. 

Der Auslagerungsbereich des bereitgestellten Betriebssystemimage ist gemäß [SAP Support Note #1999997 - FAQ: SAP HANA Memory](https://launchpad.support.sap.com/#/notes/1999997/E) (SAP-Supporthinweis #1999997 – FAQ: SAP HANA-Arbeitsspeicher) auf 2 GB festgelegt. Alle anderen gewünschten Einstellungen müssen von Ihnen als Kunde festgelegt werden.

[SUSE Linux Enterprise Server 12 SP1 for SAP Applications](https://www.suse.com/products/sles-for-sap/hana) ist die Distribution von Linux, die für SAP HANA in Azure (große Instanzen) installiert wird. Diese besondere Distribution bietet &quot;standardmäßig&quot; SAP-spezifische Funktionen (einschließlich vorab festgelegter Parameter für die effektive Ausführung von SAP unter SLES).

Auf der SUSE-Website unter [Ressourcen Library/White Papers](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) und unter [SAP on SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) im SAP Community Network (SCN) finden Sie verschiedene nützliche Ressourcen zur Bereitstellung von SAP HANA unter SLES (einschließlich u.a. zur Einrichtung für Hochverfügbarkeit und Sicherheitshärtung für den SAP-Betrieb).

Zusätzliche und nützliche Links zu SAP unter SUSE:

- [Website zu SAP HANA unter SUSE Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Best Practice for SAP: Enqueue Replication - SAP NetWeaver on SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113) (Bewährte Methode für SAP: Enqueue-Replikation – SAP NetWeaver unter SUSE Linux Enterprise 12)
- [ClamSAP - SLES Virus Protection for SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (ClamSAP – SLES-Virenschutz für SAP) (einschließlich SLES 12 für SAP-Anwendungen)

SAP-Supporthinweise zur Implementierung von SAP HANA unter SLES 12:

- [SAP Support Note #1944799 – SAP HANA Guidelines for SLES Operating System Installation](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html) (SAP-Supporthinweis #1944799 – SAP HANA-Richtlinien für die Installation des SLES-Betriebssystems)
- [SAP Support Note #2205917 – SAP HANA DB Recommended OS Settings for SLES 12 for SAP Applications](https://launchpad.support.sap.com/#/notes/2205917/E) (SAP-Supporthinweis #2205917 – SAP HANA DB – Empfohlene Betriebssystemeinstellungen für SLES 12 für SAP-Anwendungen)
- [SAP Support Note #1984787 – SUSE Linux Enterprise Server 12: Installation Notes](https://launchpad.support.sap.com/#/notes/1984787) (SAP-Supporthinweis #1984787 – SUSE Linux Enterprise Server 12: Installationshinweise)
- [SAP Support Note #171356 – SAP Software on Linux: General Information](https://launchpad.support.sap.com/#/notes/1984787) (SAP-Supporthinweis #171356 – SAP-Software unter Linux: Allgemeine Informationen)
- [SAP Support Note #1391070 – Linux UUID Solutions](https://launchpad.support.sap.com/#/notes/1391070) (SAP-Supporthinweis #1391070 – Linux UUID-Lösungen)

[Red Hat Enterprise Linux für SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) ist ein weiteres Angebot zum Ausführen von SAP HANA auf großen SAP HANA-Instanzen. Versionen von RHEL 6.7 und 7.2 sind verfügbar. Beachten Sie, dass im Gegensatz zu nativen virtuellen Azure-Computern, für die nur RHEL 7.2 und neuere Releases unterstützt werden, „große“ HANA-Instanzen auch RHEL 6.7 unterstützen. Wir empfehlen jedoch die Verwendung eines RHEL 7.x-Release.

Zusätzliche und nützliche Links zu SAP unter Red Hat:
- [SAP HANA on Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat) (Website zu SAP HANA unter Red Hat)

SAP-Supporthinweise zur Implementierung von SAP HANA unter Red Hat:

- [SAP Support Note #2009879 – SAP HANA Guidelines for Red Hat Enterprise Linux (RHEL) Operating System](https://launchpad.support.sap.com/#/notes/2009879/E) (SAP-Supporthinweis #2009879 – SAP HANA-Richtlinien für das RHEL-Betriebssystem (Red Hat Enterprise Linux))
- [SAP Support Note #2292690 - SAP HANA DB: Recommended OS Settings for RHEL 7](https://launchpad.support.sap.com/#/notes/2292690) (SAP-Supporthinweis #2292690 – SAP HANA DB: Empfohlene Betriebssystemeinstellungen für RHEL 7)
- [SAP Support Note #2247020 - SAP HANA DB: Recommended OS settings for RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020) (SAP-Supporthinweis #2247020 – SAP HANA DB: Empfohlene Betriebssystemeinstellungen für RHEL 6.7)
- [SAP Support Note #1391070 – Linux UUID Solutions](https://launchpad.support.sap.com/#/notes/1391070) (SAP-Supporthinweis #1391070 – Linux UUID-Lösungen)
- [SAP Support Note #2228351 - Linux: SAP HANA Database SPS 11 revision 110 (or higher) on RHEL 6 or SLES 11](https://launchpad.support.sap.com/#/notes/2228351) (SAP-Supporthinweis #2228351 – Linux: SAP HANA-Datenbank SPS 11 Revision 110 (oder höher) unter RHEL 6 oder SLES 11)
- [SAP Support Note #2397039 - FAQ: SAP on RHEL](https://launchpad.support.sap.com/#/notes/2397039) (SAP-Supporthinweis #2397039 – FAQ: SAP unter RHEL)
- [SAP Support Note #1496410 - Red Hat Enterprise Linux 6.x: Installation and Upgrade](https://launchpad.support.sap.com/#/notes/1496410) (SAP-Supporthinweis #1496410 – Red Hat Enterprise Linux 6.x: Installation und Upgrade)
- [SAP Support Note #2002167 - Red Hat Enterprise Linux 7.x: Installation and Upgrade](https://launchpad.support.sap.com/#/notes/2002167) (SAP-Supporthinweis #2002167 – Red Hat Enterprise Linux 7.x: Installation und Upgrade)

### <a name="time-synchronization"></a>Zeitsynchronisierung

SAP-Anwendungen, die basierend auf der SAP NetWeaver-Architektur erstellt werden, reagieren sehr empfindlich auf Zeitunterschiede bei den verschiedenen Komponenten, aus denen das SAP-System besteht. SAP ABAP-Kurzdumps mit dem Fehlertitel ZDATE\_LARGE\_TIME\_DIFF sind Ihnen wahrscheinlich vertraut. Diese Kurzdumps werden nämlich angezeigt, wenn die Systemzeit auf den verschiedenen Servern bzw. VMs zu stark voneinander abweicht.

Bei SAP HANA in Azure (große Instanzen) gilt die in Azure erfolgende Zeitsynchronisierung nicht für die Compute-Einheiten in „Große Instanz“-Stapeln. Diese Synchronisierung gilt nicht für die native Ausführung von SAP-Anwendungen auf nativen Azure-VMs, da in Azure sichergestellt wird, dass die Uhrzeit eines Systems richtig synchronisiert wird. Deshalb muss ein getrennter Zeitserver eingerichtet werden, der von auf Azure-VMs ausgeführten SAP-Anwendungsservern und den SAP HANA-Datenbankinstanzen verwendet werden kann, die in HANA (große Instanzen) ausgeführt werden. Der Speicherinfrastruktur in „Große Instanz“-Stapeln wird mit NTP-Servern zeitlich synchronisiert.


## <a name="networking"></a>Netzwerk
Es wird davon ausgegangen, dass Sie beim Entwerfen Ihrer Azure-VNets und Verbinden dieser VNets mit den Einheiten von HANA (große Instanzen) den Empfehlungen gefolgt sind, die in diesen Dokumenten beschrieben sind:

- [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Es gibt einige Details, die zu dem Netzwerkbetrieb der einzelnen Einheiten bekannt sein sollten. Jede HANA (große Instanzen)-Einheit wird mit zwei oder drei IP-Adressen geliefert, die zwei oder drei Netzwerkkartenports der Einheit zugewiesen sind. Drei IP-Adressen werden in horizontal skalierten HANA-Konfigurationen und im Szenario für HANA-Systemreplikation verwendet. Eine der IP-Adressen, die der Netzwerkkarte der Einheit zugewiesen sind, stammt aus dem Server-IP-Pool, der in [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) beschrieben ist.

Ethernet-Details für Ihre Architektur finden Sie unter [Unterstützte HLI-Szenarien](hana-supported-scenario.md).

## <a name="storage"></a>Speicher

Die Speicheranordnung für SAP HANA in Azure (große Instanzen) wird von der SAP HANA in Azure-Dienstverwaltung gemäß den von SAP empfohlenen Richtlinien konfiguriert. Diese Richtlinien sind im Whitepaper [SAP HANA Storage Requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (SAP HANA-Speicheranforderungen) dokumentiert. Die allgemeinen Größen der unterschiedlichen Volumes mit den einzelnen SKUs für HANA (große Instanzen) sind unter [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dokumentiert.

Die Benennungskonventionen der Speichervolumes sind in der folgenden Tabelle aufgeführt:

| Speicherverwendung | Bereitstellungsname | Volumename | 
| --- | --- | ---|
| HANA-Daten | /hana/data/SID/mnt0000<m> | Speicher-IP: /hana_data_SID_mnt00001_tenant_vol |
| HANA-Protokoll | /hana/log/SID/mnt0000<m> | Speicher-IP: /hana_log_SID_mnt00001_tenant_vol |
| HANA-Protokollsicherung | /hana/log/backups | Speicher-IP: /hana_log_backups_SID_mnt00001_tenant_vol |
| HANA-Freigabe | /hana/shared/SID | Speicher-IP: /hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/sap/SID | Speicher-IP: /hana_shared_SID_mnt00001_tenant_vol/usr_sap |

SID = System-ID der HANA-Instanz 

tenant = interne Enumeration von Vorgängen beim Bereitstellen eines Mandanten

Sie sehen, dass für die HANA-Freigabe und „usr/sap“ dasselbe Volume verwendet wird. Die Nomenklatur der Bereitstellungspunkte enthält sowohl die System-ID der HANA-Instanzen als auch die Bereitstellungsnummer. Bei Bereitstellungen für zentrales Skalieren ist nur eine Bereitstellung (Mount) vorhanden, z.B. „mnt00001“. Bei der Bereitstellung für horizontales Skalieren werden so viele „Mounts“ verwendet, wie Worker und Masterknoten vorhanden sind. Für die Umgebung für horizontales Skalieren werden Daten-, Protokoll- und Protokollsicherungsvolumes gemeinsam genutzt und jeweils an die Knoten der Konfiguration für horizontales Skalieren angefügt. Für Konfigurationen mit mehreren SAP-Instanzen wird ein anderer Satz mit Volumes erstellt und an die Einheit von HANA (große Instanzen) angefügt. Speicherlayoutdetails für Ihr Szenario finden Sie unter [Unterstützte HLI-Szenarien](hana-supported-scenario.md).

Während Sie das Dokument lesen und sich eine Einheit von HANA (große Instanzen) ansehen, werden Sie feststellen, dass die Einheiten mit einem ziemlich großzügigen Datenträgervolume für „HANA/data“ bereitgestellt werden und dass es das Volume „HANA/log/backup“ gibt. Wir haben „HANA/data“ so großzügig dimensioniert, weil für die Speichermomentaufnahmen, die Ihnen als Kunde angeboten werden, dasselbe Datenträgervolume verwendet wird. Dies bedeutet Folgendes: Je mehr Speichermomentaufnahmen Sie erstellen, desto mehr Speicherplatz wird von Momentaufnahmen in Ihren zugewiesenen Speichervolumes verbraucht. Das Volume „HANA/log/backup“ ist nicht für die Speicherung von Datenbanksicherungen vorgesehen. Seine Größe ist so ausgelegt, dass es als Sicherungsvolume für Sicherungen von HANA-Transaktionsprotokollen verwendet werden kann. Ausführlichere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Zusätzlich zu dem bereitgestellten Speicher können Sie weitere Speicherkapazität in Schritten von 1 TB erwerben. Dieser zusätzliche Speicher kann HANA (große Instanzen) als neue Volumes hinzugefügt werden.

Während des Onboardings mit SAP HANA in Azure-Dienstverwaltungsteam gibt der Kunde eine Benutzer-ID (UID) und Gruppen-ID (GID) für den sidadm-Benutzer und die „sapsys“-Gruppe an (Beispiel: 1000,500). Es ist erforderlich, dass während der Installation des SAP HANA-Systems genau diese Werte verwendet werden. Da Sie mehrere HANA-Instanzen in einer Einheit bereitstellen möchten, erhalten Sie mehrere Gruppen von Volumes (eine Gruppe für jede Instanz). Daher müssen Sie zur Bereitstellungszeit Folgendes definieren:

- Die SID der unterschiedlichen HANA-Instanzen (sidadm wird daraus abgeleitet).
- Arbeitsspeichergrößen der unterschiedlichen HANA-Instanzen. Die Arbeitsspeichergröße pro Instanz definiert die Größe der Volumes in den einzelnen Volumesätzen.

Basierend auf den Empfehlungen von Speicheranbietern werden für alle bereitgestellten Volumes (mit Ausnahme der Start-LUN) die folgenden Bereitstellungsoptionen konfiguriert:

- nfs    rw, vers=4, hard, timeo=600, rsize=1048576, wsize=1048576, intr, noatime, lock 0 0

Diese Bereitstellungspunkte werden unter „/etc/fstab“ konfiguriert, wie in den folgenden Grafiken dargestellt:

![fstab von bereitgestellten Volumes in Einheit von HANA (große Instanzen)](./media/hana-installation/image1_fstab.PNG)

Die Ausgabe des Befehls df -h in einer S72m-Einheit von HANA (große Instanzen) sieht wie folgt aus:

![fstab von bereitgestellten Volumes in Einheit von HANA (große Instanzen)](./media/hana-installation/image2_df_output.PNG)


Der Speichercontroller und die Knoten in den „Große Instanz“-Stapeln werden mit NTP-Servern synchronisiert. Beim Synchronisieren der SAP HANA in Azure (große Instanzen)-Einheiten und Azure-VMs mit einem NTP-Server darf es zwischen der Infrastruktur und den Compute-Einheiten in Azure- oder „Große Instanz“-Stapeln keine signifikante Zeitabweichung geben.

Um SAP HANA für den darunter verwendeten Speicher zu optimieren, sollten Sie auch die folgenden SAP HANA-Konfigurationsparameter festlegen:

- max_parallel_io_requests 128
- async_read_submit on
- async_write_submit_active on
- async_write_submit_blocks all
 
Für SAP HANA 1.0-Versionen bis SPS12 können diese Parameter während der Installation der SAP HANA-Datenbank festgelegt werden, wie dies in [SAP Note #2267798 - Configuration of the SAP HANA Database](https://launchpad.support.sap.com/#/notes/2267798) beschrieben ist.

Sie können die Parameter auch nach der Installation der SAP HANA-Datenbank konfigurieren, indem Sie das hdbparam-Framework verwenden. 

Seit SAP HANA Version 2.0 ist das hdbparam-Framework veraltet. Daher müssen die Parameter nun über SQL-Befehle festgelegt werden. Ausführliche Informationen finden Sie in [SAP Note #2399079: Elimination of hdbparam in HANA 2](https://launchpad.support.sap.com/#/notes/2399079).

Informationen zum Speicherlayout für Ihre Architektur finden Sie unter [Unterstützte HLI-Szenarien](hana-supported-scenario.md).


**Nächste Schritte**

- Lesen Sie [HANA-Installation unter HLI](hana-example-installation.md).










































 







 





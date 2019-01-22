---
title: Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload| Microsoft-Dokumentation
description: Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87d3a44b01dff81242f935c7737bd170fe744536
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246873"
---
# <a name="considerations-for-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[deployment-guide]:deployment-guide.md
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[planning-guide]:planning-guide.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Dieses Handbuch ist Teil der Dokumentation zur Implementierung und Bereitstellung der SAP-Software in Microsoft Azure. Lesen Sie vor diesem Handbuch das [Planungs- und Implementierungshandbuch][planning-guide]. Dieses Dokument behandelt die allgemeinen Aspekte der Bereitstellung von DBMS-Systemen für SAP auf Microsoft Azure Virtual Machines (VMs) mit der IaaS-Funktion (Infrastructure-as-a-Service) von Azure.

Das Dokument ergänzt die SAP-Installationsdokumentation und die SAP-Hinweise, die die primäre Ressource für Installationen und Bereitstellungen von SAP-Software auf verschiedenen Plattformen darstellen.

In diesem Dokument werden Überlegungen zum Ausführen von DBMS-Systemen für SAP auf Azure-VMs vorgestellt. Das Kapitel enthält nur wenige Verweise auf spezifische DBMS-Systeme. Diese werden weiter unten in diesem Dokument behandelt.

## <a name="definitions-upfront"></a>Definitionen
In diesem Dokument werden die folgenden Begriffe verwendet:

* IaaS: Infrastructure-as-a-Service.
* PaaS: Platform-as-a-Service.
* SaaS: Software-as-a-Service.
* SAP-Komponente: Eine einzelne SAP-Anwendung wie ECC, BW, Solution Manager oder EP. SAP-Komponenten können auf herkömmlichen ABAP- oder Java-Technologien oder auf einer Nicht-NetWeaver-basierten Anwendung wie Business Objects basieren.
* SAP-Umgebung: eine oder mehrere SAP-Komponenten, die logisch gruppiert sind, um eine Geschäftsfunktion wie Entwicklung, QAS, Schulung, DR oder Produktion auszuführen.
* SAP-Landschaft: Alle SAP-Ressourcen in der IT-Landschaft eines Kunden. Die SAP-Landschaft umfasst alle Produktions- und anderen Umgebungen.
* SAP-System: Die Kombination aus DBMS-Schicht und Anwendungsschicht in einem SAP ERP-Entwicklungssystem, einem SAP BW-Testsystem, einem SAP CRM-Produktionssystem usw. In Azure-Bereitstellungen wird die Aufteilung dieser beiden Ebenen zwischen lokalen Systemen und Azure nicht unterstützt. Dies bewirkt, dass ein SAP-System entweder lokal oder in Azure bereitgestellt wird. Allerdings können Sie die verschiedenen Systeme einer SAP-Landschaft in Azure oder lokal bereitstellen. Sie konnte z.B. die SAP CRM-Entwicklungs- und Testsysteme in Azure und die SAP CRM-Produktionssysteme lokal bereitstellen.
* Standortübergreifend: Beschreibt ein Szenario, in dem virtuelle Computer mit einem Azure-Abonnement bereitgestellt werden, das Site-to-Site-, Multisite- oder ExpressRoute-Verbindungen zwischen den lokalen Rechenzentren und Azure umfasst. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als "Cross-Premises"-Szenarien bezeichnet. Durch die Verbindung sollen lokale Domänen, das lokale Active Directory und das lokale DNS auf Azure ausgeweitet werden. Die lokale Landschaft wird auf die Azure-Ressourcen des Abonnements erweitert. Durch diese Erweiterung können die virtuellen Computer Teil der lokalen Domäne sein. Domänenbenutzer der lokalen Domäne können auf die Server zugreifen und Dienste auf diesen virtuellen Computern ausführen (z.B. DBMS-Dienste). Die Kommunikation und Namensauflösung zwischen lokal bereitgestellten virtuellen Computern und in Azure bereitgestellten virtuellen Computern ist möglich. Dieses Szenario ist das gängigste für die Bereitstellung von SAP-Ressourcen in Azure. Weitere Informationen finden Sie unter [Planung und Entwurf für VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Standortübergreifende Bereitstellungen von SAP-Systemen, in denen virtuelle Azure-Computer mit SAP-Systemen Mitglieder einer lokalen Domäne sind, werden für SAP-Produktionssysteme unterstützt. Standortübergreifende Konfigurationen werden für die Bereitstellung von Teilen von SAP-Landschaften oder vollständigen SAP-Landschaften in Azure unterstützt. Auch für das Ausführen von vollständigen SAP-Landschaften in Azure müssen diese VMs Teil der lokalen Domäne und von AD/LDAP sein. Bereits in früheren Versionen der Dokumentation wurden Hybrid-IT-Szenarios erwähnt. Der Begriff *Hybrid* bezieht sich dabei darauf, dass eine standortübergreifende Konnektivität zwischen lokalen Systemen und Azure besteht. In diesem Fall bedeutet *Hybrid* auch, dass die virtuellen Computer in Azure Teil des lokalen Active Directory sind.
>
>

In einigen Microsoft-Dokumentationen werden standortübergreifende Szenarien etwas anders beschrieben, insbesondere bei DBMS-HA-Konfigurationen. In Dokumenten zu SAP bezieht sich ein standortübergreifendes Szenario nur auf eine Site-to-Site- oder private [ExpressRoute](https://azure.microsoft.com/services/expressroute/)-Konnektivität und die Tatsache, dass die SAP-Landschaft zwischen lokalen Systemen und Azure verteilt ist.

## <a name="resources"></a>Ressourcen
Es gibt verschiedene Artikel zu SAP-Workload in Azure. Es wird empfohlen, zuerst den Artikel [Erste Schritte mit SAP in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) zu lesen und dann mit dem gewünschten Thema fortzufahren.

Die nachstehenden SAP-Hinweise beziehen sich auf SAP in Azure und die in diesem Dokument behandelten Themen:

| Hinweisnummer | Titel |
| --- | --- |
| [1928533] |SAP-Anwendungen auf Azure: Unterstützte Produkte und Azure VM-Typen |
| [2015553] |SAP auf Microsoft Azure: Voraussetzungen für die Unterstützung |
| [1999351] |Problembehandlung für die erweiterte Azure-Überwachung für SAP |
| [2178632] |Wichtige Überwachungsmetriken für SAP in Microsoft Azure |
| [1409604] |Virtualisierung unter Windows: Erweiterte Überwachung |
| [2191498] |SAP unter Linux mit Azure: Erweiterte Überwachung |
| [2039619] |SAP-Anwendungen auf Microsoft Azure mit der Oracle-Datenbank: Unterstützte Produkte und Versionen |
| [2233094] |DB6: SAP-Anwendungen auf Azure mit IBM DB2 für Linux, UNIX und Windows – Weitere Informationen |
| [2243692] |Microsoft Azure (IaaS)-VM: SAP-Lizenzprobleme |
| [1984787] |SUSE LINUX Enterprise Server 12: Installationshinweise |
| [2002167] |Red Hat Enterprise Linux 7.x: Installation und Upgrade |
| [2069760] |Oracle Linux 7.x SAP: Installation und Upgrade |
| [1597355] |Empfehlung zu Auslagerungsbereichen für Linux |
| [2171857] |Oracle Database 12c – Dateisystemunterstützung unter Linux |
| [1114181] |Oracle Database 11g – Dateisystemunterstützung unter Linux |


Lesen Sie bitte auch das [SCN-Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes), das alle SAP-Hinweise für Linux enthält.

Sie sollten über Kenntnisse zur Microsoft Azure-Architektur sowie zur Bereitstellung und Verwendung von virtuellen Microsoft Azure-Computern verfügen. Weitere Informationen finden Sie in der [Azure-Dokumentation](https://docs.microsoft.com/azure/).

Was IaaS angeht, sind Installation und Konfiguration von Windows, Linux und DBMS generell weitgehend identisch mit allen VMs oder Bare-Metal-Computern, die Sie lokal installieren. Es gibt jedoch einige Entscheidungen bei der Implementierung der Architektur und Systemverwaltung, die bei der Nutzung von Azure IaaS anders sind. In diesem Dokument werden die spezifischen Unterschiede bei Architektur und Systemverwaltung beschrieben, die Sie bei der Verwendung von Azure IaaS kennen sollten.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Speicherstruktur einer VM für RDBMS-Bereitstellungen
Damit Sie die Informationen in diesem Kapitel nachvollziehen können, müssen Sie [dieses][deployment-guide-3] Kapitel im [Bereitstellungshandbuch][deployment-guide] verstanden haben. Sie sollten die verschiedenen VM-Serien samt ihren Unterschieden und die Unterschiede zwischen Azure-Standardspeicher und [Storage Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) kennen, damit Sie dieses Kapitel verstehen.

In Bezug auf Azure Storage für Azure VMs sollten Sie die folgenden Artikel gelesen haben:

- [Informationen zu Datenträgern für virtuelle Azure-Windows-Computer](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds)
- [Informationen zu Datenträgern für virtuelle Azure-Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/about-disks-and-vhds)

In einer Basiskonfiguration eignet sich eine Bereitstellungsstruktur, bei der das Betriebssystem, das DBMS und eventuelle SAP-Binärdateien von den Datenbankdateien getrennt sind. Daher wird empfohlen, dass bei SAP-Systemen, die auf Azure-VMs ausgeführt werden, das Basis-VHD (oder der Datenträger) mit dem Betriebssystem, den ausführbaren Dateien für das Datenbankverwaltungssystem und den ausführbaren SAP-Dateien installiert wird. Die DBMS-Datendateien und -Protokolldateien werden in Azure Storage (Standardspeicher oder Storage Premium) auf separaten Datenträgern gespeichert und als logische Datenträger an die ursprünglichen Azure-VM mit dem Betriebssystemimage angefügt. Vor allem für Linux-Bereitstellungen sind möglicherweise unterschiedliche Empfehlungen dokumentiert. Dies gilt insbesondere für SAP HANA.

Bei der Planung des Datenträgerlayouts müssen Sie die beste Kombination in Hinblick auf die folgenden Aspekte ermitteln:

* Die Anzahl der Datendateien.
* Die Anzahl von Datenträgern, die die Dateien enthalten
* Die IOPS-Kontingente eines einzelnen Datenträgers
* Den Datendurchsatz pro Datenträger
* Die Anzahl der für die jeweilige VM-Größe möglichen zusätzlichen Datenträger
* Der Gesamtspeicherdurchsatz, den ein virtueller Computer bereitstellen kann.
* Die Latenz verschiedener Azure Storage-Typen.
* SLAs für VMs

Azure erzwingt ein IOPS-Kontingent pro Datenträger. Diese Kontingente sind für Datenträger, die im Azure-Standardspeicher und in Storage Premium gehostet werden, unterschiedlich. Auch die E/A-Latenz unterscheidet sich zwischen den beiden Speichertypen. Storage Premium bietet eine bessere E/A-Latenz. Für jeden der verschiedenen VM-Typen können Sie eine begrenzte Anzahl von Datenträgern anfügen. Eine weitere Einschränkung besteht darin, dass nur bestimmte Typen von virtuellen Computern Azure Storage Premium nutzen können. Folglich wird die Entscheidung für einen bestimmten VM-Typ nicht nur durch die Anforderungen an CPU und Arbeitsspeicher bestimmt, sondern auch durch IOPS-Wert, Latenz und Anforderungen an den Datenträgerdurchsatz, die normalerweise abhängig von der Anzahl von Datenträgern und vom Typ der Storage Premium-Datenträger skaliert werden. Vor allem bei Storage Premium wird die Größe eines Datenträgers möglicherweise auch von der Anzahl von IOPS und dem Durchsatz diktiert, der von den einzelnen Datenträgern erzielt werden muss.

> [!NOTE]
> Bei DBMS-Bereitstellungen wird die Verwendung von Storage Premium für alle Daten, Transaktionsprotokolle und Wiederholungsdateien empfohlen. Dabei spielt es keine Rolle, ob Sie Produktionssysteme bereitstellen möchten oder nicht.

> [!NOTE]
> Um die Vorteile vom [SLA für VMs](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) von Azure zu nutzen, müssen alle angefügten Datenträger vom Typ Azure Storage Premium sein, einschließlich der Basis-VHD.


> [!NOTE]
> Es wird nicht unterstützt, Hauptdatenbankdateien (Daten- und Protokolldateien) von SAP-Datenbanken auf Speicherhardware zu hosten, die sich in gemeinsam untergebrachten Drittanbieter-Datencentern neben Azure-Datencentern befindet. Für SAP-Workload wird für Daten- und Transaktionsprotokolldateien von SAP-Datenbanken nur Speicher unterstützt, der als nativer Azure-Dienst umgesetzt ist.
> 

Die Platzierung der Datenbankdateien, der Protokolldateien und der Wiederholungsdateien sowie der verwendete Azure Storage-Typ müssen durch die Anforderungen an IOPS, Latenz und Durchsatz bestimmt werden. Damit ausreichend IOPS verfügbar sind, müssen Sie möglicherweise mehrere Datenträger oder einen größeren Storage Premium-Datenträger verwenden. Falls Sie mehrere Datenträger verwenden, müssen Sie einen Softwarestripe über die Datenträger hinweg erstellen, die die Datendateien oder Protokoll-/Wiederholungsdateien enthalten. In solchen Fällen sind die IOPS- und die Datenträgerdurchsatz-SLAs der zugrunde liegenden Storage Premium-Datenträger oder der maximal erzielbare IOPS der Azure-Standardspeicherdatenträger kumulativ für das resultierende Stripeset.

Wenn Ihre IOPS-Anforderungen das übersteigen, was eine einzelne VHD bereitstellen kann, müssen Sie den für die Datenbankdateien benötigten IOPS über die Anzahl von VHDs ausgleichen. Die einfachste Möglichkeit, die IOPS-Last auf verschiedene Datenträger zu verteilen, ist das Erstellen eines Softwarestripes über die verschiedenen Datenträger hinweg. Fügen Sie dann den LUNs im Softwarestripe eine Anzahl von Datendateien des SAP-DBMS hinzu. Die Anzahl von Datenträgern im Stripe wird von den Anforderungen an IOPS, Datenträgerdurchsatz und Volumes bestimmt.


- - -
> ![Windows][Logo_Windows] Windows
>
> Es wird empfohlen, Windows-Speicherplätze zum Erstellen solcher Stripesets über mehrere Azure-VHDs hinweg zu verwenden. Außerdem ist es ratsam, mindestens Windows Server 2012 R2 oder Windows Server 2016 zu verwenden.
>
> ![Linux][Logo_Linux] Linux
>
> Nur MDADM und LVM (Logical Volume Manager) werden unterstützt, um ein Software-RAID unter Linux zu erstellen. Weitere Informationen finden Sie in den folgenden Artikeln:
>
> - [Konfigurieren von Software-RAID unter Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) mithilfe von MDADM
> - [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) (LVM)
>
>

- - -

> [!NOTE]
> Da Azure Storage drei Images der VHD speichert, ist es nicht ratsam, beim Striping eine Redundanz zu konfigurieren. Sie müssen nur das Striping so konfigurieren, dass die E/A-Vorgänge auf die verschiedenen VHDs verteilt werden.
>

### <a name="managed-or-non-managed-disks"></a>Verwaltete und nicht verwaltete Datenträger
Ein Azure-Speicherkonto ist nicht nur ein Konzept für Verwaltungsaufgaben, sondern stellt auch eine Gruppe von Einschränkungen dar. Die Einschränkungen unterscheiden sich zwischen Azure-Standardspeicherkonten und Azure Storage Premium-Konten. Die genauen Funktionen und Einschränkungen finden Sie im Artikel [Skalierbarkeits- und Leistungsziele für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

Beachten Sie für Azure-Standardspeicher, dass die IOPS pro Speicherkonto beschränkt sind (siehe Zeile **Gesamtanforderungsrate** in Artikel [Skalierbarkeits- und Leistungsziele für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)). Es gibt außerdem eine anfängliche Einschränkung für Speicherkonten pro Azure-Abonnement. Deswegen müssen Sie VHDs für größere SAP-Landschaften auf verschiedene Speicherkonten verteilen, um die Grenzen dieser Speicherkonten nicht zu überschreiten. Das ist sehr aufwändig, wenn Sie einige Hundert VMs mit über Tausend VHDs haben.

Da nicht empfohlen wird, Azure-Standardspeicher für DBMS-Bereitstellungen in Verbindung mit SAP-Workload zu verwenden, sind die entsprechenden Verweise und Empfehlungen auf [diesen kurzen Artikel](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx) beschränkt.

Um die Verwaltung der Planung und Bereitstellung von VHDs über verschiedene Azure-Speicherkonten hinweg zu vermeiden, führte Microsoft 2017 den Dienst [Managed Disks](https://azure.microsoft.com/services/managed-disks/) ein. Managed Disks ist verfügbar für Azure-Standardspeicher und Azure Storage Premium. Das sind die Hauptvorteile von Managed Disks – im Vergleich zu nicht verwalteten Datenträgern:

- Für Managed Disks verteilt Azure die verschiedenen VHDs bei der Bereitstellung automatisch auf verschiedene Speicherkonten und vermeidet so, dass die Grenzwerte des Azure Storage-Kontos für Datenmenge, E/A-Durchsatz und IOPS überschritten werden.
- Im Rahmen von Managed Disks nutzt Azure Storage Azure-Verfügbarkeitsgruppen und stellt so die Basis-VHD und den angefügten Datenträger einer VM in verschiedenen Fehler- und Updatedomänen bereit, wenn die VM Teil einer Azure-Verfügbarkeitsgruppe ist.


> [!IMPORTANT]
> Angesichts der Vorteile wird dringend empfohlen, Azure Managed Disks für Ihre DBMS-Bereitstellungen und SAP-Bereitstellungen im Allgemeinen zu verwenden.
>

In den folgenden Artikeln finden Sie weitere Informationen zum Konvertieren nicht verwalteter Datenträger in verwaltete Datenträger:

- [Konvertieren einer Windows-VM von nicht verwalteten Datenträgern in verwaltete Datenträger](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Konvertieren einer Linux-VM von nicht verwalteten Datenträgern in verwaltete Datenträger](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Zwischenspeichern für VMs und Datenträger
Wenn Sie Datenträger auf VMs bereitstellen, können Sie auswählen, ob der E/A-Datenverkehr zwischen der VM und diesen Datenträgern in Azure Storage zwischengespeichert wird. Azure Standard und Storage Premium verwenden zwei unterschiedliche Technologien für diese Art von Cache.

Die Empfehlungen unten gehen von den folgenden E/A-Eigenschaften für das Standard-DBMS aus:

- Es handelt sich hauptsächlich um Lesevorgänge für Datendateien einer Datenbank. Diese Lesevorgänge sind leistungskritisch für das DBMS-System.
- Das Schreiben in die Datendateien erfolgt in Bursts, die auf Prüfpunkten oder einem konstanten Datenstrom basieren. Dennoch, gemittelt über den Tag, gibt es weniger Schreib- als Lesevorgänge. Im Gegensatz zum Lesen von Datendateien sind diese Schreibvorgänge asynchron und verzögern keine Benutzertransaktionen.
- Es gibt kaum Lesevorgänge aus den Transaktionsprotokollen oder Wiederholungsdateien. Ausnahmen sind große E/A-Vorgänge beim Ausführen von Transaktionsprotokollsicherungen.
- Bei Transaktions- und Wiederholungsprotokolldateien stellen Schreibvorgänge die Hauptlast dar. Abhängig von der Art des Workloads kann die Größe von E/As bis zu 4 KB herunter- oder in anderen Fällen bis zu 1 MB oder mehr heraufreichen.
- Alle Schreibvorgänge müssen zuverlässig und dauerhaft auf einem Datenträger gespeichert werden.

Azure-Standardspeicher hat diese möglichen Cachetypen:

* Keine
* Lesen
* Lesen/Schreiben

Um eine konsistente und deterministische Leistung zu erzielen, sollten Sie das Zwischenspeichern im Azure-Standardspeicher für alle Datenträger mit **DBMS-bezogenen Datendateien, Protokoll-/Wiederholungsdateien und Tablespaces auf NONE** festlegen. Für das Zwischenspeichern der Basis-VHD kann die Standardeinstellung übernommen werden.

Azure Storage Premium hat die folgenden Optionen zum Zwischenspeichern:

* Keine
* Lesen
* Lesen/Schreiben
* Keine & Schreibbeschleunigung (nur für Azure-VMs der M-Serie)
* Lesen & Schreibbeschleunigung (nur für Azure-VMs der M-Serie)

Für Azure Storage Premium wird empfohlen, das **Zwischenspeichern von Lesevorgängen für Datendateien** in der SAP-Datenbank zu nutzen und **Kein Zwischenspeichern für die Datenträger von Protokolldateien** festzulegen.

Für Bereitstellungen der M-Serie wird dringend empfohlen, die Azure-Schreibbeschleunigung für die DBMS-Bereitstellung zu verwenden. Weitere Informationen, Einschränkungen und Bereitstellungsdetails für die Azure-Schreibbeschleunigung finden Sie im Dokument [Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


### <a name="azure-non-persistent-disks"></a>Flüchtiger Speicher in Azure
Azure-VMs bieten Datenträger mit flüchtigem Speicher, nachdem eine VM bereitgestellt wurde. Bei einem Neustart der VM werden alle Inhalte auf diesen Laufwerken gelöscht. Deswegen sollten Datendateien und Protokoll- bzw. Wiederholungsdateien von Datenbanken unter keinen Umständen auf diesen nicht permanenten Laufwerken gespeichert werden. Es gibt möglicherweise Ausnahmen für einige der Datenbanken, in denen diese nicht permanenten Laufwerke für tempdb und temporäre Tabellenbereiche geeignet sind. Verwenden Sie diese Laufwerke jedoch nicht für VMs der A-Serie, da diese nicht permanenten Laufwerke mit diesen VMs nur einen begrenzten Durchsatz ermöglichen. Weitere Informationen finden Sie im Artikel [Understanding the temporary drive on Windows Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) (Grundlegendes zum temporären Laufwerk auf virtuellen Windows-Computern in Azure).

- - -
> ![Windows][Logo_Windows] Windows
>
> Laufwerk „D:\“ auf einer Azure-VM ist ein nicht permanentes Laufwerk, das durch einige lokale Datenträger im Azure-Serverknoten gesichert wird. Da es nicht permanent ist, bedeutet dies, dass alle Änderungen am Inhalt auf dem Laufwerk „D:\“ verloren gehen, wenn der virtuelle Computer neu gestartet wird. „Alle Änderungen“ beziehen sich u. a. auf gespeicherte Dateien, erstellte Verzeichnisse und installierte Anwendungen.
>
> ![Linux][Logo_Linux] Linux
>
> Azure-VMs unter Linux stellen ein Laufwerk automatisch unter „/mnt/resource“ bereit. Dies ist ein nicht permanentes Laufwerk, das durch lokale Datenträger im Azure-Serverknoten gesichert wird. Da es nicht permanent ist, bedeutet dies, dass alle Änderungen am Inhalt unter „/mnt/resource“ verloren gehen, wenn die VM neu gestartet wird. „Alle Änderungen“ beziehen sich u. a. auf gespeicherte Dateien, erstellte Verzeichnisse und installierte Anwendungen.
>
>

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage – Resilienz
Microsoft Azure Storage speichert die Basis-VM (mit Betriebssystem) und angefügte Datenträger oder BLOBs in mindestens drei getrennten Speicherknoten. Das wird als „lokaler redundanter Speicher „ (LRS) bezeichnet. LRS ist die Standardeinstellung für alle Azure-Speichertypen.

Es gibt mehrere weitere Redundanzmethoden, die im Artikel [Azure Storage-Replikation](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) beschrieben werden.

> [!NOTE]
>Ab Azure Storage Premium, dem empfohlenen Speichertyp für DBMS-VMs und Datenträger, die Datenbank- und Protokoll-/Wiederholungsdateien speichern, ist LRS die einzige verfügbare Methode. Daher müssen Sie Datenbankmethoden wie SQL Server Always On, Oracle Data Guard oder HANA-Systemreplikation konfigurieren, um die Datenbank-Datenreplikation in einer anderen Azure-Region oder -Verfügbarkeitszone zu aktivieren.


> [!NOTE]
> Für DBMS-Bereitstellungen wird die Verwendung von georedundantem Speicher, verfügbar mit Azure-Standardspeicher, nicht empfohlen, da er erhebliche Leistungseinbußen zur Folge hat und die Schreibreihenfolge auf verschiedenen VHDs, die an eine VM angeschlossen sind, nicht berücksichtigt. Das Ignorieren der Schreibreihenfolge über verschiedene VHDs hinweg erhöht das Risiko für inkonsistente Datenbanken auf Seiten des Replikationsziels, wenn Datenbank- und Protokoll-/Wiederholungsdateien über mehrere VHDs (wie üblich) auf der Quell-VM verteilt werden.



## <a name="vm-node-resiliency"></a>VM-Knoten – Resilienz
Azure Platform bietet verschiedene SLAs für VMs. Ausführlichere Informationen finden Sie in der neuesten Version von [SLA für Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). Da die DBMS-Schicht in der Regel ein wichtiger Bestandteil für die Verfügbarkeit eines SAP-Systems ist, sollten Sie die Konzepte der Verfügbarkeitsgruppen, Verfügbarkeitszonen und Wartungsereignisse kennen. Weitere Informationen zu diesen Konzepten finden Sie in den Artikeln [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) und [Verwalten der Verfügbarkeit virtueller Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

Für produktive DBMS-Szenarien mit SAP-Workload wird empfohlen, mindestens folgende Aspekte zu beachten:

- Bereitstellen von zwei VMs in einer separaten Verfügbarkeitsgruppe in derselben Azure-Region.
- Diese beiden VMs werden im selben Azure-VNET ausgeführt, und ihnen sind Netzwerkadapter aus demselben Subnetz angefügt.
- Verwenden Sie Datenbankmethoden, um einen unmittelbar betriebsbereiten Standbyserver mit der zweiten VM beizubehalten. Mögliche Methoden sind SQL Server Always On, Oracle Data Guard oder HANA-Systemreplikation.

Darüber hinaus können Sie eine dritte VM in einer anderen Azure-Region bereitstellen und die gleichen Datenbankmethoden verwenden, um ein asynchrones Replikat in einer anderen Azure-Region anzugeben.

Dieses [Tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) zeigt, wie Sie Azure-Verfügbarkeitsgruppen einrichten.



## <a name="azure-network-considerations"></a>Überlegungen zum Azure-Netzwerk
Für große SAP-Bereitstellungen wird empfohlen, dass Sie die Blaupause aus [Virtuelles Rechenzentrum in Microsoft Azure](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) für die VNET-Konfiguration, die Berechtigungen und die Rollenzuweisungen zu verschiedenen Organisationsteilen verwenden.

Es gibt mehrere Best Practices, die infolge von Hunderten von Kundenbereitstellungen entstanden sind:

- Die VNETs, in denen die SAP-Anwendung bereitgestellt wird, haben keinen Zugriff auf das Internet.
- Die Datenbank-VMs werden im selben VNET ausgeführt wie die Anwendungsschicht.
- Die VMs im VNET haben eine statische Zuordnung der privaten IP-Adresse. Weitere Informationen finden Sie im Artikel [IP-Adresstypen und Zuordnungsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm).
- Routingeinschränkungen in und aus DBMS-VMs werden **NICHT** mit Firewalls festgelegt, die auf den lokalen DBMS-VMs installiert sind. Stattdessen wird Routing mit [Azure-Netzwerksicherheitsgruppen (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) definiert.
- Weisen Sie der VM verschiedene Netzwerkadapter zu, um den Datenverkehr zur DBMS-VM zu trennen und zu isolieren. Jeder Netzwerkadapter hat eine andere IP-Adresse und ist einem anderen VNET-Subnetz zugeordnet, das wiederum andere NSG-Regeln hat. Bedenken Sie, dass die Isolation oder Trennung des Datenverkehrs nur eine Routingmaßnahme ist und nicht das Festlegen von Kontingenten für den Netzwerkdurchsatz ermöglicht.

> [!NOTE]
> Sie sollten einzelnen virtuellen Netzwerkkarten statische IP-Adressen über Azure-Tools zuweisen. Virtuellen Netzwerkkarten sollten nicht innerhalb des Gastbetriebssystems statische IP-Adressen zugewiesen werden. Einige Azure-Dienste wie der Azure Backup-Dienst basieren darauf, dass mindestens die primäre virtuelle Netzwerkkarte auf DHCP festgelegt ist und nicht auf statische IP-Adressen. Weitere Informationen finden Sie auch im Dokument zur [Problembehandlung bei der Sicherung virtueller Azure-Computer](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Wenn Sie einer VM mehrere statische IP-Adressen zuweisen müssen, müssen Sie ihr auch mehrere virtuelle Netzwerkkarten zuweisen.
>


> [!IMPORTANT]
> Aus Funktionalitätsgründen und – was noch wichtiger ist – aus Leistungsgründen wird die Konfiguration von [virtuellen Azure-Netzwerkgeräten](https://azure.microsoft.com/solutions/network-appliances/) im Kommunikationspfad zwischen der SAP-Anwendung und der DBMS-Schicht eines SAP NetWeaver-, Hybris- oder S/4HANA-basierten SAP-Systems nicht unterstützt. Die Kommunikation zwischen der SAP-Anwendungsschicht und der DBMS-Schicht muss direkt erfolgen. Die Einschränkung gilt nicht für [Azure ASG und NSG-Regeln](https://docs.microsoft.com/azure/virtual-network/security-overview), solange diese ASG- und NSG-Regeln eine direkte Kommunikation ermöglichen. Weitere Szenarien, in denen virtuelle Netzwerkgeräte nicht unterstützt werden, betreffen Kommunikationspfade zwischen virtuellen Azure-Computern, die Linux Pacemaker-Clusterknoten und SBD-Geräte darstellen. Dies ist unter [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs auf dem SUSE Linux Enterprise Server for SAP Applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) beschrieben. Oder in Kommunikationspfaden zwischen Azure-VMs und Windows Server SOFS, die wie unter [Gruppieren einer SAP ASCS/SCS-Instanz in einem Windows-Failovercluster per Dateifreigabe in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share) beschrieben eingerichtet wurden. Mit virtuellen Netzwerkgeräten in Kommunikationspfaden kann die Netzwerklatenz zwischen zwei Kommunikationspartnern auf einfache Weise verdoppelt werden. Außerdem kann der Durchsatz in kritischen Pfaden zwischen der SAP-Anwendungsschicht und der DBMS-Schicht eingeschränkt werden. In einigen Kundenszenarien kann es aufgrund von virtuellen Netzwerkgeräten für Pacemaker Linux-Cluster zu Ausfällen kommen, bei denen die Kommunikation zwischen den Linux Pacemaker-Clusterknoten und dem SBD-Gerät über ein virtuelles Netzwerkgerät erfolgen muss.
>

> [!IMPORTANT]
> Ein weiteres **NICHT** unterstütztes Design ist die Aufteilung der SAP-Anwendungsschicht und der DBMS-Schicht in verschiedene virtuelle Azure-Netzwerke, für die kein [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) konfiguriert ist. Es wird empfohlen, die SAP-Anwendungsschicht und die DBMS-Schicht durch Subnetze innerhalb eines virtuellen Azure-Netzwerks zu trennen, statt verschiedene virtuelle Azure-Netzwerke zu verwenden. Wenn Sie sich dafür entscheiden, der Empfehlung nicht zu folgen und stattdessen die beiden Schichten in verschiedene virtuelle Netzwerke aufzuteilen, muss für die beiden virtuellen Netzwerke ein [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) konfiguriert sein. Beachten Sie, dass für den Netzwerkdatenverkehr zwischen zwei virtuellen Azure-Netzwerken mit [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Übertragungskosten anfallen. Durch das riesige Datenvolumen im Terabytebereich, das zwischen der SAP-Anwendungsschicht und der DBMS-Schicht ausgetauscht wird, können erhebliche Kosten anfallen, wenn die SAP-Anwendungsschicht und die DBMS-Schicht in zwei virtuellen Azure-Netzwerken voneinander getrennt werden.

Wenn Sie zwei VMs für Ihre DBMS-Produktionsbereitstellung in einer Azure-Verfügbarkeitsgruppe und ein separates Routing für die SAP-Anwendungsschicht sowie den Datenverkehr für die Verwaltung und Vorgänge zu den beiden DBMS-VMs verwenden, würde das Diagramm etwa so aussehen:

![Diagramm: zwei VMs in zwei Subnetzen](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="azure-load-balancer-for-redirecting-traffic"></a>Azure Load Balancer-Instanz zum Umleiten von Datenverkehr
Die Verwendung von privaten virtuellen IP-Adressen, die in Funktionen wie SQL Server Always On oder HANA-Systemreplikation verwendet werden, erfordert die Konfiguration einer Azure Load Balancer-Instanz. Die Azure Load Balancer-Instanz kann mithilfe von Testports den aktiven DBMS-Knoten ermitteln und den Datenverkehr ausschließlich an diesen aktiven Datenbankknoten weiterleiten. Bei einem Failover des Datenbankknotens muss die SAP-Anwendung nicht erneut konfiguriert werden. Stattdessen werden die gängigsten SAP-Anwendungsarchitekturen wieder mit der privaten virtuellen IP-Adresse verbunden. Inzwischen hat die Azure Load Balancer-Instanz auf das Failover des Knotens reagiert, indem er den Datenverkehr an die private virtuelle IP-Adresse auf dem zweiten Knoten umgeleitet hat.

Azure bietet zwei verschiedene [Load Balancer-SKUs](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview): eine Basic-SKU und eine Standard-SKU. Wenn Sie nicht über Azure-Verfügbarkeitszonen hinweg bereitstellen möchten, ist die Load-Balancer-SKU „Basic“ vollkommen ausreichend.

Wird der Datenverkehr zwischen den DBMS-VMs und der SAP-Anwendungsschicht immer über Azure Load Balancer weitergeleitet? Die Antwort hängt von der Load Balancer-Konfiguration ab. Zu diesem Zeitpunkt wird der eingehende Datenverkehr auf der DBMS-VM immer über eine Azure Load Balancer-Instanz weitergeleitet. Der ausgehende Datenverkehr von der DBMS-VM zur Anwendungsschicht-VM hängt von der Konfiguration der Azure Load Balancer-Instanz ab. Die Load Balancer-Instanz bietet eine Möglichkeit, DirectServerReturn. Wenn diese Option konfiguriert ist, wird der Datenverkehr von der DBMS-VM zur SAP-Anwendungsschicht **NICHT** über die Azure Load Balancer-Instanz weitergeleitet. Stattdessen geht er direkt zur Anwendungsschicht. Wenn DirectServerReturn nicht konfiguriert ist, wird der zur SAP-Anwendungsschicht zurückkommende Datenverkehr über die Azure Load Balancer-Instanz weitergeleitet.

Es wird empfohlen, DirectServerReturn in Kombination mit den Azure Load Balancer-Instanzen zu konfigurieren, die zwischen der SAP-Anwendungsschicht und der DBMS-Schicht positioniert sind, um die Netzwerklatenz zwischen den beiden Schichten zu reduzieren.

Ein Beispiel für das Einrichten einer solchen Konfiguration für SQL Server Always On finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Wenn Sie veröffentlichte GitHub-JSON-Vorlagen als Referenz für Ihre SAP-Infrastrukturbereitstellungen in Azure verwenden möchten, sehen Sie sich diese [Vorlage für ein dreischichtiges SAP-System](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md) an. In dieser Vorlage finden Sie auch die korrekten Einstellungen für die Azure Load Balancer-Instanz.

### <a name="azure-accelerated-networking"></a>Beschleunigter Netzwerkbetrieb in Azure
Um die Netzwerklatenz zwischen Azure-VMs weiter zu reduzieren, wird dringend empfohlen, die Option [Beschleunigter Netzwerkbetrieb in Azure](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) auszuwählen, wenn Sie Azure-VMs für SAP-Workload bereitstellen. Dies gilt insbesondere für die SAP-Anwendungsschicht und die SAP-DBMS-Schicht.

> [!NOTE]
> Nicht alle VM-Typen unterstützen den beschleunigten Netzwerkbetrieb. Der referenzierte Artikel führt die VM-Typen auf, die den beschleunigten Netzwerkbetrieb unterstützen.
>

- - -
> ![Windows][Logo_Windows] Windows
>
> Der Artikel [Erstellen eines virtuellen Windows-Computers mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) behandelt die Verwendung unter Windows. Dort erfahren Sie mehr über die Konzepte und die Bereitstellung von VMs mit beschleunigtem Netzwerkbetrieb.
>
> ![Linux][Logo_Linux] Linux
>
> Im Artikel [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) finden Sie weitere Informationen zur Verwendung unter Linux.
>
>

- - -

> [!NOTE]
> Im Falle von SUSE, Red Hat und Oracle Linux wird der beschleunigte Netzwerkbetrieb von den aktuellen Releases unterstützt. Ältere Releases wie SLES 12 SP2 oder RHEL 7.2 unterstützen nicht den beschleunigten Netzwerkbetrieb in Azure.
>


## <a name="deployment-of-host-monitoring"></a>Bereitstellen der Hostüberwachung
Für die produktive Verwendung von SAP-Anwendungen auf Azure-VMs benötigt SAP die Möglichkeit, Hostüberwachungsdaten von den physischen Hosts abzurufen, auf denen die Azure-VMs ausgeführt werden. Es wird eine bestimmte SAP-Host-Agent-Patchebene benötigt, die diese Funktion in SAPOSCOL und SAP-Host-Agent ermöglicht. Die Patchebenen sind in SAP-Hinweis [1409604]genau dokumentiert.

Weitere Informationen zur Bereitstellung von Komponenten, die Hostdaten für SAPOSCOL und den SAP-Host-Agent bereitstellen, sowie zur Verwaltung des Lebenszyklus dieser Komponenten finden Sie im [Bereitstellungshandbuch][deployment-guide].


## <a name="next-steps"></a>Nächste Schritte
Die Dokumentation zu bestimmten DBMS finden Sie in diesen Artikeln:

- [Azure Virtual Machines – SQL Server-DBMS-Bereitstellung für SAP-Workload](dbms_guide_sqlserver.md)
- [Azure Virtual Machines – Oracle-DBMS-Bereitstellung für SAP-Workload](dbms_guide_oracle.md)
- [Azure Virtual Machines – IBM DB2-DBMS-Bereitstellung für SAP-Workload](dbms_guide_ibm.md)
- [Azure Virtual Machines – SAP ASE-DBMS-Bereitstellung für SAP-Workload](dbms_guide_sapase.md)
- [Bereitstellung von SAP MaxDB, SAP liveCache und SAP Content Server in Azure](dbms_guide_maxdb.md)
- [SAP HANA in Azure – Betriebshandbuch](hana-vm-operations.md)
- [Hochverfügbarkeit von SAP HANA für virtuelle Azure-Computer](sap-hana-availability-overview.md)
- [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md)


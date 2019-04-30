---
title: SAP HANA-Infrastrukturkonfigurationen und -Vorgänge in Azure | Microsoft-Dokumentation
description: Betriebshandbuch für SAP HANA-Systeme, die auf virtuellen Azure-Computern bereitgestellt werden.
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
ms.openlocfilehash: acccc553c5b63b2acd0f9793b0397b25145449dd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699329"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>SAP HANA-Infrastrukturkonfigurationen und -Vorgänge in Azure
In diesem Artikel erfahren Sie, wie Sie die Azure-Infrastruktur konfigurieren und SAP HANA-Systeme betreiben, die auf nativen virtuellen Azure-Computern bereitgestellt werden. Der Artikel enthält auch Informationen zur Konfiguration für die horizontale SAP HANA-Skalierung für die VM-SKU M128s. Dieser Artikel ersetzt nicht die SAP-Standarddokumentation, zu der folgende Inhalte gehören:

- [SAP-Administratorhandbuch](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-Installationshandbücher](https://service.sap.com/instguides)
- [SAP-Hinweise](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Voraussetzungen
Um dieses Handbuch zu verwenden, benötigen Sie grundlegende Kenntnisse der folgenden Azure-Komponenten:

- [Dokumentation zu virtuellen Computern](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-Netzwerk und virtuelle Netzwerke](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage (in englischer Sprache)](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Weiterführende Informationen zu SAP NetWeaver und zu anderen SAP-Komponenten in Azure finden Sie im Abschnitt [SAP in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) in der [Azure-Dokumentation](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Grundlegende Überlegungen zum Setup
In den folgenden Abschnitten werden grundlegende Einrichtungsüberlegungen für die Bereitstellung von SAP HANA-Systemen auf virtuellen Azure-Computern beschrieben.

### <a name="connect-to-azure-virtual-machines"></a>Herstellen von Verbindungen mit virtuellen Azure-Computern
Wie im [Planungshandbuch für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) dokumentiert, werden zum Herstellen einer Verbindung mit virtuellen Azure-Computern zwei grundlegende Methoden verwendet:

- Herstellen einer Verbindung über das Internet und öffentliche Endpunkte auf einem virtuellen Jumpbox-Computer oder auf dem virtuellen Computer, auf dem SAP HANA ausgeführt wird
- Herstellen einer Verbindung über eine [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) oder Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

Site-to-Site-Konnektivität über VPN oder ExpressRoute ist für Produktionsszenarien erforderlich. Diese Art von Verbindung ist auch für produktionsfremde Szenarien erforderlich, die Produktionsszenarien unterstützen, in denen SAP-Software verwendet wird. Die folgende Abbildung zeigt ein Beispiel zu standortübergreifender Konnektivität:

![Standortübergreifende Konnektivität](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Auswählen der Arten von virtuellen Azure-Computern
Die Arten von virtuellen Azure-Computern, die für Produktionsszenarien verwendet werden können, sind in der [SAP-Dokumentation für IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html) aufgeführt. Für produktionsfremde Szenarien ist eine größere Auswahl nativer virtueller Azure-Computer verfügbar.

>[!NOTE]
> Verwenden Sie für produktionsfremde Szenarien die Arten von virtuellen Computern, die im [SAP-Hinweis 1928533](https://launchpad.support.sap.com/#/notes/1928533) aufgeführt sind. Überprüfen Sie zur Verwendung virtueller Azure-Computer für Produktionsszenarien die von SAP veröffentlichte [Liste zertifizierter IaaS-Plattformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) auf für SAP HANA zertifizierte virtuelle Computer.

Verwenden Sie Folgendes, um die virtuellen Computer in Azure bereitzustellen:

- Das Azure-Portal
- Azure PowerShell-Cmdlets
- Die Azure-Befehlszeilenschnittstelle

Sie können eine vollständig installierte SAP HANA-Plattform auch über die [SAP-Cloudplattform](https://cal.sap.com/) in den Azure-VM-Diensten bereitstellen. Informationen zum Installationsprozess finden Sie unter [Bereitstellen von SAP S/4HANA oder BW/4HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Informationen zur veröffentlichten Automatisierung finden Sie in [diesem GitHub-Artikel](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-an-azure-storage-type"></a>Auswählen eines Azure Storage-Typs
Azure bietet zwei Arten von Speicher, die sich für virtuelle Azure-Computer mit SAP HANA eignen:  

- Standard-Festplattenlaufwerke (HDDs)
- Premium-SSD-Datenträger

Informationen zu diesen Datenträgertypen finden Sie unter [Auswählen eines Datenträgertyps](../../windows/disks-types.md).

Azure bietet zwei Bereitstellungsmethoden für virtuelle Festplatten (VHDs) in Azure Storage Standard und Storage Premium. Wenn das allgemeine Szenario dies zulässt, sollten Sie Bereitstellungen mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/) verwenden.

Eine Liste der Speichertypen und deren Vereinbarungen zum Servicelevel für IOPS- und Speicherdurchsatz finden Sie in der [Azure-Dokumentation für verwaltete Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configure-the-storage-for-azure-virtual-machines"></a>Konfigurieren des Speichers für virtuelle Azure-Computer

Wahrscheinlich haben Sie sich bislang nie mit E/A-Subsystemen und deren Funktionen auseinandergesetzt, da der Gerätehersteller dafür gesorgt hat, dass die Mindestspeicheranforderungen für SAP HANA erfüllt wurden. Wenn Sie die Azure-Infrastruktur selbst erstellen, müssen Sie mit einigen dieser Anforderungen vertraut sein. Zudem sollten Sie die Konfigurationsanforderungen kennen, die in den folgenden Abschnitten vorgestellt werden. Falls Sie die virtuellen Computer konfigurieren, auf denen SAP HANA ausgeführt werden soll, müssen Sie unter Umständen folgende Schritte ausführen:

- Aktivieren des Lese-/Schreibvolumens in „/hana/log“ mit mindestens 250 MB/s für E/A-Größen von 1 MB
- Aktivieren der Leseaktivität mit mindestens 400 MB/s in „/hana/data“ für E/A-Größen von 16 MB und 64 MB
- Aktivieren der Schreibaktivität mit mindestens 250 MB/s in „/hana/data“ für E/A-Größen von 16 MB und 64 MB

Für DBMS-Systeme ist eine geringe Speicherlatenz wichtig, auch wenn DBMS (ebenso wie SAP HANA) Daten im Arbeitsspeicher behält. Der kritische Pfad beim Speichern sind in der Regel die Schreibvorgänge in das Transaktionsprotokoll der DBMS-Systeme. Aber auch Vorgänge wie das Schreiben von Sicherungspunkten oder das Laden von Daten in den Arbeitsspeicher nach der Wiederherstellung nach einem Systemabsturz können wichtig sein. 

Für die Volumes „/hana/data“ und „/hana/log“ müssen Azure-Premium-Datenträger verwendet werden. Um den von SAP gewünschten Mindestdurchsatz von „/hana/log“ und „/hana/data“ zu erreichen, müssen Sie mithilfe von mdadm oder mithilfe eines Managers für logische Volumes (Logical Volume Manager, LVM) über mehrere Azure Storage Premium-Datenträger ein RAID 0-Volume erstellen und die RAID-Volumes als die Volumes „/hana/data“ und „/hana/log“ verwenden. Für das RAID 0-Volume werden folgende Stripegrößen empfohlen:

- 64 KB oder 128 KB für „/hana/data“
- 32 KB für „/hana/log“

> [!NOTE]
> Sie müssen keine Redundanzebenen mit RAID-Volumes konfigurieren, da Azure Storage Premium und Storage Standard drei Images einer virtuellen Festplatte beibehalten. Die Verwendung eines RAID-Volumes dient lediglich dazu, Volumes mit genügend E/A-Durchsatz zu konfigurieren.

Die Kumulierung einer Reihe von Azure-VHDs unter einem RAID-Volume ist für den IOPS- und Speicherdurchsatz kumulativ. Wenn Sie also ein RAID 0-Volume über drei Azure Storage Premium-P30-Datenträger platzieren, erhalten Sie den dreifachen IOPS- und den dreifachen Speicherdurchsatz eines einzelnen Azure Storage Premium-P30-Datenträgers.

Bei den folgenden Cacheempfehlungen werden die E/A-Merkmale für SAP HANA zugrunde gelegt:

- Für die HANA-Datendateien entsteht so gut wie keine Arbeitsauslastung durch Lesevorgänge. Ausnahmen sind umfangreiche E/A-Vorgänge nach dem Neustart der HANA-Instanz oder beim Laden von Daten in HANA. Ein anderer Fall umfangreicherer Lese-E/As für Datendateien sind Sicherungen der HANA-Datenbank. Das Zwischenspeichern von Lesevorgängen ist somit nicht sinnvoll, da in den meisten Fällen alle Datendateivolumes vollständig gelesen werden müssen.
- Das Schreiben in die Datendateien geschieht in Bursts auf der Grundlage der HANA-Sicherungspunkte und der HANA-Absturzwiederherstellung. Das Schreiben von Sicherungspunkten erfolgt asynchron und verzögert keine Benutzertransaktionen. Beim Schreiben von Daten während der Wiederherstellung nach Abstürzen ist die Leistung ein kritischer Faktor, um schnell wieder zu einem reaktionsbereiten System zu kommen. Die Wiederherstellung nach Abstürzen dürfte eher eine Ausnahmesituation darstellen.
- Es gibt kaum Lesevorgänge aus den HANA-Wiederholungsdateien. Ausnahmen sind umfangreiche E/A-Vorgänge beim Ausführen von Sicherungen des Transaktionsprotokolls, der Wiederherstellung nach Abstürzen oder in der Neustartphase einer HANA-Instanz.  
- Den Großteil der Last im Zusammenhang mit der SAP HANA-Wiederholungsprotokolldatei machen Schreibvorgänge aus. Je nach Art der Workload kann die E/A-Größe zwischen 4 KB und 1 MB oder mehr betragen. Wartezeiten beim Schreiben in das SAP HANA-Wiederholungsprotokoll sind ein kritischer Faktor für die Leistung.
- Alle Schreibvorgänge müssen zuverlässig und dauerhaft auf einem Datenträger gespeichert werden.

Aufgrund dieser E/A-Muster von SAP HANA muss die Zwischenspeicherung für die verschiedenen Volumes, die Azure Storage Premium verwenden, wie folgt festgelegt werden:

- **/hana/data:** Keine Zwischenspeicherung
- **/hana/log:** Keine Zwischenspeicherung, ausgenommen für virtuelle Computer der M-Serie (mehr dazu im weiteren Verlauf des Artikels)
- **/hana/shared:** Zwischenspeicherung von Lesevorgängen


Behalten Sie auch den E/A-Gesamtdurchsatz eines virtuellen Computers im Hinterkopf, wenn Sie die Größe festlegen oder sich für einen virtuellen Computer entscheiden. Der Gesamtspeicherdurchsatz virtueller Computer ist unter [Arbeitsspeicheroptimierte Größen virtueller Computer](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) dokumentiert.

#### <a name="linux-io-scheduler-mode"></a>E/A-Scheduler-Modus für Linux
Linux verfügt über mehrere verschiedene E/A-Scheduling-Modi. Linux-Anbieter und SAP empfehlen im Allgemeinen, den E/A-Scheduler-Modus für Datenträgervolumes von **cfq** in **noop** zu ändern. Weitere Informationen finden Sie im [SAP-Hinweis 1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-write-accelerator-for-azure-m-series-virtual-machines"></a>Speicherlösung mit Schreibbeschleunigung für virtuelle Azure-Computer der M-Serie
 Die Schreibbeschleunigung ist ein Azure-Feature, das exklusiv für virtuelle Azure-Computer der M-Serie eingeführt wird. Die Funktion dient zur Verbesserung der E/A-Wartezeit bei Schreibvorgängen für Azure Storage Premium. Für SAP HANA ist die Schreibbeschleunigung nur für das Volume „/hana/log“ vorgesehen. Daher müssen die bisher gezeigten Konfigurationen geändert werden. Die Hauptänderung besteht in der Aufteilung von „/hana/data“ und „/hana/log“, um die Schreibbeschleunigung nur für das Volume „/hana/log“ zu verwenden. 

> [!IMPORTANT]
> Die SAP HANA-Zertifizierung für virtuelle Azure-Computer der M-Serie gilt ausschließlich mit Schreibbeschleunigung für das Volume „/hana/log“. Folglich müssen SAP HANA-Bereitstellungen in Produktionsszenarien auf virtuellen Azure-Computern der M-Serie mit Schreibbeschleunigung für das Volume „/hana/log“ konfiguriert werden.

> [!NOTE]
> Überprüfen Sie für Produktionsszenarien in der [SAP-Dokumentation für IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), ob ein bestimmter VM-Typ von SAP für SAP HANA unterstützt wird.

Die folgende Tabelle enthält die empfohlenen Konfigurationen:

| VM-SKU | RAM | E/A-Maximalwert für den virtuellen Computer<br /> throughput | /hana/data | /hana/log | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GB | 1.000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1.000 GiB | 1.000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1.750 GiB | 1.000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2.000 GiB | 2.000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3.800 GiB | 2.000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Überprüfen Sie, ob der Speicherdurchsatz für die verschiedenen vorgeschlagenen Volumes für die Workload ausreicht, die Sie ausführen möchten. Wenn die Workload größere Volumes für „/hana/data“ und „/hana/log“ erfordert, erhöhen Sie die Anzahl von Azure Storage Premium-VHDs. Wenn Sie ein Volume mit mehr VHDs ausstatten als in der Liste angegeben, erhöht sich der IOPS- und E/A-Durchsatz innerhalb der Grenzen des Azure-VM-Typs.

Die Schreibbeschleunigung funktioniert nur in Verbindung mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/). Daher müssen zumindest die Azure Storage Premium-Datenträger, die das Volume „/hana/log“ bilden, als verwaltete Datenträger bereitgestellt werden.

Von der Schreibbeschleunigung wird pro virtuellem Computer nur eine begrenzte Anzahl von Azure Storage Premium-VHDs unterstützt. Die aktuellen Limits lauten wie folgt:

- 16 VHDs für einen virtuellen Computer vom Typ M128xx
- 8 VHDs für einen virtuellen Computer vom Typ M64xx
- 4 VHDs für einen virtuellen Computer vom Typ M32xx

Weitere Informationen zum Aktivieren der Schreibbeschleunigung finden Sie unter [Aktivieren der Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

In der gleichen Dokumentation finden Sie auch weitere Informationen zur Schreibbeschleunigung sowie zu den damit verbundenen Einschränkungen.


#### <a name="cost-conscious-azure-storage-configuration"></a>Kostenbewusste Azure Storage-Konfiguration
In der folgenden Tabelle ist eine Konfiguration mit VM-Typen aufgeführt, die Kunden häufig verwenden, um SAP HANA auf Azure-VMs zu hosten. Einige VM-Typen erfüllen unter Umständen nicht alle Mindestkriterien für SAP HANA. Und einige VM-Typen werden von SAP möglicherweise nicht offiziell für die Verwendung mit SAP HANA unterstützt. Diese virtuellen Computer haben in produktionsfremden Szenarien bislang gut funktioniert. 

> [!NOTE]
> Überprüfen Sie für Produktionsszenarien in der [SAP-Dokumentation für IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), ob ein bestimmter VM-Typ von SAP für SAP HANA unterstützt wird.


| VM-SKU | RAM | E/A-Maximalwert für den virtuellen Computer<br /> throughput | „/hana/data“ und „/hana/log“<br /> Striping mit LVM oder mdadm | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GB | 384 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 432 GiB | 1.200 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2.000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GB | 1.000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1.000 GiB | 1.000 MB/s | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1.750 GiB | 1.000 MB/s | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2.000 GiB | 2.000 MB/s |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3.800 GiB | 2.000 MB/s | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Die Datenträger, die für die kleineren VM-Typen mit 3 x P20 empfohlen werden, haben Übergröße im Vergleich zu den Speicherplatzempfehlungen, die im [SAP-Whitepaper zu TDI-Speicher](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) angegeben sind. Die in der Tabelle gezeigte Wahl wurde getroffen, um genügend Datenträgerdurchsatz für SAP HANA bereitzustellen. Sie können problemlos Anpassungen am Volume **/hana/backup** vornehmen, dessen Größe so festgelegt wurde, dass es Sicherungen aufnehmen kann, die dem Zweifachen der Arbeitsspeichergröße entsprechen. 

Überprüfen Sie, ob der Speicherdurchsatz für die verschiedenen vorgeschlagenen Volumes für die Workload ausreicht, die Sie ausführen möchten. Wenn die Workload größere Volumes für „/hana/data“ und „/hana/log“ erfordert, erhöhen Sie die Anzahl von Azure Storage Premium-VHDs. Wenn Sie ein Volume mit mehr VHDs ausstatten als in der Liste angegeben, erhöht sich der IOPS- und E/A-Durchsatz innerhalb der Grenzen des Azure-VM-Typs. 

> [!NOTE]
> Die vorherigen Konfigurationen profitieren nicht von der [Vereinbarung zum Servicelevel für einzelne virtuelle Azure-Computer](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), da Azure Storage Premium und Azure Storage Standard gemeinsam verwendet werden. Die Auswahl wurde aus Kostengründen getroffen. Wählen Sie für alle Datenträger, die in der Tabelle als Azure Storage Standard (Sxx) angegeben sind, Storage Premium aus, um die Konfiguration des virtuellen Computers mit der Vereinbarung zum Servicelevel für einen einzelnen virtuellen Azure-Computer konform zu machen.


> [!NOTE]
> Der Empfehlungen für die Datenträgerkonfiguration dienen zur Erfüllung der Mindestanforderungen, die SAP gegenüber Infrastrukturanbietern angibt. In echten Kundenbereitstellungen und Workloadszenarien waren diese Empfehlungen in bestimmten Situationen nicht ausreichend. Bei einem Kunden mussten beispielsweise die Daten nach einem Neustart von HANA schneller neu geladen werden, und für Sicherungskonfigurationen war eine höhere Speicherbandbreite erforderlich. In anderen Fällen waren bei einer bestimmten Workload 5.000 IOPS für „/hana/log“ nicht ausreichend. Betrachten Sie diese Empfehlungen daher als Ausgangspunkt, und nehmen Sie Anpassungen auf Grundlage der Anforderungen der Workload vor.
>  

### <a name="set-up-azure-virtual-networks"></a>Einrichten von virtuellen Azure-Netzwerken
Wenn Sie über Site-to-Site-Konnektivität mit Azure per VPN oder Azure ExpressRoute verfügen, benötigen Sie mindestens ein virtuelles Azure-Netzwerk, das über ein virtuelles Gateway mit der VPN- oder ExpressRoute-Verbindung verbunden ist. Bei einfachen Bereitstellungen kann das virtuelle Gateway in einem Subnetz des virtuellen Azure-Netzwerks bereitgestellt werden, das auch die SAP HANA-Instanzen hostet. 

Um SAP HANA zu installieren, erstellen Sie zwei weitere Subnetze innerhalb des virtuellen Azure-Netzwerks. In einem Subnetz werden die virtuellen Computer gehostet, auf denen die SAP HANA-Instanzen ausgeführt werden. Im anderen Subnetz werden virtuelle Jumpbox- oder Verwaltungscomputer ausgeführt, auf denen SAP HANA Studio, andere Verwaltungssoftware oder Ihre Anwendungssoftware gehostet wird.

> [!IMPORTANT]
> Das Konfigurieren [virtueller Azure-Netzwerkgeräte](https://azure.microsoft.com/solutions/network-appliances/) im Kommunikationspfad zwischen der SAP-Anwendung und der DBMS-Schicht eines SAP NetWeaver-, Hybris- oder S/4HANA-basierten SAP-Systems wird nicht unterstützt. Diese Einschränkung gilt aus Funktions- und Leistungsgründen. Der Kommunikationspfad zwischen der SAP-Anwendungsschicht und der DBMS-Schicht muss ein direkter Pfad sein. Die Einschränkung gilt nicht für [Regeln für Anwendungssicherheitsgruppen (ASGs) und Netzwerksicherheitsgruppen (NSGs)](https://docs.microsoft.com/azure/virtual-network/security-overview), wenn diese ASG- und NSG-Regeln einen direkten Kommunikationspfad zulassen. 
>
> Weitere Szenarien, in denen virtuelle Netzwerkgeräte nicht unterstützt werden: 
>
> - Kommunikationspfade zwischen virtuellen Azure-Computern, die Linux Pacemaker-Clusterknoten und SBD-Geräte darstellen. Dies ist unter [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs auf dem SUSE Linux Enterprise Server for SAP Applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) beschrieben.
> - Kommunikationspfade zwischen virtuellen Azure-Computern und einem Windows Server-Dateiserver mit horizontaler Skalierung, die wie unter [Gruppieren einer SAP ASCS/SCS-Instanz in einem Windows-Failovercluster per Dateifreigabe in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share) beschrieben eingerichtet wurden. 
>
> Virtuelle Netzwerkgeräte in Kommunikationspfaden können die Netzwerklatenz zwischen zwei Kommunikationspartnern locker verdoppeln. Außerdem können sie den Durchsatz in kritischen Pfaden zwischen der SAP-Anwendungsschicht und der DBMS-Schicht einschränken. In einigen Kundenszenarien können virtuelle Netzwerkgeräte fehlerhafte Linux Pacemaker-Clustern zur Folge zu haben. In diesen Fällen erfolgt die Kommunikation zwischen den Linux Pacemaker-Clusterknoten und dem SBD-Gerät über ein virtuelles Netzwerkgerät.  
> 

> [!IMPORTANT]
> Ein weiterer *nicht* unterstützter Entwurf ist die Aufteilung der SAP-Anwendungsschicht und der DBMS-Schicht auf verschiedene virtuelle Azure-Netzwerke, für die kein [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) konfiguriert ist. Es wird empfohlen, die SAP-Anwendungsschicht und die DBMS-Schicht durch Subnetze innerhalb eines virtuellen Azure-Netzwerks zu trennen, anstatt verschiedene virtuelle Azure-Netzwerke zu verwenden. 
>
>Wenn Sie sich dafür entscheiden, der Empfehlung nicht zu folgen und stattdessen die beiden Schichten auf verschiedene virtuelle Netzwerke aufzuteilen, muss für die beiden virtuellen Netzwerke ein [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) konfiguriert werden. 
>
> Beachten Sie, dass für den Netzwerkdatenverkehr zwischen zwei virtuellen Azure-Netzwerken mit [Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Übertragungskosten anfallen. Zwischen der SAP-Anwendungsschicht und der DBMS-Schicht werden riesige Datenmengen im Terabytebereich ausgetauscht. Somit können erhebliche Kosten entstehen, wenn die SAP-Anwendungsschicht und die DBMS-Schicht auf zwei mittels Peering verknüpfte virtuelle Azure-Netzwerke aufgeteilt sind. 

Wenn Sie die virtuellen Computer zum Ausführen von SAP HANA installieren, benötigen die virtuellen Computer Folgendes:

- Zwei installierte virtuelle NICs: Eine NIC ist mit dem Verwaltungssubnetz verbunden. Eine weitere NIC wird verwendet, um vom lokalen Netzwerk oder von anderen Netzwerken aus eine Verbindung mit der SAP HANA-Instanz auf dem virtuellen Azure-Computer herzustellen.
- Statische private IP-Adressen, die für beide virtuellen Netzwerkkarten bereitgestellt werden

> [!NOTE]
> Statische IP-Adressen über Azure zuzuweisen bedeutet, sie einzelnen virtuellen NICs zuzuweisen. Weisen Sie statische IP-Adressen nicht innerhalb des Gastbetriebssystems einer virtuellen NIC zu. Einige Azure-Dienste (beispielsweise Azure Backup) sind darauf angewiesen, dass mindestens die primäre virtuelle NIC auf DHCP festgelegt ist und nicht auf statische IP-Adressen. Weitere Informationen finden Sie unter [Problembehandlung bei der Sicherung virtueller Azure-Computer](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Wenn Sie einem virtuellen Computer mehrere statische IP-Adressen zuweisen möchten, müssen Sie ihm mehrere virtuelle NICs zuweisen.
>
>

Erstellen Sie für dauerhafte Bereitstellungen eine virtuelle Datencenter-Netzwerkarchitektur in Azure. Bei dieser Architektur empfiehlt sich die Trennung des virtuellen Azure-Netzwerkgateways, das die Verbindung mit der lokalen Umgebung herstellt, in ein separates virtuelles Azure-Netzwerk. Dieses separate virtuelle Netzwerk hostet den gesamten ausgehenden Datenverkehr, der entweder an die lokale Umgebung oder an das Internet geleitet wird. Mit diesem Ansatz können Sie Software zur Überprüfung und Protokollierung des eingehenden Datenverkehrs für das virtuelle Datencenter in Azure in diesem separaten virtuellen Hub-Netzwerk bereitstellen. Dadurch verfügen Sie über ein virtuelles Netzwerk, das sämtliche Software und Konfigurationen bezüglich des ein- und ausgehenden Datenverkehrs für Ihre Azure-Bereitstellung hostet.


Weitere Informationen zum Konzept des virtuellen Datencenters und zum dazugehörigen Entwurf eines virtuellen Azure-Netzwerks finden Sie hier: 

- [Azure virtual datacenter: A network perspective](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) (Virtuelles Azure-Datencenter: Eine Netzwerkperspektive)
- [Azure virtual datacenter and the enterprise control plane](https://docs.microsoft.com/azure/architecture/vdc/) (Virtuelles Azure-Datencenter und die Steuerungsebene für Unternehmen)


>[!NOTE]
>Für Datenverkehr, der unter Verwendung von [Peering in virtuellen Azure-Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen einem virtuellen Hub-Netzwerk und einem virtuellen Spoke-Netzwerk übertragen wird, fallen zusätzliche [Kosten](https://azure.microsoft.com/pricing/details/virtual-network/) an. Basierend auf diesen Kosten müssen Sie möglicherweise Kompromisse zwischen dem Betrieb eines strengen Hub-Spoke-Netzwerks und dem Betrieb mehrerer [Azure-ExpressRoute-Gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) eingehen, die Sie mit Spokes verbinden, um das Peering virtueller Netzwerke zu umgehen. 
>
> Für Azure-ExpressRoute-Gateways fallen ebenfalls zusätzliche [Kosten](https://azure.microsoft.com/pricing/details/vpn-gateway/) an. Darüber hinaus können zusätzliche Kosten für Software von Drittanbietern anfallen, die Sie für die Protokollierung, Überprüfung und Überwachung von Netzwerkdatenverkehr verwenden. Vergleichen Sie die Kosten für den Datenaustausch über das Peering virtueller Netzwerke mit den Kosten, die durch zusätzliche ExpressRoute-Gateways und Softwarelizenzen entstehen. Sie können sich für eine Mikrosegmentierung innerhalb eines virtuellen Netzwerks entscheiden und dabei Subnetze als Isolationseinheiten verwenden (anstelle von virtuellen Netzwerken).


Einen Überblick über verschiedene Methoden zum Zuweisen von IP-Adressen finden Sie unter [IP-Adresstypen und Zuordnungsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Arbeiten Sie bei virtuellen Computern, auf denen SAP HANA ausgeführt wird, mit den zugewiesenen statischen IP-Adressen. Der Grund ist, dass einige Konfigurationsattribute für HANA auf IP-Adressen verweisen.

[Azure-NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) werden verwendet, um Datenverkehr zu leiten, der an die SAP HANA-Instanz oder an die Jumpbox weitergeleitet wird. Die NSGs und [Anwendungssicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) sind mit dem SAP HANA-Subnetz und dem Verwaltungssubnetz verknüpft.

Die folgende Abbildung zeigt eine Übersicht eines allgemeinen Bereitstellungsschemas für SAP HANA mit einer Hub-Spoke-Architektur für ein virtuelles Netzwerk:

![Allgemeines Bereitstellungsschema für SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Wenn Sie SAP HANA in Azure ohne Site-to-Site-Verbindung bereitstellen möchten, müssen Sie die SAP HANA-Instanz vom öffentlichen Internet abschirmen und hinter einem Weiterleitungsproxy platzieren. In diesem einfachen Szenario werden für die Bereitstellung in Azure integrierte DNS-Dienste verwendet, um Hostnamen aufzulösen. In einer komplexeren Bereitstellung, in der öffentlich zugängliche IP-Adressen verwendet werden, sind die in Azure integrierten DNS-Dienste besonders wichtig. Verwenden Sie Azure-NSGs und [virtuelle Azure-Netzwerkgeräte](https://azure.microsoft.com/solutions/network-appliances/), um das Routing aus dem Internet an Ihre virtuelle Azure-Netzwerkarchitektur in Azure zu überwachen. 

Die folgende Abbildung zeigt ein allgemeines Schema für die Bereitstellung von SAP HANA ohne Site-to-Site-Verbindung in einer Hub-Spoke-Architektur für ein virtuelles Netzwerk:
  
![Allgemeines Bereitstellungsschema für SAP HANA ohne eine Standort-zu-Standort-Verbindung](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Eine weitere Beschreibung der Verwendung virtueller Azure-Netzwerkgeräte zum Steuern und Überwachen des Zugriffs über das Internet ohne Hub-Spoke-Architektur für ein virtuelles Netzwerk finden Sie im Artikel [Bereitstellen hochverfügbarer virtueller Netzwerkgeräte](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configure-azure-infrastructure-for-sap-hana-scale-out"></a>Konfigurieren der Azure-Infrastruktur für die horizontale SAP HANA-Skalierung
Microsoft bietet eine VM-SKU der M-Serie, die für eine Konfiguration mit horizontaler SAP HANA-Skalierung zertifiziert ist. Der VM-Typ M128s wurde für eine horizontale Skalierung von bis zu 16 Knoten zertifiziert. Informationen zu Änderungen bei der Zertifizierung der horizontalen SAP HANA-Skalierung für virtuelle Azure-Computer finden Sie in der [Liste der zertifizierten IaaS-Plattformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Für die Bereitstellung von Konfigurationen mit horizontaler Skalierung auf virtuellen Azure-Computern wird jeweils mindestens die folgende Betriebssystemversion vorausgesetzt:

- SUSE Linux 12 SP3
- Red Hat Linux 7.4

Für die Zertifizierung der horizontalen Skalierung mit 16 Knoten gelten folgende Voraussetzungen:

- Ein Knoten ist der Masterknoten.
- Bis zu 15 Knoten sind Workerknoten.

>[!NOTE]
>In Azure-VM-Bereitstellungen mit horizontaler Skalierung kann kein Standbyknoten verwendet werden.
>

Das Konfigurieren eines Standbyknotens ist aus zwei Gründen nicht möglich:

- Azure bietet an dieser Stelle keinen nativen NFS-Dienst. Daher müssen NFS-Freigaben mithilfe von Drittanbieterfunktionen konfiguriert werden.
- Keine der NFS-Drittanbieterkonfigurationen kann mit den in Azure bereitgestellten Lösungen die Speicherlatenzkriterien für SAP HANA erfüllen.

Daher können die Volumes „/hana/data“ und „/hana/log“ nicht freigegeben werden. Da die Volumes der einzelnen Knoten nicht freigegeben werden, kann in einer Konfiguration mit horizontaler Skalierung auch kein SAP HANA-Standbyknoten verwendet werden.

Die folgende Abbildung zeigt den grundlegenden Entwurf für einen einzelnen Knoten in einer Konfiguration mit horizontaler Skalierung:

![Grundlagen der horizontalen Skalierung eines einzelnen Knotens](media/hana-vm-operations/scale-out-basics.PNG)

Die grundlegende Konfiguration von einem VM-Knoten für die horizontale SAP HANA-Skalierung sieht wie folgt aus:

- Für „/hana/shared“ erstellen Sie einen hoch verfügbaren NFS-Cluster, der auf SUSE Linux 12 SP3 basiert. Dieser Cluster hostet die NFS-Freigaben vom Typ „/hana/shared“ Ihrer Konfiguration mit horizontaler Skalierung sowie SAP NetWeaver oder BW/4HANA Central Services. Informationen zum Erstellen einer solchen Konfiguration finden Sie unter [Hochverfügbarkeit für NFS auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
- Alle anderen Datenträgervolumes werden nicht für die verschiedenen Knoten freigegeben und basieren nicht auf NFS. Installationskonfigurationen und Schritte für HANA-Installationen mit horizontaler Skalierung und nicht freigegebenen Volumes vom Typ „/hana/data“ und „/hana/log“ finden Sie im weiteren Verlauf dieses Artikels.

>[!NOTE]
>Der in der Grafik dargestellte hoch verfügbare NFS-Cluster wird derzeit nur mit SUSE Linux unterstützt. Eine hoch verfügbare NFS-Lösung basierend auf Red Hat wird später erläutert.

Die Größenanpassung der Volumes für die Knoten entspricht der horizontalen Skalierung (mit Ausnahme von „/hana/shared“). Die folgende Tabelle enthält die vorgeschlagenen Größen und Typen für die VM-SKU M128s:

| VM-SKU | RAM | E/A-Maximalwert für den virtuellen Computer<br /> throughput | /hana/data | /hana/log | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2.000 GiB | 2.000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Überprüfen Sie, ob der Speicherdurchsatz für die verschiedenen vorgeschlagenen Volumes für die Workload ausreicht, die Sie ausführen möchten. Wenn die Workload größere Volumes für „/hana/data“ und „/hana/log“ erfordert, erhöhen Sie die Anzahl von Azure Storage Premium-VHDs. Wenn Sie ein Volume mit mehr VHDs ausstatten als in der Liste angegeben, erhöht sich der IOPS- und E/A-Durchsatz innerhalb der Grenzen des Azure-VM-Typs. Wenden Sie auch die Schreibbeschleunigung auf die Datenträger an, die das Volume „/hana/log“ bilden.
 

Eine Formel, die die Größe des Volumes „/hana/shared“ für die horizontale Skalierung als Speichergröße eines einzelnen Workerknotens pro vier Workerknoten definiert, finden Sie unter [SAP HANA-TDI-Speicheranforderungen](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

Bei Verwendung des für die horizontale SAP HANA-Skalierung zertifizierten virtuellen Azure-Computers M128s mit etwa 2 TB Arbeitsspeicher können die SAP-Empfehlungen wie folgt zusammengefasst werden:

- Bei einem Masterknoten und bis zu vier Workerknoten sollte die Größe des Volumes „/hana/shared“ 2 TB betragen.
- Bei einem Masterknoten und fünf bis acht Workerknoten sollte die Größe des Volumes „/hana/shared“ 4 TB betragen.
- Bei einem Masterknoten und neun bis 12 Workerknoten sollte die Größe des Volumes „/hana/shared“ 6 TB betragen.
- Bei einem Masterknoten und 12 bis 15 Workerknoten sollte die Größe des Volumes „/hana/shared“ 8 TB betragen.

Der andere wichtige Aspekt, der in der Grafik mit der Einzelknotenkonfiguration für einen virtuellen SAP HANA-Computer mit horizontaler Skalierung zu sehen ist, ist das virtuelle Netzwerk mit der Subnetzkonfiguration. SAP empfiehlt dringend eine Trennung zwischen dem client- und anwendungsseitigen Datenverkehr und der Kommunikation zwischen den HANA-Knoten. 

Fügen Sie hierzu zwei verschiedene virtuelle NICs an den virtuellen Computer an, wie in der Grafik gezeigt. Beide virtuellen NICs befinden sich in unterschiedlichen Subnetzen und besitzen zwei verschiedene IP-Adressen. Anschließend steuern Sie den Datenverkehr mithilfe von Routingregeln über NSGs oder benutzerdefinierte Routen.

Insbesondere in Azure gibt es keine Möglichkeiten und Methoden, um die Dienstqualität und Kontingente für bestimmte virtuelle NICs zu erzwingen. Daher bietet die Trennung der client- und anwendungsseitigen Kommunikation und der knotenübergreifenden Kommunikation keine Möglichkeit, einen der beiden Datenverkehrsströme zu priorisieren. Stattdessen bleibt die Trennung eine Sicherheitsmaßnahme zur Abschirmung der knotenübergreifenden Kommunikation bei Konfigurationen mit horizontaler Skalierung.  

>[!IMPORTANT]
>SAP empfiehlt dringend, den Netzwerkdatenverkehr auf der Client- und Anwendungsseite und den knotenübergreifenden Datenverkehr zu trennen, wie in diesem Artikel beschrieben. Es empfiehlt sich, eine Architektur wie in der obigen Grafik einzurichten.
>

Die folgende Abbildung zeigt die mindestens erforderliche Netzwerkarchitektur aus der Netzwerkperspektive:

![Grundlagen der horizontalen Skalierung eines einzelnen Knotens](media/hana-vm-operations/scale-out-networking-overview.PNG)

Bislang werden neben einem einzelnen Masterknoten bis zu 15 Workerknoten unterstützt.

Die folgende Abbildung zeigt die Speicherarchitektur aus der Speicherperspektive:


![Grundlagen der horizontalen Skalierung eines einzelnen Knotens](media/hana-vm-operations/scale-out-storage-overview.PNG)

Das Volume „/hana/shared“ befindet sich in einer hoch verfügbaren NFS-Freigabekonfiguration. Alle anderen Laufwerke sind lokal in die einzelnen virtuellen Computer eingebunden. 

### <a name="highly-available-nfs-share"></a>Hoch verfügbare NFS-Freigabe
Der hoch verfügbare NFS-Cluster funktioniert bislang nur mit SUSE Linux. Informationen zur Einrichtung finden Sie unter [Hochverfügbarkeit für NFS auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs). Wenn Sie den NFS-Cluster nicht für andere HANA-Konfigurationen außerhalb des virtuellen Azure-Netzwerks freigeben möchten, in dem die SAP HANA-Instanzen ausgeführt werden, installieren Sie ihn im gleichen virtuellen Netzwerk. Installieren Sie ihn in einem eigenen Subnetz, und stellen Sie sicher, dass nicht jeder beliebige Datenverkehr Zugang zu dem Subnetz hat. Schränken Sie stattdessen den Datenverkehr für dieses Subnetz auf die IP-Adressen des virtuellen Computers ein, über die der Datenverkehr für das Volume „/hana/shared“ abgewickelt wird.

Hinsichtlich der virtuellen NIC eines virtuellen HANA-Computers mit horizontaler Skalierung, der für das Routing des Datenverkehrs von „/hana/shared“ zuständig ist, wird Folgendes empfohlen:

- Da der eingehende Datenverkehr für „/hana/shared“ moderat ist, leiten Sie ihn über die virtuelle NIC, die dem Clientnetzwerk in der Minimalkonfiguration zugewiesen ist.
- Für den bei „/hana/shared“ eingehenden Datenverkehr können Sie letztendlich ein drittes Subnetz in dem virtuellen Netzwerk bereitstellen, in dem Sie die Konfiguration mit horizontaler SAP HANA-Skalierung bereitstellen. Weisen Sie eine dritte virtuelle NIC zu, die in diesem Subnetz gehostet wird. Verwenden Sie die dritte virtuelle NIC und die zugeordnete IP-Adresse für den Datenverkehr an die NFS-Freigabe. Sie können dann separate Zugriffs- und Routingregeln anwenden.

>[!IMPORTANT]
>Der Netzwerkdatenverkehr zwischen den virtuellen Computern, auf denen SAP HANA mit horizontaler Skalierung bereitgestellt ist, und dem hoch verfügbaren NFS darf unter keinen Umständen über ein [virtuelles Netzwerkgerät](https://azure.microsoft.com/solutions/network-appliances/) oder über ähnliche virtuelle Geräte geleitet werden. Azure-NSGs zählen nicht zu diesen Geräten. Überprüfen Sie Ihre Routingregeln, um sich zu vergewissern, dass virtuelle Netzwerkgeräte oder ähnliche virtuelle Geräte beim Zugriff auf die hoch verfügbare NFS-Freigabe über die virtuellen Computer mit SAP HANA umgeleitet werden.
> 

Wenn Sie den hoch verfügbaren NFS-Cluster für SAP HANA-Konfigurationen freigeben möchten, verschieben Sie alle diese HANA-Konfigurationen in das gleiche virtuelle Netzwerk. 
 

### <a name="install-an-sap-hana-scale-out-in-azure"></a>Installieren einer horizontalen SAP HANA-Skalierung in Azure
Für die Installation einer SAP-Konfiguration mit horizontaler Skalierung sind folgende allgemeine Schritte erforderlich:

- Bereitstellen oder Anpassen einer neuen Infrastruktur für ein virtuelles Azure-Netzwerk
- Bereitstellen der neuen virtuellen Computer unter Verwendung Azure-verwalteter Storage Premium-Volumes
- Bereitstellen eines neuen hoch verfügbaren NFS-Clusters oder Anpassen eines bereits vorhandenen
- Anpassen des Netzwerkroutings, um beispielsweise sicherzustellen, dass die knotenübergreifende Kommunikation zwischen virtuellen Computern nicht über ein [virtuelles Netzwerkgerät](https://azure.microsoft.com/solutions/network-appliances/) geleitet wird. Gleiches gilt für den Datenverkehr zwischen den virtuellen Computern und dem hoch verfügbaren NFS-Cluster.
- Installieren des SAP HANA-Masterknotens
- Anpassen der Konfigurationsparameter des SAP HANA-Masterknotens
- Fortsetzen der Installation der SAP HANA-Workerknoten

#### <a name="install-sap-hana-in-a-scale-out-configuration"></a>Installieren von SAP HANA in einer Konfiguration mit horizontaler Skalierung
Ihre Azure-VM-Infrastruktur ist nun bereitgestellt, und alle anderen Vorbereitungen sind abgeschlossen. Führen Sie als Nächstes die folgenden Schritte aus, um die Konfiguration mit horizontaler SAP HANA-Skalierung zu installieren:

- Installieren Sie den SAP HANA-Masterknoten gemäß der SAP-Dokumentation.
- *Ändern Sie nach der Installation die Datei „global.ini“, und fügen Sie ihr den Parameter „basepath_shared = no“ hinzu.* Durch diesen Parameter kann SAP HANA mit horizontaler Skalierung ausgeführt werden, ohne dass die Volumes „/hana/data“ und „/hana/log“ zwischen den Knoten freigegeben werden. Weitere Informationen finden Sie im [SAP-Hinweis 2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Starten Sie die SAP HANA-Instanz neu, nachdem Sie den Parameter in der Datei „global.ini“ geändert haben.
- Fügen Sie zusätzliche Workerknoten hinzu. Weitere Informationen finden Sie im [SAP HANA-Administratorhandbuch](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html). Geben Sie während oder nach der Installation das interne Netzwerk für die knotenübergreifende SAP HANA-Kommunikation an. Verwenden Sie dazu beispielsweise „hdblcm“ (lokal). Weitere Informationen finden Sie im [SAP-Hinweis 2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Nach dieser Einrichtung verwendet die von Ihnen installierte Konfiguration mit horizontaler Skalierung nicht freigegebene Datenträger für die Ausführung von „/hana/data“ und „/hana/log“. Das Volume „/hana/shared“ wird in der hoch verfügbaren NFS-Freigabe platziert.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA Dynamic Tiering 2.0 für virtuelle Azure-Computer

Zusätzlich zu den SAP HANA-Zertifizierungen für virtuelle Azure-Computer der M-Serie wird SAP HANA Dynamic Tiering 2.0 (DT 2.0) auch in Microsoft Azure unterstützt. Links zur Dokumentation zu SAP HANA Dynamic Tiering finden Sie weiter unten in diesem Artikel im Abschnitt „Links zur DT 2.0-Dokumentation“. Es gibt keinen Unterschied zwischen der Installation und der Verwendung des Produkts (beispielsweise über SAP HANA Cockpit innerhalb eines virtuellen Azure-Computers). Für die offizielle Unterstützung in Azure werden jedoch einige wichtige Punkte vorausgesetzt. Diese zentralen Punkte werden in den folgenden Abschnitten beschrieben. 

SAP HANA DT 2.0 wird von SAP BW oder S4HANA nicht unterstützt. Das Hauptanwendungsgebiet sind derzeit native HANA-Anwendungen.


### <a name="overview"></a>Übersicht

Die folgende Abbildung gibt einen Überblick über die DT 2.0-Unterstützung in Microsoft Azure. Erfüllen Sie diese obligatorischen Anforderungen, um der offiziellen Zertifizierung gerecht zu werden:

- DT 2.0 muss auf einer dedizierten Azure-VM installiert sein. Die Ausführung auf dem gleichen virtuellen Computer, auf dem auch SAP HANA ausgeführt wird, ist nicht möglich.
- Virtuelle Computer für SAP HANA und DT 2.0 müssen im gleichen virtuellen Azure-Netzwerk bereitgestellt werden.
- Die virtuellen Computer für SAP HANA und DT 2.0 müssen mit aktiviertem beschleunigtem Netzwerkbetrieb bereitgestellt werden.
- Der Speichertyp für die virtuellen Computer für DT 2.0 muss Azure Storage Premium sein.
- An den virtuellen Computer für DT 2.0 müssen mehrere Azure-Datenträger angefügt sein.
- Ein Software-RAID/Stripesetvolume mit übergreifendem Azure-Datenträger-Striping muss erstellt werden (per LVM oder mdadm).

Weitere Erläuterungen finden Sie in den folgenden Abschnitten.

![Übersicht über die Architektur von SAP HANA DT 2.0](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Dedizierte Azure-VM für SAP HANA DT 2.0

In Azure IaaS wird DT 2.0 nur auf einem dedizierten virtuellen Computer unterstützt. DT 2.0 darf nicht auf dem gleichen virtuellen Azure-Computer ausgeführt werden, auf dem auch die HANA-Instanz ausgeführt wird. Zu Beginn können zwei VM-Typen zum Ausführen von SAP HANA DT 2.0 verwendet werden:

- M64-32ms 
- E32sv3 

Beschreibungen der VM-Typen finden Sie unter [Arbeitsspeicheroptimierte Größen virtueller Computer](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

Angesichts der Grundidee von DT 2.0 (Abladung warmer Daten, um Kosten zu sparen) ist es sinnvoll, entsprechende VM-Größen zu verwenden. Es gibt keine strenge Regel bezüglich möglicher Kombinationen. Dies hängt von der spezifischen Kundenworkload ab.

Die folgende Tabelle enthält die empfohlenen Konfigurationen:

| SAP HANA-VM-Typ | DT 2.0-VM-Typ |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Alle Kombinationen aus SAP HANA-zertifizierten virtuellen Computern der M-Serie und unterstützten virtuellen Computern für DT 2.0 (wie etwa M64-32ms und E32sv3) sind möglich.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure-Netzwerke und SAP HANA DT 2.0

Die Installation von DT 2.0 auf einem dedizierten virtuellen Computer setzt einen Netzwerkdurchsatz von mindestens 10 GB zwischen dem virtuellen Computer für DT 2.0 und dem virtuellen Computer für SAP HANA voraus. Aus diesem Grund müssen Sie alle virtuellen Computer im gleichen virtuellen Azure-Netzwerk platzieren und den beschleunigten Netzwerkbetrieb aktivieren.

Weitere Informationen zum beschleunigten Netzwerkbetrieb in Azure finden Sie unter [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

### <a name="vm-storage-for-sap-hana-dt-20"></a>VM-Speicher für SAP HANA DT 2.0

Laut bewährten DT 2.0-Methoden muss der Datenträger-E/A-Durchsatz pro physischem Kern mindestens 50 MB/s betragen. Die Spezifikation für die beiden für DT 2.0 unterstützten Azure-VM-Typen gibt den maximalen Datenträger-E/A-Durchsatz für den virtuellen Computer wie folgt an:

- **E32sv3:**   768 MB/s (nicht zwischengespeichert), also ein Verhältnis von 48 MB/s pro physischem Kern
- **M64-32ms:**  1.000 MB/s (nicht zwischengespeichert), also ein Verhältnis von 62,5 MB/s pro physischem Kern

An den virtuellen Computer für DT 2.0 müssen mehrere Azure-Datenträger angefügt und auf der Betriebssystemebene muss ein Software-RAID (Striping) erstellt werden, um den maximalen Datenträgerdurchsatz pro virtuellem Computer zu erzielen. Ein einzelner Azure-Datenträger bietet nicht genügend Durchsatz, um die VM-Obergrenze zu erreichen. Für die Ausführung von DT 2.0 wird Azure Storage Premium vorausgesetzt. 

- Weitere Informationen zu verfügbaren Azure-Datenträgertypen finden Sie unter [Welche Datenträgertypen stehen in Azure zur Verfügung?](../../windows/disks-types.md).
- Weitere Informationen zum Erstellen von Software-RAID mittels mdadm finden Sie unter [Konfigurieren von Software-RAID unter Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid).
- Weitere Informationen zum Konfigurieren von LVM, um ein Stripesetvolume für maximalen Durchsatz zu erstellen, finden Sie unter [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm).

Je nach den Größenanforderungen stehen Ihnen verschiedene Optionen zum Erreichen des maximalen Durchsatzes eines virtuellen Computers zur Verfügung. Hier sind die möglichen Datenvolumedatenträger-Konfigurationen zum Erreichen der Obergrenze des VM-Durchsatzes für jeden DT 2.0-VM-Typ aufgeführt. Der virtuelle Computer E32sv3 ist eine Einstiegsoption für kleinere Workloads. Sollte er nicht schnell genug sein, muss ggf. ein virtueller Computer vom Typ M64-32ms verwendet werden.

Da der virtuelle Computer M64-32ms über viel Arbeitsspeicher verfügt, wird das Limit bei der E/A-Last möglicherweise nicht erreicht (insbesondere bei leseintensiven Workloads). Abhängig von der kundenspezifischen Workload sind unter Umständen weniger Datenträger im Stripeset ausreichend. Aus Sicherheitsgründen wurden die folgenden Datenträgerkonfigurationen ausgewählt, um den maximalen Durchsatz zu gewährleisten:


| VM-SKU | Datentr.-Konfig. 1 | Datentr.-Konfig. 2 | Datentr.-Konfig. 3 | Datentr.-Konfig. 4 | Datentr.-Konfig. 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3,5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2,5 TB | 6 x P15 -> 1,5 TB | 


Vor allem bei leseintensiven Workloads lässt sich durch die Aktivierung der Schreibschutzeinstellung für den Azure-Hostcache (wie für die Datenvolumes von Datenbanksoftware empfohlen) ggf. die E/A-Leistung steigern. Für das Transaktionsprotokoll darf der Azure-Hostdatenträger-Cache nicht aktiviert sein. 

Als Startpunkt für die Größe des Protokollvolumes werden heuristische 15 Prozent der Datengröße empfohlen. Verwenden Sie abhängig von Kosten und Durchsatzanforderungen unterschiedliche Azure-Datenträgertypen für die Erstellung des Protokollvolumes. Das Protokollvolume erfordert einen hohen E/A-Durchsatz. 

Bei Verwendung des virtuellen Computers M64-32ms sollte die [Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) aktiviert werden. Die Schreibbeschleunigung ist ein Azure-Feature, das eine optimale Datenträgerschreiblatenz für das Transaktionsprotokoll gewährleistet. Es ist nur für die M-Serie verfügbar. Dabei müssen jedoch ein paar Punkte berücksichtigt werden – beispielsweise die maximale Anzahl von Datenträgern pro VM-Typ. Weitere Informationen zur Schreibbeschleunigung finden Sie unter [Aktivieren der Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


Die folgende Tabelle enthält einige Beispiele, die Sie beim Bestimmen der Größe des Protokollvolumes unterstützen.

| Datenvolumegröße und Datenträgertyp | Protokollvolume und Datentr.-Typ-Konfig. 1 | Protokollvolume und Datentr.-Typ-Konfig. 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2,5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1,5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Ähnlich wie bei SAP HANA mit horizontaler Skalierung muss das Verzeichnis „/hana/shared“ zwischen dem virtuellen Computer für SAP HANA und dem virtuellen Computer für DT 2.0 freigegeben werden. Es empfiehlt sich, die gleiche Architektur wie bei SAP HANA mit horizontaler Skalierung zu verwenden – mit dedizierten virtuellen Computern, die als hoch verfügbare NFS-Server fungieren. Verwenden Sie den identischen Entwurf, um ein freigegebenes Sicherungsvolume bereitzustellen. Entscheiden Sie jedoch selbst, ob Hochverfügbarkeit erforderlich ist, oder ob es ausreicht, einen dedizierten virtuellen Computer zu verwenden, dessen Speicherkapazität ausreicht, um als Sicherungsserver zu fungieren.



### <a name="links-to-dt-20-documentation"></a>Links zur DT 2.0-Dokumentation 

- [SAP HANA Dynamic Tiering: Installation and Update Guide](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US) (SAP HANA Dynamic Tiering: Installations- und Aktualisierungshandbuch)
- [SAP HANA Dynamic Tiering: Master Guide](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US) (SAP HANA Dynamic Tiering: Masterhandbuch)
- [SAP HANA dynamic tiering – delivering on low TCO warm data management, with impressive performance](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/) (SAP HANA Dynamic Tiering: Management warmer Daten zu niedrigen TCO mit eindrucksvoller Leistung)
- [SAP HANA 2.0 SPS 02 dynamic tiering enhancements (Verbesserungen)](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Vorgänge für die Bereitstellung von SAP HANA auf virtuellen Azure Computern
In den folgenden Abschnitten werden einige der Vorgänge beschrieben, die auszuführen sind, um SAP HANA-Systeme auf virtuellen Azure-Computern bereitzustellen.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Sicherungs- und Wiederherstellungsvorgänge auf virtuellen Azure-Computern
In den folgenden Dokumenten ist beschrieben, wie Sie Ihre SAP HANA-Bereitstellung sichern und wiederherstellen:

- [Übersicht über SAP HANA-Sicherungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA-Sicherungen auf Dateiebene](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Benchmark von SAP HANA-Speichermomentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Starten und Neustarten von virtuellen Computern, die SAP HANA enthalten
Ein markantes Merkmal der öffentlichen Azure-Cloud ist, dass Ihnen Gebühren nur für die Datenverarbeitungsminuten berechnet werden. Wenn Sie beispielsweise einen virtuellen Computer herunterfahren, auf dem SAP HANA ausgeführt wird, werden Ihnen nur die Speicherkosten für diese Zeit in Rechnung gestellt. Ein weiteres Merkmal ist verfügbar, wenn Sie in Ihrer Erstbereitstellung statische IP-Adressen für Ihre virtuellen Computer angeben. Wenn Sie einen virtuellen Computer neu starten, auf dem SAP HANA ausgeführt wird, wird der virtuelle Computer mit seinen vorherigen IP-Adressen neu gestartet. 


### <a name="use-saprouter-for-sap-remote-support"></a>Verwenden von SAPRouter für SAP-Remoteunterstützung
Wenn Sie über eine Site-to-Site-Verbindung zwischen Ihren lokalen Standorten und Azure verfügen und SAP-Komponenten ausführen, wird SAProuter wahrscheinlich bereits ausgeführt. Führen Sie in diesem Fall die folgenden Schritte für die Remoteunterstützung aus:

- Übernehmen der privaten und der statischen IP-Adresse des virtuellen Computers, auf dem SAP HANA gehostet wird, in die SAProuter-Konfiguration
- Konfigurieren der NSG des Subnetzes, in dem der virtuelle HANA-Computer gehostet wird, für Zulassen von Datenverkehr über den TCP/IP-Port 3299

Wenn Sie über das Internet eine Verbindung mit Azure herstellen und über keinen SAP-Router für den virtuellen Computer mit SAP HANA verfügen, installieren Sie die Komponente. Installieren Sie SAProuter auf einem separaten virtuellen Computer im Verwaltungssubnetz. 

Die folgende Abbildung zeigt ein allgemeines Schema für die Bereitstellung von SAP HANA ohne eine Standort-zu-Standort-Verbindung und mit SAProuter:

![Allgemeines Bereitstellungsschema für SAP HANA ohne eine Standort-zu-Standort-Verbindung und SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Installieren Sie SAProuter unbedingt auf einem separaten virtuellen Computer und nicht auf Ihrem virtuellen Jumpbox-Computer. Der separate virtuelle Computer muss eine statische IP-Adresse haben. Um eine Verbindung zwischen Ihrer SAProuter-Instanz und der von SAP gehosteten SAProuter-Instanz herzustellen, fordern Sie von SAP eine IP-Adresse an. Die von SAP gehostete SAProuter-Instanz ist das Gegenstück zu der SAProuter-Instanz, die Sie auf Ihrem virtuellen Computer installieren. Verwenden Sie die IP-Adresse von SAP, um Ihre SAProuter-Instanz zu konfigurieren. In den Konfigurationseinstellungen ist der TCP-Port 3299 der einzige erforderliche Port.

Weitere Informationen zum Einrichten und Verwalten von Verbindungen für die Remoteunterstützung über SAProuter finden Sie in der [SAP-Dokumentation](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hochverfügbarkeit mit SAP HANA auf nativen virtuellen Azure-Computern
Wenn Sie mit SUSE Linux Enterprise Server für SAP-Anwendungen 12 SP1 oder höher arbeiten, können Sie einen Pacemaker-Cluster mit STONITH Geräten einrichten. Sie können die Geräte verwenden, um eine SAP HANA-Konfiguration einzurichten, in der eine synchrone Replikation mit HANA-Systemreplikation und automatischem Failover verwendet wird. Weitere Informationen zur Einrichtung finden Sie im [Leitfaden zur Hochverfügbarkeit von SAP HANA für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

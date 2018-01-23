---
title: "SAP HANA-Vorgänge in Azure | Microsoft-Dokumentation"
description: "Betriebshandbuch für SAP HANA-Systeme, die auf virtuellen Azure-Computern bereitgestellt werden."
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: juergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2017
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d6991d40b9bb8543898bbbdc9d7c905dfe11536
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA in Azure-Vorgängen – Anleitung
Dieses Dokument enthält Anleitungen für den Betrieb von SAP HANA-Systemen, die auf nativen virtuellen Azure-Computern bereitgestellt werden. Dieses Dokument ist nicht als Ersatz für die SAP-Standarddokumentation gedacht, zu der folgende Inhalte gehören:

- [SAP-Administratorhandbuch](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-Installationshandbücher](https://service.sap.com/instguides)
- [SAP-Hinweise](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Voraussetzungen
Um dieses Handbuch zu verwenden, benötigen Sie grundlegende Kenntnisse der folgenden Azure-Komponenten:

- [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-Netzwerk und virtuelle Netzwerke](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Weiterführende Informationen zu SAP NetWeaver und zu anderen SAP-Komponenten in Azure finden Sie im Abschnitt [SAP in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) in der [Azure-Dokumentation](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Grundlegende Überlegungen zum Setup
In den folgenden Abschnitten werden grundlegende Einrichtungsüberlegungen für die Bereitstellung von SAP HANA-Systemen auf virtuellen Azure-Computern beschrieben.

### <a name="connect-into-azure-virtual-machines"></a>Herstellen von Verbindungen mit virtuellen Azure-Computern
Wie im [Planungshandbuch für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) dokumentiert, gibt es zwei grundlegende Methoden zum Herstellen einer Verbindung mit virtuellen Azure-Computern:

- Herstellen einer Verbindung über das Internet und öffentliche Endpunkte auf einer Jump-VM oder auf dem virtuellen Computer, auf dem SAP HANA ausgeführt wird
- Herstellen einer Verbindung über eine [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) oder Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

Site-to-Site-Konnektivität über VPN oder ExpressRoute ist für Produktionsszenarien erforderlich. Dieser Typ von Verbindung ist auch für Nicht-Produktionsszenarien erforderlich, die in Produktionsszenarien übertragen, in denen SAP-Software verwendet wird. Die folgende Abbildung zeigt ein Beispiel zu standortübergreifender Konnektivität:

![Standortübergreifende Konnektivität](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Auswählen der Arten von virtuellen Azure-Computern
Die Arten von virtuellen Azure-Computern, die für Produktionsszenarien verwendet werden können, sind in der [SAP-Dokumentation für IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html) aufgeführt. Für Nicht-Produktionsszenarien ist eine größere Auswahl von virtuellen Azure-Computern verfügbar.

>[!NOTE]
>Für Nicht-Produktionsszenarien sollten Sie die Arten von virtuellen Computern verwenden, die im [SAP-Hinweis 1928533](https://launchpad.support.sap.com/#/notes/1928533) aufgeführt sind.

Stellen Sie die virtuellen Computer in Azure bereit, indem Sie Folgendes verwenden:

- Das Azure-Portal
- Azure PowerShell-Cmdlets
- Die Azure-Befehlszeilenschnittstelle

Sie können eine vollständig installierte SAP HANA-Plattform auch über die [SAP Cloudplattform](https://cal.sap.com/) auf den Azure Virtual Machine-Diensten bereitstellen. Der Installationsvorgang ist unter [Bereitstellen von SAP S/4HANA oder BW/4HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) beschrieben.

### <a name="choose-azure-storage-type"></a>Auswählen des Azure-Speichertyps
Azure bietet zwei Speichertypen, die sich für virtuelle Azure-Computer mit SAP HANA eignen:

- [Azure Storage Standard](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium-Speicher](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure bietet zwei Bereitstellungsmethoden für virtuelle Festplatten (VHDs) in Azure Storage Standard und Premium. Wenn das allgemeine Szenario dies zulässt, sollten Sie Bereitstellungen mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/) verwenden.

Eine Liste der Speichertypen und deren Vereinbarungen zum Servicelevel finden Sie in der [Azure-Dokumentation für verwaltete Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).

Für die Volumes „/hana/data“ und „/hana/log“ werden Azure Premium-Datenträger empfohlen. Sie können auch ein LVM-RAID über mehrere Storage Premium-Datenträger erstellen und die RAID-Volumes als „/hana/data“ und „/hana/log“ verwenden.

In der folgenden Tabelle ist eine Konfiguration mit Arten von virtuellen Computern aufgeführt, die Kunden häufig verwenden, um SAP HANA auf virtuellen Azure-Computern zu hosten:

| VM-SKU | RAM | Maximal VM-E/A<br /> Throughput | „/hana/data“ und „/hana/log“<br /> Striping mit LVM oder MDADM | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128 GB | 384 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 GiB | 1.200GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1000 GiB | 1000 GB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 GiB | 1000 GB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 GiB | 2000 GB |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 GiB | 2000 GB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


> [!NOTE]
> Die Datenträger, die für die kleineren Typen virtueller Computer mit 3 x P20 empfohlen werden, haben Übergröße im Vergleich zu den Speicherplatzempfehlungen, die im [SAP-Whitepaper zu TDI Speicher](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) angegeben sind. Die in der Tabelle gezeigte Wahl wurde getroffen, um ausreichend Datenträgerdurchsatz für SAP HANA bereitzustellen. Wenn Sie weniger E/A-Durchsatz benötigen, können Sie die Wahl von Storage Premium-Datenträgern für „/hana/data“ und „/hana/log“ anpassen. Dasselbe gilt für das Volume „/hana/backup“, dessen Größe so festgelegt wurde, dass es Sicherungen aufnehmen kann, die dem Zweifachen der Arbeitsspeichergröße entsprechen. Wenn Sie weniger Speicherplatz benötigen, können Sie dies anpassen. Darüber hinaus sollten Sie den E/A-Gesamtdurchsatz eines virtuellen Computers beachten, wenn Sie die Größe festlegen oder sich für einen virtuellen Computer entscheiden. Der Gesamtdurchsatz eines virtuellen Computers ist im Artikel [Arbeitsspeicheroptimierte Größen virtueller Computer](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory) beschrieben.  

> [!NOTE]
> Wenn Sie von [SLA für Virtuelle Computer](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) profitieren möchten, müssen Sie alle virtuellen Festplatten, die als Standardspeicher (Sxx) aufgelistet werden, in Storage Premium (Pxx) ändern. 


### <a name="set-up-azure-virtual-networks"></a>Einrichten von virtuellen Azure-Netzwerken
Wenn Sie Site-to-Site-Konnektivität mit Azure über VPN oder ExpressRoute haben, müssen Sie mindestens ein [virtuelles Azure-Netzwerk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) haben, das über ein virtuelles Gateway mit der VPN- oder ExpressRoute-Verbindung verbunden ist. Das virtuelle Gateway befindet sich in einem Subnetz im virtuellen Azure-Netzwerk. Um SAP HANA zu installieren, erstellen Sie zwei weitere Subnetze innerhalb des virtuellen Netzwerks. In einem Subnetz werden die virtuellen Computer gehostet, auf denen die SAP HANA-Instanzen ausgeführt werden. Im anderen Subnetz werden virtuelle Jumpbox- oder Verwaltungscomputer ausgeführt, auf denen SAP HANA Studio oder andere Verwaltungssoftware gehostet wird.

Wenn Sie die virtuellen Computer zum Ausführen von SAP HANA installieren, benötigen die virtuellen Computer Folgendes:

- Zwei installierte virtuelle Netzwerkkarten: eine Netzwerkkarte, um eine Verbindung mit dem Verwaltungssubnetz herzustellen, und eine Netzwerkkarte, um eine Verbindung aus dem lokalen Netzwerk oder aus anderen Netzwerken mit der SAP HANA-Instanz auf dem virtuellen Azure-Computer herzustellen
- Statische private IP-Adressen, die für beide virtuellen Netzwerkkarten bereitgestellt werden

Einen Überblick über die verschiedenen Methoden zum Zuweisen von IP-Adressen finden Sie unter [IP-Adresstypen und Zuordnungsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

[Azure-Netzwerksicherheitsgruppen (NSGs)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) werden verwendet, um Datenverkehr zu leiten, der an die SAP HANA-Instanz oder die Jumpbox weitergeleitet wird. Die Netzwerksicherheitsgruppen sind mit dem SAP HANA-Subnetz und dem Verwaltungssubnetz verknüpft.

Die folgende Abbildung zeigt eine Übersicht eines allgemeinen Bereitstellungsschemas für SAP HANA:

![Allgemeines Bereitstellungsschema für SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Um SAP HANA in Azure ohne eine Standort-zu-Standort-Verbindung bereitzustellen, greifen Sie über eine öffentliche IP-Adresse auf die SAP HANA-Instanz zu. Die IP-Adresse muss dem virtuellen Azure-Computer zugewiesen sein, auf dem Ihre Jumpbox-VM ausgeführt wird. In diesem einfachen Szenario werden für die Bereitstellung in Azure integrierte DNS-Dienste verwendet, um Hostnamen aufzulösen. In einer komplexeren Bereitstellung, in der öffentlich zugängliche IP-Adressen verwendet werden, sind die in Azure integrierten DNS-Dienste besonders wichtig. Verwenden Sie Azure-NSGs, um die offenen Ports oder die IP-Adressbereiche zu beschränken, über die Verbindungen mit den Azure Subnetzen über Ressourcen hergestellt werden können, die öffentlich zugängliche IP-Adressen haben. Die folgende Abbildung zeigt ein allgemeines Schema für die Bereitstellung von SAP HANA ohne eine Standort-zu-Standort-Verbindung:
  
![Allgemeines Bereitstellungsschema für SAP HANA ohne eine Standort-zu-Standort-Verbindung](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Vorgänge für die Bereitstellung von SAP HANA auf virtuellen Azure Computern
In den folgenden Abschnitten werden einige der Vorgänge beschrieben, die auszuführen sind, um SAP HANA-Systeme auf virtuellen Azure-Computern bereitzustellen.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Sicherungs- und Wiederherstellungsvorgänge auf virtuellen Azure-Computern
In den folgenden Dokumenten ist beschrieben, wie Sie Ihre SAP HANA-Bereitstellung sichern und wiederherstellen:

- [Übersicht über SAP HANA-Sicherungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA-Sicherungen auf Dateiebene](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Benchmark von SAP HANA-Speichermomentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Starten und Neustarten von virtuellen Computern, die SAP HANA enthalten
Ein markantes Merkmal der öffentlichen Azure-Cloud ist, dass Ihnen Gebühren nur für die Datenverarbeitungsminuten berechnet werden. Wenn Sie beispielsweise einen virtuellen Computer herunterfahren, auf dem SAP HANA ausgeführt wird, werden Ihnen nur die Speicherkosten für diese Zeit in Rechnung gestellt. Ein weiteres Merkmal ist verfügbar, wenn Sie in Ihrer Erstbereitstellung statische IP-Adressen für Ihre virtuellen Computer angeben. Wenn Sie einen virtuellen Computer neu starten, auf dem SAP HANA ausgeführt wird, wird der virtuelle Computer mit seinen vorherigen IP-Adressen neu gestartet. 


### <a name="use-saprouter-for-sap-remote-support"></a>Verwenden von SAPRouter für SAP-Remoteunterstützung
Wenn Sie eine Standort-zu-Standort-Verbindung zwischen Ihren lokalen Standorten und Azure haben, und wenn Sie SAP-Komponenten ausführen, wird SAProuter wahrscheinlich bereits ausgeführt. In diesem Fall führen Sie die folgenden Schritte für Remoteunterstützung aus:

- Übernehmen der privaten und der statischen IP-Adresse des virtuellen Computers, auf dem SAP HANA gehostet wird, in die SAProuter-Konfiguration
- Konfigurieren der NSG des Subnetzes, in dem der virtuelle HANA-Computer gehostet wird, für Zulassen von Datenverkehr über den TCP/IP-Port 3299

Wenn Sie eine Verbindung mit Azure über das Internet herstellen und keinen SAP-Router für den virtuellen Computer mit SAP HANA haben, müssen Sie die Komponente installieren. Installieren Sie SAProuter auf einem separaten virtuellen Computer im Verwaltungssubnetz. Die folgende Abbildung zeigt ein allgemeines Schema für die Bereitstellung von SAP HANA ohne eine Standort-zu-Standort-Verbindung und mit SAProuter:

![Allgemeines Bereitstellungsschema für SAP HANA ohne eine Standort-zu-Standort-Verbindung und SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Installieren Sie SAProuter unbedingt auf einem separaten virtuellen Computer und nicht auf Ihrer Jumpbox-VM. Der separate virtuelle Computer muss eine statische IP-Adresse haben. Um die Verbindung Ihres SAProuters mit dem SAProuter herzustellen, der von SAP gehostet wird, wenden Sie sich an SAP, um eine IP-Adresse zu erhalten. (Der SAProuter, der von SAP gehostet wird, ist das Gegenstück zu der SAProuter-Instanz, die Sie auf Ihrem virtuellen Computer installieren.) Verwenden Sie die IP-Adresse von SAP, um Ihre SAProuter-Instanz zu konfigurieren. In den Konfigurationseinstellungen ist der TCP-Port 3299 der einzige erforderliche Port.

Weitere Informationen zum Einrichten und Verwalten von Verbindungen zur Remoteunterstützung über SAProuter finden Sie in der [SAP-Dokumentation](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hochverfügbarkeit mit SAP HANA auf nativen virtuellen Azure-Computern
Wenn Sie mit SUSE Linux 12 SP1 oder höher arbeiten, können Sie einen Pacemaker-Cluster mit STONITH Geräten einrichten. Sie können die Geräte verwenden, um eine SAP HANA-Konfiguration einzurichten, in der synchrone Replikation mit HANA-Systemreplikation und automatischem Failover verwendet wird. Weitere Informationen über die Einrichtungsvorgehensweise finden Sie unter [Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

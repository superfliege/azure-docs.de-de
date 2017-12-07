---
title: "SAP HANA-Vorgänge in Azure | Microsoft-Dokumentation"
description: "Vorgänge von SAP HANA auf nativen Azure-VMs"
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
ms.openlocfilehash: 0328bdc40429e1e82a76f290f5bde39089db0a9d
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA in Azure-Vorgängen – Anleitung
Dieses Anleitung gilt für den Betrieb von SAP HANA-Systemen, die auf virtuellen Azure-Computern bereitgestellt wurden. Dieses Dokument soll keine der SAP-Standarddokumentationen ersetzen. Sie finden die SAP-Anleitungen und -Anmerkungen in folgenden Dokumenten:

- [SAP-Administratorhandbuch](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-Installationshandbücher](https://service.sap.com/instguides)
- [SAP-Hinweise](https://sservice.sap.com/notes)

Voraussetzung ist, dass Sie über grundlegende Kenntnisse zu verschiedenen Azure-Komponenten verfügen:

- [Dokumentation zu virtuellen Computern](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Verwalten von virtuellen Azure-Netzwerken und virtuellen Linux-Computern mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage (in englischer Sprache)](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Weiterführendes Dokumentationsmaterial zu SAP NetWeaver und anderen SAP-Komponenten in Azure finden Sie im Abschnitt [SAP HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) der [Azure-Dokumentation](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Grundlegende Überlegungen zum Setup
### <a name="connecting-into-azure"></a>Herstellen einer Verbindung mit Azure
Wie unter [Azure Virtual Machines – Planung und Implementierung für SAP NetWeaver][planning-guide] beschrieben, gibt es zwei grundlegende Methoden für Verbindungen mit Azure Virtual Machines. 

- Herstellen einer Verbindung über das Internet und öffentliche Endpunkte auf einer Jump-VM oder dem virtuellen Computer mit SAP HANA
- Herstellen einer Verbindung über eine [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) oder Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

Für die Produktion oder Szenarien, in denen Nicht-Produktionsumgebungen unterstützend für Produktionsszenarien mit SAP-Software verwendet werden, benötigen Sie eine Site-to-Site-Verbindung über VPN oder ExpressRoute, wie in dieser Abbildung dargestellt:

![Standortübergreifende Konnektivität](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choice-of-azure-vm-types"></a>Auswählen der Azure-VM-Typen
Sie finden die Azure-VM-Typen, die für Produktionsszenarien verwendet werden können, [hier](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Für Nicht-Produktionsszenarien können Sie aus einer größeren Bandbreite von nativen Azure-VMs auswählen. Sie müssen sich jedoch auf die VM-Typen beschränken, die in [SAP-Hinweis #1928533](https://launchpad.support.sap.com/#/notes/1928533) aufgeführt werden. Die Bereitstellung dieser virtuellen Computer in Azure kann wie folgt durchgeführt werden:

- Azure-Portal
- Azure PowerShell-Cmdlets
- Azure-Befehlszeilenschnittstelle

Sie können über die [SAP Cloudplattform](https://cal.sap.com/) auch eine vollständig installierte SAP HANA-Plattform auf Azure Virtual Machines bereitstellen. Eine Beschreibung finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choice-of-azure-storage"></a>Auswählen des Azure-Speichers
Azure bietet zwei Hauptspeichertypen, die sich für Azure-VMs mit SAP HANA eignen.

- [Azure Storage Standard](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium-Speicher](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure bietet zwei Bereitstellungsmethoden für virtuelle Festplatten (VHDs) in Azure Storage Standard und Premium. Wenn das allgemeine Szenario es erlaubt, sollten Sie Bereitstellungen mit [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) verwenden.

Weitere Informationen zu den genauen Speichertypen und SLAs für diese Speichertypen finden Sie in [dieser Dokumentation](https://azure.microsoft.com/pricing/details/managed-disks/).

Es wird empfohlen, Azure Premium-Datenträger für die Volumes „/hana/data“ und „/hana/log“ zu verwenden. Es ist auch möglich, ein LVM-RAID über mehrere Storage Premium-Datenträger zu erstellen und diese RAID-Volumes als „/hana/data“ und „/hana/log“ zu verwenden.

Eine mögliche Konfiguration für verschiedene allgemeine VM-Typen, die Kunden bisher für das Hosten von SAP HANA auf Azure-VMs verwenden, könnte wie folgt aussehen:

| VM-SKU | RAM | „/hana/data“ und „/hana/log“<br /> Striping mit LVM oder MDADM | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S10 |
| E32v3 | 256 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1 TB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1,7 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128ms | 3,8 TB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 5 x S30 |


### <a name="azure-networking"></a>Azure-Netzwerke
Wenn Sie über eine VPN- oder ExpressRoute-Site-to-Site-Verbindung mit Azure verfügen, verfügen Sie über mindestens ein [Azure-VNET](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), das über ein virtuelles Gateway mit dem VPN oder ExpressRoute verbunden ist. Das virtuelle Gateway befindet sich in einem Subnetz im Azure-VNET. Damit Sie HANA installieren können, erstellen Sie zwei weitere Subnetze innerhalb des virtuellen Netzwerks. Ein Subnetz hostet die VMs, auf denen die SAP HANA-Instanzen ausgeführt werden, und ein anderes Subnetz führt die möglichen Jumpbox- oder Verwaltungs-VMs aus, die SAP HANA Studio oder andere Verwaltungssoftware hosten.
Wenn Sie die virtuellen Computer installieren, auf denen HANA ausgeführt werden soll, benötigen die virtuellen Computer Folgendes:

- Zwei installierte virtuelle NICs, von denen eine mit dem Verwaltungssubnetz verbunden ist und eine verwendet wird, um von einem lokalen Standort oder aus anderen Netzwerken eine Verbindung mit der SAP HANA-Instanz auf der Azure-VM herzustellen
- Statische private IP-Adressen für beide NICs

Eine Übersicht über die verschiedenen Möglichkeiten der IP-Adresszuweisung finden Sie [hier](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Das Datenverkehrsrouting (entweder direkt zur SAP HANA-Instanz oder an die Jumpbox) wird durch [Azure-Netzwerksicherheitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) gesteuert, die dem HANA-Subnetz und dem Verwaltungssubnetz zugeordnet sind.

Insgesamt würde das grobe Bereitstellungsschema wie folgt aussehen:

![Allgemeines Bereitstellungsschema für SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Wenn Sie nur SAP HANA in Azure bereitstellen – ohne eine Site-to-Site-Verbindung (VPN oder ExpressRoute in Azure) –, greifen Sie auf die SAP HANA-Instanz über eine öffentliche IP-Adresse zu, die der Azure-VM zugewiesen ist, auf der Ihre Jumpbox-VM ausgeführt wird. Im einfachsten Fall nutzen Sie auch die in Azure integrierten DNS-Dienste zum Auflösen von Hostnamen. Insbesondere bei der Verwendung öffentlich zugänglicher IP-Adressen sollten Sie Azure-Netzwerksicherheitsgruppen verwenden, um die offenen Ports oder IP-Adressbereiche einzuschränken, die Verbindungen mit den Azure Subnetzen, in denen Ressourcen mit öffentlich zugänglichen IP-Adressen ausgeführt werden, herstellen dürfen. Das Schema einer solchen Bereitstellung könnte wie folgt aussehen:
  
![Allgemeines Bereitstellungsschema für SAP HANA ohne Standort-zu-Standort-Verbindung](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations"></a>Vorgänge
### <a name="backup-and-restore-operations-on-azure-vms"></a>Sicherungs- und Wiederherstellungsvorgänge auf Azure-VMs
In den folgenden Dokumenten werden Möglichkeiten zur Sicherung und Wiederherstellung von SAP HANA beschrieben:

- [Übersicht über SAP HANA-Sicherungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA-Sicherungen auf Dateiebene](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Benchmark von SAP HANA-Speichermomentaufnahmen](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)



### <a name="start-and-restart-of-vms-containing-sap-hana"></a>Starten und Neustarten virtueller Computer mit SAP HANA
Eine der Stärken der öffentlichen Azure-Cloud ist die Tatsache, dass Ihnen nur die Computezeit in Rechnung gestellt wird, die Sie auch verbrauchen. Wenn Sie also einen virtuellen Computer mit SAP HANA herunterfahren, werden während dieser Zeit nur die Kosten für den Speicher in Rechnung gestellt. Wenn Sie den virtuellen Computer mit SAP HANA erneut starten, erhält der virtuelle Computer erneut die gleichen IP-Adressen (bei einer Bereitstellung mit statischen IP-Adressen). 


### <a name="saprouter-enabling-sap-remote-support"></a>SAPRouter für SAP-Remoteunterstützung
Wenn Sie über eine Standort-zu-Standort-Verbindung zwischen Ihren lokalen Standorten und Azure verfügen und Sie die SAP-Komponenten bereits ausführen, verwenden Sie sehr wahrscheinlich bereits den SAPRouter. In diesem Fall müssen Sie keine speziellen Aktionen für SAP HANA-Instanzen ausführen, die Sie in Azure bereitstellen. Sie verwalten lediglich die private und statische IP-Adresse des virtuellen Computers, auf dem HANA gehostet wird, in der SAPRouter-Konfiguration und passen die NSG des Subnetzes, in dem die HANA-VM gehostet wird, an (Zulassen von Datenverkehr über TCP/IP-Port 3299).

Wenn Sie SAP HANA bereitstellen und über das Internet eine Verbindung mit Azure herstellen, ohne über einen SAP-Router in dem VNET, in dem die VM mit SAP HANA ausgeführt wird, zu verfügen, sollten Sie den SAPRouter auf einem separaten virtuellen Computer im Verwaltungssubnetz installieren, wie in der folgenden Darstellung gezeigt:


![Allgemeines Bereitstellungsschema für SAP HANA ohne Standort-zu-Standort-Verbindung und SAPRouter](media/hana-vm-operations/hana-simple-networking3.PNG)

Sie sollten den SAPRouter auf einem separaten virtuellen Computer und nicht auf Ihrer Jumpbox-VM installieren. Die separate VM benötigt eine statische IP-Adresse. Für eine Verbindung zwischen dem SAPRouter und dem von SAP gehosteten SAPRouter (Gegenstück der auf Ihrer VM installieren SAPRouter-Instanz) wenden Sie sich an SAP, um eine IP-Adresse von SAP zu erhalten, mit der Sie die SAPRouter-Instanz konfigurieren. Der einzige erforderlich Port ist der TCP-Port 3299.
Weitere Informationen zum Einrichten und Verwalten von Verbindungen zur Remoteunterstützung über SAPRouter finden Sie in dieser [SAP-Quelle](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hochverfügbarkeit mit SAP HANA auf nativen Azure-VMs
Wenn Sie SUSE Linux 12 SP1 oder höher ausführen, können Sie einen Pacemaker-Cluster mit STONITH Geräten erstellen, um eine SAP HANA-Konfiguration einzurichten, die die synchrone Replikation mit HANA-Systemreplikation und automatischem Failover verwendet. Die Einrichtung wird im Artikel [Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs)](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-high-availability) beschrieben.

 











---
title: Architektur des Operations Management Suite (OMS) | Microsoft Docs
description: "Microsoft Operations Management Suite (OMS) ist die Microsoft Cloud-basierten IT-verwaltungslösung, mit denen die verwalten und Schützen Ihrer lokalen und cloud-Infrastruktur.  In diesem Artikel identifiziert die verschiedenen Dienste, die in der OMS enthalten und enthält Links zu ihren detaillierten Inhalt."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 76f69946724b5297b1f9a1f715819c69c4a4a51d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="oms-architecture"></a>OMS-Architektur
[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) ist eine Sammlung von cloudbasierten Diensten für die Verwaltung Ihrer lokales und Cloudumgebungen.  Dieser Artikel beschreibt die verschiedenen lokalen und cloudkomponenten von OMS und deren übergeordnete Cloud computing-Architektur.  Sie können in der Dokumentation für jeden Dienst Detailinformationen verweisen.

## <a name="log-analytics"></a>Protokollanalysen
Alle Daten, die von gesammelten [Protokollanalyse](https://azure.microsoft.com/documentation/services/log-analytics/) befindet sich im OMS-Repository der in Azure gehostet wird.  Verbundene Datenquellen Generieren von Daten im OMS-Repository erfasst.  Es gibt derzeit drei Typen von verbundene Datenquellen unterstützt.

* Ein Agent installiert ein [Windows](../log-analytics/log-analytics-windows-agents.md) oder [Linux](../log-analytics/log-analytics-linux-agents.md) Computer direkt mit OMS verbunden.
* Eine Verwaltungsgruppe von System Center Operations Manager (SCOM) [verbunden mit Log Analytics](../log-analytics/log-analytics-om-agents.md) .  SCOM-Agents mit Verwaltungsservern kommunizieren weiterhin die Ereignisse und Leistungsdaten an Protokollanalyse weiterzuleiten.
* Ein [Azure-Speicherkonto](../log-analytics/log-analytics-azure-storage.md) , sammelt [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) Daten aus einer workerrolle, Webrolle oder virtuellen Computer in Azure.

Datenquellen definieren die Daten, die Log Analytics verbundene Datenquellen, einschließlich von Ereignisprotokollen und Leistungsindikatoren gesammelt werden.  Lösungen fügen Funktionen zu OMS hinzu und können problemlos hinzugefügt werden, in den Arbeitsbereich aus der [OMS-Lösungskatalog](../log-analytics/log-analytics-add-solutions.md).  Einige Lösungen erfordern möglicherweise eine direkte Verbindung mit der Protokollanalyse SCOM-Agents, während andere erfordern die ein zusätzlichen Agents installiert werden können.

Log Analytics ist ein webbasiertes Portal, die Sie verwenden können, um OMS-Ressourcen verwalten, hinzufügen und Konfigurieren der OMS-Lösungen und anzeigen und Analysieren von Daten im OMS-Repository.

![Log Analytics-Architektur](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure Automation
[Azure Automation-Runbooks](http://azure.microsoft.com/documentation/services/automation) in der Azure-Cloud ausgeführt werden und Zugriff auf Ressourcen, die in Azure, in anderen Clouddiensten sowie über das öffentliche Internet zugänglich sind.  Sie können auch lokale Computer festlegen, in Ihrem lokalen Datencenter [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) , damit Runbooks auf lokale Ressourcen zugreifen können.

[DSC-Konfigurationen](../automation/automation-dsc-overview.md) gespeichert, die Azure Automation können direkt auf virtuelle Azure-Computer angewendet werden.  Andere physische und virtuelle Computer können Konfigurationen vom Azure Automation DSC-Pull-Server anfordern.

Azure Automation bietet eine OMS-Lösung, die Statistiken und Links zum Starten des Azure-Portals für beliebige Vorgänge anzeigt.

![Azure Automation-Architektur](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure-Sicherung
Geschützte Daten in [Azure Backup](http://azure.microsoft.com/documentation/services/backup) befindet sich in einem sicherungstresor, die sich in einer bestimmten geografischen Region befinden.  Die Daten werden innerhalb der gleichen Region repliziert und je nach Art des Tresors, möglicherweise auch repliziert werden in einer anderen Region für erweiterten Redundanz.

Azure Backup verfügt über drei grundlegende Szenarien.

* Windows-Computer mit Azure Backup-Agent.  Dadurch können Sie zum Sichern von Dateien und Ordner von einem Windows-Server oder Client direkt in den Azure backup-Tresor.  
* System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Server. Dadurch können Sie DPM oder Microsoft Azure Backup Server zum Sichern von Dateien und Ordnern, zusätzlich zu anwendungsauslastungen wie SQL und SharePoint in den lokalen Speicher, und klicken Sie dann auf den Azure backup-Tresor replizieren zu nutzen.
* Azure VM-Erweiterungen.  Dadurch können Sie Azure virtuelle Computer in Ihrem Azure backup-Tresor sichern.

Azure Backup bietet eine OMS-Lösung, die Statistiken und Links zum Starten des Azure-Portals für beliebige Vorgänge anzeigt.

![Azure Backup-Architektur](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) koordiniert Replikation, Failover und Failback von virtuellen Computern und physischen Servern. Replikationsdaten werden zwischen Hyper-V-Hosts, VMware-Hypervisoren und physischen Servern in primären und sekundären Datencentern oder zwischen dem Datencenter und Azure-Speicher ausgetauscht.  Site Recovery speichert Metadaten in Tresoren, die sich in einer bestimmten geografischen Azure-Region befinden. Keine replizierten Daten vom Site Recovery-Dienst gespeichert.

Azure Site Recovery verfügt über drei grundlegende Replikationsszenarien.

**Replikation von Hyper-V-Computer**

* Wenn Hyper-V-Maschinen in VMM-Clouds verwaltet werden, können Sie ein sekundäres Datencenter oder in Azure-Speicher replizieren.  Die Replikation in Azure erfolgt über eine sichere Internetverbindung.  Die Replikation erfolgt in ein sekundäres Datencenter über das LAN.
* Wenn Hyper-V-Maschinen nicht von VMM verwaltet werden, können Sie den Azure-Speicher replizieren.  Die Replikation in Azure erfolgt über eine sichere Internetverbindung.

**Replikation von virtuellen VMWare-Maschinen**

* Sie können virtuelle VMware-Maschinen in ein VMware ausführendes sekundäres Datencenter oder in Azure-Speicher replizieren.  Replikation in Azure kann über eine Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Datencenter erfolgt über den InMage Scout-Datenkanal.

**Replikation von physischen Windows- und Linux-Servern** 

* Sie können physische Server in ein sekundäres Datencenter oder in Azure-Speicher replizieren. Replikation in Azure kann über eine Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Datencenter erfolgt über den InMage Scout-Datenkanal.  Azure Site Recovery bietet eine OMS-Lösung, die einige Statistiken anzeigt, aber Sie müssen das Azure-Portal für beliebige Vorgänge verwenden.

![Azure Site Recovery-Architektur](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollanalysen](http://azure.microsoft.com/documentation/services/log-analytics).
* Erfahren Sie mehr über [Azure Automation](https://azure.microsoft.com/documentation/services/automation).
* Erfahren Sie mehr über [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Erfahren Sie mehr über [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).


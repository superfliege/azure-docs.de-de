---
title: Architektur für die Replikation von VMware und physischen Servern in Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die Komponenten und die Architektur, die beim Replizieren von lokalen VMware-VMs oder physischen Servern unter Windows oder Linux an einen sekundären VMware-Standort mit Azure Site Recovery verwendet werden.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: 97a990aa3ed9043280888900d8fc7b604b6c22b5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
ms.locfileid: "29854272"
---
# <a name="vmware-vmphysical-server-to-vmware-replication-architecture"></a>Architektur für die Replikation von VMware-VMs/physischen Servern zu VMware

Dieser Artikel beschreibt die Architektur und Prozesse, die bei der Replikation, der Ausführung eines Failovers und dem Wiederherstellen von lokalen virtuellen VMware-Computern (VMs) oder physischen Windows-/Linux-Servern an einen sekundären VMware-Standort mithilfe von [Azure Site Recovery](site-recovery-overview.md) verwendet werden.


## <a name="architectural-components"></a>Komponenten der Architektur

**Bereich** | **Komponente** | **Details**
--- | --- | ---
**Azure** | Sie stellen dieses Szenario mit InMage Scout bereit. | Zum Herunterladen von InMage Scout benötigen Sie ein Azure-Abonnement.<br/><br/> Nachdem Sie einen Recovery Services-Tresor erstellt haben, laden Sie InMage Scout herunter und installieren die aktuellen Updates, um die Bereitstellung einzurichten.
**Prozessserver** | Befindet sich am primären Standort. | Der Prozessserver wird zur Abwicklung von Zwischenspeicherung, Komprimierung und Datenoptimierung bereitgestellt.<br/><br/> Außerdem erfolgt hierüber die Pushinstallation des vereinheitlichten Agents auf Maschinen, die Sie schützen möchten.
**Konfigurationsserver** | Befindet sich am sekundären Standort. | Der Konfigurationsserver dient zum Verwalten, Konfigurieren und Überwachen Ihrer Bereitstellung (entweder über die Verwaltungswebsite oder über die vContinuum-Konsole).
**vContinuum-Server** | Optional. Wird an demselben Standort wie der Konfigurationsserver installiert. | Er enthält eine Konsole zum Verwalten und Überwachen Ihrer geschützten Umgebung.
**Masterzielserver** | Befindet sich am sekundären Standort. | Der Masterzielserver enthält replizierte Daten. Er empfängt Daten vom Prozessserver und erstellt einen Replikatcomputer am sekundären Standort. Außerdem enthält er die Punkte für die Beibehaltung der Daten.<br/><br/> Die Anzahl der benötigten Masterzielserver richtet sich nach der Anzahl der Computer, die Sie schützen.<br/><br/> Wenn Sie ein Failback zum primären Standort durchführen möchten, benötigen Sie dort ebenfalls einen Masterzielserver. Der vereinheitlichte Agent wird auf diesem Server installiert.
**VMware ESX/ESXi und vCenter-Server** |  Virtuelle Computer werden auf ESX/ESXi-Hosts gehostet. Hosts werden mit einem vCenter-Server verwaltet. | Sie benötigen eine VMware-Infrastruktur, um VMware-VMs zu replizieren.
**VMs/physische Server** |  Vereinheitlichter Agent, installiert auf virtuellen VMware-Computern und auf physischen Servern, die Sie replizieren möchten. | Der Agent fungiert als Kommunikationsanbieter zwischen allen Komponenten.

### <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Komponentenserver an jedem Standort ein (Konfiguration, Prozess, Masterziel) und installieren den vereinheitlichten Agent auf Computern, die Sie replizieren möchten.
2. Nach der ersten Replikation sendet der Agent auf jedem Computer die Deltareplikationsänderungen an den Prozessserver.
3. Der Prozessserver optimiert die Daten und überträgt sie an den Masterzielserver am sekundären Standort. Der Konfigurationsserver verwaltet den Replikationsprozess.

**Abbildung 6: Replikation von VMware in VMware**

![VMware zu VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Nächste Schritte

[Richten Sie die Notfallwiederherstellung](vmware-physical-secondary-disaster-recovery.md) von virtuellen VMware-Computern und physischen Servern an einem sekundären Standort ein.

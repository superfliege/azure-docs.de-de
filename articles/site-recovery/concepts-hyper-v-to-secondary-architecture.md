---
title: "Überprüfen der Architektur für die Hyper-V-Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Architektur zum Replizieren lokaler virtueller Hyper-V-Computer an einem sekundären System Center-VMM-Standort mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 26475782-a21a-408a-b089-35382d7e010e
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: a7a493097a4eaacc2c8d8449906b4a57eb411827
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="hyper-v-replication-to-a-secondary-site"></a>Hyper-V-Replikation an einem sekundären Standort

Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replikation lokaler virtueller Hyper-V-Computer (Virtual Machines, VMs) in System Center-VMM-Clouds (Virtual Machine Manager) an einem sekundären VMM-Standort mithilfe des Diensts [Azure Site Recovery](site-recovery-overview.md) über das Azure-Portal.


## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Tabelle und Grafik bietet eine Übersicht der Komponenten, die für die Hyper-V-Replikation an einem sekundären Standort verwendet werden.

**Komponente** | **Anforderung** | **Details**
--- | --- | ---
**Azure** | Azure-Abonnement | Sie erstellen einen Recovery Services-Tresor im Azure-Abonnement, um die Replikation zwischen VMM-Standorten zu orchestrieren und zu verwalten.
**VMM-Server** | Sie benötigen einen primären und sekundären VMM-Standort. | Wir empfehlen Ihnen die Verwendung eines VMM-Servers am primären Standort und eines VMM-Servers am sekundären Standort.
**Hyper-V-Server** |  Mindestens ein Hyper-V-Hostserver in der primären und der sekundären VMM-Cloud. | Die Daten werden zwischen dem primären und sekundären Hyper-V-Hostserver über das LAN oder VPN per Kerberos- oder Zertifikatauthentifizierung repliziert.  
**Virtuelle Hyper-V-Computer** | Auf dem Hyper-V-Hostserver. | Der Quellhostserver muss über mindestens einen virtuellen Computer verfügen, den Sie replizieren möchten.

**Lokal-zu-lokal-Architektur**

![Lokal zu lokal](./media/concepts-hyper-v-to-secondary-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Replikationsprozess

1. Wenn die erste Replikation ausgelöst wird, wird eine [Momentaufnahem der Hyper-V-VM](https://technet.microsoft.com/library/dd560637.aspx) erstellt.
2. Virtuelle Festplatten auf der VM werden nacheinander an den sekundären Standort repliziert.
3. Wenn während der Durchführung der ersten Replikation Datenträgeränderungen auftreten. 
4. Die Deltareplikation startet, wenn die erste Replikation abgeschlossen wurde. Der Replication Tracker für Hyper-V-Replikate verfolgt die Änderungen als Hyper-V-Replikationsprotokolle (.hrl). Diese Protokolldateien befinden sich im gleichen Ordner wie die Datenträger. Jeder Datenträger verfügt über eine zugeordnete HRL-Datei, die an den sekundären Speicher gesendet wird. Beachten Sie, dass die Momentaufnahme- und Protokolldateien Festplattenressourcen belegen, während die anfängliche Replikation durchgeführt wird.
5. Die im Protokoll erfassten Datenträgeränderungen werden synchronisiert und mit dem übergeordneten Datenträger zusammengeführt.
6. 

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

1. Sie können ein Failover für einen einzelnen Computer ausführen oder Wiederherstellungspläne erstellen, um das Failover von mehreren Computern zu orchestrieren.
2. Sie können ein geplantes oder ungeplantes Failover zwischen lokalen Standorten ausführen. Wenn Sie ein geplantes Failover durchführen, werden die Quell-VMs heruntergefahren, um sicherzustellen, dass kein Datenverlust auftritt.
    - Nach einem nicht geplanten Failover zu einem sekundären Standort sind die Computer am sekundären Standort nicht geschützt.
    - Nach einem geplanten Failover sind die Computer am sekundären Standort geschützt.
3. Nachdem das erste Failover ausgeführt wird, committen Sie es, um von der Replikat-VM auf die Workload zuzugreifen.

Sie können ein Failback ausführen, wenn der primäre Standort wieder verfügbar ist.

1. Leiten Sie eine umgekehrte Replikation ein, um die Replikation vom sekundären an den primären Standort zu starten. Die umgekehrte Replikation versetzt die virtuellen Computer in einen geschützten Zustand, aber das sekundäre Rechenzentrum bleibt weiterhin aktiv.
2. Initiieren Sie ein geplantes Failover vom sekundären zum primären Standort, und führen Sie anschließend eine erneute umgekehrte Replikation durch, um den primären Standort wieder zum aktiven Standort zu machen.



## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die Supportmatrix. Folgen Sie dem Tutorial, um die Hyper-V-Replikation zwischen VMM-Clouds zu aktivieren.
Führen Sie ein Failover und ein Failback aus.

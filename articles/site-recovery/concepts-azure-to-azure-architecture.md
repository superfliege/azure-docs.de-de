---
title: "Überprüfen der Architektur für die Replikation von Azure-VMs zwischen Azure-Regionen | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die beim Replizieren von Azure-VMs zwischen Azure-Regionen mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: f9cd57e47a8463440148b2bcc6c21beacd54382a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-to-azure-replication-architecture"></a>Architektur der Azure-zu-Azure-Replikation


Dieser Artikel beschreibt die Architektur und Prozesse, die mithilfe des Dienstes [Azure Site Recovery](site-recovery-overview.md) bei der Replikation, bei der Ausführung eines Failovers und beim Wiederherstellen von virtuellen Azure-Computern (VMs) zwischen Azure-Regionen verwendet werden.

>[!NOTE]
>Die Replikation von Azure-VMs mit dem Site Recovery-Dienst ist derzeit in der Vorschau verfügbar.



## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Abbildung enthält einen allgemeinen Überblick über eine Azure-VM-Umgebung in einer bestimmten Region (in diesem Beispiel wird der Standort „USA, Osten“ verwendet). In einer Azure-VM-Umgebung:
- Apps können auf VMs mit Datenträgern über alle Speicherkonten hinweg ausgeführt werden.
- Die VMs können in einem oder in mehreren Subnetzen innerhalb eines virtuellen Netzwerks enthalten sein.


**Azure-zu-Azure-Replikation**

![Kundenumgebung](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Replikationsprozess

### <a name="step-1"></a>Schritt 1

Wenn Sie die Replikation für Azure-VMs aktivieren, werden die unten genannten Ressourcen automatisch in der Zielregion und auf Grundlage der Einstellungen in der Quellregion erstellt. Bei Bedarf können Sie die Zielressourceneinstellungen anpassen. 

![Schritt 1: Aktivieren des Replikationsprozesses](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Ressource** | **Details**
--- | ---
**Zielressourcengruppe** | Die Ressourcengruppe, zu denen replizierte VMs nach einem Failover gehören.
**Virtuelles Zielnetzwerk** | Das virtuelle Netzwerk, in dem sich nach einem Failover replizierte VMs befinden. Eine Netzwerkzuordnung von virtuellen Quell- zu Zielnetzwerken (und umgekehrt) wird erstellt.
**Cachespeicherkonten** | Bevor Änderungen an Quell-VMs in ein Zielspeicherkonto repliziert werden, werden sie nachverfolgt und an das Cachespeicherkonto am Zielstandort gesendet. Dadurch wird sichergestellt, dass die Auswirkungen auf Produktion-Apps, die auf der VM ausgeführt werden, so gering wie möglich sind.
**Zielspeicherkonten**  | Speicherkonten am Zielstandort, an dem die Daten repliziert werden.
**Zielverfügbarkeitsgruppen**  | Verfügbarkeitsgruppen, in denen sich nach einem Failover die replizierten VMs befinden.

### <a name="step-2"></a>Schritt 2

Da die Replikation aktiviert ist, wird automatisch die Site Recovery-Erweiterung „Mobility Service“ auf der VM installiert:

1. Die VM wird bei Site Recovery registriert.

2. Für die VM wird die fortlaufende Replikation konfiguriert. Datenschreibvorgänge auf VM-Datenträgern werden kontinuierlich auf das Cachespeicherkonto am Quellstandort übertragen.

   ![Schritt 2: Aktivieren des Replikationsprozesses](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

  
 Site Recovery benötigt nie eingehende Verbindungen mit der VM. Es ist nur ausgehende Konnektivität mit den URLs oder IP-Adressen des Site Recovery-Diensts, den URLs oder IP-Adressen der Office 365-Authentifizierung und den IP-Adressen des Cachespeicherkontos erforderlich.

### <a name="step-3"></a>Schritt 3

Wenn die fortlaufende Replikation ausgeführt wird, werden Schreibvorgänge auf dem Datenträger sofort auf das Cachespeicherkonto übertragen. Site Recovery verarbeitet die Daten und sendet sie an das Zielspeicherkonto. Nachdem die Daten verarbeitet wurden, werden alle paar Minuten Wiederherstellungspunkte im Zielspeicherkonto generiert.

## <a name="failover-process"></a>Failoverprozess

Bei der Initiierung eines Failovers werden die VMs in der Zielressourcengruppe, im virtuellen Zielnetzwerk, im Zielsubnetz und in der Zielverfügbarkeitsgruppe erstellt. Bei einem Failover können Sie einen beliebigen Wiederherstellungspunkt verwenden.

![Failoverprozess](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die Supportmatrix. Folgen Sie dem Tutorial, um die Replikation von Azure-VMs in eine sekundäre Region zu aktivieren.
Führen Sie ein Failover und ein Failback aus.
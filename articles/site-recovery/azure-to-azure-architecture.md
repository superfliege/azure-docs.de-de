---
title: Architektur der Azure-zu-Azure-Replikation in Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die beim Replizieren von Azure-VMs zwischen Azure-Regionen mit dem Azure Site Recovery-Dienst verwendet werden.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 33ab90f958e5033c0c563e4fd8921ee1f7d57c47
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915672"
---
# <a name="azure-to-azure-replication-architecture"></a>Architektur der Azure-zu-Azure-Replikation


Dieser Artikel beschreibt die Architektur, die unter Verwendung des [Azure Site Recovery](site-recovery-overview.md)-Diensts für die Replikation, die Ausführung eines Failovers und die Wiederherstellung von virtuellen Azure-Computern (VMs) zwischen Azure-Regionen verwendet wird.




## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Abbildung enthält einen allgemeinen Überblick über eine Azure-VM-Umgebung in einer bestimmten Region (in diesem Beispiel wird der Standort „USA, Osten“ verwendet). In einer Azure-VM-Umgebung:
- Apps können auf virtuellen Computern mit verwalteten oder nicht verwalteten Datenträgern über alle Speicherkonten hinweg ausgeführt werden.
- Die VMs können in einem oder in mehreren Subnetzen innerhalb eines virtuellen Netzwerks enthalten sein.


**Azure-zu-Azure-Replikation**

![Kundenumgebung](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Replikationsprozess

### <a name="step-1"></a>Schritt 1

Wenn Sie die Azure-VM-Replikation aktivieren, werden die folgenden Ressourcen auf Grundlage der Einstellungen in der Quellregion automatisch in der Zielregion erstellt. Bei Bedarf können Sie die Zielressourceneinstellungen anpassen.

![Schritt 1: Aktivieren des Replikationsprozesses](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Ressource** | **Details**
--- | ---
**Zielressourcengruppe** | Die Ressourcengruppe, zu denen replizierte VMs nach einem Failover gehören. Der Speicherort der Ressourcengruppe kann eine beliebige Azure-Region sein, mit Ausnahme der Azure-Region, in der die virtuellen Quellcomputer gehostet werden.
**Virtuelles Zielnetzwerk** | Das virtuelle Netzwerk, in dem sich nach einem Failover replizierte VMs befinden. Eine Netzwerkzuordnung von virtuellen Quell- zu Zielnetzwerken (und umgekehrt) wird erstellt.
**Cachespeicherkonten** | Bevor Änderungen an Quell-VMs in ein Zielspeicherkonto repliziert werden, werden sie nachverfolgt und an das Cachespeicherkonto im Quellspeicherort gesendet. Dieser Schritt stellt sicher, dass die Auswirkungen auf die auf dem virtuellen Computer ausgeführten Produktion-Apps so gering wie möglich sind.
**Zielspeicherkonten (wenn die Quell-VM keine verwalteten Datenträger verwendet)**  | Speicherkonten am Zielstandort, an dem die Daten repliziert werden.
**Verwaltete Replikatdatenträger (wenn die Quell-VM verwaltete Datenträger verwendet)**  | Verwaltete Datenträger am Zielstandort, an dem die Daten repliziert werden.
**Zielverfügbarkeitsgruppen**  | Verfügbarkeitsgruppen, in denen sich nach einem Failover die replizierten VMs befinden.

### <a name="step-2"></a>Schritt 2

Da die Replikation aktiviert ist, wird automatisch die Site Recovery-Erweiterung „Mobility Service“ auf der VM installiert:

1. Die VM wird bei Site Recovery registriert.

2. Für die VM wird die fortlaufende Replikation konfiguriert. Datenschreibvorgänge auf VM-Datenträgern werden kontinuierlich auf das Cachespeicherkonto am Quellstandort übertragen.

   ![Schritt 2: Aktivieren des Replikationsprozesses](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Site Recovery benötigt nie eingehende Verbindungen mit der VM. Für Folgendes ist nur ausgehende Konnektivität erforderlich.

 - URLs/IP-Adressen des Site Recovery-Diensts
 - URLs/IP-Adressen für die Office 365-Authentifizierung
 - IP-Adressen des Cachespeicherkontos

Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über den Port 20004 miteinander. Stellen Sie sicher, dass die interne Kommunikation zwischen den VMs über Port 20004 nicht durch eine Firewallappliance blockiert wird.

> [!IMPORTANT]
Wenn Sie Linux-VMs in eine Replikationsgruppe einschließen möchten, stellen Sie sicher, dass der ausgehende Datenverkehr auf Port 20004 gemäß den Anweisungen für die jeweilige Linux-Version manuell geöffnet wird.

### <a name="step-3"></a>Schritt 3

Wenn die fortlaufende Replikation ausgeführt wird, werden Schreibvorgänge auf dem Datenträger sofort auf das Cachespeicherkonto übertragen. Site Recovery verarbeitet die Daten und sendet sie an das Zielspeicherkonto oder an verwaltete Replikatdatenträger. Nachdem die Daten verarbeitet wurden, werden alle paar Minuten Wiederherstellungspunkte im Zielspeicherkonto generiert.

## <a name="failover-process"></a>Failoverprozess

Bei der Initiierung eines Failovers werden die VMs in der Zielressourcengruppe, im virtuellen Zielnetzwerk, im Zielsubnetz und in der Zielverfügbarkeitsgruppe erstellt. Bei einem Failover können Sie einen beliebigen Wiederherstellungspunkt verwenden.

![Failoverprozess](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Nächste Schritte

[Schnelles Replizieren](azure-to-azure-quickstart.md) einer Azure-VM in eine sekundäre Region

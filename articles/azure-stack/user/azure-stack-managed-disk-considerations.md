---
title: Unterschiede und Aspekte für Managed Disks in Azure Stack | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Unterschiede und zu berücksichtigende Aspekte bei der Verwendung von Managed Disks in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 2870bf8911c48ac4cbb442278f172b37a474152b
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078052"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack Managed Disks: Unterschiede und Überlegungen
Dieser Artikel beschreibt die bekannten Unterschiede zwischen Azure Stack Managed Disks und Managed Disks für Azure. Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Artikel [Key considerations](azure-stack-considerations.md) (Wichtige Aspekte).

Managed Disks vereinfacht die Datenträgerverwaltung für IaaS-VMs durch die Verwaltung der [Speicherkonten](/azure/azure-stack/azure-stack-manage-storage-accounts), die den VM-Datenträgern zugeordnet sind.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Spickzettel: Unterschiede zwischen Managed Disks

| Feature | Azure (global) | Azure Stack |
| --- | --- | --- |
|Verschlüsselung für ruhende Daten |Azure Storage Service Encryption (SSE), Azure Disk Encryption (ADE)     |BitLocker-128-Bit-AES-Verschlüsselung      |
|Image          | Unterstützung von verwalteten benutzerdefinierten Images |Noch nicht unterstützt|
|Sicherungsoptionen |Unterstützung für Azure Backup-Dienst |Noch nicht unterstützt |
|Optionen für die Notfallwiederherstellung |Unterstützung von Azure Site Recovery |Noch nicht unterstützt|
|Datenträgertypen     |SSD Premium, SSD Standard (Vorschau) und HDD Standard |SSD Premium, HDD Standard |
|Premium-Datenträger  |Vollständig unterstützt |Kann bereitgestellt werden, jedoch ohne Leistungsgrenzwerte oder Garantien  |
|Premium-Datenträger-IOPs  |Abhängig von der Größe des Datenträgers  |2.300 IOPS pro Datenträger |
|Durchsatz Premium-Datenträger |Abhängig von der Größe des Datenträgers |145 MB/Sekunde pro Datenträger |
|Maximale Datenträgergröße  |4 TB       |1 TB       |
|Analyse Datenträgerleistung |Unterstützung für aggregierte Metriken und Metriken pro Datenträger |Noch nicht unterstützt |
|Migration      |Bereitstellung eines Tools für die Migration von vorhandenen, nicht verwalteten Azure Resource Manager VMs, ohne dass der virtuelle Computer neu erstellt werden muss.  |Noch nicht unterstützt |


## <a name="metrics"></a>Metriken
Es gibt auch Unterschiede zu Speichermetriken:
- Mit Azure Stack unterscheiden die Transaktionsdaten in Speichermetriken nicht zwischen interner und externer Netzwerkbandbreite.
- In Azure Stack beinhalten die Transaktionsdaten in Speichermetriken keinen Zugriff auf die bereitgestellten Datenträger über virtuelle Computer.


## <a name="api-versions"></a>API-Versionen
Azure Stack Managed Disks unterstützt die folgenden API-Versionen:
- 2017-03-30


## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über virtuelle Azure Stack-Computer](azure-stack-compute-overview.md)

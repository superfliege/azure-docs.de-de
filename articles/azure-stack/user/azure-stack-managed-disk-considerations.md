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
ms.openlocfilehash: 4bd36744cc417e85f49e58f9a08d2b9006da9fe4
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284028"
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
|Datenträgergröße  |Azure Premium-Datenträger: P4 (32 GiB) bis P80 (32 TiB)<br>Azure SSD Standard-Datenträger: E10 (128 GiB) bis E80 (32 TiB)<br>Azure HDD Standard-Datenträger: S4 (32 GiB) bis S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1.024 GiB |
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

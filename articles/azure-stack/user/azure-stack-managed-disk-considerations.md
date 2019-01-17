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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 3445974cf832b7ed594f704615482e1d9b0e351c
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159365"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack Managed Disks: Unterschiede und Überlegungen

Dieser Artikel beschreibt die bekannten Unterschiede zwischen [Azure Stack Managed Disks](azure-stack-manage-vm-disks.md) und [Managed Disks für Azure](../../virtual-machines/windows/managed-disks-overview.md). Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Artikel [Key considerations](azure-stack-considerations.md) (Wichtige Aspekte).

Managed Disks vereinfacht die Datenträgerverwaltung für IaaS-VMs durch die Verwaltung der [Speicherkonten](../azure-stack-manage-storage-accounts.md), die den VM-Datenträgern zugeordnet sind.

> [!Note]  
> Managed Disks steht in Azure Stack ab 1808 Update zur Verfügung. Es ist jetzt standardmäßig aktiviert, wenn virtuelle Computer über das Azure Stack-Portal ab 1811 Update erstellt werden.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Cheat Sheet: Unterschiede zwischen verwalteten Datenträgern

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
|Datenträgergröße  |Azure Premium Disk: P4 (32 GiB) bis P80 (32 TiB)<br>Azure SSD Standard-Datenträger: E10 (128 GiB) bis E80 (32 TiB)<br>Azure HDD Standard-Datenträger: S4 (32 GiB) bis S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GB<br>M15: 256 GiB<br>M20: 512 GB<br>M30: 1024 GiB |
|Datenträger-Momentaufnahme Kopie|Momentaufnahme: Verwaltete Azure-Datenträger, die an einen ausgeführten virtuellen Computer angefügt sind, werden unterstützt|Noch nicht unterstützt |
|Analyse Datenträgerleistung |Unterstützung für aggregierte Metriken und Metriken pro Datenträger |Noch nicht unterstützt |
|Migration      |Bereitstellung eines Tools für die Migration von vorhandenen, nicht verwalteten Azure Resource Manager VMs, ohne dass der virtuelle Computer neu erstellt werden muss  |Noch nicht unterstützt |

> [!NOTE]  
> Managed Disks-IOPs und Durchsatz in Azure Stack ist eine Cap-Zahl statt einer bereitgestellten Zahl, die durch Hardware und in Azure Stack ausgeführte Workloads beeinflusst werden kann.

## <a name="metrics"></a>Metriken

Es gibt auch Unterschiede zu Speichermetriken:

- Mit Azure Stack unterscheiden die Transaktionsdaten in Speichermetriken nicht zwischen interner und externer Netzwerkbandbreite.
- In Azure Stack beinhalten die Transaktionsdaten in Speichermetriken keinen Zugriff auf die bereitgestellten Datenträger über virtuelle Computer.

## <a name="api-versions"></a>API-Versionen

Azure Stack Managed Disks unterstützt die folgenden API-Versionen:

- 2017-03-30

## <a name="known-issues"></a>Bekannte Probleme

Nach dem Anwenden des Updates ab 1808 können beim Bereitstellen von virtuellen Computern mit Managed Disks die folgenden Probleme auftreten:

- Wenn das Abonnement vor dem Update 1808 erstellt wurde, schlägt die Bereitstellung eines virtuellen Computers mit Managed Disks möglicherweise mit einer internen Fehlermeldung fehl. Um den Fehler zu beheben, führen Sie die folgenden Schritte für jedes Abonnement aus:
   1. Navigieren Sie im Mandantenportal zu **Abonnements**, und suchen Sie nach dem Abonnement. Klicken Sie auf **Ressourcenanbieter**, klicken Sie dann auf **Microsoft.Compute**, und klicken Sie anschließend auf **Erneut registrieren**.
   2. Navigieren Sie unter dem gleichen Abonnement zu **Zugriffssteuerung (IAM)**, und überprüfen Sie, ob **Azure Stack – Verwalteter Datenträger** aufgeführt wird.
- Wenn Sie eine Umgebung mit mehreren Mandanten konfiguriert haben, schlägt die Bereitstellung von virtuellen Computern in einem Abonnement, dem ein Gastverzeichnis zugeordnet ist, möglicherweise mit einer internen Fehlermeldung fehl. Zum Beheben des Fehlers führen Sie die in [diesem Artikel](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) beschriebenen Schritte aus, um alle Gastverzeichnisse neu zu konfigurieren.


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über virtuelle Azure Stack-Computer](azure-stack-compute-overview.md)

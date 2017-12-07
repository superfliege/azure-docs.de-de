---
title: Migrieren von Azure IaaS-VMs zwischen Azure-Regionen | Microsoft-Dokumentation
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure-IaaS-Computer aus einer Azure-Region in eine andere zu migrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: raynew
ms.openlocfilehash: f645150aa7b93ebbd98899cc8fdd495dfeaf3076
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery

Willkommen bei Azure Site Recovery! Verwenden Sie diesen Artikel, wenn Sie Azure-VMs zwischen Azure-Regionen migrieren möchten.
>[!NOTE]
>
> Informationen zum Replizieren virtueller Azure-Computer in einer anderen Region für die Notfallwiederherstellung und Migration finden Sie in [diesem Dokument](../site-recovery-azure-to-azure.md). Die Site Recovery-Replikation für virtuelle Azure-Computer ist derzeit als Vorschauversion verfügbar.


## <a name="prerequisites"></a>Voraussetzungen
Für diese Bereitstellung benötigen Sie Folgendes:

* **Virtuelle IaaS-Computer**: die virtuellen Computer, die Sie migrieren möchten. Migrieren Sie diese virtuellen Computer, indem Sie sie wie physische Computer behandeln.

## <a name="deployment-steps"></a>Bereitstellungsschritte
Dieser Abschnitt beschreibt die Bereitstellungsschritte im neuen Azure-Portal.

1. [Erstellen Sie einen Tresor](../site-recovery-azure-to-azure.md#create-a-recovery-services-vault).
2. [Aktivieren Sie die Replikation]../(site-recovery-azure-to-azure.md) für die virtuellen Computer, die Sie migrieren möchten, und wählen Sie Azure als Quelle aus.
  >[!NOTE]
  >
  > Derzeit wird die native Replikation virtueller Azure-Computer mit verwalteten Datenträgern nicht unterstützt. Sie können virtuelle Computer mit verwalteten Datenträgern mithilfe der Option „Physical nach Azure“ in [diesem Dokument](../site-recovery-vmware-to-azure.md) migrieren.
3. [Ausführen eines Failovers](../site-recovery-failover.md). Nach Abschluss der ersten Replikation können Sie ein Failover von einer Azure-Region auf eine andere durchführen. Optional können Sie einen Wiederherstellungsplan erstellen und ein Failover durchführen, um mehrere virtuelle Computer zwischen Regionen zu migrieren. [hier](../site-recovery-create-recovery-plans.md) .

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu anderen Replikationsszenarien finden Sie in [Was ist Azure Site Recovery?](../site-recovery-overview.md)

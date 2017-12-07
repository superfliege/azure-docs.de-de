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
ms.date: 08/31/2017
ms.author: raynew
ms.openlocfilehash: 805582d89ee906e21fbe588e895ce0fe3a926a66
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery

Verwenden Sie diesen Artikel, wenn Sie virtuelle Azure-Computer (VMs) mit dem [Site Recovery](../site-recovery-overview.md)-Dienst zwischen Azure-Regionen migrieren möchten.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen die IaaS-VMs, die Sie migrieren möchten.

## <a name="deployment-steps"></a>Bereitstellungsschritte

1. [Erstellen Sie einen Tresor](azure-to-azure-tutorial-enable-replication.md#create-a-vault).
2. [Aktivieren Sie die Replikation](azure-to-azure-tutorial-enable-replication.md#enable-replication) für die virtuellen Computer, die Sie migrieren möchten, und wählen Sie Azure als Quelle aus. Derzeit wird die native Replikation virtueller Azure-Computer mit verwalteten Datenträgern nicht unterstützt.
3. [Ausführen eines Failovers](../site-recovery-failover.md). Nach Abschluss der ersten Replikation können Sie ein Failover von einer Azure-Region auf eine andere durchführen. Optional können Sie einen Wiederherstellungsplan erstellen und ein Failover durchführen, um mehrere virtuelle Computer zwischen Regionen zu migrieren. [hier](../site-recovery-create-recovery-plans.md) .

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu anderen Replikationsszenarien finden Sie unter [Was ist Azure Site Recovery?](../site-recovery-overview.md).

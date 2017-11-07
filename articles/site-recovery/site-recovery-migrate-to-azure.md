---
title: Migrieren von lokalen VMs und physischen Servern zu Azure mit Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie lokale VMs und physische Server mit Azure Site Recovery zu Azure migriert werden.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 423a1727efb0e1fd54eb0f8d5971ace3f8efc6cb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-to-azure-with-site-recovery"></a>Durchführen der Migration zu Azure mit Site Recovery

Lesen Sie diesen Artikel, um weitere Informationen zur Verwendung des Diensts [Azure Site Recovery](site-recovery-overview.md) zum Migrieren von lokalen virtuellen Computern (VMs) und physischen Servern zu Azure-VMs zu erhalten.

## <a name="before-you-start"></a>Vorbereitung

Dieses Video enthält eine kurze Übersicht über die Schritte, die für die Migration zu Azure erforderlich sind.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]


## <a name="what-do-we-mean-by-migration"></a>Was bedeutet „Migration“?

Sie können Site Recovery zum Replizieren und Migrieren von lokalen VMs und physischen Servern bereitstellen.

- Beim Replizieren konfigurieren Sie lokale Computer für die regelmäßige Replikation in Azure. Wenn es dann zu einem Ausfall kommt, führen Sie für die Computer ein Failover vom lokalen Standort zu Azure durch und greifen dort darauf zu. Wenn der lokale Standort wieder verfügbar ist, erfolgt das Failback aus Azure.
- Wenn Sie Site Recovery für die Migration verwenden, replizieren Sie lokale Computer zu Azure. Anschließend führen Sie das Failover von Ihrem lokalen Standort zu Azure durch und schließen den Migrationsprozess ab. Hierbei erfolgt kein Failback.  

## <a name="what-can-site-recovery-migrate"></a>Was kann mit Site Recovery migriert werden?

Ihre Möglichkeiten:

- **Migrieren vom lokalen Standort**: Migrieren Sie lokale Hyper-V-VMs, VMware-VMs und physische Server zu Azure. Nach der Migration werden Workloads, die auf den lokalen Computern ausgeführt werden, auf Azure-VMs ausgeführt. 
- **Migrieren in Azure**: Migrieren Sie Azure-VMs zwischen Azure-Regionen. 
- **Migrieren von AWS**: Migrieren Sie AWS Windows-Instanzen zu Azure IaaS-VMs. 

## <a name="migrate-from-on-premises-to-azure"></a>Migrieren vom lokalen Standort zu Azure

Zum Migrieren von lokalen VMware-VMs, Hyper-V-VMs und physischen Servern führen Sie fast die gleichen Schritte wie bei der vollständigen Replikation aus. 


## <a name="migrate-between-azure-regions"></a>Migrieren zwischen Azure-Regionen

Zum Migrieren von Azure-VMs zwischen Regionen führen Sie fast die gleichen Schritte wie bei der vollständigen Migration aus.

1. Sie [aktivieren die Replikation](azure-to-azure-tutorial-enable-replication.md) für die Computer, die Sie migrieren möchten.
2. Sie [führen ein schnelles Testfailover aus](azure-to-azure-tutorial-dr-drill.md), um sicherzustellen, dass alles richtig funktioniert.
3. Anschließend [führen Sie ein ungeplantes Failover aus](azure-to-azure-tutorial-failover-failback.md), indem Sie die Option **Migration abschließen** verwenden.
4. Nach Abschluss der Migration können Sie die [Replikation für die Notfallwiederherstellung einrichten](site-recovery-azure-to-azure-after-migration.md), und zwar aus der Azure-Region, zu der migriert wurde, in eine sekundäre Region.



## <a name="migrate-aws-to-azure"></a>Migrieren von AWS zu Azure

Sie können AWS-Instanzen zu Azure-VMs migrieren.
- In diesem Szenario wird nur die Migration unterstützt. Anders ausgedrückt: Sie können die AWS-Instanzen replizieren und dafür ein Failover zu Azure durchführen, aber es ist kein Failback möglich.
- AWS-Instanzen werden für Migrationszwecke genauso wie physische Server behandelt. Sie richten einen Recovery Services-Tresor ein, stellen einen lokalen Konfigurationsserver zum Verwalten der Replikation bereit, fügen ihn dem Tresor hinzu und geben die Replikationseinstellungen an.
- Sie aktivieren die Replikation für die Computer, die Sie migrieren möchten, und führen ein schnelles Testfailover aus. Anschließend führen Sie ein ungeplantes Failover aus, indem Sie die Option **Migration abschließen** verwenden.






## <a name="next-steps"></a>Nächste Schritte

- [Migrieren von lokalen Computern zu Azure](tutorial-migrate-on-premises-to-azure.md)
- [Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery](site-recovery-migrate-azure-to-azure.md)
- [Migrieren von AWS zu Azure](tutorial-migrate-aws-to-azure.md)

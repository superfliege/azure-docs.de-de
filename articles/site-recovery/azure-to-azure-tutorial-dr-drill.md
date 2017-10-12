---
title: "Durchführen eines Notfallwiederherstellungsverfahrens für Azure-VMs in eine sekundäre Azure-Region mit Azure Site Recovery (Vorschauversion)"
description: "Erfahren Sie, wie Sie ein Notfallwiederherstellungsverfahren für Azure-VMs in eine sekundäre Azure-Region mit Azure Site Recovery durchführen."
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: a3b453028b7fd32bd3ed22823a337f7a978d9aa7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region-preview"></a>Durchführen eines Notfallwiederherstellungsverfahrens für Azure-VMs in eine sekundäre Azure-Region (Vorschau)

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Tutorial wird erläutert, wie ein Notfallwiederherstellungsverfahren für eine Azure-VM aus einer Azure-Region in eine andere mit einem Testfailover durchgeführt wird. Bei dem Verfahren wird Ihre Replikationsstrategie ohne Datenverlust oder Ausfallzeiten überprüft. Dies hat keinen Einfluss auf Ihre Produktionsumgebung. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen
> * Ausführen eines Testfailovers für eine einzelne VM

## <a name="prerequisites"></a>Voraussetzungen

- Bevor Sie ein Testfailover ausführen, empfehlen wir Ihnen, die VM-Eigenschaften zu überprüfen, um sicherzustellen, dass sie den Erwartungen entsprechen.  Die VM-Einstellungen finden Sie unter **Replizierte Elemente**. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.
- Es wird empfohlen, ein separates Azure-VM-Netzwerk für das Testfailover zu nutzen und nicht das Standardnetzwerk, das bei der Aktivierung der Replikation eingerichtet wurde.


## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf das VM-Symbol **+Testfailover**.

2. Wählen Sie unter **Testfailover** einen Wiederherstellungspunkt für das Failover aus:

   - **Letzte Verarbeitung**: Führt ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt aus, der vom Site Recovery-Dienst verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten verwendet und die Recovery Time Objective (RTO) niedrig gehalten.
   - **Letzte App-Konsistenz**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt.
   - **Benutzerdefiniert**: Wählen Sie einen beliebigen Wiederherstellungspunkt aus.

3. Wählen Sie das virtuelle Azure-Zielnetzwerk aus, mit dem Azure-VMs in der sekundären Region nach dem Failover verbunden werden.

4. Um das Failover zu starten, klicken Sie auf **OK**. Klicken Sie zum Überwachen des Fortschritts auf die VM, um die Eigenschaften aufzurufen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM ausgeführt wird, die passende Größe hat und mit dem entsprechenden Netzwerk verbunden ist.
6. Um die VMs zu löschen, die beim Testfailover erstellt wurden, klicken Sie im replizierten Element oder im Wiederherstellungsplan auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen eines Produktionsfailovers](azure-to-azure-tutorial-failover-failback.md)

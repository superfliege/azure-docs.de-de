---
title: Ausführen eines Failbacks von in einer sekundären Azure-Region replizierten virtuellen Azure-Computern zur Notfallwiederherstellung mit dem Azure Site Recovery-Dienst
description: Es wird beschrieben, wie Sie mit dem Azure Site Recovery-Dienst ein Failback für Azure-VMs durchführen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c8ce05e644ad556542314b17151b808586734824
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315316"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Failback für Azure-VMs zwischen Azure-Regionen

Der Dienst [Azure Site Recovery](site-recovery-overview.md) verwaltet und orchestriert die Replikation, das Failover und das Failback von lokalen Computern sowie Azure-VMs und unterstützt dadurch Ihre Strategie zur Notfallwiederherstellung.

In diesem Tutorial wird beschrieben, wie Sie das Failback für eine einzelne Azure-VM durchführen. Nach dem Failover erfolgt ein Failback auf die primäre Region, wenn sie verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> 
> * Ausführen eines Failbacks für den virtuellen Azure-Computer in der sekundären Region
> * Erneutes Schützen des primären virtuellen Azure-Computers in der sekundären Region
> 
> [!NOTE]
> 
> In diesem Tutorial erfahren Sie, wie Sie mit minimalem Anpassungsaufwand für einige virtuelle Computer ein Failover auf eine Zielregion sowie ein Failback auf die Quellregion ausführen. Ausführlichere Anweisungen finden Sie in den Anleitungsartikeln für virtuelle Azure-Computer.

## <a name="before-you-start"></a>Vorbereitung

> * Vergewissern Sie sich, dass sich der virtuelle Computer im Zustand **Commit für Failover ausgeführt** befindet.
> * Vergewissern Sie sich außerdem, dass die primäre Region verfügbar ist und dass Sie darin neue Ressourcen erstellen und darauf zugreifen können.
> * Stellen Sie sicher, dass die Option für das erneute Schützen aktiviert ist.

## <a name="fail-back-to-the-primary-region"></a>Ausführen eines Failbacks auf die primäre Region

Nachdem virtuelle Computer erneut geschützt wurden, können Sie bei Bedarf ein Failback auf die primäre Region ausführen.

1. Klicken Sie auf **Replizierte Elemente**, und wählen Sie den virtuellen Computer aus, der erneut geschützt wurde.

    ![Ausführen eines Failbacks auf die primäre Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Klicken Sie auf **Testfailover**, um ein Testfailover auf die primäre Region auszuführen.
4. Wählen Sie den Wiederherstellungspunkt und das virtuelle Netzwerk für das Testfailover aus, und wählen Sie anschließend **OK** aus. Sie können den virtuellen Computer überprüfen, der zu Testzwecken in der primären Region erstellt wurde.
5. Klicken Sie nach erfolgreicher Ausführung des Testfailovers auf **Testfailover bereinigen**, um die Ressourcen zu bereinigen, die in der Quellregion für das Testfailover erstellt wurden.
6. Wählen Sie unter **Replizierte Elemente** den virtuellen Computer aus, und klicken Sie auf **Failover**.
7. Wählen Sie unter **Failover** einen Wiederherstellungspunkt für das Failover aus.
    - **Neuester (Standardeinstellung)**: Bei dieser Option werden alle Daten in Site Recovery verarbeitet, und sie bietet den niedrigsten RPO-Wert (Recovery Point Objective).
    - **Letzte Verarbeitung**: Bei dieser Option wird für den virtuellen Computer der Zustand des letzten Wiederherstellungspunkts wiederhergestellt, der von Site Recovery verarbeitet wurde.
    - **Benutzerdefiniert**: Bei dieser Option wird ein Failover auf einen bestimmten Wiederherstellungspunkt ausgeführt. Diese Option eignet sich für ein Testfailover.

8. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Hinweis: Site Recovery bereinigt die Quelle nach dem Failover nicht.
9. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
10. Überprüfen Sie den virtuellen Computer nach Abschluss des Failovers, indem Sie sich bei ihm anmelden. Der Wiederherstellungspunkt kann bei Bedarf geändert werden.
11. Klicken Sie nach der Überprüfung des Failovers auf **Commit**. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht. Die Option „Wiederherstellungspunkt ändern“ ist nicht mehr verfügbar.
12. Für den virtuellen Computer wird angezeigt, dass ein Failover und ein Failback ausgeführt wurden.

    ![VM in primärer und sekundärer Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Die virtuellen Computer für die Notfallwiederherstellung bleiben im heruntergefahrenen Zustand mit aufgehobener Zuordnung. Dies ist so beabsichtigt, da Site Recovery die VM-Informationen speichert, was später ggf. bei einem Failover von der primären auf die sekundäre Region hilfreich ist. Da für die virtuellen Computer mit aufgehobener Zuordnung keine Gebühren anfallen, empfiehlt es sich, sie beizubehalten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [erneuten Schutz](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection).

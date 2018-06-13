---
title: Durchführen eines Failbacks von Azure zu VMware | Microsoft-Dokumentation
description: Nach einem Failover von virtuellen Computern auf Azure können Sie die virtuellen Computer mittels Failback wieder in die lokale Umgebung übertragen. Erfahren Sie, welche Schritte für ein Failback ausgeführt werden.
services: site-recovery
documentationcenter: ''
author: nsoneji
manager: gauravd
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: nisoneji
ms.openlocfilehash: 50dccf6196b7affd3d21087f851b929d0e850f6d
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
ms.locfileid: "29767625"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Failback von Azure zu einem lokalen Standort

In diesem Artikel wird beschrieben, wie Sie für virtuelle Computer ein Failback von Azure Virtual Machines in die lokale VMware-Umgebung durchführen. Befolgen Sie die Anweisungen in diesem Artikel, um ein Failback Ihrer virtuellen VMware-Computer oder Ihrer physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort nach Azure (beschrieben im Tutorial [Failover in Site Recovery](site-recovery-failover.md)) zurück an den lokalen Standort durchzuführen.

## <a name="prerequisites"></a>Voraussetzungen
- Sie sollten vorher unbedingt die Informationen zu den [verschiedenen Typen von Failbacks](concepts-types-of-failback.md) und den entsprechenden Einschränkungen lesen.

> [!WARNING]
> Wenn Sie entweder die [Migration abgeschlossen](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), den virtuellen Computer in eine andere Ressourcengruppe verschoben oder den virtuellen Azure-Computer gelöscht haben, ist danach kein Failback möglich. Wenn Sie den Schutz des virtuellen Computers deaktivieren, ist kein Failback möglich.

> [!WARNING]
> Für einen geschützten physischen Server unter Windows Server 2008 R2 SP1, für den ein Failover auf Azure ausgeführt wurde, kann kein Failback ausgeführt werden.

> [!NOTE]
> Wenn für VMware-VMs ein Failover erfolgt ist, ist kein Failback auf einen Hyper-V-Host möglich.


- Führen Sie vor dem Fortfahren alle Schritte für das erneute Schützen aus, damit sich die virtuellen Computer in einem replizierten Zustand befinden und Sie ein Failover zurück zu einem lokalen Standort initiieren können. Weitere Informationen finden Sie unter [Erneutes Schützen von Azure zu lokal](site-recovery-how-to-reprotect.md).

- Stellen Sie sicher, dass die vCenter-Instanz verbunden ist, bevor Sie ein Failback ausführen. Andernfalls führt das Trennen und erneute Anfügen von Datenträgern an den virtuellen Computer zu einem Fehler.

- Beim Failover zu Azure ist der lokale Standort möglicherweise nicht verfügbar und daher kann der Konfigurationsserver nicht verfügbar oder heruntergefahren sein. Während des erneuten Schützens und des Failbacks sollte der lokale Konfigurationsserver ausgeführt werden und einen fehlerfreien verbundenen Zustand aufweisen. 

- Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, ansonsten schlägt das Failback fehl. Stellen Sie daher sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers ausführen. Im Notfall müssen Sie den Server mit der ursprünglichen IP-Adresse wiederherstellen, damit das Failback funktioniert.

- Der Masterzielserver sollte vor dem Auslösen des erneuten Schützens/Failbacks keine Momentaufnahmen enthalten.

## <a name="overview-of-failback"></a>Übersicht über das Failback
Nach einem Failover zu Azure erfolgt das Failback zu Ihrem lokalen Standort anhand der folgenden Schritte:

1. [Schützen Sie die virtuellen Computer in Azure erneut](site-recovery-how-to-reprotect.md), damit sie mit der Replikation auf virtuelle VMware-Computer an Ihrem lokalen Standort beginnen. Im Rahmen dieses Vorgangs müssen Sie auch folgende Schritte ausführen:
    1. Einrichten eines lokalen Masterziels: Windows-Masterziel für virtuelle Windows-Computer und [Linux-Masterziel](site-recovery-how-to-install-linux-master-target.md) für virtuelle Linux-Computer
    2. Einrichten eines [Prozessservers](site-recovery-vmware-setup-azure-ps-resource-manager.md)
    3. Initiieren des [erneuten Schützens](site-recovery-how-to-reprotect.md) Dabei wird der lokale virtuelle Computer deaktiviert, und die Azure-VM-Daten werden mit den lokalen Datenträgern synchronisiert.

1. Initiieren Sie nach der Replikation Ihrer virtuellen Azure-Computer am lokalen Standort ein Failover von Azure zum lokalen Standort.

1. Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer erneut, damit diese mit der Replikation nach Azure beginnen.

Sehen Sie sich das folgende Video zum Failback zu einem lokalen Standort an, um eine kurze Übersicht zu erhalten.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Schritte für das Failback

> [!IMPORTANT]
> Stellen Sie vor dem Initiieren des Failbacks sicher, dass der erneute Schutz für die virtuellen Computer abgeschlossen ist. Die virtuellen Computer müssen geschützt sein und den Integritätsstatus **OK** aufweisen. Informationen zum erneuten Schutz der virtuellen Computer finden Sie unter [Erneuter Schutz](site-recovery-how-to-reprotect.md).

1. Wählen Sie auf der Seite mit den replizierten Elementen den virtuellen Computer aus, und klicken Sie mit der rechten Maustaste darauf, um **Ungeplantes Failover** auszuwählen.
2. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie dann den Wiederherstellungspunkt aus (den neuesten oder den neuesten App-konsistenten Wiederherstellungspunkt), der für das Failover verwendet werden soll. Der App-konsistente Punkt liegt vor dem aktuellen Zeitpunkt und verursacht einige Datenverluste.
3. Während des Failovers fährt Site Recovery die virtuellen Computer auf Azure herunter. Vergewissern Sie sich, dass das Failback wie erwartet abgeschlossen wurde, und überprüfen Sie anschließend, ob die virtuellen Computer auf Azure heruntergefahren wurden.
4. Ein **Commit** ist erforderlich, um den virtuellen Computer, für den ein Failover durchgeführt wurde, aus Azure zu entfernen. Klicken Sie mit der rechten Maustaste auf das geschützte Element, und klicken Sie dann auf **Commit**. Ein Auftrag entfernt die virtuellen Computer, für die in Azure ein Failover ausgeführt wurde.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Auf welchen Wiederherstellungspunkt kann ich für die virtuellen Computer ein Failback durchführen?

Während des Failbacks haben Sie für das Failback der virtuellen Computer bzw. des Wiederherstellungsplans zwei Möglichkeiten.

- Wenn Sie den letzten Verarbeitungspunkt auswählen, wird für alle virtuellen Computer ein Failover zum letzten verfügbaren Zeitpunkt durchgeführt. Falls im Wiederherstellungsplan eine Replikationsgruppe enthalten ist, wird für jeden virtuellen Computer der Replikationsgruppe ein Failover zu ihrem unabhängigen letzten Zeitpunkt durchgeführt.

    Sie können das Failback eines virtuellen Computers erst ausführen, wenn er mindestens einen Wiederherstellungspunkt aufweist. Ein Failback für einen Wiederherstellungsplan ist erst möglich, wenn alle beteiligten virtuellen Computer mindestens einen Wiederherstellungspunkt aufweisen.

> [!NOTE]
> Bei einem letzten Wiederherstellungspunkt handelt es sich um einen absturzkonsistenten Wiederherstellungspunkt.

- Wenn Sie den anwendungskonsistenten Wiederherstellungspunkt auswählen, wird bei einem Failback für einen einzelnen virtuellen Computer die Wiederherstellung des letzten verfügbaren anwendungskonsistenten Wiederherstellungspunkts durchgeführt. Bei einem Wiederherstellungsplan mit einer Replikationsgruppe erfolgt für jede Replikationsgruppe die Wiederherstellung für den gemeinsam verfügbaren Wiederherstellungspunkt.
Anwendungskonsistente Wiederherstellungspunkte können zeitlich weiter zurück liegen, und es kann zu einem Verlust von Daten kommen.

## <a name="what-happens-to-vmware-tools-post-failback"></a>Was geschieht mit VMware-Tools nach dem Failback?

Bei einem virtuellen Windows-Computer deaktiviert Azure Site Recovery die VMware-Tools während des Failovers. Im Verlauf des Failbacks eines virtuellen Windows-Computers werden die VMware-Tools wieder aktiviert. 


## <a name="reprotect-from-on-premises-to-azure"></a>Erneutes Schützen von einem lokalen Standort nach Azure
Nach Abschluss des Failbacks und dem Initiieren des Commits werden die wiederhergestellten virtuellen Computer in Azure gelöscht. Nun befindet sich Ihr virtueller Computer wieder am lokalen Standort, er ist jedoch nicht geschützt. Führen Sie daher folgende Schritte aus, um die Replikation zu Azure erneut zu starten:

1. Wählen Sie unter **Tresor** > **Einstellung** > **Replizierte Elemente** die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie dann auf **Erneut schützen**.
2. Geben Sie den Wert des Prozessservers an, der zum Zurücksenden von Daten an Azure verwendet werden muss.
3. Klicken Sie auf **OK**, um den Auftrag zum erneuten Schützen zu starten.

> [!NOTE]
> Nachdem ein lokaler virtueller Computer gestartet wurde, dauert es eine Weile, bis der Agent auf dem Konfigurationsserver registriert wird (bis zu 15 Minuten). Während dieses Zeitraums schlägt das erneute Schützen fehl, und eine zurückgegebene Fehlermeldung gibt an, dass der Agent nicht installiert ist. Warten Sie einige Minuten, und versuchen Sie dann noch einmal, das erneute Schützen durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem der Auftrag zum erneuten Schützen abgeschlossen ist, wird der virtuelle Computer zurück nach Azure repliziert, und Sie können ein [Failover](site-recovery-failover.md) ausführen, um die virtuellen Computer wieder nach Azure zu verschieben.



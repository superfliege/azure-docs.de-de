---
title: "Failover und Failback von Hyper-V-VMs, die mit Site Recovery nach einem sekundären Rechenzentrum repliziert werden | Microsoft-Dokumentation"
description: "Erfahren Sie, wie mit Azure Site Recovery ein Failover von Hyper-V-VMs auf Ihren sekundären lokalen Standort und ein Failback auf den primären Standort ausgeführt wird."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44a662fa-2e7a-4996-86df-fdd6d6f5dedf
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/16/2017
ms.author: raynew
ms.openlocfilehash: 8f139070de99c4249207d048d445e86dd41e9060
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Failover und Failback von Hyper-V-VMs, die nach Ihrem sekundären lokalen Standort repliziert werden

Der [Azure Site Recovery](site-recovery-overview.md)-Dienst verwaltet und koordiniert Replikation, Failover und Failback von lokalen Computern sowie virtuellen Azure-Computern (VMs).

In diesem Tutorial wird beschrieben, wie ein Failover einer Hyper-V-VM, die in einer System Center Virtual Machine Manager-Cloud (VMM) verwaltet wird, auf einen sekundären VMM-Standort ausgeführt wird. Nach dem Failover erfolgt ein Failback zum lokalen Standort, wenn er verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Ausführen eines Failovers einer Hyper-V-VM aus einer primären VMM-Cloud auf eine sekundäre VMM-Cloud.
> * Erneutes Schützen vom sekundären Standort zum primären und Ausführen des Failbacks
> * Optionales Starten der Replikation vom primären zurück nach dem sekundären Standort

## <a name="overview"></a>Übersicht

Failover und Failback weisen drei Phasen auf:

1. **Failover zum sekundären Standort**: Ausführen eines Failovers der Computer vom primären auf den sekundären Standort.
2. **Failback vom sekundären Standort**: Replizieren der VMs vom sekundären nach dem primären Standort und Ausführen eines geplanten Failovers, um ein Failback auszuführen.
3. Starten Sie nach dem geplanten Failover optional erneut die Replikation vom primären nach dem sekundären Standort.


## <a name="fail-over-to-a-secondary-site"></a>Failover auf einen sekundären Standort

### <a name="failover-prerequisites"></a>Failovervoraussetzungen

Führen Sie unbedingt eine [Notfallwiederherstellungs-Übung](tutorial-dr-drill-secondary.md) durch, um zu überprüfen, ob alles wie erwartet funktioniert.


### <a name="run-a-failover-from-primary-to-secondary"></a>Ausführen eines Failovers vom primären auf den sekundären Standort

Sie können ein reguläres oder geplantes Failover für virtuelle Hyper-V-Computer ausführen.

- Verwenden Sie ein reguläres Failover für unerwartete Ausfälle. Beim Ausführen dieses Failovers erstellt Site Recovery einen virtuellen Computer am sekundären Standort und fährt ihn hoch. Sie führen das Failover für einen bestimmten Wiederherstellungspunkt aus. Je nachdem, welchen Wiederherstellungspunkt Sie verwenden, können Datenverluste auftreten.
- Ein geplantes Failover kann zu Wartungszwecken oder bei erwarteten Ausfällen verwendet werden. Mit dieser Option vermeiden Sie jeglichen Datenverlust. Wenn ein geplantes Failover ausgelöst wird, werden die Quell-VMs heruntergefahren. Nicht synchronisierte Daten werden synchronisiert, und das Failover wird ausgelöst. 
- 
In diesem Verfahren erfahren Sie, wie Sie ein reguläres Failover durchführen.


1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Failover**.
2. Wählen Sie in **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Sie können eine der folgenden Optionen auswählen:
    - **Neueste** (Standard): Mit dieser Option werden zuerst alle an Site Recovery gesendeten Daten verarbeitet. Sie bietet die niedrigste RPO (Recovery Point Objective), da der nach dem Failover erstellte virtuelle Replikatscomputer über alle Daten verfügt, die bei Auslösung des Failovers nach Site Recovery repliziert wurden.
    - **Letzte Verarbeitung**: Mit dieser Option wird ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde. Diese Option bietet eine niedrige Recovery Time Objective (RTO), da keine Zeit für die Verarbeitung unverarbeiteter Daten aufgewendet wird.
    - **Letzte App-Konsistenz**: Mit dieser Option wird ein Failover des virtuellen Computers auf den letzten App-konsistenten Wiederherstellungspunkt ausgeführt, der von Site Recovery verarbeitet wurde. 
3. Der Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
4. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Site Recovery wird vor dem Auslösen des Failovers auch versuchen, lokale Daten zu synchronisieren, die noch nicht an den sekundären Standort gesendet wurden. Beachten Sie, dass das Failover auch dann fortgesetzt wird, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
5. Sie sollten nun die VM in der sekundären VMM-Cloud sehen.
6. Nachdem Sie die VM überprüft haben, **committen** Sie das Failover. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht.

> [!WARNING]
> **Brechen Sie ein Failover in Bearbeitung nicht ab:** Vor dem Starten des Failovers wird die VM-Replikation beendet. Wenn Sie ein Failover in Bearbeitung abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.  


## <a name="reprotect-and-fail-back-from-secondary-to-primary"></a>Erneutes Schützen und Failback vom sekundären auf den primären Standort

### <a name="prerequisites-for-failback"></a>Voraussetzungen für ein Failback

Um das Failback abzuschließen, stellen Sie sicher, dass die primären und sekundären VMM-Server mit Site Recovery verbunden sind.


### <a name="reprotect-and-fail-back"></a>Erneutes Schützen und Failback

Starten Sie die Replikation vom sekundären nach dem primären Standort, und führen Sie ein Failback auf den primären Standort aus. Sobald die virtuellen Computer wieder am primären Standort ausgeführt werden, können Sie sie wieder nach dem sekundären Standort replizieren.  

1. Klicken Sie in **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer, und aktivieren Sie **Umgekehrt replizieren**. Die Replikation des virtuellen Computers zurück nach dem primären Standort wird gestartet.
2. Klicken Sie auf den virtuellen Computer > **Geplantes Failover**.
3. Überprüfen Sie in **Geplantes Failover bestätigen** die Failoverrichtung (von der sekundären VMM-Cloud), und wählen Sie Quell- und Zielspeicherort. 
4. Geben Sie in **Datensynchronisierung** an, wie Sie synchronisieren möchten:
    - **Daten vor dem Failover synchronisieren (nur Deltaänderungen synchronisieren):** Diese Option minimiert die VM-Ausfallzeit, da die Synchronisierung ohne Herunterfahren der VM ausgeführt wird. Dies funktioniert folgendermaßen:
        - Eine Momentaufnahme der Replikat-VM wird erstellt, und sie wird in den primären Hyper-V-Host kopiert. Die Replikat-VM wird weiterhin ausgeführt.
        - Die Replikat-VM wird heruntergefahren, sodass dort keine neuen Änderungen auftreten. Der abschließende Satz von Deltaänderungen wird zum primären Standort übertragen, und der virtuelle Computer am primären Standort wird gestartet.
    - **Daten nur während Failover synchronisieren (vollständiger Download)**: Verwenden Sie diese Option nach Ausführung am sekundären Standort über einen längeren Zeitraum. Diese Option ist schneller, da mehrere Datenträgeränderungen erwartet werden, und keine Zeit für die Berechnung von Prüfsummen aufgewendet wird. Diese Option führt einen Datenträgerdownload durch. Sie ist auch nützlich, wenn der primäre virtuelle Computer gelöscht wurde.
5. Der Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
6. Initiieren Sie das Failover. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt.
7. Wenn Sie das Synchronisieren der Daten vor dem Failover aktiviert haben, klicken Sie auf **Aufträge** > Auftragsname des geplanten Failovers > **Failover abschließen**, sobald die erste Datensynchronisierung abgeschlossen ist und Sie zum Herunterfahren der Replikats-VM am sekundären Standort bereit sind. Hiermit wird der sekundäre virtuelle Computer heruntergefahren, die neuesten Änderungen werden an den primären Standort übertragen, und der primäre virtuelle Computer wird gestartet.
8. Überprüfen Sie, ob der virtuelle Computer in der primären VMM-Cloud verfügbar ist.
9. Der primäre virtuelle Computer befindet sich nun im Status „Commit Ausstehend“. Klicken Sie auf **Committen**, um das Failover zu committen.
10. Wenn Sie das Replizieren des primären virtuellen Computers zurück nach dem sekundären Standort starten möchten, aktivieren Sie **Umgekehrt replizieren**.


> [!NOTE]
> Die umgekehrte Replikation repliziert nur Änderungen, die seit dem Ausschalten der Replikats-VM aufgetreten sind, und es werden nur Deltaänderungen gesendet.


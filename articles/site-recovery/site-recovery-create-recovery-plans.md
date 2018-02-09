---
title: "Erstellen und Anpassen von Wiederherstellungsplänen für Failover und Wiederherstellung in Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt das Erstellen und Anpassen von Wiederherstellungsplänen in Azure Site Recovery für Failover und Wiederherstellung von virtuellen Computern und physischen Servern"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 9839a989246b28c1a194b8d1f0e99c1bd80ac2e5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="create-recovery-plans"></a>Erstellen von Wiederherstellungsplänen


Dieser Artikel enthält Informationen zum Erstellen und Anpassen von Wiederherstellungsplänen in [Azure Site Recovery](site-recovery-overview.md).

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

 Erstellen Sie Wiederherstellungspläne für folgende Anwendungsbereiche:

* Definieren von Gruppen von Computern, für die das Failover und das anschließende Starten zusammen ausgeführt werden.
* Modellieren von Abhängigkeiten zwischen Computern, indem sie in einer Wiederherstellungsplangruppe zusammengefasst werden Beispiel: Zum Ausführen eines Failovers und Erstellen einer spezifischen Anwendung gruppieren Sie alle virtuellen Computer für diese Anwendung in derselben Wiederherstellungsplangruppe.
* Ausführen eines Failovers. Sie können für einen Wiederherstellungsplan ein Testfailover oder ein geplantes bzw. ungeplantes Failover durchführen.

## <a name="why-use-recovery-plans"></a>Gründe für die Verwendung von Wiederherstellungsplänen

Mit Wiederherstellungsplänen können Sie einen systematischen Wiederherstellungsprozess planen, indem Sie kleine unabhängige Einheiten erstellen, die Sie verwalten können. Diese Einheiten stellen in der Regel eine Anwendung in Ihrer Umgebung dar. Mit einem Wiederherstellungsplan können Sie nicht nur die Abfolge definieren, nach der virtuelle Computer gestartet werden, sondern Sie können auch Aufgaben automatisieren, die während der Wiederherstellung häufig anfallen.


**Eine wichtige Möglichkeit zur Überprüfung, ob Sie auf die Cloudmigration oder die Notfallwiederherstellung vorbereitet sind, ist die folgende Maßnahme: Stellen Sie sicher, dass bei Ihnen alle Anwendungen Teil eines Wiederherstellungsplans sind und die einzelnen Wiederherstellungspläne auf die Wiederherstellung in Microsoft Azure getestet wurden. Nachdem Sie diese Vorbereitungsmaßnahmen getroffen haben, können Sie für Ihr gesamtes Datencenter selbstbewusst die Migration bzw. das Failover zu Microsoft Azure durchführen.**
 
Im Folgenden sind die drei wichtigsten Wertbeiträge eines Wiederherstellungsplans aufgeführt:

### <a name="model-an-application-to-capture-dependencies"></a>Modellieren einer Anwendung für die Erfassung von Abhängigkeiten

Ein Wiederherstellungsplan umfasst eine Gruppe mit virtuellen Computern, die normalerweise eine Anwendung bilden und für die gemeinsam ein Failover durchgeführt wird. Mit den Wiederherstellungsplankonstrukten können Sie diese Gruppe so erweitern, dass Ihre anwendungsspezifischen Eigenschaften erfasst werden.
 
Wir verwenden hier ein Beispiel für eine Anwendung mit drei Ebenen:

* SQL-Back-End
* Middleware
* Web-Front-End

Der Wiederherstellungsplan kann angepasst werden, um sicherzustellen, dass die virtuellen Computer nach einem Failover in der richtigen Reihenfolge bereitgestellt werden. Das SQL-Back-End sollte zuerst, anschließend die Middleware und zuletzt das Web-Front-End gestartet werden. Mit dieser Reihenfolge wird sichergestellt, dass die Anwendung funktioniert, nachdem der letzte virtuelle Computer hochgefahren wurde. Wenn beispielweise die Middleware gestartet wird, wird versucht, dafür eine Verbindung mit der SQL-Ebene herzustellen. Durch den Wiederherstellungsplan wurde dafür gesorgt, dass die SQL-Ebene bereits ausgeführt wird. Außerdem wird durch das Hochfahren der Front-End-Server an letzter Stelle sichergestellt, dass Endbenutzer nicht versehentlich eine Verbindung mit der Anwendungs-URL herstellen, bevor alle Komponenten ausgeführt werden und die Anwendung für die Annahme von Anforderungen bereit ist. Zur Erstellung dieser Abhängigkeiten können Sie den Wiederherstellungsplan anpassen, um Gruppen hinzuzufügen. Wählen Sie anschließend einen virtuellen Computer aus, und ändern Sie die dazugehörige Gruppe, um sie zwischen Gruppen zu verschieben.

![Beispiel für einen Wiederherstellungsplan](./media/site-recovery-create-recovery-plans/rp.png)

Nachdem Sie die Anpassung abgeschlossen haben, können Sie die genauen Wiederherstellungsschritte visualisieren. Hier ist die Reihenfolge der Schritte angegeben, die während des Failovers eines Wiederherstellungsplans ausgeführt werden:

* Der erste Schritt umfasst das Herunterfahren, wobei versucht wird, die lokalen virtuellen Computer auszuschalten (mit Ausnahme eines Testfailovers, bei dem der primäre Standort weiter ausgeführt werden muss).
* Als Nächstes wird das parallele Failover aller virtuellen Computer des Wiederherstellungsplans ausgelöst. Im Failoverschritt werden die Datenträger der virtuellen Computer mit replizierten Daten vorbereitet.
* Zuletzt erfolgt der Reihenfolge nach die Ausführung für die Startgruppen, und die virtuellen Computer der einzelnen Gruppen werden gestartet: zuerst Gruppe 1, anschließend Gruppe 2 und zuletzt Gruppe 3. Falls eine Gruppe mehr als einen virtuellen Computer enthält (z.B. ein Web-Front-End mit Lastenausgleich), werden alle parallel gestartet.

**Durch die gruppenübergreifende Sequenzierung wird sichergestellt, dass Abhängigkeiten zwischen verschiedenen Anwendungsebenen berücksichtigt werden und der RTO-Wert der Anwendungswiederherstellung, falls zutreffend, per Parallelität verbessert werden kann.**

   > [!NOTE]
   > Für Computer, die Teil einer einzelnen Gruppe sind, wird das Failover parallel durchgeführt. Für Computer, die Teil unterschiedlicher Gruppen sind, wird das Failover in der Reihenfolge der Gruppen durchgeführt. Erst nachdem für alle Computer von Gruppe 1 das Failover und der Startvorgang durchgeführt wurden, wird das Failover für die Computer von Gruppe 2 gestartet.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Automatisieren der meisten Wiederherstellungsaufgaben zur Verringerung des RTO-Werts

Das Wiederherstellen von großen Anwendungen kann eine komplexe Aufgabe sein. Außerdem ist es schwierig, sich nach einem Failover oder einer Migration an die genauen Anpassungsschritte zu erinnern. Es kann vorkommen, dass nicht Sie, sondern eine andere Person das Failover auslösen muss, die mit den Eigenheiten der Anwendung nicht vollständig vertraut ist. In Notfällen ist es schwierig und fehleranfällig, wenn zu viele manuelle Schritte im Kopf behalten werden müssen. Mit einem Wiederherstellungsplan können Sie die erforderlichen Aktionen automatisieren, die Sie für die einzelnen Schritte durchführen müssen, indem Sie Microsoft Azure Automation-Runbooks nutzen. Mit Runbooks können Sie häufig anfallende Wiederherstellungsaufgaben automatisieren (wie in den Beispielen unten angegeben). Für Aufgaben, die nicht automatisiert werden können, haben Sie mit Wiederherstellungsplänen auch die Möglichkeit, manuelle Aktionen einzufügen.

* Aufgaben auf dem virtuellen Azure-Computer nach dem Failover: Sind normalerweise erforderlich, damit Sie eine Verbindung mit dem virtuellen Computer herstellen können, z.B.:
    * Erstellen einer öffentlichen IP auf dem virtuellen Computer nach dem Failover
    * Zuweisen einer NSG zur NIC des virtuellen Computers, für den das Failover durchgeführt wurde
    * Hinzufügen eines Lastenausgleichs zu einer Verfügbarkeitsgruppe
* Aufgaben innerhalb des virtuellen Computers nach dem Failover: Hiermit wird die Anwendung neu konfiguriert, damit sie in der neuen Umgebung weiterhin richtig funktioniert, z.B.:
    * Ändern der Datenbank-Verbindungszeichenfolge innerhalb des virtuellen Computers
    * Ändern der Webserverkonfiguration/-regeln

**Mit einem vollständigen Wiederherstellungsplan, mit dem die Aufgaben nach der Wiederherstellung mithilfe von Automation-Runbooks automatisiert werden, können Sie ein Failover per Klick und eine Optimierung des RTO-Werts erzielen.**

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testfailover als Vorbereitung auf einen Notfall

Ein Wiederherstellungsplan kann verwendet werden, um sowohl ein Failover als auch ein Testfailover auszulösen. Sie sollten immer ein Testfailover für die Anwendung durchführen, bevor Sie ein Failover durchführen. Mithilfe des Testfailovers können Sie prüfen, ob die Anwendung am Wiederherstellungsstandort gestartet wird.  Falls Sie etwas übersehen haben, können Sie dann leicht eine Bereinigung auslösen und das Testfailover erneut durchführen. Führen Sie das Testfailover mehrere Male durch, bis Sie über die Sicherheit verfügen, dass die Anwendung reibungslos wiederhergestellt wird.

![Testen des Wiederherstellungsplans](./media/site-recovery-create-recovery-plans/rptest.png)

**Jede Anwendung ist anders, und Sie sollten Wiederherstellungspläne erstellen, die jeweils richtig angepasst sind. Außerdem ändern sich die Anwendungen und ihre Abhängigkeiten in der heutigen dynamischen Datencenter-Welt ständig. Führen Sie für Ihre Anwendungen einmal pro Quartal ein Testfailover durch, um zu prüfen, ob der Wiederherstellungsplan noch aktuell ist.**

## <a name="how-to-create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Klicken Sie auf **Wiederherstellungspläne** > **Wiederherstellungsplan erstellen**.
   Geben Sie einen Namen für den Wiederherstellungsplan sowie die Quelle und das Ziel an. Der Quellspeicherort muss virtuelle Computer aufweisen, die für Failover und Wiederherstellung aktiviert sind. Wählen Sie basierend auf den virtuellen Computern, die Teil des Wiederherstellungsplans sein sollen, eine Quelle und ein Ziel aus. 

   |Szenario                   |Quelle               |Ziel           |
   |---------------------------|---------------------|-----------------|
   |Azure zu Azure             |Azure-Region         |Azure-Region     |
   |VMware zu Azure            |Konfigurationsserver |Azure            |
   |VMM zu Azure               |VMM-Anzeigename    |Azure            |
   |Hyper-V-Site zu Azure      |Name der Hyper-V-Site    |Azure            |
   |Physische Computer zu Azure |Konfigurationsserver |Azure            |
   |VMM zu VMM                 |VMM-Anzeigename    |VMM-Anzeigename|

   > [!NOTE]
   > Ein Wiederherstellungsplan kann virtuelle Computer mit derselben Quelle und demselben Ziel aufweisen. Virtuelle Computer vom Typ VMware und VMM können nicht Teil desselben Wiederherstellungsplans sein. Virtuelle VMware-Computer und physische Computer können aber demselben Plan hinzugefügt werden, da als Quelle jeweils ein Konfigurationsserver dient.

2. Wählen Sie unter **Virtuelle Computer auswählen** die virtuellen Computer (oder die Replikationsgruppe) aus, die Sie der Standardgruppe (Gruppe 1) im Wiederherstellungsplan hinzufügen möchten. Nur die virtuellen Computer, die auf der Quelle geschützt waren (gemäß Auswahl im Wiederherstellungsplan) und auch auf dem Ziel geschützt sind (gemäß Auswahl im Wiederherstellungsplan) können ausgewählt werden.

## <a name="how-to-customize-and-extend-recovery-plans"></a>Anpassen und Erweitern von Wiederherstellungsplänen

Sie können Wiederherstellungspläne anpassen und erweitern, indem Sie auf dem Ressourcenblatt für den Site Recovery-Wiederherstellungsplan auf die Registerkarte „Anpassen“ klicken.

Sie können Wiederherstellungspläne anpassen und erweitern:

- **Hinzufügen neuer Gruppen:** Fügen Sie (bis zu sieben) zusätzliche Wiederherstellungsplangruppen zur Standardgruppe hinzu, und fügen Sie dann weitere Computer oder Replikationsgruppen zu diesen Wiederherstellungsplangruppen hinzu. Die Gruppen sind in der Reihenfolge nummeriert, in der sie von Ihnen hinzugefügt werden. Ein virtueller Computer oder eine Replikationsgruppe kann nur in eine Wiederherstellungsplangruppe eingefügt werden.
- **Hinzufügen einer manuellen Aktion**: Sie können manuelle Aktionen hinzufügen, die vor oder nach einer Wiederherstellungsplangruppe ausgeführt werden. Wenn der Wiederherstellungsplan ausgeführt wird, wird er an dem Punkt angehalten, an dem Sie die manuelle Aktion eingefügt haben. Ein Dialogfeld fordert Sie auf anzugeben, dass die manuelle Aktion abgeschlossen wurde.
- **Hinzufügen eines Skripts:** Sie können Skripts für die Ausführung vor oder nach einer Wiederherstellungsplangruppe hinzufügen. Hierbei wird für die Gruppe ein neuer Satz mit Aktionen hinzugefügt. Eine Gruppe von Vorabschritten für „Group 1“ wird beispielsweise mit dem folgenden Namen erstellt: „Group 1: Pre-steps“. Alle Vorabschritte werden in dieser Gruppe aufgelistet. Sie können am primären Standort nur dann ein Skript hinzufügen, wenn Sie einen VMM-Server bereitgestellt haben. [Weitere Informationen](site-recovery-how-to-add-vmmscript.md)
- **Hinzufügen von Azure-Runbooks:** Sie können Wiederherstellungspläne mit Azure-Runbooks erweitern, beispielsweise zum Automatisieren von Aufgaben oder zum Erstellen einer Wiederherstellung in einem Schritt. [Weitere Informationen](site-recovery-runbook-automation.md)


## <a name="how-to-add-a-script-runbook-or-manual-action-to-a-plan"></a>Hinzufügen eines Skripts, eines Runbooks oder einer manuellen Aktion zu einem Plan

Sie können der standardmäßigen Wiederherstellungsplangruppe ein Skript oder eine manuelle Aktion hinzufügen, nachdem Sie die virtuellen Computer oder Replikationsgruppen hinzugefügt und den Plan erstellt haben.

1. Öffnen Sie den Wiederherstellungsplan.
2. Klicken Sie auf ein Element in der Liste **Schritte**, und klicken Sie dann auf **Skript** oder **Manuelle Aktion**.
3. Geben Sie an, ob Sie das Skript oder die Aktion vor oder nach dem ausgewählten Eintrag hinzufügen möchten. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten**, um die Position des Skripts nach oben oder unten zu verschieben.
4. Wenn Sie ein VMM-Skript hinzufügen, wählen Sie **Failover to VMM script** (Failover zu VMM-Skript) aus. Geben Sie unter **Skriptpfad** den relativen Pfad zur Freigabe ein. Geben Sie im VMM-Beispiel unten folgenden Pfad an: **\RPScripts\RPScript.PS1**.
5. Wenn Sie ein Azure-Automatisierungsrunbook hinzufügen, geben Sie das Azure Automation-Konto an, unter dem sich das Runbook befindet, und wählen das gewünschte Azure-Runbookskript aus.
6. Führen Sie ein Failover für den Wiederherstellungsplan aus, um sicherzustellen, dass das Skript wie erwartet funktioniert.

Die Skript- bzw. Runbookoptionen sind in den folgenden Szenarien nur verfügbar, wenn ein Failover oder Failback durchgeführt wird. Eine manuelle Aktion ist sowohl für Failover als auch für Failbacks verfügbar.


|Szenario               |Failover |Failback |
|-----------------------|---------|---------|
|Azure zu Azure         |Runbooks |Runbook  |
|VMware zu Azure        |Runbooks |Nicht verfügbar       | 
|VMM zu Azure           |Runbooks |Skript   |
|Hyper-V-Site zu Azure  |Runbooks |Nicht verfügbar       |
|VMM zu VMM             |Skript   |Skript   |


## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen](site-recovery-failover.md) zum Ausführen von Failovern

Sehen Sie sich dieses Video an, um den Wiederherstellungsplan in Aktion zu erleben.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]

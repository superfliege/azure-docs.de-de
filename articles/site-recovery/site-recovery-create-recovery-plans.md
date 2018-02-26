---
title: "Erstellen und Anpassen eines Wiederherstellungsplans für Failover und Wiederherstellung in Azure Site Recovery | Microsoft Docs"
description: "Erfahren Sie, wie Sie in Azure Site Recovery Wiederherstellungspläne erstellen und anpassen können. Dieser Artikel beschreibt, wie Sie Failover und Wiederherstellung von virtuellen Computern und physischen Servern ausführen können."
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
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Erstellen eines Wiederherstellungsplans mit Site Recovery

Dieser Artikel beschreibt, wie Sie einen Wiederherstellungsplan in [Azure Site Recovery](site-recovery-overview.md) erstellen und anpassen.

Erstellen Sie einen Wiederherstellungsplan, um die folgenden Aktionen auszuführen:

* Definieren von Gruppen von Computern, für die das Failover und das anschließende Starten zusammen ausgeführt werden.
* Modellieren von Abhängigkeiten zwischen Computern, indem sie in einer Wiederherstellungsplangruppe zusammengefasst werden. Beispiel: Zum Ausführen eines Failovers und Erstellen einer spezifischen Anwendung schließen Sie alle virtuellen Computer für diese Anwendung in dieselbe Wiederherstellungsplangruppe ein.
* Ausführen eines Failovers. Sie können für einen Wiederherstellungsplan ein Testfailover oder ein geplantes bzw. ungeplantes Failover durchführen.

## <a name="why-use-a-recovery-plan"></a>Gründe für die Verwendung eines Wiederherstellungsplans

Mit Wiederherstellungsplänen können Sie einen systematischen Wiederherstellungsprozess vorbereiten, indem Sie kleine unabhängige Einheiten erstellen, die Sie verwalten können. Diese Einheiten stellen in der Regel eine Anwendung in Ihrer Umgebung dar. Ein Wiederherstellungsplan kann Ihnen dabei helfen, die Reihenfolge zu definieren, in der die virtuellen Computer gestartet werden. Sie können einen Wiederherstellungsplan auch verwenden, um allgemeine Aufgaben während der Wiederherstellung zu automatisieren.

> [!TIP]
> Eine Möglichkeit, um zu überprüfen, ob Sie auf Cloudmigration oder Notfallwiederherstellung vorbereitet sind, besteht darin, sicherzustellen, dass jede Ihrer Anwendungen Teil eines Wiederherstellungsplans ist. Stellen Sie außerdem sicher, dass jeder Wiederherstellungsplan für die Wiederherstellung in Azure getestet wird. Nachdem Sie diese Vorbereitungsmaßnahmen getroffen haben, können Sie für Ihr gesamtes Datencenter die Migration bzw. das Failover zu Azure sicher ausführen.
 
Ein Wiederherstellungsplan umfasst drei zentrale Wertvorschläge:

* Modellieren einer Anwendung für die Erfassung von Abhängigkeiten.
* Automatisieren der meisten Wiederherstellungsaufgaben zur Verringerung des RTO-Werts.
* Testfailover als Vorbereitung auf einen Notfall.

### <a name="model-an-application-to-capture-dependencies"></a>Modellieren einer Anwendung für die Erfassung von Abhängigkeiten

Ein Wiederherstellungsplan umfasst eine Gruppe mit virtuellen Computern, die normalerweise eine Anwendung bilden und für die gemeinsam ein Failover ausgeführt wird. Mit den Wiederherstellungsplankonstrukten können Sie eine Wiederherstellungsplangruppe so erweitern, dass Ihre anwendungsspezifischen Eigenschaften erfasst werden. 

In diesem Artikel verwenden wir eine typische dreischichtige Anwendung, die über ein SQL-Back-End, Middleware und ein Web-Front-End verfügt. Sie können den Wiederherstellungsplan anpassen, um sicherzustellen, dass die virtuellen Computer nach einem Failover in der richtigen Reihenfolge gestartet werden. Das SQL-Back-End sollte zuerst gestartet werden, dann die Middleware und das Web-Front-End zuletzt. Mit dieser Reihenfolge wird sichergestellt, dass die Anwendung funktioniert, nachdem der letzte virtuelle Computer gestartet wurde. 

Wenn die Middleware z.B. gestartet wird, versucht sie, eine Verbindung mit der SQL-Schicht herzustellen. Der Wiederherstellungsplan stellt sicher, dass die SQL-Schicht bereits ausgeführt wird. Außerdem wird durch das Starten eines Front-End-Servers an letzter Stelle sichergestellt, dass Endbenutzer keine Verbindung mit der Anwendungs-URL herstellen, bevor alle Komponenten ausgeführt werden und die Anwendung für die Annahme von Anforderungen bereit ist. Um diese Abhängigkeiten zu erstellen, passen Sie den Wiederherstellungsplan an, um Gruppen hinzuzufügen, und wählen Sie dann einen virtuellen Computer aus. Ändern Sie zum Verschieben eines virtuellen Computers zwischen Gruppen die Gruppe des virtuellen Computers.

![Screenshot eines Beispielwiederherstellungsplans in Site Recovery](./media/site-recovery-create-recovery-plans/rp.png)

Nachdem Sie die Anpassung abgeschlossen haben, können Sie die genauen Wiederherstellungsschritte visualisieren. Dies ist die Reihenfolge der Schritte, die während des Failovers eines Wiederherstellungsplans ausgeführt werden:

1. Ein Schritt zum Herunterfahren versucht, den virtuellen Computer lokal zu deaktivieren. (Mit Ausnahme bei einem Testfailover, bei dem der primäre Standort weiterhin ausgeführt werden muss.)
2. Der Versuch des Herunterfahrens löst das parallele Failover aller virtuellen Computer des Wiederherstellungsplans aus. Im Failoverschritt werden die Datenträger der virtuellen Computer mit replizierten Daten vorbereitet.
3. Die Startgruppen werden in ihrer Reihenfolge ausgeführt und starten die virtuellen Computer in jeder Gruppe. Zuerst wird Gruppe 1 ausgeführt, dann Gruppe 2 und schließlich Gruppe 3. Wenn in einer Gruppe mehrere virtuelle Computer vorhanden sind (z.B. ein Web-Front-End mit Lastenausgleich), werden alle virtuellen Computer parallel gestartet.

> [!TIP]
> Die gruppenübergreifende Sequenzierung stellt sicher, dass Abhängigkeiten zwischen verschiedenen Anwendungsschichten berücksichtigt werden. Parallelität verbessert den RTO-Wert der Anwendungswiederherstellung (wo dies sinnvoll ist).

   > [!NOTE]
   > Für Computer, die Teil einer einzelnen Gruppe sind, wird das Failover parallel ausgeführt. Für Computer, die Teil unterschiedlicher Gruppen sind, wird das Failover in der Reihenfolge der Gruppen ausgeführt. Erst nachdem für alle Computer von Gruppe 1 das Failover und der Startvorgang ausgeführt wurden, wird das Failover für die Computer von Gruppe 2 gestartet.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Automatisieren der meisten Wiederherstellungsaufgaben zur Verringerung des RTO-Werts

Das Wiederherstellen von großen Anwendungen kann eine komplexe Aufgabe sein. Es ist schwierig, sich in Zeiten des Chaos viele manuelle Schritte merken zu müssen, und es macht den Vorgang fehleranfällig. Auch kann eine Person, die sich der Komplexität der Anwendung nicht bewusst ist, das Failover tatsächlich auslösen. 

Sie können einen Wiederherstellungsplan verwenden, um die erforderlichen Aktionen, die Sie bei jedem Schritt ausführen müssen, zu automatisieren. Sie können die erforderlichen Aktionen mithilfe von Azure Automation-Runbooks einrichten. Mit Runbooks können Sie häufig anfallende Wiederherstellungsaufgaben automatisieren, z.B. die in den folgenden Beispielen angegebenen Aufgaben. Für Aufgaben, die nicht automatisiert werden können, können Sie manuelle Aktionen in Ihre Wiederherstellungspläne einfügen.

* **Aufgaben auf dem virtuellen Azure-Computer nach dem Failover**: Diese Aufgaben sind normalerweise erforderlich, um eine Verbindung mit dem virtuellen Computer herstellen zu können. Beispiele:
    * Erstellen einer öffentlichen IP-Adresse auf dem virtuellen Computer nach dem Failover.
    * Zuweisen einer Netzwerksicherheitsgruppe zur Netzwerkkarte auf dem virtuellen Computer mit Failover.
    * Hinzufügen eines Lastenausgleichs zu einer Verfügbarkeitsgruppe.
* **Aufgaben innerhalb des virtuellen Computers nach dem Failover**: Mit diesen Aufgaben wird die Anwendung neu konfiguriert, damit sie in der neuen Umgebung weiterhin ordnungsgemäß funktioniert. Beispiele:
    * Ändern der Datenbank-Verbindungszeichenfolge innerhalb des virtuellen Computers.
    * Ändern der Webserverkonfiguration oder -regeln.

> [!TIP]
> Erreichen von Failover mit einem Klick und Optimieren des RTO-Werts, indem ein vollständiger Wiederherstellungsplan erstellt wird, der die Aufgaben nach der Wiederherstellung mithilfe von Automation-Runbooks automatisiert.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testfailover als Vorbereitung auf einen Notfall

Sie können einen Wiederherstellungsplan verwenden, um ein Failover oder ein Testfailover auszulösen. Sie sollten immer ein Testfailover für die Anwendung abschließen, bevor Sie ein Failover ausführen. Mithilfe des Testfailovers können Sie überprüfen, ob die Anwendung am Wiederherstellungsstandort gestartet wird. Falls Sie in Ihrem Setup etwas übersehen haben, können Sie dann leicht eine Bereinigung auslösen und das Testfailover anschließend erneut ausführen. Führen Sie das Testfailover mehrere Male aus, bis Sie sicher sind, dass die Anwendung reibungslos wiederhergestellt wird.

![Screenshot eines Beispiel-Testwiederherstellungsplans in Site Recovery](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Da jede Anwendung anders ist, sollten Sie Wiederherstellungspläne erstellen, die für jede Anwendung entsprechend angepasst sind. 
>
> In der dynamischen, auf Datencenter ausgerichteten Umgebung von heute ändern sich Anwendungen und ihre Abhängigkeiten außerdem häufig. Um sicherzustellen, dass der Wiederherstellungsplan aktuell ist, testen Sie das Failover für Ihre Anwendungen vierteljährlich.

## <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Wählen Sie **Wiederherstellungspläne** > **Wiederherstellungsplan erstellen** aus.
   Geben Sie einen Namen für den Wiederherstellungsplan sowie die Quelle und das Ziel an. Der Quellspeicherort muss virtuelle Computer aufweisen, die für Failover und Wiederherstellung aktiviert sind. Wählen Sie basierend auf den virtuellen Computern, die Teil des Wiederherstellungsplans sein sollen, eine Quelle und ein Ziel aus. 

   |Szenario                   |Quelle               |Ziel           |
   |---------------------------|---------------------|-----------------|
   |Azure zu Azure             |Azure-Region         |Azure-Region     |
   |VMware zu Azure            |Konfigurationsserver |Azure            |
   |Virtual Machine Manager (VMM) zu Azure               |VMM-Anzeigename    |Azure            |
   |Hyper-V-Standort zu Azure      |Name der Hyper-V-Site    |Azure            |
   |Physische Computer zu Azure |Konfigurationsserver |Azure            |
   |VMM zu VMM                 |VMM-Anzeigename    |VMM-Anzeigename|

   > [!NOTE]
   > Ein Wiederherstellungsplan kann virtuelle Computer mit derselben Quelle und demselben Ziel aufweisen. Virtuelle VMware- und System Center Virtual Machine Manager-Computer (VMM) dürfen sich nicht im gleichen Wiederherstellungsplan befinden. Allerdings können Sie virtuelle VMware-Computer und physische Computer dem gleichen Wiederherstellungsplan hinzufügen. In diesem Fall ist die Quelle für beide Computer ein Konfigurationsserver.

2. Wählen Sie unter **Virtuelle Computer auswählen** die virtuellen Computer (oder die Replikationsgruppe) aus, die Sie der Standardgruppe (Gruppe 1) im Wiederherstellungsplan hinzufügen möchten. Sie können nur virtuelle Computer auswählen, die auf der Quelle geschützt waren (gemäß Auswahl im Wiederherstellungsplan) und auch auf dem Ziel geschützt sind (gemäß Auswahl im Wiederherstellungsplan).

## <a name="customize-and-extend-recovery-plans"></a>Anpassen und Erweitern von Wiederherstellungsplänen

Zum Anpassen und Erweitern von Wiederherstellungsplänen navigieren Sie zum Bereich für die Site Recovery-Wiederherstellungsplanressource. Wählen Sie die Registerkarte **Anpassen** aus. Sie können Wiederherstellungspläne mithilfe der folgenden Optionen anpassen und erweitern:

- **Neue Gruppen hinzufügen**: Sie können bis zu sieben zusätzliche Wiederherstellungsplangruppen zur Standardgruppe hinzufügen. Sie können diesen Wiederherstellungsplangruppen weitere Computer oder Replikationsgruppen hinzufügen. Die Gruppen sind in der Reihenfolge nummeriert, in der sie von Ihnen hinzugefügt werden. Sie können einen virtuellen Computer oder eine Replikationsgruppe nur in eine Wiederherstellungsplangruppe einfügen.
- **Hinzufügen einer manuellen Aktion**: Sie können manuelle Aktionen hinzufügen, die vor oder nach einer Wiederherstellungsplangruppe ausgeführt werden. Wenn der Wiederherstellungsplan ausgeführt wird, wird er an dem Punkt angehalten, an dem Sie die manuelle Aktion eingefügt haben. Ein Dialogfeld fordert Sie auf anzugeben, dass die manuelle Aktion abgeschlossen wurde.
- **Hinzufügen eines Skripts**: Sie können Skripts für die Ausführung vor oder nach einer Wiederherstellungsplangruppe hinzufügen. Hierbei wird für die Gruppe ein neuer Satz mit Aktionen hinzugefügt. Eine Gruppe von Vorabschritten für „Group 1“ wird beispielsweise mit dem Namen *Group 1: pre-steps* erstellt. Alle Vorabschritte werden in dieser Gruppe aufgelistet. Sie können am primären Standort nur dann ein Skript hinzufügen, wenn Sie einen VMM-Server bereitgestellt haben. Weitere Informationen finden Sie unter [Hinzufügen eines VMM-Skripts zu einem Wiederherstellungsplan](site-recovery-how-to-add-vmmscript.md).
- **Hinzufügen von Azure-Runbooks**: Sie können Wiederherstellungspläne mit Azure Runbooks erweitern. Beispielsweise können Sie ein Runbook verwenden, um Aufgaben zu automatisieren oder eine Einzelschrittwiederherstellung zu erstellen. Weitere Informationen finden Sie unter [Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Hinzufügen eines Skripts, eines Runbooks oder einer manuellen Aktion zu einem Plan

Nachdem Sie virtuelle Computer oder Replikationsgruppen zu einer standardmäßigen Wiederherstellungsplangruppe hinzugefügt haben, können Sie der Wiederherstellungsplangruppe ein Skript oder eine manuelle Aktion hinzufügen.

1. Öffnen Sie den Wiederherstellungsplan.
2. Wählen Sie in der Liste **Schritte** ein Element aus. Wählen Sie dann **Skript** oder **Manuelle Aktion** aus.
3. Geben Sie an, ob Sie das Skript oder die Aktion vor oder hinter dem ausgewählten Eintrag hinzufügen möchten. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten**, um die Position des Skripts nach oben oder unten zu verschieben.
4. Wenn Sie ein VMM-Skript hinzufügen, wählen Sie **Failover to VMM script** (Failover zu VMM-Skript) aus. Geben Sie unter **Skriptpfad** den relativen Pfad zur Freigabe ein. Beispiel: **\RPScripts\RPScript.PS1**.
5. Wenn Sie ein Automation-Runbook hinzufügen, geben Sie das Automation-Konto an, in dem sich das Runbook befindet. Wählen Sie dann das Azure-Runbook-Skript aus.
6. Führen Sie ein Failover für den Wiederherstellungsplan aus, um sicherzustellen, dass das Skript wie erwartet funktioniert.

Die Skript- bzw. Runbookoptionen sind nur in den folgenden Szenarien und während eines Failovers oder Failbacks verfügbar. Eine manuelle Aktion ist sowohl für Failover als auch für Failbacks verfügbar.


|Szenario               |Failover |Failback |
|-----------------------|---------|---------|
|Azure zu Azure         |Runbook |Runbook  |
|VMware zu Azure        |Runbook |Nicht verfügbar       | 
|VMM zu Azure           |Runbook |Skript   |
|Hyper-V-Standort zu Azure  |Runbook |Nicht verfügbar       |
|VMM zu VMM             |Skript   |Skript   |


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [Ausführen von Failovern](site-recovery-failover.md).  
* Sehen Sie sich dieses Video an, um den Wiederherstellungsplan in Aktion zu erleben:
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]

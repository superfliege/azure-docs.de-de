---
title: Erstellen eines Watchertasks im Azure Automation-Kontos | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie einen Watchertask im Azure Automation-Konto erstellen, um die Erstellung neuer Dateien in einem Ordner zu überwachen."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/15/2017
ms.author: eamono
ms.openlocfilehash: 0ddd31f7ce2217c1136eccd391bb30bd4461c3e5
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="azure-automation-watcher-tasks-enable-you-to-respond-to-events-happening-in-your-local-datacenter"></a>Mit Azure Automation-Watchertasks können Sie auf Ereignisse reagieren, die in Ihrem lokalen Rechenzentrum auftreten.

In diesem Tutorial erfahren Sie, wie Sie einen neuen Watchertask erstellen, um Folgendes durchzuführen:

> [!div class="checklist"]
> * Erstellen Sie ein Watcherrunbook, das in einem Verzeichnis nach neuen Dateien sucht.
> * Erstellen Sie eine Automation-Variable, um den letzten Zeitpunkt zu bewahren, an dem eine Datei vom Watcher verarbeitet wurde.
> * Erstellen Sie ein Aktionsrunbook, das aufgerufen wird, wenn das Watcherrunbook eine neue Datei findet.
> * Erstellen Sie einen Watchertask, der das Watcherrunbook und das Aktionsrunbook auswählt.
> * Lösen Sie einen Watcher aus, indem Sie eine neue Datei zu einem Verzeichnis hinzufügen.
> * Untersuchen Sie die Ausgabe des Aktionsrunbooks, das Informationen zur neuen Dateien anzeigt.  

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Tutorials müssen folgende Voraussetzungen erfüllt sein.
+ Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
+ [Automation-Konto](automation-offering-get-started.md), um die Watcher- und Aktionsrunbooks und den Watchertask aufzunehmen.
+ [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), in dem der Watchertask ausgeführt wird.

## <a name="create-a-watcher-runbook-that-looks-for-new-files"></a>Erstellen eines Watcherrunbooks für die Suche nach neuen Dateien
1.  Öffnen Sie das Automation-Konto, und klicken Sie auf die Seite „Runbooks“.
2.  Klicken Sie auf die Schaltfläche „Katalog durchsuchen“.
![Runbookliste über die Benutzeroberfläche](media/automation-watchers-tutorial/WatcherTasksRunbookList.png)
3.  Suchen Sie nach „Watch-NewFile“, und importieren Sie das Runbook in das Automation-Konto.
![Veröffentlichen des Runbooks über die Benutzeroberfläche](media/automation-watchers-tutorial/Watch-NewFileRunbook.png)
4.  Klicken Sie auf „Bearbeiten“, um die Runbookquelle anzuzeigen, und klicken Sie dann auf die Schaltfläche „Veröffentlichen“.

## <a name="create-an-automation-variable-to-keep-the-last-time-a-file-was-processed-by-the-watcher"></a>Erstellen einer Automation-Variable, um den letzten Zeitpunkt zu bewahren, an dem eine Datei vom Watcher verarbeitet wurde
1.  Öffnen Sie die Seite „Variablen“ unter „FREIGEGEBENE RESSOURCEN“, und klicken Sie auf „Variable hinzufügen“. ![Auflisten von Variablen über die Benutzeroberfläche](media/automation-watchers-tutorial/WatcherVariableList.png)
2.  Geben Sie „Watch-NewFileTimestamp“ für den Namen ein.
3.  Wählen Sie für „Typ“ die Option „DateTime“ aus, und klicken Sie dann auf die Schaltfläche „Erstellen“.
![Erstellen einer Wasserzeichenvariablen über die Benutzeroberfläche](media/automation-watchers-tutorial/WatcherWatermarkVariable.png)

## <a name="create-an-action-runbook-that-is-called-when-the-watcher-runbook-finds-a-new-file"></a>Erstellen eines Aktionsrunbooks, das aufgerufen wird, wenn das Watcherrunbook eine neue Datei findet
1.  Klicken Sie unter der Kategorie „PROZESSAUTOMATISIERUNG“ auf die Seite „Runbooks“.
2.  Klicken Sie auf die Schaltfläche „Katalog durchsuchen“.
3.  Suchen Sie nach „Process-NewFile“, und importieren Sie das Runbook in das Automation-Konto.
4.  Klicken Sie auf „Bearbeiten“, um die Runbookquelle anzuzeigen, und klicken Sie dann auf die Schaltfläche „Veröffentlichen“.
![Verarbeiten des Watchers über die Benutzeroberfläche](media/automation-watchers-tutorial/Watch-ProcessNewFile.png)


## <a name="create-a-watcher-task-that-selects-the-watcher-runbook-and-action-runbook"></a>Erstellen eines Watchertasks, der das Watcherrunbook und das Aktionsrunbook auswählt
1.  Öffnen Sie die Seite „Watchertasks“, und klicken Sie auf die Schaltfläche „Watchertask hinzufügen“.
![Watcherliste über die Benutzeroberfläche](media/automation-watchers-tutorial/WatchersList.png)
2.  Geben Sie „WatchMyFolder“ als Namen ein.
3.  Wählen Sie „Watcher konfigurieren“ und dann das Runbook „Watch-NewFile“ aus.
![Konfigurieren des Watchers über die Benutzeroberfläche](media/automation-watchers-tutorial/ConfigureWatcher.png)
4.  Geben Sie die folgende Werte für die Parameter ein:
    *   ORDNERPFAD: Ein Ordner auf dem Hybrid Worker, auf dem die neuen Dateien erstellt werden.
    *   ERWEITERUNG: Lassen Sie dieses Feld leer, um alle Dateierweiterungen zu verarbeiten.
    *   REKURSIV DURCHLAUFEN: Übernehmen Sie den Standardwert.
    *   AUSFÜHRUNGSEINSTELLUNGEN: Wählen Sie den Hybrid Worker aus.
5.  Klicken Sie auf „OK“ und dann auf „Auswählen“, um zur Watcherseite zurückzukehren.
6.  Wählen Sie „Aktion konfigurieren“ und dann das Runbook „Prozess-NewFile“ aus.
![Konfigurieren der Watcheraktion über die Benutzeroberfläche](media/automation-watchers-tutorial/ConfigureAction.png)
7.  Geben Sie die folgende Werte für die Parameter ein:
    *   EREIGNISDATEN: Lassen Sie dieses Feld leer. Die Daten werden vom Watcherrunbook übergeben.
    *   AUSFÜHRUNGSEINSTELLUNGEN: Übernehmen Sie hier „Azure“, da dieses Runbook im Automation-Dienst ausgeführt wird.
8.  Klicken Sie auf „OK“ und dann auf „Auswählen“, um zur Watcherseite zurückzukehren.
9.  Klicken Sie auf „OK“, um den Watchertask zu erstellen.

## <a name="trigger-a-watcher-by-adding-a-new-file-to-a-directory"></a>Auslösen eines Watchers, indem eine neue Datei zu einem Verzeichnis hinzugefügt wird
1.  Wechseln Sie remote in den Hybrid Worker.
2.  Fügen Sie eine neue Textdatei zu dem Ordner hinzu, der vom Watchertask überwacht wird.

## <a name="inspect-the-output-from-the-action-runbook-that-shows-information-on-the-new-file"></a>Untersuchen der Ausgabe des Aktionsrunbooks, das Informationen zur neuen Dateien anzeigt
1.  Klicken Sie auf den Watchertask für „WatchMyFolder“.
2.  Klicken Sie auf „Watcherdatenströme anzeigen“, um zu überprüfen, ob der Watcher die neue Datei gefunden und das Aktionsrunbook gestartet hat.
3.  Klicken Sie auf „Watcheraktionsaufträge anzeigen“, um den Aktionsrunbookauftrag anzuzeigen.
![Watcheraktionsaufträge über die Benutzeroberfläche](media/automation-watchers-tutorial/WatcherActionJobs.png)


## <a name="next-steps"></a>Nächste Schritte:

Weitere Informationen finden Sie unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md).









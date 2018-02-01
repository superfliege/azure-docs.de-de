---
title: Importieren von Log Analytics-Daten in Power BI | Microsoft Dokumentation
description: "Power BI ist ein cloudbasierter Business Analytics-Dienst von Microsoft, der umfassende Visualisierungen und Berichte zur Analyse verschiedener Datensätze bietet.  In diesem Artikel wird beschrieben, wie Log Analytics-Daten in Power BI importiert werden und wie die automatische Aktualisierung konfiguriert wird."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: bwren
ms.openlocfilehash: e687a1ee8ac4f565062e57b07cdfa9ac5e6bbf4f
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Importieren von Log Analytics-Daten in Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) ist ein cloudbasierter Business Analytics-Dienst von Microsoft, der umfassende Visualisierungen und Berichte zur Analyse verschiedener Datensätze bietet.  Sie können die Ergebnisse einer Log Analytics-Suche in ein Power BI-Dataset importieren, damit Sie Features wie das Durchsuchen von Daten aus verschiedenen Quellen und das Freigeben von Berichten im Web und auf mobilen Geräten nutzen können.

Dieser Artikel enthält Details zum Importieren von Log Analytics-Daten in Power BI und zum Planen der automatischen Aktualisierung.  Für einen [aktualisierten](#upgraded-workspace) Arbeitsbereich und einen [Vorgängerarbeitsbereich](#legacy-workspace) sind verschiedene Prozesse enthalten.

## <a name="upgraded-workspace"></a>Aktualisierter Arbeitsbereich


Zum Importieren von Daten aus einem [aktualisierten Log Analytics-Arbeitsbereich](log-analytics-log-search-upgrade.md) in Power BI erstellen Sie in Power BI basierend auf einer Protokollsuchabfrage in Log Analytics ein Dataset.  Die Abfrage wird bei jeder Aktualisierung des Datasets ausgeführt.  Sie können dann Power BI-Berichte erstellen, die Daten aus dem Dataset verwenden.  Zum Erstellen des Datasets in Power BI exportieren Sie Ihre Abfrage aus Log Analytics in die [Power Query-Sprache (M) ](https://msdn.microsoft.com/library/mt807488.aspx).  Diese verwenden Sie dann zum Erstellen einer Abfrage in Power BI Desktop, die Sie anschließend als Dataset in Power BI veröffentlichen.  Die Details für diesen Prozess werden nachfolgend beschrieben.

![Von Log Analytics nach Power BI](media/log-analytics-powerbi/overview.png)

### <a name="export-query"></a>Exportieren einer Abfrage
Erstellen Sie zunächst eine [Protokollsuche](log-analytics-log-search-new.md), welche die Daten von Log Analytics zurückgibt, mit denen das Power BI-Dataset aufgefüllt werden soll.  Anschließend exportieren Sie die Abfrage in der [Power Query-Sprache (M)](https://msdn.microsoft.com/library/mt807488.aspx), die von Power BI Desktop verwendet werden kann.

1. Erstellen Sie die Protokollsuche in Log Analytics zum Extrahieren der Daten für das Dataset.
2. Wenn Sie das Protokollsuchportal verwenden, klicken Sie auf **Power BI**.  Wenn Sie das Analytics-Portal verwenden, wählen Sie **exportieren** > **Power BI Query (M)**.  Mit beiden Optionen wird die Abfrage in eine Textdatei namens **PowerBIQuery.txt** exportiert. 

    ![Exportieren einer Protokollsuche](media/log-analytics-powerbi/export-logsearch.png) ![Exportieren einer Protokollsuche](media/log-analytics-powerbi/export-analytics.png)

3. Öffnen Sie die Textdatei, und kopieren Sie den Inhalt.

### <a name="import-query-into-power-bi-desktop"></a>Importieren einer Abfrage in Power BI Desktop
Power BI Desktop ist eine Desktopanwendung, die Ihnen das Erstellen von Datasets und Berichten ermöglicht, die in Power BI veröffentlicht werden können.  Sie können damit auch eine Abfrage in der aus Log Analytics exportierten Power Query-Sprache erstellen. 

1. Installieren Sie [Power BI Desktop](https://powerbi.microsoft.com/desktop/), wenn Sie es noch nicht haben, und öffnen Sie die Anwendung.
2. Wählen Sie **Daten abrufen** > **Leere Abfrage**, um eine neue Abfrage zu öffnen.  Wählen Sie dann **Erweiterter Editor**, und fügen Sie den Inhalt der exportierten Datei in die Abfrage ein. Klicken Sie auf **Fertig**.

    ![Power BI Desktop-Abfrage](media/log-analytics-powerbi/desktop-new-query.png)

5. Die Abfrage wird ausgeführt, und die Ergebnisse werden angezeigt.  Sie werden möglicherweise aufgefordert, Anmeldeinformationen zum Verbinden mit Azure einzugeben.  
6. Geben Sie einen beschreibenden Namen für die Abfrage ein.  Der Standardwert lautet **Abfrage1**. Klicken Sie auf **Schließen und übernehmen**, um das Dataset dem Bericht hinzuzufügen.

    ![Power BI Desktop-Name](media/log-analytics-powerbi/desktop-results.png)



### <a name="publish-to-power-bi"></a>Veröffentlichen in Power BI
Wenn Sie in Power BI veröffentlichen, werden ein Dataset und ein Bericht erstellt.  Wenn Sie einen Bericht in Power BI Desktop erstellen, wird dieser mit Ihren Daten veröffentlicht.  Wenn nicht, wird ein leerer Bericht erstellt.  Sie können den Bericht in Power BI ändern oder einen neuen auf Basis des Datasets erstellen.

8. Erstellen Sie einen Bericht auf Basis Ihrer Daten.  Wenn Sie mit Power BI Desktop nicht vertraut sind, lesen Sie in der [Dokumentation](https://docs.microsoft.com/power-bi/desktop-report-view) dazu nach.  Wenn bereit sind, ihn an Power BI zu senden, klicken Sie auf **Veröffentlichen**.  Wählen Sie ein Ziel in Ihrem Power BI-Konto aus, wenn Sie dazu aufgefordert werden.  Sofern Sie kein bestimmtes Ziel verwenden möchten, verwenden Sie **Mein Arbeitsbereich**.

    ![Veröffentlichen in Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Wenn die Veröffentlichung abgeschlossen ist, klicken Sie auf **In Power BI öffnen**, um Power BI mit Ihrem neuen Dataset zu öffnen.


### <a name="configure-scheduled-refresh"></a>Konfigurieren von geplanten Aktualisierungen
Das in Power BI erstellte Dataset enthält dieselben Daten, die zuvor in Power BI Desktop angezeigt wurden.  Sie müssen das Dataset in regelmäßigen Abständen aktualisieren, um die Abfrage erneut auszuführen und es mit den aktuellen Daten aus Log Analytics zu füllen.  

1. Klicken Sie auf den Arbeitsbereich, in dem Sie Ihren Bericht hochgeladen haben, und wählen Sie dann das Menü **Datasets**. Wählen Sie das Kontextmenü neben dem neuen Dataset und dann **Einstellungen** aus. Unter **Anmeldeinformationen für die Datenquelle** sollte eine Meldung angezeigt werden, dass die Anmeldeinformationen ungültig sind.  Dies liegt daran, dass Sie noch keine Anmeldeinformationen für das Dataset zum Aktualisieren seiner Daten bereitgestellt haben.  Klicken Sie auf **Anmeldeinformationen bearbeiten**, und geben Sie Anmeldeinformationen mit Zugriff auf Log Analytics an.

    ![Power BI-Zeitplan](media/log-analytics-powerbi/powerbi-schedule.png)

5. Legen Sie unter **Geplante Aktualisierung** die Option **Halten Sie Ihre Daten aktuell** fest.  Sie haben die Möglichkeit, die **Aktualisierungshäufigkeit** und eine oder mehrere bestimmte Uhrzeiten für die Ausführung der Aktualisierung zu ändern.

    ![Power BI-Aktualisierung](media/log-analytics-powerbi/powerbi-schedule-refresh.png)

## <a name="legacy-workspace"></a>Vorgängerarbeitsbereich
Wenn Sie Power BI für Log Analytics mit einem [Log Analytics-Vorgängerarbeitsbereich](log-analytics-powerbi.md) konfigurieren, können Sie Protokollabfragen erstellen, die ihre Ergebnisse in die entsprechenden Datasets in Power BI exportieren.  Der Abfrage- und Exportvorgang wird weiterhin automatisch nach einem von Ihnen definieren Zeitplan ausgeführt, damit das Dataset jederzeit mit den neuesten von Log Analytics gesammelten Daten aktualisiert wird.

![Von Log Analytics nach Power BI](media/log-analytics-powerbi/overview-legacy.png)

### <a name="power-bi-schedules"></a>Power BI-Zeitpläne
Ein *Power BI-Zeitplan* enthält eine Protokollsuche, die einen Satz von Daten aus Log Analytics in ein entsprechendes Dataset in Power BI exportiert. In einem Zeitplan ist definiert, wie häufig diese Suche ausgeführt wird, damit das Dataset aktuell bleibt.

Die Felder im Dataset entsprechen den Eigenschaften der von der Protokollsuche zurückgegebenen Datensätze.  Wenn eine Suche Datensätze verschiedener Typen zurückgibt, schließt das Dataset alle Eigenschaften aller enthalten Datensatztypen ein.  

### <a name="connecting-log-analytics-workspace-to-power-bi"></a>Verbinden des Log Analytics-Arbeitsbereichs mit Power BI
Bevor Sie Daten aus Log Analytics nach Power BI exportieren können, müssen Sie Ihren Arbeitsbereich mit Ihrem Power BI-Konto verbinden. Führen Sie dazu die folgenden Schritte aus:  

1. Klicken Sie in der OMS-Konsole auf die Kachel **Einstellungen** .
2. Wählen Sie **Konten**aus.
3. Klicken Sie im Abschnitt **Arbeitsbereichsinformationen** auf **Mit Power BI-Konto verbinden**.
4. Geben Sie die Anmeldeinformationen für Ihr Power BI-Konto ein.

### <a name="create-a-power-bi-schedule"></a>Erstellen eines Power BI-Zeitplans
Erstellen Sie einen Power BI-Zeitplan für jedes Dataset. Führen Sie dazu die folgenden Schritte aus.

1. Klicken Sie in der OMS-Konsole auf die Kachel **Protokollsuche** .
2. Geben Sie eine neue Abfrage ein, oder wählen Sie eine gespeicherte Suche aus, die die Daten zurückgibt, die Sie nach **Power BI**exportieren möchten.  
3. Klicken Sie oben auf der Seite auf die Schaltfläche **Power BI**, um das Dialogfeld **Power BI** zu öffnen.
4. Stellen Sie die Informationen aus der folgenden Tabelle bereit, und klicken Sie auf **Speichern**.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| NAME |Der Name, anhand dessen der Zeitplan in der Liste der Power BI-Zeitpläne identifiziert werden kann. |
| Gespeicherte Suche |Die auszuführende Protokollsuche.  Sie können entweder die aktuelle Abfrage auswählen oder aus dem Dropdownfeld eine vorhandene gespeicherte Suche auswählen. |
| Schedule |Die Häufigkeit der Ausführung der gespeicherten Suche und des Exports in das Power BI-Dataset.  Der Wert muss zwischen 15 Minuten und 24 Stunden liegen. |
| Datasetname |Der Name des Datasets in Power BI.  Der Name wird erstellt, wenn noch keiner vorhanden ist. Ist bereits ein Name vorhanden, wird dieser aktualisiert. |

### <a name="viewing-and-removing-power-bi-schedules"></a>Anzeigen und Entfernen von Power BI-Zeitplänen
Führen Sie die folgenden Schritte aus, um die Liste der vorhandenen Power BI-Zeitpläne anzuzeigen.

1. Klicken Sie in der OMS-Konsole auf die Kachel **Einstellungen** .
2. Wählen Sie **Power BI**aus.

Zusätzlich zu den Details zum Zeitplan werden die Anzahl der Ausführungen des Zeitplans in der letzten Woche und der Status der letzten Synchronisierung angezeigt.  Wenn bei der Synchronisierung Fehler auftreten, können Sie auf den Link klicken, um eine Protokollsuche nach Datensätzen mit Details zum Fehler auszuführen.

Sie können einen Zeitplan entfernen, indem Sie in der Spalte **Entfernen** auf das **X** klicken.  Sie können einen Zeitplan deaktivieren, indem Sie **Aus**auswählen.  Um einen Zeitplan zu ändern, müssen Sie ihn entfernen und mit den neuen Einstellungen neu erstellen.

![Power BI-Zeitpläne](media/log-analytics-powerbi/schedules.png)

### <a name="sample-walkthrough"></a>Exemplarische Vorgehensweise
Der folgende Abschnitt zeigt anhand eines Beispiels, wie Sie einen Power BI-Zeitplan erstellen und das Dataset dieses Zeitplans zum Erstellen eines einfachen Berichts verwenden.  In diesem Beispiel werden alle Leistungsdaten für eine Gruppe von Computern nach Power BI exportiert. Anschließend wird ein Liniendiagramm erstellt, um die CPU-Nutzung anzuzeigen.

#### <a name="create-log-search"></a>Erstellen einer Protokollsuche
Zunächst erstellen Sie eine Protokollsuche für die Daten, die Sie an das Dataset senden möchten.  In diesem Beispiel wird eine Abfrage verwendet, die alle Leistungsdaten für Computer zurückgibt, deren Name mit *srv*beginnt.  

![Power BI-Zeitpläne](media/log-analytics-powerbi/walkthrough-query.png)

#### <a name="create-power-bi-search"></a>Erstellen einer Power BI-Suche
Klicken Sie auf die Schaltfläche **Power BI** , um das Power BI-Dialogfeld zu öffnen und die erforderlichen Informationen bereitzustellen.  Diese Suche soll einmal pro Stunde ausgeführt werden und ein Dataset mit der Bezeichnung *Contoso Perf*erstellen.  Da die Suche, mit der die gewünschten Daten erstellt werden, bereits geöffnet ist, behalten Sie die Standardeinstellung *Aktuelle Suchabfrage verwenden* für die Option **Gespeicherte Suche**bei.

![Power BI-Suche](media/log-analytics-powerbi/walkthrough-schedule.png)

#### <a name="verify-power-bi-search"></a>Überprüfen einer Power BI-Suche
Um sicherzustellen, dass der Zeitplan ordnungsgemäß erstellt wurde, zeigen Sie auf der Kachel **Einstellungen** des OMS-Dashboards die Liste der Power BI-Suchvorgänge an.  Warten Sie einige Minuten, und aktualisieren Sie diese Ansicht solange, bis gemeldet wird, dass die Synchronisierung ausgeführt wurde.  In der Regel planen Sie eine automatische Aktualisierung des Datasets.

![Power BI-Suche](media/log-analytics-powerbi/walkthrough-schedules.png)

#### <a name="verify-the-dataset-in-power-bi"></a>Überprüfen des Datasets in Power BI
Melden Sie sich bei Ihrem Konto für [powerbi.microsoft.com](http://powerbi.microsoft.com/) an, und scrollen Sie im linken Bereich nach unten zu **Datasets** .  Sie sehen, dass das Dataset *Contoso Perf* aufgeführt wird – dies bedeutet, dass der Export erfolgreich ausgeführt wurde.

![Power BI-Dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

#### <a name="create-report-based-on-dataset"></a>Erstellen eines Berichts anhand eines Datasets
Wählen Sie das Dataset **Contoso Perf** aus, und klicken Sie rechts im Bereich **Felder** auf **Ergebnisse**, um die Felder anzuzeigen, die zu diesem Dataset gehören.  Um ein Liniendiagramm zu erstellen, das die CPU-Nutzung für jeden Computer anzeigt, führen Sie folgende Aktionen aus.

1. Wählen Sie den Visualisierungstyp „Liniendiagramm“.
2. Ziehen Sie **ObjectName** auf **Berichtsstufenfilter**, und aktivieren Sie das Kontrollkästchen **Prozessor**.
3. Ziehen Sie **CounterName** auf **Berichtsstufenfilter**, und aktivieren Sie das Kontrollkästchen **% Prozessorzeit**.
4. Ziehen Sie **CounterValue** auf **Werte**.
5. Ziehen Sie **Computer** auf **Legende**.
6. Ziehen Sie **TimeGenerated** auf **Achse**.

Wie Sie sehen, wird das resultierende Liniendiagramm mit den Daten aus dem Dataset angezeigt.

![Power BI-Liniendiagramm](media/log-analytics-powerbi/walkthrough-linegraph.png)

#### <a name="save-the-report"></a>Speichern des Berichts
Speichern Sie den Bericht, indem Sie am oberen Bildschirmrand auf die Schaltfläche „Speichern“ klicken, und überprüfen, ob er jetzt auf der linken Seite im Abschnitt „Berichte“ aufgeführt wird.

![Power BI-Berichte](media/log-analytics-powerbi/walkthrough-report.png)



## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr zu [Protokollsuchvorgängen](log-analytics-log-searches.md) zum Erstellen von Abfragen, die nach Power BI exportiert werden können.
* Erfahren Sie mehr über [Power BI](http://powerbi.microsoft.com), um Visualisierungen anhand von Log Analytics-Exporten zu erstellen.

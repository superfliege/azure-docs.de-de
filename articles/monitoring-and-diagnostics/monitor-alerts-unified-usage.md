---
title: Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor – Warnungen (Vorschauversion) | Microsoft-Dokumentation
description: Verwenden Sie die neue einheitliche Oberfläche für Azure-Warnungen, um über eine zentrale Oberfläche Metrik- und Protokollwarnungsregeln zu erstellen, anzuzeigen und zu verwalten.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 36729da3-e002-4a64-86b2-2513ca2cbb58
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: vinagara
ms.openlocfilehash: b6b6bfee5b9e9036a6d7ff17ff1a8d4de542bbd3
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="create-view-and-manage-alerts-using-azure-monitor---alerts-preview"></a>Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor – Warnungen (Vorschauversion)

## <a name="overview"></a>Übersicht
In diesem Artikel wird erläutert, wie Sie im Azure-Portal mithilfe der neuen Oberfläche „Warnungen (Vorschauversion)“ Warnungen einrichten. Die Definition einer Warnungsregel setzt sich aus drei Teilen zusammen:
- Ziel: Eine bestimmte zu überwachende Azure-Ressource
- Kriterien: Eine bestimmte Bedingung oder Logik, die beim Vorkommen in einem Signal eine Aktion auslösen soll
- Aktion: Eine bestimmte Form der Kontaktaufnahme, die an einen Empfänger einer Benachrichtigung (z.B. E-Mail, SMS oder Webhook) gesendet wird.

Bei der Oberfläche „Warnungen (Vorschauversion)“ wird die Bezeichnung **Protokollwarnungen** verwendet, um basierend auf [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) oder [Azure Application Insights](../application-insights/app-insights-analytics.md) Warnungen zu beschreiben, bei denen das Signal eine benutzerdefinierte Abfrage darstellt. Die Metrikwarnungsfunktion, die in der bereits bestehenden Warnungsoberfläche als [Metrikwarnungen nahezu in Echtzeit](monitoring-near-real-time-metric-alerts.md) bezeichnet wird, lautet bei der Oberfläche „Warnungen (Vorschauversion)“ **Metrikwarnungen**. Einige Ressourcentypen bei *Metrikwarnungen* stellen für bestimmte Azure-Ressourcen [mehrdimensionale Metriken](monitoring-metric-charts.md) bereit, weshalb Warnungen für derartige Ressourcen durch zusätzliche Filter für Dimensionen spezifischer gestaltet werden können. Diese Warnungen werden als **mehrdimensionale Metrikwarnungen** bezeichnet.
Die Oberfläche „Warnungen (Vorschauversion)“ von Azure bietet außerdem eine einheitliche Ansicht für all Ihre Warnungsregeln und die Möglichkeit, diese über eine einzige Oberfläche zentral zu verwalten. Auch nicht aufgelöste Warnungen werden angezeigt. Weitere Informationen zur Funktionalität finden Sie unter [Azure-Warnungen (Vorschauversion) – Übersicht](monitoring-overview-unified-alerts.md).

> [!NOTE]
> Die Azure-Oberfläche „Warnungen (Vorschauversion)“ bietet zwar eine neue und verbesserte Oberfläche für die Erstellung von Warnungen in Azure, aber die vorhandene Oberfläche [Azure-Warnungen](monitoring-overview-alerts.md) steht nach wie vor für die Verwendung zur Verfügung.
>

Im Folgenden wird Schritt für Schritt die Verwendung der Azure-Oberfläche „Warnungen (Vorschauversion)“ erläutert.

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Erstellen einer Warnungsregel mit dem Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com/) die Option **Monitor** und im Abschnitt „MONITOR“ dann **Warnungen (Vorschauversion)** aus.  
    ![Überwachung](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

2. Wählen Sie die Schaltfläche **Neue Warnungsregel** aus, um eine neue Warnung in Azure zu erstellen.
    ![Warnung hinzufügen](./media/monitor-alerts-unified/AlertsPreviewOption.png)

3. Der aus drei Teilen bestehende Abschnitt „Warnung erstellen“ wird angezeigt: *Warnungsbedingung definieren*, *Warnungsdetails definieren* und *Aktionsgruppe definieren*.

    ![Erstellen einer Regel](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

4.  Definieren Sie die Warnungsbedingung, indem Sie auf den Link **Ressource auswählen** klicken und dann durch Auswahl einer Ressource das Ziel angeben. Filtern Sie entsprechend, indem Sie die erforderlichen Optionen *Abonnement*, *Ressourcentyp* und schließlich *Ressource* auswählen.

    >[!NOTE]

    > Überprüfen Sie die für die ausgewählte Ressource verfügbaren Signale, bevor Sie den Vorgang fortsetzen.

    ![Auswählen einer Ressource](./media/monitor-alerts-unified/Alert-SelectResource.png)

 Da die Azure-Oberfläche „Warnungen“ (Vorschauversion) die Erstellung von verschiedenen Arten von Warnungen über eine zentrale Oberfläche ermöglicht, wählen Sie basierend auf dem gewünschten Warnungstyp den nächsten Schritt aus:

    - Führen Sie bei **Metrikwarnungen** die Schritte 5 bis 7 durch, und fahren Sie dann direkt mit Schritt 11 fort.
    - Fahren Sie bei **Protokollwarnungen** mit Schritt 8 fort.

    > [!NOTE]

    > Der Dienst für einheitliche Warnungen (Vorschauversion) unterstützt ebenfalls Aktivitätsprotokollwarnungen. [Weitere Informationen](monitoring-activity-log-alerts-new-experience.md)

5. *Metrikwarnungen*: Stellen Sie sicher, dass **Ressourcentyp** mit dem Signaltyp **Metrik** ausgewählt ist. Klicken Sie nach dem Auswählen der entsprechenden **Ressource** auf die Schaltfläche *Fertig*, um zu „Warnung erstellen“ zurückzukehren. Verwenden Sie als Nächstes die Schaltfläche **Kriterien hinzufügen**, um für die zuvor ausgewählte Ressource das jeweilige Signal aus der Liste von Signaloptionen, den entsprechenden Überwachungsdienst und den aufgeführten Typ auszuwählen.

    ![Auswählen einer Ressource](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]

    >  Alle Ressourcen, für die [Warnungen nahezu in Echtzeit](monitoring-near-real-time-metric-alerts.md) möglich sind, werden mit dem Überwachungsdienst **Plattform** und dem Signaltyp **Metrik** aufgeführt.

6. *Metrikwarnungen*: Sobald das Signal ausgewählt ist, kann die Logik für Warnungen angegeben werden. Als Referenz werden Verlaufsdaten zum Signal mit der Option angezeigt, über **Verlauf anzeigen** das Zeitfenster von den letzten sechs Stunden bis zur letzten Woche anzupassen. Ist die Visualisierung eingerichtet, kann die **Warnungslogik** aus den angezeigten Optionen „Bedingung“, „Aggregation“ und schließlich „Schwellenwert“ ausgewählt werden. Die Bedingung wird als Vorschauversion der angegebenen Logik zusammen mit dem Signalverlauf in der Visualisierung angezeigt, wodurch dargestellt wird, wann die Warnung ausgelöst wurde. Geben Sie schließlich an, für welchen Zeitraum eine Warnung auf die angegebene Bedingung prüfen soll. Legen Sie hierfür den **Zeitraum** und die **Häufigkeit** fest, wie oft eine Warnung ausgeführt werden soll.

    ![Konfigurieren der Signallogik für Metriken](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

7. *Metrikwarnungen*: Wenn es sich beim Signal um eine Metrik handelt, kann das Warnungsfenster nach verschiedenen Datenpunkten oder Dimensionen für die genannte Azure-Ressource gefiltert werden. Ähnlich wie bei Metrikwarnungen kann die Visualisierung des Signalverlaufs durch Auswahl der Dauer aus der Dropdownliste **Verlauf anzeigen** ausgewählt werden. Darüber hinaus können für die ausgewählte Metrik Dimensionen zum Filtern nach erforderlichen Zeitreihen ausgewählt werden. Die Auswahl von Dimensionen (bis zu fünf Optionen) ist optional. Die **Warnungslogik** kann aus den angezeigten Optionen „Bedingung“, „Aggregation“ und schließlich „Schwellenwert“ ausgewählt werden. Die Bedingung wird als Vorschauversion der angegebenen Logik zusammen mit dem Signalverlauf in der Visualisierung angezeigt, wodurch dargestellt wird, wann die Warnung in der Vergangenheit ausgelöst wurde. Geben Sie schließlich an, für welchen Zeitraum eine Warnung auf die angegebene Bedingung prüfen soll. Legen Sie hierfür den **Zeitraum** und die **Häufigkeit** fest, wie oft eine Warnung ausgeführt werden soll.

    ![Konfigurieren der Signallogik für mehrdimensionale Metriken](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

8. *Protokollwarnungen:* Stellen Sie sicher, dass als **Ressourcentyp** eine Analysequelle wie *Log Analytics* oder *Application Insights* und der Signaltyp **Protokoll** ausgewählt sind, und klicken Sie nach Auswahl der entsprechenden **Ressource** auf *Fertig*. Klicken Sie als Nächstes auf die Schaltfläche **Kriterien hinzufügen**, um die Liste mit den für die Ressource verfügbaren Signaloptionen und die Option **Benutzerdefinierte Protokollsuche** für den ausgewählten Protokollüberwachungsdienst (z.B. *Log Analytics* oder *Application Insights*) aus der Signalliste anzuzeigen.

   ![Auswählen einer Ressource – benutzerdefinierte Protokollsuche](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Über die Listen von „Warnungen (Vorschauversion)“ kann eine Analytics-Abfrage als Signaltyp **Log (Saved Query)** (Protokoll (gespeicherte Abfrage)) importiert werden. Dies ist in der obigen Abbildung dargestellt. Benutzer können Ihre Abfrage so in Analytics verfeinern und zur zukünftigen Nutzung in Warnungen speichern. Weitere Informationen zum Speichern von Abfragen finden Sie unter [Suchen von Daten mit Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md) oder [Was ist Log Analytics?](../log-analytics/log-analytics-overview.md). 

9.  *Protokollwarnungen*: Wenn Sie diese Option ausgewählt haben, können im Feld **Suchabfrage** Abfragen für Warnungen angegeben werden. Ist die Abfragesyntax falsch, wird eine Fehlermeldung in ROT angezeigt. Wenn die Abfragesyntax korrekt ist, werden für die angegebene Abfrage Referenzverlaufsdaten als Diagramm angezeigt. Dabei besteht die Möglichkeit, das Zeitfenster von den letzten sechs Stunden bis zur letzten Woche anzupassen.

 ![Konfigurieren einer Warnungsregel](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Die Visualisierung von Verlaufsdaten kann nur angezeigt werden, wenn die Abfrageergebnisse Details in Bezug auf die Zeit enthalten. Wenn Ihre Abfrage zusammengefasste Daten oder bestimmte Spaltenwerte zurückgibt, werden diese als einzelner Plot angezeigt.

    >  Wenn Sie als Typ für die Metrikmessung Protokollwarnungen mit Application Insights verwenden, können Sie angeben, nach welcher Variablen die Daten gruppiert werden sollen. Verwenden Sie dazu die Option **Aggregieren auf**, wie hier zu sehen:

    ![Option „Aggregieren auf“](./media/monitor-alerts-unified/aggregate-on.png)

10.  *Protokollwarnungen*: Ist die Visualisierung eingerichtet, kann die **Warnungslogik** aus den angezeigten Optionen „Bedingung“, „Aggregation“ und schließlich „Schwellenwert“ ausgewählt werden. Geben Sie schließlich über die Option **Zeitraum** den Zeitraum für die Bewertung der angegebenen Bedingung in der Logik an. Legen Sie durch Auswahl der **Häufigkeit** außerdem fest, wie oft die Warnung ausgeführt werden soll.
**Protokollwarnungen** können auf Folgendem basieren:
   - *Anzahl von Datensätzen*: Eine Warnung wird erstellt, wenn die Anzahl der von der Abfrage zurückgegebenen Datensätze entweder größer als oder kleiner als der Wert ist, den Sie angeben.
   - *Metrische Maßeinheit*: Eine Warnung wird erstellt, wenn die einzelnen *aggregierten Werte* in den Ergebnissen den angegebenen Schwellenwert überschreiten und *nach dem ausgewählten Wert gruppiert* werden. Die Anzahl von Sicherheitsverletzungen für eine Warnung ist die Anzahl der Häufigkeit, mit der im ausgewählten Zeitraum der Schwellenwert überschritten wird. Sie können „Sicherheitsverletzungen gesamt“ für eine beliebige Kombination aus Verletzungen in den Ergebnissen angeben oder „Aufeinanderfolgende Sicherheitsverletzungen“, um vorauszusetzen, dass die Sicherheitsverletzungen in aufeinanderfolgenden Stichproben auftreten müssen. Erfahren Sie mehr über [Protokollwarnungen und die zugehörigen Typen](monitor-alerts-unified-log.md).

    > [!TIP]
    > Derzeit können Protokollsuchwarnungen auf der Oberfläche „Warnungen (Vorschauversion)“ für *Zeitraum* und *Häufigkeit* einen benutzerdefinierten Wert in Minute(n) enthalten. Die Werte können sich von 5 Minuten auf 1440 Minuten, d.h. 24 Stunden, erstrecken. Wenn der Warnungszeitraum z.B. drei Stunden betragen soll, konvertieren Sie diesen vor der Verwendung in Minuten (d.h. 180 Minuten).

11. Legen Sie im zweiten Schritt einen Namen für ihre Warnung im Feld **Name der Warnungsregel** fest, und geben Sie eine **Beschreibung** mit Einzelheiten zur Warnung sowie den **Schweregrad** über die angegebenen Optionen an. Diese Details werden in allen Warnungs-E-Mails bzw. Benachrichtigungen wiederverwendet oder mithilfe von Push von Azure Monitor übertragen. Darüber hinaus kann der Benutzer durch Umschalten der Option **Regel beim Erstellen aktivieren** festlegen, dass die Warnungsregel bei der Erstellung sofort aktiviert werden soll.

    Nur bei **Protokollwarnungen** sind einige zusätzliche Funktionen in den Warnungsdetails verfügbar:

    - **Warnungen unterdrücken**: Wenn Sie die Unterdrückung für die Warnungsregel aktivieren, werden Aktionen für die Regel nach dem Erstellen einer neuen Warnung für einen vorher festgelegten Zeitraum deaktiviert. Die Regel wird weiter ausgeführt und erstellt Warnungsdatensätze, wenn die Kriterien erfüllt sind. Dies erspart Ihnen die Zeit, das Problem zu beheben, ohne doppelte Aktionen durchzuführen.

        ![Unterdrücken von Warnungen für Protokollwarnungen](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

12. Legen Sie im dritten und letzten Schritt bei Bedarf fest, dass eine **Aktionsgruppe** für die Warnungsregel ausgelöst werden muss, wenn die Warnungsbedingung erfüllt ist. Sie können eine vorhandene Aktionsgruppe mit einer Warnung auswählen oder eine neue Aktionsgruppe erstellen. Entsprechend der ausgewählten Aktionsgruppe werden beim Auslösen einer Warnung u.a. folgende Vorgänge in Azure ausgeführt: Senden von E-Mails, Senden von SMS, Aufrufen von Webhooks, Wiederherstellen mit Azure Runbooks und Übertragen von Inhalten an das ITSM-Tool mithilfe von Push. Erfahren Sie mehr über [Aktionsgruppen](monitoring-action-groups.md).

    Für **Protokollwarnungen** sind einige zusätzliche Funktionen zum Überschreiben der Standardaktionen verfügbar:

    - **E-Mail-Benachrichtigung:**: Überschreibt den *E-Mail-Betreff* in der E-Mail, die über die Aktionsgruppe gesendet wurde wenn mindestens eine E-Mail-Aktion in der genannten Aktionsgruppe vorhanden ist. Der Text der E-Mail kann nicht geändert werden, und dieses Feld ist **nicht** für E-Mail-Adressen vorgesehen.
    - **Benutzerdefinierte JSON-Nutzlast einschließen:** Diese Option überschreibt den von Aktionsgruppen verwendeten JSON-Webhook, wenn mindestens eine Webhookaktion in der genannten Aktionsgruppe vorhanden ist. Der Benutzer kann das JSON-Format angeben, das für alle in zugewiesenen Aktionsgruppen konfigurierte Webhooks verwendet werden kann. Weitere Informationen zu Webhookformaten finden Sie unter [Webhookaktionen für Protokollwarnungsregeln](monitor-alerts-unified-log-webhook.md). Mit der Option zum Testen von Webhooks können das Format und die Verarbeitung nach Ziel mithilfe von JSON-Beispielformaten überprüft werden. Diese Option ist nur zu **Testzwecken** vorgesehen.

        ![Aktionsüberschreibungen für Protokollwarnungen](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

13. Wenn alle Felder gültig sind und mit einem grünen Häkchen versehen sind, kann auf die Schaltfläche **Warnungsregel erstellen** geklickt werden, wodurch eine Warnung in Azure Monitor unter „Warnungen (Vorschauversion)“ erstellt wird. Alle Warnungen können über das Dashboard „Warnungen (Vorschauversion)“ angezeigt werden.

    ![Regelerstellung](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

    Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="view-your-alerts-in-azure-portal"></a>Anzeigen von Warnungen im Azure-Portal

1. Wählen Sie im [Portal](https://portal.azure.com/) die Option **Monitor** und im Abschnitt „MONITOR“ dann **Warnungen (Vorschauversion)** aus.  

2. Das **Dashboard „Warnungen (Vorschauversion)“** wird angezeigt, auf dem alle Azure-Warnungen vereinheitlicht und auf einem zentralen ![Warnungsdashboard](./media/monitoring-overview-unified/alerts-preview-overview.png) angezeigt werden.
3. Das Dashboard zeigt auf einem Blick folgende Elemente (von oben links nach rechts), auf die durch Klicken eine ausführliche Auflistung angezeigt wird:
    - *Ausgelöste Warnungen*: Die gegenwärtige Anzahl von Warnungen, die die Logik erfüllt haben und den Zustand „Ausgelöst“ aufweisen
    - *Warnungsregeln gesamt*: Die Anzahl der erstellten Warnungsregeln und auf niedrigster Textebene die Anzahl der derzeit aktivierten Regeln
4. Es wird eine Liste aller ausgelösten Warnungen angezeigt, auf die der Benutzer zum Anzeigen von Details klicken kann.
5. Zur Unterstützung bei der Suche nach bestimmten Warnungen können zusätzlich die Dropdownoptionen verwendet werden, um nach bestimmten Kriterien (*Abonnement, Ressourcengruppe und/oder Ressource*) zu filtern. Für nicht aufgelöste Warnungen ist zudem die Option *Warnungen filtern* verfügbar, um entsprechend des angegebenen Schlüsselworts über *Name, Warnungskriterien, Ressourcengruppe und Zielressource* nach bestimmten übereinstimmenden Warnungen zu suchen.

## <a name="managing-your-alerts-in-azure-portal"></a>Verwalten von Warnungen im Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com/) die Option **Monitor** und im Abschnitt „MONITOR“ dann **Warnungen (Vorschauversion)** aus.  
2. Wählen Sie in der oberen Leiste die Schaltfläche **Regeln verwalten** aus, um zum Abschnitt „Regelverwaltung“ zu navigieren, wobei alle erstellten Warnungsregeln sowie deaktivierte Warnungen aufgeführt sind.
3. Bei bestimmten Warnungsregeln können die Dropdownfilter im oberen Bereich verwendet werden, die die Eingrenzung von Warnungsregeln nach bestimmten Kriterien (*Abonnement, Ressourcengruppen und/oder Ressource*) ermöglichen. Neben der Verwendung des als *Warnungen filtern* gekennzeichneten Suchbereichs oberhalb der Liste der Warnungsregeln kann ein Schlüsselwort angegeben werden, das entsprechend *Warnungsname, Bedingung und Zielressource* ausschließlich Abgleichsregeln anzeigt.
   ![Verwaltung von Warnungsregeln](./media/monitoring-overview-unified/alerts-preview-rules.png)
4. Klicken Sie zum Anzeigen oder Ändern bestimmter Warnungsregeln auf den jeweiligen Namen, der als anklickbarer Link angezeigt wird.
5. Die definierte Warnung wird mit einer dreiteiligen Struktur angezeigt: 1) Warnungsbedingung, 2) Warnungsdetails, 3) Aktionsgruppe. Durch Klicken auf **Zielkriterien** können Sie die Warnungslogik ändern oder ein neues Kriterium hinzufügen, nachdem Sie die vorherige Logik über das Papierkorbsymbol gelöscht haben. Auf ähnliche Weise können im Abschnitt „Warnungsdetails“ **Beschreibung** und **Schweregrad** geändert werden. Die Aktionsgruppe kann dahingehend geändert werden, dass sie mit der Warnung verknüpft wird, oder mit der Schaltfläche **Neue Aktionsgruppe** kann eine neue Aktionsgruppe erstellt werden.

   ![Ändern einer Warnungsregel](./media/monitor-alerts-unified/AlertsPreviewEdit.png)

6. Im oberen Bereich können Änderungen an der Warnung vorgenommen werden. Hierzu gehören folgende Vorgänge: **Speichern**, um sämtliche Änderungen an der Warnung zu committen, **Verwerfen**, um ohne Committen von Änderungen an der Warnung zurückzukehren und **Deaktivieren**, um die Warnung zu deaktivieren, wonach keine Aktionen mehr ausgeführt oder ausgelöst werden. Zu guter Letzt steht auch die Option **Löschen** zur Verfügung, mit der die gesamte Warnungsregel dauerhaft aus Azure entfernt werden kann.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die neuen [Metrikwarnungen, die nahezu in Echtzeit stattfinden (Vorschau)](monitoring-near-real-time-metric-alerts.md).
- Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
- Erfahren Sie mehr über [Protokollwarnungen auf der Azure-Oberfläche „Warnungen (Vorschauversion)“](monitor-alerts-unified-log.md).
- [Erfahren Sie mehr über Aktivitätsprotokollwarnungen auf der Oberfläche „Warnungen (Vorschau)“.](monitoring-activity-log-alerts-new-experience.md)

---
title: "Planen von Aufgaben und regelmäßig ausgeführten Workflows – Azure Logic Apps | Microsoft-Dokumentation"
description: "Es wird beschrieben,wie Sie regelmäßig ausgeführte Aufgaben, Aktionen, Workflows, Prozesse und Workloads mit Logik-Apps erstellen und planen."
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 77567302c529e6e06e58534ffc9db44c9a85bdb7
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="schedule-tasks-and-workflows-that-run-regularly-with-logic-apps"></a>Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps

Zum Planen von regelmäßig ausgeführten Aufgaben, Aktionen, Workloads oder Prozessen können Sie einen Logik-App-Workflow erstellen, der mit dem [Trigger](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) **Zeitplan: Wiederholung** gestartet wird. Mit diesem Trigger können Sie ein Datum und eine Uhrzeit zum Starten der Wiederholung und einen Wiederholungszeitplan für die Durchführung von Aufgaben festlegen. Hier sind einige Beispiele angegeben:

* Get internal data (Interne Daten abrufen): Dient zum täglichen [Ausführen einer gespeicherten SQL-Prozedur](../connectors/connectors-create-api-sqlazure.md).
* Get external data (Externe Daten abrufen): Dient zum Pullen von Wetterberichten von NOAA im Abstand von 15 Minuten.
* Report data (Daten melden): Dient zum Senden einer Zusammenfassung mit allen Bestellungen der letzten Woche, die einen bestimmten Betrag übersteigen, per E-Mail.
* Process data (Daten verarbeiten): Dient zum Komprimieren der hochgeladenen Bilder des Tages an jedem Wochentag außerhalb der Spitzenzeiten.
* Clean up data (Daten bereinigen): Dient zum Löschen aller Tweets, die älter als drei Monate sind.
* Archive data (Daten archivieren): Dient zum Pushen von Rechnungen an einen Sicherungsdienst einmal pro Monat.

Dieser Trigger unterstützt viele Muster, z.B.:

* Sofortige Ausführung und Wiederholung jeweils nach *n* Sekunden, Minuten, Stunden, Tagen, Wochen oder Monaten.
* Start zu einem bestimmten Zeitpunkt und anschließende Ausführung und Wiederholung jeweils nach *n* Sekunden, Minuten, Stunden, Tagen, Wochen oder Monaten.
* Ausführung und Wiederholung einmal oder mehrmals pro Tag, z.B. 8:00 und 17:00 Uhr.
* Ausführung und Wiederholung einmal pro Woche, aber nur an bestimmten Tagen, z.B. Samstag und Sonntag.
* Ausführung und Wiederholung einmal pro Woche, aber nur an bestimmten Tagen und zu festen Uhrzeiten, z.B. Montag bis Freitag um 8:00 und 17:00 Uhr.

Wenn jeweils der Wiederholungstrigger ausgelöst wird, erstellt Logic Apps eine neue Instanz Ihres Logik-App-Workflows und führt sie aus.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Sie können sich aber auch [für ein Abonnement mit nutzungsbasierter Bezahlung registrieren](https://azure.microsoft.com/pricing/purchase-options/).

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/logic-apps-create-a-logic-app.md) 

## <a name="add-a-recurrence-trigger-to-your-logic-app"></a>Hinzufügen eines Wiederholungstriggers zu Ihrer Logik-App

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an. Erstellen Sie eine leere Logik-App, oder informieren Sie sich zuerst über das [Erstellen einer leeren Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

2. Geben Sie im Suchfeld „Wiederholung“ als Filter ein, wenn der Designer für Logik-Apps angezeigt wird. Wählen Sie den Trigger **Zeitplan: Wiederholung**. 

   ![Trigger „Zeitplan: Wiederholung“](./media/connectors-native-recurrence/add-recurrence-trigger.png)

   Dieser Trigger ist jetzt der erste Schritt in Ihrer Logik-App.

3. Legen Sie das Intervall und die Häufigkeit für die Wiederholung fest. Legen Sie in diesem Beispiel diese Eigenschaften fest, um Ihren Workflow jede Woche auszuführen. 

   ![Festlegen von Intervall und Häufigkeit](./media/connectors-native-recurrence/recurrence-trigger-details.png)

4. Wählen Sie **Erweiterte Optionen anzeigen**, um mehr Planungsoptionen anzuzeigen. 

   ![Weitere Optionen](./media/connectors-native-recurrence/recurrence-trigger-more-options.png)

5. Sie können jetzt diese Optionen festlegen: 

   * Legen Sie Startdatum und -uhrzeit für die Auslösung des Triggers fest. 
   Wenn Sie ein Startdatum und die dazugehörige Uhrzeit angeben, können Sie auch eine Zeitzone anwenden. 

   * Wenn Sie für die Häufigkeit „Tag“ oder „Woche“ auswählen, können Sie bestimmte Uhrzeiten für die Wiederholung wählen. 

   * Bei Auswahl von „Woche“ können Sie auch bestimmte Tage der Woche angeben.
   
   ![Erweiterte Planungsoptionen](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   Angenommen, heute ist Montag, der 4. September 2017. 
   Der folgende Wiederholungstrigger wird *frühestens* zum festgelegten Startdatum und -zeitpunkt ausgelöst, also am Montag, den 18. September 2017, um 8:00 Uhr (PST). 
   Der Wiederholungszeitplan ist aber für die Uhrzeiten 10:30, 12:30 und 14:30 ausschließlich an Montagen festgelegt. Also wird um 10:30 Uhr zum ersten Mal der Trigger ausgelöst und eine Instanz des Logik-App-Workflows erstellt. 
   Weitere Informationen zur Funktionsweise von Startzeiten finden Sie in diesen [Beispielen zu Startzeiten](#start-time).
   Die zukünftigen Ausführungen erfolgen um 12:30 und 14:30 Uhr an demselben Tag. 
   Für jede Wiederholung wird eine eigene Workflowinstanz erstellt. Anschließend wird der gesamte Zeitplan am nächsten Montag wiederholt. 
   [*Welche anderen Beispiele für Wiederholungen gibt es?*](#example-recurrences)

   ![Beispiel für erweiterte Planung](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > Für den Trigger wird nur dann eine Vorschau für Ihre angegebene Wiederholung angezeigt, wenn Sie als Häufigkeit „Tag“ oder „Woche“ wählen.
   
6. Erstellen Sie Ihren restlichen Workflow nun mit Aktionen oder Anweisungen zur Flusssteuerung. Weitere Aktionen, die Sie hinzufügen können, finden Sie in der [Liste mit den Connectors](../connectors/apis-list.md). 

## <a name="trigger-details"></a>Triggerdetails

Sie können diese Eigenschaften für den Wiederholungstrigger konfigurieren.

| Name | Erforderlich | Eigenschaftenname | Typ | Beschreibung | 
|----- | -------- | ------------- | ---- | ----------- | 
| **Frequency** | Ja | frequency | String | Die Zeiteinheit für die Wiederholung: **Second** (Sekunde), **Minute** (Minute), **Hour** (Stunde), **Day** (Tag), **Week** (Woche) oder **Month** (Monat) | 
| **Intervall** | Ja | interval | Integer | Eine positive ganze Zahl, die beschreibt, wie oft der Workflow basierend auf der Häufigkeit ausgeführt wird. <p>Das Standardintervall beträgt 1. Zulässige Mindest- und Maximalintervalle: <p>- Month: 1 - 16 Monate </br>- Day: 1 - 500 Tage </br>- Hour: 1 - 12.000 Stunden </br>- Minute: 1 - 72.000 Minuten </br>- Second: 1 - 9.999.999 Sekunden<p>Wenn das Intervall also beispielsweise auf „6“ und die Häufigkeit auf „Month“ festgelegt ist, erfolgt die Wiederholung alle sechs Monate. | 
| **Zeitzone** | Nein | timeZone | String | Nur relevant, wenn Sie eine Startzeit angeben, da dieser Trigger keine [UTC-Abweichung](https://en.wikipedia.org/wiki/UTC_offset) akzeptiert. Wählen Sie die anzuwendende Zeitzone aus. | 
| **Startzeit** | Nein | startTime | String | Geben Sie eine Startzeit im folgenden Format ein: <p>JJJJ-MM-TTThh:mm:ss (bei Auswahl einer Zeitzone) <p>Oder <p>JJJJ-MM-TTThh:mm:ssZ (wenn keine Zeitzone ausgewählt wird) <p>Für „18. September 2017, 14:00 Uhr“ müssten Sie also beispielsweise „2017-09-18T14:00:00“ und eine Zeitzone (z.B. „Pacific Time“) auswählen. Alternativ können Sie „2017-09-18T14:00:00Z“ ohne Zeitzone angeben. <p>**Hinweis:** Diese Startzeit muss dem [ISO 8601-Format für Datums-/Uhrzeitangaben](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) entsprechen und im [UTC-Datums-/Zeitformat](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) angegeben werden, aber ohne [UTC-Abweichung](https://en.wikipedia.org/wiki/UTC_offset). Wenn Sie keine Zeitzone auswählen, müssen Sie den Buchstaben „Z“ ohne Leerzeichen anhängen. „Z“ bezieht sich auf die entsprechende [nautische Zeit](https://en.wikipedia.org/wiki/Nautical_time). <p>Bei einfachen Zeitplänen ist die Startzeit das erste Vorkommen. Bei komplexeren Zeitplänen wird der Trigger nicht vor der Startzeit ausgelöst. [*Wie kann ich Startdatum und -uhrzeit verwenden?*](#start-time) | 
| **An diesen Tagen** | Nein | weekDays | Zeichenfolge oder Zeichenfolgenarray | Bei Auswahl von „Week“ (Woche) können Sie einen oder mehrere Tage angeben, an denen der Workflow ausgeführt werden soll: **Monday** (Montag), **Tuesday** (Dienstag), **Wednesday** (Mittwoch), **Thursday** (Donnerstag), **Friday** (Freitag), **Saturday** (Samstag) und **Sunday** (Sonntag) | 
| **Zu diesen Stunden** | Nein | hours | Ganze Zahl oder Ganzzahlarray | Bei Auswahl von „Day“ (Tag) oder „Week“ (Woche) können Sie eine oder mehrere ganze Zahlen zwischen 0 und 23 als Stunden des Tages angeben, zu denen der Workflow ausgeführt werden soll. <p>Wenn Sie also etwa „10“, „12“ und „14“ angeben, erhalten Sie die vollen Stunden „10 Uhr“, „12 Uhr“ und „14 Uhr“. | 
| **Zu diesen Minuten** | Nein | minutes | Ganze Zahl oder Ganzzahlarray | Bei Auswahl von „Day“ (Tag) oder „Week“ (Woche) können Sie eine oder mehrere ganze Zahlen zwischen 0 und 59 als Minuten der Stunden angeben, in denen der Workflow ausgeführt werden soll. <p>Wenn Sie also beispielsweise „30“ als Minutenwert angeben und das vorherige Beispiel für Stunden des Tages verwenden, erhalten Sie „10:30 Uhr“, „12:30 Uhr“ und „14:30 Uhr“. | 
||||| 

## <a name="json-example"></a>JSON-Beispiel

Hier ist ein Beispiel für die Definition eines Wiederholungstriggers angegeben:

``` json
{
    "triggers": {
        "Recurrence": {
            "type": "Recurrence",
            "recurrence": {
                "frequency": "Week",
                "interval": 1,
                "schedule": {
                    "hours": [
                        10,
                        12,
                        14
                    ],
                    "minutes": [
                        30
                    ],
                    "weekDays": [
                        "Monday"
                    ]
                },
               "startTime": "2017-09-07T14:00:00",
               "timeZone": "Pacific Standard Time"
            }
        }
    }
}
```

## <a name="faq"></a>Häufig gestellte Fragen

<a name="example-recurrences"></a>

**F:** Welche anderen Beispiele für Wiederholungszeitpläne gibt es? </br>
**A:** Hier sind weitere Beispiele angegeben:

| Serie | Intervall | Frequency | Startzeit | An diesen Tagen | Zu diesen Stunden | Zu diesen Minuten | Hinweis |
| ---------- | -------- | --------- | ---------- | ------------- | -------------- | ---------------- | ---- |
| Ausführung alle 15 Minuten (ohne Startdatum und -uhrzeit) | 15 | Minute | {keine} | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird sofort gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. | 
| Ausführung alle 15 Minuten (mit Startdatum und -uhrzeit) | 15 | Minute | *startDate*T*startTime*Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. | 
| Ausführung einmal pro Stunde zur vollen Stunde (mit Startdatum und -uhrzeit) | 1 | Hour | *startDate*Thh:00:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet. Zukünftige Wiederholungen werden einmal pro Stunde zur vollen Stunde („00“) ausgeführt. <p>Wenn für die Häufigkeit „Week“ (Woche) oder „Month“ (Monat) ausgewählt wurde, wird der Zeitplan nur an einem Tag der Woche bzw. einem Tag des Monats ausgeführt. | 
| Ausführung täglich einmal pro Stunde (ohne Startdatum und -uhrzeit) | 1 | Hour | {keine} | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird sofort gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. <p>Wenn für die Häufigkeit „Week“ (Woche) oder „Month“ (Monat) ausgewählt wurde, wird der Zeitplan nur an einem Tag der Woche bzw. einem Tag des Monats ausgeführt. | 
| Ausführung täglich einmal pro Stunde (mit Startdatum und -uhrzeit) | 1 | Hour | *startDate*T*startTime*Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. <p>Wenn für die Häufigkeit „Week“ (Woche) oder „Month“ (Monat) ausgewählt wurde, wird der Zeitplan nur an einem Tag der Woche bzw. einem Tag des Monats ausgeführt. | 
| Ausführung einmal pro Stunde jeweils 15 Minuten nach der vollen Stunde (mit Startdatum und -uhrzeit) | 1 | Hour | *startDate*T00:15:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet und jeweils um 00:15, 1:15, 2:15 usw. ausgeführt. | 
| Ausführung einmal pro Stunde jeweils 15 Minuten nach der vollen Stunde (ohne Startdatum und -uhrzeit) | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Dieser Zeitplan wird um 00:15, 1:15, 2:15 usw. ausgeführt. Außerdem entspricht dieser Zeitplan der Häufigkeit „Hour“ (Stunde) mit einer Startuhrzeit von „15“ Minuten. | 
| Ausführung alle 15 Minuten zur 15-Minuten-Marke (ohne Startdatum und -uhrzeit) | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dieser Zeitplan wird erst gestartet, wenn die nächste angegebene 15-Minuten-Marke erreicht ist. | 
| Ausführung täglich um 8:00 Uhr (ohne Startdatum und -uhrzeit) | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8 | {keine} | Dieser Zeitplan wird täglich um 8:00 Uhr basierend auf dem angegebenen Zeitplan ausgeführt. | 
| Ausführung täglich um 8:00 Uhr (mit Startdatum und -uhrzeit) | 1 | Day (Tag) | *startDate*T08:00:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird täglich um 8:00 Uhr basierend auf der angegebenen Startuhrzeit ausgeführt. | 
| Ausführung täglich um 8:30 Uhr (ohne Startdatum und -uhrzeit) | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8 | 30 | Dieser Zeitplan wird täglich um 8:30 Uhr basierend auf dem angegebenen Zeitplan ausgeführt. | 
| Ausführung täglich um 8:30 Uhr (mit Startdatum und -uhrzeit) | 1 | Day (Tag) | *startDate*T08:30:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird am angegebenen Startdatum um 8:30 Uhr gestartet. | 
| Ausführung täglich um 8:30 und 16:30 Uhr | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8, 16 | 30 | | 
| Ausführung täglich um 8:30 Uhr, 8:45 Uhr, 16:30 Uhr und 16:45 Uhr | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8, 16 | 30, 45 | | 
| Ausführung jeden Samstag um 17:00 Uhr (ohne Startdatum und -uhrzeit) | 1 | Woche | {keine} | „Saturday“ (Samstag) | 17 | 00 | Dieser Zeitplan wird jeden Samstag um 17:00 Uhr ausgeführt. | 
| Ausführung jeden Samstag um 17:00 Uhr (mit Startdatum und -uhrzeit) | 1 | Woche | *startDate*T17:00:00Z | „Saturday“ (Samstag) | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet, also hier am 9. September 2017 um 17:00 Uhr. Zukünftige Wiederholungen werden jeden Samstag um 17:00 Uhr ausgeführt. | 
| Ausführung jeden Dienstag und Donnerstag um 17:00 Uhr | 1 | Woche | {keine} | „Tuesday“ (Dienstag), „Thursday“ (Donnerstag) | 17 | {keine} | Dieser Zeitplan wird jeden Dienstag und Donnerstag um 17:00 Uhr ausgeführt. | 
| Ausführung einmal pro Stunde während der Geschäftszeiten | 1 | Woche | {keine} | Wählen Sie alle Tage mit Ausnahme von Samstag und Sonntag aus. | Wählen Sie die gewünschten Stunden des Tages aus. | Wählen Sie die gewünschten Minuten der Stunde aus. | Wenn Ihre Geschäftszeiten beispielsweise den Zeitraum von 8:00 bis 17:00 Uhr umfassen, wählen Sie „8, 9, 10, 11, 12, 13, 14, 15, 16, 17“ als Stunden des Tages aus. <p>Wählen Sie bei einer Geschäftszeit von 8:30 bis 17:30 Uhr die obigen Stunden des Tages sowie „30“ als Minuten der Stunde aus. | 
| Ausführung einmal pro Tag an Wochenenden | 1 | Woche | {keine} | „Saturday“ (Samstag), „Sunday“ (Sonntag) | Wählen Sie die gewünschten Stunden des Tages aus. | Wählen Sie wie gewünscht die Minuten der Stunde aus. | Dieser Zeitplan wird jeden Samstag und Sonntag basierend auf dem angegebenen Zeitplan ausgeführt. | 
| Ausführung alle zwei Wochen nur am Montag im Abstand von 15 Minuten | 2 | Woche | {keine} | „Monday“ (Montag) | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dieser Zeitplan wird jeden zweiten Montag jeweils zur 15-Minuten-Marke ausgeführt. | 
| Ausführung jede Stunde an einem Tag des Monats | 1 | Month (Monat) | {siehe Hinweis} | {nicht verfügbar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {siehe Hinweis} | Wenn Sie Startdatum und -uhrzeit nicht angeben, werden in diesem Zeitplan das Datum und die Uhrzeit der Erstellung verwendet. Geben Sie die Minuten der Stunde oder eine Startuhrzeit an, oder verwenden Sie die Uhrzeit der Erstellung, um die Minuten für den Zeitplan der Wiederholung zu steuern. Wenn die Startuhrzeit oder die Uhrzeit der Erstellung beispielsweise 8:25 Uhr lautet, wird dieser Zeitplan um 8:25, 9:25, 10:25 usw. ausgeführt. | 
||||||||| 

<a name="start-time"></a>

**F:** Wie kann ich Startdatum und -uhrzeit verwenden? </br>
**A:** Hier wird anhand einiger Muster verdeutlicht, wie Sie die Wiederholung mit dem Startdatum und der Startuhrzeit steuern können und wie das Logic Apps-Modul diese Wiederholungen ausführt:

| Startzeit | Wiederholung ohne Zeitplan | Wiederholung mit Zeitplan | 
| ---------- | --------------------------- | ------------------------ | 
| {keine} | Die erste Workload wird sofort ausgeführt. <p>Zukünftige Workloads werden basierend auf der letzten Ausführungszeit ausgeführt. | Die erste Workload wird sofort ausgeführt. <p>Zukünftige Workloads werden basierend auf dem angegebenen Zeitplan ausgeführt. | 
| Startuhrzeit liegt in der Vergangenheit | Die Ausführungszeiten werden basierend auf der angegebenen Startuhrzeit berechnet, und vergangene Ausführungszeiten werden verworfen. Die nächste Workload wird zum nächsten zukünftigen Ausführungszeitpunkt ausgeführt. <p>Zukünftige Workloads werden basierend auf Berechnungen der letzten Ausführungszeit ausgeführt. <p>Eine ausführlichere Erläuterung finden Sie im Beispiel im Anschluss an diese Tabelle. | Die erste Workload wird *frühestens* zur Startuhrzeit basierend auf dem Zeitplan ausgeführt, der anhand der Startuhrzeit berechnet wurde. <p>Zukünftige Workloads werden basierend auf dem angegebenen Zeitplan ausgeführt. <p>**Hinweis:** Wenn Sie eine Wiederholung mit einem Zeitplan angeben, aber keine Stunden oder Minuten für den Zeitplan, werden zukünftige Ausführungszeiten anhand der Stunden bzw. Minuten der ersten Ausführungszeit berechnet. | 
| Startuhrzeit liegt in Gegenwart oder Zukunft | Die erste Workload wird zur angegebenen Startuhrzeit ausgeführt. <p>Zukünftige Workloads werden basierend auf Berechnungen der letzten Ausführungszeit ausgeführt. | Die erste Workload wird *frühestens* zur Startuhrzeit basierend auf dem Zeitplan ausgeführt, der anhand der Startuhrzeit berechnet wurde. <p>Zukünftige Workloads werden basierend auf dem angegebenen Zeitplan ausgeführt. <p>**Hinweis:** Wenn Sie eine Wiederholung mit einem Zeitplan angeben, aber keine Stunden oder Minuten für den Zeitplan, werden zukünftige Ausführungszeiten anhand der Stunden bzw. Minuten der ersten Ausführungszeit berechnet. | 
||||

**Beispiel für eine Startuhrzeit in der Vergangenheit mit Wiederholung und ohne Zeitplan** 

| Startzeit | Die aktuelle Zeit | Serie | Zeitplan |
| ---------- | ------------ | ---------- | -------- | 
| 2017-09-**07**T14:00:00Z | 2017-09-**08**T13:00:00Z | alle 2 Tage | {keine} | 
||||| 

In diesem Szenario berechnet das Logic Apps-Modul die Ausführungszeiten basierend auf der Startuhrzeit, verwirft vergangene Ausführungszeiten und verwendet die nächste in der Zukunft liegende Startuhrzeit für die erste Ausführung. Nach dieser ersten Ausführung basieren zukünftige Ausführungen auf dem Zeitplan, der anhand der Startuhrzeit berechnet wird. Diese Wiederholung sieht wie folgt aus:

| Startzeit | Erste Ausführungszeit | Zukünftige Ausführungszeiten | 
| ---------- | ------------ | ---------- | 
| 2017-09-**07** um 14:00 Uhr | 2017-09-**09** um 14:00 Uhr | 2017-09-**11** um 14:00 Uhr </br>2017-09-**13** um 14:00 Uhr </br>2017-09-**15** um 14:00 Uhr </br>usw.
||||| 

Für dieses Szenario gilt also Folgendes: Ihre erste Ausführungszeit ist unabhängig davon identisch, wie weit zurückliegend Sie die Startuhrzeit angegeben haben, z.B. 2017-09-**05** um 14:00 Uhr oder 2017-09-**01** um 14:00 Uhr.

## <a name="next-steps"></a>Nächste Schritte

* [Workflowaktionen und -trigger](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger)
* [Connectors](../connectors/apis-list.md)

---
title: Azure Application Insights-Nutzungskohorten | Microsoft-Dokumentation
description: Analysieren unterschiedlicher Gruppen von Benutzern, Sitzungen, Ereignissen oder Vorgängen, die eine Gemeinsamkeit aufweisen
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/10/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: 68453499cc6477cc079a342906614e6873938fc8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="application-insights-cohorts"></a>Application Insights-Kohorten

Eine Kohorte ist eine Gruppe von Benutzern, Sitzungen, Ereignissen oder Vorgängen, die eine Gemeinsamkeit aufweisen. In Azure Application Insights werden Kohorten durch eine Analytics-Abfrage definiert. Wenn Sie eine bestimmte Gruppe von Benutzern oder Ereignissen wiederholt analysieren, haben Sie mit Kohorten noch mehr Flexibilität, um genau die gewünschte Gruppe anzugeben.

![Bereich „Kohorten“](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Kohorten im Vergleich zu einfachen Filtern

Kohorten werden zwar auf ähnliche Weise wie Filter verwendet, lassen sich jedoch deutlich besser anpassen und sind komplexer, da die Definition einer Kohorte über benutzerdefinierte Analytics-Abfragen erstellt wird. Im Unterschied zu Filtern können Sie Kohorten speichern, sodass andere Mitglieder Ihres Teams sie wiederverwenden können.

Sie können beispielsweise eine Kohorte von Benutzern definieren, die alle ein neues Feature in Ihrer App getestet haben. Wenn Sie diese Kohorte in Ihrer Application Insights-Ressource speichern, kann diese spezifische Gruppe von Benutzern zukünftig mit nur einem Mausklick analysiert werden.

> [!NOTE]
> Nach der Erstellung sind Kohorten in den Tools für Benutzer, Sitzungen, Ereignisse und Benutzerabläufe verfügbar.

## <a name="example-engaged-users"></a>Beispiel: aktive Benutzer

Ihr Team definiert jede Person als „aktiven Benutzer“, die Ihre App in einem bestimmten Monat mindestens fünfmal verwendet. Wir definieren nun eine Kohorte dieser aktiven Benutzer.

1. Öffnen Sie das Tool **Kohorten**.

2. Klicken Sie auf die Registerkarte **Vorlagenkatalog**. Hier finden Sie eine Sammlung von Vorlagen für verschiedene Kohorten.

3. Wählen Sie **Engaged Users** – by Days Used“ (Aktive Benutzer – nach Verwendungstagen) aus.

    Für diese Kohorte sind drei Parameter verfügbar:
      * **Activities:** Hier können Sie auswählen, welche Ereignisse und Seitenansichten als „Verwendung“ gezählt werden sollen.
      * **Period:** die Definition eines Monats.
      * **UsedAtleastCustom:** die Häufigkeit, mit der Benutzer ein Objekt innerhalb eines bestimmten Zeitraums verwenden müssen, um als aktiver Benutzer zu gelten.

4. Ändern Sie **UsedAtleastCustom** in „5+ days“ (5+ Tage), und behalten Sie unter **Period** (Zeitraum) den Standardwert von 28 Tagen bei.

    ![Image](.\media\app-insights-usage-cohorts\003.png)

    Diese Kohorte stellt nun alle Benutzer-IDs dar, die mit einem benutzerdefinierten Ereignis oder einer benutzerdefinierten Seitenansicht an fünf verschiedenen Tagen in den letzten 28 Tagen gesendet wurden.

5. Klicken Sie auf **Speichern**.

   > [!TIP]
   >  Geben Sie der Kohorte einen Namen, z.B. „Engaged Users (5+ Days)“, und speichern Sie sie unter „Meine Berichte“ oder „Freigegebene Berichte“, je nachdem, ob Sie festlegen möchten, dass andere Personen mit Zugriff auf diese Application Insights-Ressource diese Kohorte anzeigen können sollen.

6. Klicken Sie auf **Back to Gallery** (Zurück zum Katalog).

### <a name="what-can-you-do-with-this-cohort"></a>Wozu können Sie diese Kohorte nutzen?

Öffnen Sie das Tool **Benutzer**. Wählen Sie in der Dropdownliste **Anzeigen** die Kohorte aus, die Sie unter **Users who belong to…** (Benutzer mit Zugehörigkeit zu…) aus.

Das Tool „Benutzer“ wird nach dieser Kohorte von Benutzern gefiltert:

![Bereich „Benutzer“, gefiltert nach einer bestimmten Kohorte](.\media\app-insights-usage-cohorts\004.png)

Folgende wichtige Punkte sind zu berücksichtigen:
   * Hierbei handelt es sich um eine Gruppe, die Sie mit normalen Filtern nicht erstellen könnten. Die Datenzugriffslogik ist komplexer.
   * Sie können diese Kohorte mit den normalen Filtern im Tool „Benutzer“ weiter filtern. Auch wenn die Kohorte mit Zeitfenstern von 28 Tagen definiert ist, können Sie den Zeitbereich im Tool „Benutzer“ dennoch auf 30, 60 oder 90 Tage anpassen. 

So können Sie komplexere Fragen aufwerfen, z.B. _Wie verhielten sich die Personen, die in den letzten 28 Tagen aktiv waren, in den letzten 60 Tagen?_, die über den Abfrage-Generator sonst nicht möglich wären.

## <a name="example-events-cohort"></a>Beispiel: Ereigniskohorte

Sie können auch Kohorten von Ereignissen erstellen. Wir definieren eine Kohorte der Ereignisse und Seitenansichten, und verwenden sie dann in den anderen Tools. Dies kann nützlich sein, um eine Gruppe von Ereignissen zu definieren, die Ihr Team der _aktiven Verwendung_ zuordnet oder die mit einem bestimmten neuen Feature verbunden sind.

1. Öffnen Sie das Tool **Kohorten**.

2. Klicken Sie auf die Registerkarte **Vorlagenkatalog**. Hier finden Sie eine Sammlung von Vorlagen für verschiedene Kohorten.

3. Wählen Sie **Events picker** (Ereignisauswahl) aus.

    ![Screenshot von „Events picker“ (Ereignisauswahl)](.\media\app-insights-usage-cohorts\006.png)

4. Wählen Sie in der Dropdownliste **Activities** die Ereignisse aus, die in der Kohorte enthalten sein sollen.

5. Geben Sie der Kohorte einen Namen, und speichern Sie sie.

## <a name="example-active-users-where-you-modify-query"></a>Beispiel: Aktive Benutzer, bei denen Sie die Abfrage ändern

Die beiden vorherigen Kohorten wurden mithilfe von Dropdownlisten definiert. Zudem können Kohorten auch vollkommen flexibel mit Analytics-Abfragen definiert werden. Zur Veranschaulichung erstellen wir eine Kohorte von Benutzern aus Großbritannien.

![Animierte Grafik zur Verwendung des Tools „Kohorten“](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Öffnen Sie das Tool **Kohorten**. Klicken Sie auf die Registerkarte **Vorlagenkatalog**. Wählen Sie **Leere Kohorte für Benutzer** aus.

    ![Leere Kohorte für Benutzer](.\media\app-insights-usage-cohorts\001.png)

    Drei Abschnitte stehen zur Verfügung:
       * Ein Abschnitt für Markdowntext, in dem Sie die Kohorte detailliert für andere Teammitglieder beschreiben können.

       * Ein Abschnitt für Parameter, in dem Sie benutzerdefinierte Parameter festlegen können, z.B. **Activities** und andere Dropdownlisten aus den beiden vorherigen Beispielen.

       * Ein Abschnitt für Abfragen, in dem Sie die Kohorte mit einer Analytics-Abfrage definieren.

    Im Abschnitt für Abfragen [schreiben Sie eine Analytics-Abfrage](https://docs.loganalytics.io/index), mit der genau die Gruppe von Zeilen ausgewählt wird, die die definierte Kohorte beschreiben. Im Tool „Kohorten“ wird dann der Abfrage implizit die Klausel „| summarize by user_Id“ hinzugefügt. Dies wird als Vorschau in einer Tabelle unterhalb der Abfrage angezeigt, sodass Sie überprüfen können, ob die Abfrage Ergebnisse zurückgibt.

    > [!NOTE]
    > Wenn die Abfrage nicht zu sehen ist, vergrößern Sie den Abschnitt, sodass die Abfrage angezeigt wird. In der animierten GIF-Datei am Anfang dieses Abschnitts wird das Verhalten bei der Größenanpassung veranschaulicht.

2. Kopieren Sie Folgendes, und fügen Sie es in den Abfrage-Editor ein:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Klicken Sie auf **Run Query** (Abfrage ausführen). In der Tabelle sollten Benutzer-IDs angezeigt werden. Wenn dies nicht der Fall ist, wählen Sie ein Land aus, in dem Benutzer Ihre Anwendung verwenden.

4. Geben Sie der Kohorte einen Namen, und speichern Sie sie.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

_Ich habe eine Kohorte von Benutzern aus einem bestimmten Land definiert. Wenn ich diese Kohorte im Tool „Benutzer“ mit dem Festlegen eines Filters für dieses Land im Tool „Benutzer“ vergleiche, werden unterschiedliche Ergebnisse angezeigt. Warum?_

Kohorten und Filter unterscheiden sich. Angenommen, Sie haben eine Kohorte von Benutzern aus Großbritannien definiert (wie im Beispiel oben), und Sie vergleichen die Ergebnisse mit dem Festlegen des Filters „Country or region = United Kingdom“.

* Für die Kohorte werden alle Ereignisse von Benutzern angezeigt, die im aktuellen Zeitbereich mindestens ein Ereignis aus Großbritannien gesendet haben. Bei der Aufteilung nach Land oder Region werden wahrscheinlich viele Länder und Regionen angezeigt.
* Mit dem Filter werden lediglich Ereignisse aus Großbritannien angezeigt. Selbst bei der Aufteilung nach Land oder Region wird ausschließlich Großbritannien angezeigt.

## <a name="learn-more"></a>Weitere Informationen
- [Analytics-Abfragesprache](https://go.microsoft.com/fwlink/?linkid=856587)
- [Benutzer, Sitzungen, Ereignisse](app-insights-usage-segmentation.md)
- [Benutzerabläufe](app-insights-usage-flows.md)
- [Nutzungsübersicht](app-insights-usage-overview.md)
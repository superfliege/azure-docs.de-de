---
title: Azure Application Insights-Transaktionsdiagnose | Microsoft-Dokumentation
description: Application Insights-End-to-End-Transaktionsdiagnose
services: application-insights
documentationcenter: .net
author: SoubhagyaDash
manager: victormu
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2018
ms.author: sdash
ms.openlocfilehash: 8c1d8600b7f4aaa1e95f4acfbbdd55fdbfebb8fb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Einheitliche komponentenübergreifende Transaktionsdiagnose

*Diese Benutzeroberfläche befindet sich aktuell im Vorschaustadium und ersetzt die vorhandenen Diagnoseblätter für serverseitige Anforderungen, Abhängigkeiten und Ausnahmen.*

Mit der Vorschauversion wird eine neue, einheitliche Diagnoseoberfläche eingeführt, die serverseitige Telemetrie übergreifend von allen Ihren mithilfe von Application Insights überwachten Komponenten automatisch in einer einzelnen Ansicht in Beziehung setzt. Es spielt keine Rolle, ob Sie über mehrere Ressourcen mit separaten Instrumentierungsschlüsseln verfügen – Application Insights erkennt die zugrundeliegende Beziehung und ermöglicht Ihnen die komfortable Diagnose der Anwendungskomponente, Abhängigkeit oder Ausnahme, die zur langsamen Ausführung oder einem Fehler bei einer Transaktion geführt hat.

## <a name="what-does-component-mean-in-the-context-of-application-insights"></a>Was bedeutet „Komponente“ im Kontext von Application Insights?

Komponenten sind unabhängig bereitstellbare Teile Ihrer verteilten Anwendung/Microserviceanwendung. Entwickler und Betriebsteams verfügen über Sichtbarkeit oder Zugriff für die von diesen Anwendungskomponenten generierte Telemetrie auf Codeebene.

* Komponenten unterscheiden sich von „beobachteten“ externen Abhängigkeiten, wie SQL, EventHub usw., auf die Ihr Team/Ihre Organisation möglicherweise keinen Zugriff hat (Code oder Telemetrie).
* Komponenten können in einer beliebigen Anzahl von Server-/Rollen-/Containerinstanzen ausgeführt werden.
* Komponenten können separate Application Insights-Instrumentierungsschlüssel (sogar in verschiedenen Abonnements) oder verschiedene Rollen aufweisen, die alle an einen einzelnen Application Insights-Instrumentierungsschlüssel berichten. Auf der neuen Benutzeroberfläche werden Details übergreifend über alle Komponenten angezeigt, unabhängig von deren Einrichtung.

> [!Tip]
> Um optimale Ergebnisse zu erzielen, achten Sie darauf, dass alle Komponenten mit den neuesten stabilen Application Insights-SDKs instrumentiert sind. Wenn es sich um verschiedene Application Insights-Ressourcen handelt, sorgen Sie für passende Berechtigungen zum Anzeigen ihrer Telemetrie.

## <a name="enable-and-access"></a>Aktivieren und Zugriff
Aktivieren Sie in der [Vorschauliste](app-insights-previews.md) "Unified details: E2E Transaction Diagnostics" (Einheitliche Details: E2E-Transaktionsdiagnose)

![Aktivieren der Vorschau](media/app-insights-e2eTxn-diagnostics/previews.png)

Diese Vorschau ist aktuell für serverseitige Anforderungen, Abhängigkeiten und Ausnahmen verfügbar. Sie können aus den Selektierungsoberflächen **Suchergebnisse**, **Leistung** oder **Fehler** auf die neue Benutzeroberfläche zugreifen. Die Vorschauversion ersetzt die entsprechenden klassischen Detailblätter.

![Leistungsstichproben](media/app-insights-e2eTxn-diagnostics/performanceSamplesClickThrough.png)

## <a name="transaction-diagnostics-experience"></a>Oberfläche zur Transaktionsdiagnose
Diese Ansicht ist in drei Hauptteile gegliedert: ein komponentenübergreifendes Transaktionsdiagramm, eine Liste der Zeitabfolge für die gesamte Telemetrie aus einem bestimmten Komponentenvorgang und den Detailbereich für ein bestimmtes auf der linken Seite ausgewähltes Telemetrieelement.

![Hauptbestandteile](media/app-insights-e2eTxn-diagnostics/3partsCrossComponent.png)

### <a name="1-cross-component-transaction-chart"></a>[1] Komponentenübergreifendes Transaktionsdiagramm

Dieses Diagramm stellt eine komponentenübergreifende Zeitachse mit horizontalen Balken für die Dauer von Anforderungen und Abhängigkeiten bereit. Alle gesammelten Ausnahmen werden ebenfalls auf der Zeitachse markiert.

* Die oberste Zeile dieses Diagramms stellt den Einstiegspunkt dar, die eingehende Anforderung für die erste in dieser Transaktion aufgerufene Komponente. Die Dauer stellt hier die insgesamt für den Abschluss der Transaktion in Anspruch genommene Zeit dar.
* Alle Aufrufe von externen Abhängigkeiten sind einfache, nicht zuklappbare Zeilen mit Symbolen, die den Typ der Abhängigkeit darstellen.
* Aufrufe an andere Komponenten sind in zuklappbaren Zeilen aufgeführt. Jede Zeile entspricht einem bestimmten für die Komponenten aufgerufenen Vorgang.
* Standardmäßig wird die zu Anfang von Ihnen ausgewählte Anforderung, Abhängigkeit oder Ausnahme im Diagramm angezeigt.
* Wählen Sie eine beliebige Zeile aus, um ihre Details auf der rechten Seite anzuzeigen. Klicken Sie auf "Open profiler traces" (Profiler-Überwachungen öffnen) oder "Open debug snapshot" (Debugmomentaufnahme öffnen), um die Diagnose auf Codeebene in den entsprechenden Detailbereichen anzuzeigen.

> [!NOTE]
Aufrufe an andere Komponenten weisen zwei Zeilen auf : Eine Zeile stellt den ausgehenden Aufruf (Abhängigkeit) von der aufrufenden Komponente dar, die andere Zeile entspricht der eingehenden Anforderung bei der aufgerufenen Komponente. Das führende Symbol und die unterschiedliche Gestaltung der Balken für die Dauer erleichtern die Unterscheidung zwischen ihnen.

### <a name="2-time-sequenced-telemetry-of-the-selected-component-operation"></a>[2] Telemetrie des ausgewählten Komponentenvorgangs in der zeitlichen Abfolge

Jede im komponentenübergreifenden Transaktionsdiagramm ausgewählte Zeile bezieht sich auf einen Vorgang, der bei einer bestimmten Komponente aufgerufen wurde. Dieser ausgewählte Komponentenvorgang spiegelt sich im Titel des unteren Bereichs wider. Öffnen Sie diesen Abschnitt, um die Abfolge der gesamten Telemetrie im Zusammenhang mit diesem bestimmten Vorgang im zeitlichen Verlauf anzuzeigen. Sie können in dieser Liste beliebige Telemetrieelemente auswählen, um die entsprechenden Details auf der rechten Seite anzuzeigen.

![Zeitliche Abfolge der gesamten Telemetrie](media/app-insights-e2eTxn-diagnostics/allTelemetryDrawerOpened.png)

### <a name="3-details-pane"></a>[3] Detailbereich

In diesem Bereich werden die Details der in einem der beiden Abschnitte links ausgewählten Elemente angezeigt. Mit "Alle anzeigen" wird eine Liste der erfassten Standardattribute angezeigt. Eventuelle benutzerdefinierte Attribute werden separat unterhalb des Standardsatzes aufgeführt.

![Ausnahmedetail](media/app-insights-e2eTxn-diagnostics/exceptiondetail.png)

## <a name="profiler-and-snapshot-debugger"></a>Profiler und Momentaufnahmedebugger

Der [Application Insights Profiler](app-insights-profiler.md) oder der [Momentaufnahmedebugger](app-insights-snapshot-debugger.md) unterstützen die Diagnose von Leistungs- und Ausfallproblemen auf Codeebene. Dank dieser Benutzeroberfläche können Sie Profilerüberwachungen oder Momentaufnahmen von beliebigen Komponenten mit nur einem Klick anzeigen.

Setzen Sie sich bei Problemen mit dem Profiler unter **serviceprofilerhelp@microsoft.com** mit uns in Verbindung.

Setzen Sie sich bei Problemen mit dem Momentaufnahmedebugger unter **snapshothelp@microsoft.com** mit uns in Verbindung.

![Debuggerintegration](media/app-insights-e2eTxn-diagnostics/debugSnapshot.png)

## <a name="faq"></a>Häufig gestellte Fragen

*Es wird eine einzelne Komponente im Diagramm angezeigt, und die anderen werden nur als externe Abhängigkeiten ohne Details zu den Vorgängen innerhalb der Komponenten dargestellt.*

Mögliche Ursachen:

* Sind die anderen Komponenten mit Application Insights instrumentiert?
* Verwenden sie das neueste stabile Application Insights-SDK?
* Wenn es sich bei diesen Komponenten um separate Application Insights-Ressourcen handelt, haben Sie den erforderlichen Zugriff auf ihre Telemetrie?

Wenn Sie Zugriff haben und die Komponenten mit den neuesten Application Insights-SDKs instrumentiert sind, informieren Sie uns über den Feedbackkanal in der oberen rechten Ecke.

*Ich habe nur externe Abhängigkeiten. Sollte ich mich trotzdem für diese Vorschauversion interessieren?*

Ja. Die neue Benutzeroberfläche führt die gesamte zusammengehörige serverseitige Telemetrie in einer einzelnen Ansicht zusammen. Die älteren Detailblätter werden zukünftig durch diese Oberfläche ersetzt – es ist also sinnvoll, dass Sie sie ausprobieren und uns Feedback geben. So sieht sie für einen Einzelkomponentenvorgang aus:

![Benutzeroberfläche für eine einzelne Komponente](media/app-insights-e2eTxn-diagnostics/singleComponent.png)

*Ich sehe doppelte Zeilen für die Abhängigkeiten. Entspricht dies dem erwarteten Verhalten?*

Zurzeit zeigen wir den ausgehenden Abhängigkeitsaufruf separat von der eingehenden Anforderung an. Normalerweise sehen die beiden Aufrufe identisch aus, lediglich der Wert für die Dauer unterscheidet sich aufgrund des Netzwerk-Roundtrips. Das führende Symbol und die unterschiedliche Gestaltung der Balken für die Dauer erleichtern die Unterscheidung zwischen ihnen. Finden Sie diese Darstellung der Daten verwirrend? Senden Sie uns Ihr Feedback!

*Treten Uhrabweichungen über verschiedene Komponenteninstanzen hinweg auf?*

Die Zeitachsen im Transaktionsdiagramm gleichen Uhrabweichungen aus. Sie können die genauen Zeitstempel im Detailbereich oder mithilfe von Analytics anzeigen.

*Warum fehlen auf der neuen Benutzeroberfläche die meisten der zugehörigen Elementabfragen?*

Dies ist beabsichtigt. Alle zugehörigen Elemente über alle Komponenten hinweg stehen bereits auf der linken Seite zur Verfügung (oberer und unterer Abschnitt). Die neue Benutzeroberfläche weist zwei zugehörige Elemente auf, die von der linken Seite nicht abgedeckt werden: die gesamte Telemetrie fünf Minuten vor und nach dem Ereignis und die Benutzerzeitachse.

*Warum fehlt in der neuen Benutzeroberfläche ein bestimmtes Feature, das ich auf den älteren Blättern sehr geschätzt habe?*

Geben Sie uns Feedback! Wir möchten Ihre Bedenken berücksichtigen, bevor die neue Benutzeroberfläche allgemein verfügbar wird, da die älteren Ansichten zu diesem Zeitpunkt als veraltet markiert werden. Fürs Erste können Sie die Vorschauversion deaktivieren und zu den älteren Blättern zurückkehren.

## <a name="known-issues"></a>Bekannte Probleme

* Die Fehlerstichproben in der Anwendungsübersicht verweisen auf die älteren Detailblätter.
* Die auf Autocluster basierenden Einblicke in den Suchergebnissen verweisen auf die älteren Detailblätter.
* Auf der neuen Benutzeroberfläche ist keine Integration von Arbeitsaufgaben verfügbar.

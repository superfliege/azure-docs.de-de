---
title: Übersicht über Azure Time Series Insights | Microsoft-Dokumentation
description: Übersicht über Azure Time Series Insights
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 12/03/2018
ms.openlocfilehash: 83513ab9f7bee5c51fc909e5f4246105b6f145ca
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876130"
---
# <a name="azure-time-series-insights-overview"></a>Übersicht über Azure Time Series Insights

Azure Time Series Insights (TSI) ist eine End-to-End-Plattform (Platform-as-a-Service) zum Erfassen, Verarbeiten, Speichern und Abfragen von stark kontextualisierten IoT-Daten, die für Zeitreihen optimiert sind. Daher ist Azure TSI ideal für Ad-hoc-Datenuntersuchungen und Betriebsanalysen geeignet. Bei TSI handelt es sich um ein speziell erweiterbares, angepasstes Dienstangebot, das für die weit reichenden Anforderungen von IoT-Bereitstellungen in der Industrie konzipiert ist.

## <a name="what-is-iot-data"></a>Was sind IoT-Daten?

IoT-Daten sind alle „Industriedaten“, die in ressourcenintensiven Organisationen vorhanden sind. IoT-Daten sind häufig sehr unstrukturiert, da sie von vielen unterschiedlichen Ressourcen gesendet werden, die die verschiedensten Messungen durchführen, z.B. in den Bereichen Temperatur, Bewegung und Luftfeuchtigkeit. Außerdem zeichnen sich diese Datenströme häufig durch erhebliche Lücken, beschädigte Nachrichten und fehlerhafte Messwerte aus. Daten aus diesen Datenströmen müssen bereinigt werden, bevor eine Analyse durchgeführt werden kann. IoT-Daten sind oft nur in Verbindung mit zusätzlichen Dateneingaben aussagekräftig, die aus Datenquellen von Erstanbietern (z.B. CRM oder ERP) oder Drittanbietern (z.B. Wetter oder Standort) stammen.

Das Ergebnis ist, dass nur ein vernachlässigbarer Teil dieser Daten für betriebliche oder geschäftliche Zwecke genutzt wird. Daten dieser Art liefern einheitliche, umfassende, aktuelle und korrekte Informationen für die Berichterstellung und Analysen in Unternehmen. Damit aus erfassten IoT-Daten verwertbare Erkenntnisse gezogen werden können, werden daher einige Schlüsselfunktionen benötigt:

* Datenverarbeitung zum Bereinigen, Filtern, Interpolieren, Transformieren und Aufbereiten von Daten für die Analyse
* Struktur, damit eine Navigation durch die Daten möglich ist und diese verstanden werden können (für die Normalisierung und Kontextualisierung der Daten)
* Kostengünstige Speicherung zur langen bzw. endlosen Aufbewahrung von verarbeiteten (abgeleiteten) Daten und Rohdaten mehrerer Jahrzehnte

Der typische IoT-Datenfluss kann wie folgt dargestellt werden:

  ![IoT-Datenfluss][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Azure Time Series Insights für industrielle IoT-Daten

Die derzeitige IoT-Landschaft ist vielfältig. Sie umfasst Kunden aus den Bereichen Fertigung, Automobilbau, Öl und Gas, Stromversorgung und andere Versorger, Gebäudeautomatisierung und Consulting. Beispiele für Szenarien sind die Ad-hoc-Datenuntersuchung, bei der die Form der Daten unbekannt ist, oder die betriebsbezogene Analyse von schematisierten (explizit modellierten) Daten, um die betriebliche Effizienz zu fördern. Diese Szenarien sind normalerweise parallel vorhanden und unterstützen unterschiedliche Anwendungsfälle. Plattformfunktionen wie die Speicherung auf mehreren Ebenen („Warm Storage“ und „Cold Storage“), die Speicherung der Zeitreihendaten von Jahrzehnten und die explizite Modellierung und Optimierung von Abfragen für ressourcenbasierte betriebsbezogene Intelligence-Daten werden immer mehr zum Schlüssel für den Erfolg von Industrieunternehmen im IoT-Bereich und der damit verbundenen digitalen Revolution.

Azure TSI ist ein umfassendes Dienstangebot für eine End-to-End-Plattform (Platform-as-a-Service), die sowohl die Untersuchung von IoT-Daten als auch die Gewinnung von betriebsbezogenen Erkenntnissen ermöglicht. Mit Time Series Insights wird ein vollständig verwalteter Clouddienst zum Analysieren von IoT-Zeitreihendaten bereitgestellt.

Kunden können Rohdaten in einem schemalosen In-Memory-Speicher speichern und interaktive Ad-hoc-Abfragen über eine Engine für verteilte Abfragen und eine API durchführen. Darüber hinaus können sie unsere umfassende Benutzeroberfläche nutzen, um innerhalb weniger Sekunden Milliarden von Ereignissen zu visualisieren. Erfahren Sie mehr zu unseren [Funktionen für die Untersuchung von Daten](./time-series-insights-overview.md).

TSI verfügt auch über Funktionen zur Gewinnung von betriebsbezogenen Erkenntnissen, die sich derzeit in der öffentlichen Vorschauphase (Public Preview) befinden. Zusammen mit den Funktionen für die interaktive Datenuntersuchung und betriebsbezogene Intelligence-Daten ermöglicht Time Series Insights Kunden das Ableiten eines höheren Nutzens aus Daten, die über IoT-Ressourcen erfasst wurden. Für das Public Preview-Angebot werden die folgenden wichtigen Funktionen unterstützt:

* Ein skalierbarer Zeitreihendaten-Speicher, der in Bezug auf Leistung und Kosten optimiert ist und es für eine cloudbasierte IoT-Lösung ermöglicht, für Zeitreihendaten mehrerer Jahre innerhalb von wenigen Sekunden Trendinformationen zu erstellen.
* Unterstützung des Semantikmodells zur Beschreibung der Domänen- und Metadaten, die den abgeleiteten und nicht abgeleiteten Signalen von Ressourcen und Geräten zugeordnet sind.
* Eine verbesserte Benutzeroberfläche, für die ressourcenbasierte Datenerkenntnisse mit einer umfassenden Ad-hoc-Datenanalyse zur Förderung der geschäftlichen und betriebsbezogenen Intelligence kombiniert werden.
* Integration von erweiterten Machine Learning- und Analysetools, z.B. Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter Notebooks, Power BI usw., um Kunden beim Bewältigen von Herausforderungen in Bezug auf Zeitreihendaten und beim Fördern der betrieblichen Effizienz zu unterstützen.

Die Funktionen für betriebsbezogene Erkenntnisse und die Datenuntersuchung werden gemeinsam mit einem einfachen Preismodell mit nutzungsbasierter Bezahlung für Datenverarbeitung, -speicherung und -abfragen angeboten. Kunden erhalten so ein deutlich besser skalierbares Modell, mit dem sie ihre sich ständig ändernden Geschäftsanforderungen erfüllen können.

Unten ist ein allgemeines Datenflussdiagramm angegeben, in dem die aktualisierten Funktionen dargestellt sind:

  ![Wichtige Funktionen][2]

Aufgrund der Einführung dieser wichtigen IoT-Funktionen für die Industrie verfügt Azure TSI über die unten angegebenen entscheidenden Vorteile.

* Speicherung auf mehreren Ebenen für IoT-Zeitreihendaten

  * Über eine zentrale Datenverarbeitungspipeline zur Erfassung von Daten können Kunden Daten für interaktive Abfragen in Warm Storage speichern. Cold Storage dient zum Speichern von großen Datenmengen und zur Durchführung von ressourcenbasierten Abfragen mit hoher Leistung.

  * Das dynamische Routing über Speicherebenen hinweg ist in Kürze ebenfalls verfügbar.

* Zeitreihenmodell zum Kontextualisieren von Telemetrie-Rohdaten und Gewinnen von ressourcenbasierten Erkenntnissen

  * Kunden können Telemetrie-Rohdaten mit einem beschreibenden Zeitreihenmodell kontextualisieren und umfassende betriebsbezogene Intelligence-Daten mit gerätebasierten Abfragen ableiten, die in Bezug auf Leistung und Kosten stark optimiert sind.

* Nahtlose Integration in andere Datenlösungen
  
  * Da Daten unter Azure Time Series Insights in Apache Parquet-Open-Source-Dateien gespeichert werden, können Kunden für End-to-End-Szenarien, z.B. Business Intelligence, erweitertes maschinelles Lernen, Predictive Analytics usw., auf einfache Weise eine Integration in andere Datenlösungen (Erst- oder Drittanbieter) durchführen.

* Datenuntersuchung nahezu in Echtzeit

  * Die Benutzeroberfläche des Azure Time Series Insights-Explorers ermöglicht die Visualisierung für den gesamten Datenstreamingprozess über die Erfassungspipeline. Schon kurze Zeit nach dem Herstellen der Verbindung mit einer Ereignisquelle können Kunden Ereignisdaten anzeigen, untersuchen und abfragen, um zu überprüfen, ob ein Gerät Daten erwartungsgemäß sendet. Außerdem können sie eine IoT-Ressource auf Integrität, Produktivität und allgemeine Effektivität überwachen.

* Ursachenanalyse und Anomalieerkennung

  * Azure Time Series Insights-Explorer unterstützt Muster und perspektivische Ansichten zum Durchführen und Speichern von mehrstufigen Ursachenanalysen. In Kombination mit Azure Stream Analytics können Kunden Time Series Insights nutzen, um nahezu in Echtzeit Warnungen und Anomalien zu erkennen.

* Erstellen von benutzerdefinierten Anwendungen auf der Time Series Insights-Plattform

  * Azure Time Series Insights unterstützt das JavaScript SDK mit umfassenden Steuerungsmöglichkeiten und einfachem Zugriff auf Abfragen, damit Kunden zusätzlich zur Time Series Insights-Plattform benutzerdefinierte IoT-Anwendungen erstellen können, um die individuellen Anforderungen von Unternehmen zu erfüllen.
  * Darüber hinaus können Kunden Time Series Insights-Abfrage-APIs direkt verwenden, um Daten in benutzerdefinierte IoT-Anwendungen zu befördern.

## <a name="next-steps"></a>Nächste Schritte

Sie sind jetzt für die ersten Schritte mit Azure TSI (Vorschauversion) bereit:

> [!div class="nextstepaction"]
> [Lesen Sie die Schnellstartanleitung](./time-series-insights-update-quickstart.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png
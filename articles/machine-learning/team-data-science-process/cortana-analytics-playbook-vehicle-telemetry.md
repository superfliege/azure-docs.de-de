---
title: Vorhersagen von Fahrzeugzustand und Fahrgewohnheiten – Azure | Microsoft-Dokumentation
description: Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten.
services: machine-learning
author: marktab
ms.author: tdsp
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 03/14/2018
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 688a821667b38423b43af87b7a26525e52d9a7aa
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444158"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Lösungsplaybook zur Fahrzeugtelemetrieanalyse

Supercomputer stehen nicht mehr im Labor, sondern sind jetzt in der Garage geparkt. Sie befinden sich in hochmodernen Fahrzeugen, die mit unzähligen Sensoren ausgestattet sind. Mit diesen Sensoren können pro Sekunde mehrere Millionen Ereignisse nachverfolgt und überwacht werden. Bis 2020 werden die meisten dieser Fahrzeuge mit dem Internet verbunden sein. Auf der Grundlage dieser riesigen Datensammlung werden Funktionen für mehr Sicherheit, Zuverlässigkeit und ein besseres Fahrerlebnis bereitgestellt. Microsoft lässt diesen Traum mit Cortana Intelligence Wirklichkeit werden.

Cortana Intelligence ist eine vollständig verwaltete Big Data-Lösung für erweiterte Analysen, mit der Sie Ihre Daten in intelligente Aktionen umwandeln können. Die Lösungsvorlage für die Cortana Intelligence-Fahrzeugtelemetrieanalyse veranschaulicht, wie Automobilhändler, Automobilhersteller und Versicherungsgesellschaften in Echtzeit und prädiktiv Einblicke in Fahrzeugzustand und Fahrgewohnheiten gewinnen können.

Die Lösung ist als ein [Lambda-Architekturmuster](https://en.wikipedia.org/wiki/Lambda_architecture) implementiert und verfügt dadurch über das vollständige Potenzial der Cortana Intelligence-Plattform für Echtzeit- und Batchverarbeitung.

## <a name="architecture"></a>Architecture
Die Lösungsarchitektur für die Fahrzeugtelemetrieanalyse ist in diesem Diagramm dargestellt:

![Diagramm der Lösungsarchitektur](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Diese Lösung umfasst die folgenden Cortana Intelligence-Komponenten und stellt ihre Integration vor:

* **Azure Event Hubs** für das Erfassen von Millionen von Telemetrieereignissen zu Fahrzeugen in Azure
* **Azure Stream Analytics** für das Gewinnen von Einblicken in Echtzeit zum Fahrzeugzustand und für das Speichern dieser Daten in beständigem Speicher zur umfangreicheren Batchanalyse
* **Azure Machine Learning** erkennt Anomalien in Echtzeit und stellt mithilfe der Batchverarbeitung Prognoseinformationen bereit.
* **Azure HDInsight** transformiert die Daten nach Bedarf.
* **Azure Data Factory** übernimmt die Orchestrierung, Planung, Ressourcenverwaltung und Überwachung der Batchverarbeitungs-Pipeline.
* **Power BI** bietet dieser Lösung ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und Predictive Analytics.

Diese Lösung greift auf die folgenden beiden unterschiedlichen Datenquellen zu: 

* **Simulierte Fahrzeugsignale und -diagnose:** Ein Fahrzeugtelematiksimulator gibt Diagnoseinformationen und Signale aus, die dem Zustand des Fahrzeugs und dem Fahrverhalten zu einem bestimmten Zeitpunkt entsprechen. 
* **Fahrzeugkatalog:** Dieses Referenzdataset ordnet Modellen Fahrgestellnummern zu.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu dieser Lösung finden Sie unter [Lösungs-Playbook zur Fahrzeugtelemetrieanalyse: Ausführliche Betrachtung der Lösung](cortana-analytics-playbook-vehicle-telemetry-deep-dive.md).

Informationen zum Konfigurieren der Power BI-Berichte und -Dashboards für diese Lösung finden Sie unter [Lösungsvorlage „Analyse von Telemetriedaten für Fahrzeuge“ – Anweisungen zur Einrichtung des Power BI-Dashboards](cortana-analytics-playbook-vehicle-telemetry-powerbi.md).

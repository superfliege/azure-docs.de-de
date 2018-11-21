---
title: Vorhersagen von Fahrzeugzustand und Fahrgewohnheiten – Azure | Microsoft-Dokumentation
description: Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.openlocfilehash: 0594b9b40fdf8647f99946a31dbb47a95c9694ac
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300352"
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

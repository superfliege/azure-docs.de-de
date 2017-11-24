---
title: "Vorhersagen von Fahrzeugzustand und Fahrgewohnheiten – Azure | Microsoft-Dokumentation"
description: "Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: bec5066a2e1ba0e4e5e81c4e1be28ed8eb93ceed
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Lösungsplaybook zur Fahrzeugtelemetrieanalyse
Dieses Menü bietet Links zu den Kapiteln in diesem Playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Übersicht
Supercomputer stehen nicht mehr im Labor, sondern sind jetzt in der Garage geparkt. Diese hochmodernen Fahrzeuge enthalten unzählige Sensoren, sodass sie Millionen von Ereignissen pro Sekunde verfolgen und überwachen können. Bis 2020 werden die meisten dieser Fahrzeuge mit dem Internet verbunden sein. Auf der Basis dieser riesigen Datensammlung können Funktionen für mehr Sicherheit, Zuverlässigkeit und ein besseres Fahrgefühl bereitgestellt werden. Microsoft lässt diesen Traum mit Cortana Intelligence Wirklichkeit werden.

Cortana Intelligence ist eine vollständig verwaltete Big Data-Lösung für erweiterte Analysen, mit der Sie Ihre Daten in intelligente Aktionen umwandeln können. Die Lösungsvorlage für die Cortana Intelligence-Fahrzeugtelemetrieanalyse veranschaulicht, wie Automobilhändler, Automobilhersteller und Versicherungsgesellschaften in Echtzeit und prädiktiv Einblicke in Fahrzeugzustand und Fahrgewohnheiten gewinnen können. 

Die Lösung ist als ein [Lambda-Architekturmuster](https://en.wikipedia.org/wiki/Lambda_architecture) implementiert und verfügt dadurch über das vollständige Potenzial der Cortana Intelligence-Plattform für Echtzeit- und Batchverarbeitung.

## <a name="architecture"></a>Architektur
Die Lösungsarchitektur für die Fahrzeugtelemetrieanalyse ist in diesem Diagramm dargestellt:

![Diagramm der Lösungsarchitektur](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Diese Lösung umfasst die folgenden Cortana Intelligence-Komponenten und stellt ihre End-to-End-Integration vor:

* **Azure Event Hubs** für das Erfassen von Millionen von Telemetrieereignissen zu Fahrzeugen in Azure
* **Azure Stream Analytics** für das Gewinnen von Einblicken in Echtzeit zum Fahrzeugzustand und für das Speichern dieser Daten in beständigem Speicher zur umfangreicheren Batchanalyse
* **Azure Machine Learning** erkennt Anomalien in Echtzeit und stellt mithilfe der Batchverarbeitung Prognoseinformationen bereit.
* **Azure HDInsight** transformiert die Daten nach Bedarf.
* **Azure Data Factory** übernimmt die Orchestrierung, Planung, Ressourcenverwaltung und Überwachung der Batchverarbeitungs-Pipeline.
* **Power BI** bietet dieser Lösung ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und Predictive Analytics.

Diese Lösung greift auf die folgenden beiden unterschiedlichen Datenquellen zu: 

* **Simulierte Fahrzeugsignale und -diagnose:**Ein Fahrzeugtelematiksimulator gibt Diagnoseinformationen und Signale aus, die dem Zustand des Fahrzeugs und dem Fahrverhalten zu einem bestimmten Zeitpunkt entsprechen. 
* **Fahrzeugkatalog:** Dieses Referenz-DataSet ordnet VINs Modellen zu.


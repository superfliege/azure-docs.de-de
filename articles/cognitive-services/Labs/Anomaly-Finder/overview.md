---
title: Worum handelt es sich bei der Anomaliesuche? – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Verwenden Sie hoch entwickelte Algorithmen in der Anomaliesuche, um Anomalien in Zeitreihendaten zu erkennen und Informationen in Microsoft Cognitive Services zurückzugeben.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 1080bb0ad1d901a8b9a5ace4993d4e0d46924a03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375354"
---
# <a name="what-is-anomaly-finder"></a>Worum handelt es sich bei der Anomaliesuche?

Mithilfe der Anomaliesuche können Sie Daten im Zeitverlauf überwachen und Anomalien beim maschinellen Lernen ermitteln, das sich durch die automatische Anwendung des richtigen Statistikmodells an Ihre Daten anpasst, unabhängig von Branche, Szenario oder Datenvolumen. Auf der Grundlage einer Zeitreihe als Eingabe gibt die API zur Anomaliesuche zurück, ob ein Datenpunkt eine Anomalie darstellt oder nicht, bestimmt den erwarteten Wert und die Ober- und Untergrenze für die Visualisierung. Als vorgefertigter AI-Dienst benötigt die Anomaliesuche keine Fachkenntnisse auf dem Gebiet des maschinellen Lernens über das Wissen um die Verwendung einer RESTful API hinaus. Dadurch wird die Entwicklung einfach und flexibel, da dies mit beliebigen Zeitreihendaten funktioniert und sogar in Streamingdatensysteme integriert werden kann. Die Anomaliesuche umfasst eine große Bandbreite von Anwendungsfällen – beispielsweise Finanztools für die Bearbeitung von Betrug, Diebstahl, sich ändernden Märkten und möglichen Geschäftsvorfällen oder das Überwachen von Datenverkehr mit IoT-Geräten bei Aufrechterhaltung der Anonymität. Diese Lösung kann auch als Teil einer Dienstleistung für Endkunden zum Verstehen von Änderungen an den Daten, dem Ausgabeverhalten, dem Return-on-Investment oder der Benutzeraktivität monetarisiert werden.
Testen Sie die Anomaliesuche-API, und erwerben Sie ein tieferes Verständnis Ihrer Daten. 

Sehen Sie, was Sie mit dieser API erstellen können:

* Lernen Sie, die erwarteten Werte auf der Grundlage von historischen Daten in der Zeitreihe vorherzusagen
* Erkennen Sie anhand eines historischen Musters, ob ein Datenpunkt eine Anomalie darstellt
* Generieren Sie ein Band, um den Bereich der „normalen“ Werte zu visualisieren

![Anomalie_Suche](./media/anomaly_detection1.png) 

Abb. 1: Erkennen von Anomalien in Vertriebsumsätzen

![Anomalie_Suche](./media/anomaly_detection2.png)

Abb. 2: Erkennen von Musteränderungen in Dienstanforderungen

## <a name="requirements"></a>Anforderungen

- Mindestdaten für die Eingabe von Zeitreihen: Mindestens 13 Datenpunkte für Zeitreihen ohne eindeutige Periodizität, mindestens vier Zyklen von Datenpunkten für die Zeitreihe mit bekannter Periodizität. 
- Datenintegrität: Datenpunkte von Zeitreihen liegen voneinander getrennt innerhalb des gleichen Intervalls vor, und es fehlen keine Punkte. 

## <a name="identify-anomalies"></a>Erkennen von Anomalien

Die API zur Anomalieerkennung gibt für alle angegebenen Datenpunkte das Ergebnis zurück, ob sie Anomalien darstellen oder nicht, und bietet darüber hinaus die folgenden zusätzlichen Informationen
* Period (Periode): Die Periodizität, die von der API zum Erkennen der Anomaliepunkte verwendet wurde.
* Waning Text (Warntext): Die möglichen Warninformationen.
* ExpectedValue (ErwarteterWert): Der auf der Grundlage des lernbasierten Modells vorhergesagte Wert
* IsAnomaly (IstAnomalie): Das Ergebnis, ob die Datenpunkte Anomalien darstellen oder nicht
* IsAnomaly_Neg (IstAnomalie_neg): Das Ergebnis, ob die Datenpunkte Anomalien darstellen in negativer Richtung (Senken)
* IsAnomaly_Pos (IstAnomalie_pos): Das Ergebnis, ob die Datenpunkte Anomalien darstellen in positiver Richtung (Zacken)
* UpperMargin (Obergrenze): Die Summe von „ExpectedValue“ und „UpperMargin“ bestimmt die Obergrenze, unterhalb derer ein Datenpunkt noch als normal angesehen wird
* LowerMargin (Untergrenze): („ExpectedValue“ - „LowerMargin“) bestimmt die Untergrenze, oberhalb derer der betreffende Datenpunkt noch als normal angesehen wird

> [!Note]
> „UpperMargin“ und „LowerMargin“ können verwendet werden, um ein die eigentliche Zeitreihe umgebendes Band zu generieren, das den Bereich der normalen Werte visualisiert. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Anpassen von Unter- und Obergrenzen in der Nachbearbeitung der Antwort

Die API zur Anomalieerkennung gibt ein Standardergebnis dazu zurück, ob ein Datenpunkt eine Anomalie darstellt oder nicht, und die Unter- und Obergrenze kann aus „ExpectedValue“ und „UpperMargin“/„LowerMargin“ berechnet werden. Diese Standardwerte sollten in den meisten Fällen gut funktionieren. Einige Szenarien erfordern jedoch andere als die Standardgrenzen. Die empfohlene Vorgehensweise besteht in der Anwendung von Koeffizienten auf „UpperMargin“ oder „LowerMargin“, um die dynamischen Grenzen anzupassen.

### <a name="examples-with-1152-as-coefficiency"></a>Beispiel mit den Koeffizienten 1/1,5/2

![Standardempfindlichkeit](./media/sensitivity_1.png)

![Empfindlichkeit 1,5](./media/sensitivity_1.5.png)

![Empfindlichkeit 2](./media/sensitivity_2.png)

Anforderung mit Beispieldaten

[!INCLUDE [Request](./includes/request.md)]

JSON-Beispielantwort

[!INCLUDE [Response](./includes/response.md)]

---
title: Includedatei
description: Includedatei
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 3cc0e521e43f6855397a19fe34fce99da3e20494
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228866"
---
Mit der [API für die Suche nach Anomalien](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder) können Sie Zeitreihendaten im JSON-Format im API-Endpunkt hochladen, und die Ergebnisse dann aus der API-Antwort lesen. Jeder Datenpunkt enthält folgende Angaben:  
* Timestamp: Zeitstempel des Datenpunkts. Überprüfen Sie, ob die UTC-Zeichenfolge für Datum/Uhrzeit verwendet wird, z. B. „2017-08-01T00:00:00Z“.
* Value: Messung des Datenpunkts.

Die Ergebnisse umfassen Folgendes:
* Period: Periodizität, die die API zur Anomalieerkennung verwendet
* WarningText: mögliche Warninformationen
* ExpectedValue: vorhergesagter Wert auf Basis des lernbasierten Modells
* IsAnomaly: Ergebnis, ob die Datenpunkte Anomalien in beide Richtungen darstellen (Hoch oder Tief)
* IsAnomaly_Neg: Ergebnis, ob die Datenpunkte Anomalien in negativer Richtung darstellen (Tief)
* IsAnomaly_Pos: Ergebnis, ob die Datenpunkte Anomalien in positiver Richtung darstellen (Hoch)
* UpperMargin: Summe aus „ExpectedValue“ und „UpperMargin“; bestimmt die Obergrenze, unterhalb derer ein Datenpunkt als normal gilt
* LowerMargin: Differenz aus „ExpectedValue“ und „LowerMargin“; bestimmt die Untergrenze, oberhalb derer ein Datenpunkt als normal gilt

Details zum Datenvertrag [finden Sie hier](../apiref.md).


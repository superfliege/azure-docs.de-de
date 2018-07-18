---
title: Includedatei
description: Includedatei
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: ff36202b67f6262b7ba67fe48ef37f2b656b91fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375363"
---
<a name="definitions"></a>
## <a name="definitions"></a>Definitionen

<a name="point"></a>
### <a name="point"></a>Point

|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**Timestamp**  <br>*optional*|Der Zeitstempel für den Datenpunkt. Achten Sie darauf, dass er an Mitternacht ausgerichtet ist, und verwenden Sie eine UTC-Zeichenfolge für Datum/Uhrzeit, z.B. 2017-08-01T00:00:00Z.|string (Datum/Uhrzeit)|
|**Wert**  <br>*optional*|Ein Datenmesswert.|number (double)|


<a name="request"></a>
### <a name="request"></a>Anforderung

|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**Zeitraum**  <br>*optional*|Der Zeitraum der Datenpunkte. Wenn der Wert NULL oder nicht vorhanden sind, bestimmt die API den Zeitraum automatisch.|number (double)|
|**Points**  <br>*optional*|Die Datenpunkte der Zeitreihe. Die Daten sollten nach dem Zeitstempel aufsteigend sortiert sein, um dem Anomalieergebnis zu entsprechen. Wenn die Daten nicht ordnungsgemäß sortiert sind oder ein Zeitstempel doppelt vorhanden ist, erkennt die API die Anomaliepunkte richtig, Sie können die zurückgegebenen Punkte der Eingabe aber nur schlecht zuordnen. In diesem Fall wird der Antwort eine Warnmeldung hinzugefügt.|< [point](#point) > Array|


<a name="response"></a>
### <a name="response"></a>response

|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**ExpectedValues**  <br>*optional*|Der vom lernbasierten Modell vorhergesagte Wert. Wenn die Eingabedatenpunkte aufsteigend nach dem Zeitstempel sortiert sind, kann der Index des Arrays verwendet werden, um den erwarteten Wert und den ursprünglichen Wert einander zuzuordnen.|< number (double) > Array|
|**IsAnomaly**  <br>*optional*|Ergebnis, ob die Datenpunkte Anomalien in beide Richtungen darstellen (Hoch oder Tief). „true“ bedeutet, dass der Punkt eine Anomalie darstellt, „false“, dass er keine Anomalie ist. Wenn die Eingabedatenpunkte aufsteigend nach dem Zeitstempel sortiert sind, kann der Index des Arrays verwendet werden, um den erwarteten Wert und den ursprünglichen Wert einander zuzuordnen.|< boolean > Array|
|**IsAnomaly_Neg**  <br>*optional*|Ergebnis, ob die Datenpunkte Anomalien in negativer Richtung darstellen (Tief). „true“ bedeutet, dass die Richtung der Anomalie negativ ist. Wenn die Eingabedatenpunkte aufsteigend nach dem Zeitstempel sortiert sind, kann der Index des Arrays verwendet werden, um den erwarteten Wert und den ursprünglichen Wert einander zuzuordnen.|< boolean > Array|
|**IsAnomaly_Pos**  <br>*optional*|Ergebnis, ob die Datenpunkte Anomalien in positiver Richtung darstellen (Hoch). „true“ bedeutet, dass die Richtung der Anomalie positiv ist. Wenn die Eingabedatenpunkte aufsteigend nach dem Zeitstempel sortiert sind, kann der Index des Arrays verwendet werden, um den erwarteten und den ursprünglichen Wert einander zuzuordnen.|< boolean > Array|
|**LowerMargin**  <br>*optional*|Differenz aus „ExpectedValue“ und „LowerMargin“; bestimmt die Untergrenze, oberhalb derer ein Datenpunkt als normal gilt. Wenn die Eingabedatenpunkte aufsteigend nach dem Zeitstempel sortiert sind, kann der Index des Arrays verwendet werden, um den erwarteten Wert und den ursprünglichen Wert einander zuzuordnen.|< number (double) > Array|
|**Zeitraum**  <br>*optional*|Der Zeitraum, der von der API zum Erkennen der Anomaliepunkte verwendet wurde.|number (float)|
|**UpperMargin**  <br>*optional*|Summe aus „ExpectedValue“ und „UpperMargin“; bestimmt die Obergrenze, unterhalb derer ein Datenpunkt als normal gilt. Wenn die Eingabedatenpunkte aufsteigend nach dem Zeitstempel sortiert sind, kann der Index des Arrays verwendet werden, um den erwarteten Wert und den ursprünglichen Wert einander zuzuordnen.|< number (double) > Array|
|**WarningText**  <br>*optional*|Wenn die bereitgestellten Eingabedatenpunkte nicht der Regel folgen, die für die API erforderlich ist, und die Daten von der API trotzdem erkannt werden können, analysiert die API die Daten und fügt die Warninformationen in diesem Feld an.|Zeichenfolge|




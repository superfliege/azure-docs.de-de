---
title: Verwenden des Rückrufs zur Entitätserkennung in Verbindung mit einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie den Rückruf zur Entitätserkennung in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f168018a23d03ffb957da2dd1f67881420a21208
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171102"
---
# <a name="how-to-use-entity-detection-callback"></a>Verwenden des Rückrufs zur Entitätserkennung

Dieses Tutorial veranschaulicht den Rückruf zur Entitätserkennung und demonstriert ein allgemeines Muster zum Auflösen von Entitäten.

## <a name="video"></a>Video

[![Tutorial 10 – Vorschau](http://aka.ms/cl-tutorial-10-preview)](http://aka.ms/blis-tutorial-10)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der `tutorialEntityDetectionCallback`-Bot ausgeführt werden:

    npm run tutorial-entity-detection

## <a name="details"></a>Details
Der Rückruf zur Entitätserkennung ermöglicht die Verwendung von benutzerdefiniertem Code zum Verarbeiten von Geschäftsregeln im Zusammenhang mit Entitäten. In dieser Demo verwenden wir Rückrufe und programmgesteuerte Entitäten, um den von einem Benutzer eingegebenen Stadtnamen in einen kanonischen Namen aufzulösen – also beispielsweise „Big Apple“ in „New York“.

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste auf „Tutorial-10-EntityDetenctionCallback“. 

### <a name="entities"></a>Entitäten

In diesem Modell sind drei Entitäten definiert.

![](../media/tutorial10_entities.PNG)

1. „City“ (Stadt) ist eine benutzerdefinierte Entität, die der Benutzer als Texteingabe bereitstellt.
2. „CityUnknown“ (UnbekannteStadt) ist eine programmgesteuerte Entität. Diese Entität wird vom System aufgefüllt. Sie besteht aus einer Kopie der Benutzereingabe, wenn das System die gesuchte Stadt nicht erkennt.
3. „CityResolved“ (AufgelösteStadt) ist die Stadt, die vom System erkannt wird. Dabei handelt es sich um den kanonischen Namen der Stadt – z. B. wird „Big Apple“ in „New York“ aufgelöst.

### <a name="actions"></a>Actions

In diesem Modell sind drei Aktionen definiert.

![](../media/tutorial10_actions.PNG)

1. Die erste Aktion ist „Welche Stadt möchten Sie?“
2. Die zweite ist „Ich kenne diese Stadt nicht, ‚$CityUknown‘. Welche Stadt möchten Sie?“
3. Die dritte ist „Sie haben ‚$City‘ gesagt, und ich habe das in ‚$CityResolved‘ aufgelöst“

### <a name="callback-code"></a>Rückrufcode

Sehen wir uns den Code an. Sie können die EntityDetectionCallback-Methode in der Datei „C:\<Installationspfad>\src\demos\tutorialSessionCallbacks.ts“ finden.

![](../media/tutorial10_callbackcode.PNG)

Diese Funktion wird aufgerufen, nachdem eine Entitätsauflösung aufgetreten ist.
 
- Sie löscht als erste Aktion „$CityUknown“. „$CityUknown“ bleibt nur für einen einzelnen Durchgang erhalten, da es stets am Anfang gelöscht wird.
- Anschließend rufen Sie die Liste der Städte ab, die erkannt wurden. Die erste wird ausgewählt, und es wird versucht, sie aufzulösen.
- Die Funktion zur Auflösung (resolveCity) ist weiter oben im Code definiert. Sie weist eine Liste mit kanonischen Stadtnamen auf. Wenn sie den Stadtnamen in der Liste findet, gibt sie ihn zurück. Andernfalls schlägt sie in „cityMap“ nach und gibt den zugeordneten Namen zurück. Wenn sie eine Stadt nicht finden kann, gibt sie NULL zurück.
- Wenn die Stadt schließlich in einen Namen aufgelöst wurde, wird sie in der Entität „$CityKnown“ gespeichert. Andernfalls wird die Benutzerangabe gelöscht und die Entität „$CityUknown“ aufgefüllt.

### <a name="train-dialogs"></a>Trainingsdialoge

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welches Stadt möchten Sie?“ aus.
2. Geben Sie „New York“ ein.
    - Der Text wird als Stadtentität erkannt.
5. Klicken Sie auf „Bewertungsaktionen“.
    - `City` und `CityResolved` wurden aufgefüllt.
6. Wählen Sie „Sie haben ‚$City‘ gesagt, und ich habe das in ‚$CityResolved‘ aufgelöst“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

Fügen Sie einen weiteren Beispieldialog hinzu:

1. Klicken Sie auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welches Stadt möchten Sie?“ aus.
2. Geben Sie „Big Apple“ ein.
    - Der Text wird als Stadtentität erkannt.
5. Klicken Sie auf „Bewertungsaktionen“.
    - `CityResolved` zeigt die Auswirkung des ausgeführten Codes.
6. Wählen Sie „Sie haben ‚$City‘ gesagt, und ich habe das in ‚$CityResolved‘ aufgelöst“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

Fügen Sie einen weiteren Beispieldialog hinzu:

1. Klicken Sie auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welches Stadt möchten Sie?“ aus.
2. Geben Sie „foo“ ein.
    - Dies ist ein Beispiel für eine Stadt, die das System nicht kennt. 
5. Klicken Sie auf „Bewertungsaktionen“.
6. Wählen Sie „Ich kenne diese Stadt nicht, ‚$CityUknown‘. Welche Stadt möchten Sie?“ aus.
7. Geben Sie „New York“ ein.
8. Klicken Sie auf „Bewertungsaktionen“.
    - `CityUknown` wurde gelöscht und `CityResolved` wird aufgefüllt.
6. Wählen Sie „Sie haben ‚$City‘ gesagt, und ich habe das in ‚$CityResolved‘ aufgelöst“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Sitzungsrückrufe](./11-session-callbacks.md)

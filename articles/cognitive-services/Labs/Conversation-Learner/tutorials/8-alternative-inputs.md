---
title: Alternative Eingaben im Unterhaltungslernmodul – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Informationen zum Verwenden alternativer Eingaben im Unterhaltungslernmodul
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3b38cc265b4adfb301dc1165e02b17a8aa7c9589
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51252546"
---
# <a name="how-to-use-alternative-inputs"></a>Alternative Eingaben

Dieses Tutorial zeigt Ihnen, wie Sie das Feld „Alternative Eingaben“ für Benutzereingaben in der Trainingsoberfläche verwenden.

## <a name="video"></a>Video

[![Tutorial 8 – Vorschau](https://aka.ms/cl-tutorial-08-preview)](https://aka.ms/blis-tutorial-08)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Alternative Eingaben sind alternative Benutzeräußerungen, die möglicherweise an einer bestimmten Stelle in einem Dialog auftreten. Mit alternativen Eingaben können Sie die Variationen der Benutzeräußerungen kompakter bestimmen, ohne jede Variante in einem separaten Trainingsdialog aufzulisten.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ „AlternativeInputs“ ein. Klicken Sie auf „Erstellen“.

### <a name="create-an-entity"></a>Erstellen einer Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Bezeichnung „Stadt“ ein.
3. Klicken Sie auf „Erstellen“.

### <a name="create-three-actions"></a>Erstellen von drei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ die Frage „Welche Stadt?“ ein.
3. Geben Sie im Feld „Disqualifizierende Entitäten“ die Zeichenfolge „$city“ ein.
3. Klicken Sie auf „Erstellen“.

Erstellen Sie dann die zweite Aktion:

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
3. Geben Sie im Feld „Antwort“ den Satz „Das Wetter in $Stadt ist wahrscheinlich sonnig“ ein.
4. Geben Sie als erforderliche Entitäten die Zeichenfolge „$city“ ein.
4. Klicken Sie auf „Erstellen“.

Erstellen Sie die dritte Aktion:

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
3. Geben Sie im Feld „Antwort“ „Nach dem Wetter fragen“ ein.
    - Dies wäre eine mögliche Antwort auf eine Benutzerfrage wie „Was kann das System tun?“.
4. Geben Sie im Feld „Disqualifizierende Entitäten“ die Zeichenfolge „$city“ ein.
4. Klicken Sie auf „Erstellen“.

Jetzt haben Sie drei Aktionen.

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Wie ist das Wetter“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welche Stadt?“ aus.
2. Geben Sie „Denver“ ein.
3. Doppelklicken Sie auf „Denver“, und wählen Sie „Stadt“ aus.
    - Dadurch wird der Eintrag als Entität „Stadt“ gekennzeichnet.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Nun befindet sich „Denver“ in der Entität „Stadt“. 
6. Wählen Sie „Das Wetter in $city ist wahrscheinlich sonnig“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

Fügen Sie einen weiteren Beispieldialog hinzu:

1. Klicken Sie auf „Neue Aktion“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie die Frage „Was kannst du tun?“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie die „Nach dem Wetter fragen“ aus.
2. Geben Sie „Wie ist das Wetter in Seattle“ ein.
3. Doppelklicken Sie auf „Seattle“, und wählen Sie „Stadt“ aus.
    - Dadurch wird der Eintrag als Entität „Stadt“ gekennzeichnet.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Nun befindet sich „Seattle“ in der Entität „Stadt“. 
6. Wählen Sie „Das Wetter in $city ist wahrscheinlich sonnig“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

Sehen wir uns nun an, was passiert, wenn Sie etwas semantisch Ähnliches äußern:

1. Klicken Sie auf „Neue Aktion“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hilfe“ ein.
3. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
    - Die Werte für die beiden möglichen Antworten liegen sehr nahe beieinander. Das zeigt, dass das Modell die Grenze zwischen den beiden Aktionen nicht genau bestimmen kann.
6. Klicken Sie auf „Training abbrechen“ und auf „Bestätigen“.

![](../media/tutorial8_closescores.png)

In diesem Fall wäre es nützlich, alternative Eingaben für Dialoge hinzuzufügen. Sie können sie beim Training hinzufügen. Außerdem können Sie zurückkehren und diese später hinzufügen.

2. Klicken Sie auf „Was kannst du tun?“ in den Trainingsdialogen.
2. Klicken Sie im Dialog auf „Was kannst du tun?“, um die Option auszuwählen.
    1. Geben Sie im rechten Bereich unter „Entitätserkennung“ im Feld „Alternative Eingabe hinzufügen“ einige Alternativen ein:
    1. Geben Sie „Welche Möglichkeiten hab ich?“ ein.
    2. Geben Sie „Möglichkeiten aufzeigen“ ein.
    3. Geben Sie „Hilfe“ ein.
    1. Klicken Sie auf „Änderungen übermitteln“.


![](../media/tutorial8_helpalternates.png)

2. Klicken Sie auf „Wie ist das Wetter in Seattle“.
    1. Geben Sie im Feld „Alternative Eingabe hinzufügen“ „Vorhersage für Seattle“ ein.
    2. Doppelklicken Sie auf „Seattle“, und wählen Sie „Stadt“ aus. Die Entitäten für alternative Eingaben müssen verfügbar und die Menge der Entitäten identisch sein. Der Inhalt der Entitäten darf sich unterscheiden.
    3. Geben Sie im Feld „Alternative Eingabe hinzufügen“ „Wird es heute in Denver regnen“ ein.
    4. Klicken Sie auf „Denver“, und wählen Sie „Stadt“ aus.
    5. Klicken Sie auf „Änderungen übermitteln“ und auf „Fertig“.


Fügen Sie dem ersten Dialog eine alternative Eingabe hinzu:

1. Klicken Sie auf „Trainingsdialoge“.
2. Klicken Sie auf den Dialog, der mit „Wie ist das Wetter“ beginnt.
2. Klicken Sie zum Auswählen im linken Bereich auf „Wie ist das Wetter“:
    1. Geben Sie im Feld „Alternative Eingabe hinzufügen“ „Wettervorhersage“ ein.
    2. Geben Sie „Wird es regnen?“ ein.
    3. Klicken Sie auf „Änderungen übermitteln“.
4. Klicken Sie zum Auswählen im linken Bereich auf „Denver“:
    1. Geben Sie im Feld „Alternative Eingabe hinzufügen“ „für Denver“ ein.
    2. Geben Sie „Vorhersage für Austin“ ein.
        - Der vollständige Ausdruck wird hervorgehoben. Klicken Sie auf den Ausdruck und auf das rote „X“. Wählen Sie „Austin“ aus, und klicken Sie auf „Stadt“.
        - Klicken Sie auf „Änderungen übermitteln“.
    1. Klicken Sie auf „Fertig“. Daraufhin wird das Modell erneut trainiert.

![](../media/tutorial8_altcities.png)

Probieren Sie die folgenden Varianten aus:

1. Klicken Sie auf „Neuer Trainingsdialog“.
2. Geben Sie „Welche Funktionen hast du“ ein.
3. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
    - Die Werte sind jetzt eindeutiger in Hinblick auf die nächste Aktion. Das zeigt die Sicherheit des Modells.
2. Wählen Sie „Nach dem Wetter fragen“ aus.
6. Klicken Sie auf „Training abgeschlossen“.

Sie haben gelernt, wie Sie alternative Eingaben für mögliche Varianten von Benutzeräußerungen verwenden. So müssen Sie nicht zahlreiche ähnliche Dialoge erstellen, sondern können ähnliche Varianten in einem einzelnen Dialog zusammenfassen, und dort die möglichen Äußerungen auflisten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Protokollieren von Dialogen](./9-log-dialogs.md)

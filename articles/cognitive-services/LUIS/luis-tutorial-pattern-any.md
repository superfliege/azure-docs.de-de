---
title: 'Tutorial 5: Entität „Pattern.any“ für Freiformtext'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die Entität „pattern.any“, um Daten aus Äußerungen zu extrahieren, in denen die Äußerungen wohlgeformt sind und das Ende der Daten leicht mit den verbleibenden Wörtern der Äußerung zu verwechseln ist.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: ee5ba6783a50ad50489a20aa662f17af2ca4ae6a
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52421996"
---
# <a name="tutorial-5-extract-free-form-data"></a>Tutorial 5: Extrahieren von Freiformdaten

In diesem Tutorial verwenden Sie die Entität „pattern.any“, um Daten aus Äußerungen zu extrahieren, in denen die Äußerungen wohlgeformt sind und das Ende der Daten leicht mit den verbleibenden Wörtern der Äußerung zu verwechseln ist. 

Mit der Entität „pattern.any“ können Sie Freiformdaten finden, bei denen die Formulierung der Entität erschwert, das Ende der Entität vom Rest der Äußerung zu unterscheiden. 

Diese Human Resources-App hilft Mitarbeitern, Unternehmensformulare zu finden. 

|Äußerung|
|--|
|Wo ist **HRF-123456**?|
|Wer hat **HRF-123234** erstellt?|
|Ist **HRF-456098** in Französisch veröffentlicht?|

Jedes Formular verfügt jedoch sowohl über einen formatierten Name, der in der obigen Tabelle verwendet wird, als auch einen Anzeigenamen, z.B. `Request relocation from employee new to the company 2018 version 5`. 

Äußerungen mit dem Anzeigenamen sehen folgendermaßen aus:

|Äußerung|
|--|
|Wo ist **Request relocation from employee new to the company 2018 version 5**?|
|Wer hat **Request relocation from employee new to the company 2018 version 5** erstellt?|
|Wurde **Request relocation from employee new to the company 2018 version 5** in Französisch veröffentlicht?|

Die unterschiedlichen Längen enthalten Wörter, die es LUIS erschweren, das Ende von Entitäten zu erkennen. Mit einer Pattern.any-Entität in einem Muster können Sie den Anfang und das Ende des Formularnamens angeben, damit LUIS diesen ordnungsgemäß extrahiert.

|Beispiel für eine Vorlagenäußerung|
|--|
|Wo ist {FormName}[?]|
|Wer hat {FormName} erstellt[?]|
|Wurde {FormName} in Französisch veröffentlicht[?]|

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen von Beispieläußerungen zu einer vorhandenen Entität
> * Erstellen einer Pattern.any-Entität
> * Erstellen eines Musters
> * Trainieren
> * Testen des neuen Musters

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-roles-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `patt-any`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="add-example-utterances"></a>Hinzufügen von Beispieläußerungen 
Entfernen Sie die vordefinierte keyPhrase-Entität, wenn es schwierig ist, die FormName-Entität zu erstellen und zu bezeichnen. 

1. Wählen Sie **Erstellen** im oberen Navigationsbereich und dann **Absichten** im linken Navigationsbereich aus.

2. Wählen Sie **FindForm** in der Absichtenliste aus.

3. Fügen Sie einige Beispieläußerungen hinzu:

    |Beispieläußerung|
    |--|
    |Wo ist das Formular **What to do when a fire breaks out in the Lab**, und wer muss es unterzeichnen, nachdem ich es gelesen habe?|
    |Wo ist **Request relocation from employee new to the company** auf dem Server?|
    |Wer hat **Health and wellness requests on the main campus** erstellt, und was ist die aktuelle Version?|
    |Ich suche das Formular mit dem Namen **Office move request including physical assets**. |

    Ohne eine Pattern.any-Entität wäre es für LUIS schwierig, zu verstehen, wo der Titel des Formulars endet, weil viele Variationen des Formularnamens vorliegen.

## <a name="create-a-patternany-entity"></a>Erstellen einer Pattern.any-Entität
Die Pattern.any-Entität extrahiert Entitäten unterschiedlicher Länge. Sie funktioniert nur in einem Muster, da das Muster Anfang und Ende der Entität kennzeichnet. Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](luis-concept-patterns.md#explicit-lists), um dieses Problem zu beheben. 

1. Wählen Sie im linken Navigationsbereich **Entitäten** aus.

2. Wählen Sie **Neue Entität erstellen** aus, geben Sie den Namen `FormName` ein, und wählen Sie **Pattern.any** als Typ aus. Wählen Sie **Fertig**aus. 

    Sie können die Entität in der Absicht nicht bezeichnen, weil eine Pattern.any-Entität nur in einem Muster gültig ist. 

    Wenn Sie möchten, dass die extrahierten Daten weitere Entitäten einschließen, z.B. „number“ oder datetimeV2, müssen Sie eine zusammengesetzte Entität erstellen, die Pattern.any als auch „number“ und datetimeV2 enthält.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Hinzufügen eines Muster, das Pattern.any verwendet

1. Wählen Sie im linken Navigationsbereich **Muster** aus.

2. Wählen Sie die Absicht **FindForm** aus.

3. Geben Sie die folgenden Vorlagenäußerungen ein, die die neue Entität verwenden:

    |Vorlagenäußerungen|
    |--|
    |Wo ist das Formular ["]{FormName}["], und wer muss es unterzeichnen, nachdem ich es gelesen habe[?]|
    |Wo ist ["]{FormName}["] auf dem Server[?]|
    |Wer hat ["]{FormName}["] erstellt, und was ist die aktuelle Version[?]|
    |Ich suche das Formular mit dem Namen ["]{FormName}["][.]|

    Wenn Sie Variationen des Formulars wie z.B. einfache Anführungszeichen statt doppelter Anführungszeichen oder einen Punkt anstelle eines Fragezeichens berücksichtigen möchten, erstellen Sie für jede Variation ein neues Muster.

4. Wenn Sie die keyPhrase-Entität entfernt haben, fügen Sie sie wieder der App hinzu. 

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testen des neuen Musters für das Extrahieren von Freiformdaten
1. Wählen Sie auf der oberen Leiste **Test** aus, um den Testbereich zu öffnen. 

2. Geben Sie die folgende Äußerung ein: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Wählen Sie unter dem Ergebnis **Überprüfen** aus, um die Testergebnisse für die Entität und die Absicht anzuzeigen.

    Zuerst wird die Entität `FormName` gefunden, und danach das Muster, das die Absicht angibt. Wenn bei einem Testergebnis die Entitäten nicht erkannt wurden und damit das Muster nicht gefunden wurde, müssen Sie der Absicht (nicht dem Muster) weitere Beispieläußerungen hinzufügen.

4. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden einer vorhandenen Absicht Beispieläußerungen hinzugefügt und dann eine neue Pattern.any für den Formularnamen erstellt. Anschließend erstellte das Tutorial ein Muster für die vorhandene Absicht mit den neuen Beispieläußerungen und der Entität. Durch interaktives Testen wurde gezeigt, dass das Muster und seine Absicht vorhergesagt wurden, weil die Entität gefunden wurde. 

> [!div class="nextstepaction"]
> [Verwenden von Rollen mit einem Muster](luis-tutorial-pattern-roles.md)

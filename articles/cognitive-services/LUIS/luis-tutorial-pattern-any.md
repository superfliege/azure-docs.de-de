---
title: Tutorial zur Verwendung der pattern.any-Entität zur Verbesserung von LUIS-Vorhersagen – Azure | Microsoft-Dokumentation
titleSuffix: Cognitive Services
description: In diesem Tutorial verwenden Sie die pattern.any-Entität, um die Vorhersagen von LUIS zu Absichten und Entitäten zu verbessern.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 1587debecd82072c29d4caffc2b81629b1f52b0e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527363"
---
# <a name="tutorial-improve-app-with-patternany-entity"></a>Tutorial: Verbessern der App mit der pattern.any-Entität

In diesem Tutorial verwenden Sie die pattern.any-Entität, um die Vorhersagen zu Absichten und Entitäten zu verbessern.  

> [!div class="checklist"]
* Erfahren Sie, wann und wie Sie pattern.any verwenden
* Erstellen eines Muster, das pattern.any verwendet
* Überprüfen der Verbesserungen bei Vorhersagen

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personalabteilungs-App aus dem [Musterrollen](luis-tutorial-pattern-roles.md)-Tutorial verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-roles-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `patt-any`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="the-purpose-of-patternany"></a>Der Zweck von pattern.any
Mit der Entität pattern.any können Sie Freiformdaten finden, bei denen die Formulierung der Entität erschwert, das Ende der Entität vom Rest der Äußerung zu unterscheiden. 

Diese Human Resources-App hilft Mitarbeitern, Unternehmensformulare zu finden. Die Formulare wurden im [Tutorial zu regulären Ausdrücken](luis-quickstart-intents-regex-entity.md) hinzugefügt. Die Formularnamen aus diesem Tutorial verwendeten einen regulären Ausdruck, um einen korrekt formatierten Formularnamen zu extrahieren, z.B. den Formularnamen in Fettdruck in der folgenden Äußerungentabelle:

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

Die unterschiedlichen Längen enthalten Ausdrücke, die LUIS erschweren, das Ende von Entitäten zu erkennen. Mit einer Pattern.any-Entität in einem Muster können Sie den Anfang und das Ende des Formularnamens angeben, damit LUIS diesen ordnungsgemäß extrahiert.

**Muster ermöglichen Ihnen, weniger Beispieläußerungen bereitzustellen. Doch wenn die Entitäten nicht erkannt werden, stimmt das Muster nicht überein.**

## <a name="add-example-utterances-to-the-existing-intent-findform"></a>Hinzufügen von Beispieläußerungen zu der vorhandenen Absicht-FindForm 
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

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testen des neuen Musters für das Extrahieren von Freiformdaten
1. Wählen Sie auf der oberen Leiste **Test** aus, um den Testbereich zu öffnen. 

2. Geben Sie die folgende Äußerung ein: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Wählen Sie unter dem Ergebnis **Überprüfen** aus, um die Testergebnisse für die Entität und die Absicht anzuzeigen.

    Zuerst wird die Entität `FormName` gefunden, und danach das Muster, das die Absicht angibt. Wenn bei einem Testergebnis die Entitäten nicht erkannt wurden und damit das Muster nicht gefunden wurde, müssen Sie der Absicht (nicht dem Muster) weitere Beispieläußerungen hinzufügen.

4. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden von Rollen mit einem Muster](luis-tutorial-pattern-roles.md)
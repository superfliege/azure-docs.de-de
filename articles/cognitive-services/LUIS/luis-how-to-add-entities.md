---
title: Hinzufügen von Entitäten in LUIS-Apps
titleSuffix: Azure Cognitive Services
description: Hinzufügen von Entitäten (Schlüsseldaten in Ihrer Anwendungsdomäne) in Language Understanding-Apps (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.openlocfilehash: 178f4314f9ede86444ee60fd2a64f85dc283080b
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138530"
---
# <a name="create-entities-without-utterances"></a>Erstellen von Entitäten ohne Äußerungen

Die Entität stellt ein Wort oder einen Ausdruck innerhalb der Äußerung dar, die extrahiert werden soll. Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Entität stellt eine Klasse dar, einschließlich einer Sammlung ähnlicher Objekte (Orte, Dinge, Personen, Ereignisse oder Konzepte). Entitäten beschreiben Informationen mit Bezug zur Absicht. Manchmal sind sie auch für die Ausführung der Aufgabe Ihrer App wichtig. 

Sie können Entitäten vor dem Erstellen von Absichten oder in einem separaten Vorgang erstellen.

Sie können Entitäten in Ihrer LUIS-App über die **Entitätenlisten** auf der Seite **Entitäten** hinzufügen, bearbeiten oder löschen. LUIS verfügt über zwei Haupttypen von Entitäten: [vordefinierte Entitäten](luis-reference-prebuilt-entities.md) und Ihre eigenen [benutzerdefinierten Entitäten](luis-concept-entity-types.md#types-of-entities).

Nachdem eine Entität erstellt wurde, kann sie auf der Seite mit den Details zur **Absicht** in der Beispieläußerung einer Absicht markiert werden. 

## <a name="add-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität

Einer Anwendung hinzugefügte allgemeine vordefinierte Entitäten sind *number* und *datetimeV2*. 

1. Wählen Sie in Ihrer App im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten**.
 
1. Wählen Sie auf der Seite **Entitäten** die Option **Vordefinierte Entitäten hinzufügen**.

1. Wählen Sie im Dialogfeld **Vordefinierte Entitäten hinzufügen** die vordefinierten Entitäten **number** und **datetimeV2** aus. Wählen Sie dann **Fertig** aus.

    ![Screenshot des Dialogfelds „Vordefinierte Entität hinzufügen“](./media/add-entities/list-of-prebuilt-entities.png)

## <a name="add-simple-entities"></a>Hinzufügen einfacher Entitäten

Eine einfache Entität beschreibt ein einzelnes Konzept. Verwenden Sie das folgende Verfahren zum Erstellen einer Entität, mit der die Namen von Unternehmensabteilungen extrahiert werden, z.B. *Personalwesen* oder *Operativer Betrieb*.   

1. Wählen Sie in Ihrer App den Abschnitt **Build**, im linken Bereich die Option **Entitäten** und dann die Option **Neue Entität erstellen**.

1. Geben Sie im Popupdialogfeld das Wort `Location` in das Feld **Entitätsname** ein, und wählen Sie in der Liste **Entitätstyp** die Option **Einfach** aus. Wählen Sie anschließend **Fertig**.

    Navigieren Sie nach dem Erstellen dieser Entität zu allen Absichten mit Beispieläußerungen, die die Entität enthalten. Wählen Sie den Text in der Beispieläußerung aus, und kennzeichnen Sie ihn als Entität. 

    Normalerweise wird eine [Ausdrucksliste](luis-concept-feature.md) verwendet, um das Signal einer einfachen Entität zu verstärken.

## <a name="add-regular-expression-entities"></a>Hinzufügen von Entitäten des Typs „Regulärer Ausdruck“

Eine Entität des Typs „Regulärer Ausdruck“ dient zum Abrufen von Daten aus der Äußerung basierend auf einem regulären Ausdruck, den Sie bereitstellen. 

1. Wählen Sie in Ihrer App im linken Navigationsbereich **Entitäten** und dann **Neue Entität erstellen** aus.

1. Geben Sie im Popupdialogfeld `Human resources form name` in das Feld **Entitätsname** ein, wählen Sie **Regulärer Ausdruck** in der Liste **Entitätstyp** aus, geben Sie den regulären Ausdruck `hrf-[0-9]{6}` ein, und wählen Sie dann **Fertig** aus. 

    Dieser reguläre Ausdruck (Literal) enthält die Zeichen `hrf-` und dann sechs Ziffern, um eine Formularnummer für ein Formular aus dem Personalwesen zu bilden.

## <a name="add-hierarchical-entities"></a>Hinzufügen hierarchischer Entitäten

Eine hierarchische Entität ist eine Kategorie von im Kontext erlernten und konzeptionell verwandten Entitäten. Die Entität im folgenden Beispiel enthält Abflug- und Zielorte. 

In der Äußerung `Move John Smith from Seattle to Cairo` ist Seattle der Abflug- und Kairo der Zielort. Die Orte unterscheiden sich durch den Kontext und werden aufgrund der Wortstellung und Wortwahl in der Äußerung erlernt.

Um hierarchische Entitäten hinzuzufügen, führen Sie die folgenden Schritte aus: 

1. Wählen Sie in Ihrer App im linken Navigationsbereich **Entitäten** und dann **Neue Entität erstellen** aus.

1. Geben Sie im Popupdialogfeld `Location` in das Feld **Entitätsname** ein, und wählen Sie dann **Hierarchisch** in der Liste **Entitätstyp** aus.

    ![Hinzufügen einer Entität vom Typ „Hierarchisch“](./media/add-entities/hier-location-entity-creation.png)

1. Wählen Sie **Untergeordnetes Element hinzufügen**, und geben Sie im ersten Feld **Untergeordnete Entität** dann `Origin` ein. 

1. Wählen Sie **Untergeordnetes Element hinzufügen**, und geben Sie im zweiten Feld **Untergeordnete Entität** dann `Destination` ein. Wählen Sie **Fertig**aus.

    >[!CAUTION]
    >Die Namen untergeordneter Entitäten müssen für alle Entitäten einer einzelnen App eindeutig sein. Zwei unterschiedliche hierarchische Entitäten dürfen keine untergeordneten Elemente mit dem gleichen Namen enthalten. 

    Navigieren Sie nach dem Erstellen dieser Entität zu allen Absichten mit Beispieläußerungen, die die Entität enthalten. Wählen Sie den Text in der Beispieläußerung aus, und kennzeichnen Sie ihn als Entität. 

## <a name="add-composite-entities"></a>Hinzufügen einer zusammengesetzten Entität

Sie können Beziehungen zwischen Entitäten unterschiedlichen Typs durch das Erstellen einer zusammengesetzten Entität definieren. Im folgenden Beispiel enthält die Entität einen regulären Ausdruck und eine vordefinierte Entität mit einem Namen.  

In der Äußerung `Send hrf-123456 to John Smith` wird die Zeichenfolge `hrf-123456` mit einem [regulären Ausdruck](#add-regular-expression-entities) des Personalwesens abgeglichen, und `John Smith` wird mit der vordefinierten Entität „personName“ extrahiert. Jede Entität ist Teil einer größeren übergeordneten Entität. 

1. Wählen Sie in Ihrer App im linken Navigationsbereich des Abschnitts **Build** die Option **Entitäten** und dann **Vordefinierte Entität hinzufügen**.

1. Fügen Sie die vordefinierte Entität **PersonName** hinzu. Anweisungen hierzu finden Sie unter [Hinzufügen einer vordefinierten Entität](#add-prebuilt-entity). 

1. Wählen Sie im linken Navigationsbereich **Entitäten** und dann **Neue Entität erstellen** aus.

1. Geben Sie im Popupdialogfeld die Zeichenfolge `SendHrForm` in das Feld **Entitätsname** ein, und wählen Sie in der Liste **Entitätstyp** dann die Option **Zusammengesetzt** aus.

1. Wählen Sie **Untergeordnetes Element hinzufügen** aus, um ein neues untergeordnetes Element hinzuzufügen.

1. Wählen Sie im ersten Feld **Untergeordnete Entität** die Entität **number** in der Liste aus.

1. Wählen Sie im zweiten Feld **Untergeordnete Entität** in der Liste die Entität **Human resources form name** (Name für Personalwesenformular) aus. 

1. Wählen Sie **Fertig**aus.

## <a name="add-patternany-entities"></a>Hinzufügen der „Pattern.any“-Entitäten

[Pattern.Any](luis-concept-entity-types.md)-Entitäten sind nur in [Mustern](luis-how-to-model-intent-pattern.md) gültig, nicht in Absichten. Mit diesem Entitätstyp kann LUIS das Ende von Entitäten unterschiedlicher Länge und Wortwahl finden. Da diese Entität in einem Muster verwendet wird, weiß LUIS, wo sich das Ende der Entität in der Äußerungsvorlage befindet.

Wenn eine App eine `FindHumanResourcesForm`-Absicht hat, kann ein Konflikt zwischen dem extrahierten Formulartitel und der Absichtsvorhersage auftreten. Um zu verdeutlichen, welche Wörter der Formulartitel enthält, verwenden Sie eine Pattern.any-Entität in einem Muster. Die LUIS-Vorhersage beginnt mit der Äußerung. Zunächst wird die Äußerung überprüft und mit Entitäten abgeglichen. Wenn Entitäten gefunden werden, wird das Muster überprüft und abgeglichen. 

In der Äußerung `Where is Request relocation from employee new to the company on the server?` ist der Formulartitel problematisch, da aus dem Kontext nicht hervorgeht, wo der Titel endet und wo der restliche Teil der Äußerung beginnt. Titel können eine beliebigen Wortreihenfolge aufweisen und beispielsweise ein einzelnes Wort, komplexe Ausdrücke mit Satzzeichen und eine unsinnige Reihenfolge von Wörtern enthalten. Ein Muster ermöglicht Ihnen, eine Entität zu erstellen, wobei die vollständige und genaue Entität extrahiert werden kann. Sobald der Titel gefunden wurde, wird die `FindHumanResourcesForm`-Absicht vorhergesagt, da dies die Absicht für das Muster ist.

1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und dann **Neue Entität erstellen**.

1. Geben Sie im Dialogfeld **Entität hinzufügen** in das Feld „Entitätsname“ den Namen „HumanResourcesFormTitle“ ein, und wählen Sie **Pattern.any** als **Entitätstyp** aus.

    Um die pattern.any-Entität zu verwenden, fügen Sie auf der Seite **Muster** im Abschnitt **Improve app performance** (App-Leistung verbessern) mit der richtigen Syntax mit geschweiften Klammern ein Muster ein, z.B. `Where is **{HumanResourcesFormTitle}** on the server?`.

    Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](luis-concept-patterns.md#explicit-lists), um dieses Problem zu beheben. 

## <a name="add-a-role-to-pattern-based-entity"></a>Hinzufügen einer Rolle zur musterbasierten Entität

Eine Rolle ist ein benannter Untertyp einer Entität, basierend auf dem Kontext. Dies ist vergleichbar mit einer [hierarchischen](#add-hierarchical-entities) Entität, aber Rollen werden nur in [Mustern](luis-how-to-model-intent-pattern.md) verwendet. 

Wenn dasselbe Beispiel wie bei der hierarchischen Entität mit Ausgangs- und Zielorten verwendet wird, besteht der Unterschied darin, dass eine Rolle als Ausgangsort (origin) und nicht als hierarchisches untergeordnetes Element bezeichnet wird. 

Die Syntax für eine Rolle ist **{Entitätsname:Rollenname}**, wobei auf den Entitätsnamen ein Doppelpunkt und dann der Rollenname folgt. Beispiel: `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten**.

1. Wählen Sie **Neue Entität erstellen** aus. Geben Sie den Namen `LocationUsingRoles` ein. Wählen Sie den Typ **Einfach** und dann **Fertig** aus. 

1. Wählen Sie im linken Bereich die Option **Entitäten** und dann die neue Entität **LocationUsingRoles** aus, die im vorherigen Schritt erstellt wurde.

1. Geben Sie im Textfeld **Rollenname** den Namen der Rolle `Origin` ein, und drücken Sie die EINGABETASTE. Fügen Sie eine zweite Rolle namens `Destination` hinzu. 

    ![Screenshot vom Hinzufügen der Rolle „Origin“ zur Entität „Location“](./media/add-entities/roles-enter-role-name-text.png)

## <a name="add-list-entities"></a>Hinzufügen von Listenentitäten

Listenentitäten stellen eine feste, abgeschlossene Gruppe mit verwandten Wörtern dar. 

Für eine Personal-App können Sie eine Liste mit allen Abteilungen und den Synonymen für die Abteilungen verwenden. Sie müssen nicht alle Werte kennen, wenn Sie die Entität erstellen. Sie können weitere Werte hinzufügen, nachdem Sie wirkliche Benutzeräußerungen mit Synonymen überprüft haben.

1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und dann **Neue Entität erstellen**.

1. Geben Sie im Dialogfeld **Entität hinzufügen** `Department` in das Feld **Entitätsname** ein, und wählen Sie **Liste** als **Entitätstyp** aus. Wählen Sie **Fertig**aus.
  
1. Auf der Listenentitätsseite können Sie normalisierte Namen hinzufügen. Geben Sie im Textfeld **Werte** einen Abteilungsnamen für eine Liste ein, z.B. `HumanResources`, und drücken Sie anschließend auf der Tastatur die EINGABETASTE. 

1. Geben Sie rechts neben dem normalisierten Wert die Synonyme ein, und drücken Sie nach jedem Element die EINGABETASTE.

1. Wenn Sie weitere normalisierte Elemente für die Liste wünschen, wählen Sie **Empfehlen** aus, um Optionen aus dem [semantischen Wörterbuch](luis-glossary.md#semantic-dictionary) anzuzeigen.

    ![Screenshot vom Hinzufügen der Rolle „Origin“ zur Entität „Location“](./media/add-entities/hr-list-2.png)


1. Wählen Sie ein Element in der empfohlenen Liste aus, um es als normalisierten Wert hinzuzufügen, oder wählen Sie **Alle hinzufügen** aus, um alle Elemente hinzuzufügen. 
    Sie können Werte in eine vorhandene Listenentität importieren, indem Sie das folgende JSON-Format verwenden:

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```


## <a name="change-entity-type"></a>Ändern des Entitätstyps

LUIS erlaubt Ihnen nicht, den Entitätstyps zu ändern, da die Anwendung nicht weiß, was zum Erstellen der Entität hinzugefügt oder entfernt werden muss. Um den Typ zu ändern, sollten Sie besser eine neue Entität des richtigen Typs mit einem etwas anderen Namen erstellen. Sobald die Entität erstellt ist, entfernen Sie in jeder Äußerung den alten bezeichnenden Entitätsnamen, und fügen Sie den neuen Entitätsnamen hinzu. Sobald alle Äußerungen ihre neuen Bezeichnungen erhalten haben, löschen Sie die alte Entität. 

## <a name="create-a-pattern-from-an-utterance"></a>Erstellen eines Musters aus einer Äußerung

Weitere Informationen finden Sie unter [Hinzufügen eines Musters aus einer vorhandenen Äußerung auf der Seite der Absicht oder der Entität](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="train-your-app-after-changing-model-with-entities"></a>Trainieren Ihrer App nach dem Ändern des Modells mit Entitäten

Nachdem Sie Entitäten hinzugefügt, bearbeitet oder entfernt haben, [trainieren](luis-how-to-train.md) und [veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App, damit die Änderungen auf Endpunktabfragen angewandt werden. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu vordefinierten Entitäten finden Sie unter dem Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). 

Weitere Informationen zur Anzeige der Entität in der Antwort auf die JSON-Endpunktabfrage finden Sie unter [Extrahieren von Daten](luis-concept-data-extraction.md).

Wenn Sie Absichten, Äußerungen und Entitäten hinzugefügt haben, verfügen Sie über eine einfache LUIS-App. Erfahren Sie, wie Sie Ihre App [trainieren](luis-how-to-train.md), [testen](luis-interactive-test.md) und [veröffentlichen](luis-how-to-publish-app.md) können.
 

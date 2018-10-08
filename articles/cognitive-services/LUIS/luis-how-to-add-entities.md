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
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: e82955da24e127e5536c2e40ad2cccf07c5fa173
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032001"
---
# <a name="manage-entities"></a>Verwalten von Entitäten
Nachdem Sie die [Absichten](luis-concept-intent.md) Ihrer App identifiziert haben, müssen Sie [Beispieläußerungen](luis-concept-utterance.md) mit [Entitäten](luis-concept-entity-types.md) bezeichnen. Entitäten sind die wichtigen Teile eines Befehls oder einer Frage und können entscheidend dafür sein, dass Ihre Client-App ihre Aufgabe ausführt. 

Sie können Entitäten in Ihrer App über die **Entitätenlisten** auf der Seite **Entitäten** hinzufügen, bearbeiten oder löschen. LUIS bietet zwei Typen von Entitäten: [vordefinierte Entitäten](luis-reference-prebuilt-entities.md) und Ihre eigenen benutzerdefinierten Entitäten.

Die folgenden Abschnitte sind nur vom Abschnitt **Erstellen** aus in einer LUIS-App verfügbar. Der Link **Erstellen** befindet sich in der oberen Navigationsleiste. Wählen Sie im Abschnitt **Erstellen** im linken Navigationsmenü **Entitäten** aus. Sobald eine Entität der Anwendung hinzugefügt ist, und es sich um eine durch maschinelles Lernen erworbene Entität handelt, können Sie in der Äußerung [die Entität bezeichnen](luis-how-to-add-example-utterances.md). Sobald die App trainiert und veröffentlicht ist, können Sie aus der Vorhersage [extrahierte](luis-concept-data-extraction.md) Entitätsdaten erhalten. 

## <a name="add-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität
Vordefinierte Entitäten werden im [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text)-Projekt definiert. Einer Anwendung hinzugefügte allgemeine vordefinierte Entitäten sind *number* und *datetimeV2*. 

1. Klicken Sie in Ihrer App im Abschnitt **Erstellen** im linken Bereich auf **Entitäten**.
 
2. Wählen Sie auf der Seite **Entitäten** die Option **Vordefinierte Entitäten verwalten** aus.

3. Wählen Sie im Dialogfeld **Vordefinierte Entitäten hinzufügen oder entfernen** die vordefinierten Entitäten **number** und **datetimeV2** aus. Wählen Sie dann **Fertig** aus.

    ![Screenshot des Dialogfelds „Vordefinierte Entität hinzufügen“](./media/add-entities/list-of-prebuilt-entities.png)

    Weitere Informationen zum Extrahieren der vordefinierten Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#prebuilt-entity-data).

## <a name="add-simple-entities"></a>Hinzufügen einfacher Entitäten
Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt. 

1. Klicken Sie in Ihrer App im Abschnitt **Erstellen** im linken Bereich auf **Entitäten**, und wählen Sie **Neue Entität erstellen** aus.

2. Geben Sie im Popupdialogfeld `Airline` in das Feld **Entitätsname** ein, und wählen Sie **Einfach** in der Liste **Entitätstyp** und dann **Fertig** aus.

    ![Screenshot des Dialogfelds für die Erstellung einer einfachen Airline-Entität](./media/add-entities/create-simple-airline-entity.png)

    Weitere Informationen zum Extrahieren der einfachen Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#simple-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-primary-and-secondary-data.md) für einfache Entitäten aus, um mehr über das Verwenden einer einfachen Entität zu erfahren.

## <a name="add-regular-expression-entities"></a>Hinzufügen von Entitäten des Typs „Regulärer Ausdruck“
Eine Entität des Typs „Regulärer Ausdruck“ dient zum Abrufen von Daten aus der Äußerung basierend auf einem regulären Ausdruck, den Sie bereitstellen. 

1. Wählen Sie in Ihrer App im linken Navigationsbereich **Entitäten** und dann **Neue Entität erstellen** aus.

2. Geben Sie im Popupdialogfeld `AirFrance Flight` in das Feld **Entitätsname** ein, wählen Sie **Regulärer Ausdruck** in der Liste **Entitätstyp** aus, geben Sie den regulären Ausdruck `AFR[0-9]{3,4}` ein, und wählen Sie dann **Fertig** aus. 

    Dieser reguläre Ausdruck „AirFrance Flight“ erwartet drei Zeichen, buchstäblich `AFR`, und dann 3 oder 4 Ziffern. Die Ziffern können beliebige Zahlen von 0 bis 9 sein. Der reguläre Ausdruck stimmt mit AirFrance-Flugnummern wie z.B. „AFR101“, „ARF1302“ und „AFR5006“ überein. Weitere Informationen zum Extrahieren der Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md).

    ![Bild des Dialogfelds zum Erstellen einer Entität des Typs „Regulärer Ausdruck“](./media/add-entities/regex-entity-create-dialog.png)

    Weitere Informationen zum Extrahieren der Entität des Typs „Regulärer Ausdruck“ aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#regular-expression-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intents-regex-entity.md) für Entitäten des Typs „Regulärer Ausdruck“ aus, um mehr über das Verwenden einer Entität des Typs „Regulärer Ausdruck“ zu erfahren.

## <a name="add-hierarchical-entities"></a>Hinzufügen hierarchischer Entitäten
Eine hierarchische Entität ist eine Kategorie von im Kontext erlernten und konzeptionell verwandten Entitäten. Die Entität im folgenden Beispiel enthält Abflug- und Zielorte. 

In der Äußerung `Book 2 tickets from Seattle to Cairo` ist Seattle der Abflug- und Kairo der Zielort. Die Orte unterscheiden sich durch den Kontext und werden aufgrund der Wortstellung und Wortwahl in der Äußerung erlernt.

Um hierarchische Entitäten hinzuzufügen, führen Sie die folgenden Schritte aus: 

1. Wählen Sie in Ihrer App im linken Navigationsbereich **Entitäten** und dann **Neue Entität erstellen** aus.

2. Geben Sie im Popupdialogfeld `Location` in das Feld **Entitätsname** ein, und wählen Sie dann **Hierarchisch** in der Liste **Entitätstyp** aus.

    ![Hinzufügen einer Entität vom Typ „Hierarchisch“](./media/add-entities/hier-location-entity-creation.png)

3. Wählen Sie **Untergeordnetes Element hinzufügen** aus, und geben Sie „Origin“ in das erste Feld **Untergeordnetes Element** ein. 

4. Wählen Sie **Untergeordnetes Element hinzufügen** aus, und geben Sie „Destination“ in das zweite Feld **Untergeordnetes Element** ein. Wählen Sie **Fertig**aus.
5. 
    >[!NOTE]
    >Um ein untergeordnetes Element zu löschen, wählen Sie das Papierkorbsymbol daneben aus.

    >[!CAUTION]
    >Die Namen untergeordneter Entitäten müssen für alle Entitäten einer einzelnen App eindeutig sein. Zwei unterschiedliche hierarchische Entitäten dürfen keine untergeordneten Elemente mit dem gleichen Namen enthalten. 

    Weitere Informationen dazu, wie Sie die hierarchische Entität aus der Antwort der JSON-Endpunktabfrage extrahieren, finden Sie unter [Datenextrahierung](luis-concept-data-extraction.md#hierarchical-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intent-and-hier-entity.md) für hierarchische Entitäten aus, um mehr über das Verwenden einer hierarchischen Entität zu erfahren.

## <a name="add-composite-entities"></a>Hinzufügen einer zusammengesetzten Entität
Sie können Beziehungen zwischen mehreren vorhandenen Entitäten durch das Erstellen einer zusammengesetzten Entität definieren. Die Entität im folgenden Beispiel enthält die Anzahl der Tickets, Abflug- und Zielorte. 

In der Äußerung `Book 2 tickets from Seattle to Cairo` entspricht die Zahl 2 einer vordefinierten Entität, Seattle ist der Abflug- und Kairo der Zielort. Nach Erstellen der zusammengesetzten Entität ist jede Entität Teil einer größeren, übergeordneten Entität.

1. Fügen Sie in Ihrer App die vordefinierte Entität **number** hinzu. Anweisungen hierzu finden Sie unter [Hinzufügen einer vordefinierten Entität](#add-prebuilt-entity). 

2. Fügen Sie die hierarchische Entität `Location` hinzu, einschließlich der Untertypen: `origin`, `destination`. Weitere Anweisungen finden Sie unter [Hinzufügen hierarchischer Entitäten](#add-hierarchical-entities). 

3. Wählen Sie im linken Navigationsbereich **Entitäten** und dann **Neue Entität erstellen** aus.

4. Geben Sie im Popupdialogfeld `TicketsOrder` in das Feld **Entitätsname** ein, und wählen Sie dann **Zusammengesetzt** in der Liste **Entitätstyp** aus.

5. Wählen Sie **Untergeordnetes Element hinzufügen** aus, um ein neues untergeordnetes Element hinzuzufügen.

6. Wählen Sie im ersten Feld **Untergeordnete Entität** die Entität **number** in der Liste aus.

7. Wählen Sie im zweiten Feld **Untergeordnete Entität** die Entität **Location::Origin** in der Liste aus. 

8. Wählen Sie im dritten Feld **Untergeordnete Entität** die Entität **Location::Destination** in der Liste aus. 

9. Wählen Sie **Fertig**aus.

    ![Abbildung des Dialogfelds zum Erstellen einer zusammengesetzten Entität](./media/add-entities/ticketsorder-composite-entity.png)

    >[!NOTE]
    >Um ein untergeordnetes Element zu löschen, wählen Sie das Papierkorbsymbol daneben aus.

    Weitere Informationen zum Extrahieren der zusammengesetzten Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#composite-entity-data). Probieren Sie das [Tutorial](luis-tutorial-composite-entity.md) für zusammengesetzte Entitäten aus, um mehr über das Verwenden einer zusammengesetzten Entität zu erfahren.


## <a name="add-patternany-entities"></a>Hinzufügen der „Pattern.any“-Entitäten
[Pattern.Any](luis-concept-entity-types.md)-Entitäten sind nur in [Mustern](luis-how-to-model-intent-pattern.md) gültig. Diese Entität kann LUIS helfen, das Ende von Entitäten unterschiedlicher Länge und Wortwahl zu finden. Da diese Entität in einem Muster verwendet wird, weiß LUIS, wo das Ende der Entität in der Äußerung ist.

Wenn eine App eine `FindBookInfo`-Absicht hat, kann ein Konflikt zwischen dem Titel des Buchs und der Absichtsvorhersage auftreten. Um zu verdeutlichen, welche Wörter der Buchtitel enthält, verwenden Sie eine Pattern.any-Entität in einem Muster. Die LUIS-Vorhersage beginnt mit der Äußerung. Zunächst wird die Äußerung überprüft und mit Entitäten abgeglichen. Wenn Entitäten gefunden werden, wird das Muster überprüft und abgeglichen. 

In der Äußerung `Who wrote the book Ask and when was it published?` ist der Buchtitel „Ask“ problematisch, da aus dem Kontext nicht hervorgeht, wo der Titel endet und der übrige Teil der Äußerung beginnt. Buchtitel können eine beliebigen Wortreihenfolge aufweisen, einschließlich eines einzelnen Worts, komplexer Ausdrücke mit Satzzeichen und unsinniger Reihenfolge der Wörter. Ein Muster ermöglicht Ihnen, eine Entität zu erstellen, wobei die vollständige und genaue Entität extrahiert werden kann. Sobald der Buchtitel gefunden ist, wird die `FindBookInfo`-Absicht vorhergesagt, die die Absicht für das Muster ist.

1. Klicken Sie in Ihrer App im Abschnitt **Erstellen** im linken Bereich auf **Entitäten**, und wählen Sie **Neue Entität erstellen** aus.

2. Geben Sie im Dialogfeld **Entität hinzufügen** `BookTitle` in das Feld **Entitätsname** ein, und wählen Sie **Pattern.any** als **Entitätstyp** aus.
 
    ![Screenshot: Erstellen einer pattern.any-Entität](./media/add-entities/create-pattern-any-entity.png)

    Um die pattern.any-Entität zu verwenden, fügen Sie auf der Seite **Muster** (im Abschnitt **Verbessern der App-Leistung**) mit der richtigen Syntax mit geschweiften Klammern ein Muster ein, z.B. „Wer ist der Autor von **{BookTitle}**?“.

    Weitere Informationen zum Extrahieren der Pattern.any-Entität aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#patternany-entity-data). Probieren Sie das [Muster](luis-tutorial-pattern.md)-Tutorial aus, um weitere Informationen zum Verwenden einer Pattern.any-Entität zu erhalten.

Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](luis-concept-patterns.md#explicit-lists), um dieses Problem zu beheben. 

## <a name="add-role-to-pattern-based-entity"></a>Hinzufügen von Rollen zu musterbasierten Entitäten
Eine Rolle ist ein benannter Untertyp einer Entität, basierend auf dem Kontext. Dies ist vergleichbar mit einer [hierarchischen](#add-hierarchical-entities) Entität, aber Rollen werden nur in [Mustern](luis-how-to-model-intent-pattern.md) verwendet. 

Ein Flugticket hat beispielsweise einen *Abflugort* und einen *Zielort*, aber beide sind Städte. LUIS ermittelt, dass beide Städte sind, und kann Abflug- und Zielort basierend auf dem Kontext von Wortreihenfolge und Wortwahl bestimmen. 

Die Syntax für eine Rolle ist **{Entitätsname:Rollenname}**, wobei auf den Entitätsnamen ein Doppelpunkt folgt, und darauf der Rollenname. Beispiel: „Ticket von {Location:Origin} nach {Location:Destination} buchen“.

1. Wählen Sie in Ihrer App im Abschnitt **Erstellen** im linken Bereich **Entitäten** aus.

2. Wählen Sie **Neue Entität erstellen** aus. Geben Sie den Namen `Location` ein. Wählen Sie den Typ **Einfach** und dann **Fertig** aus.

3. Wählen Sie im linken Bereich **Entitäten** und dann die in Schritt 2 erstellte neue Entität **Location** aus.

4. Geben Sie im Textfeld **Rollenname** den Namen der Rolle `Origin` ein, und drücken Sie die EINGABETASTE. Fügen Sie eine zweite Rolle namens `Destination` hinzu. Beispielsweise hat eine Flugreise einen Abflug- und einen Zielort. Die beiden Rollen sind „Origin“ und „Destination“.

    ![Screenshot vom Hinzufügen der Rolle „Origin“ zur Entität „Location“](./media/add-entities/roles-enter-role-name-text.png)

    Weitere Informationen zum Extrahieren von Rollen aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md). Probieren Sie das Muster-Tutorial aus, um weitere Informationen zum Verwenden einer Pattern.any-Entität zu erhalten.

## <a name="add-list-entities"></a>Hinzufügen von Listenentitäten
Listenentitäten stellen eine feste, abgeschlossene Gruppe verwandter Wörter in Ihrem System dar. 

Die Entität „drinks list“ (Getränkeliste) beispielsweise kann zwei normalisierte Werte aufweisen: Water und Soda Pop. Jeder normalisierte Name verfügt über Synonyme. Die Synonyme für Water lauten H20, gas, flat (H2O, sprudelig, still). Die Synonyme für Soda Pop lauten fruit, cola, ginger (Frucht, Cola, Ingwer). Sie müssen nicht alle Werte kennen, wenn Sie die Entität erstellen. Sie können weitere Werte hinzufügen, nachdem Sie wirkliche Benutzeräußerungen mit Synonymen überprüft haben.

|Normalisierter Name|Synonyme|
|--|--|
|Water|H20, gas, flat|
|Soda pop|Fruit, cola, ginger|

1. Klicken Sie in Ihrer App im Abschnitt **Erstellen** im linken Bereich auf **Entitäten**, und wählen Sie **Neue Entität erstellen** aus.

2. Geben Sie im Dialogfeld **Entität hinzufügen** `Drinks` in das Feld **Entitätsname** ein, und wählen Sie **Liste** als **Entitätstyp** aus. Wählen Sie **Fertig**aus.
 
    ![Abbildung des Dialogfelds zum Erstellen der Listenentität „Drinks“](./media/add-entities/menu-list-dialog.png)
  
3.  Auf der Listenentitätsseite können Sie normalisierte Namen hinzufügen. Geben Sie im Textfeld **Werte** ein Element für die Liste ein, z.B. `Water` für die Getränkeliste, und drücken Sie dann die EINGABETASTE. 

    ![Screenshot der Getränkelistenentität mit normalisiertem Wert „Water“ im Textfeld](./media/add-entities/entity-list-normalized-name.png)

4. Geben Sie rechts neben dem normalisierten Wert **Water** die Synonyme `h20`, `flat`, und `gas` ein, und drücken Sie nach jedem Element die EINGABETASTE.

    ![Screenshot der Getränkelistenentität, wobei die Synonymelemente für Wasser hervorgehoben sind](./media/add-entities/menu-list-synonyms.png)

5. Wenn Sie weitere normalisierte Elemente für die Liste wünschen, wählen Sie **Empfehlen** aus, um Optionen aus dem [semantischen Wörterbuch](luis-glossary.md#semantic-dictionary) anzuzeigen.

    ![Screenshot der Getränkelistenentität, wobei die empfohlenen Elemente hervorgehoben sind](./media/add-entities/entity-list-recommended-list.png)

6. Wählen Sie ein Element in der empfohlenen Liste aus, um es als normalisierten Wert hinzuzufügen, oder wählen Sie **Alle hinzufügen** aus, um alle Elemente hinzuzufügen. 

    ![Screenshot der Getränkelistenentität, wobei das empfohlene Element „beer“ und die Schaltfläche „Alle hinzufügen“ hervorgehoben sind](./media/add-entities/list-entity-add-suggestions.png)

    Weitere Informationen zum Extrahieren von Listenentitäten aus der Antwort der JSON-Endpunktabfrage finden Sie unter [Datenextraktion](luis-concept-data-extraction.md#list-entity-data). Probieren Sie den [Schnellstart](luis-quickstart-intent-and-list-entity.md) aus, um mehr über das Verwenden einer Listenentität zu erfahren.

## <a name="import-list-entity-values"></a>Importieren von Listenentitätwerten
Sie können Werte in eine vorhandene Listenentität importieren.

 1. Wählen Sie auf der Listenentitätsseite **Listen importieren** aus.

 2. Wählen Sie im Dialogfeld **Neue Einträge importieren** die Option **Datei auswählen** aus, und wählen Sie die JSON-Datei aus, die die Liste enthält.

    ![Screenshot des Popupdialogfelds „Einträge in Listenentität importieren“](./media/add-entities/menu-list-import-json-dialog-with-file.png)

    >[!NOTE]
    >LUIS importiert nur Dateien mit der Erweiterung JSON.

 3. Um Informationen zu der unterstützten Listensyntax im JSON-Format zu erfahren, wählen Sie **Informationen zu unterstützter Listensyntax** aus, um das Dialogfeld zu erweitern und ein Beispiel zulässiger Syntax anzuzeigen. Um das Dialogfeld zu reduzieren und die Syntax auszublenden, wählen Sie den Linktitel erneut aus.

 4. Wählen Sie **Fertig**aus.

    Ein gültiges JSON-Beispiel für eine Listenentität **Farben** wird im folgenden Code im JSON-Format dargestellt:

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

## <a name="edit-entity-name"></a>Bearbeiten des Entitätsnamens
1. Wählen Sie auf der Seite **Entitäten** die Entität in der Liste aus. Damit gelangen Sie auf die **Entität**-Seite.

2. Auf der **Entität**-Seite bearbeiten Sie den Namen der Entität, indem Sie das Bearbeitungssymbol neben dem Namen der Entität auswählen. Der Entitätstyp kann nicht bearbeitet werden. 

## <a name="delete-entity"></a>Löschen der Entität

Wählen Sie auf der **Entität**-Seite die Schaltfläche **Entität löschen** aus. Wählen Sie dann in der Bestätigungsmeldung **OK** aus, um den Löschvorgang zu bestätigen.
 
![Screenshot der Location-Entitätsseite, wobei die Schaltfläche „Entität löschen“ hervorgehoben ist](./media/add-entities/entity-delete.png)

>[!NOTE]
>* Beim Löschen einer hierarchischen Entität werden alle untergeordneten Entitäten gelöscht.
>* Beim Löschen einer zusammengesetzten Entität werden nur die Zusammensetzung gelöscht und die Zusammensetzungsbeziehung aufgehoben, aber nicht die Entitäten gelöscht, die sie bilden.

## <a name="changing-entity-type"></a>Ändern des Entitätstyps
LUIS erlaubt Ihnen nicht, den Entitätstyps zu ändern, da die Anwendung nicht weiß, was zum Erstellen der Entität hinzugefügt oder entfernt werden muss. Um den Typ zu ändern, sollten Sie besser eine neue Entität des richtigen Typs mit einem etwas anderen Namen erstellen. Sobald die Entität erstellt ist, entfernen Sie in jeder Äußerung den alten bezeichnenden Entitätsnamen, und fügen Sie den neuen Entitätsnamen hinzu. Sobald alle Äußerungen ihre neuen Bezeichnungen erhalten haben, löschen Sie die alte Entität. 

## <a name="create-a-pattern-from-an-utterance"></a>Erstellen eines Musters aus einer Äußerung
Weitere Informationen finden Sie unter [Hinzufügen eines Musters aus einer vorhandenen Äußerung auf der Seite der Absicht oder der Entität](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="search-utterances"></a>Durchsuchen von Äußerungen
Sie können Äußerungen mit dem Lupensymbol auf der Symbolleiste durchsuchen und filtern. 

## <a name="train-your-app-after-changing-model-with-entities"></a>Trainieren Ihrer App nach dem Ändern des Modells mit Entitäten
Nachdem Sie Entitäten hinzugefügt, bearbeitet oder entfernt haben, [trainieren](luis-how-to-train.md) und [veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App, damit die Änderungen auf Endpunktabfragen angewandt werden. 

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Absichten, Äußerungen und Entitäten hinzugefügt haben, verfügen Sie über eine einfache LUIS-App. Erfahren Sie, wie Sie Ihre App [trainieren](luis-how-to-train.md), [testen](luis-interactive-test.md) und [veröffentlichen](luis-how-to-publish-app.md) können.
 

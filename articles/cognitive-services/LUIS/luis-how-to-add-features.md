---
title: Hinzufügen von Features in LUIS-Anwendungen | Microsoft-Dokumentation
description: Verwenden Sie Language Understanding (LUIS), um App-Features hinzuzufügen, die die Erkennung oder Vorhersage von Absichten und Entitäten verbessern können, die Kategorien und Muster
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/30/2018
ms.author: diberry
ms.openlocfilehash: 5ec75436c7df5c08f5507794229bec1f9adb2804
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222952"
---
# <a name="use-features-to-improve-your-luis-apps-performance"></a>Verwenden von Features zum Verbessern der Leistung Ihrer LUIS-App  

Sie können Ihrer LUIS-App Features hinzufügen, um ihre Genauigkeit zu verbessern. Features unterstützen LUIS mit Hinweisen, dass bestimmte Wörter und Ausdrücke Teil einer Kategorie sind. Wenn LUIS lernt, ein Mitglied der Kategorie zu erkennen, kann LUIS die anderen auf ähnliche Weise behandeln.

## <a name="add-phrase-list"></a>Hinzufügen einer Begriffsliste

1. Öffnen Sie die App, indem Sie auf der Seite **Meine Apps** auf ihren Namen, dann auf **Erstellen** und im linken Bereich Ihrer App auf **Begriffslisten** klicken. 

    ![Navigation in der Begriffsliste](./media/luis-add-features/phrase-list-nav.png)

2. Klicken Sie auf der Seite **Begriffslisten** auf **Neue Begriffsliste erstellen**. 
 
    ![Erstellen einer neuen Begriffsliste](./media/luis-add-features/create-new-phrase-list.png)
    
3. Geben Sie im Dialogfeld **Begriffsliste hinzufügen** „Cities“ als Namen für die Begriffsliste ein. Geben Sie im Feld **Wert** die Werte der Begriffsliste ein. Sie können jeweils einen Wert oder eine Gruppe durch Kommas getrennter Werte eingeben und dann die **EINGABETASTE** drücken.

    ![Hinzufügen der Begriffsliste „Cities“](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS kann vorschlagen, verwandte Werte Ihrer Begriffsliste hinzuzufügen. Klicken Sie auf **Empfehlen**, um eine Gruppe von vorgeschlagenen Werte abzurufen, die semantisch mit den hinzugefügten Werten verwandt sind. Sie können auf die einzelnen vorgeschlagenen Werte klicken, oder auf **Alle hinzufügen**, um alle hinzuzufügen.

    ![Begriffsliste – vorgeschlagene Werte](./media/luis-add-features/related-values.png)

5. Klicken Sie auf **Diese Werte sind austauschbar**, wenn die hinzugefügten Begriffslistenwerte Alternativen sind, die austauschbar verwendet werden können.

    ![Begriffsliste – vorgeschlagene Werte](./media/luis-add-features/interchangeable.png)

6. Klicken Sie auf **Speichern**. Die Begriffsliste „Cities“ wird der Seite **Begriffslisten** hinzugefügt.

    ![Hinzugefügte Begriffsliste](./media/luis-add-features/phrase-list-cities.png)

## <a name="edit-phrase-list"></a>Bearbeiten einer Begriffsliste

Klicken Sie auf der Seite **Begriffslisten** auf den Namen der Begriffsliste. Nehmen Sie im nun geöffneten Dialogfeld **Begriffsliste bearbeiten** ggf. erforderliche Änderungen vor, und klicken Sie dann auf **Speichern**.

 ![Hinzugefügte Begriffsliste](./media/luis-add-features/edit-phrase-list.png)

## <a name="delete-phrase-list"></a>Löschen einer Begriffsliste 

Klicken Sie auf die Auslassungspunkte (***...***) am Ende der Zeile, und wählen Sie **Löschen** aus.

 ![Löschen der hinzugefügten Liste](./media/luis-add-features/delete-phrase-list.png)

## <a name="deactivate-phrase-list"></a>Deaktivieren der Begriffsliste 

Klicken Sie auf die Auslassungspunkte (***...***) am Ende der Zeile, und wählen Sie **Deaktivieren** aus.

 ![Deaktivieren der hinzugefügten Liste](./media/luis-add-features/deactivate-phrase-list.png)

## <a name="pattern-regular-expression-feature"></a>Musterfeature (regulärer Ausdruck) 
**Dieses Feature ist veraltet**. Neue Musterfeature können LUIS nicht hinzugefügt werden. Vorhandene Musterfeatures werden bis Mai 2018 unterstützt. Leisten Sie im [Recognizers-Text-Github-Repository](https://github.com/Microsoft/Recognizers-Text) Ihren Beitrag zu standardmäßigen regulären Ausdrücken von LUIS in PR-Übereinstimmung. 

## <a name="next-steps"></a>Nächste Schritte

Nach dem Hinzufügen, Bearbeiten, Löschen oder Deaktivieren einer Begriffsliste [trainieren und testen Sie die App](luis-interactive-test.md) erneut, um festzustellen, ob die Leistung besser wurde.

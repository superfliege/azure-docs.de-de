---
title: Begriffslisten zum Verbessern der Entitätserkennung
titleSuffix: Azure Cognitive Services
description: Verwenden Sie Language Understanding (LUIS), um App-Features hinzuzufügen, die die Erkennung oder Vorhersage von Absichten und Entitäten verbessern können, die Kategorien und Muster
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 6831f67a33d114ca5c42ddacf8ef4de704e21711
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036955"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Verwenden von Begriffslisten, um das Signal von Wortlisten zu verstärken

Sie können Ihrer LUIS-App Features hinzufügen, um ihre Genauigkeit zu verbessern. Features unterstützen LUIS mit Hinweisen, dass bestimmte Wörter und Ausdrücke Teil eines App-Domänenvokabulars sind. 

## <a name="add-phrase-list"></a>Hinzufügen einer Begriffsliste

1. Öffnen Sie die App, indem Sie auf der Seite **Meine Apps** auf ihren Namen, dann auf **Erstellen** und im linken Bereich Ihrer App auf **Begriffslisten** klicken. 

2. Klicken Sie auf der Seite **Begriffslisten** auf **Neue Begriffsliste erstellen**. 
 
3. Geben Sie im Dialogfeld **Begriffsliste hinzufügen** „Cities“ als Namen für die Begriffsliste ein. Geben Sie im Feld **Wert** die Werte der Begriffsliste ein. Sie können jeweils einen Wert oder eine Gruppe durch Kommas getrennter Werte eingeben und dann die **EINGABETASTE** drücken.

    ![Hinzufügen der Begriffsliste „Cities“](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS kann vorschlagen, verwandte Werte Ihrer Begriffsliste hinzuzufügen. Klicken Sie auf **Empfehlen**, um eine Gruppe von vorgeschlagenen Werte abzurufen, die semantisch mit den hinzugefügten Werten verwandt sind. Sie können auf die einzelnen vorgeschlagenen Werte klicken, oder auf **Alle hinzufügen**, um alle hinzuzufügen.

    ![Begriffsliste – vorgeschlagene Werte](./media/luis-add-features/related-values.png)

5. Klicken Sie auf **Diese Werte sind austauschbar**, wenn die hinzugefügten Begriffslistenwerte Alternativen sind, die austauschbar verwendet werden können.

    ![Begriffsliste – vorgeschlagene Werte](./media/luis-add-features/interchangeable.png)

6. Klicken Sie auf **Speichern**. Die Begriffsliste „Cities“ wird der Seite **Begriffslisten** hinzugefügt.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Über die Schaltfläche mit den Auslassungspunkten (***...***) am Ende jeder Zeile einer Begriffsliste lassen sich Begriffslisten bearbeiten, löschen oder deaktivieren.

## <a name="pattern-regular-expression-feature"></a>Musterfeature (regulärer Ausdruck) 
**Dieses Feature ist veraltet**. Neue Musterfeature können LUIS nicht hinzugefügt werden. Vorhandene Musterfeatures werden bis Mai 2018 unterstützt. Leisten Sie im [Recognizers-Text-Github-Repository](https://github.com/Microsoft/Recognizers-Text) Ihren Beitrag zu standardmäßigen regulären Ausdrücken von LUIS in PR-Übereinstimmung. 

## <a name="next-steps"></a>Nächste Schritte

Nach dem Hinzufügen, Bearbeiten, Löschen oder Deaktivieren einer Begriffsliste [trainieren und testen Sie die App](luis-interactive-test.md) erneut, um festzustellen, ob die Leistung besser wurde.

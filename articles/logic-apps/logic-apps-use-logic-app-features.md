---
title: "Hinzufügen von Bedingungen und Workflows - Azure-Logik-Apps starten | Microsoft Docs"
description: "Steuern Sie, wie Workflows im Azure-Logik-Apps ausgeführt, durch Hinzufügen von bedingter Logik, Trigger, Aktionen und Parameter."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: e632c48ed31e82536db55a9c54438bece0c38fd4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="use-logic-apps-features"></a>Verwenden von Logik-App-features

In einem [Vorheriges Thema](../logic-apps/logic-apps-create-a-logic-app.md), Ihre erste Logik-app erstellen. Um Ihre Logik-app-Workflows zu steuern, können Sie unterschiedliche Pfade für die Logik-app ausführen und wie Daten in Arrays, Auflistungen und Batches verarbeitet angeben. Sie können diese Elemente in Ihrem Workflow der Logik-app einschließen:

* Bedingungen und [wechseln Anweisungen](../logic-apps/logic-apps-switch-case.md) können Ihre Logik-app führen Sie verschiedene Aktionen basierend auf, ob bestimmte Bedingungen erfüllt sind.

* [Schleifen](../logic-apps/logic-apps-loops-and-scopes.md) können Ihre Logik-app, die Schritte wiederholt auszuführen. Angenommen, Sie können Aktionen wiederholen über ein Array bei Verwendung einer **For_each** Schleife. Oder Sie können Aktionen wiederholen, bis eine Bedingung erfüllt ist, bei der Verwendung einer **bis** Schleife.

* [Bereiche](../logic-apps/logic-apps-loops-and-scopes.md) können Sie gruppieren Abfolge von Aktionen, z. B. um die Implementierung der Ausnahmebehandlung.

* [Rückgängigmachen einer Batchverarbeitung](../logic-apps/logic-apps-loops-and-scopes.md) können Ihre Logik-app starten getrennten Workflows für Elemente in einem Array, bei der Verwendung der **SplitOn** Befehl.

In diesem Thema werden andere Konzepte zum Erstellen von Logik-app:

* So bearbeiten Sie eine vorhandene Logik-app in der Codeansicht
* Optionen zum Starten eines Workflows

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Bedingungen: Führen Sie Schritte erst nach einer Bedingung erfüllen

Damit Ihre Logik-app, die Schritte nur ausführen, wenn Daten auf bestimmte Kriterien erfüllen, können Sie eine Bedingung hinzufügen, die Daten in den Workflow anhand bestimmter Felder oder Werte verglichen.

Angenommen Sie, Sie verfügen über eine Logik-app, die Sie zu viele e-Mails für Beiträge auf einer Website RSS-feed sendet. Sie können eine Bedingung hinzufügen, sodass Ihre Logik-app-e-Mail sendet nur, wenn Sie der neue Beitrag zu einer bestimmten Kategorie gehört.

1. In der [Azure-Portal](https://portal.azure.com), suchen und öffnen Sie die Logik-app in der Logik-App-Designer.

2. Fügen Sie eine Bedingung zum gewünschten Speicherort für den Workflow hinzu. 

   Um die Bedingung zwischen vorhandenen Schritten in der Logik-app-Workflow hinzufügen möchten, bewegen Sie den Zeiger auf den Pfeil, in dem Sie die Bedingung hinzufügen möchten. 
   Wählen Sie die **Pluszeichen** (**+**), wählen Sie dann **Hinzufügen einer Bedingung**. Beispiel:

   ![Fügen Sie Bedingung hinzu, um die Logik-app](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Wenn Sie eine Bedingung am Ende des aktuellen Workflows hinzufügen möchten, wechseln Sie zum unteren Rand der Logik-app, und wählen Sie **+ neuen Schritt**.

3. Definieren Sie nun die Bedingung ein. Geben Sie das Quellfeld, das Sie auswerten möchten, der den auszuführenden Vorgang, und das Ziel Feld oder Wert. Um vorhandene Felder die Bedingung hinzuzufügen, wählen Sie aus der **hinzufügen dynamischer Inhalte Liste**.

   Beispiel:

   ![Bearbeiten einer Bedingung im Standardmodus](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Hier ist die vollständige Bedingung:

   ![Vollständige Bedingung](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Um die Bedingung im Code zu definieren, wählen Sie **im erweiterten Modus bearbeiten**. Beispiel:
   > 
   > ![Bearbeiten einer Bedingung im code](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. Klicken Sie unter **Wenn Ja** und **Wenn Nein**, fügen Sie die Schritte zum Ausführen basierend auf, ob die Bedingung erfüllt ist.

   Beispiel:

   ![Bedingung mit "Ja" und keine Pfade](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Sie können die vorhandene Aktionen zu ziehen die **Wenn Ja** und **Wenn Nein** Pfade.

5. Wenn Sie fertig sind, speichern Sie Ihre Logik-app.

Jetzt erhalten Sie e-Mail-Nachrichten nur, wenn die Beiträge die Bedingung erfüllen.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Wiederholen Sie Aktionen über eine Liste mit forEach

Die foreach-Schleife gibt ein Array, um über eine Aktion zu wiederholen. Wenn es sich nicht um ein Array ist, schlägt fehl, der Datenfluss. Z. B. Wenn Sie action1, die ein Array von Nachrichten ausgibt, und jede Nachricht gesendet werden soll, zählen Sie dieser foreach-Anweisung in den Eigenschaften der Aktion:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-the-code-definition-for-a-logic-app"></a>Bearbeiten Sie die Codedefinition für eine Logik-app

Obwohl Sie die Logik-App-Designer verfügen, können Sie den Code, der eine Logik-app definiert direkt bearbeiten.

1. Wählen Sie in der Befehlsleiste **Codeansicht**.

    Ein vollständige-Editor wird geöffnet und zeigt die Definition, die Sie bearbeitet.

    ![In der Codeansicht](media/logic-apps-use-logic-app-features/codeview.png)

    In den Text-Editor können Sie kopieren und fügen Sie eine beliebige Anzahl von Aktionen innerhalb der gleichen Logik-app oder zwischen Logik-apps. 
    Sie können leicht hinzufügen oder entfernen Sie die gesamte Abschnitte aus der Definition und können Sie auch Definitionen für andere Benutzer freigeben.

2. Um Ihre Änderungen zu speichern, wählen Sie **speichern**.

## <a name="parameters"></a>Parameter

Einige Funktionen von Logik-Apps sind nur in der Codeansicht, z. B. Parameter verfügbar. Parameter erleichtern die Werte in der gesamten Logik-app wiederverwenden. Wenn eine e-Mail-Adresse vorhanden ist, die Sie in mehrere Aktionen verwenden möchten, sollten Sie diese e-Mail-Adresse als Parameter definieren.

Parameter sind gut für herausziehen Werte, die wahrscheinlich zu viel geändert werden. Sie sind besonders nützlich, wenn Sie Parameter in verschiedenen Umgebungen außer Kraft setzen müssen. Außerkraftsetzen der Parameter auf Grundlage der Umgebung finden Sie unter [Logik-app-Definitionen erstellen](../logic-apps/logic-apps-author-definitions.md) und [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/logic).

Dieses Beispiel zeigt, wie Sie Ihre vorhandene Logik-app aktualisieren, sodass Sie die Parameter für den Abfrageausdruck verwenden können.

1. Suchen Sie in der Codeansicht der `parameters : {}` -Objekt, und fügen eine `currentFeedUrl` Objekt:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Wechseln Sie zu der `When_a_feed-item_is_published` Aktion Suchen der `queries` Abschnitt, und Ersetzen Sie den Abfragewert mit:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    Sie können auch verwenden, um zwei oder mehr Zeichenfolgen zu verknüpfen, die `concat` Funktion. 
    Beispielsweise `"@concat('#',parameters('currentFeedUrl'))"` funktioniert genauso wie die oben beschrieben.

3.  Wenn Sie fertig sind, wählen Sie **Speichern** aus. 

    Jetzt können Sie die Website-RSS-feed durch Übergeben einer anderen URL durch Ändern der `currentFeedURL` Objekt.

Erfahren Sie mehr über [Gewusst wie: Erstellen von Logik-app-Definitionen](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Starten Sie die Logik-app-workflows

Sie haben verschiedene Optionen zum Starten des Workflows in Ihrer Logik-app definiert. Sie können einen Workflow bei Bedarf immer starten, der [Azure-Portal].

### <a name="recurrence-triggers"></a>Wiederholung Trigger

Ein Wiederholung-Trigger ausgeführt wird, in einem Intervall an, die Sie angeben. Wenn der Trigger bedingten Logik aufweist, bestimmt der Trigger an, ob der Workflow ausgeführt werden muss. Ein Trigger gibt an, der Workflow ausgeführt werden sollte, wird durch Zurückgeben einer `200` Statuscode. Wenn der Workflow nicht ausführen muss, gibt der Trigger ein `202` Statuscode.

### <a name="callback-using-rest-apis"></a>Rückruf mithilfe von REST-APIs

Zum Starten eines Workflows können Dienste einen Logik-app-Endpunkt aufrufen. Um diese Art von Logik-app bei Bedarf zu starten, wählen Sie **jetzt ausführen** in der Befehlszeile. Finden Sie unter [Starten des Workflows durch Aufrufen von Logik Anwendungsendpunkte als Trigger](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure-Portal]: https://portal.azure.com

## <a name="next-steps"></a>Nächste Schritte

* [Switch-Anweisungen](../logic-apps/logic-apps-switch-case.md) 
* [Schleifen, seine Bereiche und Rückgängigmachen einer Batchverarbeitung](../logic-apps/logic-apps-loops-and-scopes.md)
* [Autor Logik-app-Definitionen](../logic-apps/logic-apps-author-definitions.md)
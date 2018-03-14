---
title: "switch-Anweisungen – Ausführen von Schritten basierend auf bestimmten Werten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Ausführen verschiedener Schritte basierend auf den Werten von Objekten, Ausdrücken oder Token in Logik-Apps"
services: logic-apps
keywords: switch-Anweisung
author: ecfan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: e1f515189be8a5659af0f6c29b3fac0550abc9f9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="switch-statements-run-different-steps-based-on-specific-values-in-logic-apps"></a>switch-Anweisungen: Ausführen verschiedener Schritte basierend auf bestimmten Werten in Logik-Apps

Zum Ausführen verschiedener Schritte basierend auf den Werten eines Objekts, Ausdrucks oder Tokens verwenden Sie eine *switch*-Anweisung. Diese Struktur wertet das Objekt, den Ausdruck oder das Token aus, wählt den Fall, der dem Ergebnis entspricht, und führt Schritte nur für diesen Fall aus. Beim Ausführen der switch-Anweisung sollte nur ein Fall mit dem Ergebnis übereinstimmen.

Angenommen, Sie möchten, dass eine Logik-App verschiedene Schritte basierend auf einer Option ausführt, die in einer E-Mail ausgewählt ist. In diesem Beispiel überprüft die Logik-App den RSS-Feed einer Website auf neuen Inhalt. Wenn ein neues Element im RSS-Feed angezeigt wird, sendet die Logik-App eine E-Mail an eine genehmigende Person. Je nachdem, ob die genehmigende Person „Genehmigen“ oder „Ablehnen“ auswählt, führt die Logik-App unterschiedliche Schritte aus.

> [!TIP]
> Wie alle Programmiersprachen unterstützt die switch-Anweisung nur Gleichheitsoperatoren. Wenn Sie andere relationale Operatoren verwenden müssen – z.B. „größer als“ – verwenden Sie eine [Bedingungsanweisung](#conditions).
> Um deterministisches Ausführungsverhalten zu gewährleisten, müssen Fälle einen eindeutigen und statischen Wert anstelle von dynamischen Token oder Ausdrücken enthalten.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Um dem Beispiel in diesem Artikel zu folgen, [erstellen Sie diese Beispiel-Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) mit einem Outlook.com- oder Office 365 Outlook-Konto.

  1. Beim Hinzufügen der Aktion zum Senden einer E-Mail wählen Sie stattdessen **Genehmigungs-E-Mail senden**.

     ![Auswählen von „Genehmigungs-E-Mail senden“](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  2. Füllen Sie die Pflichtfelder wie „E-Mail-Adresse“ für die Person aus, die die Genehmigungs-E-Mail erhält. 
  Geben Sie unter **Benutzeroptionen** „Genehmigen, Ablehnen“ ein.

     ![Eingeben von E-Mail-Details](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-a-switch-statement"></a>Hinzufügen einer switch-Anweisung

1. Wählen Sie am Ende des Beispielworkflows die Option **+ Neuer Schritt** > **... Weitere** > **Parameter hinzufügen**. 

   ![Hinzufügen einer switch-Anweisung](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Eine switch-Anweisung wird mit einem Fall und einem Standardfall angezeigt. 
   Eine switch-Anweisung erfordert neben dem Standardfall mindestens einen weiteren Fall. 

   Wenn Sie eine switch-Anweisung zwischen Schritten hinzufügen möchten, bewegen Sie den Zeiger auf den Pfeil an der Stelle, an der Sie die switch-Anweisung hinzufügen möchten. 
   Wählen Sie das angezeigte **Pluszeichen** (**+**) und dann **Hinzufügen einer switch-Anweisung** aus.

4. Wählen Sie im **Aktivierungsfeld** das Feld **SelectedOption** aus, dessen Ausgabe die durchzuführende Aktion bestimmt. 
   
   Sie können das Feld in der Liste **Dynamischen Inhalt hinzufügen** auswählen, die angezeigt wird.

5. Zum Verarbeiten der Fälle, bei denen die genehmigende Person `Approve` oder `Reject` auswählt, fügen Sie einen weiteren Fall zwischen **Fall** und **Standard** hinzu. 
   
6. Fügen Sie den entsprechenden Fällen diese Aktionen hinzu:

   | Fallnr. | **SelectedOption** | anzuzeigen. |
   |:------ |:-------------------|:------ |
   | Fall 1 | **Genehmigen** | Fügen Sie die Outlook-Aktion **E-Mail senden** für das Senden von Details zum RSS-Element nur hinzu, wenn die genehmigende Person **Genehmigen** ausgewählt hat. |
   | Fall 2 | **Ablehnen** | Fügen Sie die Outlook-Aktion **E-Mail senden** hinzu, um andere genehmigende Personen zu darüber zu benachrichtigen, dass das RSS-Element abgelehnt wurde. |
   | Standard | \<keine\> | Keine Aktion erforderlich. In diesem Beispiel ist der **Standardfall** leer, da **SelectedOption** nur zwei Optionen aufweist. |
   |         |          |

   ![switch-Anweisung](./media/logic-apps-control-flow-switch-statement/switch.png)

7. Speichern Sie Ihre Logik-App. 

   Zum manuellen Testen dieses Beispiels wählen Sie **Ausführen**, bis die Logik-App ein neues RSS-Element findet und eine Genehmigungs-E-Mail sendet. 
   Wählen Sie **Genehmigen** aus, um die Ergebnisse zu beobachten.

## <a name="json-definition"></a>JSON-Definition

Da Sie nun eine Logik-App mit einer switch-Anweisung erstellt haben, sehen wir uns die allgemeine Codedefinition an, die der switch-Anweisung zugrunde liegt.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case" : {
         "actions" : {
           "Send_an_email": { }
         },
         "case" : "Approve"
      },
      "Case_2" : {
         "actions" : {
           "Send_an_email_2": { }
         },
         "case" : "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

| Bezeichnung              | BESCHREIBUNG |
| :----------------- | :---------- |
| `"Switch"`         | Name der switch-Anweisung, den Sie zur besseren Lesbarkeit umbenennen können |
| `"type": "Switch"` | Gibt an, dass die Aktion eine switch-Anweisung ist |
| `"expression"`     | Gibt in diesem Beispiel die ausgewählte Option der genehmigenden Person an, für die weiter unten in der Definition für jeden Fall eine Auswertung erfolgt |
| `"cases"` | Definiert eine beliebige Anzahl von Fällen. `"Case_*"` der jeweilige Standardname für jeden Fall, den Sie zur besseren Lesbarkeit ändern können |
| `"case"` | Gibt den Wert des Falls an, der ein konstanter und eindeutiger Wert sein muss, den die switch-Anweisung für den Vergleich verwendet. Wenn keine Fälle dem Ergebnis des switch-Ausdrucks entsprechen, werden die Aktionen im Abschnitt `"default"` ausgeführt.
|           |         |

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf einer Bedingung (Bedingungsanweisungen)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
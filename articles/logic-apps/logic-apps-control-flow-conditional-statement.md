---
title: Hinzufügen bedingter Anweisungen zu Workflows – Azure Logic Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Bedingungen erstellen, die Aktionen in Workflows in Azure Logic Apps steuern.
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: e8d84944d44588602593c762c4f60c375e480343
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298167"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Erstellen bedingter Anweisungen zum Steuern von Workflowaktionen in Azure Logic Apps

Fügen Sie eine *bedingte Anweisung* hinzu, falls bestimmte Aktionen in Ihrer Logik-App erst nach dem Übergeben einer bestimmten Bedingung ausgeführt werden sollen. Diese Struktur vergleicht die Daten in Ihrem Workflow mit bestimmten Werten oder Feldern. Sie können verschiedene Aktionen definieren, die abhängig davon ausgeführt werden, ob die Daten die Bedingung erfüllen. Sie können Bedingungen ineinander schachteln.

Angenommen Sie verfügen über eine Logik-App, die zu viele E-Mails sendet, wenn neue Elemente auf dem RSS-Feed einer Website verfügbar gemacht werden. Sie können Bedingungsanweisung hinzufügen, um nur dann eine E-Mail zu senden, wenn das neue Element eine bestimmte Zeichenfolge enthält. 

> [!TIP]
> Zum Ausführen verschiedener Schritte basierend auf anderen bestimmten Werten verwenden Sie stattdessen eine [*switch-Anweisung*](../logic-apps/logic-apps-control-flow-switch-statement.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Um dem Beispiel in diesem Artikel zu folgen, [erstellen Sie diese Beispiel-Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) mit einem Outlook.com- oder Office 365 Outlook-Konto.

## <a name="add-a-condition"></a>Bedingung hinzufügen

1. Öffnen Sie Ihre Logik-App im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> im Logik-App-Designer.

2. Fügen Sie an der gewünschten Position eine Bedingung hinzu. 

   Um eine Bedingung zwischen Schritten hinzuzufügen, bewegen Sie den Zeiger auf den Pfeil an der Position, an der Sie die Bedingung hinzufügen möchten. Wählen Sie das angezeigte **Pluszeichen** (**+**) und dann **Bedingung hinzufügen** aus. Beispiel: 

   ![Hinzufügen einer Bedingung zwischen Schritten](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Wenn Sie eine Bedingung am Ende Ihres Workflows hinzufügen möchten, wählen Sie in Ihrer Logik-App unten die Option **+ Neuer Schritt** > **Bedingung hinzufügen**.

3. Erstellen Sie unter **Bedingung** Ihre Bedingung. 

   1. Geben Sie im linken Feld die Daten oder das Feld an, die verglichen werden sollen.

      In der Liste **Dynamischen Inhalt hinzufügen** können Sie vorhandene Felder aus Ihrer Logik-App auswählen.

   2. Wählen Sie in der mittleren Liste den auszuführenden Vorgang aus. 
   3. Geben Sie im rechten Feld einen Wert oder ein Feld als Suchkriterium an.

   Beispiel: 

   ![Bearbeiten einer Bedingung im Standardmodus](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode.png)

   Hier ist die vollständige Bedingung:

   ![Vollständige Bedingung](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Zum Erstellen einer komplexeren Bedingung oder zum Verwenden von Ausdrücken wählen Sie **Im erweiterten Modus bearbeiten**. Sie können Ausdrücke verwenden, die durch die [Definitionssprache für Workflows](../logic-apps/logic-apps-workflow-definition-language.md) definiert sind.
   > 
   > Beispiel: 
   >
   > ![Bearbeiten einer Bedingung im Code](./media/logic-apps-control-flow-conditional-statement/edit-condition-advanced-mode.png)

5. Fügen Sie unter **Wenn Ja** und **Wenn Nein** die auszuführenden Schritte basierend darauf hinzu, ob die Bedingung erfüllt ist. Beispiel: 

   ![Bedingung mit Ja- und Nein-Pfaden](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Sie können die vorhandenen Aktionen in die **Wenn Ja** und **Wenn Nein**-Pfade ziehen.

6. Speichern Sie Ihre Logik-App.

Diese Logik-App sendet jetzt nur eine E-Mail, wenn die neuen Elemente im RSS-Feed Ihre Bedingung erfüllen.

## <a name="json-definition"></a>JSON-Definition

Da Sie nun eine Logik-App mit einer Bedingungsanweisung erstellt haben, sehen wir uns die allgemeine Codedefinition an, die der Bedingungsanweisung zugrunde liegt.

``` json
"actions": {
  "myConditionName": {
    "type": "If",
    "expression": "@contains(triggerBody()?['summary'], 'Microsoft')",
    "actions": {
      "Send_an_email": {
        "inputs": { },
        "runAfter": {}
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf verschiedenen Werten (switch-Anweisungen)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)

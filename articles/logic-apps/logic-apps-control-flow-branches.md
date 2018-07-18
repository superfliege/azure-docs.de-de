---
title: Erstellen oder Verknüpfen paralleler Verzweigungen – Azure Logic Apps | Microsoft-Dokumentation
description: Vorgehensweise zum Erstellen oder Verknüpfen paralleler Verzweigungen für Workflows in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2a8dcd82b67ee64e5687d8687415056b0aab39aa
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298854"
---
# <a name="create-or-join-parallel-branches-for-workflow-actions-in-azure-logic-apps"></a>Erstellen oder Verknüpfen paralleler Verzweigungen für Workflowaktionen in Azure Logic Apps

Standardmäßig werden Ihre Aktionen in Logik-App-Workflows nacheinander ausgeführt. Zum gleichzeitigen Ausführen unabhängiger Aktionen können Sie [parallele Verzweigungen](#parallel-branches) erstellen und später in Ihrem Flow [diese Verzweigungen verknüpfen](#join-branches). 

> [!TIP] 
> Wenn Sie einen Auslöser verwenden, der ein Array empfängt, und für jedes Arrayelement einen Workflow ausführen möchten, können Sie dieses Array mit der [**Auslösereigenschaft** SplitOn](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) *aus dem Batch lösen*.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-a-parallel-branch"></a>Hinzufügen einer parallelen Verzweigung

Zum gleichzeitigen Ausführen unabhängiger Schritte können Sie neben einem vorhandenen Schritt parallele Verzweigungen hinzufügen. 

![Paralleles Ausführen von Schritten](media/logic-apps-control-flow-branches/parallel.png)

Ihre Logik-App wartet, bis alle Verzweigungen abgeschlossen sind, bevor der Workflow fortgesetzt wird.
Parallele Verzweigungen werden nur ausgeführt, wenn ihre `runAfter`-Eigenschaftswerte dem Status des abgeschlossenen übergeordneten Schritts entsprechen. Beispielsweise wird die Ausführung von sowohl `branchAction1` als auch `branchAction2` nur festgelegt, wenn `parentAction` mit dem Status `Succeded` abgeschlossen wird.

> [!NOTE]
> Bevor Sie beginnen, muss Ihre Logik-App bereits über einen Schritt verfügen, dem Sie parallele Verzweigungen hinzufügen können.

1. Öffnen Sie Ihre Logik-App im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> im Logik-App-Designer.

2. Bewegen Sie den Mauszeiger auf den Pfeil über dem Schritt, dem Sie parallele Verzweigungen hinzufügen möchten.

3. Wählen Sie das **Pluszeichen** (**+**), wählen Sie **Hinzufügen einer parallelen Verzweigung**, und wählen Sie das Element aus, das Sie hinzufügen möchten.

   ![Hinzufügen einer parallelen Verzweigung](media/logic-apps-control-flow-branches/add-parallel-branch.png)

   Ihr ausgewähltes Element wird jetzt in einer parallelen Verzweigung angezeigt.

4. Fügen Sie für jede parallele Verzweigung die gewünschten Schritte hinzu. Um einer parallelen Verzweigung eine sequenzielle Aktion hinzuzufügen, bewegen Sie die Maus unter die Aktion, der Sie die sequenzielle Aktion hinzufügen möchten. Wählen Sie das **Pluszeichen** (**+**) und den Schritt, den Sie hinzufügen möchten.

   ![Hinzufügen eines sequenziellen Schritts zu einer parallelen Verzweigung](media/logic-apps-control-flow-branches/add-sequential-action-parallel-branch.png)

5. [Verknüpfen Sie Ihre parallelen Verzweigungen](#join-branches), um Verzweigungen wieder zusammenführen. 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Definition (JSON) einer parallelen Verzweigung

Wenn Sie in der Codeansicht arbeiten, können Sie die parallele Struktur stattdessen in der JSON-Definition der Logik-App definieren, z.B.:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<a name="join-branches"></a>

## <a name="join-parallel-branches"></a>Verknüpfen von parallelen Verzweigungen

Um parallele Verzweigungen zusammenführen fügen Sie einfach einen Schritt im unteren Bereich unter allen Verzweigungen hinzu. Dieser Schritt wird ausgeführt, nachdem die Ausführung aller parallelen Verzweigungen abgeschlossen ist.

![Verknüpfen von parallelen Verzweigungen](media/logic-apps-control-flow-branches/join.png)

1. Suchen oder öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer. 

2. Fügen Sie den auszuführenden Schritt unter den parallelen Verzweigungen hinzu, die Sie verknüpfen möchten.

   ![Hinzufügen eines Schritts zum Verknüpfen paralleler Verzweigungen](media/logic-apps-control-flow-branches/join-steps.png)

   Ihre parallelen Verzweigungen sind jetzt zusammengeführt.

<a name="join-json"></a>

## <a name="join-definition-json"></a>Verknüpfungsdefinition (JSON)

Wenn Sie in der Codeansicht arbeiten, können Sie die Verknüpfungsstruktur stattdessen in der JSON-Definition der Logik-App definieren, z.B.:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "joinAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "branchAction1": [
          "Succeeded"
        ],
        "branchAction2": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf einer Bedingung (Bedingungsanweisungen)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Ausführen von Schritten basierend auf verschiedenen Werten (switch-Anweisungen)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)

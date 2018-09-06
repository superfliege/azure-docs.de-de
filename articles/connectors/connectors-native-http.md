---
title: Herstellen einer Verbindung mit einem beliebigen HTTP-Endpunkt mit Azure Logic Apps | Microsoft-Dokumentation
description: Verwenden von Azure Logic Apps zum Automatisieren von Aufgaben und Workflows, die mit jedem HTTP-Endpunkt kommunizieren
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: e1561e3be95847efccf487c96bd9c9a8104f161b
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106447"
---
# <a name="call-http-or-https-endpoints-with-azure-logic-apps"></a>Aufrufen von HTTP- oder HTTPS-Endpunkten mit Azure Logic Apps

Mit Azure Logic Apps und dem HTTP-Connector (Hypertext Transfer Protocol) können Sie durch Erstellung von Logik-Apps Workflows automatisieren, die mit HTTP- oder HTTPS-Endpunkten kommunizieren. Sie können beispielsweise den Dienstendpunkt für Ihre Website überwachen. Wenn ein Ereignis (beispielsweise ein Ausfall Ihrer Website) an diesem Endpunkt auftritt, löst das Ereignis den Workflow Ihrer Logik-App aus und führt die angegebenen Aktionen aus. 

Sie können den HTTP-Trigger als ersten Schritt in Ihrem Workflow verwenden, um einen Endpunkt gemäß einem Zeitplan zu überprüfen oder *abzurufen*. Bei jeder Überprüfung sendet der Trigger einen Aufruf oder eine *Anforderung* an den Endpunkt. Die Antwort des Endpunkts bestimmt, ob der Workflow der Logik-App ausgeführt wird. Der Trigger übergibt alle Inhalte aus der Antwort an die Aktionen in Ihrer Logik-App. 

Sie können die HTTP-Aktion wie jeden anderen Schritt in Ihrem Workflow zum Abrufen des Endpunkts zum gewünschten Zeitpunkt verwenden. Die Antwort des Endpunkts bestimmt, wie die restlichen Aktionen des Workflows ausgeführt werden.

Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>. 

* Die URL für den Zielendpunkt, den Sie aufrufen möchten 

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, über die Sie den Zielendpunkt aufrufen möchten. [Erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md), um mit dem HTTP-Trigger zu beginnen. Starten Sie Ihre Logik-App mit einem Trigger, um die HTTP-Aktion zu verwenden.

## <a name="add-http-trigger"></a>Hinzufügen eines HTTP-Triggers

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre leere Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Geben Sie im Suchfeld „HTTP“ als Filter ein. Wählen Sie in der Triggerliste den **HTTP**-Trigger aus. 

   ![Auswählen des HTTP-Triggers](./media/connectors-native-http/select-http-trigger.png)

1. Geben Sie die [Parameter und Werte des HTTP-Triggers](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) ein, die Sie in den Aufruf des Zielendpunkts aufnehmen möchten. Geben Sie mithilfe einer Wiederholung an, wie oft der Trigger den Zielendpunkt überprüfen soll.

   ![Eingeben der HTTP-Triggerparameter](./media/connectors-native-http/http-trigger-parameters.png)

   Weitere Informationen zum HTTP-Trigger sowie zu den Parametern und Werten finden Sie in der [Referenz zu Trigger- und Aktionstypen für die Workflowdefinitionssprache in Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger).

1. Fahren Sie mit dem Erstellen des Workflows Ihrer Logik-App fort, und fügen Sie weitere Aktionen hinzu, die bei Auslösung des Triggers ausgeführt werden.

## <a name="add-http-action"></a>Hinzufügen einer HTTP-Aktion

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Wählen Sie im letzten Schritt zum Hinzufügen der HTTP-Aktion die Option **Neuer Schritt** aus. 

   In diesem Beispiel beginnt die Logik-App mit dem HTTP-Trigger als erstem Schritt.

1. Geben Sie im Suchfeld „HTTP“ als Filter ein. Wählen Sie in der Liste mit den Aktionen die **HTTP**-Aktion aus.

   ![Auswählen der HTTP-Aktion](./media/connectors-native-http/select-http-action.png)

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
   Wählen Sie das daraufhin angezeigte Pluszeichen (**+**) und dann **Aktion hinzufügen** aus.

1. Geben Sie die [Parameter und Werte der HTTP-Aktion](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) ein, die Sie in den Aufruf des Zielendpunkts aufnehmen möchten. 

   ![Eingeben der Parameter für die HTTP-Aktion](./media/connectors-native-http/http-action-parameters.png)

1. Speichern Sie Ihre Logik-App, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern**. 

## <a name="authentication"></a>Authentifizierung

Klicken Sie zum Festlegen der Authentifizierung in der Aktion oder im Trigger auf **Erweiterte Optionen anzeigen**. Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP-Trigger und -Aktionen finden Sie unter [Referenz zu Trigger- und Aktionstypen für die Workflowdefinitionssprache in Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
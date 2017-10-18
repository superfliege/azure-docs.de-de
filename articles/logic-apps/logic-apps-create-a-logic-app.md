---
title: "Erstellen Ihres ersten automatisierten Workflows zwischen Systemen und Clouddiensten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Automatisieren von Geschäftsprozessen und Workflows für Systemintegrations- und EAI-Szenarien (Enterprise Application Integration) durch Erstellen und Ausführen von Logik-Apps"
author: ecfan
manager: anneta
editor: 
services: logic-apps
keywords: "Workflow, Cloud-Apps, Cloud-Dienste, Geschäftsprozesse, Systemintegration, Enterprise Application Integration, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: LADocs; estfan
ms.openlocfilehash: d62255ba6e3d5bdfbd792a47f3a92d4c88876742
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-logic-app-for-automating-workflows-and-processes-through-the-azure-portal"></a>Erstellen Ihrer ersten Logik-App zum Automatisieren von Workflows und Prozessen über das Azure-Portal

Mit [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) können Sie automatisierte Workflows erstellen und ausführen, um Systeme und Dienste ganz ohne Programmieraufwand zu integrieren. Dieses Tutorial zeigt, wie einfach sich Aufgaben mit einem Workflow automatisieren lassen. Dazu wird eine einfache Logik-App erstellt, die einen RSS-Feed auf einer Website auf neue Inhalte prüft und für jedes neue Feedelement eine E-Mail sendet. 

![Übersicht – Beispiel für eine erste Logik-App](./media/logic-apps-create-a-logic-app/logic-app-overview.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer leeren Logik-App
> * Hinzufügen eines Triggers, damit die Logik-App startet, wenn ein neues RSS-Feedelement veröffentlicht wird
> * Hinzufügen einer Aktion, damit eine E-Mail mit Details zu dem RSS-Feedelement gesendet wird
> * Ausführen und Testen der Logik-App

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Sie können sich aber auch [für ein Abonnement mit nutzungsbasierter Bezahlung registrieren](https://azure.microsoft.com/pricing/purchase-options/).

* Ein E-Mail-Konto bei [einem beliebigen von Azure Logic Apps unterstützten E-Mail-Anbieter](../connectors/apis-list.md) für den Versand von Benachrichtigungen. Sie können beispielsweise Office 365 Outlook, Outlook.com, Gmail oder einen anderen unterstützten Anbieter verwenden. Dieses Tutorial verwendet Office 365 Outlook.

  > [!TIP]
  > Besitzer eines privaten [Microsoft-Kontos](https://account.microsoft.com/account) verfügen über ein Outlook.com-Konto. Falls Sie über ein Geschäfts-, Schul- oder Unikonto für Azure verfügen, besitzen Sie ein Office 365 Outlook-Konto.

* Einen Link zum RSS-Feed einer Website. Dieses Beispiel verwendet die [RSS-Feeds für Topmeldungen von der CNN.com-Website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="1-create-a-blank-logic-app"></a>1. Erstellen einer leeren Logik-App 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an.

2. Navigieren Sie über das Hauptmenü von Azure zu **Neu** > **Enterprise Integration** > **Logik-App**.

   ![Azure-Portal, Neu, Enterprise Integration, Logik-App](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Erstellen Sie Ihre Logik-App mit den Einstellungen aus der Tabelle.

   ![Angeben von Details zur Logik-App](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Name** | *Name Ihrer Logik-App* | Geben Sie einen eindeutigen Namen für die Logik-App an. | 
   | **Abonnement** | *Ihr Azure-Abonnement* | Wählen Sie das gewünschte Azure-Abonnement aus. | 
   | **Ressourcengruppe** | *Ihre Azure-Ressourcengruppe* | Erstellen Sie eine Azure-Ressourcengruppe, oder wählen Sie eine aus. Mit einer Azure-Ressourcengruppe lassen sich verwandte Azure-Ressourcen leichter organisieren und verwalten. | 
   | **Standort** | *Ihre Azure-Region* | Wählen Sie die Datencenterregion für die Bereitstellung Ihrer Logik-App aus. | 
   |||| 

4. Aktivieren Sie abschließend das Kontrollkästchen **An Dashboard anheften**, und klicken Sie auf **Erstellen**.

   Sie haben nun eine Azure-Ressource für Ihre Logik-App erstellt. 
   Nachdem Azure Ihre Logik-App bereitgestellt hat, zeigt der Designer für Logik-Apps Vorlagen für allgemeine Muster an, die Ihnen einen schnelleren Einstieg ermöglichen.

   > [!NOTE] 
   > Wenn Sie das Kontrollkästchen **An Dashboard anheften** aktivieren, wird Ihre Logik-App nach der Bereitstellung auf dem Azure-Dashboard angezeigt und automatisch im Designer für Logik-Apps geöffnet. Andernfalls können Sie nach der Logik-App suchen und sie manuell öffnen.

5. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App** aus, um Ihre Logik-App neu zu erstellen.

   ![Auswählen der Logik-App-Vorlage](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   Der Designer für Logik-Apps zeigt nun die verfügbaren [*Connectors*](../connectors/apis-list.md) und die dazugehörigen [*Trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) zum Starten des Logik-App-Workflows an.

   ![Logik-App-Trigger](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="2-add-a-trigger-for-starting-the-workflow"></a>2. Hinzufügen eines Triggers zum Starten des Workflows

Jede Logik-App muss mit einem [*Trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) gestartet werden. Der Trigger wird ausgelöst, wenn ein bestimmtes Ereignis eintritt oder neue Daten die festgelegte Bedingung erfüllen. Das Logic Apps-Modul erstellt dann eine Logik-App-Instanz zur Ausführung Ihres Workflows. Bei jeder Triggerauslösung wird eine weitere separate Instanz erstellt, die Ihren Logik-App-Workflow ausführt.

1. Geben Sie im Suchfeld die Zeichenfolge „rss“ als Filter ein. Wählen Sie den folgenden Trigger aus: **RSS – Beim Veröffentlichen eines Feedelements**. 

   ![Trigger „RSS – Beim Veröffentlichen eines Feedelements“ auswählen](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Geben Sie den Link für den RSS-Feed der Website an, den Sie überwachen möchten (beispielsweise `http://rss.cnn.com/rss/cnn_topstories.rss`). Legen Sie das Intervall und die Frequenz für die Wiederholung fest. In diesem Beispiel legen wir die Eigenschaften so fest, dass der Feed einmal täglich geprüft wird. 

   ![Einrichten eines Triggers mit RSS-Feed, Häufigkeit und Intervall](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

3. Speichern Sie Ihre Arbeit. Wählen Sie auf der Symbolleiste des Designers **Speichern**.
Klicken Sie zum Reduzieren und Ausblenden der Triggerdetails auf die Titelleiste des Triggers.

   ![Speichern Ihrer Logik-App](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Ihre Logik-App ist nun aktiv, prüft aber bislang nur den RSS-Feed auf neue Elemente, da dem Workflow noch keine Aktionen hinzugefügt wurden. 

## <a name="3-add-an-action-that-responds-to-the-trigger"></a>3. Hinzufügen einer Aktion als Reaktion auf den Trigger

Fügen Sie als Nächstes eine [*Aktion*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) hinzu. Hierbei handelt es sich um eine Aufgabe, die Ihr Logik-App-Workflow ausführen soll. In diesem Beispiel fügen wir eine Aktion hinzu, die eine E-Mail sendet, wenn im RSS-Feed ein neues Element vorhanden ist.

1. Klicken Sie im Designer für Logik-Apps unter dem Trigger auf **+ Neuer Schritt** > **Aktion hinzufügen**.

   ![Hinzufügen einer Aktion](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Der Designer zeigt [verfügbare Connectors](../connectors/apis-list.md) an, damit Sie eine Aktion auswählen können, die ausgeführt werden soll, wenn Ihr Trigger ausgelöst wird.

   ![Aktion aus der Liste auswählen](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Geben Sie im Suchfeld den Begriff „E-Mail senden“ als Filter ein. Suchen Sie abhängig von Ihrem E-Mail-Anbieter nach dem entsprechenden Connector, und wählen Sie diesen aus. Wählen Sie anschließend die Aktion „E-Mail senden“ für Ihren Connector aus. Beispiel: 

   * Wählen Sie für ein Geschäfts-, Schul- oder Unikonto in Azure den Office 365 Outlook-Connector aus. 
   * Wählen Sie für persönliche Microsoft-Konten den Outlook.com-Connector aus. 
   * Wählen Sie für Gmail-Konten den Gmail-Connector aus. 

   Wir fahren mit dem Office 365 Outlook-Connector fort. 
   Wenn Sie einen anderen Anbieter verwenden, werden dieselben Schritte durchgeführt, allerdings kann das Erscheinungsbild der Benutzeroberfläche eventuell abweichen. 

   ![Aktion „Office 365 Outlook – E-Mail senden“ auswählen](./media/logic-apps-create-a-logic-app/actions.png)

3. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich mit dem Benutzernamen und dem Kennwort für Ihr E-Mail-Konto an. 

4. Geben Sie die Details aus der Tabelle an, und wählen Sie die gewünschten Felder für die E-Mail aus.

   | Aufgabe | Schritte | 
   | -- | ----- | 
   | Auswählen verfügbarer Felder für Ihren Workflow | Klicken Sie auf ein Bearbeitungsfeld, sodass die Liste **Dynamischer Inhalt** geöffnet wird, oder klicken Sie auf **Dynamischen Inhalt hinzufügen**. | 
   | Anzeigen weiterer verfügbarer Felder | Klicken Sie in der Liste **Dynamischer Inhalt** für die einzelnen Abschnitte auf **See more** (Mehr anzeigen).  | 
   | Hinzufügen von Leerzeilen in einem Bearbeitungsfeld | Drücken Sie UMSCHALT+EINGABE. | 
   | Schließen der Liste **Dynamischer Inhalt** | Klicken Sie erneut auf **Dynamischen Inhalt hinzufügen**. | 
   ||| 

   ![Auswählen der Daten, die die E-Mail enthalten soll](./media/logic-apps-create-a-logic-app/rss-action-setup.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **An** | *E-Mail-Adresse des Empfängers* | Geben Sie die E-Mail-Adresse des Empfängers ein. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. | 
   | **Betreff** | Neuer CNN-Beitrag: **Feedtitel** | Geben Sie den Inhalt des Betreffs der E-Mail ein. <p>Verwenden Sie in diesem Tutorial den vorgeschlagenen Text, und wählen Sie das Feld **Feedtitel** des Triggers aus, das den Titel des Feedelements anzeigt. | 
   | **Text** | Titel: **Feedtitel** <p>Veröffentlichungsdatum: **Datum der Feedveröffentlichung** <p>Link: **Link zum primären Feed** | Geben Sie den Inhalt des Texts der E-Mail ein. <p>Geben Sie für dieses Tutorial den vorgeschlagenen Text ein, und wählen Sie folgende Triggerfelder aus: <p>- **Feedtitel:** Zeigt erneut den Titel des Feedelements an. </br>- **Datum der Feedveröffentlichung:** Zeigt den Veröffentlichungszeitpunkt (Datum und Uhrzeit) des Elements an. </br>- **Link zum primären Feed:** Zeigt die URL für das Feedelement an. | 
   |||| 

   > [!NOTE] 
   > Wenn Sie ein Feld auswählen, das ein Array speichert, schließt der Designer die Aktion automatisch in eine ForEach-Schleife ein, die auf das Array verweist. Auf diese Weise führt Ihre Logik-App diese Aktion für jedes Arrayelement durch.

5. Speichern Sie Ihre Änderungen, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

   ![Fertige Logik-App](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

   Um nun Ihre Logik-App zu testen, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="4-run-and-test-your-workflow"></a>4. Ausführen und Testen des Workflows

1. Sie können Ihre Logik-App zu Testzwecken manuell ausführen. Klicken Sie hierzu auf der Symbolleiste des Designers auf **Ausführen**. Alternativ kann Ihre Logik-App den angegebenen RSS-Feed auch gemäß dem eingerichteten Zeitplan überprüfen.

   ![Logik-App ausführen](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

   Wenn Ihre Logik-App neue Elemente findet, sendet sie eine E-Mail mit den von Ihnen ausgewählten Daten. Beispiel:

   ![Gesendete E-Mail bei neuem RSS-Feedelement](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

   Sollte Ihre Logik-App keine neuen Elemente finden, überspringt sie die Aktion zum Senden einer E-Mail und wiederholt die Prüfung im nächsten Intervall. 

2. Klicken Sie zum Überprüfen des Ausführungs- und Triggerverlaufs Ihrer Logik-App in Ihrem Logik-App-Menü auf **Übersicht**.
Um weitere Details einer Ausführung anzuzeigen, wählen Sie die Zeile dieser Ausführung.

   ![Überwachen und Anzeigen des Ausführungs- und Triggerverlaufs Ihrer Logik-App](./media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Sollten Sie nicht die erwarteten Daten vorfinden, klicken Sie auf der Symbolleiste auf **Aktualisieren**.

   Die Ausführungsdetails geben unabhängig vom Erfolg der Ausführung Aufschluss über die erfolgreichen oder nicht erfolgreichen Schritte. 

   ![Details für eine Logik-App-Ausführung anzeigen](./media/logic-apps-create-a-logic-app/logic-app-run-details.png)

   Weitere Informationen zum Status sowie zum Ausführungs- und Triggerverlauf Ihrer Logik-App sowie Informationen zum Diagnostizieren Ihrer Logik-App finden Sie unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md).

3. Um die Eingaben und Ausgaben für die einzelnen Schritte anzuzeigen, erweitern Sie den Schritt, den Sie überprüfen möchten. Diese Informationen helfen Ihnen bei der Diagnose und beim Debuggen von Problemen in Ihrer Logik-App. Beispiel:

   ![Schrittdetails anzeigen](./media/logic-apps-create-a-logic-app/logic-app-run-details-expanded.png)

   Weitere Informationen finden Sie unter [Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Damit haben Sie Ihre erste einfache Logik-App erstellt und ausgeführt. Dieses Beispiel zeigt, wie einfach Sie ganz ohne Programmieraufwand Workflows zur Automatisierung von Prozessen erstellen können, um Systeme und Dienste zu integrieren.

> [!NOTE]
> Ihre Logik-App wird weiter ausgeführt, bis Sie sie deaktivieren. Informationen zum vorübergehenden Deaktivieren Ihrer App finden Sie im nächsten Abschnitt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden Ressourcen verwendet und Aktionen durchgeführt, für die unter Umständen Gebühren in Ihrem Azure-Abonnement anfallen. Wenn Sie mit der Durchführung des Tutorials und dem Testen fertig sind, deaktivieren oder löschen Sie unbedingt alle Ressourcen, für die keine Gebühren anfallen sollen.

Sie können die Ausführung Ihrer Logik-App und das Senden von E-Mail durch die Logik-App stoppen, ohne die App zu löschen. Wählen Sie in Ihrem Logik-App-Menü **Übersicht**. Wählen Sie auf der Symbolleiste **Deaktivieren** aus.

![Deaktivieren der Logik-App](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Häufig gestellte Fragen

**F:** Welche Aktionen kann ich noch für meine Logik-App ausführen? </br>
**A:** Sie können Ihre Logik-App beispielsweise bearbeiten, ihre JSON-Definition anzeigen, das Aktivitätsprotokoll prüfen oder die Logik-App löschen.

Für weitere Logik-App-Verwaltungsaufgaben stehen im Logik-App-Menü folgende Befehle zur Verfügung:

| Aufgabe | Schritte | 
| ---- | ----- | 
| Anzeigen von Status, Ausführungs- und Triggerverlauf und allgemeinen Informationen zu Ihrer App | Wählen Sie **Übersicht** aus. | 
| Bearbeiten Ihrer App | Wählen Sie **Logik-App-Designer** aus. | 
| Anzeigen der JSON-Definition Ihres App-Workflows | Wählen Sie **Logik-App-Codeansicht** aus. | 
| Anzeigen der Vorgänge, die von Ihrer Logik-App ausgeführt wurden | Wählen Sie **Aktivitätsprotokoll** aus. | 
| Anzeigen älterer Versionen für Ihre Logik-App | Wählen Sie **Versionen** aus. | 
| Vorübergehendes Deaktivieren Ihrer App | Wählen Sie **Übersicht** und anschließend auf der Symbolleiste die Option **Deaktivieren** aus. | 
| Löschen Ihrer App | Wählen Sie **Übersicht** und anschließend auf der Symbolleiste die Option **Löschen** aus. Geben Sie den Namen der Logik-App ein, und wählen Sie **Löschen**. | 
||| 

## <a name="get-support"></a>Support

* Sollten Sie Fragen zur Azure Logic Apps haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Sie möchten zur Verbesserung von Azure Logic Apps und der Connectors beitragen? Auf der [Benutzerfeedbackwebsite für Azure Logic Apps](http://aka.ms/logicapps-wish) können Sie über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Ihrer Logik-App mit Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Hinzufügen von Bedingungen und Ausführen von Workflows](../logic-apps/logic-apps-use-logic-app-features.md)
*   [Logik-App-Vorlagen](../logic-apps/logic-apps-use-logic-app-templates.md)
* [Erstellen einer Logik-App mithilfe einer Vorlage](../logic-apps/logic-apps-arm-provision.md)
* [Preismodell für Logik-Apps](../logic-apps/logic-apps-pricing.md) 
* [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps)

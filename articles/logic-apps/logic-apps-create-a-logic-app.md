---
title: "Automatisieren von Workflows zwischen Systemen und Clouddiensten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Logik-Apps zum Automatisieren von Workflows für Szenarien mit Systemintegration und Enterprise Application Integration (EAI) automatisieren."
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
ms.date: 10/20/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 5906605192f9b03f612e6ca3a445434a23713d7f
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2017
---
# <a name="automate-your-first-workflow-to-process-data-with-a-logic-app"></a>Automatisieren Ihres ersten Workflows zum Verarbeiten von Daten mit einer Logik-App

Sie können Workflows und Geschäftsprozesse mit [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) automatisieren, um Systeme und Dienste schneller für Ihre Organisation zu integrieren. In dieser Schnellstartanleitung erfahren Sie, wie einfach das Erstellen und Ausführen eines automatisierten Workflows ist, indem Sie eine Logik-App erstellen. Die Beispiel-App veranschaulicht, wie Sie einen Workflow automatisieren, der einen Website-RSS-Feed auf neue Elemente überprüft und für jedes Element eine E-Mail sendet.

Mit der Logik-App in diesem Beispiel wird etwa folgende E-Mail gesendet:

![Gesendete E-Mail bei neuem RSS-Feedelement](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Hier ist der allgemeine Logik-App-Workflow angegeben, den Sie erstellen:

![Übersicht – Logik-App-Beispiel](./media/logic-apps-create-a-logic-app/logic-app-simple-overview.png)

In dieser Schnellstartanleitung wird Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer leeren Logik-App
> * Fügen Sie einen Trigger hinzu, mit dem der Workflow jeweils gestartet wird, wenn im RSS-Feed ein neues Element erscheint.
> * Fügen Sie eine Aktion hinzu, mit der eine E-Mail mit Details zum RSS-Feedelement gesendet wird.
> * Führen Sie Ihren Logik-App-Workflow aus.

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/), bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein E-Mail-Konto bei einem beliebigen von Azure Logic Apps unterstützten E-Mail-Anbieter für den Versand von Benachrichtigungen. Beispiele: Office 365 Outlook, Outlook.com oder Google Mail. Weitere unterstützte E-Mail-Connectors finden Sie im [Artikel zu den Connectors](https://docs.microsoft.com/connectors/). In dieser Schnellstartanleitung wird Office 365 Outlook verwendet.

  > [!TIP]
  > Besitzer eines privaten [Microsoft-Kontos](https://account.microsoft.com/account) verfügen über ein Outlook.com-Konto. Falls Sie über ein Geschäfts-, Schul- oder Unikonto für Azure verfügen, besitzen Sie ein Office 365 Outlook-Konto.

* Einen Link zum RSS-Feed einer Website. In diesem Beispiel wird der [RSS-Feed für Topmeldungen von der CNN.com-Website](http://feeds.reuters.com/reuters/topNews) verwendet: `http://feeds.reuters.com/reuters/topNews`

Für diesen Schnellstart muss kein Code geschrieben werden, aber Logic Apps unterstützt andere Szenarien mit Code, z.B. das Ausführen Ihres eigenen Codes über eine Logik-App mit [Azure Functions](../azure-functions/functions-overview.md).

## <a name="create-a-blank-logic-app"></a>Erstellen einer leeren Logik-App 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an. 

2. Navigieren Sie über das Hauptmenü von Azure zu **Neu** > **Enterprise Integration** > **Logik-App**.

   ![Azure-Portal, Neu, Enterprise Integration, Logik-App](./media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Erstellen Sie Ihre Logik-App mit den Einstellungen in der Tabelle unter dieser Abbildung:

   ![Angeben von Details zur Logik-App](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Name** | *Name Ihrer Logik-App* | Geben Sie einen eindeutigen Namen für die Logik-App an. | 
   | **Abonnement** | *Name-Ihres-Azure-Abonnements* | Wählen Sie das gewünschte Azure-Abonnement aus. | 
   | **Ressourcengruppe** | *Name-Ihrer-Azure-Ressourcengruppe* | Erstellen Sie eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) für diese Logik-App und zum Organisieren aller Ressourcen, die der App zugeordnet sind. | 
   | **Standort** | *Ihre-Azure-Datencenterregion* | Wählen Sie die Datencenterregion für die Bereitstellung Ihrer Logik-App aus, z.B. „USA, Westen“. | 
   | **Log Analytics** | Aus | Aktivieren Sie die Diagnoseprotokollierung für Ihre Logik-App, aber behalten Sie für diesen Schnellstart die Einstellung **Aus** bei. | 
   |||| 

4. Wählen Sie die Option **An Dashboard anheften**, wenn Sie fertig sind. Ihre Logik-App wird dann automatisch in Ihrem Azure-Dashboard angezeigt und nach der Bereitstellung geöffnet. Wählen Sie **Erstellen**.

   > [!NOTE]
   > Wenn Sie Ihre Logik-App nicht anheften möchten, müssen Sie sie nach der Bereitstellung manuell suchen und öffnen, um fortfahren zu können.

   Nachdem Ihre Logik-App von Azure bereitgestellt wurde, wird der Designer für Logik-Apps geöffnet, und es wird eine Seite mit einem Einführungsvideo angezeigt. 
   Unterhalb des Videos sind Vorlagen für häufig verwendete Logik-App-Muster angegeben. 
   In diesem Schnellstart wird Ihre Logik-App von Grund auf erstellt. 

5. Scrollen Sie weiter, um den Inhalt nach dem Einführungsvideo und den häufig verwendeten Triggern anzuzeigen. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der Vorlage „Leere Logik-App“](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   Im Designer für Logik-Apps werden die verfügbaren Connectors mit den dazugehörigen Triggern angezeigt, die zum Starten von Logik-App-Workflows verwendet werden.

   ![Logik-App-Trigger](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="add-a-trigger-to-detect-new-items"></a>Hinzufügen eines Triggers zum Erkennen neuer Elemente

Jeder Logik-App-Workflow beginnt mit einem [Trigger](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). Der Trigger wird ausgelöst, wenn ein bestimmtes Ereignis eintritt oder neue Daten die von Ihnen festgelegte Bedingung erfüllen. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz, mit der Ihr Workflow gestartet und ausgeführt wird.

1. Geben Sie im Suchfeld den Begriff „rss“ als Filter ein. Wählen Sie den folgenden Trigger aus: **RSS – Beim Veröffentlichen eines Feedelements**. 

   ![Trigger „RSS – Beim Veröffentlichen eines Feedelements“ auswählen](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. Geben Sie den Link für den RSS-Feed an, den Sie überwachen möchten, z.B. `http://feeds.reuters.com/reuters/topNews`. Legen Sie das Intervall und die Häufigkeit für die Wiederholung fest. In diesem Beispiel wird der Feed alle fünf Minuten überprüft.

   ![Einrichten eines Triggers mit RSS-Feed, Häufigkeit und Intervall](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

   Logic Apps erstellt eine Verbindung mit dem RSS-Feed.

   > [!TIP]
   > Zum Vereinfachen Ihrer Ansicht im Designer können Sie die Details einer Form reduzieren und ausblenden, indem Sie einfach in die Titelleiste der Form klicken.

3. Speichern Sie Ihre Arbeit. Wählen Sie auf der Symbolleiste des Designers **Speichern**. 

   ![Speichern Ihrer Logik-App](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Ihre Logik-App befindet sich jetzt im Livemodus, aber es wird vorerst nur der RSS-Feed überprüft. Wir fügen daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="add-an-action-to-send-email"></a>Hinzufügen einer Aktion zum Senden einer E-Mail

Nachdem Sie nun über einen Trigger verfügen, können Sie eine [Aktion](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) hinzufügen, mit der eine E-Mail gesendet wird, wenn im RSS-Feed ein neues Element erscheint. Ihr Workflow führt diese Aktion durch, nachdem der Trigger ausgelöst wurde.

1. Klicken Sie im Designer für Logik-Apps unter dem Trigger auf **+ Neuer Schritt** > **Aktion hinzufügen**.

   ![Hinzufügen einer Aktion](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Im Designer werden die Aktionen angezeigt, die von Ihrer Logik-App durchgeführt werden können, wenn der Trigger ausgelöst wird.

   ![Auswählen eines Eintrags aus der Liste mit den Aktionen](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Geben Sie im Suchfeld den Begriff „E-Mail senden“ als Filter ein. Suchen Sie nach dem gewünschten E-Mail-Connector, und wählen Sie ihn aus. Wählen Sie anschließend die Aktion „E-Mail senden“ für diesen Connector aus. Beispiel: 

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“. 
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“. 
   * Wählen Sie für Google Mail-Konten die Option „Google Mail“. 

   In dieser Schnellstartanleitung wird Office 365 Outlook verwendet. 
   Wenn Sie einen anderen E-Mail-Anbieter verwenden, werden dieselben Schritte ausgeführt, aber das Erscheinungsbild der Benutzeroberfläche kann eventuell abweichen. 

   ![Aktion „Office 365 Outlook – E-Mail senden“ auswählen](./media/logic-apps-create-a-logic-app/actions.png)

3. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich mit dem Benutzernamen und dem Kennwort für Ihr E-Mail-Konto an. 

   Logic Apps erstellt eine Verbindung mit Ihrem E-Mail-Konto.

4. Geben Sie nun die Daten an, die Sie in die E-Mail einbinden möchten. 

   1. Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. 
   Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben.

   2. Geben Sie im Feld **Betreff** den Betreff der E-Mail ein. 
   Geben Sie für dieses Beispiel wie gezeigt „New RSS item:“ (Neues RSS-Element:) ein:

      ![Eingeben des E-Mail-Betreffs](./media/logic-apps-create-a-logic-app/logic-app-add-subject.png)

      Wenn Sie in das Bearbeitungsfeld klicken, wird die Liste **Dynamischen Inhalt hinzufügen** geöffnet. Sie können dann die verfügbaren Datenfelder auswählen, die in Ihre Aktion eingebunden werden sollen. 
      Wenn die Liste mit dem dynamischen Inhalt nicht geöffnet wird, können Sie unter dem jeweiligen Bearbeitungsfeld die Option **Dynamischen Inhalt hinzufügen** wählen.

   3. Wählen Sie in der Liste **Dynamischen Inhalt hinzufügen** die Option **Feedtitel**, mit der der Titel des Elements in die E-Mail eingefügt wird.

      ![Eingeben des E-Mail-Betreffs](./media/logic-apps-create-a-logic-app/logic-app-select-field.png)

      Wenn der Vorgang abgeschlossen ist, sieht der Betreff der E-Mail wie in diesem Beispiel aus:

      ![Hinzugefügter Feedtitel](./media/logic-apps-create-a-logic-app/added-feed-title.png)

      > [!NOTE] 
      > Wenn Sie ein Feld auswählen, das ein Array enthält, z.B. **categories-item**, fügt der Designer automatisch eine For-Each-Schleife für die Aktion hinzu, mit der auf das Feld verwiesen wird. Auf diese Weise kann Ihre Logik-App diese Aktion für jedes Arrayelement durchführen. 
      > 
      > Wählen Sie zum Entfernen der Schleife die Auslassungspunkte (**...**) in der Titelleiste der Schleife und dann **Löschen**.

   4. Geben Sie im Feld **Body** (Text) den Inhalt für den E-Mail-Text ein. 
   Geben Sie für dieses Beispiel diesen Text ein bzw. wählen Sie diese Felder aus:

      ![Hinzufügen des Inhalts für den E-Mail-Text](./media/logic-apps-create-a-logic-app/logic-app-complete.png)

      | Feld | Beschreibung | 
      | ----- | ----------- | 
      | **Feedtitel** | Titel des Elements anzeigen | 
      | **Feed veröffentlicht am** | Datum und Uhrzeit der Elementveröffentlichung anzeigen | 
      | **Link zum primären Feed** | URL für das Element anzeigen | 
      ||| 

      > [!TIP]
      > Drücken Sie UMSCHALT+EINGABETASTE, um in einem Bearbeitungsfeld leere Zeilen hinzuzufügen. 
      
5. Speichern Sie Ihre Arbeit. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

   ![Fertige Logik-App](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

## <a name="run-your-logic-app-workflow"></a>Ausführen Ihres Logik-App-Workflows

Wählen Sie in der Symbolleiste des Designers die Option **Ausführen**, um Ihre Logik-App manuell zu starten. Andernfalls können Sie warten, bis die Logik-App gemäß dem von Ihnen festgelegten Zeitplan ausgeführt wird.

![Logik-App ausführen](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

Falls der RSS-Feed über neue Elemente verfügt, sendet Ihre Logik-App für jedes neue Element eine E-Mail. Hier ist beispielsweise ein Beispiel für eine Outlook-E-Mail angegeben, die von dieser Logik-App gesendet wird:

![Gesendete E-Mail bei neuem RSS-Feedelement](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Falls der Feed keine neuen Elemente enthält, überspringt die Logik-App den Schritt zum Senden einer E-Mail und wartet auf das nächste Intervall, um die Prüfung zu wiederholen. 

> [!TIP]
> Überprüfen Sie Ihren Ordner mit den Junk-E-Mails, falls Sie keine E-Mails erhalten. Wenn Sie unsicher sind, ob Ihre Logik-App richtig ausgeführt wurde, helfen Ihnen die Informationen unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Glückwunsch! Sie haben Ihre erste Logik-App erstellt und ausgeführt. In diesem Schnellstart wurde gezeigt, wie einfach und schnell Sie automatisierte Workflows zum Integrieren von Systemen und Diensten erstellen können.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ihre Logik-App wird weiter ausgeführt, und unter Umständen fallen Gebühren für Ihr Azure-Abonnement an, bis Sie die App deaktivieren oder löschen. Wenn Sie Verbindungen für Ihre Logik-App erstellen, bleiben diese außerdem auch nach dem Löschen der Logik-App erhalten. 

Wenn Sie fertig sind, sollten Sie unbedingt alle Ressourcen deaktivieren oder löschen, für deren Gebühren Sie nicht aufkommen möchten bzw. die Sie nicht beibehalten möchten. Sie können alle für diesen Schnellstart erstellten Ressourcen löschen, indem Sie die Azure-Ressourcengruppe löschen, die Sie für die Logik-App erstellt haben. 

### <a name="delete-resource-group"></a>Ressourcengruppe löschen

Falls Sie keinerlei Daten dieser Logik-App beibehalten möchten, können Sie die Ressourcengruppe, die Sie für diesen Schnellstart erstellt haben, und alle dazugehörigen Ressourcen löschen. Informieren Sie sich unter [Verwalten von Ressourcen](../azure-resource-manager/resource-group-portal.md#manage-resources) darüber, wie Sie Azure-Ressourcengruppen verwalten.

1. Wählen Sie im Azure-Menü die Option **Ressourcengruppen**.

2. Wählen Sie die Ressourcengruppe aus, die Sie löschen möchten. Wählen Sie im Ressourcengruppenmenü die Option **Übersicht**, falls sie noch nicht ausgewählt wurde. 

3. Prüfen Sie in der Gruppe, die Sie löschen möchten, alle Ressourcen. Wählen Sie in der Symbolleiste der Ressourcengruppe die Option **Ressourcengruppe löschen**, wenn Sie fertig sind.

### <a name="turn-off-logic-app"></a>Deaktivieren der Logik-App

Deaktivieren Sie Ihre Logik-App, um die Ausführung zu beenden, ohne Ihre Arbeit zu löschen. 

Wählen Sie in Ihrem Logik-App-Menü **Übersicht**. Wählen Sie auf der Symbolleiste **Deaktivieren** aus.

  ![Deaktivieren der Logik-App](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Wenn das Logik-App-Menü nicht angezeigt wird, können Sie zum Azure-Dashboard zurückwechseln und versuchen, Ihre Logik-App wieder zu öffnen.

### <a name="delete-logic-app"></a>Löschen der Logik-App

Sie können auch nur Ihre Logik-App löschen und alle dazugehörigen Ressourcen beibehalten, z.B. die von Ihnen erstellten Verbindungen.

1. Wählen Sie im Logik-App-Menü die Option **Übersicht**. Wählen Sie in der Symbolleiste die Option **Löschen**. 

   ![Löschen Ihrer Logik-App](./media/logic-apps-create-a-logic-app/delete-logic-app.png)

   > [!TIP]
   > Wenn das Logik-App-Menü nicht angezeigt wird, können Sie zum Azure-Dashboard zurückwechseln und versuchen, Ihre Logik-App wieder zu öffnen.

2. Bestätigen Sie, dass Sie die Logik-App löschen möchten, und wählen Sie dann die Option **Löschen**.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder Ihre Stimme abgeben möchten, können Sie dies auf der [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish) durchführen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Create logic app workflows from prebuilt templates](../logic-apps/logic-apps-create-logic-apps-from-templates.md) (Erstellen von Logik-App-Workflows aus vorgefertigten Vorlagen)
---
title: "Erstellen von Workflows zum Verarbeiten von E-Mails und Anlagen – Azure Logic Apps | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie, wie Sie mit Azure Logic Apps, Azure Storage und Azure Functions automatisierte Workflows für die Verarbeitung von E-Mails und Anlagen erstellen."
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 8c327599585e67ccc6ebdf849d3e9cf9b95e7398
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>Verarbeiten von E-Mails und Anlagen mit einer Logik-App

Mit Azure Logic Apps können Sie Workflows automatisieren und Daten übergreifend in Azure-Dienste, Microsoft-Dienste, andere SaaS-Apps (Software-as-a-Service) und lokale Systeme integrieren. In diesem Tutorial erfahren Sie, wie Sie eine [Logik-App](../logic-apps/logic-apps-overview.md) erstellen, die eingehende E-Mails und deren Anlagen verarbeitet. Die Logik-App verarbeitet den Inhalt, speichert ihn in Azure Storage und sendet Benachrichtigungen zur Prüfung des Inhalts. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten von [Azure Storage](../storage/common/storage-introduction.md) und dem Storage-Explorer zur Prüfung gespeicherter E-Mails und Anlagen
> * Erstellen einer [Azure-Funktion](../azure-functions/functions-overview.md), die HTML-Code aus E-Mails entfernt. (Das Tutorial enthält den Code, den Sie für diese Funktion verwenden können.)
> * Erstellen einer leeren Logik-App
> * Hinzufügen eines Triggers, der E-Mails auf Anlagen überwacht
> * Hinzufügen einer Bedingung, die überprüft, ob E-Mails über Anlagen verfügen
> * Hinzufügen einer Aktion, die die Azure-Funktion aufruft, wenn eine E-Mail über Anlagen verfügt
> * Hinzufügen einer Aktion, die Speicherblobs für E-Mails und Anlagen erstellt
> * Hinzufügen einer Aktion zum Senden von E-Mail-Benachrichtigungen

Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Allgemeine Darstellung der fertigen Logik-App](./media/tutorial-process-email-attachments-workflow/overview.png)

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein E-Mail-Konto eines von Logic Apps unterstützten E-Mail-Anbieters wie etwa Office 365 Outlook, Outlook.com oder Gmail. Informationen zu Connectors für andere Anbieter finden Sie in [dieser Liste](https://docs.microsoft.com/connectors/).

  In dieser Logik-App verwenden wir ein Office 365 Outlook-Konto. 
  Bei Verwendung eines anderen E-Mail-Kontos bleiben die allgemeinen Schritte zwar gleich, die Benutzeroberfläche sieht aber unter Umständen etwas anders aus.

* Laden Sie den kostenlosen <a href="http://storageexplorer.com/" target="_blank">Microsoft Azure Storage-Explorer</a> herunter, und installieren Sie ihn. Mit diesem Tool können Sie überprüfen, ob Ihr Speichercontainer ordnungsgemäß eingerichtet ist.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> an.

## <a name="set-up-storage-to-save-attachments"></a>Einrichten von Speicher zum Speichern von Anlagen

Eingehende E-Mails und Anlagen können als Blobs in einem [Azure-Speichercontainer](../storage/common/storage-introduction.md) gespeichert werden. 

1. [Erstellen Sie zunächst ein Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) mit den folgenden Einstellungen, um einen Speichercontainer erstellen zu können:

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Name** | attachmentstorageacct | Der Name Ihres Speicherkontos. | 
   | **Bereitstellungsmodell** | Resource Manager | Das [Bereitstellungsmodell](../azure-resource-manager/resource-manager-deployment-model.md) zum Verwalten der Ressourcenbereitstellung. | 
   | **Kontoart** | Allgemeiner Zweck | Der [Speicherkontotyp](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Leistung** | Standard | Diese Einstellung gibt die unterstützten Datentypen und die Medien für die Datenspeicherung an. Weitere Informationen finden Sie unter [Speicherkontentypen](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Replikation** | Lokal redundanter Speicher (LRS) | Diese Einstellung gibt an, wie Ihre Daten kopiert, gespeichert, verwaltet und synchronisiert werden. Weitere Informationen finden Sie unter [Replikation](../storage/common/storage-introduction.md#replication). | 
   | **Sichere Übertragung erforderlich** | Deaktiviert | Diese Einstellung gibt die erforderliche Sicherheit für Anforderungen von Verbindungen an. Weitere Informationen finden Sie unter [Vorschreiben einer sicheren Übertragung in Azure Storage](../storage/common/storage-require-secure-transfer.md). | 
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Der Name Ihres Azure-Abonnements | 
   | **Ressourcengruppe** | LA-Tutorial-RG | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die zum Organisieren und Verwalten verwandter Ressourcen verwendet wird. <p>**Hinweis:** Eine Ressourcengruppe befindet sich in einer bestimmten Region. Die Elemente in diesem Tutorial sind unter Umständen nicht in allen Regionen verfügbar. Versuchen Sie aber nach Möglichkeit, die gleiche Region zu verwenden. | 
   | **Location** | USA (Ost) 2 | Die Region, in der die Informationen zu Ihrem Speicherkonto gespeichert werden sollen. | 
   | **Konfigurieren virtueller Netzwerke** | Deaktiviert | Behalten Sie für dieses Tutorial die Einstellung **Deaktiviert** bei. | 
   |||| 

   Sie können auch [Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) oder die [Azure-Befehlszeilenschnittstelle](../storage/common/storage-quickstart-create-storage-account-cli.md) verwenden.
  
2. Nachdem Azure Ihr Speicherkonto bereitgestellt hat, können Sie Ihren Speicherkonto-Zugriffsschlüssel abrufen:

   1. Klicken Sie in Ihrem Speicherkontomenü unter **Einstellungen** auf **Zugriffsschlüssel**. 
   2. Suchen Sie unter **Standardschlüssel** und dem Namen Ihres Speicherkontos nach **key1**.

      ![Kopieren und Speichern von Speicherkontoname und -schlüssel](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   Sie können auch [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) oder die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list) verwenden. 

3. Erstellen Sie einen Speichercontainer für Ihre E-Mail-Anlagen.
   
   1. Klicken Sie in Ihrem Speicherkontomenü im Bereich **Übersicht** unter **Dienste** auf **Blobs** und anschließend auf **+ Container**.

   2. Geben Sie als Containername die Zeichenfolge „attachments“ ein. Wählen Sie unter **Öffentliche Zugriffsebene** die Option **Container (Anonymer Lesezugriff für Container und Blobs)** aus, und klicken Sie anschließend auf **OK**.

   Sie können auch [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) oder die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create) verwenden. 
   Wenn Sie die hier angegebenen Schritte ausgeführt haben, finden Sie Ihren Speichercontainer im Azure-Portal unter Ihrem Speicherkonto:

   ![Fertiger Speichercontainer](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Stellen Sie als Nächstes eine Storage-Explorer-Verbindung mit Ihrem Speicherkonto her.

## <a name="set-up-storage-explorer"></a>Einrichten des Storage-Explorers

Stellen Sie eine Storage-Explorer-Verbindung mit Ihrem Speicherkonto her, und vergewissern Sie sich, dass Ihre Logik-App Anlagen ordnungsgemäß als Blobs in Ihrem Speichercontainer speichert.

1. Öffnen Sie den Microsoft Azure-Storage-Explorer. Wenn Sie im Storage-Explorer zur Angabe einer Verbindung mit Azure Storage aufgefordert werden, klicken Sie auf **Use a storage account name and key** (Speicherkontoname und -schlüssel verwenden) > **Weiter**.
Sollte eine Aufforderung angezeigt werden, klicken Sie auf der Symbolleiste des Explorers auf **Konto hinzufügen**.

2. Geben Sie unter **Attach using Name and Key** (Mit Name und Schlüssel anfügen) den Namen Ihres Speicherkontos und den zuvor gespeicherten Zugriffsschlüssel ein. Klicken Sie auf **Weiter** > **Verbinden**.

3. Vergewissern Sie sich, dass Speicherkonto und Container ordnungsgemäß im Storage-Explorer angezeigt werden:

   1. Erweitern Sie unter **Explorer** die Knoten **(Local and Attached)** (Lokal und angefügt) > 
   **Speicherkonten** > **attachmentstorageaccount** > 
   **Blobcontainer**.

   2. Vergewissern Sie sich, dass der Container „attachments“ angezeigt wird. 
   Beispiel: 

      ![Storage-Explorer: Überprüfen des Speichercontainers](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Erstellen Sie als Nächstes eine [Azure-Funktion](../azure-functions/functions-overview.md), die HTML-Code aus eingehenden E-Mails entfernt.

## <a name="create-a-function-to-clean-html"></a>Erstellen einer Funktion zum Entfernen von HTML-Code

Erstellen Sie nun mithilfe des in diesen Schritten bereitgestellten Codeausschnitts eine Azure-Funktion, die HTML-Code aus jeder eingehenden E-Mail entfernt. Dadurch wird der E-Mail-Inhalt übersichtlicher und lässt sich leichter verarbeiten. Diese Funktion können Sie dann über Ihre Logik-App aufrufen.

1. [Erstellen Sie zunächst eine Funktions-App](../azure-functions/functions-create-function-app-portal.md) mit den folgenden Einstellungen, um eine Funktion erstellen können:

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **App-Name** | CleanTextFunctionApp | Ein global eindeutiger und aussagekräftiger Name für Ihre Funktions-App. | 
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Das gleiche Azure-Abonnement, das Sie auch zuvor verwendet haben. | 
   | **Ressourcengruppe** | LA-Tutorial-RG | Die gleiche Azure-Ressourcengruppe, die Sie auch zuvor verwendet haben. | 
   | **Hostingplan** | Verbrauchstarif | Diese Einstellung bestimmt die Zuordnung und Skalierung von Ressourcen (beispielsweise Rechenleistung) zum Ausführen Ihrer Funktions-App. Weitere Informationen finden Sie im [Vergleich der Hostingpläne](../azure-functions/functions-scale.md). | 
   | **Location** | USA (Ost) 2 | Die gleiche Region, die Sie auch zuvor verwendet haben. | 
   | **Speicher** | cleantextfunctionstorageacct | Erstellen Sie ein Speicherkonto für Ihre Funktions-App. Verwenden Sie nur Kleinbuchstaben und Zahlen. <p>**Hinweis:** Dieses Speicherkonto enthält Ihre Funktions-Apps und unterscheidet sich vom zuvor erstellten Speicherkonto für E-Mail-Anlagen. | 
   | **Application Insights** | Aus | Dient zum Aktivieren der Anwendungsüberwachung mit [Application Insights](../application-insights/app-insights-overview.md). Für dieses Tutorial behalten wir jedoch die Einstellung **Aus** bei. | 
   |||| 

   Falls Ihre Funktions-App nach der Bereitstellung nicht automatisch geöffnet wird, suchen Sie sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>. Klicken Sie im Azure-Hauptmenü auf **App Services**, und wählen Sie Ihre Funktions-App aus.

   ![Erstellte Funktions-App](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Sollte **App Services** im Azure-Menü nicht angezeigt werden, navigieren Sie stattdessen zu **Weitere Dienste**. Suchen Sie über das Suchfeld nach **Funktionen-Apps**. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md).

   Sie können auch die [Azure-Befehlszeilenschnittstelle](../azure-functions/functions-create-first-azure-function-azure-cli.md) oder [PowerShell und Resource Manager-Vorlagen](../azure-resource-manager/resource-group-template-deploy.md) verwenden.

2. Erweitern Sie unter **Funktionen-Apps** den Knoten **CleanTextFunctionApp**, und wählen Sie **Funktionen** aus. Klicken Sie auf der Funktionen-Symbolleiste auf **+ Neue Funktion**.

   ![Erstellen einer neuen Funktion](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. Wählen Sie unter **Wählen Sie unten eine Vorlage aus, oder wechseln Sie zum Schnellstart** die Funktionsvorlage **HttpTrigger - C#** aus.

   ![Auswählen der Funktionsvorlage](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. Geben Sie unter **Funktion benennen** Folgendes ein: ```RemoveHTMLFunction```. Behalten Sie unter **HTTP-Trigger** > **Autorisierungsstufe** den Standardwert **Funktion** bei, und klicken Sie auf **Erstellen**.

   ![Benennen Ihrer Funktion](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Ersetzen Sie den Vorlagencode im Editor durch den folgenden Code. Dieser entfernt den HTML-Code und gibt Ergebnisse an den Aufrufer zurück.

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. Wenn Sie fertig sind, wählen Sie **Speichern** aus. Klicken Sie unter dem Pfeilsymbol (**<**) am rechten Rand des Editors auf **Testen**, um die Funktion zu testen. 

   ![Öffnen des Testbereichs](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. Geben Sie im Bereich **Testen** unter **Anforderungstext** die folgende Zeile ein, und klicken Sie anschließend auf **Ausführen**:

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![Testen der Funktion](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   Im Fenster **Ausgabe** wird das folgende Ergebnis der Funktion angezeigt:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Nachdem Sie sich vergewissert haben, dass die Funktion funktioniert, können Sie Ihre Logik-App erstellen. In diesem Tutorial wird zwar gezeigt, wie Sie eine Funktion erstellen, die HTML-Code aus E-Mails entfernt, Logic Apps verfügt aber auch über einen Connector zum Konvertieren von **HTML in Text**.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Unternehmensintegration** > **Logik-App**.

   ![Erstellen einer Logik-App](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. Geben Sie unter **Logik-App erstellen** die Informationen wie beschrieben ein. Wenn Sie fertig sind, wählen Sie **An Dashboard anheften** > **Erstellen** aus.

   ![Angeben von Informationen zur Logik-App](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Name** | LA-ProcessAttachment | Der Name Ihrer Logik-App | 
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Das gleiche Azure-Abonnement, das Sie auch zuvor verwendet haben. | 
   | **Ressourcengruppe** | LA-Tutorial-RG | Die gleiche Azure-Ressourcengruppe, die Sie auch zuvor verwendet haben. |
   | **Location** | USA (Ost) 2 | Die gleiche Region, die Sie auch zuvor verwendet haben. | 
   | **Log Analytics** | Aus | Behalten Sie für dieses Tutorial die Einstellung **Aus** bei. | 
   |||| 

3. Nachdem Ihre App von Azure bereitgestellt wurde, wird der Designer für Logik-Apps geöffnet und eine Seite mit einem Einführungsvideo und Vorlagen für allgemeine Logik-App-Mustern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der Vorlage „Leere Logik-App“](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Fügen Sie als Nächstes einen [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der auf eingehende E-Mails mit Anlagen lauscht. Jede Logik-App beginnt mit einem Trigger, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder neue Daten eine bestimmte Bedingung erfüllen. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Überwachen eingehender E-Mails

1. Geben Sie in das Suchfeld des Designers den Text „Wenn E-Mail empfangen wird“ ein. Wählen Sie den folgenden Trigger für Ihren E-Mail-Anbieter aus: **<*Ihr E-Mail-Anbieter*> - Wenn eine neue E-Mail empfangen wird**. Beispiel:

   ![Auswählen des Triggers „Wenn eine neue E-Mail empfangen wird“ für den E-Mail-Anbieter](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“. 
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“. 

2. Falls Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich bei Ihrem E-Mail-Konto an, damit Logic Apps eine Verbindung mit Ihrem E-Mail-Konto herstellen kann.

3. Geben Sie nun die Kriterien an, die der Trigger zum Filtern neuer E-Mails verwendet.

   1. Geben Sie den Ordner, das Intervall und die Häufigkeit für die E-Mail-Überprüfung an.

      ![Angeben von Ordner, Intervall und Häufigkeit für die E-Mail-Überprüfung](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Einstellung | Wert | BESCHREIBUNG | 
      | ------- | ----- | ----------- | 
      | **Ordner** | Posteingang | Der zu überprüfende E-Mail-Ordner. | 
      | **Intervall** | 1 | Die Anzahl von Warteintervallen zwischen Überprüfungen | 
      | **Frequency** | Minute | Die Zeiteinheit für die Intervalle zwischen Überprüfungen. | 
      |  |  |  | 
  
   2. Klicken Sie auf **Erweiterte Optionen anzeigen**, und geben Sie folgende Einstellungen an:

      | Einstellung | Wert | BESCHREIBUNG | 
      | ------- | ----- | ----------- | 
      | **Mit Anlage** | Ja | Ruft nur E-Mails mit Anlagen ab. <p>**Hinweis:** Der Trigger entfernt keine E-Mails aus Ihrem Konto. Er überprüft nur neue Nachrichten und verarbeitet nur E-Mails, die dem Betrefffilter entsprechen. | 
      | **Anlagen einschließen** | Ja | Ruft die Anlagen als Eingabe für Ihren Workflow ab, anstatt nur nach Anlagen zu suchen. | 
      | **Filter für Betreff** | ```Business Analyst 2 #423501``` | Der Text, nach dem im Betreff der E-Mail gesucht werden soll. | 
      |  |  |  | 

4. Klicken Sie auf die Titelleiste des Triggers, um die Triggerdetails vorerst auszublenden.

   ![Ausblenden der Details durch Reduzieren des Bereichs](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

   Ihre Logik-App befindet sich jetzt im Livemodus, überprüft bislang aber lediglich Ihre E-Mails. 
   Als Nächstes fügen Sie eine Bedingung hinzu, die Kriterien für die Fortsetzung des Workflows angibt.

## <a name="check-for-attachments"></a>Überprüfen auf Anlagen

1. Klicken Sie unter dem Trigger auf **+ Neuer Schritt** > **Bedingung hinzufügen**.

   Der Bedingungsbereich wird standardmäßig entweder mit der Parameterliste oder mit der Liste mit den dynamischen Inhalten angezeigt, die alle Parameter aus dem vorherigen Schritt enthält, die Sie als Workfloweingabe einbeziehen können. 
   Die Breite des Browserfensters bestimmt, welche Liste angezeigt wird.

2. Versehen Sie die Bedingung mit einem aussagekräftigeren Namen.

   1. Klicken Sie auf der Titelleiste der Bedingung auf die **Schaltfläche mit den Auslassungspunkten** (**...**) und anschließend auf **Umbenennen**.

      Beispiel für die schmale Browseransicht:

      ![Umbenennen der Bedingung](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Wenn Sie die breite Browseransicht verwenden und die Liste mit den dynamischen Inhalten die Schaltfläche mit den Auslassungspunkten verdeckt, klicken Sie innerhalb der Bedingung auf **Dynamischen Inhalt hinzufügen**, um die Liste zu schließen. 
      
      ![Schließen der Liste mit den dynamischen Inhalten](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Nennen Sie Ihre Bedingung wie folgt: ```If email has attachments and key subject phrase```

3. Beschreiben Sie die Bedingung durch Angabe eines Ausdrucks. 

   1. Klicken Sie im Bedingungsbereich auf **Im erweiterten Modus bearbeiten**.

      ![Bearbeiten der Bedingung im erweiterten Modus](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. Geben Sie den folgenden Ausdruck in das Textfeld ein:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      Dieser Ausdruck vergleicht den Eigenschaftswert von **HasAttachment** aus dem Triggertext (in diesem Tutorial: die E-Mail) mit dem booleschen Objekt ```True```. 
      Sind beide Werte gleich, besitzt die E-Mail mindestens eine Anlage, die Bedingung ist erfüllt, und der Workflow wird fortgesetzt.

      Nun sieht Ihre Bedingung wie im folgendem Beispiel aus:

      ![Bedingungsausdruck](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Klicken Sie auf **Im Standardmodus bearbeiten**. Der Ausdruck wird nun wie hier gezeigt aufgelöst:

      ![Aufgelöster Ausdruck](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > Zur manuellen Erstellung eines Ausdrucks müssen Sie sich im Standardmodus befinden und die dynamische Liste geöffnet haben, um den Ausdrucks-Generator verwenden zu können. Unter **Ausdruck** können Sie Funktionen auswählen. Unter **Dynamischer Inhalt** können Sie Parameterfelder für diese Funktionen auswählen.
      > Die manuelle Ausdruckserstellung wird später in diesem Tutorial gezeigt.

4. Speichern Sie Ihre Logik-App.

### <a name="test-your-condition"></a>Testen der Bedingung

Testen Sie nun, ob die Bedingung ordnungsgemäß funktioniert:

1. Falls Ihre Logik-App noch nicht ausgeführt wird, klicken Sie auf der Symbolleiste des Designers auf **Ausführen**.

   Dadurch wird Ihre Logik-App manuell gestartet, ohne auf den Ablauf des angegebenen Intervalls zu warten. 
   Bis zum Eingang der Test-E-Mail Ihrem Posteingang passiert allerdings nichts. 

2. Senden Sie sich selbst eine E-Mail, die folgende Kriterien erfüllt:

   * Der E-Mail-Betreff enthält den Text, den Sie unter **Filter für Betreff** des Triggers angegeben haben: ```Business Analyst 2 #423501```

   * Ihre E-Mail verfügt über eine Anlage. 
   Erstellen Sie vorerst einfach eine leere Textdatei, und fügen Sie sie an Ihre E-Mail an.

   Wenn die E-Mail eingeht, wird sie von Ihrer Logik-App auf Anlagen und den angegebenen Betrefftext überprüft.
   Ist die Bedingung erfüllt, wird der Trigger ausgelöst, woraufhin das Logic Apps-Modul eine Logik-App-Instanz erstellt und den Workflow startet. 

3. Klicken Sie im Logik-App-Menü auf **Übersicht**, und vergewissern Sie sich, dass der Trigger ausgelöst und die Logik-App erfolgreich ausgeführt wurde.

   ![Überprüfen von Trigger- und Ausführungsverlauf](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Wurde Ihre Logik-App nicht ausgelöst oder trotz eines erfolgreichen Triggers nicht ausgeführt, lesen Sie unter [Behandeln und Diagnostizieren von Logik-App-Fehlern](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Als Nächstes definieren Sie die Aktionen, die für die Verzweigung **Bei TRUE** ausgeführt werden sollen. Entfernen Sie jeglichen HTML-Code aus dem E-Mail-Text, und erstellen Sie anschließend Blobs im Speichercontainer für die E-Mail und die Anlagen, um die E-Mail zusammen mit sämtlichen Anlagen zu speichern.

> [!NOTE]
> Für die Verzweigung **Bei FALSE** muss die Logik-App keinerlei Aktionen ausführen, da die E-Mail dann keine Anlagen enthält. Als Zusatzübung können Sie nach Abschluss dieses Tutorials für die Verzweigung **Bei FALSE** eine beliebige geeignete Aktion hinzufügen.

## <a name="call-the-removehtmlfunction"></a>Aufrufen der Funktion „RemoveHTMLFunction“

1. Klicken Sie im Logik-App-Menü auf **Logik-App-Designer**. Klicken in der Verzweigung **Bei TRUE** auf **Aktion hinzufügen**.

2. Suchen Sie nach „Azure Functions“, und wählen Sie die folgende Aktion aus: **Azure Functions – Azure-Funktion wählen**

   ![Auswählen einer Aktion für „Azure Functions - Azure-Funktion wählen“](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Wählen Sie die Funktions-App **CleanTextFunctionApp** aus, die Sie zuvor erstellt haben.

   ![Auswählen Ihrer Azure-Funktions-App](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Wählen Sie nun Ihre Funktion (**RemoveHTMLFunction**) aus.

   ![Auswählen Ihrer Azure-Funktion](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Benennen Sie den Funktionsbereich wie folgt um: ```Call RemoveHTMLFunction to clean email body``` 

6. Geben Sie im Funktionsbereich die Eingabe ein, die Ihre Funktion verarbeiten soll. Geben Sie den E-Mail-Text wie hier dargestellt und beschrieben an:

   ![Angeben des zu erwartenden Anforderungstexts für die Funktion](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. Geben Sie unter **Anforderungstext** den folgenden Text ein: 
   
      ```{ "emailBody": ``` 

      Während der nächsten Schritte erscheint eine Fehlermeldung mit einem Hinweis auf ungültigen JSON-Code, solange diese Eingabe nicht abgeschlossen ist.
      Bei vorherigen Tests dieser Funktion wurde für die Eingabe, die für diese Funktion angegeben wurde, das JSON-Format (JavaScript Object Notation) verwendet. 
      Der Anforderungstext muss daher das gleiche Format besitzen. 

   2. Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Wenn eine neue E-Mail empfangen wird** das Feld **Text** aus.
   Fügen Sie nach dem Feld **Text** die schließende geschweifte Klammer hinzu: ```}```

      ![Angeben des Anforderungstexts, der an die Funktion übergeben werden soll](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      In der Definition der Logik-App wird dieser Eintrag im folgenden Format angezeigt:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Aktion hinzu, die in Ihrem Speichercontainer ein Blob zum Speichern des E-Mail-Texts erstellt.

## <a name="create-blob-for-email-body"></a>Erstellen eines Blobs für E-Mail-Text

1. Klicken Sie im Azure-Funktionsbereich auf **Aktion hinzufügen**. 

2. Suchen Sie unter **Aktion auswählen** nach „blob“, und wählen Sie die folgende Aktion aus: **Azure Blob Storage – Blob erstellen**

   ![Hinzufügen einer Aktion zum Erstellen eines Blobs für E-Mail-Text](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Sollte keine Verbindung mit einem Azure-Speicherkonto bestehen, erstellen Sie eine Verbindung mit Ihrem Speicherkonto, und verwenden Sie dabei die hier gezeigten und beschriebenen Einstellungen. Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

   ![Erstellen einer Speicherkontoverbindung](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Verbindungsname** | AttachmentStorageConnection | Ein aussagekräftiger Name für die Verbindung. | 
   | **Speicherkonto** | attachmentstorageacct | Der Name des Speicherkontos, das Sie zuvor zum Speichern von Anlagen erstellt haben. | 
   |||| 

4. Benennen Sie die Aktion **Blob erstellen** wie folgt um: ```Create blob for email body```

5. Geben Sie in der Aktion **Blob erstellen** die folgenden Informationen an, und wählen Sie die folgenden Parameter aus, um das Blob wie hier dargestellt und beschrieben zu erstellen:

   ![Angeben von Blobinformationen für E-Mail-Text](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Ordnerpfad** | /attachments | Pfad und Name des zuvor erstellten Containers. Sie können auch zu einem Container navigieren und ihn auswählen. | 
   | **Blobname** | Feld **Von** | Übergeben Sie den Namen des E-Mail-Absenders als Blobname. Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Wenn eine neue E-Mail empfangen wird** das Feld **Von** aus. | 
   | **Blobinhalt** | Feld **Inhalt** | Übergeben Sie den HTML-freien E-Mail-Text als Blobinhalt. Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Call RemoveHTMLFunction to clean email body** die Option **Text** aus. |
   |||| 

6. Speichern Sie Ihre Logik-App. 

### <a name="check-attachment-handling"></a>Überprüfen der Behandlung von Anlagen

Testen Sie nun, ob E-Mails von Ihrer Logik-App wie gewünscht behandelt werden:

1. Falls Ihre Logik-App noch nicht ausgeführt wird, klicken Sie auf der Symbolleiste des Designers auf **Ausführen**.

2. Senden Sie sich selbst eine E-Mail, die folgende Kriterien erfüllt:

   * Der E-Mail-Betreff enthält den Text, den Sie unter **Filter für Betreff** des Triggers angegeben haben: ```Business Analyst 2 #423501```

   * Ihre E-Mail verfügt über mindestens eine Anlage. 
   Erstellen Sie vorerst einfach eine leere Textdatei, und fügen Sie sie an Ihre E-Mail an.

   * Die E-Mail enthält etwas Text zu Testzwecken. Beispiel: 

     ```
     Testing my logic app
     ```

   Wurde Ihre Logik-App nicht ausgelöst oder trotz eines erfolgreichen Triggers nicht ausgeführt, lesen Sie unter [Behandeln und Diagnostizieren von Logik-App-Fehlern](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

3. Vergewissern Sie sich, dass die Logik-App die E-Mail im richtigen Speichercontainer gespeichert hat. 

   1. Erweitern Sie im Storage-Explorer die Knoten **(Local and Attached)** (Lokal und angefügt) > 
   **Speicherkonten** > **attachmentstorageacct (Extern)** > 
   **Blobcontainer** > **attachments**.

   2. Überprüfen Sie, ob der Container **attachments** die E-Mail enthält. 

      Bislang enthält der Container lediglich die E-Mail, da die Logik-App die Anlagen noch nicht verarbeitet.

      ![Prüfen auf gespeicherte E-Mail im Storage-Explorer](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Löschen Sie die E-Mail im Storage-Explorer, wenn Sie fertig sind.

4. Optional: Zum Testen der Verzweigung **Bei FALSE** (die momentan noch nichts bewirkt) können Sie eine E-Mail senden, die die Kriterien nicht erfüllt.

Fügen Sie als Nächstes eine Schleife für die Verarbeitung aller E-Mail-Anlagen hinzu.

## <a name="process-attachments"></a>Verarbeiten der Anlagen

In dieser Logik-App wird zur Verarbeitung der einzelnen E-Mail-Anlagen eine **For Each**-Schleife verwendet.

1. Klicken Sie unter dem Bereich **Create blob for email body** auf **... Mehr** und anschließend auf den folgenden Befehl: **"For Each"-Schleife hinzufügen**

   ![Hinzufügen einer For Each-Schleife](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Nennen Sie Ihre Schleife wie folgt: ```For each email attachment```

3. Geben Sie nun die Daten an, die die Schleife verarbeiten soll. Klicken Sie auf das Feld **Ausgabe von vorherigen Schritten auswählen**. Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten die Option **Anlagen** aus. 

   ![Auswählen von „Anlagen“](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   Das Feld **Anlagen** übergibt ein Array mit allen Anlagen einer E-Mail. 
   Die **For Each**-Schleife wiederholt Aktionen für jedes Element, das über das Array übergeben wird.

4. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes die Aktion hinzu, die jede Anlage als Blob in Ihrem Speichercontainer **attachments** speichert.

## <a name="create-blobs-for-attachments"></a>Erstellen von Blobs für Anlagen

1. Klicken Sie in der **For Each**-Schleife auf **Aktion hinzufügen**, um die Aufgabe anzugeben, die für jede gefundene Anlage ausgeführt werden soll.

   ![Hinzufügen einer Aktion zur Schleife](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. Suchen Sie unter **Aktion auswählen** nach „blob“, und wählen Sie anschließend die folgende Aktion aus: **Azure Blob Storage – Blob erstellen**

   ![Hinzufügen einer Aktion zum Erstellen eines Blobs](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Benennen Sie die Aktion **Blob erstellen 2** wie folgt um: ```Create blob for each email attachment```

4. Geben Sie in der Aktion **Create blob for each email attachment** die folgenden Informationen an, und wählen Sie die Parameter aus, um die einzelnen Blobs wie hier dargestellt und beschrieben zu erstellen:

   ![Angeben der Blobinformationen](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Ordnerpfad** | /attachments | Pfad und Name des zuvor erstellten Containers. Sie können auch zu einem Container navigieren und ihn auswählen. | 
   | **Blobname** | Feld **Name** | Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten die Option **Name** aus, um den Namen der Anlage für den Blobnamen zu übergeben. | 
   | **Blobinhalt** | Feld **Inhalt** | Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten die Option **Inhalt** aus, um den Inhalt der Anlage für den Blobinhalt zu übergeben. |
   |||| 

5. Speichern Sie Ihre Logik-App. 

### <a name="check-attachment-handling"></a>Überprüfen der Behandlung von Anlagen

Testen Sie als Nächstes, ob die Anlagen von Ihrer Logik-App wie gewünscht behandelt werden:

1. Falls Ihre Logik-App noch nicht ausgeführt wird, klicken Sie auf der Symbolleiste des Designers auf **Ausführen**.

2. Senden Sie sich selbst eine E-Mail, die folgende Kriterien erfüllt:

   * Der E-Mail-Betreff enthält den Text, den Sie unter **Filter für Betreff** des Triggers angegeben haben: ```Business Analyst 2 #423501```

   * Ihre E-Mail verfügt über mindestens zwei Anlagen. 
   Erstellen Sie vorerst einfach zwei leere Textdateien, und fügen Sie sie an Ihre E-Mail an.

   Wurde Ihre Logik-App nicht ausgelöst oder trotz eines erfolgreichen Triggers nicht ausgeführt, lesen Sie unter [Behandeln und Diagnostizieren von Logik-App-Fehlern](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

3. Vergewissern Sie sich, dass die Logik-App die E-Mail und die Anlagen im richtigen Speichercontainer gespeichert hat. 

   1. Erweitern Sie im Storage-Explorer die Knoten **(Local and Attached)** (Lokal und angefügt) > 
   **Speicherkonten** > **attachmentstorageacct (Extern)** > 
   **Blobcontainer** > **attachments**.

   2. Überprüfen Sie, ob der Container **attachments** sowohl die E-Mail als auch die Anlagen enthält.

      ![Überprüfen, ob E-Mail und Anlagen gespeichert wurden](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Löschen Sie die E-Mail und die Anlagen im Storage-Explorer, wenn Sie fertig sind.

Fügen Sie als Nächstes eine Aktion hinzu, damit Ihre Logik-App E-Mails zur Überprüfung der Anlagen sendet.

## <a name="send-email-notifications"></a>Senden von E-Mail-Benachrichtigungen

1. Klicken Sie in der Verzweigung **Bei TRUE** unter der Schleife **For each email attachment** auf **Aktion hinzufügen**. 

   ![Hinzufügen einer Aktion unter der For Each-Schleife](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. Suchen Sie unter **Aktion auswählen**, nach „E-Mail senden“, und wählen Sie dann für den gewünschten E-Mail-Anbieter die Aktion „E-Mail senden“ aus. Um die Liste der Aktionen nach einem bestimmten Dienst zu filtern, können Sie zuerst unter **Connectors** den Connector auswählen.

   ![Auswählen der Aktion „E-Mail senden“ für Ihren E-Mail-Anbieter](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“. 
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“. 

3. Falls Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich bei Ihrem E-Mail-Konto an, damit Logic Apps eine Verbindung mit Ihrem E-Mail-Konto erstellt.

4. Benennen Sie die Aktion **E-Mail senden** wie folgt um: ```Send email for review```

5. Geben Sie wie hier gezeigt und beschrieben die Informationen für diese Aktion an, und wählen Sie die Felder aus, die Sie in die E-Mail aufnehmen möchten. Drücken Sie UMSCHALT+EINGABETASTE, um in einem Bearbeitungsfeld leere Zeilen hinzuzufügen.  

   Beispiel für die Liste mit den dynamischen Inhalten:

   ![Senden von E-Mail-Benachrichtigungen](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Sollte die Liste ein erwartetes Feld nicht enthalten, klicken Sie in der Liste mit den dynamischen Inhalten oder am Ende der Parameterliste neben **Wenn eine neue E-Mail empfangen wird** auf **Mehr anzeigen**.

   | Einstellung | Wert | Notizen | 
   | ------- | ----- | ----- | 
   | **An** | <*E-Mail-Adresse des Empfängers*> | Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. | 
   | **Betreff**  | ```ASAP - Review applicant for position: ``` **Betreff** | Der Betreff der E-Mail, den Sie einschließen möchten. Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Wenn eine neue E-Mail empfangen wird** das Feld **Betreff** aus. | 
   | **Text** | ```Please review new applicant:``` <p>```Applicant name: ``` **Von** <p>```Application file location: ``` **Pfad** <p>```Application email content: ``` **Text** | Der Inhalt des E-Mail-Texts. Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten die folgenden Felder aus: <p>- Das Feld **Von** (unter **Wenn eine neue E-Mail empfangen wird**) </br>- Das Feld **Pfad** (unter **Create blob for email body**) </br>- Das Feld **Text** (unter **Call RemoveHTMLFunction to clean email body**) | 
   |||| 

   Wenn Sie ein Feld mit einem Array auswählen (beispielsweise **Inhalt** – ein Array mit Anlagen), schließt der Designer die Aktion, die auf dieses Feld verweist, automatisch in eine For Each-Schleife ein. 
   Auf diese Weise kann Ihre Logik-App diese Aktion für jedes Arrayelement durchführen. 
   Wenn Sie die Schleife entfernen möchten, entfernen Sie das Feld für das Array, verschieben Sie die verweisende Aktion aus der Schleife, klicken Sie auf der Titelleiste der Schleife auf die Auslassungspunkte (**...**), und klicken Sie anschließend auf **Löschen**.
     
6. Speichern Sie Ihre Logik-App. 

Testen Sie als Nächstes Ihre Logik-App, die nun wie im folgenden Beispiel aussieht:

![Fertiggestellte Logik-App](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

1. Senden Sie sich selbst eine E-Mail, die folgende Kriterien erfüllt:

   * Der E-Mail-Betreff enthält den Text, den Sie unter **Filter für Betreff** des Triggers angegeben haben: ```Business Analyst 2 #423501```

   * Ihre E-Mail verfügt über mindestens eine Anlage. 
   Sie können eine leere Textdatei aus dem vorherigen Test wiederverwenden. 
   Sie können aber auch eine Datei mit einem Lebenslauf anfügen, um das Szenario realistischer zu gestalten.

   * Die E-Mail enthält folgenden Text, den Sie kopieren und in die E-Mail einfügen können:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Führen Sie Ihre Logik-App aus. Bei erfolgreicher Ausführung sendet Ihre Logik-App Ihnen eine E-Mail, die wie folgt aussieht:

   ![Von der Logik-App gesendete E-Mail-Benachrichtigung](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Sollten Sie keine E-Mails erhalten, überprüfen Sie Ihren Ordner für Junk-E-Mails. 
   E-Mails dieser Art werden unter Umständen durch Ihren Junk-E-Mail-Filter umgeleitet. 
   Wenn Sie unsicher sind, ob Ihre Logik-App richtig ausgeführt wurde, helfen Ihnen die Informationen unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Geschafft! Sie haben eine Logik-App erstellt und ausgeführt, die Aufgaben über verschiedene Azure-Dienste hinweg automatisiert und benutzerdefinierten Code aufruft.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie sie nicht mehr benötigen, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen. Wechseln Sie im Azure-Hauptmenü zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe für Ihre Logik-App aus. Klicken Sie auf **Ressourcengruppe löschen**. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und klicken Sie auf **Löschen**.

![Löschen der Ressourcengruppe für die Logik-App](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Logik-App erstellt, die E-Mail-Anlagen verarbeitet und speichert, und dabei Azure-Dienste wie Azure Storage und Azure Functions integriert. Informieren Sie sich als Nächstes ausführlicher über andere Connectors für die Erstellung von Logik-Apps.

> [!div class="nextstepaction"]
> Weitere Informationen zu Connectors für Logic Apps finden Sie [hier](../connectors/apis-list.md).

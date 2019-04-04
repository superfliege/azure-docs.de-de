---
title: 'Herstellen einer Verbindung zu SAP-Systemen: Azure Logic Apps | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, die Sie auf SAP-Ressourcen zugreifen und diese verwalten, indem Sie Workflows mit Azure Logic Apps automatisieren.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 09/14/2018
tags: connectors
ms.openlocfilehash: 468e73c64037a76da612cba8d6c2e9507dd3ac87
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57887084"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Herstellen einer Verbindung zu SAP-Systemen: Azure Logic Apps

In diesem Artikel wird gezeigt, wie Sie aus einer Logik-App auf Ihre lokalen SAP-Ressourcen zugreifen, indem Sie den SAP ECC-Connector (ERP Central Component) verwenden. Der Connector funktioniert lokal sowohl in ECC- als auch in S/4 HANA-Systemen. Der SAP ECC-Connector unterstützt die Nachrichten- oder Datenintegration in SAP Netweaver-basierten Systemen über Intermediate Document (IDoc), Business Application Programming Interface (BAPI) oder Remote Function Call (RFC).

Der SAP ECC-Connector verwendet die <a href="https://support.sap.com/en/product/connectors/msnet.html">SAP-NCo-Bibliothek (.NET-Connector)</a> und stellt diese Vorgänge oder Aktionen bereit:

- **An SAP senden:** Hiermit senden Sie IDoc-Funktionen oder rufen BAPI-Funktionen über tRFC in SAP-Systemen auf.
- **Von SAP empfangen:** Hiermit empfangen Sie IDoc- oder BAPI-Funktionsaufrufe von SAP-Systemen über tRFC.
- **Schemas generieren:** Hiermit generieren Sie Schemas für die SAP-Artefakte für IDoc, BAPI oder RFC.

Der SAP-Connector kann über das [lokale Datengateway](https://www.microsoft.com/download/details.aspx?id=53127) in lokale SAP-Systeme integriert werden. In Sendeszenarien, in denen beispielsweise eine Nachricht von Logic Apps an ein SAP-System gesendet werden, agiert das Datengateway als RFC-Client und leitet die von Logic Apps erhaltenen Anforderungen an SAP weiter.
Ebenso fungiert das Datengateway in Empfangsszenarien als RFC-Server, der Anforderungen von SAP empfängt und an die Logik-App weiterleitet. 

In diesem Artikel wird das Erstellen von in SAP integrierbaren Beispiel-Logik-Apps gezeigt. Gleichzeitig werden die zuvor beschriebenen Integrationsszenarien veranschaulicht.

## <a name="prerequisites"></a>Voraussetzungen

Um diesem Artikel weiter folgen zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, <a href="https://azure.microsoft.com/free/" target="_blank">melden Sie sich für ein kostenloses Azure-Konto an</a>.

* Die Logik-App, von der aus Sie auf Ihr SAP-System zugreifen möchten, und einen Trigger, der Ihren Logik-App-Workflow startet. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ihr <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP-Anwendungsserver</a> oder der <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP-Nachrichtenserver</a>

* Laden Sie das neueste [lokalen Datengateway](https://www.microsoft.com/download/details.aspx?id=53127) auf einem lokalen Computer herunter, und installieren Sie es. Bevor Sie fortfahren, sollten Sie prüfen, ob Ihr Gateway im Azure-Portal eingerichtet ist. Das Gateway hilft Ihnen, sicher auf lokale Daten und Ressourcen vor Ort zuzugreifen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem lokalen SAP-System in Logik-Apps mit dem SAP-Connector](../logic-apps/logic-apps-gateway-install.md).

* Laden Sie die neueste SAP-Clientbibliothek herunter, zurzeit <a href="https://softwaredownloads.sap.com/file/0020000001865512018" target="_blank">SAP-Connector (NCo) 3.0.21.0 für Microsoft .NET Framework 4.0 und Windows 64bit (x64)</a>, und installieren Sie es auf demselben Computer wie das lokale Datengateway. Installieren Sie diese Version oder eine höhere aus folgenden Gründen:

  * Bei früheren SAP-NCo-Versionen konnte es zu einem Deadlock kommen, wenn mehrere IDoc-Nachrichten gleichzeitig gesendet werden. 
  Dadurch werden auch alle späteren Nachrichten blockiert, die an das SAP-Ziel gesendet werden, und es kommt zu einem Timeout der Nachricht.

  * Über das lokale Datengateway können nur 64-Bit-Systemen ausgeführt werden. 
  Andernfalls erhalten Sie einen Fehler „Ungültige Image“, weil der Gateway-Hostdienst 32-Bit-Assemblys nicht unterstützt.

  * Sowohl der Datengateway-Hostdienst als auch der Microsoft-SAP-Adapter verwenden .NET Framework 4.5. Der SAP .NET-Connector für .NET Framework 4.0 funktioniert mit Prozessen, die .NET Common Language Runtime 4.0 bis 4.7.1 verwenden. 
  Der SAP .NET-Connector für .NET Framework 2.0 funktioniert mit Prozessen, die .NET Runtime 2.0 bis 3.5 verwenden. Er kann jedoch nicht mehr mit dem aktuellen lokalen Datengateway verwendet werden.

* Nachrichteninhalte, die Sie an Ihren SAP-Server senden können, z.B. eine Beispiel-IDoc-Datei. Dieser Inhalt muss im XML-Format vorliegen und den Namespace für die SAP-Aktion enthalten, die Sie verwenden möchten.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Senden an SAP

In diesem Beispiel wird eine Logik-App verwendet, die Sie mit einer HTTP-Anforderung auslösen können. Die Logik-App sendet ein Intermediate Document (IDoc) an einen SAP-Server und gibt eine Antwort an den Anforderer zurück, der die Logik-App aufgerufen hat. 

### <a name="add-http-request-trigger"></a>Hinzufügen eines HTTP-Anforderungstriggers

In Azure Logic Apps muss jede Logik-App mit einem [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz und startet die Ausführung des Workflows Ihrer App.

In diesem Beispiel erstellen Sie eine Logik-App mit einem Endpunkt in Azure, damit Sie *HTTP POST-Anforderungen* an Ihre Logik-App senden können. Wenn Ihre Logik-App diese HTTP-Anforderungen empfängt, wird der Trigger ausgelöst und führt den nächsten Schritt im Workflow aus.

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine leere Logik-App, die den Logik-App-Designer öffnet. 

2. Geben Sie im Suchfeld den Begriff „http request“ als Filter ein. Wählen Sie in der Triggerliste den folgenden Trigger aus: **Anforderung - Beim Empfang einer HTTP-Anforderung**

   ![Hinzufügen eines HTTP-Anforderungstriggers](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Speichern Sie jetzt Ihre Logik-App, damit Sie eine Endpunkt-URL dafür erstellen können.
Wählen Sie auf der Symbolleiste des Designers **Speichern**. 

   Die Endpunkt-URL wird nun im Trigger angezeigt, z.B.:

   ![URL für den Endpunkt generieren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>Hinzufügen einer SAP-Aktion

In Azure Logic Apps handelt es sich bei einer [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) um einen Schritt in Ihrem Workflow, der einem Trigger oder einer anderen Aktion folgt. Wenn Sie Ihrer Logik-App noch keinen Trigger hinzugefügt haben und mit diesem Beispiel weitermachen möchten, [fügen Sie den Trigger wie in diesem Abschnitt beschrieben hinzu](#add-trigger).

1. Klicken Sie im Logik-App-Designer unter dem Trigger auf **+ Neuer Schritt** > **Aktion hinzufügen**.

   ![Hinzufügen einer Aktion](./media/logic-apps-using-sap-connector/add-action.png) 

2. Geben Sie im Suchfeld den Begriff „sap“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Nachricht an SAP senden**
  
   ![SAP-Sendeaktion auswählen](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Anstelle der Suchaktion wählen Sie alternativ die Registerkarte **Unternehmen** und dann die SAP-Aktion aus.

   ![SAP-Sendeaktion auf der Registerkarte „Unternehmen“ auswählen](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

3. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, erstellen Sie jetzt Ihre SAP-Verbindung. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können. 

   **Erstellen einer lokalen SAP-Verbindung**

   1. Stellen Sie die Verbindungsinformationen für Ihren SAP-Server bereit. 
   Wählen Sie für die Eigenschaft **Datengateway** das Datengateway aus, das Sie im Azure-Portal für Ihre Gatewayinstallation erstellt haben.

      Wenn die Eigenschaft **Anmeldetyp** auf **Anwendungsserver** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

      ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Wenn die Eigenschaft **Anmeldetyp** auf **Gruppe** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich: 

      ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Wenn Sie fertig sind, wählen Sie **Erstellen** aus. 
   
      Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

4. Suchen Sie nun eine Aktion für Ihren SAP-Server, und wählen Sie sie aus. 

   1. Klicken Sie im Feld **SAP action** (SAP-Aktion) auf das Ordnersymbol. 
   Suchen Sie in der Dateiliste nach der gewünschten SAP-Nachricht, und wählen Sie sie aus. 
   Verwenden Sie die Pfeile zum Durchlaufen der Liste.

      In diesem Beispiel wird ein IDoc mit dem Typ **Auftrag** ausgewählt. 

      ![Aktion „IDoc“ suchen und auswählen](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Wenn Sie die Aktion, die Sie verwenden möchten, nicht finden können, können Sie einen Pfad auch manuell eingeben:

      ![Pfad zur Aktion „IDoc“ manuell bereitstellen](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Geben Sie den Wert für die SAP-Aktion über den Ausdrucks-Editor ein. Auf diese Weise können Sie dieselbe Aktion für verschiedene Nachrichtentypen verwenden.

      Weitere Informationen zu IDoc-Vorgängen finden Sie unter [Nachrichtenschemas für IDOC-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   2. Klicken Sie in das Feld **Input Message** (Eingabenachricht), damit die dynamische Inhaltsliste angezeigt wird. 
   Klicken Sie in dieser Liste unter **Beim Empfang einer HTTP-Anforderung** auf das Feld **Hauptteil**. 

      Dadurch wird der Hauptteil des HTTP-Anforderungstriggers eingefügt und diese Ausgabe an Ihren SAP-Server gesendet.

      ![Feld „Hauptteil“ auswählen](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Ihre SAP-Aktion sieht dann wie im folgenden Beispiel aus:

      ![Vollständige SAP-Aktion](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>Hinzufügen der HTTP-Antwortaktion

Fügen Sie nun dem Workflow Ihrer Logik-App eine Antwortaktion hinzu, und fügen Sie die Ausgabe der SAP-Aktion ein. So gibt die Logik-App die Ergebnisse Ihres SAP-Servers an den ursprünglichen Anforderer zurück. 

1. Klicken Sie im Logik-App-Designer unter der SAP-Aktion auf die Optionen **+ Neuer Schritt** > **Aktion hinzufügen**.

2. Geben Sie im Suchfeld den Begriff „Antwort“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Anforderungsantwort**

3. Klicken Sie in das Feld **Hauptteil**, damit die dynamische Inhaltsliste angezeigt wird. Wählen Sie aus dieser Liste unter **Send to SAP** (An SAP senden) das Feld **Hauptteil** aus. 

   ![Vollständige SAP-Aktion](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

4. Speichern Sie Ihre Logik-App. 

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wenn Ihre Logik-App nicht bereits aktiviert ist, klicken Sie im Menü der Logik-App auf **Überblick**. Klicken Sie in der Symbolleiste auf **Aktivieren**. 

2. Klicken Sie in der Symbolleiste des Logik-App-Designers auf **Ausführen**. Dadurch wird die Logik-App manuell gestartet.

3. Lösen Sie Ihre Logik-App aus, indem Sie eine HTTP POST-Anforderung an die URL im HTTP-Anforderungstrigger senden, und fügen Sie den Nachrichteninhalt in Ihre Anforderung ein. Um die Anforderung zu senden, können Sie ein Tool wie [Postman](https://www.getpostman.com/apps) nutzen. 

   In diesem Artikel sendet die Anforderung eine IDoc-Datei, die im XML-Format vorliegen muss, und enthält den Namespace der SAP-Aktion, die Sie verwenden möchten, z.B.: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Nachdem Sie die HTTP-Anforderung gesendet haben, warten Sie auf die Antwort der Logik-App.

   > [!NOTE]
   > Es kann ein Timeout auftreten, wenn nicht alle für die Antwort erforderlichen Schritte innerhalb des [Timeoutlimits der Anforderung](./logic-apps-limits-and-config.md) abgeschlossen wurden. Wenn dies passiert, können Anforderungen blockiert werden. Um die Problemdiagnose zu vereinfachen, sollten Sie sich mit dem [Überprüfen und Überwachen Ihrer Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md) vertraut machen.

Herzlichen Glückwunsch! Sie haben eine Logik-App erstellt, die mit dem SAP-Server kommunizieren kann. Da Sie eine SAP-Verbindung für die Logik-App eingerichtet haben, können Sie nun andere SAP Aktionen kennenlernen, z.B. BAPI und RFC.

## <a name="receive-from-sap"></a>Empfangen von SAP

Dieses Beispiel verwendet eine Logik-App, die bei Empfang einer Nachricht von einem SAP-System ausgelöst wird. 

### <a name="add-sap-trigger"></a>Hinzufügen eines SAP-Triggers

1. Erstellen Sie im Azure-Portal eine leere Logik-App, die den Logik-App-Designer öffnet. 

2. Geben Sie im Suchfeld den Begriff „sap“ als Filter ein. Wählen Sie in der Triggerliste den folgenden Trigger aus: **Beim Empfang einer Nachricht von SAP**

   ![Hinzufügen eines SAP-Triggers](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Alternativ dazu wechseln Sie zur Registerkarte „Unternehmen“, und wählen Sie folgenden Trigger aus:

   ![SAP-Trigger von Registerkarte „Unternehmen“ hinzufügen](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

3. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, erstellen Sie jetzt Ihre SAP-Verbindung. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können. 

   **Erstellen einer lokalen SAP-Verbindung**

   1. Stellen Sie die Verbindungsinformationen für Ihren SAP-Server bereit. 
   Wählen Sie für die Eigenschaft **Datengateway** das Datengateway aus, das Sie im Azure-Portal für Ihre Gatewayinstallation erstellt haben.

      Wenn die Eigenschaft **Anmeldetyp** auf **Anwendungsserver** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

      ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Wenn die Eigenschaft **Anmeldetyp** auf **Gruppe** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

      ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

4. Geben Sie die erforderlichen Parameter basierend auf der Konfiguration Ihres SAP-Systems an. 

   Optional können Sie eine oder mehrere SAP-Aktionen angeben. 
   Diese Liste von Aktionen gibt die Nachrichten an, die der Trigger über das Datengateway von Ihrem SAP-Server empfängt. 
   Eine leere Liste gibt an, dass der Trigger alle Nachrichten empfängt. 
   Wenn die Liste mehrere Nachrichten enthält, empfängt der Trigger nur die Nachrichten, die in der Liste angegeben wurden. Alle anderen von Ihrem SAP-Server gesendeten Nachrichten werden vom Gateway abgelehnt.

   Sie können eine SAP-Aktion aus der Dateiauswahl auswählen:

   ![SAP-Aktion auswählen](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Sie können eine Aktion auch manuell angeben:

   ![SAP-Aktion manuell eingeben](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Dieses Beispiel stellt dar, wie die Aktion angezeigt wird, wenn Sie den Trigger für den Empfang mehrerer Nachrichten einrichten.

   ![Triggerbeispiel](media/logic-apps-using-sap-connector/example-trigger.png)  

   Weitere Informationen zur SAP-Aktion finden Sie unter [Nachrichtenschemas für IDOC-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

5. Speichern Sie jetzt Ihre Logik-App, um den Empfang von Nachrichten von Ihrem SAP-System zu starten.
Wählen Sie auf der Symbolleiste des Designers **Speichern**. 

Ihre Logik-App kann jetzt Nachrichten von Ihrem SAP-System empfangen. 

> [!NOTE]
> Der SAP-Trigger ist kein Abfragetrigger. Vielmehr basiert er auf Webhooks. Der Trigger wird nur dann vom Gateway aufgerufen, wenn eine Nachricht vorhanden ist. Abfragen sind daher nicht erforderlich. 

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Um Ihre Logik-App auszulösen, senden Sie eine Nachricht von Ihrem SAP-System.

2. Wählen Sie in Ihrem Logik-App-Menü die Option **Übersicht**, und überprüfen Sie den **Ausführungsverlauf** auf neue Ausführungen für Ihre Logik-App. 

3. Öffnen Sie die letzte Ausführung, in der die von Ihrem SAP-System gesendete Nachricht im Abschnitt mit den Triggerausgaben angezeigt wird.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Generieren von Schemas für Artefakte in SAP

In diesem Beispiel wird eine Logik-App verwendet, die Sie mit einer HTTP-Anforderung auslösen können. Die SAP-Aktion sendet eine Anforderung an ein SAP-System, um die Schemas für die IDoc- (Intermediate Document) und BAPI-Angabe zu generieren. Schemas, die in der Antwort zurückgegeben werden, werden über den Azure Resource Manager-Connector in ein Integrationskonto hochgeladen.

### <a name="add-http-request-trigger"></a>Hinzufügen eines HTTP-Anforderungstriggers

1. Erstellen Sie im Azure-Portal eine leere Logik-App, die den Logik-App-Designer öffnet. 

2. Geben Sie im Suchfeld den Begriff „http request“ als Filter ein. Wählen Sie in der Triggerliste den folgenden Trigger aus: **Anforderung - Beim Empfang einer HTTP-Anforderung**

   ![Hinzufügen eines HTTP-Anforderungstriggers](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Speichern Sie jetzt Ihre Logik-App, damit Sie eine Endpunkt-URL dafür erstellen können.
Wählen Sie auf der Symbolleiste des Designers **Speichern**. 

   Die Endpunkt-URL wird nun im Trigger angezeigt, z.B.:

   ![URL für den Endpunkt generieren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>Hinzufügen einer SAP-Aktion zum Generieren von Schemas

1. Klicken Sie im Logik-App-Designer unter dem Trigger auf **+ Neuer Schritt** > **Aktion hinzufügen**.

   ![Hinzufügen einer Aktion](./media/logic-apps-using-sap-connector/add-action.png) 

2. Geben Sie im Suchfeld den Begriff „sap“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Schemas generieren**
  
   ![SAP-Sendeaktion auswählen](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Alternativ dazu wählen Sie die Registerkarte **Unternehmen** und dann die SAP-Aktion aus.

   ![SAP-Sendeaktion auf der Registerkarte „Unternehmen“ auswählen](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

3. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, erstellen Sie jetzt Ihre SAP-Verbindung. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können. 

   **Erstellen einer lokalen SAP-Verbindung**

   1. Stellen Sie die Verbindungsinformationen für Ihren SAP-Server bereit. 
   Wählen Sie für die Eigenschaft **Datengateway** das Datengateway aus, das Sie im Azure-Portal für Ihre Gatewayinstallation erstellt haben.

      Wenn die Eigenschaft **Anmeldetyp** auf **Anwendungsserver** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

      ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Wenn die Eigenschaft **Anmeldetyp** auf **Gruppe** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:
   
      ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Wenn Sie fertig sind, wählen Sie **Erstellen** aus. Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

4. Geben Sie den Pfad zu dem Artefakt an, für das Sie das Schema generieren möchten.

   Sie können die SAP-Aktion über die Dateiauswahl auswählen:

   ![SAP-Aktion auswählen](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Alternativ dazu können Sie die Aktion manuell eingeben:

   ![SAP-Aktion manuell eingeben](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png) 

   Um Schemas für mehrere Artefakte zu generieren, geben Sie die Details der SAP-Aktion für jedes Artefakt an. Beispiel:

   ![„Neues Element hinzufügen“ auswählen](media/logic-apps-using-sap-connector/schema-generator-array-pick.png) 

   ![Zwei Elemente anzeigen](media/logic-apps-using-sap-connector/schema-generator-example.png) 

   Weitere Informationen zur SAP-Aktion finden Sie unter [Nachrichtenschemas für IDOC-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie in der Designer-Symbolleiste die Option **Ausführen**, um eine Ausführung für Ihre Logik-App auszulösen.

2. Öffnen Sie die Ausführung, und überprüfen Sie die Ausgaben für die Aktion **Schema generieren**. 

   Die Ausgaben zeigen die generierten Schemas für die angegebene Liste von Nachrichten.

### <a name="upload-schemas-to-integration-account"></a>Schemas in Integrationskonto hochladen

Optional können Sie die generierten Schemas in Repositorys wie ein Blob-, Speicher- oder Integrationskonto herunterladen oder speichern. Integrationskonten eignen sich hervorragend für die Verwendung mit anderen XML-Aktionen. Daher zeigt dieses Beispiel, wie Sie Schemas mithilfe des Azure Resource Manager-Connectors in ein Integrationskonto für die gleiche Logik-App hochladen.

1. Klicken Sie im Logik-App-Designer unter dem Trigger auf **Neuer Schritt** > **Aktion hinzufügen**. Geben Sie im Suchfeld den Begriff „resource manager“ als Filter ein. Wählen Sie diese Aktion aus: **Ressource erstellen oder aktualisieren**

   ![Azure Resource Manager-Aktion auswählen](media/logic-apps-using-sap-connector/select-arm-action.png) 

2. Geben Sie die Details ein, einschließlich des Azure-Abonnements, der Azure-Ressourcengruppe und des Integrationskontos. Für die anderen Felder verwenden Sie das folgende Beispiel.

   ![Details für Azure Resource Manager-Aktion eingeben](media/logic-apps-using-sap-connector/arm-action.png)

   Die SAP-Aktion **Schemas generieren** generiert Schemas als Sammlung, daher fügt der Designer der Aktion automatisch eine **For each**-Schleife hinzu. 
   In diesem Beispiel wird die Darstellung dieser Aktion gezeigt:

   ![Azure Resource Manager-Aktion mit „for each“-Schleife](media/logic-apps-using-sap-connector/arm-action-foreach.png)  

   > [!NOTE]
   > Die Schemas verwenden das Base64-codierte Format. Damit die Schemas in ein Integrationskonto hochgeladen werden können, müssen sie mithilfe der Funktion `base64ToString()` decodiert werden. Das folgende Beispiel zeigt den Code für das Element `"properties"`:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

3. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie in der Designer-Symbolleiste die Option **Ausführen**, um Ihre Logik-App manuell auszulösen.

2. Wechseln Sie nach einer erfolgreichen Ausführung zum Integrationskonto, und überprüfen Sie, ob die generierten Schemas vorhanden sind.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Zurzeit sind für den SAP-Connector folgende Probleme und Einschränkungen bekannt:

* Der SAP-Trigger unterstützt nicht den Empfang von Batch-IDOCs von SAP. Diese Aktion kann zu einem RFC-Verbindungsfehler zwischen Ihrem SAP-System und dem Datengateway führen.

* Der SAP-Trigger unterstützt keine Datengatewaycluster. In einigen Failoverfällen unterscheidet sich der Datengatewayknoten, der mit dem SAP-System kommuniziert, von dem aktiven Knoten. Dies kann zu unerwartetem Verhalten führen. In Sendeszenarien werden Datengatewaycluster unterstützt.

* In Empfangsszenarien dürfen nur NULL-Antworten zurückgegeben werden. Eine Logik-App mit einem Trigger und einer Antwortaktion führt zu unerwartetem Verhalten. 

* Mit tRFC funktioniert nur ein einzelner Aufruf bzw. eine einzelne Nachricht über „An SAP senden“. Das BAPI-Commitmuster (Business Application Programming Interface), z.B. das Durchführen mehrerer tRFC-Aufrufe in derselben Sitzung, wird nicht unterstützt.

* RFCs mit Anlagen werden für die Aktionen „An SAP senden“ und „Schemas generieren“ nicht unterstützt.

* Der SAP-Connector unterstützt derzeit keine SAP-Router-Zeichenfolgen. Das lokale Datengateway muss in demselben LAN wie das SAP-System vorhanden sein, mit dem eine Verbindung hergestellt werden soll.

* Die Konvertierung für nicht vorhandene (NULL) oder leere Werte sowie Mindest- und Höchstwerte für DATS- und TIMS-SAP-Felder kann in späteren Updates für das lokale Datengateway jederzeit geändert werden.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung aus Logik-Apps zu lokalen Systemen](../logic-apps/logic-apps-gateway-connection.md)
* In diesem Artikel lernen Sie die Überprüfung, die Transformation und andere Nachrichtenvorgänge im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) kennen.
* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)

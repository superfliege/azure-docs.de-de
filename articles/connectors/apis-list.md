---
title: Connectors für Azure Logic Apps | Microsoft-Dokumentation
description: Automatisieren von Workflows mit integrierten, verwaltete API-, lokalen, Integrationskonto- und Enterprise-Connectors für Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 06/29/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2bb3e2ce29037372395aa0b30e9f76f3e712667d
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096610"
---
# <a name="connectors-for-azure-logic-apps"></a>Connectors für Azure Logic Apps

Connectors sind ein wesentlicher Bestandteil bei der Erstellung von automatisierten Workflows mit Azure Logic Apps. Durch Verwenden von Connectors in Ihren Logik-Apps erweitern Sie die Funktionen für Ihre lokalen und Cloud-Apps und können Aufgaben mit den Daten ausführen, die Sie erstellen und über die Sie bereits verfügen. Logic Apps bietet insgesamt ca. 200 Connectors. In diesem Artikel werden beliebte und häufiger verwendete Connectors beschrieben, die in Tausenden von Apps und Millionen von Ausführungen zur Verarbeitung von Daten und Informationen erfolgreich verwendet werden.
Connectors stehen als integrierte Aktionen oder als verwaltete Connectors zur Verfügung. 

* [**Integrierte Aktionen:**](#built-ins) Mit diesen integrierten Aktionen und Triggern können Sie Logik-Apps erstellen, die in benutzerdefinierten Zeitplänen ausgeführt werden, mit anderen Endpunkten kommunizieren, Anforderungen empfangen und auf diese antworten sowie Azure-Funktionen, Azure API-Apps (Web-Apps), Ihre eigenen mit Azure API Management verwalteten und veröffentlichten APIs und geschachtelte Logik-Apps aufrufen, die Anforderungen empfangen können. Sie können zudem integrierte Aktionen verwenden, mit denen Sie den Workflow Ihrer Logik-App organisieren und steuern können und die auch mit Daten funktionieren.

* **Verwaltete Connectors:** Diese Connectors umfassen Trigger und Aktionen für den Zugriff auf andere Dienste und Systeme. Bei einigen Connectors müssen Sie zunächst Verbindungen erstellen, die von Azure Logic Apps verwaltet werden. Verwaltete Connectors sind in folgende Gruppen unterteilt:

  |   |   |
  |---|---|
  | [**Verwaltete API-Connectors**](#managed-api-connectors) | Erstellen von Logik-Apps, die Dienste wie Azure Blob Storage, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online und viele mehr verwenden. | 
  | [**Lokale Connectors**](#on-premises-connectors) | Nach dem Installieren und Einrichten des [lokalen Datengateways][gateway-doc] können Ihre Logik-Apps mithilfe dieser Connectors auf lokale Systeme wie SQL Server, SharePoint Server, Oracle DB, Dateifreigaben und andere zugreifen. | 
  | [**Integrationskonto-Connectors**](#integration-account-connectors) | Diese Connectors sind verfügbar, wenn Sie ein Integrationskonto erstellen und bezahlen. Die Connectors transformieren und validieren XML, codieren und decodieren Flatfiles und verarbeiten Business-to-Business-Nachrichten (B2B) mit AS2-, EDIFACT- und X12-Protokollen. | 
  | [**Enterprise-Connectors**](#enterprise-connectors) | Bieten gegen zusätzliche Gebühren Zugriff auf Unternehmenssysteme wie SAP und IBM MQ. |
  ||| 

  Wenn Sie beispielsweise Microsoft BizTalk Server nutzen, können Ihre Logik-Apps eine Verbindung mit Ihrer BizTalk Server-Instanz herstellen und damit kommunizieren, indem der [BizTalk Server-Connector](#on-premises-connectors) verwendet wird. 
  Sie können dann mithilfe der [Integrationskonto-Connectors](#integration-account-connectors) BizTalk-ähnliche Vorgänge in Ihren Logik-Apps erweitern oder durchführen. 

Technische Informationen zu den in einer Swagger-Beschreibung definierten Triggern und Aktionen der einzelnen Connectors sowie mögliche Einschränkungen finden Sie unter [Connectors](/connectors/). Informationen zu den Kosten finden Sie unter [Logic Apps – Preise ](https://azure.microsoft.com/pricing/details/logic-apps/) und [Preismodell für Logik-Apps](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Integrierte Aktionen

Logic Apps umfasst integrierte Trigger und Aktionen, sodass Sie zeitplanbasierte Workflows erstellen, Ihre Logik-Apps mit anderen Apps und Diensten kommunizieren lassen, den Workflow über die Logik-Apps steuern und Daten verwalten oder bearbeiten können. 

|   |   |   |   | 
|---|---|---|---| 
| [![API-Symbol][schedule-icon]<br/>**Zeitplan**][recurrence-doc] | - Ausführen Ihrer Logik-App nach einem festgelegten Zeitplan auf Grundlage einfacher bis komplexer Wiederholungen mit dem Trigger **Wiederholung**. <p>- Anhalten der Logik-App für einen bestimmten Zeitraum mit der Aktion **Verzögerung**. <p>- Anhalten der Logik-App bis zum festgelegten Zeitpunkt (Datum und Uhrzeit) mit der Aktion **Verzögern bis**. | [![API-Symbol][http-icon]<br/>**HTTP**][http-doc] | Kommunikation mit einem beliebigen Endpunkt über HTTP mit Triggern und Aktionen für HTTP, HTTP + Swagger und HTTP + Webhook. | 
| [![API-Symbol][http-request-icon]<br/>**Anforderung**][http-request-doc] | - Festlegen, dass die Logik-App über andere Apps oder Dienste aufrufbar ist und bei Event Grid-Ressourcenereignissen oder bei Antworten auf Azure Security Center-Warnungen mit dem Trigger **Anforderung** ausgelöst wird. <p>- Senden von Antworten an eine App oder einen Dienst mit der Aktion **Antwort**. | [![API-Symbol][batch-icon]<br/>**Batch**][batch-doc] | - Verarbeiten von Nachrichten in Batches mit dem Trigger **Batchnachrichten**. <p>- Aufrufen von Logik-Apps mit vorhandenen Batchtriggern mit der Aktion **Nachrichten an Batch senden**. | 
| [![API-Symbol][azure-functions-icon]<br/>**Azure Functions**][azure-functions-doc] | Aufrufen von Azure-Funktionen, die benutzerdefinierte Codeausschnitte (C# oder Node.js) aus Ihren Logik-Apps ausführen. | [![API-Symbol][azure-api-management-icon]</br>**Azure API Management**][azure-api-management-doc] | Aufrufen von Triggern und Aktionen, die in Ihren eigenen APIs definiert sind, die Sie mit Azure API Management verwalten und veröffentlichen. | 
| [![API-Symbol][azure-app-services-icon]<br/>**Azure App Services**][azure-app-services-doc] | Aufrufen von Azure API-Apps oder Web-Apps, die in Azure App Service gehostet werden. Die in diesen Apps definierten Trigger und Aktionen werden wie alle anderen erstklassigen Trigger und Aktionen angezeigt, wenn Swagger enthalten ist. | [![API-Symbol][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps**][nested-logic-app-doc] | Aufrufen anderer Logik-Apps, die mit dem Trigger „Anforderung“ gestartet werden. | 
||||| 

### <a name="control-workflow"></a>Steuern des Workflows

Im Folgenden sind integrierte Aktionen zum Strukturieren und Steuern der Aktionen im Workflow Ihrer Logik-App aufgeführt:

|   |   |   |   | 
|---|---|---|---| 
| [![Symbol für integrierte Aktion][condition-icon]<br/>**Bedingung**][condition-doc] | Auswerten einer Bedingung und Ausführen unterschiedlicher Aktionen abhängig davon, ob die Bedingung TRUE oder FALSE ist. | [![Symbol für integrierte Aktion][for-each-icon]</br>**For each**][for-each-doc] | Ausführen der gleichen Aktionen für jedes Element in einem Array. | 
| [![Symbol für integrierte Aktion][scope-icon]<br/>**Bereich**][scope-doc] | Gruppieren von Aktionen in *Bereiche*, die einen eigenen Status erhalten, nachdem die Ausführung der Aktionen im Bereich beendet ist. | [![Symbol für integrierte Aktion][switch-icon]</br>**Option**][switch-doc] | Gruppieren von Aktionen in *Fälle*, denen mit Ausnahme des Standardfalls eindeutige Werte zugewiesen werden. Es wird nur der Fall ausgeführt, dessen zugewiesener Wert mit dem Ergebnis eines Ausdrucks, Objekts oder Token übereinstimmt. Wenn keine Übereinstimmungen vorhanden sind, wird der Standardfall ausgeführt. | 
| [![Symbol für integrierte Aktion][terminate-icon]<br/>**Beenden**][terminate-doc] | Beenden des aktiv ausgeführten Workflows einer Logik-App. | [![Symbol für integrierte Aktion][until-icon]<br/>**Bis**][until-doc] | Wiederholen von Aktionen, bis die angegebene Bedingung TRUE ist oder sich ein Status geändert hat. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Verwalten oder Bearbeiten von Daten

Im Folgenden sind integrierte Aktionen zur Verwendung von Datenausgaben mit den zugehörigen Formaten aufgeführt:  

|   |   | 
|---|---| 
| ![Symbol für integrierte Aktion][data-operations-icon]<br/>**Datenvorgänge** | Ausführen von Vorgängen mit Daten: <p>- **Erstellen:** Erstellen einer einzelnen Ausgabe aus mehreren Eingaben mit verschiedenen Typen. <br>- **CSV-Tabelle erstellen:** Erstellen einer durch Trennzeichen getrennten Tabelle (CSV-Tabelle) aus einem Array mit JSON-Objekten. <br>- **HTML-Tabelle erstellen:** Erstellen einer HTML-Tabelle aus einem Array mit JSON-Objekten. <br>- **Array filtern:** Erstellen eines Arrays aus Elementen in einem anderen Array, die Ihren Kriterien entsprechen. <br>- **Verknüpfen:** Erstellen einer Zeichenfolge aus allen Elementen in einem Array und Trennen dieser Elemente mit dem angegebenen Trennzeichen. <br>- **JSON analysieren:** Erstellen von benutzerfreundlichen Token aus Eigenschaften und den zugehörigen Werten in JSON-Inhalten, sodass Sie diese Eigenschaften in Ihrem Workflow verwenden können. <br>- **Auswählen:** Erstellen eines Arrays mit JSON-Objekten durch Transformieren von Elementen oder Werten in einem anderen Array und Zuordnen dieser Elemente zu angegebenen Eigenschaften. | 
| ![Symbol für integrierte Aktion][date-time-icon]<br/>**Datum und Uhrzeit** | Ausführen von Vorgängen mit Zeitstempeln: <p>- **Zu Zeit hinzufügen:** Hinzufügen der angegebenen Anzahl von Einheiten zu einem Zeitstempel. <br>- **Zeitzone konvertieren:** Konvertieren eines Zeitstempels von der Quellzeitzone in die Zielzeitzone. <br>- **Aktuelle Uhrzeit:** Zurückgeben des aktuellen Zeitstempels als Zeichenfolge. <br>- **Zukünftige Zeit abrufen:** Zurückgeben des aktuellen Zeitstempels plus der angegebenen Zeiteinheiten. <br>- **Vergangene Zeit abrufen:** Zurückgeben des aktuellen Zeitstempels abzüglich der angegebenen Zeiteinheiten. <br>- **Von Uhrzeit abziehen:** Subtrahieren einer Anzahl von Zeiteinheiten von einem Zeitstempel. |
| [![Symbol für integrierte Aktion][variables-icon]<br/>**Variablen**][variables-doc] | Ausführen von Vorgängen mit Variablen: <p>- **An Arrayvariable anfügen:** Einfügen eines Werts als letztes Element in einem Array, das in einer Variable gespeichert ist. <br>- **An Zeichenfolgenvariable anfügen:** Einfügen eines Werts als letztes Zeichen in einer Zeichenfolge, die in einer Variable gespeichert ist. <br>- **Variablenwert verringern:** Verringern einer Variable um einen konstanten Wert. <br>- **Variable schrittweise erhöhen:** Erhöhen einer Variable um einen konstanten Wert. <br>- **Variable initialisieren:** Erstellen einer Variable und Deklarieren ihres Datentyps und Anfangswert. <br>- **Variable festlegen:** Zuweisen eines anderen Werts zu einer vorhandenen Variable. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Verwaltete API-Connectors

Im Folgenden sind beliebte Connectors zur Automatisierung von Aufgaben, Prozessen und Workflows mit folgenden Diensten oder Systemen aufgeführt:

|   |   |   |   | 
|---|---|---|---| 
| [![API-Symbol][azure-service-bus-icon]<br/>**Azure Service Bus**][azure-service-bus-doc] | Verwalten von asynchronen Nachrichten, Sitzungen und Themenabonnements mit dem am häufigsten verwendeten Connector in Logic Apps. | [![API-Symbol][sql-server-icon]<br/>**SQL Server**][sql-server-doc] | Herstellen einer Verbindung mit Ihrer lokalen SQL Server-Instanz oder einer Azure SQL-Datenbank in der Cloud, sodass Sie Datensätze verwalten, gespeicherte Prozeduren ausführen oder Abfragen ausführen können. | 
| [![API-Symbol][office-365-outlook-icon]<br/>**Office 365<br/>Outlook**][office-365-outlook-doc] | Herstellen einer Verbindung mit Ihrem Office 365-E-Mail-Konto, sodass Sie E-Mails, Aufgaben, Kalenderereignisse und Besprechungen, Kontakte, Anforderungen und mehr erstellen und verwalten können. | [![API-Symbol][azure-blob-storage-icon]<br/>**Azure Blob<br/>Storage**][azure-blob-storage-doc] | Herstellen einer Verbindung mit Ihrem Storage-Konto, sodass Sie Blobinhalte erstellen und verwalten können. | 
| [![API-Symbol][sftp-icon]<br/>**SFTP**][sftp-doc] | Herstellen einer Verbindung mit SFTP-Servern, auf die Sie über das Internet zugreifen können, sodass Sie Ihre Dateien und Ordner verwenden können. | [![API-Symbol][sharepoint-online-icon]<br/>**SharePoint<br/>Online**][sharepoint-online-doc] | Herstellen einer Verbindung mit SharePoint Online, sodass Sie Dateien, Anlagen, Ordner und mehr verwalten können. | 
| [![API-Symbol][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Herstellen einer Verbindung mit Ihrem Dynamics 365-Konto, sodass Sie Datensätze, Elemente und mehr erstellen und verwalten können. | [![API-Symbol][ftp-icon]<br/>**FTP**][ftp-doc] | Herstellen einer Verbindung mit FTP-Servern, auf die Sie über das Internet zugreifen können, sodass Sie Ihre Dateien und Ordner verwenden können. | 
| [![API-Symbol][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Herstellen einer Verbindung mit Ihrem Salesforce-Konto, sodass Sie Elemente wie Datensätze, Aufträge, Objekte usw. erstellen und verwalten können. | [![API-Symbol][twitter-icon]<br/>**Twitter**][twitter-doc] | Herstellen einer Verbindung mit Ihrem Twitter-Konto, sodass Sie Tweets, Follower, die Zeitachse usw. verwalten können. Ihre Tweets können Sie in SQL, Excel oder SharePoint speichern. | 
| [![API-Symbol][azure-event-hubs-icon]<br/>**Azure Event Hubs**][azure-event-hubs-doc] | Nutzen und Veröffentlichen von Ereignissen über einen Event Hub. Sie können beispielsweise mithilfe von Event Hubs eine Ausgabe Ihrer Logik-App abrufen und die Ausgabe anschließend an einen Echtzeitanalyseanbieter senden. | [![API-Symbol][azure-event-grid-icon]<br/>**Azure Event**</br>**Grid**][azure-event-grid-doc] | Überwachen von Ereignissen, die von Event Grid veröffentlicht werden, z.B. bei Änderungen von Azure-Ressourcen oder Drittanbieterressourcen. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Lokale Connectors 

Im Folgenden sind einige häufig verwendete Connectors aufgeführt, die Zugriff auf Daten und Ressourcen in lokalen Systemen ermöglichen. Damit Sie eine Verbindung mit einem lokalen System herstellen können, müssen Sie zunächst [ein lokales Datengateway herunterladen, installieren und einrichten][gateway-doc]. Dieses Gateway stellt einen sicheren Kommunikationskanal dar, ohne dass die erforderliche Netzwerkinfrastruktur eingerichtet werden muss. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![API-Symbol][biztalk-server-icon]<br/>**BizTalk**</br> **Server** | [![API-Symbol][file-system-icon]<br/>**Datei-</br> system**][file-system-doc] | [![API-Symbol][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![API-Symbol][ibm-informix-icon]<br/>**IBM**</br> **Informix**][ibm-informix-doc] | ![API-Symbol][mysql-icon]<br/>**MySQL** | 
| [![API-Symbol][oracle-db-icon]<br/>**Oracle-Datenbank**][oracle-db-doc] | ![API-Symbol][postgre-sql-icon]<br/>**PostgreSQL** | [![API-Symbol][sharepoint-server-icon]<br/>**SharePoint</br> Server**][sharepoint-server-doc] | [![API-Symbol][sql-server-icon]<br/>**SQL</br> Server**][sql-server-doc] | ![API-Symbol][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Integrationskonto-Connectors 

Nachfolgend sind Connectors zur Erstellung von Business-to-Business-Lösungen (B2B) mit Ihren Logik-Apps aufgeführt, wenn Sie ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) erstellen und bezahlen. Dieses ist über das Enterprise Integration Pack (EIP) in Azure verfügbar. Mit diesem Konto können Sie B2B-Artefakte wie Handelspartner, Vereinbarungen, Zuordnungen, Schemas, Zertifikate usw. erstellen und speichern. Zur Verwendung dieser Artefakte ordnen Sie Ihre Logik-Apps dem Integrationskonto zu. Wenn Sie derzeit BizTalk Server verwenden, sind Ihnen diese Connectors möglicherweise bereits bekannt.

|   |   |   |   | 
|---|---|---|---| 
| [![API-Symbol][as2-icon]<br/>**AS2-</br>Decodierung**][as2-decode-doc] | [![API-Symbol][as2-icon]<br/>**AS2-</br>Codierung**][as2-encode-doc] | [![API-Symbol][edifact-icon]<br/>**EDIFACT-</br>Decodierung**][edifact-decode-doc] | [![API-Symbol][edifact-icon]<br/>**EDIFACT-</br>Codierung**][edifact-encode-doc] | 
| [![API-Symbol][flat-file-decode-icon]<br/>**Flatfile-</br>Decodierung**][flat-file-decode-doc] | [![API-Symbol][flat-file-encode-icon]<br/>**Flatfile-</br>Codierung**][flat-file-encode-doc] | [![API-Symbol][integration-account-icon]<br/>**Integrations-<br/>konto**][integration-account-doc] | [![API-Symbol][liquid-icon]<br/>**Liquid-**</br>**Transformation**][json-liquid-transform-doc] | 
| [![API-Symbol][x12-icon]<br/>**X12-</br>Decodierung**][x12-decode-doc] | [![API-Symbol][x12-icon]<br/>**X12-</br>Codierung**][x12-encode-doc] | [![API-Symbol][xml-transform-icon]<br/>**XML-**</br>**Transformation**][xml-transform-doc] | [![API-Symbol][xml-validate-icon]<br/>**XML-<br/>Überprüfung**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Enterprise-Connectors

Ihre Logik-Apps können auf Unternehmenssysteme wie SAP und IBM MQ zugreifen:

|   |   | 
|---|---| 
| [![API-Symbol][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![API-Symbol][sap-icon]<br/>**SAP**][sap-connector-doc] |
||| 

## <a name="more-about-triggers-and-actions"></a>Weitere Informationen zu Triggern und Aktionen

Einige Connectors umfassen *Trigger*, die Ihre Logik-App benachrichtigen, wenn bestimmte Ereignisse eintreten. Wenn diese Ereignisse eintreten, erstellt der Trigger eine Instanz der Logik-App und führt sie aus. Der FTP-Connector verfügt beispielsweise über den Trigger „Wenn eine Datei hinzugefügt oder geändert wird“, der Ihre Logik-App startet, wenn eine Datei aktualisiert wird. 

In Logic Apps stehen folgende Arten von Triggern zur Verfügung:  

* *Abfragetrigger:* Diese Trigger fragen den Dienst in bestimmten Intervallen auf neue Daten ab. 

  Wenn neue Daten verfügbar sind, wird eine neue Instanz Ihrer Logik-App erstellt und mit den als Eingabe übergebenen Daten ausgeführt. 

* *Pushtrigger:* Diese Trigger lauschen auf den Eingang neuer Daten an einem Endpunkt oder auf das Eintreten eines Ereignisses, durch die eine neue Instanz Ihrer Logik-App erstellt und ausgeführt wird.

* *Wiederholungstrigger:* Dieser Trigger erstellt eine Instanz Ihrer Logik-App und führt sie nach einem festgelegten Zeitplan aus.

Connectors umfassen auch *Aktionen*, die Aufgaben im Workflow Ihrer Logik-App ausführen. So kann Ihre Logik-App etwa Daten lesen und diese in späteren Schritten der Logik-App verwenden. Genauer gesagt kann die Logik-App Kundendaten aus einer SQL-­Datenbank suchen und später im Workflow der Logik-App verarbeiten. 

Weitere Informationen zu Triggern und Aktionen finden Sie in der [Übersicht über Connectors](connectors-overview.md). 

## <a name="custom-apis-and-connectors"></a>Benutzerdefinierte APIs und Connectors 

Zum Aufrufen von APIs, die benutzerdefinierten Code ausführen oder nicht als Connector verfügbar sind, können Sie die Logic Apps-Plattform erweitern, indem Sie [benutzerdefinierte API-Apps erstellen](../logic-apps/logic-apps-create-api-app.md). Sie können außerdem für *alle* REST- oder SOAP-basierten APIs [benutzerdefinierte Connectors erstellen](../logic-apps/custom-connector-overview.md), mit denen diese APIs in allen Logik-Apps in Ihrem Azure-Abonnement verfügbar werden.
Um benutzerdefinierte API-Apps oder Connectors für alle Benutzer in Azure öffentlich zu machen, können Sie [Connectors für die Microsoft-Zertifizierung einreichen](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Auf der [Benutzerfeedbackwebsite für Logic Apps](http://aka.ms/logicapps-wish) können Sie über Ideen abstimmen oder selbst Ideen für Azure Logic Apps einreichen.

* Fehlen in der Dokumentation noch Artikel oder Details, die Ihrer Meinung nach wichtig sind? Falls ja, dann helfen Sie uns, indem Sie die vorhandenen Artikel ergänzen oder eigene erstellen. Die Dokumentation basiert auf Open Source und ist auf GitHub gehostet. Beginnen Sie mit dem [GitHub-Repository](https://github.com/Microsoft/azure-docs) der Azure-Dokumentation. 

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Erstellen benutzerdefinierter Connectors für Logik-Apps](https://docs.microsoft.com/connectors/custom-connectors/)
* [Erstellen einer benutzerdefinierten API zur Verwendung mit Logik-Apps](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Herstellen einer Verbindung mit lokalen Datenquellen über Logik-Apps mit dem lokalen Datengateway"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Integration von Logik-Apps in Azure Functions"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Erstellen einer Azure API Management-Dienstinstanz zum Verwalten und Veröffentlichen von APIs"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integration von Logik-Apps in App Service-API-Apps"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Senden von Nachrichten aus Service Bus-Warteschlangen und -Themen und Empfangen von Nachrichten aus Service Bus-Warteschlangen und -Abonnements"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Verarbeiten von Nachrichten in Gruppen oder als Batches"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Auswerten einer Bedingung und Ausführen unterschiedlicher Aktionen abhängig davon, ob die Bedingung TRUE oder FALSE ist"
[delay-doc]: ./connectors-native-delay.md "Durchführen von verzögerten Aktionen"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Ausführen der gleichen Aktionen für jedes Element in einem Array"
[http-doc]: ./connectors-native-http.md "Durchführen von HTTP-Aufrufen mit dem HTTP-Connector"
[http-request-doc]: ./connectors-native-reqres.md "Hinzufügen von Aktionen für HTTP-Anforderungen und -Antworten zu Ihren Logik-Apps"
[http-response-doc]: ./connectors-native-reqres.md "Hinzufügen von Aktionen für HTTP-Anforderungen und -Antworten zu Ihren Logik-Apps"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Durchführen von HTTP-Aufrufen mit HTTP + Swagger-Connector"
[http-webook-doc]: ./connectors-native-webhook.md "Hinzufügen von HTTP-Webhook-Aktionen und -Triggern zu Ihren Logik-Apps"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Integrieren von Logik-Apps in geschachtelte Workflows"
[query-doc]: ./connectors-native-query.md "Auswählen und Filtern von Arrays mit der Abfrageaktion"
[recurrence-doc]:  ./connectors-native-recurrence.md "Auslösen von sich wiederholenden Aktionen für Logik-Apps"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Organisieren von Aktionen in Gruppen, die einen eigenen Status erhalten, nachdem die Ausführung der Aktionen in der Gruppe beendet ist" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organisieren von Aktionen in Fälle, denen eindeutige Werte zugewiesen werden. Es wird nur der Fall ausgeführt, dessen Wert mit dem Ergebnis eines Ausdrucks, Objekts oder Token übereinstimmt. Wenn keine Übereinstimmungen vorhanden sind, wird der Standardfall ausgeführt."
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Beenden oder Abbrechen eines aktiv ausgeführten Workflows für Ihre Logik-App"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Wiederholen von Aktionen, bis die angegebene Bedingung TRUE ist oder sich ein Status geändert hat"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Ausführen von Vorgängen mit Variablen, z.B. Initialisieren, Festlegen, Erhöhen oder Verringern in einzelnen Schritten sowie Anfügen an Arrayvariable oder Zeichenfolgenvariable"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Verwalten von Dateien in Ihrem Blobcontainer mit Azure Blob Storage-Connector"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Überwachen von Ereignissen, die von Event Grid veröffentlicht werden, z.B. bei Änderungen von Azure-Ressourcen oder Drittanbieterressourcen"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Herstellen einer Verbindung mit Azure Event Hubs. Empfangen und Senden von Ereignissen zwischen Logik-Apps und Event Hubs."
[box-doc]: ./connectors-create-api-box.md "Stellen Sie eine Verbindung mit Box her. Sie können Dateien hochladen, abrufen, löschen, auflisten und vieles mehr."
[dropbox-doc]: ./connectors-create-api-dropbox.md "Stellen Sie eine Verbindung mit Dropbox her. Sie können Dateien hochladen, abrufen, löschen, auflisten und vieles mehr."
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Herstellen einer Verbindung mit Dynamics CRM Online zur Verwendung von CRM Online-Daten"
[facebook-doc]: ./connectors-create-api-facebook.md "Stellen Sie eine Verbindung mit Facebook her. Sie können eine Timeline posten, einen Seitenfeed abrufen und vieles mehr."
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Herstellen einer Verbindung mit einem lokalen Dateisystem"
[ftp-doc]: ./connectors-create-api-ftp.md "Herstellen einer Verbindung mit einem FTP/FTPS-Server für FTP-Aufgaben, z.B. Hochladen, Abrufen, Löschen von Dateien usw."
[github-doc]: ./connectors-create-api-github.md "Herstellen einer Verbindung mit GitHub und Nachverfolgen von Problemen"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Verbindung mit Google Calendar zum Verwalten von Kalendern."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Herstellen einer Verbindung mit GoogleDrive zur Verwendung mit Ihren Daten"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Herstellen einer Verbindung mit Google Sheets zur Bearbeitung von Tabellen"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Herstellen einer Verbindung mit Google Tasks zur Verwaltung Ihrer Aufgaben"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Stellen Sie eine Verbindung mit IBM DB2 in der Cloud oder lokal her. Sie können eine Zeile aktualisieren, eine Tabelle abrufen und vieles mehr."
[ibm-informix-doc]: ./connectors-create-api-informix.md "Stellen Sie eine Verbindung mit Informix in der Cloud oder lokal her. Sie können eine Zeile lesen, die Tabellen auflisten und vieles mehr."
[ibm-mq-doc]: ./connectors-create-api-mq.md "Herstellen einer Verbindung mit IBM MQ (lokal oder in Azure) und Senden und Empfangen von Nachrichten"
[instagram-doc]: ./connectors-create-api-instagram.md "Stellen Sie eine Verbindung mit Instagram her. Lösen Sie Ereignisse aus, oder reagieren Sie darauf."
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Stellen Sie eine Verbindung mit Ihrem MailChimp-Konto her. Verwalten und automatisieren Sie E-Mails."
[mandrill-doc]: ./connectors-create-api-mandrill.md "Herstellen einer Verbindung mit Mandrill zur Kommunikation"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Stellen Sie eine Verbindung mit Microsoft Translator her. Sie können Text übersetzen, Sprachen erkennen und vieles mehr." 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Stellen Sie eine Verbindung mit Ihrem Office 365-Konto her. Sie können E-Mails senden und empfangen, Ihren Kalender und Kontakte verwalten und vieles mehr."
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Abrufen von Videoinformationen, Videolisten und Kanälen und Wiedergabe-URLs für Office 365-Videos"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Stellen Sie eine Verbindung mit Ihrer persönlichen Microsoft OneDrive-Instanz her. Sie können Dateien hochladen, löschen, auflisten und vieles mehr."
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Stellen Sie eine Verbindung mit Ihrer geschäftlichen Microsoft OneDrive-Instanz her. Sie können Dateien hochladen, löschen, auflisten und vieles mehr."
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Herstellen einer Verbindung mit einer Oracle-Datenbank zum Hinzufügen, Einfügen und Löschen von Zeilen sowie für vieles mehr."
[outlook.com-doc]: ./connectors-create-api-outlook.md "Stellen Sie eine Verbindung mit Ihrem Outlook-Postfach her. Sie können E-Mails, Kalender, Kontakte usw. verwalten."
[project-online-doc]: ./connectors-create-api-projectonline.md "Stellen Sie eine Verbindung mit Microsoft Project Online her. Sie können Projekte, Aufgaben, Ressourcen usw. verwalten."
[rss-doc]: ./connectors-create-api-rss.md "Sie können Feedelemente veröffentlichen und abrufen und Vorgänge auslösen, wenn ein neues Element in einem RSS-Feed veröffentlicht wird."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Stellen Sie eine Verbindung mit Ihrem Salesforce-Konto her. Sie können Konten, Leads, Vertriebschancen usw. verwalten."
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Herstellen einer Verbindung mit einem lokalen SAP-System"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Stellen Sie eine Verbindung mit SendGrid her. Sie können E-Mails senden und die Liste der Empfänger verwalten."
[sftp-doc]: ./connectors-create-api-sftp.md "Stellen Sie eine Verbindung mit Ihrem SFTP-Konto her. Sie können Dateien hochladen, abrufen, löschen und vieles mehr."
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Stellen Sie eine Verbindung mit dem lokalen SharePoint-Server her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Stellen Sie eine Verbindung mit SharePoint Online her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[slack-doc]: ./connectors-create-api-slack.md "Herstellen einer Verbindung mit Slack zum Veröffentlichen von Nachrichten in Slack-Kanälen"
[smtp-doc]: ./connectors-create-api-smtp.md "Herstellen einer Verbindung mit einem SMTP-Server und Senden von E-Mails mit Anhängen"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Herstellen einer Verbindung mit SparkPost zur Kommunikation"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Stellen Sie eine Verbindung mit Azure SQL-Datenbank oder SQL Server her. Sie können Einträge in einer SQL-Datenbanktabelle erstellen, aktualisieren, abrufen und löschen."
[trello-doc]: ./connectors-create-api-trello.md "Stellen Sie eine Verbindung mit Trello her. Sie können Ihre Projekte verwalten und viele verschiedene Dinge organisieren."
[twilio-doc]: ./connectors-create-api-twilio.md "Stellen Sie eine Verbindung mit Twilio her. Sie können Nachrichten senden und empfangen, verfügbare Nummern abrufen, eingehende Telefonnummern verwalten und vieles mehr."
[twitter-doc]: ./connectors-create-api-twitter.md "Stellen Sie eine Verbindung mit Twitter her. Sie können Timelines abrufen, Tweets posten und vieles mehr."
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Stellen Sie eine Verbindung mit Wunderlist her. Sie können Ihre Aufgaben und Aufgabenlisten verwalten, Ihre gesamten Aufgaben synchronisieren und vieles mehr."
[yammer-doc]: ./connectors-create-api-yammer.md "Stellen Sie eine Verbindung mit Yammer her. Sie können Nachrichten posten, neue Nachrichten abrufen und vieles mehr."
[youtube-doc]: ./connectors-create-api-youtube.md "Stellen Sie eine Verbindung mit YouTube her. Sie können Ihre Videos und Kanäle verwalten."

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Informationen zu Enterprise Integration (AS2)."
[as2-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Informationen zu Enterprise Integration (AS2-Decodierung)."
[as2-encode-doc]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Informationen zu Enterprise Integration (AS2-Codierung)."
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Informationen zu Enterprise Integration (EDIFACT-Decodierung)."
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Informationen zu Enterprise Integration (EDIFACT-Codierung)."
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Informationen zu Enterprise Integration (Flatfile)."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Informationen zu Enterprise Integration (Flatfile)."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Suchen nach Schemas, Zuordnungen, Partnern und mehr in Ihrem Integrationskonto"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Informationen zu JSON-Transformationen mit Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Informationen zu Enterprise Integration (X12)."
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Informationen zu Enterprise Integration (X12-Decodierung)."
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Informationen zu Enterprise Integration (X12-Codierung)."
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Informationen zu Enterprise Integration (Transformationen)."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Informationen zu Enterprise Integration (XML-Überprüfung)."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png
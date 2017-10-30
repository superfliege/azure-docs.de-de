---
title: "Connectors für Azure Logic Apps | Microsoft-Dokumentation"
description: Bei der Erstellung von Logik-Apps stehen zahlreiche Microsoft-Connectors zur Auswahl.
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: 36d3474c2b8dde0355a265c149fe6ba1920a9f22
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="connectors-list"></a>Liste mit den Connectors
Die in der Swagger-Beschreibung der einzelnen Connectors definierten Trigger und Aktionen sowie mögliche Connectoreinschränkungen finden Sie unter [Connectors](/connectors/).

Connectors sind ein wesentlicher Bestandteil der Logik-App-Erstellung. Mithilfe dieser Connectors können Sie Ihre lokalen und cloudbasierten Anwendungen erweitern und neu erstellte oder bereits vorhandene Daten für verschiedenste Dinge nutzen. Die Connectors stehen entweder als integrierte Aktionen oder als verwaltete Connectors zur Verfügung.

**Integrierte Aktionen:** Das Logic Apps-Modul bietet integrierte Aktionen für die Kommunikation mit Endpunkten und die Ausführung von Aufgaben. Mit diesen Aktionen können Sie beispielsweise HTTP-Endpunkte, Azure Functions und Azure API Management-Vorgänge aufrufen sowie Nachrichten mit Datenvorgängen und Variablen anpassen.

**Verwaltete Connectors:** Diese ermöglichen den Zugriff auf APIs für verschiedene Dienste durch Erstellung von API-Verbindungen, die vom Logic Apps-Dienst gehostet und verwaltet werden. Bei verwalteten Connectors wird zwischen folgenden Kategorien unterschieden:

* **Standardconnectors:** Automatisch verfügbar und enthalten, wenn Sie Logik-Apps verwenden. Beispiele wären etwa Service Bus, Power BI und OneDrive.

* **Lokale Connectors:** Stellen über das [lokale Datengateway][gatewaydoc] eine Verbindung mit lokalen Serveranwendungen her. Lokale Connectors umfassen unter anderem Konnektivität mit Serveranwendungen wie SharePoint Server, SQL Server, Oracle-Datenbank und Dateifreigaben.

* **Integrationskonto-Connectors**: Diese Connectors sind verfügbar, wenn Sie ein Integrationskonto erwerben. Mithilfe dieser Connectors können Sie XML transformieren und überprüfen, B2B-Nachrichten mit AS2/X12/EDIFACT verarbeiten sowie Flatfiles codieren und decodieren. Bei Verwendung von BizTalk Server können Sie Ihre BizTalk-Workflows mit diesen Connectors sehr gut in Azure erweitern.  

    BizTalk Server verfügt auch über einen [Logic Apps-Adapter](https://msdn.microsoft.com/library/mt787163.aspx), der unter anderem das Empfangen von Daten aus einer Logik-App sowie das Senden von Daten an eine Logik-App ermöglicht.

* **Enterprise-Connectors:** Unter anderem für MQ und SAP. Hierfür fallen zusätzliche Kosten an. 

Weitere Informationen zu den Kosten finden Sie in den [Preisdetails](https://azure.microsoft.com/pricing/details/logic-apps/) und im [Preismodell](../logic-apps/logic-apps-pricing.md) für Logic Apps. 

## <a name="popular-connectors"></a>Bekannte Connectors
Es gibt Tausende von Anwendungen und Millionen von Ausführungen, die mithilfe dieser Connectors erfolgreich Daten und Informationen verarbeiten. 

### <a name="built-in-actions"></a>Integrierte Aktionen
Das Logic Apps-Modul bietet Aktionen zum Bearbeiten von Daten, zum Kommunizieren über HTTP sowie zum Steuern des Ablaufs der Logik-App-Definition. Zu diesen Aktionen zählen beispielsweise folgende:

| |  |  |  |
| --- | --- | --- | --- |
| [![API-Symbol][HTTPicon]<br/>**HTTP**][httpdoc] | Verwenden Sie Logik-Apps, um über HTTP mit einem beliebigen Endpunkt zu kommunizieren.| [![API-Symbol][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Erstellen Sie Funktionen, die benutzerdefinierte Ausschnitte von C#- oder Node.js-Code ausführen, und verwenden Sie diese Funktionen dann in Ihren Logik-Apps.  |
| [![API-Symbol][HTTP-Requesticon]<br/>**Anforderung**][HTTP-Requestdoc] | Stellt eine aufrufbare HTTPS-URL bereit, die üblicherweise als Webhook in anderen Anwendungen verwendet wird. Die Logik-App startet, wenn sie eine an diese URL gerichtete Anforderung empfängt. | [![API-Symbol][Recurrenceicon]<br/>**Zeitplan**][recurrencedoc] | Dient zum Starten von Logik-Apps auf der Grundlage einfacher oder komplexer Wiederholungszeitpläne. So können Sie beispielsweise einen ganz einfachen Zeitplan mit einer täglichen Wiederholung oder einen komplexen Zeitplan erstellen, bei dem die Wiederholung jeweils stündlich zwischen 9:00 Uhr und 17:00 Uhr am letzten Freitag des Monats stattfindet. |
| [![API-Symbol][CallLogicApp-icon]<br/>**Logik-<br/>App aufrufen**][nested-logic-appdoc] | Dient zum Aufrufen einer geschachtelten Logik-App. Jede Logik-App mit einem Anforderungstrigger kann als geschachtelte Logik-App aufgerufen werden.| [![API-Symbol][API/Web-Appicon]<br/>**API-App**][api/web-appdoc] | Dient zum Aufrufen einer App Service-API-App. API-Apps mit Swagger werden genau wie andere erstklassige Aktionen gerendert.|

### <a name="standard-connectors"></a>Standardconnectors
Die folgende Tabelle enthält die bekanntesten Connectors und einige Favoriten unserer Benutzer:

| |  |  |  |
| --- | --- | --- | --- |
| [![API-Symbol][AzureBlobStorageicon]<br/>**Azure Blob<br/>Storage**][AzureBlobStoragedoc] | Dieser Connector ist interessant, wenn Sie Aufgaben mit Ihrem Speicherkonto automatisieren möchten. Er unterstützt CRUD-Vorgänge (Create, Read, Update, Delete; Erstellen, Lesen, Aktualisieren, Löschen). | [![API-Symbol][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | Einer der am stärksten nachgefragten Connectors. Er verfügt unter anderem über Trigger und Aktionen zum Automatisieren von Workflows mit Leads. |
| [![API-Symbol][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Nutzen und veröffentlichen Sie Ereignisse auf einem Event Hub. So können Sie beispielsweise mithilfe von Event Hubs eine Ausgabe Ihrer Logik-App abrufen und die Ausgabe anschließend an einen Echtzeitanalyseanbieter senden. | [![API-Symbol][FTPicon]<br/>**FTP**][FTPdoc] | Wenn auf Ihren FTP-Server über das Internet zugegriffen werden kann, können Sie Workflows für die Verwendung von Dateien und Ordnern automatisieren. <br/><br/>Mit dem SFTP-Connector ist auch SFTP verfügbar. |
| [![API-Symbol][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Zahlreiche Trigger und noch mehr Aktionen für die Verwendung von Office 365-E-Mails und -Ereignissen innerhalb Ihrer Workflows. <br/><br/>Dieser Connector bietet auch eine Aktion vom Typ *Genehmigungs-E-Mail* zur Genehmigung von Urlaubsanträgen, Spesenabrechnungen und Ähnlichem. <br/><br/>Mit dem Office 365-Benutzer-Connector sind auch Office 365-Benutzer verfügbar.| [![API-Symbol][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Melden Sie sich einfach mit Ihrem Salesforce-Konto an, um unter anderem auf Objekte (beispielsweise Leads) zugreifen zu können. | 
| [![API-Symbol][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | Der am häufigsten verwendete Connector in Logik-Apps. Er enthält Trigger und Aktionen für asynchrones Messaging sowie zum Veröffentlichen/Abonnieren mit Warteschlangen, Abonnements und Themen. |  [![API-Symbol][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | Wenn Sie SharePoint verwenden und etwas automatisieren möchten, sollten Sie sich diesen Connector näher ansehen. Er eignet sich sowohl für lokale SharePoint-Instanzen als auch für SharePoint Online. |
| [![API-Symbol][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | Einer der am häufigsten verwendeten Connectors. Er kann eine Verbindung mit einer lokalen SQL Server-Instanz sowie mit Azure SQL-Datenbank herstellen. | [![API-Symbol][Twittericon]<br/>**Twitter**][Twitterdoc] | Melden Sie sich einfach mit einem Twitter-Konto an, und starten Sie einen neuen Workflow, wenn ein neuer Tweet gepostet wird. Die Tweets können dann in einer SQL-Datenbank oder in einer SharePoint-Liste gespeichert werden. | 

### <a name="on-premises-connectors"></a>Lokale Connectors 

Lokale Connectors ermöglichen den Zugriff auf Daten auf lokalen Servern.  Für die Verbindungsherstellung mit einem lokalen Server wird ein [lokales Datengateway][gatewaydoc] benötigt, das einen sicheren Kommunikationskanal bereitstellt, ohne die Netzwerkinfrastruktur konfigurieren zu müssen.  Zu diesen Connectors zählen beispielsweise folgende:

|  |  |  |  |
| --- | --- | --- | --- |
| [![API-Symbol][db2icon]<br/>**DB2**][db2doc] | [![API-Symbol][oracle-DB-icon]<br/>**Oracle-Datenbank**][oracle-db-doc] | [![API-Symbol][sharepointicon]<br/>**SharePoint</br> Server**][sharepointserver] | [![API-Symbol][filesystem-icon]<br/>**Datei-</br> system**][filesystemdoc] |
[![API-Symbol][sql-servericon]<br/>**SQL</br> Server**][sql-serverdoc] | ![API-Symbol][Biztalk-Servericon]<br/>**BizTalk</br> Server**| |

### <a name="integration-account-connectors"></a>Integrationskonto-Connectors 

Das Enterprise Integration Pack (EIP) umfasst Connectors, die in der BizTalk Server-Community bestens bekannt sind. Beim Kauf eines [Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) erhalten Sie auch folgende Connectors: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API-Symbol][as2icon]<br/>**AS2-</br>Decodierung**][as2decode] | [![API-Symbol][as2icon]<br/>**AS2-</br>Codierung**][as2encode] | [![API-Symbol][x12icon]<br/>**EDIFACT-</br>Decodierung**][EDIFACTdecode] | [![API-Symbol][x12icon]<br/>**EDIFACT-</br>Codierung**][EDIFACTencode] |
[![API-Symbol][flatfileicon]<br/>**Flatfile-</br>Codierung**][flatfiledoc] | [![API-Symbol][flatfiledecodeicon]<br/>**Flatfile-</br>Decodierung**][flatfiledecodedoc] | [![API-Symbol][integrationaccounticon]<br/>**Integrations-<br/>konto**][integrationaccountdoc] | [![API-Symbol][xmltransformicon]<br/>**XML-<br/>Transformation**][xmltransformdoc] |
| [![API-Symbol][x12icon]<br/>**X12-</br>Decodierung**][x12decode] | [![API-Symbol][x12icon]<br/>**X12-</br>Codierung**][x12encode] | [![API-Symbol][xmlvalidateicon]<br/>**XML-<br/>Überprüfung**][xmlvalidatedoc] | |

### <a name="enterprise-connectors"></a>Enterprise-Connectors

Stellen Sie in Ihren Logik-Apps eine Verbindung mit Ihren Unternehmensanwendungen her.

|  |  |
| --- | --- |
|[![API-Symbol][MQicon]<br/>**MQ**][mqdoc]|[![API-Symbol][SAPicon]<br/>**SAP**][sapconnector]|

> [!TIP]
> Wenn Sie sich zunächst mit Azure Logic Apps vertraut machen möchten, ehe Sie sich für ein Azure-Konto registrieren, können Sie [Logic Apps testen](https://tryappservice.azure.com/?appservice=logic). Sie können sofort eine kurzlebige Starter-Logik-App erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

## <a name="connectors-as-triggers-and-actions"></a>Connectors als Trigger und Aktionen

Ein **Trigger** startet eine Instanz Ihrer Logik-App oder führt sie aus. Einige Connectors umfassen Trigger, die Ihre App benachrichtigen, wenn bestimmte Ereignisse eintreten. Der FTP-Connector verfügt beispielsweise über den `OnUpdatedFile`-Trigger, der Ihre Logik-App startet, wenn eine Datei aktualisiert wird. 

In Logic Apps stehen folgende Arten von Triggern zur Verfügung:  

* *Abfragetrigger:* Diese Trigger fragen den Dienst in angegebenen Abständen auf neue Daten ab. 

    Sobald neue Daten verfügbar sind, wird eine neue Instanz Ihrer Logik-App ausgeführt, die diese Daten als Eingabe verwendet. 

* *Pushtrigger:* Diese Trigger lauschen auf den Eingang von Daten an einem Endpunkt oder auf das Eintreten eines Ereignisses und lösen eine neue Instanz Ihrer Logik-App aus.

* *Wiederholungstrigger:* Dieser Trigger instanziiert eine Instanz Ihrer Logik-App nach einem festgelegten Zeitplan.

Connectors bieten auch **Aktionen**, die Sie in Ihrem Workflow verwenden können. So kann Ihre Logik-App etwa Daten suchen und diese später in Ihrer Logik-App verwenden. Etwas spezifischer: Sie können Kundendaten in einer SQL-Datenbank suchen und diese Kundendaten anschließend zur Erstellung Ihres Workflows verwenden. 

> [!TIP]
> Ausführlichere Informationen zu Triggern und Aktionen finden Sie in der [Übersicht über Connectors](connectors-overview.md). 


## <a name="message-manipulation-actions"></a>Nachrichtenbearbeitungsaktionen

Logic Apps verfügt über integrierte Aktionen zum Ändern oder Bearbeiten der Nutzlastdaten. Der integrierte Connector **Datenvorgänge** enthält folgende Aktionen: 

| | |
|---|---|
| **Verfassen** | Erstellen oder generieren Sie Werte oder Objekte zur späteren Verwendung oder zur Verwendung bei der Workflowerstellung. So können Sie beispielsweise ein JSON-Objekt mit Werten aus mehreren Schritten erstellen oder eine Konstante berechnen und später bei der Ausführung einer Logik-App darauf verweisen. |
| **CSV-Tabelle erstellen**<br/>**HTML-Tabelle erstellen** | Konvertieren Sie ein Array-Resultset in eine CSV- oder HTML-Tabelle. Fügen Sie beispielsweise die CRM-Aktion „Datensätze auflisten“ und einen Filter für am aktuellen Tag hinzugefügte Datensätze hinzu. Senden Sie die Ergebnisse anschließend per E-Mail als HTML-Tabelle. |
| **Array filtern** (Abfrage) | Filtern Sie ein Resultset nach den für Sie interessanten Einträgen. Suchen Sie beispielsweise nach allen Tweets mit `#Azure`, und filtern Sie die zurückgegebenen Tweets anschließend nach Ergebnissen, die die Bedingung `Tweeted_by_followers > 50` erfüllen. |
| **Join** | Verknüpfen Sie ein Array unter Verwendung eines Trennzeichens. Der Vorgang zur Erkennung von Schlüsselausdrücken gibt beispielsweise ein Array mit Schlüsselausdrücken zurück. Diese können etwa durch `,` verknüpft werden. Anstelle von `["Some", "Phrase"]` erhalten Sie dann `"Some, Phrase"`. |
| **JSON analysieren** | Analysieren Sie Werte aus einem JSON-Objekt im Designer, und greifen Sie darauf zu. Wenn Ihre Azure-Funktion also beispielsweise eine JSON-Nutzlast zurückgibt, können Sie diese analysieren, um später in einem anderen Schritt auf die JSON-Eigenschaften zuzugreifen. Die Aktion prüft außerdem zur Laufzeit, ob der JSON-Code dem angegebenen Schema entspricht. | 
| **Auswählen** | Wählen Sie bestimmte Eigenschaften eines Arrays zur weiteren Verarbeitung aus. Wenn Sie Datensätze aus SQL auflisten und dabei 15 Spalten zurückgegeben werden, haben Sie die Möglichkeit, nur einige dieser Spalten zur weiteren Verarbeitung auszuwählen. Die Ausgabe ist ein Array, das nur die ausgewählten Eigenschaften enthält. |

## <a name="custom-connectors-and-azure-certification"></a>Benutzerdefinierte Connectors und Azure-Zertifizierung 

Zum Aufrufen von APIs, die benutzerdefinierten Code ausführen oder nicht als Connector verfügbar sind, können Sie die Logic Apps-Plattform erweitern, indem Sie [REST-basierte API-Apps erstellen](../logic-apps/logic-apps-create-api-app.md). Sie können auch eigene [benutzerdefinierte Connectors](../logic-apps/custom-connector-overview.md) erstellen, die für jede beliebige Logik-App in Ihrem Abonnement verfügbar gemacht werden können.

Wenn Sie Ihre benutzerdefinierten API-Apps öffentlich machen und für die Verwendung in Azure zur Verfügung stellen möchten, können Sie [Ihre Connectors zur Zertifizierung an Microsoft übermitteln](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

Fehlt noch ein Connector-Thema oder Details, die Ihrer Meinung nach wichtig sind? Falls ja, dann helfen Sie uns, indem Sie unsere vorhandenen Themen ergänzen oder eigene erstellen. Unsere Dokumentation basiert auf Open Source und ist auf GitHub gehostet. Beginnen Sie mit unserem [GitHub-Repository](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen Ihrer ersten Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)
* [Erstellen einer benutzerdefinierten API zur Verwendung mit Logik-Apps](../logic-apps/logic-apps-create-api-app.md)
* [Überwachen von Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[gatewaydoc]: ../logic-apps/logic-apps-gateway-connection.md "Herstellen einer Verbindung mit lokalen Datenquellen über Logik-Apps mit dem lokalen Datengateway"
[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integration von Logik-Apps in App Service-API-Apps"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Verwalten von Dateien in Ihrem Blobcontainer mit Azure Blob Storage-Connector"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Integration von Logik-Apps in Azure Functions"
[db2doc]: ./connectors-create-api-db2.md "Stellen Sie eine Verbindung mit IBM DB2 in der Cloud oder lokal her. Sie können eine Zeile aktualisieren, eine Tabelle abrufen und vieles mehr."
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Herstellen einer Verbindung mit Dynamics CRM Online zur Verwendung von CRM Online-Daten"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Herstellen einer Verbindung mit Azure Event Hubs. Empfangen und Senden von Ereignissen zwischen Logik-Apps und Event Hubs."
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Herstellen einer Verbindung mit einem lokalen Dateisystem"
[ftpdoc]: ./connectors-create-api-ftp.md "Herstellen einer Verbindung mit einem FTP/FTPS-Server für FTP-Aufgaben, z.B. Hochladen, Abrufen, Löschen von Dateien usw."
[httpdoc]: ./connectors-native-http.md "Durchführen von HTTP-Aufrufen mit dem HTTP-Connector"
[http-requestdoc]: ./connectors-native-reqres.md "Hinzufügen von Aktionen für HTTP-Anforderungen und -Antworten zu Ihren Logik-Apps"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "Durchführen von HTTP-Aufrufen mit HTTP + Swagger-Connector"
[informixdoc]: ./connectors-create-api-informix.md "Stellen Sie eine Verbindung mit Informix in der Cloud oder lokal her. Sie können eine Zeile lesen, die Tabellen auflisten und vieles mehr."
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Integrieren von Logik-Apps in geschachtelte Workflows"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Stellen Sie eine Verbindung mit Ihrem Office 365-Konto her. Sie können E-Mails senden und empfangen, Ihren Kalender und Kontakte verwalten und vieles mehr."
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Herstellen einer Verbindung mit einer Oracle-Datenbank zum Hinzufügen, Einfügen und Löschen von Zeilen sowie für vieles mehr."
[mqdoc]: ./connectors-create-api-mq.md "Herstellen einer Verbindung mit MQ (lokal oder in Azure) und Senden und Empfangen von Nachrichten"
[recurrencedoc]:  ./connectors-native-recurrence.md "Auslösen von sich wiederholenden Aktionen für Logik-Apps"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Stellen Sie eine Verbindung mit Ihrem Salesforce-Konto her. Sie können Konten, Leads, Vertriebschancen usw. verwalten."
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Herstellen einer Verbindung mit einem lokalen SAP-System"
[service-busdoc]: ./connectors-create-api-servicebus.md "Senden von Nachrichten aus Service Bus-Warteschlangen und -Themen und Empfangen von Nachrichten aus Service Bus-Warteschlangen und -Abonnements"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Stellen Sie eine Verbindung mit SharePoint Online her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Stellen Sie eine Verbindung mit dem lokalen SharePoint-Server her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Stellen Sie eine Verbindung mit Azure SQL-Datenbank oder SQL Server her. Sie können Einträge in einer SQL-Datenbanktabelle erstellen, aktualisieren, abrufen und löschen."
[twitterdoc]: ./connectors-create-api-twitter.md "Stellen Sie eine Verbindung mit Twitter her. Sie können Timelines abrufen, Tweets posten und vieles mehr."
[webhookdoc]: ./connectors-native-webhook.md "Hinzufügen von Webhookaktionen und Triggern zu Ihren Logik-Apps"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Informationen zu Enterprise Integration (AS2)."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Informationen zu Enterprise Integration (X12)."
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Informationen zu Enterprise Integration (Flatfile)."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Informationen zu Enterprise Integration (Flatfile)."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Informationen zu Enterprise Integration (XML-Überprüfung)."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Informationen zu Enterprise Integration (Transformationen)."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Informationen zu Enterprise Integration (AS2-Decodierung)."
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Informationen zu Enterprise Integration (AS2-Codierung)."
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Informationen zu Enterprise Integration (X12-Decodierung)."
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Informationen zu Enterprise Integration (X12-Codierung)."
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Informationen zu Enterprise Integration (EDIFACT-Decodierung)."
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Informationen zu Enterprise Integration (EDIFACT-Codierung)."
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Suchen nach Schemas, Zuordnungen, Partnern und mehr in Ihrem Integrationskonto"


[boxDoc]: ./connectors-create-api-box.md "Stellen Sie eine Verbindung mit Box her. Sie können Dateien hochladen, abrufen, löschen, auflisten und vieles mehr."
[delaydoc]: ./connectors-native-delay.md "Durchführen von verzögerten Aktionen"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Stellen Sie eine Verbindung mit Dropbox her. Sie können Dateien hochladen, abrufen, löschen, auflisten und vieles mehr."
[facebookdoc]: ./connectors-create-api-facebook.md "Stellen Sie eine Verbindung mit Facebook her. Sie können eine Timeline posten, einen Seitenfeed abrufen und vieles mehr."
[githubdoc]: ./connectors-create-api-github.md "Herstellen einer Verbindung mit GitHub und Nachverfolgen von Problemen"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Herstellen einer Verbindung mit GoogleDrive zur Verwendung mit Ihren Daten"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Herstellen einer Verbindung mit Google Sheets zur Bearbeitung von Tabellen"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Herstellen einer Verbindung mit Google Tasks zur Verwaltung Ihrer Aufgaben"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Verbindung mit Google Calendar zum Verwalten von Kalendern."
[http-responsedoc]: ./connectors-native-reqres.md "Hinzufügen von Aktionen für HTTP-Anforderungen und -Antworten zu Ihren Logik-Apps"
[instagramdoc]: ./connectors-create-api-instagram.md "Stellen Sie eine Verbindung mit Instagram her. Lösen Sie Ereignisse aus, oder reagieren Sie darauf."
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Stellen Sie eine Verbindung mit Ihrem MailChimp-Konto her. Verwalten und automatisieren Sie E-Mails."
[mandrilldoc]: ./connectors-create-api-mandrill.md "Herstellen einer Verbindung mit Mandrill zur Kommunikation"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Stellen Sie eine Verbindung mit Microsoft Translator her. Sie können Text übersetzen, Sprachen erkennen und vieles mehr." 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Abrufen von Videoinformationen, Videolisten und Kanälen und Wiedergabe-URLs für Office 365-Videos"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Stellen Sie eine Verbindung mit Ihrer persönlichen Microsoft OneDrive-Instanz her. Sie können Dateien hochladen, löschen, auflisten und vieles mehr."
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Stellen Sie eine Verbindung mit Ihrer geschäftlichen Microsoft OneDrive-Instanz her. Sie können Dateien hochladen, löschen, auflisten und vieles mehr."
[outlook.comdoc]: ./connectors-create-api-outlook.md "Stellen Sie eine Verbindung mit Ihrem Outlook-Postfach her. Sie können E-Mails, Kalender, Kontakte usw. verwalten."
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Stellen Sie eine Verbindung mit Microsoft Project Online her. Sie können Projekte, Aufgaben, Ressourcen usw. verwalten."
[querydoc]: ./connectors-native-query.md "Auswählen und Filtern von Arrays mit der Abfrageaktion"
[rssdoc]: ./connectors-create-api-rss.md "Sie können Feedelemente veröffentlichen und abrufen und Vorgänge auslösen, wenn ein neues Element in einem RSS-Feed veröffentlicht wird."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Stellen Sie eine Verbindung mit SendGrid her. Sie können E-Mails senden und die Liste der Empfänger verwalten."
[sftpdoc]: ./connectors-create-api-sftp.md "Stellen Sie eine Verbindung mit Ihrem SFTP-Konto her. Sie können Dateien hochladen, abrufen, löschen und vieles mehr."
[slackdoc]: ./connectors-create-api-slack.md "Herstellen einer Verbindung mit Slack zum Veröffentlichen von Nachrichten in Slack-Kanälen"
[smtpdoc]: ./connectors-create-api-smtp.md "Herstellen einer Verbindung mit einem SMTP-Server und Senden von E-Mails mit Anhängen"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Herstellen einer Verbindung mit SparkPost zur Kommunikation"
[trellodoc]: ./connectors-create-api-trello.md "Stellen Sie eine Verbindung mit Trello her. Sie können Ihre Projekte verwalten und viele verschiedene Dinge organisieren."
[twiliodoc]: ./connectors-create-api-twilio.md "Stellen Sie eine Verbindung mit Twilio her. Sie können Nachrichten senden und empfangen, verfügbare Nummern abrufen, eingehende Telefonnummern verwalten und vieles mehr."
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Stellen Sie eine Verbindung mit Wunderlist her. Sie können Ihre Aufgaben und Aufgabenlisten verwalten, Ihre gesamten Aufgaben synchronisieren und vieles mehr."
[yammerdoc]: ./connectors-create-api-yammer.md "Stellen Sie eine Verbindung mit Yammer her. Sie können Nachrichten posten, neue Nachrichten abrufen und vieles mehr."
[youtubedoc]: ./connectors-create-api-youtube.md "Stellen Sie eine Verbindung mit YouTube her. Sie können Ihre Videos und Kanäle verwalten."


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[AppServices-icon]: ./media/apis-list/AppServices.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FileSystem-icon]: ./media/apis-list/filesystem.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/appservices.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[CallLogicApp-icon]: ./media/apis-list/calllogicapp.png
[Delayicon]: ./media/apis-list/delay.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png

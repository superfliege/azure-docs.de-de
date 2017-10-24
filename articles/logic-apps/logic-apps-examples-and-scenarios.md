---
title: "Beispiele und allgemeine Szenarien – Azure Logic Apps | Microsoft-Dokumentation"
description: "Erfahren Sie anhand von Beispielen, Szenarien, Tutorials und exemplarischen Vorgehensweisen mehr über Logik-Apps."
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 09/13/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5b2b82d90dee41e80233e5f52c960be23d89ee3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Gängige Szenarien, Beispiele, Tutorials und exemplarische Vorgehensweisen für Azure Logic Apps

[Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) unterstützt Sie bei der Orchestrierung und Integration verschiedener Dienste durch die Bereitstellung von [mehr als 100 einsatzbereiten Connectors](../connectors/apis-list.md), die von lokalem SQL Server über SAP bis hin zu Microsoft Cognitive Services reichen. Der Logic Apps-Dienst arbeitet „serverlos“. Daher müssen Sie sich über Skalierung oder Instanzen keine Gedanken machen. Sie brauchen lediglich den Workflow mit einem Trigger und Aktionen definieren, die im Workflow ausgeführt werden. Die zugrundeliegende Plattform kümmert sich um Skalierung, Verfügbarkeit und Leistung. Logic Apps ist besonders nützlich für Anwendungsfälle und Szenarien, in denen Sie mehrere Aktionen auf verschiedenen Systemen koordinieren müssen.

Um mehr über die vielen Muster und Funktionen zu erfahren, die von [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) unterstützt werden, finden Sie hier einige gängige Beispiele und Szenarien.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Beliebte Ausgangspunkte für Logik-App-Workflows

Jede Logik-App beginnt mit nur einem [*Trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), der Ihren Logik-App-Workflow startet und alle Daten als Teil dieses Triggers übergibt. Einige Connectors bieten Trigger, die einen der folgenden Typen haben:

* *Abruftrigger*: Fragt einen Dienstendpunkt regelmäßig auf neue Daten ab. Wenn neue Daten vorhanden sind, wird der Trigger erstellt, der eine neue Workflowinstanz mit den Daten als Eingabe ausführt.

* *Pushtrigger*: Lauscht auf Daten am Dienstendpunkt und wartet, bis ein bestimmtes Ereignis erfolgt. Wenn das Ereignis eintritt, wird der Trigger sofort ausgelöst. Eine neue Workflowinstanz wird angelegt und ausgeführt, die alle verfügbaren Daten als Eingabe verwendet.

Hier nur einige Beispiele beliebter Trigger:

* Abruf: 

  * Der Trigger[**Zeitplan: Wiederholung**](../connectors/connectors-native-recurrence.md) ermöglicht Ihnen das Festlegen von Startdatum und -uhrzeit sowie der Wiederholung zum Auslösen Ihrer Logik-App. 
  Beispielsweise können Sie die Wochentage und Tageszeiten auswählen, um Ihre Logik-App auszulösen.

  * Mit dem Trigger "Wenn eine E-Mail empfangen wird" können Sie Ihre Logik-App von jedem E-Mail-Anbieter, der von Logic Apps unterstützt wird, z.B. [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [Outlook.com](https://docs.microsoft.com/connectors/outlook/) usw., auf neue E-Mails überprüfen.

  * Der [**HTTP**-Trigger](../connectors/connectors-native-http.md) lässt Ihre Logik-App einen angegebenen Dienstendpunkt überprüfen, indem über HTTP kommuniziert wird.
  
* Push:

  * Der Trigger [**Anforderung/Antwort: Anforderung**](../connectors/connectors-native-reqres.md) ermöglicht Ihrer Logik-App, HTTP-Anforderungen zu empfangen und in Echtzeit auf Ereignisse zu reagieren.

  * Der Trigger [**HTTP-Webhook**](../connectors/connectors-native-webhook.md) abonniert einen Dienstendpunkt durch Registrieren einer *Rückruf-URL* beim jeweiligen Dienst. 
  Auf diese Weise kann der Dienst den Trigger einfach benachrichtigen, wenn das angegebene Ereignis eintritt, sodass der Trigger den Dienst nicht mehr abfragen muss.

Nach Empfangen einer Benachrichtigung über neue Daten oder ein Ereignis wird der Trigger ausgelöst. Er erzeugt eine neue Logik-App-Workflowinstanz und führt die Aktionen im Workflow aus. Sie können im gesamten Workflow auf beliebige Daten im Trigger zugreifen. Der Trigger "Bei einem neuen Tweet" übergibt z.B. den Tweetinhalt an die Ausführung der Logik-App. 

## <a name="respond-to-triggers-and-extend-actions"></a>Reagieren auf Trigger und Erweitern von Aktionen

Für Systeme und Dienste, für die möglicherweise keine veröffentlichten Connectors verfügbar sind, können Sie Logic Apps darüber hinaus erweitern.

* [Erstellen von benutzerdefinierten Triggern oder Aktionen](../logic-apps/logic-apps-create-api-app.md)
* [Einrichten von Aktionen mit langer Ausführungszeit für Workflowausführungen](../logic-apps/logic-apps-create-api-app.md)
* [Reagieren auf externe Ereignisse und Aktionen mit Webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Aufrufen, Auslösen oder Schachteln von Workflows mithilfe von synchronen Antworten auf HTTP-Anforderungen](../logic-apps/logic-apps-http-endpoint.md)
* [Tutorial: Erstellen eines KI-gestützten sozialen Dashboards innerhalb von Minuten mithilfe von Logic Apps und Power BI](http://aka.ms/logicappsdemo)
* [Tutorial: Reagieren auf Twilio SMS-Webhooks und Senden einer Textantwort](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Ablaufsteuerungs-, Fehlerbehandlungs- und Protokollierungsfunktionen

Logic Apps enthalten umfangreiche Funktionen für die erweiterte Ablaufsteuerung, wie Bedingungen, Schalter, Schleifen und Bereiche. Um robuste Lösungen sicherzustellen, können Sie darüber hinaus in Ihren Workflows Fehler- und Ausnahmebehandlung implementieren. Für Benachrichtigungs- und Diagnoseprotokolle für den Status von Workflowausführungen stellt Azure Logic Apps ferner Überwachung und Warnungen zur Verfügung.

* [Verarbeiten von Elementen in Arrays und Sammlungen mit Schleifen und Batches in Logic Apps](../logic-apps/logic-apps-loops-and-scopes.md)
* [Ausführen von verschiedenen Aktionen mit switch-Anweisungen](../logic-apps/logic-apps-switch-case.md)
* [Erstellen von Fehler- und Ausnahmebehandlung in einem Workflow](../logic-apps/logic-apps-exception-handling.md)
* [Anwendungsfall: Wie ein Unternehmen im Gesundheitswesen Logic Apps-Ausnahmebehandlung für HL7 FHIR-Workflows einsetzt](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Monitor and get insights about logic app runs with Operations Management Suite (OMS) and Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md) (Überwachen und Gewinnen von Erkenntnissen zu Logik-App-Ausführungen mit Operations Management Suite (OMS) und Log Analytics)

## <a name="deploy-and-manage-logic-apps"></a>Bereitstellen und Verwalten von Logic Apps

Sie können Logic Apps mithilfe von Visual Studio, Visual Studio Team Services oder beliebigen anderen Tools für Quellcodeverwaltung und automatische Builderstellung vollständig entwickeln und bereitstellen. Um die Entwicklung für Workflows und abhängige Verbindungen in einer Ressourcenvorlage zu unterstützen, verwendet Logic Apps Azure-Vorlagen für die Ressourcenbereitstellung. Visual Studio-Tools generieren diese Vorlagen automatisch, die Sie zur Versionsverwaltung in die Quellcodeverwaltung einchecken können.

* [Erstellen und Bereitstellen von Logik-Apps von Visual Studio aus](../logic-apps/logic-apps-deploy-from-vs.md)
* [Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Erstellen einer Vorlage zur automatisierten Bereitstellung](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Inhaltstypen, Konvertierungen und Transformationen während der Ausführung

Mithilfe der vielen Funktionen in der [Workflowdefinitionssprache](http://aka.ms/logicappsdocs) von Azure Logic Apps können Sie auf mehrere Inhaltstypen zugreifen, sie konvertieren und transformieren. Beispielsweise können Sie mit den Workflowausdrücken `@json()` und `@xml()` zwischen einer Zeichenfolge, JSON und XML konvertieren. Das Logic Apps-Modul behält Inhaltstypen bei, um die verlustfreie Übertragung von Inhalten zwischen Diensten zu unterstützen.

* [Funktionsweise von Workflowausdrücken in Logic Apps](../logic-apps/logic-apps-author-definitions.md)
* [Verarbeitung von nicht als JSON vorliegenden Inhaltstypen](../logic-apps/logic-apps-content-type.md), wie `application/xml`, `application/octet-stream` und `multipart/formdata`
* [Referenz: Azure Logic Apps-Workflowdefinitionssprache](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Weitere Integrationen und Funktionen

Logic Apps bieten außerdem Integration mit vielen Diensten, wie etwa Azure Functions, Azure API Management, Azure App Services und benutzerdefinierten HTTP-Endpunkten, z.B. REST und SOAP.

* [Erstellen eines sozialen Dashboards in Echtzeit mit Azure Serverless](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Aufrufen von Azure Functions aus Logic-Apps](../logic-apps/logic-apps-azure-functions.md)
* [Szenario: Auslösen von Logic Apps mit Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Blog: Call SOAP endpoints from logic apps (Aufrufen von SOAP-Endpunkten aus Logic Apps)](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Komplettszenarien

* [Whitepaper: Nahtlose Fallverwaltung durch Unternehmensintegration mit Azure-Diensten wie Logic Apps](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Workflowdefinitionen mit der Workflowdefinitionssprache ](../logic-apps/logic-apps-author-definitions.md)
* [Behandeln von Fehlern und Ausnahmen in Logic Apps](../logic-apps/logic-apps-exception-handling.md)
* [Senden Sie Ihre Kommentare, Fragen, Feedback und Vorschläge für Verbesserungen an Azure-Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)
---
title: Vergleich von Flow, Logic Apps, Functions und WebJobs – Azure
description: 'Hier finden Sie einen Vergleich der für Integrationsaufträge optimierten Microsoft Cloud Services: Flow, Logic Apps, Functions und WebJobs.'
services: functions, logic-apps
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Microsoft Flow, Flow, Logic Apps, Azure Functions, Functions, Azure WebJobs, WebJobs, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 04/09/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c22d75b4eb058942da5a3b0699cbb4a23eec42f3
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634930"
---
# <a name="compare-flow-logic-apps-functions-and-webjobs"></a>Vergleich von Flow, Logic Apps, Functions und WebJobs

In diesem Artikel werden die folgenden Microsoft Cloud Services miteinander verglichen:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure-Funktionen](https://azure.microsoft.com/services/functions/)
* [WebJobs in Azure App Service](../app-service/web-sites-create-web-jobs.md)

Mit allen diesen Diensten können Integrationsprobleme gelöst und Geschäftsprozesse automatisiert werden. Jeder der Dienste kann Eingaben, Aktionen, Bedingungen und Ausgaben definieren. Sie können die Dienste individuell nach einem Zeitplan oder auslöserbasiert ausführen. Jeder Dienst hat jedoch seine eigenen Vorteile. In diesem Artikel werden die Unterschiede beschrieben.

## <a name="compare-microsoft-flow-and-azure-logic-apps"></a>Vergleich zwischen Microsoft Flow und Azure Logic Apps

Flow und Logic Apps sind Integrationsdienste nach dem *Designer First*-Prinzip, die Workflows erstellen können. Beide Dienste können in verschiedene SaaS- und Unternehmensanwendungen integriert werden. 

Flow basiert auf Logic Apps. Es werden jeweils die gleichen Workflow-Designer und [Connectors](../connectors/apis-list.md) verwendet. 

Flow ermöglicht allen Mitarbeitern im Büro das Durchführen einfacher Integrationen (z.B. einen Genehmigungsprozess für eine SharePoint-Dokumentbibliothek) ohne Umweg über Entwickler oder IT. Andererseits ermöglicht Logic Apps erweiterte Integrationen (z.B. B2B-Prozesse), die DevOps und Sicherheitsvorkehrungen auf Unternehmensebene erfordern. In der Regel werden Geschäftsworkflows im Laufe der Zeit immer komplexer. Entsprechend können Sie zunächst mit einem Datenfluss beginnen und diesen dann nach Bedarf in eine Logik-App konvertieren.

Anhand der folgenden Tabelle können Sie ermitteln, ob Flow oder Logic Apps für eine bestimmte Integration am besten geeignet ist.

|  | Flow | Logic Apps |
| --- | --- | --- |
| Benutzer |Büroangestellte, geschäftliche Benutzer, SharePoint-Administratoren |Professionelle Integratoren und Entwickler, IT-Experten |
| Szenarien |Self-Service |Erweiterte Integrationen |
| Designtool |Im Browser und in der mobilen App, nur über die Benutzeroberfläche |Im Browser und [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Codeansicht](../logic-apps/logic-apps-author-definitions.md) verfügbar |
| Application Lifecycle Management (ALM) |Entwerfen und Testen in Nicht-Produktionsumgebungen, Überführung in Produktion, nachdem die Bereitschaft erzielt wurde. |DevOps: Quellcodeverwaltung, Tests, Support, Automatisierung und Verwaltung in [Azure Resource Management](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Administratorerfahrung |Verwalten von Flow-Umgebungen und Richtlinien zur Verhinderung von Datenverlust (Data Loss Prevention, DLP), Nachverfolgen der Lizenzierung [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Verwalten von Ressourcengruppen, Verbindungen, Zugriffsverwaltung und Protokollierung [https://portal.azure.com](https://portal.azure.com) |
| Sicherheit |Office 365-Sicherheit und Konformitätsüberwachungsprotokolle, Verhinderung von Datenverlust (Data Loss Prevention, DLP), [Verschlüsselung ruhender Daten](https://wikipedia.org/wiki/Data_at_rest#Encryption) für sensible Daten usw. |Sicherheitsgarantie von Azure: [Azure Security](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [Überwachungsprotokolle](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) und vieles mehr. |

## <a name="compare-azure-functions-and-azure-logic-apps"></a>Vergleich zwischen Azure Functions und Azure Logic Apps

Functions und Logic Apps sind Azure-Dienste, die die Verwendung serverloser Workloads ermöglichen. Azure Functions ist ein serverloser Computedienst, Azure Logic Apps stellt serverlose Workflows bereit. Beide können komplexe *Orchestrierungen* erstellen. Eine Orchestrierung ist eine Sammlung von Funktionen oder Schritten (in Logic Apps *Aktionen* genannt), die für eine komplexe Aufgabe ausgeführt werden. Zur Verarbeitung einer Reihe von Aufträgen können Sie beispielsweise mehrere Instanzen einer Funktion parallel ausführen, auf die Beendigung aller Instanzen warten und anschließend eine Funktion ausführen, die ein Ergebnis für das Aggregat berechnet.

Für Azure Functions entwickeln Sie Orchestrierungen, indem Sie Code schreiben und die [Erweiterung „Durable Functions“](durable/durable-functions-overview.md) verwenden. Für Logic Apps erstellen Sie Orchestrierungen über die grafische Benutzeroberfläche oder durch Bearbeiten von Konfigurationsdateien.

In einer Orchestrierung können die Dienste nach Belieben miteinander kombiniert werden. Sie können also Funktionen in Logik-Apps aufrufen und umgekehrt. Bei der Erstellung der jeweiligen Orchestrierung können Sie sich an den Funktionen der Dienste oder an Ihren persönlichen Präferenzen orientieren. Die folgende Tabelle enthält einige der wichtigsten Unterschiede zwischen den Diensten:
 
|  | Langlebige Funktionen | Logic Apps |
| --- | --- | --- |
| Entwicklung | Code First (imperativ) | Designer First (deklarativ) |
| Konnektivität | [Etwa ein Dutzend integrierte Bindungstypen.](functions-triggers-bindings.md#supported-bindings) Schreiben Sie Code für benutzerdefinierte Bindungen. | [Umfangreiche Sammlung von Connectors](../connectors/apis-list.md), [Enterprise Integration Pack für B2B-Szenarien](../logic-apps/logic-apps-enterprise-integration-overview.md), [Erstellen von benutzerdefinierten Connectors](../logic-apps/custom-connector-overview.md) |
| Aktionen | Jede Aktivität ist eine Azure-Funktion. Schreiben Sie Code für Aktivitätsfunktionen. |[Umfangreiche Sammlung vorgefertigter Aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md)|
| Überwachung | [Azure Application Insights](../application-insights/app-insights-overview.md) | [Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md), [Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps.md)|
| Verwaltung | [REST-API](durable/durable-functions-http-api.md), [Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer) | [Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md), [REST-API](https://docs.microsoft.com/rest/api/logic/), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp/?view=azurermps-5.6.0), [Visual Studio](https://docs.microsoft.com/azure/logic-apps/manage-logic-apps-with-visual-studio) |
| Ausführungskontext | Kann [lokal](functions-runtime-overview.md) oder in der Cloud ausgeführt werden. | Kann nur in der Cloud ausgeführt werden.|

<a name="function"></a>

## <a name="compare-functions-and-webjobs"></a>Vergleich von Functions und WebJobs

Wie bei Azure Functions auch, handelt es sich bei WebJobs in Azure App Service um einen *Code-First*-Integrationsdienst, der für Entwickler konzipiert ist. Beide Dienste basieren auf [Azure App Service](../app-service/app-service-web-overview.md) und unterstützen Features wie [Integration der Quellcodeverwaltung](../app-service/app-service-continuous-deployment.md), [Authentifizierung](../app-service/app-service-authentication-overview.md) und [Überwachung per Application Insights-Integration](functions-monitoring.md).

### <a name="webjobs-and-the-webjobs-sdk"></a>WebJobs und das WebJobs SDK

Mit dem Feature *WebJobs* von App Service können Sie ein Skript oder Code im Kontext einer App Service-Web-App ausführen. Das *WebJobs SDK* ist ein für WebJobs entwickeltes Framework, mit dem der Code vereinfacht wird, den Sie als Reaktion auf Ereignisse in Azure-Diensten schreiben. Beispielsweise können Sie auf die Erstellung eines Image-Blobs in Azure Storage reagieren, indem Sie ein Miniaturbild erstellen. Das WebJobs SDK wird als .NET-Konsolenanwendung ausgeführt, die Sie in einem WebJob bereitstellen können. 

WebJobs und das WebJobs SDK funktionieren am besten zusammen, aber Sie können auch WebJobs ohne das WebJobs SDK (und umgekehrt) verwenden. Ein WebJob kann ein beliebiges Programm oder Skript ausführen, das in der App Service-Sandbox ausgeführt wird. Eine WebJobs SDK-Konsolenanwendung kann überall dort ausgeführt werden, wo eine Konsolenanwendungen ausgeführt wird (beispielsweise auf lokalen Servern).

### <a name="comparison-table"></a>Vergleichstabelle

Azure Functions basiert auf dem WebJobs SDK und verfügt daher über viele gleiche Ereignisauslöser und Verbindungen mit anderen Azure-Diensten. Hier sind einige Faktoren aufgeführt, die Sie beachten sollten, wenn Sie sich zwischen Azure Functions und WebJobs mit dem WebJobs SDK entscheiden:

|  | Functions | WebJobs mit WebJobs SDK |
| --- | --- | --- |
|[Serverloses App-Modell](https://azure.microsoft.com/solutions/serverless/) mit [automatischer Skalierung](functions-scale.md#how-the-consumption-plan-works)|✔||
|[Entwicklung und Tests im Browser](functions-create-first-azure-function.md) |✔||
|[Nutzungsbasierte Bezahlung](functions-scale.md#consumption-plan)|✔||
|[Integration in Logic Apps](functions-twitter-email.md)|✔||
| Auslösende Ereignisse |[Zeitgeber](functions-bindings-timer.md)<br>[Azure Storage-Warteschlangen und -Blobs](functions-bindings-storage-blob.md)<br>[Azure Service Bus-Warteschlangen und -Themen](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Zeitgeber](functions-bindings-timer.md)<br>[Azure Storage-Warteschlangen und -Blobs](functions-bindings-storage-blob.md)<br>[Azure Service Bus-Warteschlangen und -Themen](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Dateisystem](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Unterstützte Sprachen  |C#<br>F#<br>JavaScript<br>Java (Vorschau) |C#<sup>1</sup>|
|Paket-Manager|NPM und NuGet|NuGet<sup>2</sup>|

<sup>1</sup> Für WebJobs (ohne WebJobs SDK) werden C#, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python und mehr unterstützt. Dies ist keine umfassende Liste. Ein WebJob kann ein beliebiges Programm oder Skript ausführen, für das die Ausführung in der App Service-Sandbox möglich ist.

<sup>2</sup> Für WebJobs (ohne WebJobs SDK) werden NPM und NuGet unterstützt.

### <a name="summary"></a>Zusammenfassung

Azure Functions bietet eine höhere Entwicklerproduktivität, mehr Optionen für Programmiersprachen, mehr Optionen bei der Entwicklungsumgebung, mehr Optionen bei der Integration von Azure-Diensten und mehr Preisoptionen. Für die meisten Szenarien ist dies die beste Wahl.

Hier sind zwei Szenarien angegeben, für die WebJobs die beste Wahl sein kann:

* Sie benötigen mehr Kontrolle über den Code, mit dem auf Ereignisse gelauscht wird (`JobHost`-Objekt). Functions verfügt über eine begrenzte Anzahl von Optionen zum Anpassen des `JobHost`-Verhaltens in der Datei [host.json](functions-host-json.md). Es kann sein, dass bei Ihnen eine Anforderung besteht, die nicht mit einer Zeichenfolge in einer JSON-Datei angegeben werden kann. Beispielsweise können Sie nur mit dem WebJobs SDK eine benutzerdefinierte Wiederholungsrichtlinie für Azure Storage konfigurieren.
* Sie verfügen über eine App Service-App, für die Sie Codeausschnitte ausführen und diese gemeinsam in der gleichen DevOps-Umgebung verwalten möchten.

Wählen Sie Azure Functions anstelle von WebJobs mit dem WebJobs SDK für andere Szenarien, in denen Sie Codeausschnitte für die Integration in Azure-Dienste oder Drittanbieterdienste ausführen möchten.

<a name="together"></a>

## <a name="flow-logic-apps-functions-and-webjobs-together"></a>Gemeinsame Nutzung von Flow, Logic Apps, Functions und WebJobs

Sie müssen sich nicht unbedingt nur für einen dieser Dienste entscheiden. Sie können untereinander ebenso gut wie mit externen Diensten integriert werden.

Ein Datenfluss kann eine Logik-App aufrufen. Eine Logik-App kann eine Funktion aufrufen, und eine Funktion kann eine Logik-App aufrufen. Informationen hierzu finden Sie beispielsweise unter [Erstellen einer Funktion, die in Azure Logic Apps integriert ist](functions-twitter-email.md).

Die Integration zwischen Flow, Logic Apps und Functions wird im Laufe der Zeit weiter verbessert. Sie können etwas in einem Dienst erstellen und in den anderen Diensten verwenden.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie, indem Sie Ihre erste Flow-, Logic Apps- oder Functions-App erstellen. Klicken Sie auf einen der folgenden Links:

* [Erste Schritte mit Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)

Weitere Informationen zu diesen Integrationsdiensten finden Sie auch unter den folgenden Links:

* [Leveraging Azure Functions &amp; Azure App Service for integration scenarios (Nutzung von Azure Functions und Azure App Service für Integrationsszenarien) von Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integrations Made Simple (Integrationen leicht gemacht) von Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps-Livewebcast](https://aka.ms/logicappslive)
* [Häufig gestellte Fragen zu Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)

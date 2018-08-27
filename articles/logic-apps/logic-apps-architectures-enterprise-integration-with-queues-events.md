---
title: Azure Integration Services – Referenzarchitektur für Unternehmensintegration
description: Beschreibt die Referenzarchitektur, die zeigt, wie Sie ein Muster einer Unternehmensintegration mit Logic Apps, API Management, Service Bus und Event Grid implementieren.
author: mattfarm
manager: jonfan
editor: ''
services: logic-apps api-management
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/15/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 9eef382ea264bcf9e59dcc408d14a59355b0369b
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42445676"
---
# <a name="reference-architecture-enterprise-integration-with-queues-and-events"></a>Referenzarchitektur: Unternehmensintegration mit Warteschlangen und Ereignissen

Die folgende Referenzarchitektur zeigt eine Reihe bewährter Methoden, die Sie für eine Integrationsanwendung einsetzen können, die Azure Integration Services verwendet. Die Architektur kann als Grundlage für viele verschiedene Anwendungsmuster dienen, die HTTP-APIs, Workflow und Orchestrierung erfordern.

![Architekturdiagramm – Unternehmensintegration mit Warteschlangen und Ereignissen](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

*Es gibt viele mögliche Anwendungen für die Integrationstechnologien. Diese reichen von einer einfachen Punkt-zu-Punkt-Anwendung bis hin zu einer vollständigen Enterprise Azure Service Bus-Anwendung. Die Architekturreihe beschreibt die wiederverwendbaren Komponenten, die für die Erstellung einer generischen Integrationsanwendung genutzt werden können. Architekten sollten überlegen, welche Komponenten sie für ihre Anwendung und Infrastruktur implementieren müssen.*

## <a name="architecture"></a>Architecture

Diese Architektur *basiert auf* der Architektur der [einfachen Unternehmensintegration](logic-apps-architectures-simple-enterprise-integration.md). Hier gelten auch die [Empfehlungen für die einfache Unternehmensarchitektur](logic-apps-architectures-simple-enterprise-integration.md#recommendations). Sie sind in den [Empfehlungen](#recommendations) in diesem Artikel der Kürze wegen weggelassen. 

Die Architektur besteht aus den folgenden Komponenten:

- **Ressourcengruppe**. Eine [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ist ein logischer Container für Azure-Ressourcen.
- **Azure API Management:** [API Management](https://docs.microsoft.com/azure/api-management/) ist eine vollständig verwaltete Plattform, die zum Veröffentlichen, Schützen und Transformieren von HTTP-APIs verwendet wird.
- **Azure API Management-Entwicklerportal**. Jede Instanz von Azure API Management kann auf das [Entwicklerportal](https://docs.microsoft.com/azure/api-management/api-management-customize-styles) zugreifen. Über das API Management-Entwicklerportal erhalten Sie Zugriff auf Dokumentation und Codebeispiele. Im Entwicklerportal können Sie zudem APIs testen.
- **Azur Logic Apps**. Bei [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) handelt es sich um eine serverlose Plattform, die für den Unternehmensworkflow und die -integration verwendet wird.
- **Connectors**. Logic Apps verwendet [Connectors](https://docs.microsoft.com/azure/connectors/apis-list), um eine Verbindung zu häufig verwendeten Diensten herzustellen. Logic Apps verfügt bereits über hunderte von verschiedenen Connectors, aber Sie können auch einen benutzerdefinierten Connector erstellen.
- **Azure Service Bus** [Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) bietet sicheres und zuverlässiges Messaging. Mit Messaging können Anwendungen entkoppelt und in andere nachrichtenbasierte Systeme integriert werden.
- **Azure Event Grid**. [Event Grid](https://docs.microsoft.com/azure/event-grid/overview) ist eine serverlose Plattform zur Veröffentlichung und Bereitstellung von Anwendungsereignissen.
- **IP-Adresse**. Der Azure API Management-Dienst weist eine feste öffentliche [IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) und einen Domänennamen auf. Der Standardname für die Domäne ist eine Unterdomäne von azure-api.net, z. B.: „contoso.azure-api.net“, doch es können auch [benutzerdefinierte Domänen](https://docs.microsoft.com/azure/api-management/configure-custom-domain) konfiguriert werden. Logic Apps und Service Bus haben auch eine öffentliche IP-Adresse. In dieser Architektur beschränken wir jedoch den Aufruf von Endpunkten von Logic Apps ausschließlich auf die IP-Adresse von API Management (aus Sicherheitsgründen). Aufrufe an Service Bus werden durch eine Shared Access Signature (SAS) gesichert.
- **Azure DNS:** [Azure DNS](https://docs.microsoft.com/azure/dns/) ist ein Hostingdienst für DNS-Domänen. Azure DNS bietet eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in API Management](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD):** Verwenden Sie [Azure AD](https://docs.microsoft.com/azure/active-directory/) oder einen anderen Identitätsanbieter für die Authentifizierung. Azure AD bietet die Authentifizierung für den Zugriff auf API-Endpunkte durch die Weitergabe eines [JSON Web Token für API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) zur Validierung. Azure AD ermöglicht einen sicheren Zugriff auf das API Management-Entwicklerportal (nur Standard und Premium-Tarife).

Beim Betrieb weist diese Architektur einige grundlegende Muster auf:

* Vorhandene Back-End-HTTP-APIs werden über das API Management-Entwicklerportal veröffentlicht. Im Portal können Entwickler (entweder unternehmensintern, extern oder beides) Aufrufe dieser APIs in Anwendungen integrieren.
* Verbund-APIs werden mithilfe von Logic Apps und der Orchestrierung von Aufrufen an Software-as-a-Service (SaaS)-Systeme, Azure-Dienste und alle APIs, die in API Management veröffentlicht werden, erstellt. [Logik-Apps](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) werden auch über das API Management-Entwicklerportal veröffentlicht.
- Anwendungen verwenden Azure AD, um [einen OAuth 2.0-Sicherheitstoken zu erwerben](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad), das für den Zugriff auf eine API erforderlich ist.
- API Management [überprüft das Sicherheitstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) und leitet die Anforderung dann an die Back-End-API oder die Logik-App weiter.
- Service Bus-Warteschlangen dienen der [Entkopplung](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) von Anwendungsaktivitäten zur [Glättung von Lastspitzen](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Nachrichten werden durch Logik-Apps, Anwendungen von Drittanbietern oder (nicht abgebildet) durch die Veröffentlichung der Warteschlange als HTTP-API über API Management hinzugefügt.
- Sobald Nachrichten zu einer Service Bus-Warteschlange hinzugefügt werden, wird ein Ereignis ausgelöst. Durch das Ereignis wird eine Logik-App ausgelöst. Diese verarbeitet dann die Nachricht.
- Andere Azure-Dienste (z.B. Azure Blob Storage und Azure Event Hubs) veröffentlichen ebenfalls Ereignisse in Event Grid. Diese Dienste lösen aus, dass Logic Apps das Ereignis empfängt und dann nachfolgende Aktionen ausführt.

## <a name="recommendations"></a>Empfehlungen

Ihre spezifischen Anforderungen können von der in diesem Artikel beschriebenen generischen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="service-bus-tier"></a>Service Bus-Tarif

Verwenden Sie den Service Bus Premium-Tarif. Der Premium-Tarif unterstützt Event Grid-Benachrichtigungen. Weitere Informationen finden Sie unter [Service Bus – Preise](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Event Grid-Preise

Event Grid verwendet ein serverloses Modell. Die Abrechnung erfolgt auf Basis der Anzahl der Vorgänge (Ereignisausführung). Weitere Informationen finden Sie unter [Event Grid-Preise](https://azure.microsoft.com/pricing/details/event-grid/). Für Event Grid gibt es derzeit keine Überlegungen zu den Tarifen.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Nutzen von PeekLock für Service Bus-Nachrichten

Beim Erstellen von Logik-Apps zur Nutzung von Service Bus-Nachrichten verwenden Sie [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) innerhalb der Logik-App, um auf eine Gruppe von Nachrichten zuzugreifen. Mit PeekLock kann die Logik-App dann Schritte ausführen, um die einzelnen Nachrichten zu überprüfen, bevor sie abgeschlossen oder verworfen werden. Dieser Ansatz schützt vor versehentlichem Nachrichtenverlust.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Überprüfung auf mehrere Objekte beim Auslösen eines Event Grid-Triggers

Wenn ein Event Grid-Ereignis ausgelöst wird, bedeutet das einfach, dass „mindestens eines dieser Dinge passiert ist“. Wenn Event Grid beispielsweise eine Logik-App bei einer Nachricht auslöst, die in einer Service Bus-Warteschlange erscheint, wird in der Logik-App immer davon ausgegangen, dass eine oder mehrere Nachrichten zur Verarbeitung verfügbar sind.

### <a name="region"></a>Region

Stellen Sie API Management, Logic Apps und Service Bus in der gleichen Region bereit, um die Netzwerklatenz zu verringern. Wählen Sie grundsätzlich die Ihren Benutzern am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf. Diese gibt an, wo die Metadaten der Bereitstellung gespeichert werden und von wo aus die Bereitstellungsvorlage ausgeführt wird. Platzieren Sie die Ressourcengruppe und ihre Ressourcen in der gleichen Region, um die Verfügbarkeit während der Bereitstellung zu verbessern.

## <a name="scalability"></a>Skalierbarkeit

Mit dem Service Bus-Premium-Tarif lässt sich die Anzahl von Nachrichteneinheiten horizontal skalieren, um eine höhere Skalierbarkeit zu erreichen. Premium-Tarif-Konfigurationen können über eine, zwei oder vier Nachrichteneinheiten verfügen. Weitere Informationen zum Skalieren von Service Bus finden Sie unter [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus Messaging](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Verfügbarkeit

Derzeit beträgt die Vereinbarung zum Servicelevel (SLA) für Azure API Management für die Tarife „Basic“, „Standard“ und „Premium“ 99,9 %. Vom Premium-Tarif abgedeckte Konfigurationen mit Bereitstellung von mindestens einer Einheit in zwei oder mehr Regionen haben eine SLA von 99,95%.

Derzeit beträgt die SLA für Azure Logic Apps 99,9 %.

### <a name="disaster-recovery"></a>Notfallwiederherstellung

Erwägen Sie die Implementierung einer georedundanten Notfallwiederherstellung im Service Bus-Premium-Tarif, um ein Failover im Falle eines schwerwiegenden Ausfalls sicherzustellen. Weitere Informationen finden Sie unter [Georedundante Notfallwiederherstellung in Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Separate Ressourcengruppen erleichtern das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.

Berücksichtigen Sie beim Zuweisen von Ressourcen zu Ressourcengruppen die folgenden Punkte:

- **Lebenszyklus**. Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.
- **Zugriff**. Sie können die [rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (RBAC) verwenden, um Zugriffsrichtlinien auf die in einer Gruppe enthaltenen Ressourcen anzuwenden.
- **Abrechnung**. Sie können die anfallenden Kosten für die Ressourcengruppe anzeigen.
- **Tarif für API Management**. Für Entwicklungs- und Testumgebungen wird die Verwendung des Developer-Tarifs empfohlen. Vor der Einführung in die Produktionsumgebung empfehlen wir, ein Replikat Ihrer Produktionsumgebung bereitzustellen, Tests durchzuführen und dann herunterzufahren, um die Kosten zu minimieren.

Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Bereitstellung

Es wird empfohlen, [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) zu verwenden, um API Management, Logic Apps, Event Grid und Service Bus bereitzustellen. Mithilfe von Vorlagen können Bereitstellungen über PowerShell oder die Azure-Befehlszeilenschnittstelle einfacher automatisiert werden.

Es wird empfohlen, API Management, die einzelnen Logik-Apps, Event Grid-Themen und Service Bus-Namespaces in eigene separate Resource Manager-Vorlagen zu integrieren. Wenn Sie unterschiedliche Vorlagen verwenden, können Sie die Ressourcen in Quellcodeverwaltungs-Systemen speichern. Diese Vorlagen können Sie dann gemeinsam oder einzeln im Rahmen eines Continuous Integration/Continous Deployment-Prozesses (CI/CD) einsetzen.

### <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Service Bus, wie auch API Management und Logic Apps, lassen sich mithilfe von Azure Monitor überwachen. Azure Monitor liefert Informationen basierend auf den Metriken, die für jeden Dienst konfiguriert sind. Azure Monitor ist standardmäßig aktiviert.

## <a name="security"></a>Sicherheit

Service Bus kann mit einer SAS gesichert werden. Mit der [SAS-Authentifizierung](../service-bus-messaging/service-bus-sas.md) können Sie einem Benutzer Zugriff auf Service Bus-Ressourcen mit spezifischen Rechten gewähren. Weitere Informationen finden Sie unter [Service Bus-Authentifizierung und -Autorisierung](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Wenn eine Service Bus-Warteschlange als HTTP-Endpunkt verfügbar gemacht werden muss (zum Bereitstellen neuer Nachrichten), sollte zum Sichern API Management verwendet werden, indem der Endpunkt verfügbar gemacht wird. Dieser kann dann mit Zertifikaten oder OAuth gesichert werden. Ein Endpunkt lässt sich am einfachsten sichern, indem eine Logik-App mit einem zwischengeschalteten Anforderung/Antwort-HTTP-Trigger verwendet wird.

Event Grid sichert die Ereignisübermittlung durch einen Validierungscode. Wenn Sie Logic Apps verwenden, um das Ereignis abzurufen, wird die Validierung automatisch ausgeführt. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [einfache Unternehmensintegration](logic-apps-architectures-simple-enterprise-integration.md).

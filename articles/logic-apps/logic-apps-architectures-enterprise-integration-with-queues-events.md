---
title: Muster der Unternehmensintegrationsarchitektur – Azure Integration Services
description: Dieser Architekturverweis zeigt, wie Sie ein Unternehmensintegrationsmuster in Azure Logic Apps, Azure API Management, Azure Service Bus und Azure Event Grid implementieren.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 8fbc84b4016659b6d0d6ce9ec47c05a0b241c3d9
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855373"
---
# <a name="enterprise-integration-architecture-with-queues-and-events"></a>Unternehmensintegrationsarchitektur mit Warteschlangen und Ereignissen

Dieser Artikel beschreibt eine Unternehmensintegrationsarchitektur mit bewährten Methoden, welche Sie bei Verwendung von Azure Integration Services auf eine Integrationsanwendung anwenden können. Sie können diese Architektur als Grundlage für zahlreiche verschiedene Anwendungsmuster nutzen, die HTTP-APIs, Workflow und Orchestrierung erfordern.

![Architekturdiagramm – Unternehmensintegration mit Warteschlangen und Ereignissen](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

Diese Reihe beschreibt die wiederverwendbaren Komponenten, die für die Erstellung einer generischen Integrationsanwendung genutzt werden können. Die vielfältigen Anwendungsmöglichkeiten von Integrationstechnologie reichen von einfachen Punkt-zu-Punkt-Apps bis hin zu vollständigen Azure Service Bus-Apps für Unternehmen. Überlegen Sie daher, welche Komponenten Sie für Ihre Apps und Infrastruktur implementieren müssen.

## <a name="architecture-components"></a>Komponenten der Architektur

Diese Architektur basiert auf der im Artikel [Referenzarchitektur: einfache Unternehmensintegration](../logic-apps/logic-apps-architectures-simple-enterprise-integration.md) beschriebenen Architektur. Diese [Empfehlungen](../logic-apps/logic-apps-architectures-simple-enterprise-integration.md#recommendations) für die Architektur gelten auch hier, aus Gründen der Übersichtlichkeit sind diese Empfehlungen in diesem Artikel jedoch nicht im Abschnitt [Empfehlungen](#recommendations) aufgeführt. Diese Unternehmensintegrationsarchitektur umfasst folgende Komponenten:

- **Ressourcengruppe**: Eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) ist ein logischer Container für Azure-Ressourcen.

- **Azure API Management**: Der [API Management](https://docs.microsoft.com/azure/api-management/)-Dienst ist eine vollständig verwaltete Plattform zur Veröffentlichung, Sicherung und Transformation von HTTP-APIs.

- **Azure API Management-Entwicklerportal**: Jede Instanz von Azure API Management bietet Zugriff auf das [Entwicklerportal](../api-management/api-management-customize-styles.md). Über dieses Portal erhalten Sie Zugriff auf Dokumentation und Codebeispiele. Im Entwicklerportal können Sie zudem APIs testen.

- **Azure Logic Apps**: Bei [Logic Apps](../logic-apps/logic-apps-overview.md) handelt es sich um eine serverlose Plattform, die Unternehmensworkflows und -integrationen unterstützt.

- **Connectors**: Logic Apps verwendet [Connectors](../connectors/apis-list.md), um eine Verbindung mit häufig verwendeten Diensten herzustellen. Logic Apps bietet hunderte von Connectors, aber Sie können auch einen benutzerdefinierten Connector erstellen.

- **Azure Service Bus**: [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) bietet sicheres und zuverlässiges Messaging. Mit Messaging können Sie Anwendungen entkoppeln und in andere nachrichtenbasierte Systeme integriert werden.

- **Azure Event Grid**: [Event Grid](../event-grid/overview.md) ist eine serverlose Plattform zur Veröffentlichung und Bereitstellung von Anwendungsereignissen.

- **IP-Adresse**: Der Azure API Management-Dienst weist eine feste öffentliche [IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und einen Domänennamen auf. Der Standardname für die Domäne ist eine Unterdomäne von „azure-api.net“, z.B. „contoso.azure-api.net“, doch es können auch [benutzerdefinierte Domänen](../api-management/configure-custom-domain.md) konfiguriert werden. Logic Apps und Service Bus haben auch eine öffentliche IP-Adresse. Aus Sicherheitsgründen beschränken wir in dieser Architektur den Aufruf von Logic Apps-Endpunkten jedoch ausschließlich auf die IP-Adresse von API Management. Aufrufe an Service Bus werden durch eine Shared Access Signature (SAS) gesichert.

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) ist ein Hostingdienst für DNS-Domänen. Azure DNS bietet eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in API Management](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: Verwenden Sie [Azure AD](https://docs.microsoft.com/azure/active-directory/) oder einen anderen Identitätsanbieter für die Authentifizierung. Azure AD bietet die Authentifizierung für den Zugriff auf API-Endpunkte durch die Weitergabe eines [JSON Web Token für API Management](../api-management/policies/authorize-request-based-on-jwt-claims.md) zur Validierung. Für Standard und Premium-Tarife ermöglicht Azure AD einen sicheren Zugriff auf das API Management-Entwicklerportal.

## <a name="patterns"></a>Muster 

Diese Architektur verwendet einige grundlegende Muster für den Betrieb:

* Vorhandene Back-End-HTTP-APIs werden über das API Management-Entwicklerportal veröffentlicht. Im Portal können Entwickler (entweder unternehmensintern, extern oder beides)  
Aufrufe dieser APIs in Anwendungen integrieren.

* Verbund-APIs werden mithilfe von Logic Apps erstellt, die Aufrufe an Software-as-a-Service (SaaS)-Systeme, Azure-Dienste und alle APIs, die in API Management veröffentlicht werden, orchestrieren. Logik-Apps werden auch [über das API Management-Entwicklerportal veröffentlicht](../api-management/import-logic-app-as-api.md).

* Anwendungen verwenden Azure AD zum [Abrufen eines OAuth 2.0-Sicherheitstokens](../api-management/api-management-howto-protect-backend-with-aad.md), das für den Zugriff auf eine API erforderlich ist.

* Azure API Management [überprüft das Sicherheitstoken](../api-management/api-management-howto-protect-backend-with-aad.md) und leitet die Anforderung dann an die Back-End-API oder die Logik-App weiter.

* Azure Service Bus-Warteschlangen dienen zur [Entkopplung](../service-bus-messaging/service-bus-messaging-overview.md) von Anwendungsaktivitäten und zur [Glättung von Lastspitzen](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Nachrichten werden durch Logik-Apps, Drittanbieter-Apps oder (nicht abgebildet) durch die Veröffentlichung der Warteschlange als HTTP-API über API Management hinzugefügt.

* Sobald Nachrichten zu einer Service Bus-Warteschlange hinzugefügt werden, wird ein Ereignis ausgelöst. Das Ereignis löst eine Logik-App aus, welche die Nachricht dann verarbeitet.

* Andere Azure-Dienste wie Azure Blob Storage und Azure Event Hubs veröffentlichen ebenfalls Ereignisse in Event Grid. Diese Dienste lösen aus, dass Logic Apps das Ereignis empfängt und dann nachfolgende Aktionen ausführt.

## <a name="recommendations"></a>Empfehlungen

Ihre individuellen Anforderungen können von der in diesem Artikel beschriebenen generischen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="service-bus-tier"></a>Service Bus-Tarif

Verwenden Sie den Service Bus-Premium-Tarif, der Event Grid-Benachrichtigungen unterstützt. Weitere Informationen finden Sie unter [Service Bus – Preise](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Event Grid-Preise

Event Grid verwendet ein serverloses Modell. Die Abrechnung erfolgt auf Basis der Anzahl der Vorgänge (Ereignisausführungen). Weitere Informationen finden Sie unter [Event Grid-Preise](https://azure.microsoft.com/pricing/details/event-grid/). Für Event Grid gibt es derzeit keine Überlegungen zu den Tarifen.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Nutzen von PeekLock für Service Bus-Nachrichten

Beim Erstellen einer Logik-Apps zur Nutzung von Service Bus-Nachrichten verwenden Sie [PeekLock](../service-bus-messaging/service-bus-messaging-overview.md#queues) in der Logik-App, um auf eine Gruppe von Nachrichten zuzugreifen. Mit PeekLock kann die Logik-App dann Schritte ausführen, um die einzelnen Nachrichten zu überprüfen, bevor sie abgeschlossen oder verworfen werden. Dieser Ansatz schützt vor versehentlichem Nachrichtenverlust.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Überprüfung auf mehrere Objekte beim Auslösen eines Event Grid-Triggers

Wenn ein Event Grid-Ereignis ausgelöst wird, bedeutet diese Aktion einfach, dass „mindestens eines dieser Dinge passiert ist“. Wenn Event Grid beispielsweise eine Logik-App bei einer Nachricht auslöst, die in einer Service Bus-Warteschlange erscheint, wird in der Logik-App immer davon ausgegangen, dass eine oder mehrere Nachrichten zur Verarbeitung verfügbar sind.

### <a name="region"></a>Region

Wählen Sie die gleiche Region für API Management, Logic Apps und Service Bus, um die Netzwerklatenz zu verringern. Wählen Sie grundsätzlich die Ihren Benutzern am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf. Diese Region gibt an, wo die Metadaten der Bereitstellung gespeichert werden und die Bereitstellungsvorlage ausgeführt wird. Platzieren Sie die Ressourcengruppe und die Ressourcen in der gleichen Region, um die Verfügbarkeit während der Bereitstellung zu verbessern.

## <a name="scalability"></a>Skalierbarkeit

Mit dem Service Bus-Premium-Tarif lässt sich die Anzahl von Nachrichteneinheiten horizontal skalieren, um eine höhere Skalierbarkeit zu erreichen. Premium-Tarif-Konfigurationen können über eine, zwei oder vier Nachrichteneinheiten verfügen. Weitere Informationen zum Skalieren von Service Bus finden Sie unter [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus Messaging](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Verfügbarkeit

* Die Vereinbarung zum Servicelevel (SLA) für Azure API Management beträgt für die Tarife „Basic“, „Standard“ und „Premium“ derzeit 99,9%. Für Konfigurationen mit Premium-Tarif und Bereitstellung von mindestens einer Einheit in zwei oder mehr Regionen gilt eine SLA von 99,95%.

* Die SLA für Azure Logic Apps beträgt derzeit 99,9%.

### <a name="disaster-recovery"></a>Notfallwiederherstellung

Erwägen Sie die Implementierung einer georedundanten Notfallwiederherstellung im Service Bus-Premium-Tarif, um bei einem schwerwiegenden Ausfall ein Failover sicherzustellen. Weitere Informationen finden Sie unter [Georedundante Notfallwiederherstellung in Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Separate Ressourcengruppen erleichtern das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.

Berücksichtigen Sie beim Zuweisen von Ressourcen zu Ressourcengruppen diese Faktoren:

* **Lebenszyklus**: Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.

* **Zugriff**: Zur Anwendung von Zugriffsrichtlinien auf die Ressourcen in einer Gruppe können Sie die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) verwenden.

* **Abrechnung**: Sie können die anfallenden Kosten für die Ressourcengruppe anzeigen.

* **Tarif für API Management**: Verwenden Sie den Tarif „Developer“ für Ihre Entwicklungs- und Testumgebungen. Zur Kostenminimierung während der Präproduktion stellen Sie ein Replikat Ihrer Produktionsumgebung bereit, führen die Tests aus und fahren das Replikat dann herunter.

Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Bereitstellung

* Zum Bereitstellen von API Management, Logic Apps, Event Grid und Service Bus verwenden Sie die [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md). Vorlagen vereinfachen die Automatisierung von Bereitstellung, wenn PowerShell oder die Azure CLI verwendet wird.

* Integrieren Sie API Management, einzelne Logik-Apps, Event Grid-Themen und Service Bus-Namespaces in eigene separate Resource Manager-Vorlagen. Durch die Verwendung unterschiedlicher Vorlagen können Sie die Ressourcen in Quellcodeverwaltungs-Systemen speichern. Diese Vorlagen können Sie dann gemeinsam oder einzeln im Rahmen eines Continuous Integration/Continous Deployment-Prozesses (CI/CD) einsetzen.

## <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Service Bus, wie auch API Management und Logic Apps, lassen sich mithilfe von Azure Monitor überwachen. Diese Dienstplattform ist standardmäßig aktiviert. Azure Monitor liefert Informationen basierend auf den Metriken, die für jeden Dienst konfiguriert sind. 

## <a name="security"></a>Sicherheit

Sichern Sie Service Bus mithilfe von Shared Access Signature (SAS). Mit der [SAS-Authentifizierung](../service-bus-messaging/service-bus-sas.md) können Sie beispielsweise einem Benutzer Zugriff auf Service Bus-Ressourcen mit spezifischen Rechten gewähren. Weitere Informationen finden Sie unter [Service Bus-Authentifizierung und -Autorisierung](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Wenn Sie eine Service Bus-Warteschlange als HTTP-Endpunkt verfügbar machen müssen, beispielsweise zum Veröffentlichen neuer Nachrichten, verwenden Sie API Management zum Sichern der Warteschlange, indem der Endpunkt verfügbar gemacht wird. Anschließend können Sie den Endpunkt mit Zertifikaten oder OAuth-Authentifizierung sichern. Ein Endpunkt lässt sich am einfachsten durch die Verwendung einer Logik-App mit einem Anforderung-/Antwort-HTTP-Trigger als Zwischenstufe sichern.

Der Event Grid-Dienst sichert die Ereignisübermittlung durch einen Validierungscode. Wenn Sie Logic Apps verwenden, um das Ereignis abzurufen, wird die Validierung automatisch ausgeführt. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [einfache Unternehmensintegration](logic-apps-architectures-simple-enterprise-integration.md).

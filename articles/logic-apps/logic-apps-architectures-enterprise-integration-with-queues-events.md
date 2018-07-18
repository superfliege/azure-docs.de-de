---
title: Azure Integration Services-Referenzarchitektur
description: Referenzarchitektur, die zeigt, wie Sie eine ein Muster einer Unternehmensintegration mit Logic Apps, API Management, Service Bus und Event Grid implementieren.
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
ms.openlocfilehash: db3350b3c70f6e6f35949e8423334061849b7b37
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232034"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Unternehmensintegration mit Warteschlangen und Ereignissen: Referenzarchitektur

## <a name="overview"></a>Übersicht

Diese Referenzarchitektur zeigt eine Reihe bewährter Methoden für eine Integrationsanwendung, die Azure Integration Services verwendet. Diese Architektur kann als Grundlage für viele verschiedene Anwendungsmuster dienen, die HTTP-APIs, Workflow und Orchestrierung erfordern.

*Es gibt viele mögliche Anwendungen der Integrationstechnologie, von einer einfachen Punkt-zu-Punkt-Anwendung bis hin zu einem vollständigen Unternehmensservicebus. Diese Architekturserie legt die wiederverwendbaren Komponenten für die Erstellung jeder beliebigen Integrationsanwendung fest – daher sollten Architekten die Komponenten berücksichtigen, die sie für ihre Anwendungen und Infrastruktur benötigen.*

![Architekturdiagramm – Unternehmensintegration mit Warteschlangen und Ereignissen](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Architecture

Diese Architektur basiert auf der in [Einfache Unternehmensintegration](logic-apps-architectures-simple-enterprise-integration.md) gezeigten Architektur. Sie enthält die folgenden Komponenten:

- Ressourcengruppe: Eine Ressourcengruppe ist ein logischer Container für Azure-Ressourcen.
- Azure API Management: Azure API Management ist eine vollständig verwaltete Plattform zur Veröffentlichung, Sicherung und Transformation von HTTP-APIs.
- Azure API Management-Entwicklerportal: Jede Instanz von Azure API Management verfügt über ein Entwicklerportal, das Zugriff auf Dokumentation, Codebeispiele und Testfunktionen für die API bietet.
- Azur Logic Apps: Bei Logic Apps handelt es sich um eine serverlose Plattform, die Unternehmensworkflow und -integration unterstützt.
- Connectors: Connectors werden von Logic Apps verwendet, um eine Verbindung zu häufig verwendeten Diensten herzustellen. Logic Apps verfügt bereits über hunderte von verschiedenen Connectors, aber es kann auch ein benutzerdefinierter Connector verwendet werden.
- Azure Service Bus: Service Bus bietet sicheres und zuverlässiges Messaging. Mit Messaging können Anwendungen voneinander entkoppelt und in andere nachrichtenbasierte Systeme integriert werden.
- Azure Event Grid: Event Grid ist eine serverlose Plattform zur Veröffentlichung und Bereitstellung von Anwendungsereignissen.
- IP-Adresse: Der Azure API Management-Dienst weist eine feste öffentliche IP-Adresse und einen Domänennamen auf. Der Domänenname ist eine Unterdomäne von azure-api.net, wie z.B. contoso. azure-api.net: Logic Apps und Service Bus haben auch eine öffentliche IP-Adresse; in dieser Architektur beschränken wir jedoch den Zugriff auf die Endpunkte von Logic Apps auf die IP-Adresse von API Management (aus Sicherheitsgründen). Aufrufe an Service Bus werden durch eine Shared Access Signature (SAS) gesichert.
- Azure DNS: Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter „Konfigurieren eines benutzerdefinierten Domänennamens in Azure API Management“.
- Azure Active Directory (Azure AD): Verwenden Sie Azure AD oder einen anderen Identitätsanbieter für die Authentifizierung. Azure AD bietet Authentifizierung für den Zugriff auf API-Endpunkte (durch Übergabe eines JSON Web Token für API Management zur Überprüfung) und kann den Zugriff auf das API Management-Entwicklerportal sichern (nur Standard- & Premium-Tarife).

Bei Vorgängen weist diese Architektur einige grundlegende Muster auf:

1. Bestehende Back-End-HTTP-APIs werden über das API Management-Entwicklerportal veröffentlicht, sodass Entwickler (organisationsinterne, externe oder beide) Aufrufe dieser APIs in Anwendungen integrieren können.
2. Verbund-APIs werden mithilfe von Logic Apps erstellt; sie orchestrieren Aufrufe an SAAS-Systeme, Azure-Dienste und alle APIs, die in API Management veröffentlicht werden. Die Logic Apps werden auch über das API Management-Entwicklerportal veröffentlicht.
3. Anwendungen erhalten ein OAuth 2.0-Sicherheitstoken, das für den Zugriff auf eine API mit Azure Active Directory erforderlich ist.
4. Azure API Management überprüft das Sicherheitstoken und leitet die Anforderung an die Back-End-API/Logik-App weiter.
5. Service Bus-Warteschlangen dienen der Entkopplung von Anwendungsaktivitäten und der Glättung von Lastspitzen. Nachrichten werden durch Logic Apps, Anwendungen von Drittanbietern oder (nicht abgebildet) durch die Veröffentlichung der Warteschlange als HTTP-API über API Management hinzugefügt.
6. Sobald Nachrichten zu einer Service Bus-Warteschlange hinzugefügt werden, wird ein Ereignis ausgelöst. Durch dieses Ereignis wird dann eine Logik-App ausgelöst, die diese Nachricht verarbeitet.
7. Auf ähnliche Weise veröffentlichen andere Azure-Dienste (z.B. Blob Storage, Event Hub) Ereignisse in Event Grid. Diese lösen aus, dass Logic Apps das Ereignis empfängt und nachfolgende Aktionen ausführt.

## <a name="recommendations"></a>Empfehlungen

Ihre Anforderungen können von der hier beschriebenen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="service-bus-tier"></a>Service Bus-Tarif

Verwenden Sie den Service Bus-Premium-Tarif, da dieser derzeit Benachrichtigungen über Event Grid unterstützt. (Die Unterstützung für alle Tarife wird erwartet.) Siehe [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Event Grid-Preise

Event Grid arbeitet mit einem serverlosen Modell – die Abrechnung erfolgt basierend auf der Anzahl von Vorgängen (Ereignisausführung). Weitere Informationen finden Sie unter [Event Grid-Preise](https://azure.microsoft.com/pricing/details/event-grid/). Für Event Grid gibt es derzeit keine Überlegungen zu den Tarifen.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Nutzen von Service Bus-Nachrichten mit PeekLock

Beim Erstellen von Logik-Apps zur Nutzung von Service Bus-Nachrichten verwenden Sie [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) innerhalb der Logik-App, um auf eine Gruppe von Nachrichten zuzugreifen. Die Logik-App kann dann Schritte ausführen, um die einzelnen Nachrichten zu überprüfen, bevor sie abgeschlossen oder verworfen werden. Dieser Ansatz schützt vor versehentlichem Nachrichtenverlust.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Überprüfung auf mehrere Objekte beim Auslösen eines Event Grid-Triggers

Wenn ein Event Grid-Trigger ausgelöst wird, was bedeutet, dass „mindestens eins dieser Ereignisse aufgetreten ist“. Wenn Event Grid beispielsweise eine Logik-App bei einer Nachricht auslöst, die in einer Service-Bus-Warteschlange erscheint, wird in der Logik-App immer davon ausgegangen, dass eine oder mehrere Nachrichten zur Verarbeitung verfügbar sind.

### <a name="region"></a>Region

Stellen Sie API Management, Logic Apps und Service Bus in der gleichen Region bereit, um die Netzwerklatenz zu verringern. Wählen Sie grundsätzlich die Ihren Benutzern am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf, die angibt, wo die Metadaten der Bereitstellung gespeichert werden und von wo aus die Bereitstellungsvorlage ausgeführt wird. Implementieren Sie die Ressourcengruppe und ihre Ressourcen in der gleichen Region. Dies kann die Verfügbarkeit während der Bereitstellung verbessern.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Mit dem Premium-Tarif von Azure Service Bus lässt sich die Anzahl von Nachrichteneinheiten horizontal skalieren, um eine höhere Skalierbarkeit zu erreichen. Beim Premium-Tarif können 1, 2 oder 4 Nachrichteneinheiten vorhanden sein. Weitere Informationen zum Skalieren von Azure Service Bus finden Sie unter [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Messaging](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Zum Zeitpunkt der Erstellung beträgt die Vereinbarung zum Servicelevel (SLA) für Azure API Management für die Tarife „Basic“, „Standard“ und „Premium“ 99,9%. Vom Premium-Tarif abgedeckte Konfigurationen mit Bereitstellung von mindestens einer Einheit in zwei oder mehr Regionen haben eine SLA von 99,95%.

Zum Zeitpunkt der Erstellung liegt die SLA für Azure Logic Apps bei 99,9%.

### <a name="disaster-recovery"></a>Notfallwiederherstellung

Erwägen Sie die Implementierung einer georedundanten Notfallwiederherstellung im Service Bus-Premium-Tarif, um ein Failover im Falle eines schwerwiegenden Ausfalls sicherzustellen. Lesen Sie mehr zum Thema [Georedundante Notfallwiederherstellung in Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Dies erleichtert das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.
Berücksichtigen Sie beim Zuweisen von Ressourcengruppen die folgenden Punkte:

- Lebenszyklus. Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.
- Zugriff. Sie können die [rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (RBAC) verwenden, um Zugriffsrichtlinien auf die in einer Gruppe enthaltenen Ressourcen anzuwenden.
- Abrechnung. Sie können die angefallenen Kosten für die Ressourcengruppe anzeigen.
- Tarif für API Management – für Entwicklungs- und Testumgebungen wird die Verwendung des Developer-Tarifs empfohlen. Vor der Einführung in die Produktionsumgebung empfehlen wir, ein Replikat Ihrer Produktionsumgebung bereitzustellen, Tests durchzuführen und dann herunterzufahren, um die Kosten zu minimieren.

Weitere Informationen finden Sie in der [Ressourcengruppenübersicht](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Bereitstellung

Es wird empfohlen, [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) zu verwenden, um Azure API Management, Logic Apps, Event Grid und Service Bus bereitzustellen. Mithilfe von Vorlagen können Bereitstellungen über PowerShell oder die Azure-Befehlszeilenschnittstelle (CLI) einfacher automatisiert werden.

Es wird empfohlen, Azure API Management, die einzelnen Logik-Apps, Event Grid-Themen und Service Bus-Namespaces in eigene separate Resource Manager-Vorlagen zu integrieren. Dadurch können sie in Systemen für die Quellcodeverwaltung gespeichert werden. Diese Vorlagen können dann gemeinsam oder einzeln im Rahmen eines Continuous Integration/Continous Deployment-Prozesses (CI/CD) eingesetzt werden.

### <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Service Bus, wie auch API Management und Logic Apps, lassen sich mithilfe von Azure Monitor überwachen. Azure Monitor ist standardmäßig aktiviert und liefert Informationen basierend auf den verschiedenen Metriken, die für jeden Dienst konfiguriert sind.

## <a name="security-considerations"></a>Sicherheitshinweise

Sichern Sie Service Bus mithilfe einer Shared Access Signature (SAS). Mit der [SAS-Authentifizierung](../service-bus-messaging/service-bus-sas.md) können Sie einem Benutzer Zugriff auf Service Bus-Ressourcen mit spezifischen Rechten gewähren. Lesen Sie mehr zum Thema [Service Bus-Authentifizierung und -Autorisierung](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Wenn darüber hinaus eine Service Bus-Warteschlange als HTTP-Endpunkt verfügbar gemacht werden muss (um das Bereitstellen neuer Nachrichten zu ermöglichen), sollte API Management verwendet werden, um sie zu sichern, indem der Endpunkt verfügbar gemacht wird. Diese kann dann mit Zertifikaten oder OAuth gesichert werden. Dies lässt sich am einfachsten erreichen, indem eine Logik-App mit einem zwischengeschalteten Anforderung/Antwort-HTTP-Trigger verwendet wird.

Event Grid sichert die Ereignisübermittlung durch einen Validierungscode. Wenn Sie Logic Apps verwenden, um das Ereignis abzurufen, wird dies automatisch ausgeführt. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Nächste Schritte

* [Einfache Unternehmensintegration](logic-apps-architectures-simple-enterprise-integration.md)

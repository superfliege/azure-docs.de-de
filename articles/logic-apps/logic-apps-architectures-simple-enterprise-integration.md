---
title: Azure Integration Services – Referenzarchitektur für einfache Unternehmensintegration
description: Beschreibt die Referenzarchitektur, die zeigt, wie Sie ein Muster einer einfachen Unternehmensintegration mit Azure Logic Apps und Azure API Management implementieren.
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
ms.openlocfilehash: f73a9e59c0add664128b506172182afe566ca670
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444509"
---
# <a name="reference-architecture-simple-enterprise-integration"></a>Referenzarchitektur: einfache Unternehmensintegration

Die folgende Referenzarchitektur zeigt eine Reihe bewährter Methoden, die Sie für eine Integrationsanwendung einsetzen können, die Azure Integration Services verwendet. Die Architektur kann als Grundlage für viele verschiedene Anwendungsmuster dienen, die HTTP-APIs, Workflow und Orchestrierung erfordern.

![Architekturdiagramm – einfache Unternehmensintegration](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

*Es gibt viele mögliche Anwendungen für die Integrationstechnologien. Diese reichen von einer einfachen Punkt-zu-Punkt-Anwendung bis hin zu einer vollständigen Enterprise Azure Service Bus-Anwendung. Die Architekturreihe beschreibt die wiederverwendbaren Komponenten, die für die Erstellung einer generischen Integrationsanwendung genutzt werden können. Architekten sollten überlegen, welche Komponenten sie für ihre Anwendung und Infrastruktur implementieren müssen.*

## <a name="architecture"></a>Architecture

Die Architektur besteht aus den folgenden Komponenten:

- **Ressourcengruppe**. Eine [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ist ein logischer Container für Azure-Ressourcen.
- **Azure API Management:** [API Management](https://docs.microsoft.com/azure/api-management/) ist eine vollständig verwaltete Plattform, die zum Veröffentlichen, Schützen und Transformieren von HTTP-APIs verwendet wird.
- **Azure API Management-Entwicklerportal**. Jede Instanz von Azure API Management kann auf das [Entwicklerportal](https://docs.microsoft.com/azure/api-management/api-management-customize-styles) zugreifen. Über das API Management-Entwicklerportal erhalten Sie Zugriff auf Dokumentation und Codebeispiele. Im Entwicklerportal können Sie zudem APIs testen.
- **Azur Logic Apps**. Bei [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) handelt es sich um eine serverlose Plattform, die für den Unternehmensworkflow und die -integration verwendet wird.
- **Connectors**. Logic Apps verwendet [Connectors](https://docs.microsoft.com/azure/connectors/apis-list), um eine Verbindung zu häufig verwendeten Diensten herzustellen. Logic Apps verfügt bereits über hunderte von verschiedenen Connectors, aber Sie können auch einen benutzerdefinierten Connector erstellen.
- **IP-Adresse**. Der Azure API Management-Dienst weist eine feste öffentliche [IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) und einen Domänennamen auf. Der Standardname für die Domäne ist eine Unterdomäne von azure-api.net, z. B.: „contoso.azure-api.net“, doch es können auch [benutzerdefinierte Domänen](https://docs.microsoft.com/azure/api-management/configure-custom-domain) konfiguriert werden. Logic Apps und Service Bus haben auch eine öffentliche IP-Adresse. In dieser Architektur beschränken wir jedoch den Aufruf von Endpunkten von Logic Apps ausschließlich auf die IP-Adresse von API Management (aus Sicherheitsgründen). Aufrufe an Service Bus werden durch eine Shared Access Signature (SAS) gesichert.
- **Azure DNS:** [Azure DNS](https://docs.microsoft.com/azure/dns/) ist ein Hostingdienst für DNS-Domänen. Azure DNS bietet eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in API Management](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD):** Verwenden Sie [Azure AD](https://docs.microsoft.com/azure/active-directory/) oder einen anderen Identitätsanbieter für die Authentifizierung. Azure AD bietet die Authentifizierung für den Zugriff auf API-Endpunkte durch die Weitergabe eines [JSON Web Token für API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) zur Validierung. Azure AD ermöglicht einen sicheren Zugriff auf das API Management-Entwicklerportal (nur Standard und Premium-Tarife).

Beim Betrieb weist diese Architektur einige grundlegende Muster auf:

- Zusammengesetzte APIs werden mithilfe von Logik-Apps erstellt. Sie orchestrieren Aufrufe von Software-as-a-Service (SaaS)-Systemen, von Azure-Diensten und von allen APIs, die im API Management veröffentlicht werden. [Logik-Apps](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) werden auch über das API Management-Entwicklerportal veröffentlicht.
- Anwendungen verwenden Azure AD, um [einen OAuth 2.0-Sicherheitstoken zu erwerben](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad), das für den Zugriff auf eine API erforderlich ist.
- Azure API Management [überprüft das Sicherheitstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) und leitet die Anforderung dann an die Back-End-API oder die Logik-App weiter.

## <a name="recommendations"></a>Empfehlungen

Ihre spezifischen Anforderungen können von der in diesem Artikel beschriebenen generischen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="azure-api-management-tier"></a>Azure API Management-Tarif

Verwenden Sie die Tarife „Basic“, „Standard“ oder „Premium“ für API Management. Die Tarife bieten eine Vereinbarung zum Servicelevel für die Produktionsumgebung (SLA) und eine horizontale Skalierung innerhalb der Azure-Region (Anzahl der Einheiten variiert je nach Tarif). Der Tarif „Premium“ unterstützt zudem die horizontale Skalierung über mehrere Azure-Regionen hinweg. Richten Sie den von Ihnen gewählten Tarif an Ihrem erforderlichen Durchsatz und Funktionsumfang aus. Weitere Informationen finden Sie unter [API Management-Preise](https://azure.microsoft.com/pricing/details/api-management/).

Es entstehen Kosten für alle API Management-Instanzen, sobald sie ausgeführt werden. Wenn Sie eine Skalierung durchgeführt haben und diese Leistung nicht ständig benötigen, sollten Sie die Vorteile der stündlichen Abrechnung von API Management nutzen und zentral herunterskalieren.

### <a name="logic-apps-pricing"></a>Logic Apps-Preise

Logic Apps verwendet ein [serverloses](logic-apps-serverless-overview.md) Modell. Die Abrechnung erfolgt basierend auf Aktionen und Connectorausführung. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/). Für Logic Apps gibt es derzeit keine Überlegungen zu den Tarifen.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps für asynchrone API-Aufrufe

Logic Apps funktioniert am besten in Szenarien, für die keine niedrige Latenz erforderlich ist. Beispielsweise funktionieren sie optimal bei asynchronen API-Aufrufen oder bei Aufrufen mittlerer Dauer. Wenn eine geringe Latenz erforderlich ist (z.B. ein Aufruf, der eine Benutzeroberfläche blockiert), wird die Implementierung dieser API oder dieses Vorgangs mit einer anderen Technologie empfohlen. Verwenden Sie z.B. Azure Functions oder eine über Azure App Service bereitgestellte Web-API. Wir empfehlen weiterhin, dass diese API über API Management für API-Consumer verfügbar gemacht wird.

### <a name="region"></a>Region

Stellen Sie API Management und Logic Apps in der gleichen Region bereit, um die Netzwerklatenz zu verringern. Wählen Sie grundsätzlich die Ihren Benutzern am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf. Diese gibt an, wo die Metadaten der Bereitstellung gespeichert werden und von wo aus die Bereitstellungsvorlage ausgeführt wird. Platzieren Sie die Ressourcengruppe und ihre Ressourcen in der gleichen Region, um die Verfügbarkeit während der Bereitstellung zu verbessern.

## <a name="scalability"></a>Skalierbarkeit

API Management-Administratoren sollten gegebenenfalls [Cachingrichtlinien](../api-management/api-management-howto-cache.md) hinzufügen, um die Skalierbarkeit des Dienstes zu erhöhen. Mit Caching können Sie zudem die Last für ihre Back-End-Dienste verringern.

Die Tarife „Basic“, „Standard“ und „Premium“ von Azure API Management können in einer Azure-Region horizontal hochskaliert werden, um mehr Kapazität zu bieten. Administratoren können die Option **Kapazitätsmetrik** im **Metrikmenü** verwenden, um die Nutzung ihres Diensts zu analysieren und eine entsprechende Skalierung vorzunehmen.

Empfehlungen zur Skalierung eines API Management-Diensts:

- Beim Skalieren müssen Datenverkehrsmuster berücksichtigt werden. Kunden mit veränderlichen Datenverkehrsmustern haben einen größeren Bedarf an höherer Kapazität.
- Eine konstante Kapazität von mehr als 66 % kann darauf hinweisen, dass ein Hochskalieren notwendig ist.
- Eine konstante Kapazität unter 20 % kann darauf hinweisen, dass ein Herunterskalieren notwendig ist.
- Es ist immer empfehlenswert, Ihren API Management-Dienst mit einer repräsentativen Last zu testen, bevor Sie sie in der Produktionsumgebung einsetzen.

Dienste des Premium-Tarifs können über mehrere Azure-Regionen hinweg horizontal hochskaliert werden. Kunden, die Dienste über mehrere Azure-Regionen skalieren, erhalten ein höheres SLA (99,95 % gegenüber 99,9 %) und können Dienste in der Nähe von Benutzern in mehreren Regionen bereitstellen.

Das serverlose Modell von Logic Apps bedeutet, dass Administratoren nicht für die Skalierbarkeit der Dienste planen müssen. Der Dienst wird automatisch entsprechend den Anforderungen skaliert.

## <a name="availability"></a>Verfügbarkeit

Die SLA für Azure API Management beträgt derzeit 99,9 % für die Tarife „Basic“, „Standard“ und „Premium“. Vom Premium-Tarif abgedeckte Konfigurationen mit Bereitstellung von mindestens einer Einheit in zwei oder mehr Regionen haben eine SLA von 99,95%.

Derzeit beträgt die SLA für Azure Logic Apps 99,9 %.

### <a name="backups"></a>Backups

Ihre Azure API Management-Konfiguration muss [regelmäßig gesichert werden](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (basierend auf regelmäßigen Änderungen). Sicherungsdateien sollten an einem anderen Speicherort oder in einer anderen Azure-Region als der Dienst gespeichert werden. Kunden können dann eine von zwei Optionen für ihre Notfallwiederherstellungsstrategie wählen:

- Bei einer Notfallwiederherstellung wird eine neue API Management-Instanz bereitgestellt, die Sicherung wird auf dieser neuen Instanz wiederhergestellt, und die DNS-Datensätze werden umgeleitet.
- Kunden behalten eine passive Kopie ihres Diensts in einer anderen Azure-Region (hierfür fallen zusätzliche Kosten an). Darin werden Sicherungen regelmäßig wiederhergestellt. Im Falle einer Notfallwiederherstellung müssen nur die DNS-Datensätze umgeleitet werden, um den Dienst wiederherzustellen.

Da Logik-Apps sehr schnell neu erstellt werden können und serverlos sind, werden die Komponenten durch das Speichern einer Kopie der zugehörigen Azure Resource Manager-Vorlage gesichert. Vorlagen können in der Quellcodeverwaltung gespeichert und in den Continuous Integration/Continuous Deployment-Prozess (CI/CD) eines Kunden integriert werden.

Wenn eine Logik-App, die über API Management veröffentlicht wurde, in ein anderes Rechenzentrum verschoben wird, aktualisieren Sie den Speicherort der App. Verwenden Sie dafür ein einfaches PowerShell-Skript, das die **Back-End**-Eigenschaft der API aktualisiert.

## <a name="manageability"></a>Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Separate Ressourcengruppen erleichtern das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.

Berücksichtigen Sie beim Zuweisen von Ressourcen zu Ressourcengruppen die folgenden Punkte:

- **Lebenszyklus**. Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.
- **Zugriff**. Sie können die [rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (RBAC) verwenden, um Zugriffsrichtlinien auf die in einer Gruppe enthaltenen Ressourcen anzuwenden.
- **Abrechnung**. Sie können die anfallenden Kosten für die Ressourcengruppe anzeigen.
- **Tarif für API Management**. Für Entwicklungs- und Testumgebungen wird die Verwendung des Developer-Tarifs empfohlen. Vor der Einführung in die Produktionsumgebung empfehlen wir, ein Replikat Ihrer Produktionsumgebung bereitzustellen, Tests durchzuführen und dann herunterzufahren, um die Kosten zu minimieren.

Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Bereitstellung

Es wird empfohlen, [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) zu verwenden, um API Management und Logic Apps bereitzustellen. Mithilfe von Vorlagen können Bereitstellungen über PowerShell oder die Azure-Befehlszeilenschnittstelle einfacher automatisiert werden.

Es wird empfohlen, Azure API Management und die einzelnen Logik-Apps in eigene separate Resource Manager-Vorlagen zu integrieren. Wenn Sie unterschiedliche Vorlagen verwenden, können Sie die Ressourcen in Quellcodeverwaltungs-Systemen speichern. Sie können die Ressourcen auch zusammen oder einzeln als Teil eines CI/CD-Bereitstellungsprozess bereitstellen.

### <a name="versions"></a>Versionen

Jedes Mal, wenn Sie eine Konfigurationsänderung an einer Logik-App vornehmen (oder ein Update über eine Resource Manager-Vorlage bereitstellen), wird eine Kopie dieser Version zu Ihrer Verfügung aufbewahrt (alle Versionen, die einen Ausführungsverlauf aufweisen, werden gespeichert). Sie können diese Versionen verwenden, um Änderungen im Verlauf zu verfolgen, und eine Version als aktuelle Konfiguration der Logik-App höherstufen. So können Sie eine Logik-App beispielsweise erfolgreich zurücksetzen.

API Management bietet zwei unterschiedliche (aber sich ergänzende) [Versionierungskonzepte](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Versionen, durch die API-Consumer eine Auswahl von APIs erhalten, die sie je nach Bedarf nutzen können (z.B. v1, v2 oder Betaversion, Version für die Produktionsumgebung).
- Revisionen, die es API-Administratoren ermöglichen, Änderungen an einer API sicher vorzunehmen und diese Änderungen mit optionalen Kommentaren an Benutzer weiterzugeben.

Bei einer Bereitstellung sollten API Management-Revisionen als eine Möglichkeit betrachtet werden, um Änderungen sicher vorzunehmen, ein Änderungsprotokoll zu führen und die API-Consumer auf diese Änderungen hinzuweisen. Eine Revision kann in einer Entwicklungsumgebung erstellt und mithilfe von Resource Manager-Vorlagen in anderen Umgebungen bereitgestellt werden.

Obwohl Sie Revisionen verwenden können, um eine API zu testen, bevor Sie sie „aktuell“ und für Benutzer zugänglich machen, empfehlen wir, diesen Mechanismus nicht für Last- oder Integrationstests zu verwenden. Verwenden Sie stattdessen separate Test- oder Präproduktionsumgebungen.

### <a name="configuration"></a>Konfiguration

Fügen Sie niemals Kennwörter, Zugriffsschlüssel oder Verbindungszeichenfolgen in die Quellcodeverwaltung ein. Wenn diese Werte benötigt werden, verwenden Sie eine geeignete Methode, um diese bereitzustellen und zu sichern. 

In Logic Apps sollten alle vertraulichen Werte, die innerhalb der Logic App benötigt werden (und nicht in Form einer Verbindung erstellt werden können), in Azure Key Vault gespeichert und aus einer Resource Manager-Vorlage referenziert werden. Zudem wird die Verwendung von Vorlageparametern für die Bereitstellung und von Parameterdateien für jede Umgebung empfohlen. Weitere Informationen finden Sie unter [Sichern von Parametern und Eingaben innerhalb eines Workflows](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

In API Management werden Geheimnisse über Objekte verwaltet, die als *Benannte Werte* oder *Eigenschaften* bezeichnet werden. In den Objekten werden Werte sicher gespeichert, auf die in API Management-Richtlinien zugegriffen werden kann. Weitere Informationen finden Sie unter [Verwalten von Geheimnissen in API Management](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Sowohl [API Management](../api-management/api-management-howto-use-azure-monitor.md) als auch [Logic Apps](logic-apps-monitor-your-logic-apps.md) unterstützen die betriebliche Überwachung durch [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor liefert Informationen basierend auf den Metriken, die für jeden Dienst konfiguriert sind. Azure Monitor ist standardmäßig aktiviert.

Diese Optionen sind auch für jeden Dienst verfügbar:

- Logic Apps-Protokolle können zur eingehenderen Analyse und zum Dashboarding an [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) gesendet werden.
- API Management unterstützt die Konfiguration von Azure Application Insights für die Überwachung von DevOps.
- API Management unterstützt die [Power BI-Lösungsvorlage für benutzerdefinierte API-Analysen](http://aka.ms/apimpbi). Kunden können die Lösungsvorlage verwenden, um eine eigene benutzerdefinierte Analyselösung zu erstellen. Berichte sind in Power BI für Geschäftskunden verfügbar.

## <a name="security"></a>Sicherheit

Dieser Abschnitt enthält Sicherheitshinweise, die für die in diesem Artikel beschriebenen Azure-Dienste spezifisch sind und in dieser Architektur (wie beschrieben) bereitgestellt werden. Es handelt sich nicht um eine vollständige Liste der bewährten Sicherheitsmethoden.

- Verwenden Sie die rollenbasierte Zugriffssteuerung (RBAC), um angemessene Zugriffsebenen für Benutzer sicherzustellen.
- Sichern Sie öffentliche API-Endpunkte in API Management mit OAuth/OpenID Connect. Um öffentliche API-Endpunkte zu sichern, konfigurieren Sie einen Identitätsanbieter und fügen Sie eine JSON Web Token (JWT)-Validierungsrichtlinie hinzu.
- Stellen Sie eine Verbindung zu Back-End-Diensten aus API Management her, indem Sie gemeinsame Zertifikate verwenden.
- Sichern Sie Logik-Apps, die auf HTTP-Triggern basieren, indem Sie eine Whitelist von IP-Adressen erstellen, die auf die IP-Adresse von API Management verweist. Eine IP-Adresse auf der Whitelist verhindert, dass die Logik-App aus dem öffentlichen Internet aufgerufen wird, ohne vorher API Management zu durchlaufen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Unternehmensintegration mit Warteschlangen und Ereignissen](logic-apps-architectures-enterprise-integration-with-queues-events.md).

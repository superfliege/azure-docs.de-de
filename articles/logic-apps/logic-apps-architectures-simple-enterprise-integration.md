---
title: Einfaches Muster einer Unternehmensintegrationsarchitektur – Azure Integration Services
description: Dieser Architekturverweis zeigt, wie Sie ein einfaches Unternehmensintegrationsmuster mithilfe von Azure Logic Apps und Azure API Management implementieren.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 5ed6fa9f514bae3ea651edba6702714e2680091f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955944"
---
# <a name="simple-enterprise-integration-architecture"></a>Einfache Unternehmensintegrationsarchitektur

Dieser Artikel beschreibt eine Unternehmensintegrationsarchitektur mit bewährten Methoden, welche Sie bei Verwendung von Azure Integration Services auf eine Integrationsanwendung anwenden können. Sie können diese Architektur als Grundlage für zahlreiche verschiedene Anwendungsmuster nutzen, die HTTP-APIs, Workflow und Orchestrierung erfordern.

![Architekturdiagramm – einfache Unternehmensintegration](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

Diese Reihe beschreibt die wiederverwendbaren Komponenten, die für die Erstellung einer generischen Integrationsanwendung genutzt werden können. Die vielfältigen Anwendungsmöglichkeiten von Integrationstechnologie reichen von einfachen Punkt-zu-Punkt-Apps bis hin zu vollständigen Azure Service Bus-Apps für Unternehmen. Überlegen Sie daher, welche Komponenten Sie für Ihre Apps und Infrastruktur implementieren müssen.

## <a name="architecture-components"></a>Komponenten der Architektur

Diese Unternehmensintegrationsarchitektur umfasst folgende Komponenten:

- **Ressourcengruppe**: Eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) ist ein logischer Container für Azure-Ressourcen.

- **Azure API Management**: Der [API Management](https://docs.microsoft.com/azure/api-management/)-Dienst ist eine vollständig verwaltete Plattform zur Veröffentlichung, Sicherung und Transformation von HTTP-APIs.

- **Azure API Management-Entwicklerportal**: Jede Instanz von Azure API Management bietet Zugriff auf das [Entwicklerportal](../api-management/api-management-customize-styles.md). Über dieses Portal erhalten Sie Zugriff auf Dokumentation und Codebeispiele. Im Entwicklerportal können Sie zudem APIs testen.

- **Azure Logic Apps**: Bei [Logic Apps](../logic-apps/logic-apps-overview.md) handelt es sich um eine serverlose Plattform, die Unternehmensworkflows und -integrationen unterstützt.

- **Connectors**: Logic Apps verwendet [Connectors](../connectors/apis-list.md), um eine Verbindung mit häufig verwendeten Diensten herzustellen. Logic Apps bietet hunderte von Connectors, aber Sie können auch einen benutzerdefinierten Connector erstellen.

- **IP-Adresse**: Der Azure API Management-Dienst weist eine feste öffentliche [IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und einen Domänennamen auf. Der Standardname für die Domäne ist eine Unterdomäne von „azure-api.net“, z.B. „contoso.azure-api.net“, doch es können auch [benutzerdefinierte Domänen](../api-management/configure-custom-domain.md) konfiguriert werden. Logic Apps und Service Bus haben auch eine öffentliche IP-Adresse. Aus Sicherheitsgründen beschränken wir in dieser Architektur den Aufruf von Logic Apps-Endpunkten jedoch ausschließlich auf die IP-Adresse von API Management. Aufrufe an Service Bus werden durch eine Shared Access Signature (SAS) gesichert.

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) ist ein Hostingdienst für DNS-Domänen. Azure DNS bietet eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in API Management](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: Verwenden Sie [Azure AD](https://docs.microsoft.com/azure/active-directory/) oder einen anderen Identitätsanbieter für die Authentifizierung. Azure AD bietet die Authentifizierung für den Zugriff auf API-Endpunkte durch die Weitergabe eines [JSON Web Token für API Management](../api-management/policies/authorize-request-based-on-jwt-claims.md) zur Validierung. Für Standard und Premium-Tarife ermöglicht Azure AD einen sicheren Zugriff auf das API Management-Entwicklerportal.

## <a name="patterns"></a>Muster 

Diese Architektur verwendet einige grundlegende Muster für den Betrieb:

* Verbund-APIs werden mithilfe von Logik-Apps erstellt, die Aufrufe an Software-as-a-Service (SaaS)-Systeme, Azure-Dienste und alle APIs, die in API Management veröffentlicht werden, orchestrieren. Logik-Apps werden auch [über das API Management-Entwicklerportal veröffentlicht](../api-management/import-logic-app-as-api.md).

* Anwendungen verwenden Azure AD zum [Abrufen eines OAuth 2.0-Sicherheitstokens](../api-management/api-management-howto-protect-backend-with-aad.md), das für den Zugriff auf eine API erforderlich ist.

* Azure API Management [überprüft das Sicherheitstoken](../api-management/api-management-howto-protect-backend-with-aad.md) und leitet die Anforderung dann an die Back-End-API oder die Logik-App weiter.

## <a name="recommendations"></a>Empfehlungen

Ihre individuellen Anforderungen können von der in diesem Artikel beschriebenen generischen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="azure-api-management-tier"></a>Azure API Management-Tarif

Verwenden Sie die Tarife „Basic“, „Standard“ oder „Premium“ für API Management. Diese Tarife bieten eine Vereinbarung zum Servicelevel (SLA) für die Produktionsumgebung und unterstützen eine horizontale Skalierung innerhalb der Azure-Region. Die Anzahl der Einheiten variiert je nach Tarif. Der Tarif „Premium“ unterstützt zudem die horizontale Skalierung über mehrere Azure-Regionen hinweg. Wählen Sie Ihren Tarif basierend auf Ihrem Funktionsumfang und dem erforderlichen Durchsatz. Weitere Informationen finden Sie unter [API Management-Preise](https://azure.microsoft.com/pricing/details/api-management/).

Es entstehen Kosten für alle API Management-Instanzen, sobald sie ausgeführt werden. Wenn Sie eine Skalierung durchgeführt haben und diese Leistung nicht ständig benötigen, sollten Sie die Vorteile der stündlichen Abrechnung von API Management nutzen und zentral herunterskalieren.

### <a name="logic-apps-pricing"></a>Logic Apps-Preise

Logic Apps verwendet ein [serverloses](../logic-apps/logic-apps-serverless-overview.md) Modell. Die Abrechnung erfolgt basierend auf Aktionen und Connectorausführung. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/). Für Logic Apps gibt es derzeit keine Überlegungen zu den Tarifen.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps für asynchrone API-Aufrufe

Logic Apps funktioniert am besten in Szenarien, für die keine niedrige Latenz erforderlich ist. Beispielsweise funktioniert Logic Apps optimal bei asynchronen API-Aufrufen oder bei Aufrufen mittlerer Dauer. Wenn eine geringe Latenz erforderlich ist (z.B. ein Aufruf, der eine Benutzeroberfläche blockiert), implementieren Sie Ihre API bzw. Ihren Vorgang mit einer anderen Technologie. Verwenden Sie z.B. Azure Functions oder eine über Azure App Service bereitgestellte Web-API. Verwenden Sie API Management, um die API für Ihre API-Consumer verfügbar zu machen.

### <a name="region"></a>Region

Wählen Sie die gleiche Region für API Management, Logic Apps und Service Bus, um die Netzwerklatenz zu verringern. Wählen Sie grundsätzlich die Ihren Benutzern am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf. Diese Region gibt an, wo die Metadaten der Bereitstellung gespeichert werden und die Bereitstellungsvorlage ausgeführt wird. Platzieren Sie die Ressourcengruppe und die Ressourcen in der gleichen Region, um die Verfügbarkeit während der Bereitstellung zu verbessern.

## <a name="scalability"></a>Skalierbarkeit

Fügen Sie gegebenenfalls [Cachingrichtlinien](../api-management/api-management-howto-cache.md) hinzu, um die Skalierbarkeit beim Verwalten eines API Management-Diensts zu erhöhen. Mit Caching können Sie zudem die Last für ihre Back-End-Dienste verringern.

Sie können die Tarife „Basic“, „Standard“ und „Premium“ von Azure API Management können in einer Azure-Region horizontal hochskalieren, um mehr Kapazität zu bieten. Zum Analysieren der Nutzung für Ihren Dienst wählen Sie im Menü **Metriken** die Option **Kapazitätsmetrik** aus und nehmen dann eine entsprechende zentrale Hoch- oder Herunterskalierung vor.

Empfehlungen zur Skalierung eines API Management-Diensts:

- Berücksichtigen Sie Datenverkehrsmuster bei der Skalierung. Kunden mit veränderlichen Datenverkehrsmustern benötigen mehr Kapazität.

- Eine konstante Kapazität von mehr als 66% kann darauf hinweisen, dass eine Hochskalierung notwendig ist.

- Eine konstante Kapazität von unter 20% kann darauf hinweisen, dass eine Herunterskalierung notwendig ist.

- Führen Sie immer einen Auslastungstest mit einer repräsentativen Last für Ihren API Management-Dienst durch, bevor Sie die Last in der Produktion aktivieren.

Sie können die Dienste des Premium-Tarifs über mehrere Azure-Regionen hinweg horizontal hochskalieren. Wenn Sie Dienste über mehrere Azure-Regionen hinweg skalieren, erhalten Sie eine höhere SLA (99,95% anstatt 99,9%) und können Dienste in der Nähe von Benutzern in mehreren Regionen bereitstellen.

Das serverlose Modell von Logic Apps bedeutet, dass Administratoren die Skalierbarkeit der Dienste nicht planen müssen. Der Dienst wird automatisch entsprechend den Anforderungen skaliert.

## <a name="availability"></a>Verfügbarkeit

* Die Vereinbarung zum Servicelevel (SLA) für Azure API Management beträgt für die Tarife „Basic“, „Standard“ und „Premium“ derzeit 99,9%. Für Konfigurationen mit Premium-Tarif und Bereitstellung von mindestens einer Einheit in zwei oder mehr Regionen gilt eine SLA von 99,95%.

* Die SLA für Azure Logic Apps beträgt derzeit 99,9%.

### <a name="backups"></a>Backups

Basierend auf regelmäßigen Änderungen muss Ihre Azure API Management-Konfiguration [regelmäßig gesichert](../api-management/api-management-howto-disaster-recovery-backup-restore.md) werden. Speichern Sie Ihre Sicherungsdateien an einem Speicherort oder in einer Azure-Region, in dem bzw. der sich Ihr Dienst nicht befindet. Sie können dann eine der Optionen als Strategie zur Notfallwiederherstellung wählen:

* Bei einer Notfallwiederherstellung stellen Sie eine neue API Management-Instanz bereit, stellen die Sicherung auf dieser neuen Instanz wieder her und leiten die DNS-Einträge um.

* Behalten Sie eine passive Kopie ihres Diensts in einer anderen Azure-Region. Dafür fallen zusätzliche Kosten an. Stellen Sie regelmäßig Sicherungen dieser Kopie wieder her. Bei einer Notfallwiederherstellung des Diensts müssen Sie lediglich die DNS-Einträge umleiten.

Da Logik-Apps schnell neu erstellt werden können und serverlos sind, sichern Sie sie, indem Sie eine Kopie der zugehörigen Azure Resource Manager-Vorlage speichern. Sie können Vorlagen in der Quellcodeverwaltung speichern, und Sie können Vorlagen in Ihren CI/CD-Prozess (Continuous Integration/Continuous Deployment) integrieren.

Wenn Sie eine Logik-App über Azure API Management veröffentlicht haben und diese in ein anderes Rechenzentrum verschoben wird, aktualisieren Sie den Speicherort der App. Sie können die **Backend**-Eigenschaft Ihrer API mithilfe eines einfachen PowerShell-Skripts aktualisieren.

## <a name="manageability"></a>Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Separate Ressourcengruppen erleichtern das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.

Berücksichtigen Sie beim Zuweisen von Ressourcen zu Ressourcengruppen diese Faktoren:

* **Lebenszyklus**: Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.

* **Zugriff**: Zur Anwendung von Zugriffsrichtlinien auf die Ressourcen in einer Gruppe können Sie die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) verwenden.

* **Abrechnung**: Sie können die anfallenden Kosten für die Ressourcengruppe anzeigen.

* **Tarif für API Management**: Verwenden Sie den Tarif „Developer“ für Ihre Entwicklungs- und Testumgebungen. Zur Kostenminimierung während der Präproduktion stellen Sie ein Replikat Ihrer Produktionsumgebung bereit, führen die Tests aus und fahren das Replikat dann herunter.

Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Bereitstellung

* Verwenden Sie die [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) zum Bereitstellen von API Management und Logic Apps. Vorlagen vereinfachen die Automatisierung von Bereitstellung, wenn PowerShell oder die Azure CLI verwendet wird.

* Integrieren Sie Azure API Management und die einzelnen Logik-Apps in eigene separate Resource Manager-Vorlagen. Durch die Verwendung unterschiedlicher Vorlagen können Sie die Ressourcen in Quellcodeverwaltungs-Systemen speichern. Diese Vorlagen können Sie dann gemeinsam oder einzeln im Rahmen eines Continuous Integration/Continous Deployment-Prozesses (CI/CD) einsetzen.

### <a name="versions"></a>Versionen

Bei jeder Konfigurationsänderung an einer Logik-App und jeder Bereitstellung eines Updates über eine Resource Manager-Vorlage bewahrt Azure eine Kopie dieser Version zu Ihrer Verfügung auf und speichert alle Versionen, die einen Ausführungsverlauf aufweisen. Sie können diese Versionen verwenden, um Änderungen im Verlauf zu verfolgen, oder eine Version als aktuelle Konfiguration der Logik-App höherzustufen. So können Sie eine Logik-App beispielsweise erfolgreich zurücksetzen.

Azure API Management bietet diese unterschiedlichen, aber sich ergänzenden [Versionierungskonzepte](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

* Versionen, die Ihren API-Consumern die Möglichkeit bieten, eine API-Version basierend auf ihren Anforderungen zu wählen, z.B. v1, v2, Beta oder Produktion

* Revisionen, die es API-Administratoren ermöglichen, Änderungen an einer API sicher vorzunehmen und diese Änderungen mit optionalen Kommentaren an Benutzer weiterzugeben

Betrachten Sie API Management-Revisionen für die Bereitstellung als eine sichere Methode, um Änderungen vorzunehmen, ein Änderungsprotokoll zu führen und die Consumer Ihrer API auf diese Änderungen hinzuweisen. Sie können eine Revision in einer Entwicklungsumgebung erstellen und diese Änderung mithilfe von Resource Manager-Vorlagen in anderen Umgebungen bereitstellen.

Sie können Revisionen zwar verwenden, um eine API zu testen, bevor Sie diese Änderungen „aktuell“ und für Benutzer zugänglich machen, wir empfehlen diese Methode jedoch nicht für Last- oder Integrationstests. Verwenden Sie stattdessen separate Test- oder Präproduktionsumgebungen.

### <a name="configuration-and-sensitive-information"></a>Konfiguration und vertrauliche Informationen

Checken Sie niemals Kennwörter, Zugriffsschlüssel oder Verbindungszeichenfolgen in die Quellcodeverwaltung ein. Wenn diese Werte benötigt werden, verwenden Sie die entsprechenden Verfahren, um sie bereitzustellen und zu sichern. 

Wenn eine Logik-App in Logic Apps vertrauliche Werte erfordert, die Sie nicht innerhalb einer Verbindung erstellen können, speichern Sie diese Werte in Azure Key Vault und verweisen in einer Resource Manager-Vorlage darauf. Verwenden Sie Vorlageparameter für die Bereitstellung und Parameterdateien für jede Umgebung. Weitere Informationen finden Sie unter [Sichern von Parametern und Eingaben innerhalb eines Workflows](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Management verwaltet Geheimnisse über Objekte, die als *benannte Werte* oder *Eigenschaften* bezeichnet werden. In diesen Objekten werden Werte sicher gespeichert, auf die Sie über API Management-Richtlinien zugreifen können. Weitere Informationen finden Sie unter [Verwalten von Geheimnissen in API Management](../api-management/api-management-howto-properties.md).

## <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Sie können [Azure Monitor](../azure-monitor/overview.md) sowohl in [API Management](../api-management/api-management-howto-use-azure-monitor.md) als auch [Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md) für die betriebliche Überwachung verwenden. Azure Monitor liefert Informationen basierend auf den für die einzelnen Dienste konfigurierten Metriken und ist standardmäßig aktiviert.

Jeder Dienst verfügt außerdem über folgende Optionen:

* Zur eingehenderen Analyse und Dashboardanzeige können Sie Logic Apps-Protokolle an [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) senden.

* Zur Überwachung von DevOps können Sie Azure Application Insights für API Management konfigurieren.

* API Management unterstützt die [Power BI-Lösungsvorlage für benutzerdefinierte API-Analysen](http://aka.ms/apimpbi). Sie können diese Lösungsvorlage zur Erstellung Ihrer eigenen Analyselösung verwenden. Für Geschäftskunden stellt Power BI Berichte zur Verfügung.

## <a name="security"></a>Sicherheit

Obwohl diese Liste nicht alle bewährten Sicherheitsmethoden vollständig beschreibt, finden Sie hier einige Sicherheitshinweise, die insbesondere für die Azure-Dienste gelten, die in der in diesem Artikel beschriebenen Architektur bereitgestellt sind:

* Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC), um sicherzustellen, dass Benutzer über entsprechende Zugriffsebenen verfügen.

* Sichern Sie öffentliche API-Endpunkte in API Management mit OAuth oder OpenID Connect. Um öffentliche API-Endpunkte zu sichern, konfigurieren Sie einen Identitätsanbieter und fügen eine JSON Web Token (JWT)-Validierungsrichtlinie hinzu.

* Stellen Sie eine Verbindung zu Back-End-Diensten aus API Management her, indem Sie gemeinsame Zertifikate verwenden.

* Sichern Sie Logik-Apps, die auf HTTP-Triggern basieren, indem Sie eine Whitelist von IP-Adressen erstellen, die auf die IP-Adresse von API Management verweist. Eine IP-Adresse auf der Whitelist verhindert, dass die Logik-App aus dem öffentlichen Internet aufgerufen wird, ohne vorher API Management zu durchlaufen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [Unternehmensintegration mit Warteschlangen und Ereignissen](../logic-apps/logic-apps-architectures-enterprise-integration-with-queues-events.md).

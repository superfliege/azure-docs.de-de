---
title: Azure Integration Services – einfache Unternehmensintegration
description: Referenzarchitektur, die zeigt, wie Sie eine ein Muster einer einfachen Unternehmensintegration mit Azure Logic Apps und Azure API Management implementieren.
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
ms.openlocfilehash: 17f591a470bf65d3c9365bca9c5ae2d5a6c9574f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231976"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Einfache Unternehmensintegration – Referenzarchitektur

## <a name="overview"></a>Übersicht

Diese Referenzarchitektur zeigt eine Reihe bewährter Methoden für eine Integrationsanwendung, die Azure Integration Services verwendet. Diese Architektur kann als Grundlage für viele verschiedene Anwendungsmuster dienen, die HTTP-APIs, Workflow und Orchestrierung erfordern.

*Es gibt viele mögliche Anwendungen der Integrationstechnologie, von einer einfachen Punkt-zu-Punkt-Anwendung bis hin zu einem vollständigen Unternehmensservicebus. Diese Architekturserie legt die wiederverwendbaren Komponenten für die Erstellung jeder beliebigen Integrationsanwendung fest – daher sollten Architekten die Komponenten berücksichtigen, die sie für ihre Anwendungen und Infrastruktur benötigen.*

   ![Architekturdiagramm – einfache Unternehmensintegration](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Architecture

Die Architektur besteht aus den folgenden Komponenten:

- Ressourcengruppe: Eine Ressourcengruppe ist ein logischer Container für Azure-Ressourcen.
- Azure API Management: Azure API Management ist eine vollständig verwaltete Plattform zur Veröffentlichung, Sicherung und Transformation von HTTP-APIs.
- Azure API Management-Entwicklerportal: Jede Instanz von Azure API Management verfügt über ein Entwicklerportal, das Zugriff auf Dokumentation, Codebeispiele und Testfunktionen für die API bietet.
- Azur Logic Apps: Bei Logic Apps handelt es sich um eine serverlose Plattform, die Unternehmensworkflow und -integration unterstützt.
- Connectors: Connectors werden von Logic Apps verwendet, um eine Verbindung zu häufig verwendeten Diensten herzustellen. Logic Apps verfügt bereits über hunderte von verschiedenen Connectors, aber es kann auch ein benutzerdefinierter Connector verwendet werden.
- IP-Adresse: Der Azure API Management-Dienst weist eine feste öffentliche IP-Adresse und einen Domänennamen auf. Der Domänenname ist eine Unterdomäne von azure-api.net, wie z.B. contoso. azure-api.net: Logic Apps hat auch eine öffentliche IP-Adresse; in dieser Architektur beschränken wir jedoch den Zugriff auf die Endpunkte von Logic Apps auf die IP-Adresse von API Management (aus Sicherheitsgründen).
- Azure DNS: Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter „Konfigurieren eines benutzerdefinierten Domänennamens in Azure API Management“.
- Azure Active Directory (Azure AD): Verwenden Sie Azure AD oder einen anderen Identitätsanbieter für die Authentifizierung. Azure AD bietet Authentifizierung für den Zugriff auf API-Endpunkte (durch Übergabe eines JSON Web Token für API Management zur Überprüfung) und kann den Zugriff auf das API Management-Entwicklerportal sichern (nur Standard- & Premium-Tarife).

Bei Vorgängen weist diese Architektur einige grundlegende Muster auf:

1. Bestehende Back-End-HTTP-APIs werden über das API Management-Entwicklerportal veröffentlicht, sodass Entwickler (organisationsinterne, externe oder beide) Aufrufe dieser APIs in Anwendungen integrieren können.
2. Verbund-APIs werden mithilfe von Logic Apps erstellt; sie orchestrieren Aufrufe an SAAS-Systeme, Azure-Dienste und alle APIs, die in API Management veröffentlicht werden. Die Logic Apps werden auch über das API Management-Entwicklerportal veröffentlicht.
3. Anwendungen erhalten ein OAuth 2.0-Sicherheitstoken, das für den Zugriff auf eine API mit Azure Active Directory erforderlich ist.
4. Azure API Management überprüft das Sicherheitstoken und leitet die Anforderung an die Back-End-API/Logik-App weiter.

## <a name="recommendations"></a>Empfehlungen

Ihre Anforderungen können von der hier beschriebenen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="azure-api-management-tier"></a>Azure API Management-Tarif

Verwenden Sie die Tarife „Basic“, „Standard“ oder „Premium“, da sie eine Vereinbarung zum Servicelevel für die Produktionsumgebung bieten und eine horizontale Skalierung innerhalb der Azure-Region unterstützen (Anzahl der Einheiten variiert je nach Tarif). Der Premium-Tarif unterstützt zudem die horizontale Skalierung über mehrere Azure-Regionen hinweg. Richten Sie den von Ihnen gewählten Tarif an Ihrem erforderlichen Durchsatz und Funktionsumfang aus. Weitere Informationen finden Sie unter [API Management-Preise](https://azure.microsoft.com/pricing/details/api-management/).

Es entstehen Kosten für alle API Management-Instanzen, sobald sie ausgeführt werden. Wenn Sie eine Skalierung durchgeführt haben und diese Leistung nicht ständig benötigen, sollten Sie die Vorteile der stündlichen Abrechnung von API Management nutzen und zentral herunterskalieren.

### <a name="logic-apps-pricing"></a>Logic Apps-Preise

Logic Apps arbeitet als [serverloses](logic-apps-serverless-overview.md) Modell – die Abrechnung erfolgt basierend auf Aktionen und Connectorausführung. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/). Für Logic Apps gibt es derzeit keine Überlegungen zu den Tarifen.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps für asynchrone API-Aufrufe

Am besten wird Logic Apps in Szenarien eingesetzt, die keine geringe Latenz erfordern, z.B. asynchrone oder teilweise ausgeführte API-Aufrufe. Wenn eine geringe Latenz erforderlich ist (z.B. ein Aufruf, der eine Benutzeroberfläche blockiert), wird die Implementierung dieser API oder dieses Vorgangs mit einer anderen Technologie empfohlen, z.B. Azure Functions oder eine über App Service bereitgestellte Web-API. Dennoch wird weiterhin empfohlen, dass diese API über API Management für API-Consumer verfügbar gemacht wird.

### <a name="region"></a>Region

Stellen Sie API Management und Logic Apps in der gleichen Region bereit, um die Netzwerklatenz zu verringern. Wählen Sie grundsätzlich die Ihren Benutzern am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf, die angibt, wo die Metadaten der Bereitstellung gespeichert werden und von wo aus die Bereitstellungsvorlage ausgeführt wird. Implementieren Sie die Ressourcengruppe und ihre Ressourcen in der gleichen Region. Dies kann die Verfügbarkeit während der Bereitstellung verbessern.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

API Management-Administratoren sollten gegebenenfalls [Cachingrichtlinien](../api-management/api-management-howto-cache.md) hinzufügen, um die Skalierbarkeit des Dienstes zu erhöhen und die Last für ihre Back-End-Dienste zu verringern.

Die Basic-, Standard- und Premium-Tarife von Azure API Management können in einer Azure-Region horizontal hochskaliert werden, um mehr Kapazität zu bieten. Administratoren können die Kapazitätsmetrik im Metrikmenü verwenden, um die Nutzung ihres Diensts zu analysieren und eine entsprechende Skalierung vorzunehmen.

Empfehlungen zur Skalierung eines API Management-Diensts:

- Bei der Skalierung müssen die Datenverkehrsmuster berücksichtigt werden – Kunden mit unbeständigen Datenverkehrsmustern benötigen mehr Kapazität.
- Eine konstante Kapazität von mehr als 66% kann darauf hinweisen, dass ein zentrales Hochskalieren notwendig ist.
- Eine konstante Kapazität unter 20% kann darauf hinweisen, dass ein zentrales Herunterskalieren notwendig ist.
- Es ist immer empfehlenswert, Ihren API Management-Dienst mit einer repräsentativen Last zu testen, bevor Sie ihn in der Produktionsumgebung einsetzen.

Dienste des Premium-Tarifs können über mehrere Azure-Regionen hinweg horizontal hochskaliert werden. Kunden mit einer derartigen Bereitstellung erhalten eine höhere SLA (99,95% gegenüber 99,9%) und können Dienste in der Nähe von Benutzern in mehreren Regionen zur Verfügung stellen.

Beim serverlosen Modell von Logic Apps müssen Administratoren keine zusätzlichen Überlegungen zur Skalierbarkeit des Diensts anstellen, denn der Dienst wird bei Bedarf automatisch skaliert.

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Zum Zeitpunkt der Erstellung beträgt die Vereinbarung zum Servicelevel (SLA) für Azure API Management für die Tarife „Basic“, „Standard“ und „Premium“ 99,9%. Vom Premium-Tarif abgedeckte Konfigurationen mit Bereitstellung von mindestens einer Einheit in zwei oder mehr Regionen haben eine SLA von 99,95%.

Zum Zeitpunkt der Erstellung liegt die SLA für Azure Logic Apps bei 99,9%.

### <a name="backups"></a>Sicherungen

Die Konfiguration von Azure API Management sollte [regelmäßig gesichert](../api-management/api-management-howto-disaster-recovery-backup-restore.md) werden (entsprechend der Häufigkeit von Änderungen), und die Sicherungsdateien sollten an einem anderen Ort oder in einer anderen Azure-Region als dem Ort, an dem sich der Dienst befindet, gespeichert werden. Kunden können dann eine von zwei Optionen für ihre Notfallwiederherstellungsstrategie wählen:

1. Bei einer Notfallwiederherstellung wird eine neue API Management-Instanz bereitgestellt, auf der die Sicherung wiederhergestellt wird, und die DNS-Datensätze werden umgeleitet.
2. Kunden behalten eine passive Kopie ihres Diensts in einer anderen Azure-Region (hierfür fallen zusätzliche Kosten an), worin Sicherungen regelmäßig wiederhergestellt werden. Im Falle einer Notfallwiederherstellung müssen nur die DNS-Datensätze umgeleitet werden, um den Dienst wiederherzustellen.

Da Logic Apps sehr schnell neu erstellt werden kann und serverlos ist, werden die Komponenten durch das Speichern einer Kopie der zugehörigen Azure Resource Manager-Vorlage gesichert. Diese können in der Quellcodeverwaltung gespeichert und in den Continuous Integration/Continuous Deployment-Prozess (CI/CD) eines Kunden integriert werden.

Für Logik-Apps, die über API Management veröffentlicht wurden, müssen die Standorte aktualisiert werden, wenn sie in ein anderes Rechenzentrum verschoben werden. Dies kann durch ein einfaches PowerShell-Skript erreicht werden, das die Back-End-Eigenschaft der API aktualisiert.

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Dies erleichtert das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.
Berücksichtigen Sie beim Zuweisen von Ressourcengruppen die folgenden Punkte:

- Lebenszyklus. Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.
- Zugriff. Sie können die [rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (RBAC) verwenden, um Zugriffsrichtlinien auf die in einer Gruppe enthaltenen Ressourcen anzuwenden.
- Abrechnung. Sie können die angefallenen Kosten für die Ressourcengruppe anzeigen.
- Tarif für API Management – für Entwicklungs- und Testumgebungen wird die Verwendung des Developer-Tarifs empfohlen. Vor der Einführung in die Produktionsumgebung empfehlen wir, ein Replikat Ihrer Produktionsumgebung bereitzustellen, Tests durchzuführen und dann herunterzufahren, um die Kosten zu minimieren.

Weitere Informationen finden Sie in der [Ressourcengruppenübersicht](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Bereitstellung

Es wird empfohlen, [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) zu verwenden, um sowohl Azure API Management als auch Azure Logic Apps bereitzustellen. Vorlagen erleichtern die Automatisierung von Bereitstellungen über PowerShell oder die Azure-Befehlszeilenschnittstelle (CLI).

Es wird empfohlen, Azure API Management und die einzelnen Logik-Apps in eigene separate Resource Manager-Vorlagen zu integrieren. Dadurch können sie in Systemen für die Quellcodeverwaltung gespeichert werden. Diese Vorlagen können dann gemeinsam oder einzeln im Rahmen eines Continuous Integration/Continous Deployment-Prozesses (CI/CD) eingesetzt werden.

### <a name="versions"></a>Versionen

Jedes Mal, wenn Sie eine Konfigurationsänderung an einer Logik-App vornehmen (oder ein Update über eine Resource Manager-Vorlage bereitstellen), wird eine Kopie dieser Version zu Ihrer Verfügung aufbewahrt (alle Versionen, die einen Ausführungsverlauf aufweisen, werden gespeichert). Sie können diese Versionen verwenden, um Änderungen im Verlauf zu verfolgen, und eine Version als aktuelle Konfiguration der Logik-App höherstufen; so können Sie eine Logik-App beispielsweise erfolgreich zurücksetzen.

API Management bietet zwei unterschiedliche (aber sich ergänzende) [Versionierungskonzepte](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Versionen, durch die API-Consumer eine Auswahl von APIs erhalten, die sie je nach Bedarf nutzen können (z.B. v1, v2 oder Betaversion, Version für die Produktionsumgebung).
- Revisionen, die es API-Administratoren ermöglichen, Änderungen an einer API sicher vorzunehmen und diese mit optionalen Kommentaren an Benutzer weiterzugeben.

Bei einer Bereitstellung sollten API Management-Revisionen als eine Möglichkeit betrachtet werden, um Änderungen sicher vorzunehmen, ein Änderungsprotokoll zu führen und die API-Consumer auf diese Änderungen hinzuweisen. Eine Revision kann in einer Entwicklungsumgebung erstellt und mithilfe von Resource Manager-Vorlagen in anderen Umgebungen bereitgestellt werden.

Wenngleich Revisionen zum Testen einer API verwendet werden können, bevor diese zur „aktuellen Version“ und Benutzern zugänglich gemacht wird, ist es nicht ratsam, diesen Mechanismus für Last- oder Integrationstests zu nutzen. Stattdessen sollten separate Test- oder Vorproduktionsumgebungen verwendet werden.

### <a name="configuration"></a>Konfiguration

Fügen Sie niemals Kennwörter, Zugriffsschlüssel oder Verbindungszeichenfolgen in die Quellcodeverwaltung ein. Wenn sie benötigt werden, verwenden Sie eine geeignete Methode, um diese Werte bereitzustellen und zu sichern. 

In Logic Apps sollten alle vertraulichen Werte, die innerhalb der Logik-App benötigt werden (und nicht in Form einer Verbindung erstellt werden können), in Azure Key Vault gespeichert und aus einer Resource Manager-Vorlage referenziert werden. Zudem wird die Verwendung von Vorlageparametern für die Bereitstellung zusammen mit Parameterdateien für jede Umgebung empfohlen. Weitere Informationen finden Sie unter [Sichern von Parametern und Eingaben innerhalb eines Workflows](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

In API Management werden Geheimnisse über Objekte verwaltet, die als „Benannte Werte/Eigenschaften“ bezeichnet werden. Auf diese Werte, die so sicher gespeichert werden, kann in API Management-Richtlinien zugegriffen werden. Erfahren Sie mehr darüber, wie Sie [Geheimnisse in API Management verwalten](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Sowohl [API Management](../api-management/api-management-howto-use-azure-monitor.md) als auch [Logic Apps](logic-apps-monitor-your-logic-apps.md) unterstützen die betriebliche Überwachung durch [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor ist standardmäßig aktiviert und liefert Informationen basierend auf den verschiedenen Metriken, die für jeden Dienst konfiguriert sind.

Darüber hinaus gibt es für jeden Dienst weitere Optionen:

- Logic Apps-Protokolle können zur eingehenderen Analyse und zum Dashboarding an [Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) gesendet werden.
- API Management unterstützt die Konfiguration von Application Insights für die Überwachung von Dev Ops.
- API Management unterstützt die [Power BI-Lösungsvorlage für benutzerdefinierte API-Analysen](http://aka.ms/apimpbi). Diese Lösungsvorlage ermöglicht es Kunden, ihre eigene benutzerdefinierte Analyselösung mit Berichten zu erstellen, die in Power BI für Geschäftsbenutzer verfügbar sind.

## <a name="security-considerations"></a>Sicherheitshinweise

Dieser Abschnitt enthält Sicherheitshinweise, die für die in diesem Artikel beschriebenen Azure-Dienste spezifisch sind und in dieser Architektur (wie beschrieben) bereitgestellt werden. Es handelt sich nicht um eine vollständige Liste der bewährten Sicherheitsmethoden.

- Verwenden Sie die rollenbasierte Zugriffssteuerung (RBAC), um angemessene Zugriffsebenen für Benutzer sicherzustellen.
- Sichern Sie öffentliche API-Endpunkte in API Management mit OAuth/OpenID Connect. Hierzu konfigurieren Sie einen Identitätsanbieter und fügen eine JWT-Überprüfungsrichtlinie hinzu.
- Stellen Sie eine Verbindung zu Back-End-Diensten aus API Management her, indem Sie gemeinsame Zertifikate verwenden.
- Sichern Sie Logik-Apps, die auf HTTP-Triggern basieren, indem Sie eine Whitelist von IP-Adressen erstellen, die auf die IP-Adresse von API Management verweist. Dies verhindert, dass die Logik-App aus dem öffentlichen Internet aufgerufen wird, ohne vorher API Management zu durchlaufen.

Diese Referenzarchitektur zeigte, wie eine einfache Unternehmensintegrationsplattform mit Azure Integration Services eingerichtet werden kann.

## <a name="next-steps"></a>Nächste Schritte

* [Unternehmensintegration mit Warteschlangen und Ereignissen](logic-apps-architectures-enterprise-integration-with-queues-events.md)

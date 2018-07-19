---
title: Governance in Azure | Microsoft-Dokumentation
description: Erfahren Sie mehr über cloudbasierte Computingdienste, die sich an die Anforderungen Ihrer Anwendung oder Ihres Unternehmens anpassen lassen.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 579e900ee6616af8fd197e501364acd8e18d3e37
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970522"
---
# <a name="governance-in-azure"></a>Governance (Kontrolle) in Azure

Azure bietet Ihnen viele Sicherheitsoptionen, die Sie selbst steuern können, um die individuellen Anforderungen der Bereitstellungen Ihrer Organisation erfüllen zu können.

Cloud Governance in Azure bezieht sich auf Entscheidungsfindungsprozesse, Kriterien und Richtlinien, die für die Planung, Architektur, Beschaffung, Bereitstellung, Ausführung und Verwaltung von Cloud Computing notwendig sind. Cloud Governance in Azure bietet einen integrierten Ansatz zur Überwachung und Beratung für Organisationen, bei dem die Verwendung der Azure-Plattform überprüft wird und Organisationen entsprechend beraten werden. 

Um einen Plan für Cloud Governance in Azure zu erstellen, müssen Sie sich eingehend mit den vorhandenen Mitarbeitern, Prozessen und Technologien befassen. Dann können Sie Frameworks erstellen, die es der IT-Abteilung leicht machen, die Geschäftsanforderungen konsequent zu unterstützen und gleichzeitig den Benutzern die Flexibilität bieten, die Features von Azure zu nutzen.

## <a name="implementation-of-policies-processes-and-standards"></a>Implementierung von Richtlinien, Prozessen und Standards 

Die Verwaltung hat Rollen und Verantwortlichkeiten eingeführt, um die Implementierung der Richtlinien zur Informationssicherheit und operativer Kontinuität innerhalb von Azure zu überwachen. Die Azure-Verwaltung ist für die Überwachung der Sicherheits- und Kontinuitätspraktiken innerhalb der jeweiligen Teams (einschließlich Drittanbieter) verantwortlich. Außerdem erleichtert sie die Einhaltung von Sicherheitsrichtlinien, -prozessen und -standards.

### <a name="account-provisioning"></a>Kontobereitstellung

Das Definieren der Kontohierarchie ist ein wichtiger Schritt zur Verwendung und Strukturierung von Azure-Diensten innerhalb eines Unternehmens. Es stellt die wichtigste Governancestruktur dar. Kunden mit einem Enterprise Agreement (EA) können die Umgebung in Abteilungen, Konten und Abonnements unterteilen.

![Kontobereitstellung](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Wenn Sie nicht über ein Enterprise Agreement verfügen, sollten Sie [Azure-Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) auf Abonnementebene verwenden, um die Hierarchie zu definieren. Ein Azure-Abonnement ist die grundlegende Einheit, in der alle Ressourcen enthalten sind. Es definiert außerdem einige Beschränkungen in Azure, z.B. die Anzahl der Kerne und Ressourcen. Abonnements können [Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) enthalten, in denen wiederum Ressourcen enthalten sind. [RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)-Prinzipien (rollenbasierte Zugriffssteuerung) gelten für diese drei Ebenen.

Jedes Unternehmen ist anders. Für Firmen, die keine Unternehmen sind, ermöglicht die Hierarchie der Verwendung von Azure-Tags Flexibilität bei der Organisation von Azure. Vor der Bereitstellung von Ressourcen in Azure sollten Sie eine Hierarchie gestalten und die Auswirkungen auf die Abrechnung, den Zugriff auf Ressourcen und die Komplexität verstehen.

### <a name="subscription-controls"></a>Steuerung der Abonnements

Abonnements bestimmen, wie die Nutzung von Ressourcen gemeldet und abgerechnet wird. Abonnements können für separate Abrechnungs- und Zahlungsprozesse eingerichtet werden. Ein Azure-Konto kann mehrere Abonnements enthalten. Abonnements können verwendet werden, um die Nutzung von Azure-Ressourcen mehrerer Abteilungen in einem Unternehmen zu ermitteln.

In einem Unternehmen kann es beispielsweise IT-, HR- und Marketingabteilungen geben, die alle verschiedene Projekte durchführen. Das Unternehmen kann die Abrechnung für jede Abteilung auf der Grundlage der Verwendung von Azure-Ressourcen wie virtuellen Computern vornehmen. Hierdurch kann das Unternehmen die Finanzen der einzelnen Abteilungen kontrollieren.

Azure-Abonnements richten drei Parameter ein:

- Eindeutige Abonnenten-ID

- Rechnungsstandort

- einen Satz der verfügbaren Ressourcen

Für eine Einzelperson beinhalten diese Parameter eine Microsoft-Konto-ID, eine Kreditkartennummer und die gesamte Palette der Azure-Dienste. Microsoft setzt je nach Abonnementtyp Verbrauchsgrenzen durch.

Azure-Registrierungshierarchien legen fest, wie Dienste innerhalb eines Enterprise Agreement strukturiert sind. Das Enterprise Agreement-Portal ermöglicht Kunden, den Zugriff auf Azure-Ressourcen aufzuteilen, die einem Enterprise Agreement zugewiesen sind. Dies basiert auf flexiblen Hierarchien und kann so an die Bedürfnisse eines Unternehmens angepasst werden. Das Muster einer Hierarchie sollte mit der Verwaltung und geografischen Struktur eines Unternehmens übereinstimmen, damit der entsprechende Zugriff auf Abrechnung und Ressourcen berücksichtigt wird.

Die drei übergeordneten Hierarchiemuster sind Funktion, Geschäftseinheit und Geografie. Abteilungen sind ein administratives Konstrukt für Kontogruppierungen. In jeder Abteilung können Abonnements Konten zugewiesen werden, die Silos für die Abrechnung und zahlreiche Schlüsselgrenzwerte erstellen (z.B. die Anzahl virtueller Computer und Speicherkonten).

![Steuerung der Abonnements](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Bei Organisationen mit einem Enterprise Agreement folgen Azure-Abonnements einer vierstufigen Hierarchie:

1. Administrator für die Unternehmensregistrierung

2. Abteilungsadministrator

3. Kontobesitzer

4. Dienstadministrator

Diese Hierarchie bestimmt Folgendes:

- Abrechnungsbeziehung

- Kontoverwaltung

- Zugriff auf Ressourcen über RBAC

- Grenzen:

  - Nutzung und Abrechnung (Gebührenkarte basierend auf Anzahl der Angebote)

  - Einschränkungen

  - Virtuelles Netzwerk

- Anbindung an Azure Active Directory (Azure AD) Eine Azure AD-Instanz kann mehreren Abonnements zugeordnet werden.

- Zuordnung zu einem Konto für die Unternehmensregistrierung

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) ermöglicht ausführliche Zugriffsverwaltung von Ressourcen in Azure. Mit RBAC können Sie den Benutzern nur die Zugriffsrechte gewähren, die diese zum Ausführen ihrer Aufgaben benötigen. Unternehmen müssen sich darauf konzentrieren, Mitarbeitern genau die Berechtigungen zuzuweisen, die sie benötigen. Zu viele Berechtigungen machen ein Konto zum leichten Angriffsziel. Wenn ihre Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. 

Anstatt allen uneingeschränkte Berechtigungen in Ihrem Azure-Abonnement oder Ihren Ressourcen zu gewähren, können Sie die Berechtigungen auf bestimmte Aktionen beschränken. Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer Mitarbeiter im gleichen Abonnement SQL-Datenbanken verwalten kann.

Um Zugriff zu gewähren, weisen Sie Benutzern, Gruppen oder Anwendungen in einem bestimmten Bereich Rollen zu. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Ein Benutzer mit Zugriff auf eine Ressourcengruppe kann beispielsweise alle Ressourcen verwalten, die darin enthalten sind, z.B. Websites, virtuelle Computer und Subnetze. In jedem Abonnement können Sie bis zu 2.000 Rollenzuweisungen erstellen.

Eine Rolle ist eine Sammlung von Berechtigungen, und RBAC verfügt über verschiedene integrierte Rollen. Die folgenden integrierten Rollen gelten für alle Ressourcentypen:

- **Besitzer** verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren.

- **Mitwirkende** können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.

- **Leser** können alle Azure-Ressourcen anzeigen.

Die verbleibenden integrierten Rollen in Azure ermöglichen die Verwaltung bestimmter Azure-Ressourcen. Mit der Rolle „Mitwirkender von virtuellen Computern“ können Benutzer beispielsweise virtuelle Computer erstellen und verwalten. Eine Liste der integrierten Rollen und ihrer Vorgänge finden Sie unter [Integrierte RBAC-Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC unterstützt Verwaltungsvorgänge von Azure-Ressourcen im Azure-Portal und in den Azure Resource Manager-APIs. RBAC kann in den meisten Fällen nicht alle Vorgänge auf Datenebene für Azure-Ressourcen autorisieren. Weitere Informationen dazu, wie RBAC auf Datenvorgänge erweitert wird, finden Sie unter [Grundlegendes zu Rollendefinitionen](https://docs.microsoft.com/azure/role-based-access-control/role-definitions).

Wenn die integrierten Rollen nicht Ihre spezifischen Zugriffsanforderungen erfüllen, können Sie [eine benutzerdefinierte Rolle erstellen](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Genau wie integrierte Rollen können auch benutzerdefinierte Rollen Benutzern, Gruppen und Anwendungen auf Abonnement-, Ressourcengruppen- und Ressourcenebene zugewiesen werden. Benutzerdefinierte Rollen können mit [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), der [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) oder der [REST-API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest) erstellt werden.

### <a name="resource-management"></a>Ressourcenverwaltung

In Azure sind zwei Bereitstellungsmodelle verfügbar: [klassisches Modell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) und Azure Resource Manager.

Ressourcen sind im klassischen Modell unabhängig voneinander vorhanden. Es gibt keine Möglichkeit, verwandte Ressourcen zu gruppieren. Sie müssen manuell nachverfolgen, aus welchen Ressourcen die Lösung oder Anwendung besteht, und daran denken, sie mit einem koordinierten Ansatz zu verwalten. Die grundlegende Verwaltungseinheit ist das Abonnement. Es ist schwierig, Ressourcen innerhalb eines Abonnements aufzuschlüsseln. Dies führt zur Erstellung einer großen Anzahl von Abonnements.

Das Resource Manager-Bereitstellungsmodell beinhaltet das Konzept einer [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Eine Ressourcengruppe ist ein Container für Ressourcen, die über einen gemeinsamen Lebenszyklus verfügen. Sie kann alle Ressourcen für die Lösung oder nur die Ressourcen enthalten, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten.

Das Resource Manager-Bereitstellungsmodell bietet verschiedene Vorteile:

- Sie müssen die Dienste für Ihre Lösung nicht mehr einzeln bearbeiten, sondern können sie als Gruppe bereitstellen, verwalten und überwachen.

- Sie können die Lösung während ihres Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

- Sie können Zugriffssteuerung auf alle Ressourcen in der Ressourcengruppe anwenden. Diese Richtlinien werden automatisch angewendet, wenn der Ressourcengruppe neue Ressourcen hinzugefügt werden.

- Sie können Tags auf Ressourcen anwenden, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren.

- Sie können JavaScript Object Notation (JSON) verwenden, um die Infrastruktur für Ihre Lösung zu definieren. Die JSON-Datei wird als Resource Manager-Vorlage bezeichnet.

- Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.

![Ressourcen-Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Empfehlungen zu Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analysiert Abhängigkeiten, um sicherzustellen, dass Ressourcen in der richtigen Reihenfolge erstellt werden. Wenn eine Ressource von einem Wert einer anderen Ressource abhängig ist (z.B. ein virtueller Computer, der ein Speicherkonto für Datenträger benötigt), [legen Sie eine Abhängigkeit](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) in der Vorlage fest.

Sie können die Vorlage auch für Aktualisierungen der Infrastruktur verwenden. Beispielsweise können Sie Ihrer Lösung eine Ressource sowie Konfigurationsregeln für die Ressourcen hinzufügen, die bereits bereitgestellt wurden. Wenn in der Vorlage die Erstellung einer Ressource angegeben ist, diese aber bereits vorhanden ist, führt Resource Manager anstelle der Erstellung einer neuen Ressource eine Aktualisierung durch. Ressource Manager aktualisiert die vorhandene Ressource auf den Zustand, der für eine neue Ressource gelten würde.

Resource Manager verfügt über Erweiterungen für Szenarien, in denen zusätzliche Vorgänge, wie z.B. das Installieren von Software, die nicht Teil des Setups ist, erforderlich sind.

### <a name="resource-tracking"></a>Ressourcennachverfolgung

Wenn Benutzer in Ihrer Organisation Ressourcen zum Abonnement hinzufügen, wird es zunehmend wichtig, die Ressourcen der entsprechenden Abteilung, dem Kunden und der Umgebung zuzuordnen. Sie können über [Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) Metadaten an Ressourcen anfügen. Verwenden Sie Tags, um Informationen zur Ressource oder zum Besitzer bereitzustellen. Mit Tags können Sie Ressourcen nicht nur auf verschiedene Arten aggregieren und gruppieren, Sie können diese Daten auch für die verbrauchsbasierte Kostenzuteilung verwenden.

Die Verwendung von Tags (Markierungen) kann ratsam sein, wenn Sie über eine komplexe Sammlung von Ressourcengruppen und Ressourcen verfügen und diese Ressourcen auf möglichst sinnvolle Weise visualisieren müssen. Beispielsweise können Sie Ressourcen markieren, die in Ihrer Organisation eine ähnliche Funktion haben oder zu derselben Abteilung gehören.

Benutzer in Ihrer Organisation können mehrere Ressourcen erstellen, die ohne Tags später schwer zu identifizieren und zu verwalten sind. Es kann beispielsweise sein, dass Sie alle Ressourcen für ein Projekt löschen möchten. Wenn diese Ressourcen für das Projekt nicht mit Tags versehen sind, müssen Sie manuell danach suchen. Mit Tags lassen sich unter Umständen unnötige Kosten in Ihrem Abonnement vermeiden.

Ressourcen müssen sich nicht in derselben Ressourcengruppe befinden, um ein gemeinsames Tag aufzuweisen. Sie können Ihre eigene Tagtaxonomie erstellen, um dafür zu sorgen, dass alle Benutzer in Ihrer Organisation die gleichen Tags verwenden. So wird verhindert, dass versehentlich leicht unterschiedliche Tags angewendet werden (z.B. „dept“ anstelle von „department“).

Mithilfe von Ressourcenrichtlinien können Sie Standardregeln für Ihre Organisation erstellen. Sie können Richtlinien erstellen, um sicherzustellen, dass Ressourcen durch die entsprechenden Werte gekennzeichnet werden.

Sie haben auch die Möglichkeit, Ressourcen mit Tags über das Azure-Portal anzuzeigen. Der [Nutzungsbericht](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) für Ihr Abonnement enthält Tagnamen und -werte, sodass Sie die Kosten nach Tags aufschlüsseln können.

Weitere Informationen zu Tags finden Sie unter [Richtlinieninitiative zur Abrechnung von Tags](../azure-policy/scripts/billing-tags-policy-init.md).

Für Tags gelten folgende Einschränkungen:

- Jede Ressource oder Ressourcengruppe kann maximal 15 Tagschlüssel-Wert-Paare haben. Diese Einschränkung gilt nur für Tags, die direkt auf die Ressourcengruppe oder die Ressource angewendet werden. Eine Ressourcengruppe kann zahlreiche Ressourcen mit jeweils 15 Tagschlüssel-Wert-Paaren enthalten.

- Der Tagname darf maximal 512 Zeichen lang sein.

- Der Tagwert darf maximal 256 Zeichen lang sein.

- Auf die Ressourcengruppe angewendete Tags werden nicht von den in dieser Ressourcengruppe enthaltenen Ressourcen geerbt.

Wenn Sie einer Ressource mehr als 15 Werte zuordnen müssen, verwenden Sie eine JSON-Zeichenfolge für den Tagwert. Die JSON-Zeichenfolge kann zahlreiche Werte enthalten, die auf einen einzelnen Tagschlüssel angewendet werden.

#### <a name="tags-for-billing"></a>Tags für die Abrechnung

Mithilfe von Tags können Sie Ihre Abrechnungsdaten gruppieren. Wenn Sie z.B. mehrere virtuelle Computer für verschiedene Organisationen betreiben, gruppieren Sie die Nutzung mithilfe von Tags nach Kostenstelle. Mit Tags können Sie auch Kosten nach Runtimeumgebung kategorisieren, beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung.

Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontenportal](https://account.windowsazure.com/) oder dem [EA-Portal](https://ea.azure.com/) herunter.

Weitere Informationen zum programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)(in englischer Sprache).

Wenn Sie die CSV-Nutzungsdatei für Dienste herunterladen, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte Tags enthalten.

### <a name="critical-resource-controls"></a>Wichtige Ressourcenkontrollen

Wenn Ihre Organisation dem Abonnement zentrale Dienste hinzufügt, wird es zunehmend wichtig sicherzustellen, dass diese Dienste verfügbar sind, um eine Unterbrechung des Geschäftsbetriebs zu vermeiden. [Ressourcensperren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) ermöglichen Ihnen, Vorgänge auf wertvollen Ressourcen einzuschränken, bei denen das Ändern oder Löschen einen erheblichen Einfluss auf Ihre Anwendungen oder die Cloudinfrastruktur hätte. Sie können Sperren auf ein Abonnement, eine Ressourcengruppe oder eine Ressource anwenden. Normalerweise wenden Sie Sperren auf grundlegende Ressourcen wie Speicherkonten, virtuelle Netzwerke und Gateways an.

Ressourcensperren unterstützen aktuell zwei Werte: **CanNotDelete** und **ReadOnly**. **CanNotDelete** bedeutet, dass Benutzer (mit den entsprechenden Berechtigungen) eine Ressource lesen oder ändern, sie jedoch nicht löschen können. **ReadOnly** bedeutet, dass autorisierte Benutzer eine Ressource nicht löschen oder ändern können.

Ressourcensperren gelten nur für Vorgänge auf der Verwaltungsebene (also für Vorgänge, die an <https://management.azure.com> gesendet werden). Die Ausführung ressourceneigener Funktionen wird durch die Sperren nicht eingeschränkt. Die Ressourcenänderungen sind eingeschränkt, die Ressourcenvorgänge jedoch nicht. So verhindert beispielsweise eine **ReadOnly**-Sperre für eine SQL-Datenbankinstanz zwar, dass die Datenbank gelöscht oder angepasst wird, nicht aber das Erstellen, Aktualisieren oder Löschen von Daten in der Datenbank.

Die Anwendung von **ReadOnly** kann zu unerwarteten Ergebnissen führen, da einige Vorgänge, bei denen es sich scheinbar um Lesevorgänge handelt, zusätzliche Aktionen erfordern. Das Festlegen von **ReadOnly** für ein Speicherkonto hindert beispielsweise alle Benutzer am Auflisten der Schlüssel. Der Vorgang zum Auflisten von Schlüsseln wird über eine POST-Anforderung behandelt, da die zurückgegebenen Schlüssel für Schreibvorgänge zur Verfügung stehen.

![Wichtige Ressourcenkontrollen](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Ein weiteres Beispiel: Das Festlegen von **ReadOnly** für eine App Service-Ressource verhindert, dass der Server-Explorer von Visual Studio Dateien für die Ressource anzeigen kann, da für diese Interaktion Schreibzugriff erforderlich ist.

Im Gegensatz zur rollenbasierten Zugriffssteuerung verwenden Sie Verwaltungssperren, um eine Einschränkung für alle Benutzer und Rollen zu aktivieren. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [Verwalten des Zugriffs mit RBAC und dem Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Wenn Sie eine Sperre in einem übergeordneten Bereich anwenden, erben alle Ressourcen in diesem Bereich die entsprechende Sperre. Auch Ressourcen, die Sie später hinzufügen, erben die Sperre aus dem übergeordneten Element. Die restriktivste Sperre in der Vererbung hat Vorrang.

Zum Erstellen oder Löschen von Verwaltungssperren müssen Sie Zugriff auf Aktionen vom Typ Microsoft.Authorization/ oder Microsoft.Authorization/locks/ haben. Unter den integrierten Rollen verfügen nur „Besitzer“ und „Benutzerzugriffsadministrator“ über diese Aktionen.

### <a name="api-access-to-billing-information"></a>API-Zugriff auf Abrechnungsinformationen

Verwenden Sie Azure-Abrechnungs-APIs, um Nutzungs- und Ressourcendaten mittels Pull in Ihre bevorzugten Datenanalysetools abzurufen. Mithilfe der Azure-Ressourcennutzungs- und -Gebührenkarten-APIs können Sie Ihre Kosten genau vorhersagen und verwalten. Die APIs werden als Ressourcenanbieter implementiert und sind Teil der API-Familie, die von Azure Resource Manager bereitgestellt wird.

#### <a name="resource-usage-api-preview"></a>API zur Ressourcennutzung (Vorschau)

Mit der [Azure-Ressourcennutzungs-API](https://msdn.microsoft.com/library/azure/mt219003) können Sie Ihre geschätzten Azure-Nutzungsdaten abrufen. Die API umfasst:

- **RBAC**: Sie können Ihre Zugriffsrichtlinien im [Azure-Portal](https://portal.azure.com/) oder mit [Azure PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azure/overview) konfigurieren, um festzulegen, welche Benutzer oder Anwendungen Zugriff auf die Nutzungsdaten eines Abonnements erhalten. Aufrufer müssen zur Authentifizierung standardmäßige Azure Active Directory-Token verwenden. Ordnen Sie dem Aufrufer die Rolle „Abrechnungsleser“, „Leser“, „Besitzer“ oder „Mitwirkender“ zu, um Zugriff auf die Nutzungsdaten für ein bestimmtes Azure-Abonnement zu erhalten.

- **Stündliche oder tägliche Aggregationen**: Aufrufer können angeben, ob sie ihre Azure-Nutzungsdaten in stündlichen oder täglichen Inkrementen erhalten möchten. Die Standardeinstellung ist „Täglich“.

- **Instanzmetadaten (inkl. Ressourcentags)**: Details der Instanzebene, z.B. der vollqualifizierte Ressourcen-URI „(/subscriptions/{subscription-id}/..)“, Ressourcengruppeninformationen und Ressourcentags. Mit diesen Metadaten können Sie die Nutzung auf deterministische und programmgesteuerte Weise anhand der Tags zuordnen, z.B. bei der internen Verrechnung.

- **Ressourcenmetadaten**: Ressourcendetails (z.B. Meter Name, Meter Category, Meter Subcategory, Unit und Region) lassen den Aufrufer besser verstehen, was genutzt wurde. Außerdem arbeiten wir an einer Vereinheitlichung der Terminologie für Ressourcenmetadaten im Azure-Portal, in der CSV-Datei zur Azure-Nutzung, in der CSV-Datei zur EA-Abrechnung und in anderen öffentlich zugänglichen Benutzeroberflächen, um Ihnen die Korrelation von Daten über verschiedene Umgebungen hinweg zu ermöglichen.

- **Nutzung für alle Angebotstypen**: Nutzungsdaten sind für alle Angebotstypen verfügbar, z.B. „Nutzungsbasierte Bezahlung“, „MSDN“, „Verbindliche Zusage“, „Guthaben“ und „EA“.

#### <a name="resource-ratecard-api"></a>API für Ressourcengebührenkarte

Mit der Azure-Ressourcengebührenkarten-API (Resource RateCard) können Sie die Liste der verfügbaren Azure-Ressourcen sowie jeweils die geschätzten Preise abrufen. Die API umfasst:

- **RBAC:** Sie können Ihre Zugriffsrichtlinien im Azure-Portal oder mit Azure PowerShell-Cmdlets konfigurieren, um festzulegen, welche Benutzer oder Anwendungen Zugriff auf die Daten der Gebührenkarte (RateCard) erhalten. Aufrufer müssen zur Authentifizierung standardmäßige Azure Active Directory-Token verwenden. Ordnen Sie dem Aufrufer die Rolle „Leser“, „Besitzer“ oder „Mitwirkender“ zu, um Zugriff auf die Nutzungsdaten für ein bestimmtes Azure-Abonnement zu erhalten.

- **Unterstützung für die Angebote „Nutzungsbasierte Bezahlung“, „MSDN“, „Verbindliche Zusage“ und „Guthaben“ (nicht jedoch „EA“)**: Mit dieser API werden Preisinformationen der Azure-Angebotsebene bereitgestellt. Aufrufer dieser API müssen die Angebotsinformationen übergeben, um Ressourcendetails und -preise abzurufen. EA wird zurzeit nicht unterstützt, weil EA-Angebote über angepasste Preise pro Registrierung verfügen. 

#### <a name="scenarios"></a>Szenarien

Die Kombination aus Nutzungs- und Gebührenkarten-APIs ermöglicht diese Szenarien:

- **Informationen zu Azure-Ausgaben während des Monats**: Setzen Sie die Nutzungs- und Gebührenkarten-APIs zusammen ein, um einen besseren Einblick in ihre Cloudausgaben während eines Monats zu erhalten. Hierzu analysieren sie die stündlichen und täglichen Nutzungs- und Gebührenschätzungen.

- **Einrichten von Warnungen** : Verwenden Sie die Nutzungs- und Gebührenkarten-APIs, um die Verbrauchs- und Gebührenschätzungen für die Cloud abzurufen und ressourcenbasierte oder kostenbasierte Warnungen einzurichten.

- **Vorhersagen des Rechnungsbetrags**: Rufen Sie Ihren geschätzten Verbrauch und die geschätzten Cloudausgaben ab, und wenden Sie Machine Learning-Algorithmen an, um vorherzusagen, welcher Betrag sich am Ende des Abrechnungszeitraums ergibt.

- **Ausführen einer Kostenanalyse vor der Nutzung** : Mit der Gebührenkarten-API können Sie vorhersagen, wie hoch ihre Rechnung bei der erwarteten Nutzung wäre, wenn sie ihre Workloads in Azure verlagern. Wenn Sie über vorhandene Workloads in anderen Clouds bzw. Private Clouds verfügen, können Sie die Nutzung auch den Azure-Preisen zuordnen, um eine bessere Schätzung der Azure-Ausgaben zu erhalten. Diese Schätzungen bieten die Pivot-Funktion für ein Angebot an und ermöglichen Ihnen, verschiedene Angebotstypen über die nutzungsbasierte Bezahlung hinaus zu vergleichen und gegenüberzustellen, z.B. verbindliche Zusage und Guthaben.

- **Ausführen einer Was-wäre-wenn-Analyse**: Sie können ermitteln, ob es kostengünstiger ist, Ihre Workloads in einer anderen Region oder unter einer anderen Konfiguration der Azure-Ressource auszuführen. Azure-Ressourcenkosten können je nach der Azure-Region variieren, in der sie ausgeführt werden. Außerdem können Sie ermitteln, ob ein anderer Azure-Angebotstyp zu einem besseren Preis für eine Azure-Ressource führt.

### <a name="networking-controls"></a>Netzwerkkontrollen

Der Zugriff auf Ressourcen kann intern (innerhalb des Unternehmensnetzwerks) oder extern (über das Internet) erfolgen. Für Benutzer in Ihrer Organisation ist es einfach, versehentlich Ressourcen an der falschen Stelle zu platzieren und so böswilligen Zugriff darauf zu ermöglichen. Wie bei lokalen Geräten müssen Unternehmen die entsprechende Kontrolle implementieren, um sicherzustellen, dass Azure-Benutzer die richtigen Entscheidungen treffen.

![Netzwerkkontrollen](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Für die Abonnementgovernance stellen die folgenden Hauptressourcen grundlegende Kontrolle des Zugriffs bereit.

#### <a name="network-connectivity"></a>Netzwerkverbindung

[Virtuelle Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) sind Containerobjekte für Subnetze. Obwohl nicht unbedingt notwendig, wird ein virtuelles Netzwerk häufig für die Verbindung von Anwendungen mit internen Unternehmensressourcen verwendet. Mit dem Dienst Azure Virtual Network können Sie Azure-Ressourcen über virtuelle Netzwerke sicher miteinander verbinden.

Ein virtuelles Netzwerk ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Ein virtuelles Netzwerk ist eine logische Isolierung von der Azure Cloud für Ihr Abonnement. Sie können virtuelle Netzwerke auch mit Ihrem lokalen Netzwerk verbinden.

Folgendes sind Funktionen für virtuelle Azure-Netzwerke:

- **Isolierung**: Virtuelle Netzwerke sind voneinander isoliert. Sie können separate virtuelle Netzwerke für Entwicklung, Tests und Produktion erstellen, die die gleichen CIDR-Adressblöcke verwenden. Umgekehrt können Sie mehrere virtuelle Netzwerke erstellen, die unterschiedliche CIDR-Adressblöcke verwenden, und die Netzwerke verbinden. Sie können ein virtuelles Netzwerk in mehrere Subnetze segmentieren. Azure bietet interne Namensauflösung für virtuelle Computer und Azure Cloud Services-Rolleninstanzen, die mit einem virtuellen Netzwerk verbunden sind. Sie können wahlweise ein virtuelles Netzwerk konfigurieren, um Ihre eigenen DNS-Server anstelle der internen Namensauflösung von Azure zu verwenden.

- **Internetkonnektivität**: Für alle Azure Virtual Machines (VMs) und Cloud Services-Rolleninstanzen, die mit einem virtuellen Netzwerk verbunden sind, besteht standardmäßig Zugriff auf das Internet. Bei Bedarf können Sie auch den Zugriff in eingehender Richtung auf bestimmte Ressourcen ermöglichen.

- **Konnektivität von Azure-Ressourcen**: Sie können Azure-Ressourcen (z.B. Cloud Services und virtuelle Computer) mit demselben virtuellen Netzwerk verbinden. Für die Verbindung miteinander können für die Ressourcen auch dann private IP-Adressen verwendet werden, wenn sie sich in unterschiedlichen Subnetzen befinden. Azure bietet Standardrouting zwischen Subnetzen, virtuellen Netzwerken und lokalen Netzwerken, sodass Sie keine Routen konfigurieren und verwalten müssen.

- **Konnektivität virtueller Netzwerke**: Sie können virtuelle Netzwerke miteinander verbinden. Ressourcen, die mit einem beliebigen virtuellen Netzwerk verbunden sind, können dann mit beliebigen Ressourcen in einem beliebigen anderen virtuellen Netzwerk kommunizieren.

- **Lokale Konnektivität**: Virtuelle Netzwerke können mit lokalen Netzwerken verbunden werden, indem private Netzwerkverbindungen zwischen Ihrem Netzwerk und Azure oder Site-to-Site-VPN-Verbindungen (virtuelles privates Netzwerk) über das Internet verwendet werden.

- **Filtern des Datenverkehrs**: Sie können den Netzwerkdatenverkehr (eingehend und ausgehend) für virtuelle Computer und Cloud Services nach IP-Quelladresse und Port, IP-Zieladresse und Port sowie Protokoll filtern.

- **Routing**: Sie können das Standardrouting von Azure optional außer Kraft setzen, indem Sie Ihre eigenen Routen konfigurieren oder BGP-Routen über ein Netzwerkgateway verwenden.

#### <a name="network-access-controls"></a>Kontrolle des Netzwerkzugriffs

[Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSGs) ähneln einer Firewall und bieten Regeln dafür, wie eine Ressource über das Netzwerk „kommunizieren“ kann. Mit ihnen können Sie steuern, wie ein Subnetz (oder ein virtueller Computer) eine Verbindung mit dem Internet oder anderen Subnetzen im gleichen virtuellen Netzwerk herstellen kann.

Eine Netzwerksicherheitsgruppe enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit virtuellen Azure-Netzwerken verbunden sind, zugelassen oder abgelehnt wird. NSGs können Subnetzen, einzelnen VMs (klassisch) oder einzelnen Netzwerkadaptern (NICs), die mit virtuellen Computern (Resource Manager) verbunden sind, zugeordnet werden.

Wenn eine NSG einem Subnetz zugeordnet ist, gelten die Regeln für alle Ressourcen, die mit dem Subnetz verbunden sind. Sie können den Datenverkehr weiter einschränken, indem eine NSG einem virtuellen Computer oder einem Netzwerkadapter zugeordnet wird.

## <a name="security-and-compliance-with-organizational-standards"></a>Sicherheit und Konformität mit den Standards der Organisation

Jedes Unternehmen hat unterschiedliche Anforderungen und profitiert unterschiedlich von Cloudlösungen. Dennoch haben Kunden jeder Art die gleichen grundlegenden Bedenken über den Wechsel zur Cloud. Kunden wünschen sich Folgendes von ihren Cloud-Anbietern:

- **Sichern von Daten**: IT-Experten bestätigen, dass die Cloud mehr Datensicherheit und administrative Kontrolle bieten kann. Aber sie sind immer noch besorgt, dass die Migration in die Cloud sie anfälliger für Hacker macht als ihre aktuellen hausinternen Lösungen.

- **Geheimhaltung von Daten**: Clouddienste werfen besondere Herausforderungen im Hinblick auf den Datenschutz auf. Während Unternehmen die Cloud als Möglichkeit in Betracht ziehen, Infrastrukturkosten zu sparen und ihre Flexibilität zu verbessern, haben sie gleichzeitig Bedenken, die Kontrolle darüber zu verlieren, wo die Daten gespeichert werden, wer auf sie zugreift und wie sie verwendet werden.

- **Beibehalten der Kontrolle**: Selbst wenn Unternehmen die Vorteile der Cloud nutzen, um innovativere Lösungen bereitzustellen, befürchten sie, die Kontrolle über ihre Daten zu verlieren. Aktuelle Enthüllungen, dass Behörden mit legalen und anderen Mitteln auf Kundendaten zugreifen, lassen manche CIOs vor dem Speichern ihrer Daten in der Cloud zurückschrecken.

- **Förderung der Transparenz**: Entscheidungsträger in Unternehmen verstehen die Wichtigkeit von Sicherheit, Datenschutz und Kontrolle. Aber sie möchten auch über eine Möglichkeit verfügen, unabhängig zu überprüfen, wie ihre Daten gespeichert, abgerufen und gesichert werden.

- **Einhalten von Richtlinien**: Während Unternehmen ihre Nutzung von Cloudtechnologien erweitern, entwickeln sich Komplexität und Umfang von Standards und Richtlinien stetig weiter. Unternehmen müssen sicher sein, dass ihre Konformitätsstandards eingehalten werden.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>Sicherheitskonfiguration für die Überwachung, Protokollierung und Überprüfung

Azure-Abonnenten können ihre Cloudumgebungen mit mehreren Geräten verwalten. Diese Geräte sind z.B. Arbeitsstationen für die Verwaltung, Entwickler-PCs und sogar geeignete Endbenutzergeräte, die über aufgabenspezifische Berechtigungen verfügen. 

In einigen Fällen werden Administratorfunktionen über webbasierte Konsolen ausgeführt, z.B. das Azure-Portal. In anderen Fällen können auch direkte Verbindungen mit Azure von lokalen Systemen über VPNs, Terminal Services, Clientanwendungsprotokolle oder (programmgesteuert) die Azure-Dienstverwaltungs-API (SMAPI) bestehen. Außerdem können Clientendpunkte entweder Mitglied einer Domäne oder isoliert und unverwaltet sein, z.B. Tablets oder Smartphones.

Diese Variabilität kann das Risiko einer Cloudbereitstellung erheblich erhöhen. Administrative Aktionen lassen sich dadurch unter Umständen schwerer verwalten, nachvollziehen und überprüfen. Diese Variabilität kann auch mit Sicherheitsbedrohungen verbunden sein, wenn nicht regulierter Zugriff auf Clientendpunkte besteht, die zum Verwalten von Clouddiensten verwendet werden. Die Verwendung allgemeiner oder persönlicher Arbeitsstationen zum Entwickeln und Verwalten einer Infrastruktur geht mit unberechenbaren Bedrohungsvektoren einher. Hierzu zählen beispielsweise das Surfen im Web (etwa Watering Hole-Angriffe) oder E-Mails (etwa Social Engineering und Phishing).

Überwachung, Protokollierung und Überprüfung bilden die Grundlage für die Nachverfolgung und das Verständnis administrativer Aktivitäten. Eine Überwachung aller Aktionen in allen Details ist aufgrund der Menge der generierten Daten nicht immer möglich. Das Überprüfen der Effektivität von Verwaltungsrichtlinien ist aber eine bewährte Methode.

Azure Security Governance aus Azure Active Directory Domain Services-Gruppenrichtlinienobjekten kann Sie dabei unterstützen, alle Windows-Schnittstellen der Administratoren (z.B. die Dateifreigabe) zu kontrollieren. Binden Sie Verwaltungsarbeitsstationen in die Prozesse der Bereiche Überwachung, Protokollierung und Überprüfung ein. Verfolgen Sie für Administratoren und Entwickler den gesamten Zugriff und die Nutzung.

### <a name="azure-security-center"></a>Azure Security Center

Das [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) bietet eine zentrale Ansicht des Sicherheitsstatus von Ressourcen in Abonnements. Hier finden Sie Empfehlungen, mit deren Hilfe die Gefährdung von Ressourcen verhindert werden kann. Security Center kann detailliertere Richtlinien ermöglichen (z.B. Richtlinien auf bestimmte Ressourcengruppen anwenden, mit denen das Unternehmen anpassen kann, wie es einem bestimmten Risiko ausgesetzt wird).

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden. Nach Aktivierung der [Sicherheitsrichtlinien](https://docs.microsoft.com/azure/security-center/security-center-policies) für die Ressourcen eines Abonnements analysiert Security Center die Sicherheit Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Informationen zur Netzwerkkonfiguration sind sofort verfügbar.

Azure Security Center stellt eine Kombination aus bewährten Analysemethoden und Verwaltung der Sicherheitsrichtlinien für alle Ressourcen in einem Azure-Abonnement dar. Security Center ermöglicht Sicherheitsteams und Risikobeauftragten Sicherheitsrisiken vorzubeugen, diese zu erkennen und darauf zu reagieren. Es sammelt automatisch Sicherheitsdaten aus Ihren Azure-Ressourcen, dem Netzwerk und Partnerlösungen wie Antischadsoftwareprogrammen und Firewalls und analysiert diese.

Außerdem wendet Azure Security Center erweiterte Analysen an, einschließlich Machine Learning und Verhaltensanalyse. Es nutzt die globalen Bedrohungsdaten von Microsoft-Produkten und -Diensten, von der Microsoft Digital Crimes Unit (DCU), vom Microsoft Security Response Center (MSRC) sowie von externen Feeds. Sie können [Security Governance](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) allgemein auf Abonnementebene anwenden. Oder Sie können sie auf bestimmte Anforderungen einschränken und durch die Definition von Richtlinien auf einzelne Ressourcen anwenden.

Abschließend analysiert Azure Security Center basierend auf diesen Richtlinien die Sicherheitsintegrität der Ressource und verwendet diese Informationen, um informative Dashboards und Warnungen für Ereignisse wie die Erkennung von Schadsoftware und schädlichen IP-Verbindungsversuchen bereitzustellen. Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Azure Security Center überwacht die folgenden Azure-Ressourcen:

- Virtuelle Computer (einschließlich Clouddienste)

- Virtuelle Netzwerke

- SQL-Datenbanken

- Partnerlösungen, die in Ihr Azure-Abonnement integriert sind, z.B. die Firewall einer Webanwendung auf virtuellen Computern und in einer [App Service-Umgebung](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Es wird empfohlen, die Datensammlung aktiviert zu lassen. Sie können sie aber in der Security Center-Richtlinie [deaktivieren](https://docs.microsoft.com/azure/security-center/security-center-faq).

### <a name="log-analytics"></a>Log Analytics

Das Informationssicherheits- und [Governanceprogramm](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) des Azure Log Analytics-Softwareentwicklungs- und -Serviceteams unterstützt die geschäftlichen Anforderungen. Es befolgt die unter [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) und [Microsoft Trust Center Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx) beschriebenen Gesetze und Vorschriften. Außerdem wird hier beschrieben, wie Log Analytics Sicherheitsanforderungen einrichtet, Sicherheitskontrollen identifiziert und Risiken verwaltet und überwacht. Richtlinien, Standards, Verfahren und Leitlinien werden jährlich durch das Team überprüft.

Jedes Mitglied des Log Analytics-Entwicklungsteams erhält eine formale Anwendungssicherheitsschulung. Ein Versionskontrollsystem schützt jedes Softwareprojekt in der Entwicklung.

Microsoft hat ein Sicherheits- und Compliance-Team, das alle Microsoft-Dienste überwacht und bewertet. Dieses Team besteht aus Informationssicherheitsbeauftragten, die nicht mit den Entwicklungsabteilungen, die Log Analytics entwickeln, verbunden sind. Die Sicherheitsbeauftragten verfügen über eine eigene Managementkette. Sie führen unabhängige Beurteilungen von Produkten und Diensten aus, um Sicherheit und Konformität zu gewährleisten.

Die Kernfunktionen von Log Analytics werden durch eine Reihe von Diensten bereitgestellt, die in Azure ausgeführt werden. Jeder Dienst bietet eine bestimmte Verwaltungsfunktion, und Sie können Dienste miteinander kombinieren, um verschiedene Verwaltungsszenarien zu bewältigen.

![Azure-Dienste für die Verwaltung](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Diese Verwaltungsdienste wurden in der Cloud entwickelt. Sie werden vollständig in Azure gehostet, sodass sie keine Ressourcen vor Ort bereitstellen und verwalten müssen. Der Konfigurationsaufwand ist minimal, und die Lösung ist innerhalb weniger Minuten einsatzbereit.

Platzieren Sie einfach einen Agent auf einem beliebigen Windows- oder Linux-Computer in Ihrem Datencenter, und er sendet Daten an Log Analytics. Dort können diese zusammen mit allen anderen Daten, die aus cloudbasierten oder lokalen Diensten gesammelt werden, analysiert werden. Verwenden Sie Azure Backup und Azure Site Recovery, um die Cloud für die Sicherung und Hochverfügbarkeit lokaler Ressourcen zu nutzen.

![Verwaltungsdienste im Azure-Dashboard](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Runbooks in der Cloud können in der Regel nicht auf lokale Ressourcen zugreifen. Sie können jedoch einen Agent auf mindestens einem Computer installieren, um Runbooks in Ihrem Datencenter zu hosten. Geben Sie beim Starten eines Runbooks an, ob es in der Cloud oder für einen lokalen Worker ausgeführt werden soll.

Von Microsoft und von Partnern werden verschiedene Lösungen angeboten, die Sie Ihrem Azure-Abonnement hinzufügen können, um so den Nutzen Ihrer Log Analytics-Investition zu erhöhen. Azure bietet z.B. [Verwaltungslösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions). Dies sind vorgefertigte Logiksätze zur Implementierung eines Verwaltungsszenarios unter Verwendung mindestens eines Verwaltungsdiensts.

![Katalog der Verwaltungslösungen in Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Partner können eigene Lösungen zur Unterstützung ihrer Anwendungen und Dienste erstellen und sie Benutzern über Azure Marketplace oder über Schnellstartvorlagen zur Verfügung stellen.

## <a name="performance-alerting-and-monitoring"></a>Warnungen und Überwachung der Leistung

### <a name="alerting"></a>Warnungen

Warnungen sind eine Methode zum Überwachen von Azure-Ressourcenmetriken, -Ereignissen oder -Protokollen. Sie benachrichtigen Sie, wenn eine von Ihnen angegebene Bedingung erfüllt ist.

Warnungen sind für verschiedene Dienste verfügbar, z.B. für die folgenden:

- **Azure Application Insights**: Ermöglicht Webtest- und Metrikwarnungen. Weitere Informationen dazu finden Sie unter [Einrichten von Warnungen in Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) und [Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Log Analytics**: Ermöglicht die Weiterleitung von Aktivitäts- und Diagnoseprotokollen an Log Analytics. Sie können Metrik-, Protokoll- und andere Warnungstypen festlegen. Weitere Informationen finden Sie unter [Warnungen in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Azure Monitor**: Ermöglicht das Festlegen von Warnungen basierend auf Metrikwerten und Aktivitätsprotokollereignissen. Mit der [Azure Monitor-REST-API](https://msdn.microsoft.com/library/dn931943.aspx) können Sie Benachrichtigungen verwalten. Weitere Informationen finden Sie unter [Nutzung des Azure-Portals, von PowerShell oder der Befehlszeilenschnittstelle zum Erstellen von Warnungen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Überwachung

Leistungsprobleme in Ihrer Cloud-App können Ihr Unternehmen beeinträchtigen. Durch mehrere miteinander verbundene Komponenten und häufige Versionswechsel können Leistungseinbußen jederzeit vorkommen. Bei der Entwicklung einer App werden von Ihren Benutzern in der Regel Probleme entdeckt, die Sie beim Testen nicht gefunden haben. Sie sollten sofort über diese Probleme Bescheid wissen und über Tools zu deren Diagnose und Beseitigung verfügen.

Eine Reihe von Tools ist zum Überwachen von Azure-Anwendungen und -Diensten verfügbar. Einige ihrer Funktionen überschneiden sich. Dies liegt teils an der verschwommenen Grenze zwischen der Entwicklung und dem Betrieb einer Anwendung.

Hier sind die wichtigsten Tools aufgeführt:

- **Azure Monitor** ist ein grundlegendes Tool zum Überwachen von Diensten, die in Azure ausgeführt werden. Dieses Tool bietet Ihnen Daten auf Infrastrukturebene über den Durchsatz eines Diensts und über dessen Umgebung. Wenn Sie Ihre Apps vollständig in Azure verwalten und entscheiden müssen, ob Sie Ihre Ressourcen zentral hoch- oder herunterskalieren, bietet Azure Monitor den richtigen Ausgangspunkt.

- **Application Insights** kann für die Entwicklung und als Überwachungslösung für die Produktion verwendet werden. Dieses Tool installiert ein Paket in Ihrer App und bietet Ihnen damit eine interne Ansicht der Geschehnisse. Seine Daten umfassen die Antwortzeiten von Abhängigkeiten, Ausnahmeablaufverfolgungen, Debuggingmomentaufnahmen und Ausführungsprofilen. Es stellt Ihnen Tools zum Analysieren all dieser Telemetriedaten bereit, um Sie beim Debuggen einer App zu unterstützen und zu veranschaulichen, wie sie von den Benutzern genutzt wird. Sie können feststellen, ob eine Spitze in den Antwortzeiten auf ein Problem in einer App oder auf ein externes Ressourcenproblem zurückzuführen ist. Wenn Sie Visual Studio verwenden und die App fehlerhaft ist, können Sie direkt zu den problematischen Codezeilen gelangen und das Problem beheben.

- **Log Analytics** ist für Personen bestimmt, die die Leistung optimieren und die Wartung für Anwendungen planen müssen, die in der Produktionsumgebung ausgeführt werden. Log Analytics erfasst und aggregiert Daten aus vielen verschiedenen Quellen mit einer Verzögerung von 10 bis 15 Minuten. Es bietet eine ganzheitliche IT-Verwaltungslösung für Azure-Infrastrukturen, lokale Infrastrukturen und cloudbasierte Drittanbieterinfrastrukturen (z.B. Amazon Web Services). Log Analytics stellt Tools zur quellenübergreifenden Datenanalyse bereit, erlaubt komplexe Abfragen in allen Protokollen und kann beim Eintreten angegebener Bedingungen proaktiv Warnungen ausgeben. Sie können sogar benutzerdefinierte Daten im zugehörigen zentralen Repository sammeln, um sie anschließend abzufragen und zu visualisieren.

- **System Center Operations Manager** dient zur Verwaltung und Überwachung großer Cloudinstallationen. Möglicherweise kennen Sie dieses Tool bereits als Verwaltungstool für lokale Windows Server- und Hyper-V-basierte Clouds. Es kann jedoch auch in Azure-Apps integriert werden und diese verwalten. Unter anderem kann es Application Insights in vorhandenen Live-Apps installieren. Wenn eine App ausfällt, weist Sie Operations Manager innerhalb von Sekunden darauf hin.


## <a name="next-steps"></a>Nächste Schritte

- [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Beispiele für das Implementieren von Azure-Abonnementgovernance](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/)

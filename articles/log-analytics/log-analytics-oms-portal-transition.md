---
title: Wechsel des OMS-Portals zu Azure | Microsoft-Dokumentation
description: Das OMS-Portal hat das Ende seines Lebenszyklus erreicht und wechselt mit seiner gesamten Funktionalität zum Azure-Portal. Dieser Artikel enthält Details zu diesem Übergang.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 1a8ccc818cafac4867cb533c83f297af61a21836
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309101"
---
# <a name="oms-portal-moving-to-azure"></a>Wechsel des OMS-Portals zu Azure
Das Feedback von Log Analytics-Kunden hat wiederholt gezeigt, dass eine einzige Benutzeroberfläche zum Überwachen und Verwalten von lokalen und Azure-Workloads benötigt wird. Wie Sie wahrscheinlich wissen, dient das Azure-Portal als Hub für alle Azure-Dienste und bietet eine umfassende Verwaltungsoberfläche mit Funktionen wie Dashboards für das Anheften von Ressourcen, intelligente Suche nach Ressourcen und Tagging für die Ressourcenverwaltung. Zum Konsolidieren und Optimieren des Überwachungs- und Verwaltungsworkflows haben wir damit begonnen, die Funktionen des OMS-Portals in das Azure-Portal einzubinden. Wir freuen uns, Ihnen mitteilen zu können, dass die meisten Features des OMS-Portals nun Bestandteil des Azure-Portals sind. Tatsächlich sind einige der neuen Features, wie z.B. Traffic Analytics, nur im Azure-Portal verfügbar. Es sind nur noch wenige Lücken zu schließen, darunter einige Lösungen, die sich derzeit noch in der Übergangsphase zum Azure-Portal befinden. Sofern Sie nicht gerade diese Features verwenden, können Sie alles, was Sie bisher im OMS-Portal erledigt haben, nun mit dem Azure-Portal ausführen – und dazu noch vieles mehr. Wenn Sie das Azure-Portal noch nicht verwenden, empfehlen wir Ihnen, noch heute damit zu beginnen! 

Wir gehen davon aus, dass die verbleibenden Lücken zwischen den beiden Portalen bis August 2018 geschlossen sind. Aufgrund von Kundenfeedback teilen wir den zeitlichen Ablauf für das Lebenszyklusende des OMS-Portals mit. Wir freuen uns auf den Wechsel zum Azure-Portal und gehen davon aus, dass der Übergang reibungslos abläuft. Wir wissen aber, dass Änderungen schwierig sind und Störungen verursachen können. Senden Sie Fragen, Feedback oder Bedenken an **LAUpgradeFeedback@microsoft.com**. Im weiteren Verlauf dieses Artikels werden die wichtigsten Szenarien, die derzeitigen Lücken sowie die Roadmap für diesen Übergang beschrieben. 

## <a name="progress"></a>Status
Im Folgenden sind Updates aufgeführt, die seit früheren Versionen dieses Artikels abgeschlossen wurden.

### <a name="july-27"></a>27. Juli

- Die [DNS-Analyse](log-analytics-dns.md) ist jetzt im Azure-Portal voll funktionsfähig.
- Die [Updateverwaltung](../automation/automation-update-management.md) wurde aktualisiert und ist im Azure-Portal voll funktionsfähig. Weitere Informationen finden Sie unter [Migrieren von OMS-Updatebereitstellungen zu Azure](../automation/migrate-oms-update-deployments.md).
- [Warnungen](#changes-to-alerts) wurden jetzt vollständig in das Azure-Portal erweitert.
- [Das Vorschaufeature für benutzerdefinierte Protokolle](log-analytics-data-sources-custom-logs.md) ist jetzt für alle Arbeitsbereiche automatisch aktiviert.

## <a name="what-will-change"></a>Was ändert sich? 
Die folgenden Änderungen werden mit der Einstellung des OMS-Portals angekündigt. Die einzelnen Änderungen werden in den nachstehenden Abschnitten ausführlicher erläutert.

- Sie können nur im Azure-Portal neue Arbeitsbereiche erstellt.
- Die neue Oberfläche für die Warnungsverwaltung ersetzt die Warnungsverwaltungslösung (Alert Management).
- Die Benutzerzugriffsverwaltung erfolgt im Azure-Portal anhand der rollenbasierten Zugriffssteuerung von Azure.
- Der Application Insights-Connector ist nicht mehr erforderlich, da die gleiche Funktionalität über arbeitsbereichsübergreifende Abfragen ermöglicht wird.
- Die mobile OMS-App wird nicht mehr verwendet. 
- Die NSG-Lösung wird durch verbesserte Funktionalität ersetzt, die über die Traffic Analytics-Lösung bereitsteht.
- Neue Verbindungen von System Center Operations Manager mit Log Analytics erfordern aktualisierte Management Packs.


## <a name="current-known-gaps"></a>Derzeitige bekannte Lücken 
Es bestehen derzeit einige Funktionalitätslücken, die eine weitere Verwendung des OMS-Portals erforderlich machen. Diese Lücken werden geschlossen, und dieses Dokument wird dann entsprechend aktualisiert. Sie sollten auch unter den [Azure-Updates](https://azure.microsoft.com/updates/?product=log-analytics) auf weitere Ankündigungen zu Erweiterungen und Änderungen achten.

- Die folgenden Lösungen sind im Azure-Portal noch nicht voll funktionsfähig. Sie sollten diese Lösungen weiterhin im klassischen Portal verwenden, bis sie aktualisiert sind.

    - Windows-Analyselösungen ([Upgradebereitschaft](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [Integrität für Geräte](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) und [Updatekonformität](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Für den Zugriff auf die Log Analytics-Ressource in Azure muss der Benutzer Zugriff über den [rollenbasierten Zugriff in Azure](#user-access-and-role-migration) erhalten.


## <a name="what-should-i-do-now"></a>Wie soll ich jetzt vorgehen?  
Lesen Sie [Häufig gestellte Fragen zum Übergang vom OMS-Portal zum Azure-Portal für Log Analytics-Benutzer](../log-analytics/log-analytics-oms-portal-faq.md), um Informationen zum Übergang zum Azure-Portal zu erhalten. Falls die [oben beschriebenen Lücken](#current-known-gaps) auf Ihre Umgebung nicht zutreffen, sollten Sie damit beginnen, das Azure-Portal als primäre Benutzeroberfläche zu verwenden. Senden Sie Feedback, Fragen oder Bedenken an LAUpgradeFeedback@microsoft.com.

## <a name="new-workspaces"></a>Neue Arbeitsbereiche
Ab dem 29. Juli können Sie keine neuen Arbeitsbereiche mehr über das OMS-Portal erstellen. Folgen Sie den Anweisungen unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](log-analytics-quick-create-workspace.md), um einen neuen Arbeitsbereich im Azure-Portal zu erstellen.

## <a name="changes-to-alerts"></a>Änderungen bei Warnungen 

### <a name="alert-extension"></a>Warnungserweiterung  

> [!NOTE]
> Warnungen wurden jetzt vollständig in das Azure-Portal erweitert. Vorhandene Warnungsregeln können im OMS-Portal angezeigt werden, aber sie können nur im Azure-Portal verwaltet werden.

Warnungen werden derzeit [in das Azure-Portal erweitert](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Sobald dieser Prozess abgeschlossen ist, sind Verwaltungsaktionen für Warnungen nur noch im Azure-Portal verfügbar. Vorhandene Warnungen werden weiterhin im OMS-Portal aufgeführt. Wenn Sie programmgesteuert mithilfe der Log Analytics-REST-API für Warnungen oder der Log Analytics-Warnungsressourcenvorlage auf Warnungen zugreifen, müssen Sie in Ihren API-Aufrufen, Azure Resource Manager-Vorlagen und PowerShell-Befehlen anstelle von Aktionen nun Aktionsgruppen verwenden.

### <a name="alert-management-solution"></a>Lösung „Alert Management“
Anstelle der [Warnungsverwaltungslösung](log-analytics-solution-alert-management.md) (Alert Management) können Sie die [einheitliche Schnittstelle für Warnungen von Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) zur visuellen Darstellung und Verwaltung von Warnungen verwenden. Diese neue Oberfläche aggregiert Warnungen aus mehreren Quellen in Azure, einschließlich Protokollwarnungen aus Log Analytics. Sie können Verteilungen der Warnungen anzeigen, die automatisierte Gruppierung zusammengehöriger Warnungen mithilfe intelligenter Gruppen nutzen und Warnungen für mehrere Abonnements unter Verwendung umfangreicher Filter anzeigen. Alle diese Funktionen sind in der Vorschau ab 4. Juni 2018 verfügbar. Die Warnungsverwaltungslösung steht im Azure-Portal nicht zur Verfügung. 

Die von der Warnungsverwaltungslösung (Alert Management) gesammelten Daten (Datensätze vom Typ „Warnung“) sind weiterhin in Log Analytics enthalten, solange die Lösung für den Arbeitsbereich installiert ist. Ab August 2018 ist das Streaming von Warnungen von der einheitlichen Oberfläche für Warnungen in die Arbeitsbereiche aktiviert und ersetzt diese Funktion. Es werden einige Schemaänderungen erwartet und zu einem späteren Zeitpunkt bekannt gegeben.

## <a name="user-access-and-role-migration"></a>Benutzerzugriff und Rollenmigration
Die Zugriffsverwaltung des Azure-Portals ist umfangreicher und leistungsfähiger als die Zugriffsverwaltung im OMS-Portal, doch sind einige Konvertierungen erforderlich. Ausführliche Informationen zur Zugriffsverwaltung in Log Analytics finden Sie unter [Verwalten von Arbeitsbereichen](log-analytics-manage-access.md#manage-accounts-and-users).

Ab dem 30. Juni wird mit der automatischen Konvertierung der Zugriffssteuerungsberechtigungen des OMS-Portals in Azure-Portalberechtigungen begonnen. Nach Abschluss der Konvertierung werden Benutzer über den Abschnitt „Benutzerverwaltung“ des OMS-Portals an „Zugriffssteuerung (IAM)“ in Azure weitergeleitet. 

Während der Konvertierung prüft das System jede Benutzer- oder Sicherheitsgruppe, die Berechtigungen im OMS-Portal aufweist, und ermittelt, ob dieselbe Stufe bzw. dieselben Berechtigungen in Azure vorliegen. Wenn Berechtigungen fehlen, werden die folgenden Rollen für die jeweiligen Arbeitsbereiche und Lösungen zugewiesen.

| OMS-Portalberechtigung | Azure-Rolle |
|:---|:---|
| ReadOnly | Log Analytics-Leser |
| Mitwirkender | Log Analytics-Mitwirkender |
| Administrator | Owner (Besitzer) |

Um sicherzustellen, dass Benutzern keine zu hohen Berechtigungen zugewiesen werden, weist das System diese Berechtigungen auf Ressourcengruppenebene nicht automatisch zu. Daher müssen Arbeitsbereichsadministratoren sich selbst manuell die Rollen _Besitzer_ oder _Mitwirkender_ auf Ressourcengruppen- oder Abonnementebene zuweisen, um die folgenden Aktionen ausführen zu können.

- Hinzufügen oder Entfernen von Lösungen
- Definieren neuer benutzerdefinierter Ansichten
- Warnungen verwalten 

In einigen Fällen kann bei der automatischen Konvertierung keine Berechtigung angewendet werden, und der Administrator wird aufgefordert, Berechtigungen manuell zuzuweisen.

## <a name="oms-mobile-app"></a>Mobile OMS-App
Der Lebenszyklus der mobilen OMS-App endet zusammen mit dem OMS-Portal. Statt die mobile OMS-App zu verwenden, können Sie für den Zugriff auf Informationen über Ihre IT-Infrastruktur, Dashboards und gespeicherte Abfragen direkt über den Browser auf Ihrem mobilen Gerät auf das Azure-Portal zugreifen. Um Warnungen zu erhalten, sollten Sie [Azure-Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md) konfigurieren, damit Sie Benachrichtigungen in Form einer SMS oder eines Sprachanrufs empfangen.

## <a name="application-insights-connector-and-solution"></a>Application Insights-Connector und -Lösung
[Application Insights-Connector](log-analytics-app-insights-connector.md) bietet eine Möglichkeit, Application Insights-Daten in einen Log Analytics-Arbeitsbereich einzufügen. Diese Datenduplizierung war für die Sichtbarkeit in der gesamten Infrastruktur und für alle Anwendungsdaten erforderlich.

Aufgrund der Unterstützung [ressourcenübergreifender Abfragen](log-analytics-cross-workspace-search.md) ist diese Datenduplizierung nicht mehr erforderlich. Daher wird die vorhandene Application Insights-Lösung nicht mehr verwendet. Ab Juli können Sie keine neuen Application Insights-Ressourcen mehr mit Log Analytics-Arbeitsbereichen verknüpfen. Vorhandene Verknüpfungen und Dashboards funktionieren weiterhin bis November 2018.


## <a name="azure-network-security-group-analytics"></a>Azure-Netzwerksicherheitsgruppen-Analyse
Die [Analyselösung der Azure-Netzwerksicherheitsgruppe](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) wird durch die kürzlich gestartete [Traffic Analytics](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/)-Lösung ersetzt, die Einblicke in die Benutzer- und Anwendungsaktivität in Cloudnetzwerken bietet. Traffic Analytics erleichtert es Ihnen, die Netzwerkaktivität Ihres Unternehmens zu überwachen, Anwendungen und Daten zu schützen, die Arbeitsauslastungsleistung zu optimieren und die Kompatibilität aufrechtzuerhalten. 

Diese Lösung analysiert NSG-Datenflussprotokolle und bietet folgende Einblicke.

- Datenverkehrsflüsse über Ihre Netzwerke zwischen Azure und Internet, öffentliche Cloudregionen, VNETs und Subnetze
- Anwendungen und Protokolle in Ihrem Netzwerk, ohne die Notwendigkeit, Sniffer oder dedizierte Datenflusssammlung-Appliances einzusetzen
- Spitzennutzer, „geschwätzige“ Anwendungen, VM-Konversationen in der Cloud, Datenverkehrhotspots
- Quellen und Ziele des Datenverkehrs über VNETs, Beziehungen zwischen geschäftskritischen Diensten und Anwendungen
- Sicherheit, einschließlich schädlichen Datenverkehrs, zum Internet offenen Ports, Anwendungen oder VMs, die versuchen, auf das Internet zuzugreifen
- Kapazitätsauslastung, wodurch Sie Probleme einer Überbereitstellung oder Unterauslastung vermeiden

Sie können weiterhin Diagnoseeinstellungen verwenden, um NSG-Protokolle an Log Analytics zu senden, damit Ihre vorhandenen gespeicherten Suchen, Warnungen und Dashboards weiterhin funktionieren. Kunden, bei denen die Lösung bereits installiert ist, können diese bis auf Weiteres verwenden. Ab dem 15. August wird die Netzwerksicherheitsgruppen-Analyselösung aus dem Marketplace entfernt und über die Community als eine [Azure-Schnellstartvorlage](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights) zur Verfügung gestellt.

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Wenn Sie [Ihre Operations Manager-Verwaltungsgruppe mit Log Analytics verbunden haben](log-analytics-om-agents.md), funktioniert sie weiterhin ohne Änderungen. Für neue Verbindungen müssen Sie jedoch die Anweisungen in [Microsoft System Center Operations Manager Management Pack to configure Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/) (Microsoft System Center Operations Manager Management Pack zum Konfigurieren der Operations Management Suite) befolgen.

## <a name="next-steps"></a>Nächste Schritte
- Einen Leitfaden für den Wechsel vom OMS-Portal zum Azure-Portal finden Sie unter [Häufig gestellte Fragen zum Übergang vom OMS-Portal zum Azure-Portal für Log Analytics-Benutzer](log-analytics-oms-portal-faq.md).
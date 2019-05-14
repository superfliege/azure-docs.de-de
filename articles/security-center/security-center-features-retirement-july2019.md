---
title: Auslaufen von Security Center-Funktionen (Juli 2019) | Microsoft-Dokumentation
description: In diesem Artikel werden die Funktionen in Security Center erläutert, die am 31. Juli 2019 eingestellt werden.
services: security-center
author: yoavfrancis
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.date: 4/16/2019
ms.author: yoafr
ms.openlocfilehash: ac677a7f9586dbe893cbcbc480564aceac0149a0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147925"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Auslaufen von Security Center-Funktionen (Juli 2019)

Wir haben in den letzten sechs Monaten verschiedene [Verbesserungen](https://azure.microsoft.com/updates/?product=security-center) am Azure Security Center vorgenommen.  
Mit Verbesserung der Funktionen werden am 31. July 2019 eine Reihe redundanter Features sowie zugehörige APIs aus dem Security Center entfernt.  

Die meisten eingestellten Features können durch neue Features in Azure Security Center oder Log Analytics ersetzt werden; einige wenige Features können mit [Azure Sentinel (Vorschau)](https://azure.microsoft.com/services/azure-sentinel/) implementiert werden, das kürzlich angekündigt wurde.

Die folgenden Features werden aus dem Security Center entfernt:

- [Ereignisdashboard](#menu_events)
- [Menüeintrag „Search“](#menu_search)
- [Link „Identität und Zugriff (klassisch) anzeigen“ in Identität und Zugriff (Vorschau)](#menu_classicidentity)
- [Schaltfläche „Sicherheit Ereignisse Karte“ in Sicherheitshinweise zuordnen (Vorschau)](#menu_securityeventsmap)
- [Benutzerdefinierte Warnungsregeln (Vorschau)](#menu_customalerts)
- [Schaltfläche „Untersuchen“ in Bedrohungsschutz-Sicherheitswarnungen](#menu_investigate)
- [Teile der Sicherheitslösungen](#menu_solutions)
- [Bearbeiten von Sicherheitskonfigurationen für Sicherheitsrichtlinien](#menu_securityconfigurations)
- [Dashboard Sicherheit und Überwachung (ursprünglich im OMS-Portal) für Log Analytics-Arbeitsbereiche](#menu_securityomsdashboard)

Im Folgenden erhalten Sie ausführliche Informationen zu jedem eingestellten Feature sowie zu den Schritten, die Sie mit den Ersatzfeatures ausführen können.

## Ereignisdashboard<a name="menu_events"></a>
Security Center nutzt Microsoft Monitoring Agent für die Erfassung verschiedener sicherheitsbezogener Konfigurationen und Ereignisse auf Ihren Computern und speichert diese Ereignisse in Ihren Arbeitsbereichen. Im [Ereignisdashboard](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) können diese Daten angezeigt werden; es stellt im Wesentlichen einen weiteren Einstiegspunkt in Log Analytics dar.

Künftig wird das Ereignisdashboard eingestellt:

![Auswahlbildschirm für Ereignisarbeitsbereiche][1]

Das Ereignisdashboard, das angezeigt wird, wenn ein Benutzer auf einen Arbeitsbereich klickt, wird ebenfalls eingestellt:

![Ereignisdashboard][2]

### <a name="events-dashboard---new-experience"></a>Ereignisdashboard – Neue Benutzeroberfläche

Kunden wird empfohlen, die systemeigenen Funktionen von Log Analytics zu verwenden, um relevante Ereignisse in ihren Arbeitsbereichen anzuzeigen.
Wenn Sie bereits benutzerdefinierte relevante Ereignisse in Security Center erstellt haben, sind diese zugänglich über „Protokollanalyse“ > Arbeitsbereich auswählen > „Gespeicherte Suchvorgänge“. Ihre Daten gehen nicht verloren und bleiben unverändert erhalten. Auf demselben Bildschirm sind auch systemeigene relevante Ereignisse verfügbar.

![Gespeicherte Suchvorgänge – Arbeitsbereich][3]

## Menüeintrag „Search“<a name="menu_search"></a>
Azure Security Center verwendet derzeit die Azure Monitor-Protokollsuche, um Ihre Sicherheitsdaten abzurufen und zu analysieren. Dieser Bildschirm fungiert im Wesentlichen als Einstieg in die Seite „[Search](https://docs.microsoft.com/azure/security-center/security-center-search)“ von Log Analytics, auf der Benutzer Suchabfragen zu ihrem ausgewählten Arbeitsbereich ausführen können. Künftig wird dieses Fenster „Search“ eingestellt:

![Seite „Search“][4]

### <a name="search-menu-entry---new-experience"></a>Menüeintrag „Search“ – neue Benutzeroberfläche

Kunden wird empfohlen, die systemeigenen Funktionen von **Log Analytics** zum Ausführen von Suchabfragen für ihre Arbeitsbereiche zu nutzen. Hierfür können sie Log Analytics in Azure aufrufen und Folgendes auswählen: „Protokolle“:

![Seite „Protokolle“ von Log Analytics][5]

## Identität und Zugriff (klassisch) (Vorschau)<a name="menu_classicidentity"></a>
Die klassische Benutzeroberfläche für „Identität und Zugriff“ in Security Center bot Kunden die Möglichkeit, ihre Identitäts- und Zugriffsinformationen in Log Analytics einzusehen. Dazu wurde Folgendes ausgewählt:

Auf „View classic Identity & Access“ (Identität und Zugriff (klassisch) anzeigen) klicken

![Identitätsseite][6]

Zusammen mit diesem Bildschirm wird diese Seite geöffnet – „Dashboard für Identität und Zugriff“:

![Identitätsseite – Arbeitsbereichsauswahl][7]

Durch Klicken auf den Arbeitsbereich wird das Dashboard für Identität und Zugriff von Log Analytics geöffnet, in dem Benutzer Identitäts- und Zugriffsinformationen zu ihrem Arbeitsbereich einsehen können:

![Identitätsseite – Dashboard][8]

Künftig werden alle drei obigen Bildschirme eingestellt. Ihre Daten sind weiterhin in der Log Analytics-Sicherheitslösung verfügbar und werden weder geändert noch entfernt.

### <a name="classic-identity--access-preview---new-experience"></a>Identität und Zugriff (klassisch) (Vorschau) – Neue Benutzeroberfläche
Während das Log Analytics-Dashboard lediglich Informationen zu einem bestimmten Dashboard bereitstellte, liefern die systemeigenen Security Center-Funktionen Erkenntnisse zu allen Abonnements und alle zugeordneten Arbeitsbereichen. Die Ansicht ist benutzerfreundlich, und Sie können sich auf die wichtigen Aspekte konzentrieren, entsprechend der Sicherheitsbewertung Ihrer Identitäts- und Sicherheitsempfehlungen.
Alle Features des Dashboards „Identität und Zugriff“ von Log Analytics können durch Auswählen von „Identität und Zugriff (Vorschau)“ in Security Center aufgerufen werden:

![Identitätsseite – Einstellung der klassischen Benutzeroberfläche][9]

## Karte von Sicherheitsereignissen<a name="menu_securityeventsmap"></a>
Security Center bietet Ihnen eine [Karte](https://docs.microsoft.com/azure/security-center/security-center-threat-intel), die Ihnen das Identifizieren von Sicherheitsrisiken in der Umgebung erleichtert. Wenn Sie auf die Schaltfläche „Klicken Sie auf Sicherheit Ereignisse Karte“ klicken, gelangen Sie zu einem Dashboard, in dem Sie Rohdaten zu Sicherheitsereignissen für den ausgewählten Arbeitsbereich einsehen können.
Bei Einstellung wird die Schaltfläche zusammen mit dem arbeitsbereichsspezifischen Dashboard entfernt.

![Karte mit Sicherheitswarnungen – Schaltfläche][10]

Derzeit wird beim Klicken auf die Schaltfläche „Klicken Sie auf Sicherheit Ereignisse Karte“ das Dashboard „Informationen zu Bedrohungen“ geöffnet. Das Dashboard „Informationen zu Bedrohungen“ wird eingestellt.  

![Dashboard „Informationen zu Bedrohungen“][11]

Wenn Sie einen Arbeitsbereich auswählen, um das zugehörige Dashboard „Informationen zu Bedrohungen“ aufzurufen, wird der Bildschirm mit der Karte für Sicherheitswarnungen (Vorschau) *in Log Analytics* geöffnet. Dieser Bildschirm wird eingestellt.

![Karte mit Sicherheitswarnungen in Log Analytics][12]

Ihre vorhandenen Daten sind weiterhin in der Log Analytics-Sicherheitslösung verfügbar und werden weder geändert noch entfernt.

### <a name="security-events-map---new-experience"></a>Karte mit Sicherheitsereignissen – Neue Benutzeroberfläche
Wir empfehlen unseren Kunden die in Security Center integrierte Karte mit Sicherheitswarnungen zu verwenden – „Sicherheitshinweise zuordnen (Vorschau)“. Dies bietet eine optimierte Erfahrung und funktioniert über alle Abonnements und zugehörigen Arbeitsbereiche hinweg, sodass Sie eine Makroübersicht über Ihre Umgebung erhalten und sich nicht auf einen einzigen Arbeitsbereich beschränken müssen.

## Benutzerdefinierte Warnungsregeln (Vorschau)<a name="menu_customalerts"></a>
Die Funktion der benutzerdefinierten Warnungen wird am 30. Juni 2019 aufgrund der Außerbetriebnahme der zugrunde liegenden Infrastruktur [eingestellt](https://docs.microsoft.com/azure/security-center/security-center-custom-alert). Im Zeitraum bis zur Einstellung können Benutzer vorhandene benutzerdefinierte Warnungsregeln bearbeiten, aber keine neuen Regeln mehr hinzufügen. Benutzern wird empfohlen, [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) mit Ein-Klick-Onboarding zu aktivieren, damit ihre vorhandenen Warnungen automatisch migriert und neue erstellt werden, oder alternativ ihre Warnungen mit Azure Monitor-Protokollwarnungen neu zu erstellen.

Wenn Sie Ihre vorhandenen Warnungen beibehalten und nach Azure Sentinel migrieren möchten, starten Sie Azure Sentinel. Wählen Sie als ersten Schritt den Arbeitsbereich aus, in dem Ihre benutzerdefinierten Warnungen gespeichert sind. Wählen Sie dann das Menüelement „Analytics“ aus, damit Ihre Warnungen automatisch migriert werden.

![Benutzerdefinierte Warnungen][13]

Kunden, die an einem Einstieg in Azure Sentinel nicht interessiert sind, wird empfohlen, ihre Warnungen mit Azure Monitor-Protokollwarnungen zu erstellen. Eine Anleitung erhalten Sie unter  [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log). Anweisungen zum Erstellen von Protokollwarnungen finden Sie in folgendem Artikel: [Protokollwarnungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

Weitere Informationen zur Einstellung von benutzerdefinierten Warnungen finden Sie in der [Dokumentation zu benutzerdefinierten Warnungen in Security Center](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## Sicherheitswarnungen – Untersuchung<a name="menu_investigate"></a>
Mit dem [Untersuchungsfeature](https://docs.microsoft.com/azure/security-center/security-center-investigation) in Security Center können Sie einen potenziellen Sicherheitsvorfall selektieren, sich ein Bild vom Umfang machen und der Ursache auf den Grund gehen. Dieses Feature wird aus Security Center entfernt, da es durch eine verbesserte Funktion in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) ersetzt wurde.

![Sicherheitsvorfall][14]

Wenn Sie auf die obige Schaltfläche „Untersuchen“ klicken, wird das „Untersuchungsdashboard (Vorschau)“ in Log Analytics geöffnet. Das Untersuchungsdashboard wird eingestellt.  
Ihre vorhandenen Daten sind weiterhin in der Log Analytics-Sicherheitslösung verfügbar und werden weder geändert noch entfernt.

![Untersuchungsdashboard in Log Analytics][15]

### <a name="investigation---new-experience"></a>Untersuchung – neue Benutzeroberfläche

Wir empfehlen unseren Kunden in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) einzusteigen; damit erhalten Sie eine leistungsfähige Untersuchungsumgebung, der das Feature zum Aufspüren von Warnungen zugrunde liegt. Azure Sentinel bietet leistungsstarke Such- und Abfragetools zum Aufspüren von Sicherheitsbedrohungen in den Datenquellen Ihrer Organisation.  

## Teile der Sicherheitslösungen<a name="menu_solutions"></a>

Mit Security Center können Sie ganz einfach [integrierte Sicherheitslösungen in Azure](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) aktivieren. Die folgenden Partnerlösungen werden entfernt und nun in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) zusammen mit einer noch größeren Anzahl von Datenquellen unterstützt.

- Lösungen Firewall der nächsten Generation und Web Application Firewall (Azure Sentinel-[Dokumentation](https://docs.microsoft.com/azure/sentinel/connect-data-sources))
- Integration von Sicherheitslösungen, die das Common Event Format (CEF) unterstützen (Azure Sentinel-[Dokumentation](https://docs.microsoft.com/azure/sentinel/connect-common-event-format))
- Microsoft Advanced Threat Analytics (Azure Sentinel-[Dokumentation](https://docs.microsoft.com/azure/sentinel/connect-azure-atp))
- Azure AD Identity Protection (Azure Sentinel-[Dokumentation](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection))

Nach der Einstellung können Benutzer keine neuen verbundenen Lösungen der oben aufgeführten Typen mehr hinzufügen und keine solchen vorhandenen Lösungen mehr bearbeiten, weder über die Benutzeroberfläche noch über die API.
Die vorhandenen verbundenen Lösungen sollten bei Ablauf dieses Zeitraums nach Azure Sentinel verschoben werden.

![Security Center-Lösungen][16]

## Bearbeiten von Sicherheitskonfigurationen für Sicherheitsrichtlinien<a name="menu_securityconfigurations"></a>
Azure Security Center überwacht Sicherheitskonfigurationen durch Anwenden von [über 150 empfohlenen Regeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) für die Härtung des Betriebssystems, einschließlich Regeln für Firewalls, Überwachung, Kennwortrichtlinien und mehr. Wenn auf einem Computer eine angreifbare Konfiguration festgestellt wird, generiert Security Center eine Sicherheitsempfehlung. Im Bildschirm [Sicherheitskonfigurationen bearbeiten](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) können Benutzer die standardmäßige Sicherheitskonfiguration des Betriebssystems in Security Center anpassen.

Dies war ein Feature in der Vorschauphase, und es wird künftig eingestellt.

![Sicherheitskonfigurationen bearbeiten][17]

### <a name="edit-security-configurations---new-experience"></a>Sicherheitskonfigurationen bearbeiten – Neue Benutzeroberfläche

Security Center unterstützt in naher Zukunft den [Guest configuration agent](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) (Gastkonfigurations-Agent), und dies ermöglicht eine vielfältigere Palette von Features – u.a. die Unterstützung weiterer Betriebssysteme und die Integration von Gastkonfigurationen mit Azure-Richtlinien (In-Guest-Richtlinien). Damit werden auch die bedarfsgerechte Steuerung und das automatische Anwenden auf neue Ressourcen unterstützt.

## Dashboard Sicherheit und Überwachung (ursprünglich im OMS-Portal) für Log Analytics-Arbeitsbereiche<a name="menu_securityomsdashboard"></a>

Das Sicherheitsdashboard in Log Analytics bietet einen arbeitsbereichsspezifischen Überblick über relevante Sicherheitsereignisse und -risiken, eine Bedrohungsanalyse-Karte sowie Identitäts- und Zugriffsbewertung von Sicherheitsereignissen, die im Arbeitsbereich gespeichert sind. Das Dashboard wird in Zukunft entfernt. Wir haben bereits in der Benutzeroberfläche des Dashboards empfohlen, künftig Azure Security Center zu nutzen.

![Sicherheitsdashboard von Log Analytics][18]

### <a name="security--audit-dashboard---new-experience"></a>Sicherheits- und Überwachungsdashboard – Neue Benutzeroberfläche
Kunden wird empfohlen, Azure Security Center zu verwenden. Dieses bietet denselben Sicherheitsüberblick über mehrere zugeordnete Abonnements und Arbeitsbereiche sowie eine breitere Palette von Features.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/)
- Weitere Informationen zu [Azure Sentinel](https://docs.microsoft.com/azure/sentinel)

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png

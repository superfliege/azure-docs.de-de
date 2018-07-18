---
title: Was ist Azure Security Center?| Microsoft-Dokumentation
description: Sie erhalten Informationen zu Azure Security Center, zu dessen wichtigsten Funktionen und dazu, wie es funktioniert.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: terrylan
ms.openlocfilehash: 08102ce4caead003925aa600f4f7f005b1c336e0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
ms.locfileid: "29395648"
---
# <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Mit Security Center können Sie Sicherheitsrichtlinien für Ihre Workloads anwenden, die Angriffsfläche für Bedrohungen verringern sowie Angriffe erkennen und darauf reagieren.

Gründe für die Verwendung von Security Center

- **Zentrale Richtlinienverwaltung**: Stellen Sie die Einhaltung unternehmensspezifischer oder gesetzlicher Sicherheitsvorschriften sicher, indem Sie Sicherheitsrichtlinien für alle Hybridcloud-Workloads zentral verwalten.
- **Laufende Sicherheitsbewertung**: Überwachen Sie die Sicherheit von Computern, Netzwerken, Speicher- und Datendiensten und Anwendungen, um potenzielle Sicherheitsprobleme aufzudecken.
- **Umsetzbare Empfehlungen**: Beseitigen Sie mit priorisierten, direkt umsetzbaren Sicherheitsempfehlungen Sicherheitslücken, bevor sie von Angreifern ausgenutzt werden können.
- **Erweiterter Cloudschutz**: Reduzieren Sie Bedrohungen mithilfe eines Just-in-Time-Zugriffs auf Verwaltungsports und Whitelists, um Anwendungen zu steuern, die auf Ihren VMs ausgeführt werden.
- **Priorisierte Warnungen und Vorfälle**: Konzentrieren Sie sich mittels priorisierter Warnungen und Vorfälle auf die größten Bedrohungen.
- **Integrierte Sicherheitslösungen**: Sammeln, durchsuchen und analysieren Sie Sicherheitsdaten aus verschiedensten Quellen (einschließlich verbundener Partnerlösungen).

In der **Übersicht über Security Center** erhalten Sie einen kurzen Überblick über den Sicherheitsstatus Ihrer Azure-Workloads und Azure-fremden Workloads und können so die Sicherheit Ihrer Workloads ermitteln und bewerten sowie Risiken identifizieren und mindern. Dank integrierter Dashboards erhalten Sie sofort Einblick in Sicherheitswarnungen und -lücken, die Aufmerksamkeit erfordern.

![Übersicht][1]

## <a name="centralized-policy-management"></a>Zentrale Richtlinienverwaltung
Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei. In Security Center können Sie Richtlinien definieren und auf die Art der Workload oder Vertraulichkeit Ihrer Daten abstimmen.

Security Center-Richtlinien enthalten folgende Komponenten:

- **Datensammlung:** Bestimmt die Einstellungen für die Agent-Bereitstellung und [Sammlung von Sicherheitsdaten](security-center-enable-data-collection.md).
- **Sicherheitsrichtlinie:** Bestimmt, welche Steuerungsinstrumente Security Center überwacht und empfiehlt, indem die [Sicherheitsrichtlinie](security-center-policies.md) bearbeitet wird.
- **E-Mail-Benachrichtigungen:** Bestimmt Sicherheitskontakte und Einstellungen für [E-Mail-Benachrichtigungen](security-center-provide-security-contact-details.md).
- **Tarif:** Definiert die [Tarifauswahl](security-center-pricing.md) (Free oder Standard). Der gewählte Tarif beeinflusst, welche Security Center-Features für betroffene Ressourcen verfügbar sind.

![Sicherheitsrichtlinie][2]

Weitere Informationen finden Sie in der [Übersicht über Sicherheitsrichtlinien](security-center-policies-overview.md).

## <a name="continuous-security-assessment"></a>Laufende Sicherheitsbewertung
Security Center analysiert den Sicherheitsstatus Ihrer Computeressourcen, virtuellen Netzwerke, Speicher- und Datendienste sowie Anwendungen. Die laufende Bewertung hilft Ihnen dabei, potenzielle Sicherheitsprobleme zu erkennen, z.B. Systeme mit fehlenden Sicherheitsupdates oder ungeschützten Netzwerkports. Wählen Sie im Abschnitt „Prävention“ eine Kachel aus, um weitere Informationen anzuzeigen, einschließlich einer Liste mit Ressourcen und Sicherheitsrisiken, die erkannt wurden.

![Überwachung der Sicherheitsintegrität][3]

Weitere Informationen finden Sie unter [Überwachen der Sicherheitsintegrität](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Umsetzbare Empfehlungen
Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen und Azure-fremder Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Eine Liste mit priorisierten Sicherheitsempfehlungen führt Sie durch den Prozess der Behebung von Sicherheitsproblemen.

![Empfehlungen][4]

Weitere Informationen finden Sie unter [Verwalten von Sicherheitsempfehlungen](security-center-recommendations.md).

## <a name="just-in-time-vm-access"></a>Just-in-Time-VM-Zugriff
Verkleinern Sie die Angriffsfläche des Netzwerks mithilfe eines gesteuerten Just-in-Time-Zugriffs auf die Verwaltungsports virtueller Azure-Computer, um die Gefahr von Brute-Force- und anderen Netzwerkangriffen erheblich zu senken.

![Just-in-Time-VM-Zugriff][5]

Legen Sie Regeln fest, wie Benutzer eine Verbindung mit virtuellen Computern herstellen können. Bei Bedarf kann der Zugriff über das Security Center oder PowerShell angefordert werden. Solange die Anforderung den Regeln entspricht, wird der Zugriff für die angeforderte Zeit automatisch gewährt.

Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](security-center-just-in-time.md).

## <a name="adaptive-application-controls"></a>Adaptive Anwendungssteuerungen
Blockieren Sie Schadsoftware und andere unerwünschten Anwendungen mithilfe von Whitelist-Empfehlungen, die auf Ihre spezifischen Azure-Workloads abgestimmt sind und durch Machine Learning unterstützt werden.

![Adaptive Anwendungssteuerungen][6]

Überprüfen und klicken Sie, um die empfohlenen Whitelistregeln für Anwendungen anzuwenden, die vom Security Center generiert wurden, oder um bereits konfigurierte Regeln zu bearbeiten.

Weitere Informationen finden Sie unter [Adaptive Anwendungssteuerungen](security-center-adaptive-application.md).

## <a name="prioritized-alerts-and-incidents"></a>Priorisierte Warnungen und Vorfälle
Security Center nutzt fortschrittliche Analysefunktionen und globale Bedrohungsdaten, um eingehende Angriffe und Aktivitäten nach einem Angriff zu erkennen. Warnungen werden priorisiert und in Vorfälle gruppiert, sodass Sie sich auf die wichtigsten Bedrohungen konzentrieren können. Sie können auch eigene benutzerdefinierte Sicherheitswarnungen erstellen.

![Priorisierte Warnungen und Vorfälle][7]

Sie können Umfang und Auswirkungen eines Angriffs auf einer visuellen, interaktiven Untersuchungsoberfläche schnell einschätzen und vordefinierte oder Ad-hoc-Abfragen zur detaillierten Untersuchung von Sicherheitsdaten verwenden.

Weitere Informationen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen](security-center-managing-and-responding-alerts.md).

## <a name="integrate-your-security-solutions"></a>Integrieren Ihrer Sicherheitslösungen
Sie können in Security Center Sicherheitsdaten aus verschiedensten Quellen (einschließlich verbundener Partnerlösungen wie Netzwerkfirewalls und anderer Microsoft-Dienste) sammeln, durchsuchen und analysieren.

![Integrieren von Sicherheitslösungen][8]

Weitere Informationen finden Sie unter [Integrieren von Sicherheitslösungen](security-center-partner-integration.md).

## <a name="next-steps"></a>Nächste Schritte

- Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testabonnement](https://azure.microsoft.com/free/)registrieren.
- Der Security Center-Tarif „Free“ wird mit Ihrem Azure-Abonnement aktiviert. Um die Vorteile der erweiterten Funktionen für die Sicherheitsverwaltung und Erkennung von Bedrohungen nutzen zu können, müssen Sie ein Upgrade auf den Tarif „Standard“ durchführen. Der Standard-Tarif ist für die ersten 60 Tage kostenlos. Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).
- Wenn Sie Security Center Standard nun aktivieren möchten, erfahren Sie im [Schnellstart: Einbinden Ihres Azure-Abonnements in Azure Security Center](security-center-get-started.md), welche Schritte Sie ausführen müssen.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png

---
title: Was ist Azure Security Center?| Microsoft-Dokumentation
description: Sie erhalten Informationen zu Azure Security Center, zu dessen wichtigsten Funktionen und dazu, wie es funktioniert.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: bd0e517845b9cfcbe6090dff8d656edcca782c83
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126291"
---
# <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Mit Security Center können Sie Sicherheitsrichtlinien für Ihre Workloads anwenden, die Angriffsfläche für Bedrohungen verringern sowie Angriffe erkennen und darauf reagieren.

Gründe für die Verwendung von Security Center

- **Zentrale Richtlinienverwaltung**: Stellen Sie die Einhaltung unternehmensspezifischer oder gesetzlicher Sicherheitsvorschriften sicher, indem Sie Sicherheitsrichtlinien für alle Hybridcloud-Workloads zentral verwalten.
- **Laufende Sicherheitsbewertung**: Überwachen Sie den Sicherheitsstatus von Computern, Netzwerken, Speicher- und Datendiensten und Anwendungen, um potenzielle Sicherheitsprobleme aufzudecken.
- **Umsetzbare Empfehlungen**: Beseitigen Sie mit priorisierten, direkt umsetzbaren Sicherheitsempfehlungen Sicherheitslücken, bevor sie von Angreifern ausgenutzt werden können.
- **Priorisierte Warnungen und Vorfälle**: Konzentrieren Sie sich mittels priorisierter Warnungen und Vorfälle auf die größten Bedrohungen.
- **Erweiterter Cloudschutz**: Reduzieren Sie Bedrohungen mithilfe eines Just-in-Time-Zugriffs auf Verwaltungsports und eine adaptive Anwendungssteuerung für Ihre VMs.
- **Integrierte Sicherheitslösungen**: Sammeln, durchsuchen und analysieren Sie Sicherheitsdaten aus verschiedensten Quellen (einschließlich verbundener Partnerlösungen).

In der **Übersicht über Security Center** erhalten Sie einen kurzen Überblick über den Sicherheitsstatus Ihrer Azure-Workloads und Azure-fremden Workloads und können so die Sicherheit Ihrer Workloads ermitteln und bewerten sowie Risiken identifizieren und mindern. Dank integrierter Dashboards erhalten Sie sofort Einblick in Sicherheitswarnungen und -lücken, die Aufmerksamkeit erfordern.

![Übersicht][1]

## <a name="centralized-policy-management"></a>Zentrale Richtlinienverwaltung
Der (oben gezeigte) Abschnitt **Richtlinien und Konformität** bietet einen schnellen Überblick über Abonnementabdeckung, Richtlinienkonformität und Sicherheitsstatus.

### <a name="subscription-coverage"></a>Abonnementabdeckung
Dieser Abschnitt zeigt die Gesamtzahl der Abonnements, auf die Sie Zugriff besitzen (Lese- oder Schreibberechtigung) und die Security Center-Abdeckungsebene („Standard“ oder „Free“), mit der ein Abonnement ausgeführt wird:

- **Abgedeckt (Standard)**: Abgedeckte Abonnements werden mit der höchsten Schutzstufe ausgeführt, die von Security Center angeboten wird.
- **Abgedeckt (Free)**: Abgedeckte Abonnements werden mit der eingeschränkten kostenlosen Schutzstufe ausgeführt, die von Security Center angeboten wird.
- **Nicht abgedeckt**: Es wurden Abonnements erkannt, die nicht mit Azure Security Center überwacht werden.

Durch Auswahl des Diagramms wird das Fenster **Abdeckung** geöffnet. Bei Auswahl einer Registerkarte (**Nicht abgedeckt**, **Basic-Abdeckung** oder **Standard-Abdeckung**) wird eine Liste der Abonnements mit dem jeweiligen Status geöffnet. Durch Auswahl eines Abonnements unterhalb einer dieser Registerkarten erhalten Sie zusätzliche Informationen zu einem Abonnement. Anhand dieser Informationen können Sie den Besitzer eines Abonnements identifizieren und diesen kontaktieren, um Security Center zu aktivieren oder die Abdeckung für das Abonnement zu erhöhen.

![Security Center – Abdeckung][9]

### <a name="policy-compliance"></a>Richtlinienkonformität
Die Richtlinienkonformität wird über die Konformitätsfaktoren bestimmt, die allen Richtlinien zugewiesen sind. Die Gesamtbewertung der Konformität für eine Verwaltungsgruppe, ein Abonnement oder einen Arbeitsbereich ist der gewichtete Mittelwert der Zuweisungen. In den gewichteten Mittelwert werden die Anzahl von Richtlinien einer einzelnen Zuweisung und die Anzahl von Ressourcen einbezogen, für die die Zuweisung gilt.

Wenn Ihr Abonnement beispielsweise über zwei VMs und eine Initiative mit fünf zugewiesenen Richtlinien verfügt, weist Ihr Abonnement zehn Bewertungen auf. Falls eine der VMs die Anforderungen für zwei Richtlinien nicht erfüllt, beträgt die Gesamtbewertung für die Konformität der Abonnementzuweisung 80%.

In diesem Abschnitt werden die Gesamtkonformität und Abonnements mit der niedrigsten Konformität angezeigt. Durch Auswahl von **Richtlinienkonformität Ihrer Umgebung anzeigen** wird das Fenster **Richtlinienverwaltung** geöffnet. Im Fenster **Richtlinienverwaltung** wird die hierarchische Struktur von Verwaltungsgruppen, Abonnements und Arbeitsbereichen angezeigt. Hier verwalten Sie Ihre Sicherheitsrichtlinien, indem Sie ein Abonnement oder eine Verwaltungsgruppe auswählen.

![Richtlinienverwaltung][10]

Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei. In Security Center können Sie Richtlinien definieren und auf die Art der Workload oder Vertraulichkeit Ihrer Daten abstimmen sowie bestimmen, welche Steuerelemente Security Center überwacht und empfiehlt. Sie können die Sicherheitsrichtlinie in Security Center bearbeiten, indem Sie auf eine Verwaltungsgruppe oder ein Abonnement klicken. Sie können außerdem Azure Policy verwenden, um neue Definitionen zu erstellen, zusätzliche Richtlinien zu definieren und verwaltungsgruppenübergreifend Richtlinien zuzuweisen.

Wählen Sie **Einstellungen bearbeiten >** aus, um die folgenden Security Center-Einstellungen auf Abonnement-, Verwaltungsgruppen-, Ressourcengruppen- oder Arbeitsbereichsebene zu bearbeiten:

- **Datensammlung:** Bestimmt die Einstellungen für die Agent-Bereitstellung und [Sammlung von Sicherheitsdaten](security-center-enable-data-collection.md).
- **E-Mail-Benachrichtigungen:** Bestimmt Sicherheitskontakte und Einstellungen für [E-Mail-Benachrichtigungen](security-center-provide-security-contact-details.md).
- **Tarif:** Definiert die [Tarifauswahl](security-center-pricing.md) (Free oder Standard). Der gewählte Tarif beeinflusst, welche Security Center-Features für betroffene Ressourcen verfügbar sind.
- **Sicherheitskonfigurationen bearbeiten**: Hier können Sie die von Security Center bewerteten Betriebssystemkonfigurationen anzeigen und ändern, um potenzielle Sicherheitsrisiken zu identifizieren.

Weitere Informationen finden Sie unter [Integrieren von Security Center-Sicherheitsrichtlinien in Azure Policy](security-center-azure-policy.md).

### <a name="manage-and-govern-your-security-posture"></a>Verwalten und Steuern Ihres Sicherheitsstatus
Im rechten Bereich des Dashboards unter **Richtlinien und Konformität** werden Erkenntnisse angezeigt, anhand derer Sie umgehend den Gesamtsicherheitsstatus verbessern können. Beispiele:

- Definieren Sie Security Center-Richtlinien, und weisen Sie sie zu, um die Konformität mit Sicherheitsstandards zu prüfen und nachzuverfolgen.
- Stellen Sie Security Center-Warnungen für einen SIEM-Connector zur Verfügung.
- Richtlinienkonformität im zeitlichen Verlauf

## <a name="continuous-security-assessment"></a>Laufende Sicherheitsbewertung
Der Abschnitt zur Ressourcensicherheit unter **Security Center – Übersicht** bietet einen schnellen Überblick über die Sicherheit Ihrer Ressourcen, indem für jeden Ressourcentyp die Anzahl identifizierter Probleme und der Sicherheitszustand angezeigt wird. Die laufende Bewertung hilft Ihnen dabei, potenzielle Sicherheitsprobleme zu erkennen, z.B. Systeme mit fehlenden Sicherheitsupdates oder ungeschützten Netzwerkports.

### <a name="secure-score"></a>Sicherheitsbewertung
Die Sicherheitsbewertung in Azure Security Center zeigt Ihre Sicherheitsempfehlungen an und priorisiert diese für Sie. Auf diese Weise wissen Sie, welche Empfehlungen zuerst umgesetzt werden müssen. Außerdem erhalten Sie Unterstützung bei der Identifizierung der schwerwiegendsten Sicherheitsrisiken, sodass Sie deren Untersuchung priorisieren können. Die Sicherheitsbewertung ist ein Tool zum Verstärken der Sicherheit, sodass Sie sichere Workloads erzielen können. Weitere Informationen finden Sie unter [Sicherheitsbewertung in Azure Security Center](security-center-secure-score.md).

### <a name="health-monitoring"></a>Systemüberwachung
Bei Auswahl eines Ressourcentyps unter **Überwachung der Ressourcenintegrität** wird eine Liste der Ressourcen und potenzieller Sicherheitsrisiken angezeigt, die identifiziert wurden. Folgende Ressourcentypen stehen zur Auswahl: Compute und Apps, Netzwerk, Daten und Speicher, Identität und Zugriff.

Hier wurde **Compute und Apps** ausgewählt. **Compute** umfasst vier Registerkarten:

- **Übersicht:** Überwachung und von Security Center identifizierte Empfehlungen
- **VMs und Computer:** Liste Ihrer virtuellen und physischen Computer sowie deren aktueller Sicherheitsstatus
- **Clouddienste:** Liste mit Ihren von Security Center überwachten Web- und Workerrollen
- **App Services (Vorschau):** Liste Ihrer Webanwendungen und App Service-Umgebungen sowie deren jeweiliger aktueller Sicherheitsstatus.

![Überwachung der Sicherheitsintegrität][3]

Weitere Informationen finden Sie unter [Überwachen der Sicherheitsintegrität](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Umsetzbare Empfehlungen
Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen und Azure-fremder Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Durch Auswahl von **Empfehlungen** unter **Ressourcen** wird eine Liste mit priorisierten Sicherheitsempfehlungen geöffnet, die Sie durch den Prozess der Behebung von Sicherheitsproblemen leitet.

![Empfehlungen][4]

Weitere Informationen finden Sie unter [Verwalten von Sicherheitsempfehlungen](security-center-recommendations.md).

### <a name="most-prevalent-recommendations"></a>Am häufigsten auftretende Empfehlungen
Im rechten Bereich des Dashboards unter **Ressourcen** wird eine Liste der am häufigsten auftretenden Empfehlungen angezeigt, die für die meisten Ressourcen vorliegen. Die am häufigsten auftretenden Empfehlungen weisen darauf hin, worauf Sie Ihre Aufmerksamkeit lenken sollten. Durch Auswahl des nach rechts weisenden Pfeils wird die Empfehlung mit der stärksten Auswirkung angezeigt.

![Am häufigsten auftretende Empfehlungen][11]

Dies ist die Empfehlung mit der höchsten Auswirkung in Ihrer Umgebung. Durch Umsetzen dieser Empfehlung verbessern Sie die Konformität am meisten. In diesem Beispiel lautet die Empfehlung, die Datenträgerverschlüsselung anzuwenden. Durch Auswahl von **Konformität verbessern** wird eine Beschreibung der Empfehlung geöffnet und eine Liste der betroffenen Ressourcen angezeigt.

![Datenträgerverschlüsselung anwenden][12]

## <a name="threat-protection"></a>Bedrohungsschutz
Dieser Bereich bietet Einblick in die Sicherheitswarnungen, die für Ihre Ressourcen ermittelt wurden, und gibt den Schweregrad für diese Warnungen an.

### <a name="prioritized-alerts-and-incidents"></a>Priorisierte Warnungen und Vorfälle
Security Center nutzt fortschrittliche Analysefunktionen und globale Bedrohungsdaten, um eingehende Angriffe und Aktivitäten nach einem Angriff zu erkennen. Warnungen werden priorisiert und in Vorfälle gruppiert, sodass Sie sich auf die wichtigsten Bedrohungen konzentrieren können. Sie können auch eigene benutzerdefinierte Sicherheitswarnungen erstellen.

Durch Auswahl von **Sicherheitswarnungen nach Schweregrad** oder **Sicherheitswarnungen im zeitlichen Verlauf** erhalten Sie ausführliche Informationen zu den Warnungen.

![Priorisierte Warnungen und Vorfälle][7]

Sie können Umfang und Auswirkungen eines Angriffs auf einer visuellen, interaktiven Untersuchungsoberfläche schnell einschätzen und vordefinierte oder Ad-hoc-Abfragen zur detaillierten Untersuchung von Sicherheitsdaten verwenden.

Weitere Informationen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen](security-center-managing-and-responding-alerts.md).

Durch die Informationen im rechten Bereich des Dashboards erfahren Sie, welche Warnungen zuerst untersucht werden sollten und welche Sicherheitsrisiken in Ihrer Konfiguration am häufigsten auftreten:

- **Ressourcen mit den meisten Angriffen**: Zeigt Ressourcen mit der höchsten Anzahl von Warnungen. 
- **Häufigste Warnungen**: Warnungstypen, die die größte Anzahl von Ressourcen betreffen.

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
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
[9]: ./media/security-center-intro/coverage.png
[10]: ./media/security-center-intro/policy-management.png
[11]: ./media/security-center-intro/highest-impact.png
[12]: ./media/security-center-intro/apply-disk-encryption.png

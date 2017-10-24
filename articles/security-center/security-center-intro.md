---
title: "Einführung in Azure Security Center | Microsoft Docs"
description: Sie erhalten Informationen zu Azure Security Center, zu dessen wichtigsten Funktionen und dazu, wie es funktioniert.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: terrylan
ms.openlocfilehash: 21415af0d449d639d000e07afdb4de3680a64774
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-security-center"></a>Einführung in Azure Security Center
Sie erhalten Informationen zu Azure Security Center, zu dessen wichtigsten Funktionen und dazu, wie es funktioniert.

## <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center ermöglicht eine einheitliche Sicherheitsverwaltung und erweiterten Schutz vor Bedrohungen für Workloads, die in Azure, lokal und in anderen Clouds ausgeführt werden.  Security Center bietet Transparenz und Kontrolle für Hybridcloud-Workloads, aktive Abwehrmaßnahmen zur Verringerung Ihrer Anfälligkeit für Bedrohungen sowie eine intelligente Erkennung, damit Sie mit der schnellen Entwicklung bei Cyberangriffen Schritt halten können.

In der Übersicht von Security Center erhalten Sie einen kurzen Überblick über den Sicherheitsstatus Ihrer Azure-Workloads und Azure-fremden Workloads und können so die Sicherheit Ihrer Workloads ermitteln und bewerten sowie Risiken identifizieren und mindern.

![Übersicht](./media/security-center-intro/security-center-intro-fig1.png)

## <a name="why-use-security-center"></a>Gründe für die Verwendung von Security Center

**Einheitliche Sichtbarkeit und Steuerung**

- **Verstehen des Sicherheitsstatus hybrider Workloads**. Verwalten Sie die Sicherheit aller Hybridcloud-Workloads (lokale Clouds, Azure-Clouds und andere Cloudplattformen) über eine Konsole. Dank integrierter Dashboards erhalten Sie sofort Einblicke in Sicherheitsprobleme, die Ihre Aufmerksamkeit erfordern.
- **Einblick in die Cloudworkloads**. Halten Sie Schritt mit sich schnell ändernden Cloudworkloads. Erkennen und integrieren Sie automatisch neue Ressourcen, die in Ihren Azure-Abonnements erstellt wurden.
- **Zentrale Richtlinienverwaltung**. Stellen Sie die Einhaltung unternehmensspezifischer oder gesetzlicher Sicherheitsvorschriften sicher, indem Sie Sicherheitsrichtlinien für alle Hybridcloud-Workloads zentral verwalten.
- **Sicherheitsdaten aus zahlreichen Quellen**. Sammeln, durchsuchen und analysieren Sie Sicherheitsdaten aus verschiedensten Quellen (einschließlich verbundener Partnerlösungen wie Netzwerkfirewalls und anderer Microsoft-Dienste). 
- **Integration in vorhandene Sicherheitsworkflows**. Mit REST-APIs können Sie auf Sicherheitsinformationen zugreifen und sie integrieren und analysieren, um vorhandene Tools und Prozesse zu verknüpfen.
- **Berichterstellung zur Compliance**. Demonstrieren Sie mithilfe von Sicherheitsdaten und Datenerkenntnissen die Compliance, und erstellen Sie einfach Nachweise für Prüfer.

**Adaptiver Schutz vor Bedrohungen**

- **Fortlaufende Sicherheitsbewertung**. Überwachen Sie die Sicherheit Ihrer Computer, Netzwerke und Azure-Dienste mithilfe Hunderter integrierter oder selbst erstellter Sicherheitsbewertungen. Identifizieren Sie Softwarekomponenten und Konfigurationen, die anfällig für Angriffe sind.
- **Direkt umsetzbare Empfehlungen**. Beseitigen Sie mit priorisierten, direkt umsetzbaren Sicherheitsempfehlungen und integrierten Playbooks für die Automatisierung Sicherheitslücken, bevor sie von Angreifern ausgenutzt werden können.
- **Adaptive Anwendungssteuerungen**. Blockieren Sie Schadsoftware und andere unerwünschten Anwendungen mithilfe von Whitelist-Empfehlungen, die auf Ihre spezifischen Azure-Workloads abgestimmt sind und durch Machine Learning unterstützt werden. 
- **Sicherheit beim Netzwerkzugriff**. Verkleinern Sie die Angriffsfläche des Netzwerks mit kontrolliertem Just-in-Time-Zugriff auf die Verwaltungsports virtueller Azure-Computer, um die Gefahr von Brute-Force- und anderen Netzwerkangriffen erheblich zu senken.

**Intelligente Bedrohungserkennung und entsprechende Gegenmaßnahmen**

- **Umfassendste Threat Intelligence in der Branche**. Nutzen Sie den Microsoft Intelligent Security Graph, der Billionen Signale von Microsoft-Diensten und -Systemen weltweit nutzt, um neue und aktuelle Bedrohungen zu ermitteln.
- **Erweiterte Bedrohungserkennung**. Nutzen Sie integrierte Verhaltensanalysen und Machine Learning, um Angriffe und Zero-Day-Exploits zu erkennen. Überwachen Sie Netzwerke, Computer und Clouddienste auf neue Angriffe und auf Aktivitäten nach einer Sicherheitsverletzung.
- **Priorisierte Warnungen und Vorfälle**. Konzentrieren Sie sich zunächst auf die größten Bedrohungen. Dabei helfen Ihnen priorisierte Warnungen und Vorfälle, verschiedene Warnungstypen einem einzelnen Angriffsversuch zuzuordnen. Sie können auch eigene benutzerdefinierte Sicherheitswarnungen erstellen.
- **Optimierte Untersuchung**. Bewerten Sie über eine visuelle, interaktive Oberfläche schnell den Umfang und die Auswirkungen eines Angriffs. Verwenden Sie vordefinierte Abfragen oder Ad-hoc-Abfragen für eine ausführlichere Untersuchung von Sicherheitsdaten. 
- **Kontextbezogene Threat Intelligence**. Visualisieren Sie die Quelle von Angriffen auf einer interaktiven Weltkarte. Verwenden Sie integrierte Threat Intelligence-Berichte, um wertvolle Einblicke in die Techniken und Ziele bekannter böswilliger Akteure zu erhalten.

## <a name="get-started"></a>Erste Schritte
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Security Center wird mit Ihrem Azure-Abonnement aktiviert. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testabonnement](https://azure.microsoft.com/pricing/free-trial/)registrieren. 

[Erste Schritte mit Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started) werden Sie schnell durch die Komponenten zur Sicherheitsüberwachung und Richtlinienverwaltung von Security Center geführt. 


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurden Security Center, seine wichtigsten Funktionen sowie die ersten Schritte zu seiner Nutzung vorgestellt. Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Planungs- und Betriebshandbuch für Azure Security Center:](security-center-planning-and-operations-guide.md) Hier erfahren Sie, wie Sie die Verwendung von Security Center auf Grundlage der Sicherheitsanforderungen und des Cloudverwaltungsmodells Ihres Unternehmens optimieren können.
* [Festlegen von Sicherheitsrichtlinien:](https://docs.microsoft.com/azure/security-center/security-center-policies) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen:](https://docs.microsoft.com/azure/security-center/security-center-recommendations) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure- und Nicht-Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität:](https://docs.microsoft.com/azure/security-center/security-center-monitoring) Erfahren Sie, wie Sie die Integrität Ihrer Azure-und Nicht-Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen:](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen:](https://docs.microsoft.com/azure/security-center/security-center-partner-solutions) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Security Center – Häufig gestellte Fragen:](https://docs.microsoft.com/azure/security-center/security-center-faq) Hier finden Sie häufig gestellte Fragen zur Verwendung von Security Center.



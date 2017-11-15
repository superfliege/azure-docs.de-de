---
title: "Einführung in Azure Security Center-Sicherheitsrichtlinien | Microsoft-Dokumentation"
description: Hier finden Sie Informationen zu Azure Security Center-Sicherheitsrichtlinien und deren Hauptfunktionen.
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 60cc65bb94e05da1c0b7ee20930c0530f46e71ec
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="security-policies-overview"></a>Übersicht über Sicherheitsrichtlinien
In diesem Artikel erfahren Sie, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren.

## <a name="what-are-security-policies"></a>Was sind Sicherheitsrichtlinien?
Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei. In Azure Security Center können Sie Richtlinien für Ihre Azure-Abonnements definieren und auf die Art der Workload oder auf die Vertraulichkeit der Daten abstimmen. So kann etwa für Anwendungen mit regulierten Daten (beispielsweise personenbezogene Informationen) eine höhere Sicherheitsstufe erforderlich sein als für andere Workloads. 

Security Center-Richtlinien enthalten folgende Komponenten:

- **Datensammlung:** Bestimmt die Einstellungen für die Agent-Bereitstellung und die [Datensammlung](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection).
- **Sicherheitsrichtlinie:** Bestimmt, welche Maßnahmen Security Center überwacht und empfiehlt. Die [Sicherheitsrichtlinie](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies) kann in Security Center bearbeitet werden. Sie können auch [Azure Policy](security-center-azure-policy.md) (eingeschränkte Vorschauversion) verwenden, um neue Definitionen zu erstellen, zusätzliche Richtlinien zu definieren und verwaltungsgruppenübergreifende Richtlinien zuzuweisen.
- **E-Mail-Benachrichtigungen:** Bestimmt Sicherheitskontakte und Einstellungen für [E-Mail-Benachrichtigungen](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details).
- **Tarif:** Definiert die [Tarifauswahl](https://docs.microsoft.com/azure/security-center/security-center-pricing) (Free oder Standard). Der gewählte Tarif beeinflusst, welche Security Center-Features für betroffene Ressourcen verfügbar sind. Sie können einen Tarif für Abonnements, Ressourcengruppen und Arbeitsbereiche angeben. 


## <a name="who-can-edit-security-policies"></a>Wer kann Sicherheitsrichtlinien bearbeiten?
Security Center verwendet die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Dabei werden integrierte Rollen bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können. In Security Center werden den Benutzern nur Informationen zu den Ressourcen angezeigt, auf die sie Zugriff haben. Das bedeutet, dass Benutzern die Rolle *Besitzer*, *Mitwirkender* oder *Leser* für das Abonnement oder die Ressourcengruppe einer Ressource zugewiesen wird. Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

- **Benutzer mit Leseberechtigung für Sicherheitsfunktionen:** Können Informationen in Security Center (wie etwa Empfehlungen, Warnungen, Richtlinien und die Integrität) anzeigen, aber keine Änderungen vornehmen.
- **Sicherheitsadministrator:** Verfügt über die gleichen Anzeigeberechtigungen wie ein *Benutzer mit Leseberechtigung für Sicherheitsfunktionen*, ist zusätzlich aber auch zum Aktualisieren der Sicherheitsrichtlinie und zum Verwerfen von Empfehlungen und Warnungen berechtigt.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie sich über Sicherheitsrichtlinien in Azure Security Center informiert. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Artikeln:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Security Center-Empfehlungen zum Schutz Ihrer Azure-Ressourcen beitragen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center-Datensicherheit:](security-center-data-security.md) Hier erfahren Sie, wie Daten von Security Center verwaltet und geschützt werden.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie Antworten auf häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.



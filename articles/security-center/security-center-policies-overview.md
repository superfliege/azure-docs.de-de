---
title: "Einführung in Azure Security Center-Sicherheitsrichtlinien | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über Azure Security Center-Sicherheitsrichtlinien und deren Hauptfunktionen."
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
ms.date: 09/26/2017
ms.author: yurid
ms.openlocfilehash: f85f7077e5227818d062d114370e7344601a998e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="security-policies-overview"></a>Übersicht über Sicherheitsrichtlinien
In diesem Dokument erfahren Sie, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können.

## <a name="what-are-security-policies"></a>Was sind Sicherheitsrichtlinien?
Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei. In Security Center können Sie Richtlinien für Ihre Azure-Abonnements definieren und auf die Art der Workload oder auf die Vertraulichkeit der Daten abstimmen. Beispielsweise kann für Anwendungen mit einer Regulierung unterliegenden Daten, etwa personenbezogene Informationen (Personally Identifiable Information), eine höhere Sicherheitsstufe als für andere Workloads erforderlich sein. 

Security Center-Richtlinien enthalten folgende Komponenten:

- [Datensammlung](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection): Agent-Bereitstellung und Datensammlungseinstellungen.
- Sicherheitsrichtlinie: Eine [Azure Policy](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction), die bestimmt, welche Kontrollen von Security Center überwacht und empfohlen werden. Sie können diese in Security Center bearbeiten oder Azure Policy verwenden, um neue Definitionen zu erstellen, zusätzliche Richtlinien zu definieren und verwaltungsgruppenübergreifende Richtlinien zuzuweisen.
- E-Mail-Benachrichtigungen: Einstellungen für Sicherheitskontakte und [E-Mail-Benachrichtigungen](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details).
- [Tarif:](https://docs.microsoft.com/azure/security-center/security-center-pricing) Der gewählte Tarif (Free oder Standard) bestimmt, welche Security Center-Features für Ressourcen innerhalb des Gültigkeitsbereichs verfügbar sind. (Kann für Abonnements, Ressourcengruppen und Arbeitsbereiche angegeben werden.) 


## <a name="who-can-edit-security-policies"></a>Wer kann Sicherheitsrichtlinien bearbeiten?
Security Center verwendet die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Dabei werden integrierte Rollen bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können. Wenn Benutzer Security Center öffnen, werden nur Informationen zu den Ressourcen angezeigt, auf die jeweils Zugriff besteht. Dies bedeutet, dass dem Benutzer die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ für das Abonnement oder die Ressourcengruppe einer Ressource zugewiesen wird. Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

- Benutzer mit Leseberechtigung für Sicherheitsfunktionen: Benutzer, die zu dieser Rolle gehören, können Informationen in Security Center anzeigen. Dazu gehören Empfehlungen, Warnungen, Richtlinien und die Integrität. Änderungen können sie jedoch nicht vornehmen.
- Sicherheitsadministrator: Es gelten die gleichen Berechtigungen wie bei einem Benutzer mit Leseberechtigung für Sicherheitsfunktionen, zusätzlich sind jedoch auch Rechte zum Ändern der Sicherheitsrichtlinie und zum Verwerfen von Empfehlungen und Warnungen vorhanden.


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie mehr über Sicherheitsrichtlinien in Azure Security Center erfahren. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md) : Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center-Datensicherheit:](security-center-data-security.md) Hier erfahren Sie, wie Daten in Security Center verwaltet und geschützt werden.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.



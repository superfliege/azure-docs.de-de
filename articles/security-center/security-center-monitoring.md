---
title: Ressourcensicherheit in Azure Security Center | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Einführung in die Überwachung der Ressourcensicherheit in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/21/2018
ms.author: rkarlin
ms.openlocfilehash: 361fc2c6931dcfaed062bd39b75be7bef059b467
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122065"
---
# <a name="monitoring-resource-security-hygiene-in-azure-security-center"></a>Überwachen der Ressourcensicherheit in Azure Security Center
Dieser Artikel unterstützt Sie beim Sicherstellen möglichst hoher Ressourcensicherheit und dem Überwachen der Richtliniencompliance mithilfe der Überwachungsfunktionen von Azure Security Center.

## <a name="what-is-resource-security-hygiene"></a>Was ist Ressourcensicherheit?
Überwachung wird häufig als Beobachten und Warten auf ein Eintreten eines Ereignisses aufgefasst, damit auf diese Situation reagiert werden kann. Ressourcensicherheit bezieht sich auf eine proaktive Strategie, bei der Ihre Ressourcen überwacht werden, um Systeme zu erkennen, die nicht den Unternehmensstandards oder bewährten Methoden entsprechen.

## <a name="resource-security-hygiene"></a>Ressourcensicherheit
Nach Aktivierung der [Sicherheitsrichtlinien](security-center-policies.md) für die Ressourcen eines Abonnements analysiert Security Center die Sicherheit Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Informationen zur Netzwerkkonfiguration sind sofort verfügbar. Abhängig davon, auf wie vielen Ihrer (virtuellen und physischen) Computer der Agent installiert ist, kann es eine Stunde oder länger dauern, bis die Konfigurationsinformationen für die Computer (beispielsweise Sicherheitsupdatestatus und Betriebssystemkonfiguration) gesammelt wurden und verfügbar sind. Eine vollständige Liste der Probleme und Möglichkeiten zum Erhöhen der Sicherheit Ihres Netzwerks und Beheben von Risiken enthält die Kachel **Empfehlungen**.

Sie können den Sicherheitsstatus Ihrer Ressourcen sowie ggf. vorliegende Probleme nach Ressourcentyp anzeigen:

- Weitere Informationen zum Überwachen der Integrität Ihrer Computerressourcen und Apps sowie zum Abrufen von Empfehlungen zur Verbesserung der Sicherheit finden Sie unter [Schützen von Computern und Anwendungen in Azure Security Center](security-center-virtual-machine-protection.md)
- Weitere Informationen zum Überwachen Ihrer Netzwerkressourcen (z.B. virtuelle Computer, Netzwerksicherheitsgruppen und Endpunkte) sowie zum Abrufen von Empfehlungen zur Verbesserung der Sicherheit finden Sie unter [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md). 
- Weitere Informationen zum Überwachen Ihrer Daten- und Speicherressourcen (z.B. SQL-Server und Speicherkonten) sowie zum Abrufen von Empfehlungen zur Verbesserung der Sicherheit finden Sie unter [Schützen des Azure SQL-Diensts und Ihrer Daten in Azure Security Center](security-center-sql-service-recommendations.md). 
- Weitere Informationen zum Überwachen Ihrer Identitäts- und Zugriffsressourcen, einschließlich MFA und Kontoberechtigungen, sowie zum Abrufen von Empfehlungen zur Verbesserung der Sicherheit finden Sie unter [Überwachen von Identität und Zugriff in Azure Security Center](security-center-identity-access.md). 
- Weitere Informationen zum Überwachen des Just-In-Time-Zugriffs auf Ihre Ressourcen finden Sie unter [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](security-center-just-in-time.md). 


Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).



![Kachel „Sicherheitsintegrität der Ressource“](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)



## <a name="see-also"></a>Weitere Informationen
In diesem Artikel haben Sie erfahren, wie Sie die Überwachungsfunktionen in Azure Security Center verwenden können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitseinstellungen in Azure Security Center konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

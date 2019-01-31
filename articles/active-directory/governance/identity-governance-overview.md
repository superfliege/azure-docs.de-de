---
title: Azure AD Identity Governance| Microsoft-Dokumentation
description: Azure AD Identity Governance ermöglicht es Ihnen, den Sicherheitsbedarf Ihrer Organisation und die Produktivität von Mitarbeitern mit den richtigen Prozessen sowie Transparenz in Einklang zu bringen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/25/2018
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.openlocfilehash: 887c0bd4246c7c4215cb4d97dfd96db5d2aba6cb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55170017"
---
# <a name="what-is-azure-ad-identity-governance"></a>Was ist Azure AD Identity Governance?

Azure Active Directory (Azure AD) Identity Governance ermöglicht es Ihnen, den Sicherheitsbedarf Ihrer Organisation und die Produktivität von Mitarbeitern mit den richtigen Prozessen sowie Transparenz in Einklang zu bringen. Der Dienst bietet Funktionen, mit denen Sie sicherstellen können, dass die richtigen Benutzer geeigneten Zugriff auf die richtigen Ressourcen haben, und die Möglichkeit, den Zugriff auf kritische Ressourcen zu schützen, zu überwachen und nachzuverfolgen, während Sie gleichzeitig die Produktivität Ihrer Mitarbeiter gewährleisten.  

Mit Identity Governance können Organisationen die folgenden Aufgaben sowohl für Mitarbeiter, Geschäftspartner und Anbieter als auch für Dienste und Anwendungen ausführen:

- Steuern des Identitätslebenszyklus
- Steuern des Zugriffslebenszyklus
- Sichere Verwaltung

Insbesondere soll Identity Governance Organisationen bei der Beantwortung der folgenden vier Schlüsselfragen unterstützen:

- Welche Benutzer sollen Zugriff auf welche Ressourcen haben?
- Wozu nutzen diese Benutzer den Zugriff?
- Gibt es effektive organisatorische Kontrollen zum Verwalten des Zugriffs?
- Können Prüfer feststellen, ob die Kontrollen funktionieren?

## <a name="identity-lifecycle"></a>Identitätslebenszyklus

Identity Governance hilft Organisationen, ein Gleichgewicht herzustellen zwischen der *Produktivität* – wie schnell eine Person auf die benötigten Ressourcen zugreifen kann (beispielsweise, wenn sie der Organisation beitritt) – und der *Sicherheit* – wie sich der Zugriff im Laufe der Zeit ändern sollte (beispielsweise aufgrund von Änderungen des Beschäftigungsstatus einer Person).  Identity Lifecycle Management ist die Grundlage für Identity Governance, und eine effektive Governance in großem Maßstab erfordert eine Modernisierung der Identity Lifecycle Management-Infrastruktur für Anwendungen.

Bei vielen Organisationen ist der Identitätslebenszyklus für Mitarbeiter mit der Darstellung des Benutzers in einem Personalverwaltungssystem (Human Capital Management, HCM) verknüpft.  Wie im [Tutorial zur eingehenden Bereitstellung von Workday (Vorschauversion)](../saas-apps/workday-inbound-tutorial.md) beschrieben, verwaltet Azure AD Premium Benutzeridentitäten für die in Workday dargestellten Benutzer automatisch in Active Directory und Azure Active Directory.  Azure AD Premium umfasst auch [Microsoft Identity Manager](/microsoft-identity-manager/), eine Lösung, mit der Datensätze aus lokalen Personalverwaltungssystemen wie SAP, Oracle eBusiness und Oracle PeopleSoft importiert werden können.

Immer mehr Szenarien erfordern heute die Kollaboration mit Personen außerhalb Ihrer Organisation. Mithilfe der [Azure AD B2B](/azure/active-directory/b2b/)-Kollaboration (Business-to-Business) können Sie die Anwendungen und Dienste Ihrer Organisation für Gastbenutzer und externe Partner von beliebigen Organisationen sicher freigeben und dabei die Kontrolle über Ihre eigenen Unternehmensdaten behalten.

## <a name="access-lifecycle"></a>Zugriffslebenszyklus

Organisationen benötigen einen Prozess, um den Zugriff über die anfänglich beim Erstellen der Identität eines Benutzers zugewiesenen Rechte hinaus zu verwalten.  Zudem müssen Unternehmensorganisationen in der Lage sein, ihre Systeme effizient zu skalieren, damit sie Zugriffsrichtlinien und -kontrollen kontinuierlich entwickeln und erzwingen können.

In der Regel delegiert die IT-Abteilung Entscheidungen bezüglich der Zugriffsgenehmigung an Entscheidungsträger im Unternehmen.  Darüber hinaus kann die IT die Benutzer selbst in den Prozess einbeziehen.  Beispielsweise müssen Benutzer, die in Europa auf vertrauliche Kundendaten in der Marketinganwendung eines Unternehmens zugreifen, die Richtlinien des Unternehmens kennen. Gastbenutzer kennen die Anforderungen für die Behandlung von Daten in einer Organisation, zu der sie eingeladen wurden, möglicherweise nicht.

Organisationen können den Zugriffslebenszyklus mit Technologien wie [dynamischen Gruppen](../users-groups-roles/groups-dynamic-membership.md) in Verbindung mit der Benutzerbereitstellung in [SaaS-Apps](../saas-apps/tutorial-list.md) oder [in SCIM integrierten Apps](../manage-apps/use-scim-to-provision-users-and-groups.md) automatisieren.  Außerdem können Organisationen steuern, welche [Gastbenutzer Zugriff auf lokale Anwendungen haben](../b2b/hybrid-cloud-to-on-premises.md).  Diese Zugriffsrechte können dann regelmäßig mithilfe von wiederkehrenden [Azure AD-Zugriffsüberprüfungen](access-reviews-overview.md) überprüft werden.

Wenn ein Benutzer auf Anwendungen zuzugreifen versucht, erzwingt Azure AD Richtlinien für [bedingten Zugriff](/azure/active-directory/conditional-access/). Richtlinien für bedingten Zugriff können beispielsweise beinhalten, dass [Nutzungsbedingungen](active-directory-tou.md) angezeigt werden und [der Benutzer diesen Bedingungen zustimmen muss](../conditional-access/require-tou.md), bevor er auf eine Anwendung zugreifen kann.

## <a name="privileged-access-lifecycle"></a>Privilegierter Zugriffslebenszyklus

In der Vergangenheit wurde der privilegierte Zugriff von anderen Anbietern als eine von der Identitätsgovernance getrennte Funktion dargestellt. Wir bei Microsoft denken jedoch, dass die Steuerung des privilegierten Zugriffs ein wichtiger Bestandteil der Identitätsgovernance ist – insbesondere angesichts des mit diesen Administratorrechten einhergehenden möglichen Missbrauchs und Schadens für eine Organisation. Die Mitarbeiter, Anbieter und Auftragnehmer, denen Administratorrechte gewährt werden, müssen gesteuert werden.

Azure AD Privileged Identity Management (PIM) bietet zusätzliche Kontrollen zum Sichern des Zugriffs und Verwalten der Zugriffsrechte für Ressourcen in Azure AD, Azure und anderen Microsoft Online Services.  Zusätzlich zur mehrstufigen Authentifizierung und zum bedingten Zugriff stehen Ihnen mit den Funktionen für Just-in-Time-Zugriff und Warnungen für Rollenänderungen von Azure AD PIM umfassende Governancekontrollen zum Schützen der Ressourcen Ihres Unternehmens (Verzeichnis-, Office 365- und Azure-Ressourcenrollen) zur Verfügung. Wie bei anderen Formen des Zugriffs können Organisationen mithilfe von Zugriffsüberprüfungen eine periodische erneute Zertifizierung für alle Benutzer mit Administratorrollen konfigurieren.

## <a name="getting-started"></a>Erste Schritte

Es gibt keine perfekte Lösung oder Empfehlung, die für alle Kunden geeignet ist. Die folgenden Konfigurationen können jedoch als Leitfaden für die Basisrichtlinien dienen, die Microsoft zum Verbessern der Sicherheit und Produktivität von Mitarbeitern empfiehlt.

- [Konfigurationen für den Identitäts- und Gerätezugriff](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Sichern des privilegierten Zugriffs](../users-groups-roles/directory-admin-roles-secure.md)


### <a name="access-reviews"></a>Zugriffsüberprüfungen

- [Was ist eine Zugriffsüberprüfung?](access-reviews-overview.md)
- [Verwalten des Benutzerzugriffs mit Zugriffsüberprüfungen](manage-user-access-with-access-reviews.md)
- [Verwalten des Gastzugriffs mit Zugriffsüberprüfungen](manage-guest-access-with-access-reviews.md)
- [Starten einer Zugriffsüberprüfung für Verzeichnisrollen](../privileged-identity-management/pim-how-to-start-security-review.md)

### <a name="terms-of-use"></a>Nutzungsbedingungen

- [Welche Möglichkeiten bieten die Nutzungsbedingungen?](active-directory-tou.md)

### <a name="privileged-identity-management"></a>Privileged Identity Management

- [Was ist Azure AD PIM?](../privileged-identity-management/pim-configure.md)

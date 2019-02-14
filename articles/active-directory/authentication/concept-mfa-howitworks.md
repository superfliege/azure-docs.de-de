---
title: Azure Multi-Factor Authentication – Funktionsweise
description: Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: dabb47d4c4f39d30d8e19e8d8f41b5b84c57fd42
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217597"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>So funktioniert's: Azure Multi-Factor Authentication

Die Sicherheit der zweistufigen Überprüfung liegt im Ebenenansatz. Die Faktoren der mehrfachen Authentifizierung zu überwinden stellt eine große Herausforderung für Angreifer dar. Selbst wenn ein Angreifer das Kennwort des Benutzers herausfinden kann, ist dies nutzlos, wenn er nicht auch die zusätzliche Authentifizierungsmethode beherrscht. Dies funktioniert durch das Anfordern von mindestens zwei der folgenden Authentifizierungsmethoden:

* Etwas, das Sie wissen (normalerweise ein Kennwort)
* Etwas, das Sie haben (ein vertrautes Gerät, das nicht leicht dupliziert werden kann, wie ein Telefon)
* Etwas, das Sie sind (biometrisch)

<center>![Bild konzeptioneller Authentifizierungsmethoden](./media/concept-mfa-howitworks/methods.png)</center>

Azure Multi-Factor Authentication (MFA) trägt zum Schutz des Zugriffs auf Daten und Anwendungen bei und ist dabei für den Benutzer unkompliziert. Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache [Authentifizierungsmethoden](concept-authentication-methods.md). Benutzer können auf der Grundlage von Konfigurationsentscheidungen, die ein Administrator trifft, zur MFA aufgefordert werden oder auch nicht.

## <a name="how-to-get-multi-factor-authentication"></a>Beziehen von Multi-Factor Authentication

Multi-Factor Authentication ist Teil der folgenden Angebote:

* **Azure Active Directory Premium-Lizenzen**: Umfassende Verwendung von Azure Multi-Factor Authentication-Dienst (Cloud) oder Azure Multi-Factor Authentication-Server (lokal).
   * **Azure MFA-Dienst (Cloud)** - **: Diese Option ist der empfohlene Weg für neue Bereitstellungen**. Azure MFA in der Cloud erfordert keine lokale Infrastruktur und kann mit Partner- oder reinen Cloudbenutzern verwendet werden.
   * **Azure MFA-Server**: Wenn Ihre Organisation die dazugehörigen Infrastrukturelemente verwalten möchte und AD FS in Ihrer lokalen Umgebung bereitgestellt hat, ist dies möglicherweise eine Option.
* **Multi-Factor Authentication für Office 365**: Eine Teilmenge der Funktionen der Multi-Factor Authentication von Azure ist im Rahmen Ihres Abonnements verfügbar. Weitere Informationen zu MFA für Office 365 finden Sie im Artikel [Planen der mehrstufigen Authentifizierung für Office 365-Bereitstellungen](https://support.office.com/article/plan-for-multi-factor-authentication-for-office-365-deployments-043807b2-21db-4d5c-b430-c8a6dee0e6ba).
* **Azure Active Directory – Globale Administratoren**: Eine Teilmenge der Azure Multi-Factor Authentication-Funktionen ist als Mittel zum Schutz globaler Administratorkonten verfügbar.

> [!NOTE]
> Ab dem 1. September 2018 können Neukunden Azure Multi-Factor Authentication nicht mehr als eigenständiges Angebot erwerben. Multi-Factor Authentication ist in Azure AD Premium-Lizenzen weiterhin ein verfügbares Feature.

## <a name="supportability"></a>Unterstützungsmöglichkeiten

Die meisten Benutzer sind daran gewöhnt, für die Authentifizierung lediglich Kennwörter zu verwenden. Daher ist es wichtig, dass Ihre Organisation das Bewusstsein für diesen Prozess bei sämtlichen Benutzern fördert. So lassen sich Anrufe beim Helpdesk aufgrund kleinerer Probleme im Zusammenhang mit MFA reduzieren. In einigen Szenarien muss MFA jedoch vorübergehend deaktiviert werden. Befolgen Sie die unten stehenden Richtlinien für den Umgang mit diesen Szenarien:

* Schulen Sie Ihre Supportmitarbeiter für Szenarien, in denen Benutzer sich nicht anmelden können, da sie keinen Zugriff auf ihre Authentifizierungsmethoden haben, oder diese nicht ordnungsgemäß funktionieren.
   * Mit Richtlinien für bedingten Zugriff für den Azure MFA-Dienst können Ihre Supportmitarbeiter einen Benutzer einer Gruppe hinzufügen, die von einer MFA erfordernden Richtlinie ausgeschlossen ist.
   * Supportmitarbeiter können für Benutzer von Azure MFA-Server eine temporäre Einmalumgehung aktivieren, um einem Benutzer die Authentifizierung ohne zweistufige Überprüfung zu ermöglichen. Die Umgehung ist vorübergehend und läuft nach einer angegebenen Anzahl von Sekunden ab.   
* Erwägen Sie die Verwendung von vertrauenswürdigen IP-Adressen oder benannten Orten als Möglichkeit, die Aufforderungen zur zweistufigen Überprüfung zu minimieren. Mit diesem Feature können Administratoren eines verwalteten oder Verbundmandanten die zweistufige Überprüfung für Benutzer umgehen, die sich über einen vertrauenswürdigen Netzwerkstandort wie das Intranet ihrer Organisation anmelden.
* Stellen Sie [Azure AD Identity Protection](../active-directory-identityprotection.md) bereit, und lösen Sie die zweistufige Überprüfung auf Risikoereignissen basierend aus.

## <a name="next-steps"></a>Nächste Schritte

- Rufen Sie einen [Plan](https://aka.ms/MFADeploymentPlan) für die schrittweise MFA-Bereitstellung ab.

- Erfahren Sie Details über die [Lizenzierung Ihrer Benutzer](concept-mfa-licensing.md).

- Erfahren Sie Details darüber, [welche Version Sie bereitstellen sollten](concept-mfa-whichversion.md).

- Finden Sie Antworten auf [häufig gestellte Fragen](multi-factor-authentication-faq.md).

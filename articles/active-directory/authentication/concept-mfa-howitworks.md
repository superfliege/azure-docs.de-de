---
title: Azure Multi-Factor Authentication – Funktionsweise
description: Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 46c99011a22f855f6faf53e03169b2d1e4c6ce85
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669006"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Funktionsweise: Azure Multi-Factor Authentication

Die Sicherheit der zweistufigen Überprüfung liegt im Ebenenansatz. Die Faktoren der mehrfachen Authentifizierung zu überwinden stellt eine große Herausforderung für Angreifer dar. Selbst wenn ein Angreifer das Kennwort des Benutzers herausfinden kann, ist dies nutzlos, wenn er nicht auch die zusätzliche Authentifizierungsmethode beherrscht. Dies funktioniert durch das Anfordern von mindestens zwei der folgenden Authentifizierungsmethoden:

* Etwas, das Sie wissen (normalerweise ein Kennwort)
* Etwas, das Sie haben (ein vertrautes Gerät, das nicht leicht dupliziert werden kann, wie ein Telefon)
* Etwas, das Sie sind (biometrisch)

<center>![Bild konzeptioneller Authentifizierungsmethoden](./media/concept-mfa-howitworks/methods.png)</center>

Azure Multi-Factor Authentication (MFA) trägt zum Schutz des Zugriffs auf Daten und Anwendungen bei und ist dabei für den Benutzer unkompliziert. Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache [Authentifizierungsmethoden](concept-authentication-methods.md).

## <a name="how-to-get-multi-factor-authentication"></a>Beziehen von Multi-Factor Authentication

Multi-Factor Authentication ist Teil der folgenden Angebote:

* **Azure Active Directory Premium-Lizenzen**: Umfassende Verwendung von Azure Multi-Factor Authentication-Dienst (Cloud) oder Azure Multi-Factor Authentication-Server (lokal).
   * **Azure MFA-Dienst (Cloud)** - **: Diese Option ist der empfohlene Weg für neue Bereitstellungen**. Azure MFA in der Cloud erfordert keine lokale Infrastruktur und kann mit Partner- oder reinen Cloudbenutzern verwendet werden.
   * **Azure MFA-Server**: Wenn Ihre Organisation die dazugehörigen Infrastrukturelemente verwalten möchte und AD FS in Ihrer lokalen Umgebung bereitgestellt hat, ist dies möglicherweise eine Option.
* **Multi-Factor Authentication für Office 365**: Eine Teilmenge der Funktionen der Multi-Factor Authentication von Azure ist im Rahmen Ihres Abonnements verfügbar. Weitere Informationen zu MFA für Office 365 finden Sie im Artikel [Planen der mehrstufigen Authentifizierung für Office 365-Bereitstellungen](https://support.office.com/article/plan-for-multi-factor-authentication-for-office-365-deployments-043807b2-21db-4d5c-b430-c8a6dee0e6ba).
* **Azure Active Directory – Globale Administratoren**: Eine Teilmenge der Azure Multi-Factor Authentication-Funktionen ist als Mittel zum Schutz globaler Administratorkonten verfügbar.

> [!NOTE]
> Ab dem 1. September 2018 können Neukunden Azure Multi-Factor Authentication nicht mehr als eigenständiges Angebot erwerben. Multi-Factor Authentication ist in Azure AD Premium-Lizenzen weiterhin ein verfügbares Feature.

### <a name="auth-provider-or-mfa-license"></a>Authentifizierungsanbieter oder MFA-Lizenz

Wenn Sie Azure AD Premium oder ein anderes Lizenzpaket haben, das Azure AD Premium umfasst, verfügen Sie bereits über Azure MFA. Ihre Organisation benötigt keine weiteren Komponenten, um die zweistufige Überprüfung für alle Benutzer zu implementieren. Sie müssen nur einem Benutzer eine Lizenz zuweisen und können dann MFA aktivieren.

Wenn Sie nicht über Lizenzen verfügen, die Azure MFA enthalten, oder nicht über genügend Lizenzen für alle Benutzer, können Sie einen [MFA-Authentifizierungsanbieter](concept-mfa-authprovider.md) erstellen, um den vollständigen Funktionsumfang von MFA auf die Benutzer auszuweiten, die ihn benötigen.

> [!IMPORTANT]
> Wenn Sie nicht über genügend Lizenzen für all Ihre Benutzer verfügen, können Sie einen Multi-Factor Authentication-Anbieter mit Bezahlung pro Benutzer erstellen, um Lizenzen für den Rest Ihrer Organisation bereitzustellen. Erstellen Sie keinen Multi-Factor Authentication-Anbieter mit Bezahlung pro Authentifizierung. Wenn Sie es tun, bezahlen Sie möglicherweise für die Überprüfung von Anforderungen von Benutzern, die bereits über Lizenzen verfügen.

## <a name="supportability"></a>Unterstützungsmöglichkeiten

Die meisten Benutzer sind daran gewöhnt, für die Authentifizierung lediglich Kennwörter zu verwenden. Daher ist es wichtig, dass Ihre Organisation das Bewusstsein für diesen Prozess bei sämtlichen Benutzern fördert. So lassen sich Anrufe beim Helpdesk aufgrund kleinerer Probleme im Zusammenhang mit MFA reduzieren. In einigen Szenarien muss MFA jedoch vorübergehend deaktiviert werden. Befolgen Sie die unten stehenden Richtlinien für den Umgang mit diesen Szenarien:

* Schulen Sie Ihre Supportmitarbeiter für Szenarien, in denen Benutzer sich nicht anmelden können, da sie keinen Zugriff auf ihre Authentifizierungsmethoden haben, oder diese nicht ordnungsgemäß funktionieren.
   * Mit Richtlinien für bedingten Zugriff für den Azure MFA-Dienst können Ihre Supportmitarbeiter einen Benutzer einer Gruppe hinzufügen, die von einer MFA erfordernden Richtlinie ausgeschlossen ist.
   * Supportmitarbeiter können für Benutzer von Azure MFA-Server eine temporäre Einmalumgehung aktivieren, um einem Benutzer die Authentifizierung ohne zweistufige Überprüfung zu ermöglichen. Die Umgehung ist vorübergehend und läuft nach einer angegebenen Anzahl von Sekunden ab.
   * Mit Richtlinien für bedingten Zugriff für den Azure MFA-Dienst können Ihre Supportmitarbeiter einen Benutzer einer Gruppe hinzufügen, die von einer MFA erfordernden Richtlinie ausgeschlossen ist.
* Erwägen Sie die Verwendung von vertrauenswürdigen IP-Adressen oder benannten Orten als Möglichkeit, die Aufforderungen zur zweistufigen Überprüfung zu minimieren. Mit diesem Feature können Administratoren eines verwalteten oder Verbundmandanten die zweistufige Überprüfung für Benutzer umgehen, die sich über einen vertrauenswürdigen Netzwerkstandort wie das Intranet ihrer Organisation anmelden.
* Stellen Sie [Azure AD Identity Protection](../active-directory-identityprotection.md) bereit, und lösen Sie die zweistufige Überprüfung auf Risikoereignissen basierend aus.

## <a name="next-steps"></a>Nächste Schritte

- Rufen Sie einen [Plan](https://aka.ms/MFADeploymentPlan) für die schrittweise MFA-Bereitstellung ab.

- Erfahren Sie Details über die [Lizenzierung Ihrer Benutzer](concept-mfa-licensing.md).

- Erfahren Sie Details darüber, [welche Version Sie bereitstellen sollten](concept-mfa-whichversion.md).

- Finden Sie Antworten auf [häufig gestellte Fragen](multi-factor-authentication-faq.md).

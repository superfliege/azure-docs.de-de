---
title: Bewährte Methoden für die Azure-Identitäts- und Zugriffssicherheit | Microsoft Docs
description: In diesem Artikel werden einige bewährte Methoden zur Identitätsverwaltung und Zugriffssteuerung mit integrierten Azure-Funktionen beschrieben.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: barclayn
ms.openlocfilehash: f872c61ad0597d2307cd244668fdfc258f7a45cb
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895685"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure-Identitätsverwaltung und Sicherheit der Zugriffssteuerung – Bewährte Methoden

Die Identität wird häufig als neue Grenzschicht für die Sicherheit angesehen, indem diese Rolle vom herkömmlichen netzwerkzentrierten Ansatz übernommen wird. Die Weiterentwicklung dieses Dreh- und Angelpunkts für die Bereiche Sicherheit und Investitionen basiert darauf, dass Umkreisnetzwerke immer durchlässiger geworden sind und die Verteidigung des Umkreisnetzwerks nicht mehr so effektiv wie vor der immensen Nutzung von [BYOD](https://aka.ms/byodcg)-Geräten und Cloudanwendungen sein kann.

In diesem Artikel werden bewährte Methoden für die Azure-Identitätsverwaltung und die Sicherheit der Zugriffssteuerung beschrieben. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit [Azure AD](../active-directory/fundamentals/active-directory-whatis.md) und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Für jede bewährte Methode wird Folgendes beschrieben:

* Wobei es bei der bewährten Methode geht
* Warum Sie die bewährte Methode nutzen sollten
* Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
* Mögliche Alternativen zur bewährten Methode
* Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für die Azure-Identitätsverwaltung und Zugriffssteuerung basiert auf einer Konsensmeinung und den Fähigkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Erstellungszeitpunkt dieses Artikels existierten. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Der Artikel enthält bewährte Methoden zur Azure-Identitätsverwaltung und Sicherheit der Zugriffssteuerung für die folgenden Bereiche:

* Behandeln von Identität als primären Sicherheitsbereich
* Zentralisieren der Identitätsverwaltung
* Einmaliges Anmelden aktivieren
* Aktivieren des bedingten Zugriffs
* Aktivieren der Kennwortverwaltung
* Erzwingen der mehrstufigen Überprüfung für Benutzer
* Verwenden der rollenbasierten Zugriffssteuerung
* Senken der Gefährdung privilegierter Konten
* Steuern von Standorten, wo sich Ressourcen befinden

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Behandeln von Identität als primären Sicherheitsbereich

Viele betrachten Identität als primären Sicherheitsbereich. Dies ist eine Abkehr vom traditionellen Schwerpunkt der Netzwerksicherheit. Netzwerkumkreise werden immer durchlässiger, und die Verteidigung des Umkreises kann nicht so effektiv sein wie vor der explosionsartigen Verbreitung von [BYOD](https://aka.ms/byodcg)-Geräten und Cloudanwendungen.
[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) ist die Lösung für Identitäts- und Zugriffsverwaltung von Azure. Azure AD ist ein mehrinstanzenfähiger, cloudbasierter Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Azure AD kombiniert grundlegende Verwaltungsdienste, Zugriffsverwaltung für Anwendungen und Identitätsgovernance in einer einzigen Lösung.

In den folgenden Abschnitten werden die Best Practices für die Identitäts- und Zugriffssicherheit mit Azure AD aufgeführt.

## <a name="centralize-identity-management"></a>Zentralisieren der Identitätsverwaltung

In einem [Hybrididentitätsszenario](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) sollten Sie Ihre lokalen und Cloudverzeichnisse integrieren. Die Integration ermöglicht Ihrem IT-Team, Konten von einem zentralen Ort aus zu verwalten, unabhängig davon, wo ein Konto erstellt wird. Die Integration steigert auch die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird.


**Bewährte Methode**: Integrieren Sie Ihre lokalen Verzeichnisse mit Azure AD.  
**Detail**: Synchronisieren Sie per [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) Ihr lokales Verzeichnis mit Ihrem Cloudverzeichnis.

**Bewährte Methode**: Aktivieren Sie die Kennworthashsynchronisierung.  
**Detail**: Die Kennworthashsynchronisierung ist ein Feature zum Synchronisieren von Hashes von Benutzerkennwörtern aus einer lokalen Active Directory-Instanz mit einer cloudbasierten Azure Active Directory-Instanz (Azure AD).

Auch wenn Sie den Verbund mit Active Directory-Verbunddiensten (Active Directory Federation Services, AD FS) oder anderen Identitätsanbietern verwenden möchten, können Sie die Kennworthashsynchronisierung optional als eine Sicherung für den Fall einrichten, dass bei Ihren lokalen Servern ein Fehler auftritt oder sie vorübergehend nicht verfügbar sind. Dies ermöglicht Benutzern, sich bei dem Dienst mit dem gleichen Kennwort anzumelden, das sie zur Anmeldung bei Ihrer lokalen Active Directory-Instanz verwenden. Außerdem ermöglicht es Identity Protection das Erkennen gefährdeter Anmeldeinformationen durch Vergleichen ihrer Kennworthashes mit Kennwörtern, die bekanntermaßen gefährdet sind, wenn ein Benutzer dieselbe E-Mail-Adresse und dasselbe Kennwort bei anderen Diensten genutzt hat, die nicht mit Azure AD verbunden sind.

Weitere Informationen finden Sie unter [Implementieren der Kennworthashsynchronisierung mit der Azure AD Connect-Synchronisierung](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

Organisationen, die ihre lokale Identität nicht in ihre Cloudidentität integrieren, haben mehr Aufwand beim Verwalten der Konten. Dieser Aufwand steigert die Wahrscheinlichkeit von Fehlern und Sicherheitsverletzungen.

## <a name="enable-single-sign-on"></a>Einmaliges Anmelden aktivieren

In einer primär auf Mobil- und Cloudtechnologie ausgelegten Welt möchten Sie einmaliges Anmelden (Single Sign-On, SSO) bei Geräten, Apps und Diensten von jedem Ort ermöglichen, damit Ihre Benutzer jederzeit und überall produktiv sein können. Wenn Sie mehrere Identitätslösungen verwalten müssen, wird dies nicht nur für die IT-Abteilung zu einem Verwaltungsproblem, sondern auch für die Benutzer, die sich mehrere Kennwörter merken müssen.

Indem Sie für alle Ihre Apps und Ressourcen die gleiche Identitätslösung verwenden, können Sie SSO erreichen. Ihre Benutzer können die gleiche Gruppe von Anmeldeinformationen zum Anmelden und Zugreifen auf die benötigten Ressourcen nutzen. Dabei spielt es keine Rolle, ob eine Ressource lokal ist oder sich in der Cloud befindet.

**Bewährte Methode**: Aktivieren Sie einmaliges Anmelden.  
**Detail**: Azure AD [erweitert das lokale Active Directory](../active-directory/connect/active-directory-aadconnect.md) auf die Cloud. Benutzer können ihr primäres Geschäfts-, Schul- oder Unikonto für ihre in die Domäne eingebundenen Geräte und Unternehmensressourcen sowie alle Web-und SaaS-Anwendungen verwenden, die sie benötigen, um ihre Arbeit zu erledigen. Benutzer müssen sich keine Vielzahl von Benutzernamen und Kennwörtern mehr merken, und ihr Anwendungszugriff kann basierend auf ihrer Gruppenmitgliedschaft in der Organisation sowie ihrem Mitarbeiterstatus automatisch bereitgestellt (oder deaktiviert) werden. Und Sie können diesen Zugriff für Katalog-Apps oder für Ihre eigenen lokalen Apps steuern, die Sie über den [Azure AD-Anwendungsproxy](../active-directory/active-directory-application-proxy-get-started.md) entwickelt und veröffentlicht haben.

Verwenden Sie SSO, um Benutzern zu ermöglichen, basierend auf ihrem Geschäfts-, Schul- oder Unikonto in Azure AD auf ihre [SaaS-Anwendungen](../active-directory/active-directory-appssoaccess-whatis.md) zuzugreifen. Dies gilt nicht nur für Microsoft SaaS-Apps, sondern auch für andere Apps, z.B. [Google-Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) und [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Sie können Ihre Anwendung so konfigurieren, dass Azure AD als [SAML-Identitätsanbieter](../active-directory/fundamentals-identity.md) verwendet wird. Als Sicherheitsmaßnahme stellt Azure AD nur dann ein Token aus, das Benutzern die Anmeldung bei der Anwendung erlaubt, wenn ihnen zuvor mit Azure AD Zugriff gewährt wurde. Sie können den Zugriff direkt oder über eine Gruppe gewähren, deren Mitglied die Benutzer sind.

Organisationen, die keine gemeinsame Identität zum Einrichten von SSO für ihre Benutzer und Anwendungen erstellen, haben häufiger mit Szenarien zu tun, in denen Benutzer mehrere Kennwörter haben. Diese Szenarien erhöhen die Wahrscheinlichkeit, dass Benutzer Kennwörter wiederverwenden oder unsichere Kennwörter verwenden.

## <a name="turn-on-conditional-access"></a>Aktivieren des bedingten Zugriffs

Benutzer können mithilfe einer Vielzahl von Geräten und Apps von überall aus auf Ressourcen in Ihrer Organisation zugreifen. Als IT-Administrator möchten Sie sicherstellen, dass diese Geräte Ihren Standards für Sicherheit und Konformität entsprechen. Eine Konzentration darauf, wer auf Ihre Ressourcen Zugriff hat, reicht nicht mehr aus.

Um das Gleichgewicht zwischen Sicherheit und Produktivität zu bewahren, müssen Sie bei der Wahl der Zugriffssteuerung berücksichtigen, auf welche Weise auf eine Ressource zugegriffen wird. Mit dem bedingten Zugriff von Azure AD können Sie diese Anforderung erfüllen. Mit dem bedingten Zugriff können Sie basierend auf bestimmten Bedingungen automatisierte Entscheidungen hinsichtlich der Zugriffssteuerung für den Zugriff auf Ihre Cloud-Apps treffen.

**Bewährte Methode**: Verwalten und steuern Sie den Zugriff auf Unternehmensressourcen.  
**Detail**: Konfigurieren Sie den [bedingten Zugriff](../active-directory/active-directory-conditional-access-azure-portal.md) auf Azure AD basierend auf einer Gruppen-, Standort- und Anwendungsvertraulichkeit für SaaS-Apps und verbundene Azure AD-Apps.

## <a name="enable-password-management"></a>Aktivieren der Kennwortverwaltung

Wenn Sie mehrere Mandanten verwenden oder Benutzern das [Zurücksetzen ihres Kennworts](../active-directory/active-directory-passwords-update-your-own-password.md) ermöglichen möchten, ist es wichtig, dass Sie zur Verhinderung von Missbrauch geeignete Sicherheitsrichtlinien verwenden.

**Bewährte Methode**: Richten Sie Self-Service-Kennwortzurücksetzung (SSPR, Self-Service Password Reset) für Ihre Benutzer ein.  
**Detail**: Verwenden Sie das Feature [Self-Service-Kennwortzurücksetzung](../active-directory-b2c/active-directory-b2c-reference-sspr.md) in Azure AD.

**Bewährte Methode**: Überwachen Sie, wie oder ob SSPR tatsächlich verwendet wird.  
**Detail**: Überwachen Sie die Benutzer, die sich registrieren, mit dem [Bericht zur Registrierung für die Kennwortzurücksetzung](../active-directory/active-directory-passwords-get-insights.md) von Azure AD. Über die Berichtsfunktion, die von Azure AD bereitgestellt wird, können Sie Fragen beantworten, indem Sie vordefinierte Berichte verwenden. Wenn Sie eine ordnungsgemäße Lizenz haben, können Sie auch benutzerdefinierte Abfragen erstellen.

## <a name="enforce-multi-factor-verification-for-users"></a>Erzwingen der mehrstufigen Überprüfung für Benutzer

Sie sollten die zweistufige Überprüfung für alle Benutzer fordern. Dies schließt Administratoren und andere Personen in Ihrer Organisation ein, bei denen die Gefährdung ihrer Konten einen besonders schädlichen Einfluss haben kann (z.B. Finanzchefs).

Es gibt mehrere Optionen, um eine zweistufige Überprüfung zu erzwingen. Die beste Option für Sie hängt von Ihren Zielen ab, der Azure AD-Edition, die Sie ausführen, und Ihrem Lizenzierungsprogramm. Bestimmen Sie mithilfe des Artikels [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer](../active-directory/authentication/howto-mfa-userstates.md) die beste Option für Sie. Weitere Informationen zu Lizenzen und Preisen finden Sie auf den Preisgestaltungsseiten von [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) und [Multi-Factor Authentication – Preise](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

Im Folgenden werden Optionen und Vorteile der zweistufigen Überprüfung beschrieben:

**Option 1**: [Aktivieren Sie Azure Multi-Factor Authentication durch Ändern des Benutzerstatus](../active-directory/authentication/howto-mfa-userstates.md).   
**Vorteil**: Dies ist die herkömmliche Methode, die zweistufige Überprüfung zu anzufordern. Sie funktioniert sowohl mit der [mehrstufigen Azure-Authentifizierung in der Cloud als auch mit Azure Multi-Factor Authentication Server](../active-directory/authentication/concept-mfa-whichversion.md). Diese Methode erfordert, dass Benutzer jedes Mal, wenn sie sich anmelden, die zweistufige Überprüfung durchführen, und überschreibt Richtlinien für bedingten Zugriff.

**Option 2**: [Aktivieren Sie Multi-Factor Authentication mithilfe einer Richtlinie für bedingten Zugriff](../active-directory/authentication/howto-mfa-getstarted.md).
**Vorteil**: Mit dieser Option können Sie die zweistufige Überprüfung unter bestimmten Bedingungen mithilfe des [bedingten Zugriffs](../active-directory/active-directory-conditional-access-azure-portal.md) anfordern. Bestimmte Bedingungen können sein: Benutzeranmeldung von verschiedenen Standorten, nicht vertrauenswürdige Geräte oder Anwendungen, die Sie als risikoreich betrachten. Durch Definieren von bestimmten Bedingungen, in denen Sie die zweistufige Überprüfung erfordern, können Sie die konstante Aufforderung Ihrer Benutzer vermeiden, die eine unangenehme Benutzererfahrung sein kann.

Dies ist die flexibelste Möglichkeit, die zweistufige Überprüfung für Ihre Benutzer zu aktivieren. Die Aktivierung einer Richtlinie für bedingten Zugriff funktioniert jedoch nur bei Azure Multi-Factor Authentication in der Cloud und ist eine Premium-Funktion von Azure AD. Weitere Informationen zu dieser Methode finden Sie unter [Bereitstellen von cloudbasierter Azure Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md).

**Option 3**: Aktivieren Sie Multi-Factor Authentication mithilfe von Richtlinien für bedingten Zugriff durch die Auswertung von Benutzer und Anmelderisiko durch [Azure AD Identity Protection](../active-directory/authentication/tutorial-risk-based-sspr-mfa.md).   
**Vorteil**: Diese Option ermöglicht Ihnen Folgendes:

- Ermitteln potenzieller Sicherheitsrisiken für Identitäten Ihrer Organisation.
- Konfigurieren automatischer Reaktionen auf erkannte verdächtige Aktionen im Zusammenhang mit den Identitäten Ihrer Organisation.
- Untersuchen verdächtiger Vorfälle und Ergreifen entsprechender Maßnahmen zu deren Behebung.

Diese Methode nutzt die Risikobewertung von Azure AD Identity Protection, um die Notwendigkeit der zweistufigen Überprüfung auf Basis des Benutzers und Anmelderisikos für alle Cloudanwendungen zu bestimmen. Diese Methode erfordert die Azure Active Directory P2-Lizenzierung. Weitere Informationen zu dieser Methode finden Sie unter [Azure Active Directory Identity Protection](../active-directory/identity-protection/overview.md).

> [!Note]
> Option 1, Aktivierung von Multi-Factor Authentication durch Ändern des Benutzerstatus, überschreibt die bedingten Zugriffsrichtlinien. Da die Optionen 2 und 3 Richtlinien für bedingten Zugriff verwenden, können Sie Option 1 nicht mit ihnen zusammen verwenden.

Organisationen, die keine zusätzliche Schicht zur Identitätssicherung hinzufügen, etwa die zweistufige Überprüfung, sind anfälliger für Angriffe mit dem Ziel des Diebstahls von Anmeldeinformationen. Ein Angriff mit gestohlenen Anmeldeinformationen kann zum Kompromittieren der Daten führen.

## <a name="use-role-based-access-control-rbac"></a>Verwenden der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)

Das Einschränken des Zugriffs auf der Grundlage der Sicherheitsprinzipien [Need to know](https://en.wikipedia.org/wiki/Need_to_know) und [Least Privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ist für Organisationen unerlässlich, die Sicherheitsrichtlinien für den Datenzugriff erzwingen möchten. Sie können die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) verwenden, um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein.

Sie können [integrierte RBAC-Rollen](../role-based-access-control/built-in-roles.md) in Azure verwenden, um Benutzern Berechtigungen zuzuweisen. Organisationen, die keine Datenzugriffssteuerung mithilfe von Funktionen wie RBAC erzwingen, erteilen Ihren Benutzern möglicherweise mehr Berechtigungen als erforderlich. Dies kann zum Kompromittieren von Daten führen, indem Benutzern erlaubt wird, auf bestimmte Arten von Daten zuzugreifen (z.B. Daten mit hoher geschäftlicher Bedeutung), auf die sie eigentlich keinen Zugriff haben dürften.

## <a name="lower-exposure-of-privileged-accounts"></a>Senken der Gefährdung privilegierter Konten

Das Sichern des privilegierten Zugriffs ist ein entscheidender erster Schritt, um die geschäftlichen Ressourcen zu schützen. Halten Sie die Anzahl von Personen mit Zugriff auf sichere Informationen oder Ressourcen möglichst gering, da sich so das Risiko verringert, dass ein böswilliger Benutzer Zugriff darauf erhält, oder ein autorisierter Benutzer versehentlich eine sensible Ressource kompromittiert.

Mit privilegierten Konten werden IT-Systeme verwaltet. Cyberkriminelle versuchen diese Konten anzugreifen, um Zugriff auf die Daten und Systeme eines Unternehmens zu erhalten. Zum Schutz des privilegierten Zugriffs empfiehlt es sich, Konten und Systeme zu isolieren, um sie vor dem Zugriff durch böswillige Benutzer zu schützen.

Wir empfehlen Ihnen, eine Roadmap zum Schützen des privilegierten Zugriffs gegenüber Cyberangreifern zu entwickeln und zu befolgen. Informationen zum Erstellen eines detaillierten Plans zum Sichern von Identitäten und Zugriff, die in Azure AD, Microsoft Azure, Office 365 und anderen Clouddiensten verwaltet oder berichtet werden, finden Sie unter [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

Im Folgenden werden die Best Practices in [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md) zusammengefasst:

**Bewährte Methode**: Verwalten, steuern und überwachen Sie den Zugriff auf privilegierte Konten.   
**Detail**: Aktivieren Sie [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). Nach der Aktivierung von Privileged Identity Management erhalten Sie Benachrichtigungs-E-Mails zu Rollenänderungen bei privilegiertem Zugriff. Diese Benachrichtigungen sind eine frühzeitige Warnung, wenn zusätzliche Benutzer hoch privilegierten Rollen in Ihrem Verzeichnis hinzugefügt werden.

**Bewährte Methode**: Identifizieren und kategorisieren Sie Konten in stark privilegierten Rollen.   
**Detail**: Zeigen Sie nach dem Aktivieren von Azure AD Privileged Identity Management die Benutzer an, die die Rollen globaler Administrator, privilegierter Rollenadministrator und andere hoch privilegierte Rollen einnehmen. Entfernen Sie alle Konten, die nicht mehr in diesen Rollen benötigt werden, und kategorisieren Sie die restlichen Konten, die Administratorrollen zugewiesen sind:

- Einzeln Benutzern mit Administratorrechten zugewiesen, und kann nicht zu administrativen Zwecken (z.B. persönliche E-Mail) verwendet werden
- Einzeln Benutzern mit Administratorrechten zugewiesen und nur für Verwaltungszwecke vorgesehen
- Für mehrere Benutzer freigegeben
- Für Notfallzugriffs-Szenarios
- Für automatisierte Skripts
- Für externe Benutzer

**Bewährte Methode**: Implementieren Sie Just-in-Time-Zugriff (JIT) zur weiteren Reduzierung der Offenlegungszeit von Berechtigungen und Steigern Ihres Einblicks in die Verwendung privilegierter Konten.   
**Detail**: Azure AD Privileged Identity Management ermöglicht Ihnen Folgendes:

- Beschränken der Nutzung von Benutzerberechtigungen auf JIT.
- Zuweisen von Rollen für eine verkürzte Dauer mit der Gewissheit, dass die Berechtigungen automatisch widerrufen werden.

**Bewährte Methode**: Definieren Sie mindestens zwei Notfallzugriffskonten.   
**Detail**: Notfallzugriffskonten unterstützen Organisationen dabei, den privilegierten Zugriff innerhalb einer vorhandenen Azure Active Directory-Umgebung einzuschränken. Diese Konten verfügen über weitreichende Privilegien und werden keinen Einzelpersonen zugewiesen. Konten für den Notfallzugriff sind auf Szenarios beschränkt, in denen normale Administratorkonten nicht verwendet werden können. Organisationen müssen die Notfallkontennutzung auf die erforderliche Zeitspanne begrenzen.

Bewerten Sie die Konten, die zugewiesen werden oder für die Rolle „globaler Administrator“ berechtigt sind. Wenn Ihnen keine ausschließlichen Cloudkonten mit der `*.onmicrosoft.com`-Domäne (vorgesehen für den Notfallzugriff) angezeigt werden, erstellen Sie sie. Weitere Informationen finden Sie unter „Verwalten von Administratorkonten für den Notfallzugriff in Azure AD“.

**Bewährte Methode**: Aktivieren Sie die mehrstufige Authentifizierung, und registrieren Sie alle anderen nicht verbundenen Einzelbenutzer-Administratorkonten mit hohen Privilegien.  
**Detail**: Setzen Sie die mehrstufige Azure-Authentifizierung (Multi-Factor Authentication, MFA) bei der Anmeldung für alle einzelnen Benutzer voraus, die dauerhaft einer oder mehreren Azure AD-Administratorrollen zugewiesen sind: globaler Administrator, privilegierter Rollenadministrator, Exchange Online-Administrator und SharePoint Online-Administrator. Nutzen Sie den Leitfaden zum Aktivieren einer [mehrstufigen Authentifizierung für Ihre Administratorkonten](../active-directory/authentication/howto-mfa-userstates.md), und stellen Sie sicher, dass alle Benutzer [registriert](https://aka.ms/mfasetup) sind.

**Bewährte Methode**: Führen Sie Maßnahmen zum Abwehren der am häufigsten verwendeten Angriffstechniken durch.  
**Detail**: [Identifizieren Sie Microsoft-Konten in Administratorrollen, die auf Geschäfts-, Schul- oder Unikonten umgestellt werden müssen.](../active-directory/users-groups-roles/directory-admin-roles-secure.md#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

[Sicherstellen von separaten Benutzerkonten und E-Mail-Weiterleitung für globale Administratorkonten](../active-directory/users-groups-roles/directory-admin-roles-secure.md)  

[Stellen Sie sicher, dass die Kennwörter von Administratorkonten kürzlich geändert wurden.](../active-directory/users-groups-roles/directory-admin-roles-secure.md#ensure-the-passwords-of-administrative-accounts-have-recently-changed)  

[Aktivieren der Kennworthashsynchronisierung](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)  

[Fordern Sie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) sowohl für Benutzer in allen privilegierten Rollen als auch für Benutzer mit hoher Gefährdung.](../active-directory/users-groups-roles/directory-admin-roles-secure.md#require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users)  

[Abrufen Ihres Office 365 Secure Score (bei Verwendung von Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#obtain-your-office-365-secure-score-if-using-office-365)  

[Leitfaden: Überprüfen der Office 365-Sicherheit und -Konformität (bei Verwendung von Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#review-the-office-365-security-and-compliance-guidance-if-using-office-365)  

[Konfigurieren der Office 365-Aktivitätsüberwachung (bei Verwendung von Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#configure-office-365-activity-monitoring-if-using-office-365)  

[Einrichten von Notfallreaktionsplan-Besitzern](../active-directory/users-groups-roles/directory-admin-roles-secure.md#establish-incidentemergency-response-plan-owners)  

[Schützen Sie lokale privilegierte Administratorkonten.](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)

Wenn Sie privilegierten Zugriff nicht schützen, verfügen Sie möglicherweise über zu viele Benutzer in stark privilegierten Rollen und sind anfälliger für Angriffe. Böswillige Akteure einschließlich Cyberangreifern haben häufig Administratorkonten und andere Elemente des privilegierten Zugriffs im Visier, um mit Angriffen, deren Ziel der Diebstahl von Anmeldeinformationen ist, Zugriff auf sensible Daten und Systeme zu erhalten.

## <a name="control-locations-where-resources-are-created"></a>Steuern von Standorten, wo Ressourcen erstellt werden

Es ist sehr wichtig, für Cloudbediener die Durchführung von Aufgaben zu ermöglichen und gleichzeitig die Nichteinhaltung von Konventionen zu verhindern, die zum Verwalten der Ressourcen einer Organisation erforderlich sind. Organisationen, die die Orte steuern möchten, an denen Ressourcen erstellt werden, sollten diese Orte hartcodieren.

Sie können mit dem [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) Sicherheitsrichtlinien erstellen, mit deren Definitionen die jeweils verweigerten Aktionen oder Ressourcen beschrieben werden. Sie weisen diese Richtliniendefinitionen dem gewünschten Ziel zu, z.B. einem Abonnement, einer Ressourcengruppe oder einer einzelnen Ressource.

> [!NOTE]
> Sicherheitsrichtlinien sind nicht das gleiche wie RBAC. Tatsächlich nutzen sie RBAC, um Benutzer zum Erstellen dieser Ressourcen zu autorisieren.
>
>

Organisationen, die die Erstellung von Ressourcen nicht steuern, sind anfälliger dafür, dass Benutzer den Dienst durch die Erstellung von mehr Ressourcen als benötigt missbrauchen. Das Absichern der Ressourcenerstellung ist ein wichtiger Schritt zum Schützen eines Szenarios mit mehreren Mandanten.

## <a name="actively-monitor-for-suspicious-activities"></a>Aktives Überwachen auf verdächtige Aktivitäten

Ein aktives Identitätsüberwachungssystem kann schnell verdächtiges Verhalten erkennen und eine Warnung zur weiteren Untersuchung auslösen. Die folgende Tabelle enthält zwei Azure AD-Funktionen, mit denen Organisationen ihre Identitäten überwachen können:

**Bewährte Methode**: Verwenden Sie eine Methode, um Folgendes zu identifizieren:

- Versuch einer Anmeldung [ohne Ablaufverfolgung](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md).
- [Brute-Force](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md)-Angriffe gegen ein bestimmtes Konto.
- Versuch der Anmeldung von mehreren Standorten aus.
- Anmeldungen von [infizierten Geräten](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md).
- Verdächtige IP-Adressen.

**Detail**: Verwenden Sie [Anomalieberichte](../active-directory/active-directory-view-access-usage-reports.md) von Azure AD Premium. Sie müssen über Prozesse und Verfahren verfügen, mit denen IT-Administratoren diese Berichte täglich oder bei Bedarf ausführen können (normalerweise in einem Szenario mit Reaktionen auf Zwischenfälle).

**Bewährte Methode**: Sie haben ein aktives Überwachungssystem, das Sie über Risiken informiert, und können die Risikostufe (hoch, mittel oder niedrig) an Ihre geschäftlichen Anforderungen anpassen.   
**Detail**: Verwenden Sie [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md), das die aktuellen Risiken im eigenen Dashboard markiert und tägliche Zusammenfassungen per E-Mail sendet. Zum Schutz der Identitäten Ihrer Organisation können Sie risikobasierte Richtlinien konfigurieren, die automatisch auf erkannte Probleme reagieren, wenn eine angegebene Risikostufe erreicht ist.

Für Organisationen, die ihre Identitätssysteme nicht aktiv überwachen, besteht das Risiko, dass Anmeldeinformationen kompromittiert werden. Ohne das Wissen, dass diese Anmeldeinformationen für verdächtige Aktivitäten genutzt werden, können Organisationen dieser Art von Bedrohung nicht begegnen.

## <a name="next-step"></a>Nächster Schritt

Weitere bewährte Methoden für die Sicherheit, die Sie beim Entwerfen, Bereitstellen und Verwalten Ihrer Cloudlösungen mithilfe von Azure verwenden können, finden Sie unter [Sicherheit in Azure: bewährte Methoden und Muster](security-best-practices-and-patterns.md).

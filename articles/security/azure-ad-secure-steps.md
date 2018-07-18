---
title: Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur in Azure Active Directory
description: Dieses Dokument beschreibt eine Reihe von wichtigen Aktionen, die Administratoren implementieren sollten, um ihre Organisation mithilfe von Azure AD-Funktionen zu schützen.
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: b15fff6e868bfac973f9d2a7277f0fac1e29d845
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939836"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur

Wenn Sie dieses Dokument lesen, ist Ihnen bereits bewusst, welchen Stellenwert das Thema Sicherheit innehat. Sie sind wahrscheinlich bereits für die Sicherheit Ihrer Organisation verantwortlich. Wenn Sie andere Personen davon überzeugen müssen, wie wichtig Sicherheit ist, geben Sie ihnen die letzte Ausgabe des [Microsoft Intelligence Security-Berichts](https://www.microsoft.com/security/intelligence-report) zu lesen.

Dieses Dokument unterstützt Sie dabei, mithilfe der Funktionen von Azure Active Directory die Sicherheit in Ihrer Organisation zu verbessern. Es enthält eine Checkliste mit fünf Schritten, mit denen Sie Ihre Organisation gegen Cyberangriffe impfen können.

Diese Checkliste erläutert folgende Aspekte und hilft Ihnen dabei, empfohlene wichtige Aktionen zum Schutz Ihrer Organisation sofort auszuführen:

* Verstärken Sie Ihre Anmeldeinformationen.
* Verringern Sie die Angriffsfläche.
* Automatisieren Sie die Reaktion auf Bedrohungen.
* Schärfen Sie Ihr Bewusstsein für Überwachung und Überprüfung.
* Ermöglichen Sie mit Funktionen zur Selbsthilfe eine besser vorhersagbare und umfassendere Sicherheit für Endbenutzer.

> [!NOTE]
> Viele der Empfehlungen in diesem Dokumentation gelten nur für Anwendungen, die zur Verwendung von Azure Active Directory als Identitätsanbieter konfiguriert sind. Durch Konfigurieren der Apps für das einmalige Anmelden profitieren Sie von folgenden Vorteilen: Richtlinien für Anmeldeinformationen, Bedrohungsermittlung, Überwachung, Protokollierung sowie weiteren Features. Das [einmalige Anmelden über Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) ist das Fundament, auf dem all diese Empfehlungen beruhen.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Vor dem Start: Schützen privilegierter Konten mit MFA

Bevor Sie damit beginnen, diese Checkliste durchzuarbeiten, stellen Sie sicher, dass Ihre Organisation nicht bereits gefährdet ist, bevor Sie mit der Checkliste fertig sind. Als Erstes müssen Sie privilegierte Konten schützen.

Angreifer, die Kontrolle über privilegierte Konten erhalten, können enormen Schaden anrichten. Daher ist es ausgesprochen wichtig, zuerst diese Konten zu schützen. Verwenden Sie den [Baselineschutz](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection), um die [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (mehrstufige Authentifizierung, MFA) für alle Administratoren in Ihrer Organisation zu aktivieren und zu erzwingen. Wenn Sie MFA noch nicht implementiert haben, tun Sie das sofort! Dieses Feature hat oberste Priorität.

Alles fertig? Dann beginnen wir mit der Checkliste.

## <a name="step-1---strengthen-your-credentials"></a>Schritt 1: Verstärken Sie Ihre Anmeldeinformationen. 

Die meisten Sicherheitsverletzungen in Unternehmen rühren daher, dass ein Konto mithilfe einer von nur wenigen Methoden gefährdet wurde, beispielsweise durch Kennwortspraying, Breach Replay und Phishing. Mehr über diese Angriffsmethoden erfahren Sie in diesem Video:
> [!VIDEO https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3016/player]

Wenn Benutzer in Ihrem Identitätssystem schwache Kennwörter verwenden und diese nicht mit einer mehrstufigen Authentifizierung verstärken, lautet die Frage nicht, ob oder wann ein Angriff erfolgt, sondern „Wie oft?“.

### <a name="make-sure-your-organization-use-strong-authentication"></a>Sicherstellen, dass Ihre Organisation eine starke Authentifizierung verwendet

Angesichts der Häufigkeit, mit der Kennwörter erraten, per Phishing herausgefunden, mithilfe von Schadsoftware gestohlen oder einfach wiederverwendet werden, ist es von entscheidender Bedeutung, Kennwörter mit sicheren Anmeldeinformationen zu stärken. Erfahren Sie mehr über die [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="turn-off-traditional-complexity-expiration-rules-and-start-banning-commonly-attacked-passwords-instead"></a>Schluss mit herkömmlichen Komplexitätsanforderungen und Ablaufregeln – sperren Sie stattdessen angriffsanfällige Kennwörter

Viele Organisationen verwenden die herkömmliche Kennwortkomplexität (z.B. Sonderzeichen) und Regeln für den Ablauf von Kennwörtern. Forschungsergebnisse von Microsoft zeigen, dass diese Richtlinien schädlich sind, da sie Benutzer dazu veranlassen, Kennwörter auszuwählen, die verhältnismäßig einfach zu erraten sind.

Microsoft empfiehlt in Übereinstimmung mit den [NIST Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html), folgende drei Richtlinien zu implementieren:

1. Fordern Sie Kennwörter mit mindestens 8 Zeichen. Längere Kennwörter sind nicht unbedingt besser, da sie dazu führen, dass Benutzer vorhersagbare Kennwörter verwenden oder ihre Kennwörter in Dateien speichern oder sich aufschreiben.
2. Deaktivieren Sie Ablaufregeln – diese veranlassen Benutzer dazu, einfach zu erratende Kennwörter wie **Sommer2018!** zu verwenden.
3. Deaktivieren Sie Anforderungen an die Zeichenzusammensetzung, und hindern Sie Benutzer daran, angriffsanfällige Kennwörter zu verwenden – dies führt dazu, dass Benutzer vorhersagbare Zeichenersetzungen in Kennwörtern verwenden.

Wenn Sie Identitäten direkt in Azure AD erstellen, können Sie den [Kennwortablauf mithilfe von PowerShell verhindern](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy). Organisationen, die eine lokale Active Directory-Instanz mit Azure AD Connect verwenden, um Identitäten in Azure AD zu synchronisieren (dies wird auch als Hybridbereitstellung bezeichnet), sollten lokale [intelligente Kennwortrichtlinien](https://aka.ms/passwordguidance) implementieren. Verwenden Sie dazu die [Domänengruppen-Richtlinieneinstellungen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) oder [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

Das Azure Active Directory-Feature der [dynamisch gesperrten Kennwörter](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords) nutzt aktuelle Informationen zum Angreiferverhalten, um Benutzer daran zu hindern, leicht zu erratende Kennwörter festzulegen. Dieses Feature ist jederzeit aktiv. Organisationen mit Hybridbereitstellungen können von diesem Feature profitieren, indem sie das [Kennwortrückschreiben](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-writeback) aktivieren. Alternativ dazu können sie den [AD-Kennwortschutz für Windows Server Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) bereitstellen. Der Azure AD-Kennwortschutz hindert Benutzer daran, ganz allgemein häufige Kennwörter auszuwählen. Zudem können Sie benutzerdefinierte Kennwörter konfigurieren, die ebenfalls nicht verwendet werden dürfen.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Schutz vor kompromittierten Anmeldeinformationen und Verstärken der Ausfallsicherung

Wenn Ihre Organisation eine hybride Identitätslösung verwendet, sollten Sie aus den beiden folgenden Gründen die Kennworthashsynchronisierung aktivieren:

* Der Bericht zu [Benutzern mit kompromittierten Anmeldeinformationen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) in der Azure AD-Verwaltung warnt Sie vor Kombinationen aus Benutzername und Kennwort, die im „Dark Web“ verbreitet wurden. Eine schier unglaubliche Menge an Kennwörtern wird über drei Wege kompromittiert: Phishing, Schadsoftware und die Wiederverwendung von Kennwörtern auf Drittanbieterwebsites, die später gehackt werden. Microsoft findet eine Vielzahl dieser kompromittierten Anmeldeinformationen und teilt Ihnen in diesem Bericht mit, wenn sie Anmeldeinformationen in Ihrer Organisation entsprechen – aber nur, wenn Sie die [Kennworthashsynchronisierung aktivieren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* Sollte ein lokales System ausfallen (beispielsweise aufgrund eines Ransomwareangriffs), können Sie zur [Cloudauthentifizierung mit Kennworthashsynchronisierung](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) wechseln. Diese Backupmethode für die Authentifizierung ermöglicht es, weiterhin auf Apps zuzugreifen, die für die Authentifizierung mit Azure Active Directory konfiguriert sind, beispielsweise Office 365.

Informieren Sie sich, wie die [Kennworthashsynchronisierung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) funktioniert.

### <a name="implement-ad-fs-extranet-lockout"></a>Implementieren der AD FS-Extranetsperre

Organisationen, die Anwendungen für die direkte Authentifizierung in Azure AD konfigurieren, profitieren von der [intelligenten Kennwortsperrung von Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). Wenn Sie AD FS verwenden, implementieren Sie die AD FS-[Extranetsperre](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection). Die Extranetsperre schützt vor Brute-Force-Angriffen auf AD FS und verhindert gleichzeitig, dass Benutzer aus Active Directory ausgesperrt werden.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Die Vorteile von in sich sicheren, einfacher verwendbaren Anmeldeinformationen

Über [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) können Sie Kennwörter durch eine starke zweistufige Authentifizierung auf PCs und Mobilgeräten ersetzen. Diese Authentifizierung besteht aus einer neuen Form von Anmeldeinformationen, die an ein Gerät gebunden sind und biometrische Daten oder eine PIN verwenden.

## <a name="step-2---reduce-your-attack-surface"></a>Schritt 2: Verringern Sie die Angriffsfläche.

Angesichts des Ausmaßes an Schaden, den gehackte Kennwörter anrichten, ist es von entscheidender Bedeutung, die Angriffsfläche in Ihrer Organisation so klein wie möglich zu halten. Folgende Maßnahmen helfen dabei, die Angriffsfläche zu verringern: Außerbetriebnahme älterer, unsicherer Protokolle, Beschränken des Zugriffs auf Einstiegspunkte und eine schärfere Kontrolle des administrativen Zugriffs auf Ressourcen.

### <a name="block-legacy-authentication"></a>Blockieren älterer Authentifizierungsmethoden

Apps, die eigene, veraltete Methoden zur Authentifizierung bei Azure AD und zum Zugriff auf Unternehmensdaten verwenden, stellen für Organisationen ebenfalls ein Risiko dar. POP3-, IMAP4- oder SMTP-Clients sind einige Beispiele für Apps, die ältere Authentifizierungsmethoden verwenden. Solche Apps authentifizieren sich im Namen des Benutzers und hindern Azure AD daran, moderne Sicherheitsauswertungen durchzuführen. Moderne Authentifizierungsmethoden dagegen senken das Sicherheitsrisiko, weil sie die mehrstufige Authentifizierung und den bedingten Zugriff unterstützen. Wir empfehlen die folgenden drei Maßnahmen:

1. Blockieren Sie [ältere Authentifizierungsmethoden, wenn Sie AD FS verwenden](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Richten Sie [SharePoint Online und Exchange Online für moderne Authentifizierungsverfahren](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication) ein.
3. Verwenden Sie [Richtlinien für den bedingten Zugriff, um ältere Authentifizierungsmethoden zu blockieren](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication).

### <a name="block-invalid-authentication-entry-points"></a>Blockieren von ungültigen Authentifizierungseinstiegspunkten

Sie müssen grundsätzlich davon ausgehen, dass Sicherheitsverletzungen passieren. Vor diesem Hintergrund müssen Sie die Auswirkungen gehackter Benutzeranmeldeinformationen begrenzen, sobald die Sicherheitsverletzung aufgetreten ist. Berücksichtigen Sie für jede App in Ihrer Umgebung die gültigen Anwendungsfälle: Welche Gruppen, welche Netzwerke, welche Geräte und welche weiteren Elemente sind autorisiert? Blockieren Sie den Rest. Achten Sie darauf, die Verwendung von [Konten mit umfangreichen Berechtigungen oder Dienstkonten](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) zu beschränken. Mit dem [bedingten Zugriff von Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) können Sie bestimmte Bedingungen definieren und so steuern, wie autorisierte Benutzer auf ihre Apps und Ressourcen zugreifen.

Achten Sie besonders auf Dienstkonten (Konten, mit denen Aufgaben automatisiert ausgeführt werden). Mit dem bedingten Zugriff können Sie sicherstellen, dass solche Konten nur für den Dienst, von der IP-Adresse und zu der Tageszeit ausgeführt werden, der bzw. die angemessen ist.

### <a name="implement-azure-ad-privileged-identity-management"></a>Implementieren von Azure AD Privileged Identity Management

Die grundsätzliche Annahme, dass Sicherheitsverletzungen passieren, führt auch dazu, dass Sie die Wahrscheinlichkeit reduzieren müssen, dass ein gehacktes Konto mit einer privilegierten Rolle ausgeführt werden kann. 

[Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) unterstützt Sie dabei, folgende Aktionen auszuführen, um Kontoberechtigungen so weit wie möglich einzuschränken:

* Identifizieren und verwalten Sie Benutzer, denen Administratorrollen zugewiesen sind.
* Ermitteln Sie nicht verwendete Rollen oder Rollen mit übermäßigen Berechtigungen, die Sie entfernen sollten.
* Richten Sie Regeln ein, die sicherstellen, dass privilegierte Rollen durch mehrstufige Authentifizierung geschützt sind.
* Richten Sie Regeln ein, die sicherstellen, dass privilegierte Rollen nur so lange gewährt werden, wie zum Ausführen der entsprechenden Aufgabe erforderlich ist.

Aktivieren Sie Azure AD PIM, sehen Sie sich die Benutzer an, denen Administratorrollen zugewiesen sind, und entfernen Sie nicht benötigte Konten in diesen Rollen. Ändern Sie die Berechtigungen für die verbleibenden privilegierten Benutzer von „dauerhaft“ zu „berechtigt“. Zum Schluss richten Sie geeignete Richtlinien ein, um sicherzustellen, dass Benutzer sicher auf diese privilegierten Rollen zugreifen können, wenn es erforderlich ist.

Wenn Sie den Prozess für die Bereitstellung privilegierter Konten einrichten, befolgen Sie die [bewährten Methoden zum Einrichten von mindestens zwei Notfallkonten](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices), um sicherzustellen, dass Sie weiterhin Zugriff auf Azure AD haben, falls Sie sich selbst ausgesperrt haben.

## <a name="step-3---automate-threat-response"></a>Schritt 3: Automatisieren Sie die Reaktion auf Bedrohungen.

Azure Active Directory verfügt über eine Vielzahl von Funktionen, die Angriffe automatisch abfangen, sodass zwischen Erkennung und Reaktion keinerlei Zeit vergeht. Sie können Kosten und Risiken verringern, indem Sie den Zeitraum verkürzen, in dem sich Angreifer in Ihrer Umgebung einnisten können. Im Folgenden finden Sie die konkreten Schritte, die Sie ausführen sollten.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Implementieren einer Sicherheitsrichtlinie in Bezug auf das Benutzerrisiko mithilfe von Azure AD Identity Protection

Das Benutzerrisiko bezeichnet die Wahrscheinlichkeit, dass die Identität eines Benutzers gefährdet ist, und wird anhand der [Benutzerrisikoereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) berechnet, die der Identität eines Benutzers zugeordnet sind. Bei einer Benutzerrisikorichtlinie handelt es sich um eine Richtlinie für bedingten Zugriff, die die Risikostufe eines bestimmten Benutzers oder einer bestimmten Gruppe auswertet. Basierend auf den Risikostufen „Niedrig“, „Mittel“ oder „Hoch“ kann eine Richtlinie so konfiguriert werden, dass sie den Zugriff blockiert oder eine sichere Kennwortänderung über die mehrstufige Authentifizierung erfordert. Microsoft empfiehlt, bei Benutzern mit hohem Risiko eine sichere Kennwortänderung anzufordern.

![Benutzer mit Risikokennzeichnung](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Implementieren einer Richtlinie in Bezug auf das Anmelderisiko mithilfe von Azure AD Identity Protection

Das Anmelderisiko bezeichnet die Wahrscheinlichkeit, dass eine Person, bei der es sich nicht um den Kontobesitzer handelt, versucht, sich mit dieser Identität anzumelden. Bei einer [Richtlinie in Bezug auf das Anmelderisiko](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) handelt es sich um eine Richtlinie für bedingten Zugriff, die die Risikostufe für einen bestimmten Benutzer oder eine bestimmte Gruppe auswertet. Basierend auf der Risikostufe (hoch/mittel/niedrig) kann eine Richtlinie konfiguriert werden, die den Zugriff blockiert oder die mehrstufige Authentifizierung erzwingt. Stellen Sie sicher, dass Sie für Anmeldevorgänge mit der Stufe „mittel“ oder höher die mehrstufige Authentifizierung erzwingen.

![Anmelden von anonymen IP-Adressen](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>Schritt 4: Schärfen Sie Ihr Bewusstsein.

Die Überwachung und Protokollierung von sicherheitsbezogenen Ereignissen und die entsprechenden Warnungen sind grundlegende Komponenten einer effektiven Schutzstrategie. Sicherheitsprotokolle und Berichte bieten eine elektronische Aufzeichnung verdächtiger Aktivitäten und helfen Ihnen bei der Erkennung von Mustern, die auf versuchte oder erfolgreiche externe Eindringversuche in das Netzwerk und auf interne Angriffe hinweisen können. Sie können im Rahmen der Überwachung entsprechende Benutzeraktivitäten überwachen, die Einhaltung gesetzlicher Bestimmungen dokumentieren, forensische Analyse durchführen und vieles mehr. Warnungen benachrichtigen Sie bei Sicherheitsereignissen.

### <a name="monitor-azure-ad"></a>Überwachen von Azure AD

Die Dienste und Features von Microsoft Azure bieten Ihnen konfigurierbare Optionen für die Überwachung und Protokollierung der Sicherheit, damit Sie Lücken in Ihren Sicherheitsrichtlinien und -mechanismen identifizieren und schließen können, um Sicherheitsverletzungen zu vermeiden. Sie können die [Azure-Protokollierung und -Überwachung](https://docs.microsoft.com/azure/security/azure-log-audit) sowie [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) verwenden.

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Überwachen von Azure AD Connect Health in hybriden Umgebungen

Unter [Überwachen von AD FS mithilfe von Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) finden Sie umfassende Einblicke in mögliche Probleme und die Sichtbarkeit von Angriffen auf Ihre AD FS-Infrastruktur. Azure AD Connect Health bietet Warnungen mit detaillierten Informationen, Lösungsschritten und Links zur entsprechenden Dokumentation sowie Nutzungsanalysen für verschiedene Metriken zum Authentifizierungsdatenverkehr, Funktionen für die Leistungsüberwachung und Berichte.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Überwachen von Azure AD Identity Protection-Ereignissen

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) ist ein Benachrichtigungs-, Überwachungs- und Berichterstattungstool, mit dem Sie potenzielle Sicherheitslücken erkennen können, die Auswirkungen auf die Identitäten Ihrer Organisation haben können. Das Tool erkennt Risikoereignisse, wie z.B. kompromittierte Anmeldeinformationen, unmögliche Ortswechsel und Anmeldungen von infizierten Geräten sowie anonyme IP-Adressen, IP-Adressen in Zusammenhang mit der verdächtigen Aktivität und unbekannte Standorte. Aktivieren Sie die Benachrichtigungen, um E-Mails zu gefährdeten Benutzern und/oder eine wöchentliche Zusammenfassung zu erhalten.

![Benutzer mit Risikokennzeichnung](media/azure-ad/azure-ad-sec-steps3.png)

## <a name="step-5---enable-end-user-self-help"></a>Schritt 5: Aktivieren der Selbsthilfe für Benutzer

Ein wichtiges Ziel ist es, Sicherheit und Produktivität sorgfältig gegeneinander abzuwägen. Der Grundgedanke ist hier – ebenso wie beim Aufbau eines langfristig tragfähigen Fundaments für die Sicherheit –, dass Sie den Betriebsablauf für Ihre Organisation so reibungslos wie möglich gestalten möchten, indem Sie Ihren Benutzern mehr Selbstständigkeit bieten, aber gleichzeitig immer wachsam bleiben. 

### <a name="implement-self-service-password-reset"></a>Implementieren der Self-Service-Kennwortzurücksetzung

Die [Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) (Self-Service Password Reset, SSPR) von Azure bietet für IT-Administratoren eine einfache Methode, Benutzern das Zurücksetzen oder Entsperren ihrer Kennwörter oder Konten zu ermöglichen, ohne dass ein Administrator eingreifen muss. Das System verfügt über eine ausführliche Berichterstellung, bei der der Benutzerzugriff auf das System nachverfolgt wird, sowie über eine Benachrichtigungsfunktion, mit der Sie über eine fehlerhafte oder missbräuchliche Nutzung informiert werden. 

### <a name="implement-self-service-group-management"></a>Implementieren der Self-Service-Gruppenverwaltung

Azure AD bietet die Möglichkeit, den Zugriffs auf Ressourcen mithilfe von Sicherheitsgruppen und Office 365-Gruppen zu verwalten. Diese Gruppen können von Gruppenbesitzern anstatt von IT-Administratoren verwaltet werden. Dieses als [Self-Service-Gruppenverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) bezeichnete Feature ermöglicht es Gruppenbesitzern, denen keine Administratorrolle zugewiesen ist, Gruppen zu erstellen und zu verwalten, ohne dass Administratoren ihre Anforderungen verarbeiten müssen.

### <a name="implement-azure-ad-access-reviews"></a>Implementieren von Azure AD-Zugriffsüberprüfungen

Mit [Azure AD-Zugriffsüberprüfungen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) können Sie Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und die Zuweisungen privilegierter Rollen verwalten, um einen Sicherheitsstandard einzurichten, der Benutzern nur dann Zugriff gewährt, wenn sie diesen benötigen.

## <a name="summary"></a>Zusammenfassung

Bei sicheren Identitätsinfrastrukturen gilt es eine Vielzahl von Aspekten zu berücksichtigen, aber mit den fünf Schritten dieser Checkliste können Sie schnell eine sicherere Infrastruktur erzielen:

* Verstärken Sie Ihre Anmeldeinformationen.
* Verringern Sie die Angriffsfläche.
* Automatisieren Sie die Reaktion auf Bedrohungen.
* Schärfen Sie Ihr Bewusstsein für Überwachung und Überprüfung.
* Ermöglichen Sie mit Funktionen zur Selbsthilfe eine besser vorhersagbare und umfassendere Sicherheit für Endbenutzer.

Wir freuen uns, dass Sie die Sicherheit von Identitäten so ernst nehmen, und hoffen, dass Sie mithilfe des vorliegenden Dokuments den Sicherheitsstatus Ihrer Organisation stärken können.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Unterstützung bei der Planung und Umsetzung der Empfehlungen benötigen, sehen Sie sich die [Azure AD-Projektbereitstellungspläne](http://aka.ms/deploymentplans) an.

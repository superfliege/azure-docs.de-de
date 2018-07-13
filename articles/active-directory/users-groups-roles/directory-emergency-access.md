---
title: Verwalten von Administratorkonten für den Notfallzugriff in Azure AD | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Konten für den Notfallzugriff verwenden, um Organisationen dabei zu unterstützen, den privilegierten Zugriff innerhalb einer vorhandenen Azure Active Directory-Umgebung einzuschränken.
services: active-directory
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 4f3772abc1cdbd3b35b8b1f16e7a47c0f1a17783
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38595653"
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Verwalten von Administratorkonten für den Notfallzugriff in Azure AD 

Für die meisten der täglich anfallenden Aufgaben benötigen Ihre Benutzer keine Rechte als *globaler Administrator*. Benutzern sollte nicht dauerhaft diese Rolle zugewiesen werden, weil sie versehentlich eine Aufgabe mit höheren Berechtigungen als den für sie vorgesehenen ausführen könnten. Wenn Benutzer nicht als globale Administratoren fungieren müssen, sollte die Rollenzuweisung unter Verwendung von Azure Active Directory (Azure AD) Privileged Identity Management (PIM) aktiviert werden – entweder für ihr eigenes Konto oder ein alternatives Administratorkonto.

Sie müssen nicht nur eine missbräuchliche Verwendung von Administratorrechten durch die Benutzer verhindern – Sie müssen auch vermeiden, versehentlich von der Verwaltung Ihres Azure AD-Mandanten ausgeschlossen zu werden, weil Sie sich nicht anmelden oder ein vorhandenes Einzelbenutzerkonto nicht als Administrator aktivieren können. Sie können die Auswirkungen eines versehentlichen Verlusts des Administratorzugriffs abmildern, indem Sie mindestens zwei *Konten für den Notfallzugriff* in Ihrem Mandanten konfigurieren.

Konten für den Notfallzugriff können Organisationen dabei zu unterstützen, den privilegierten Zugriff innerhalb einer vorhandenen Azure Active Directory-Umgebung einzuschränken. Diese Konten verfügen über weitreichende Privilegien und werden keinen Einzelpersonen zugewiesen. Konten für den Notfallzugriff sind auf Notfallsituationen oder Szenarien beschränkt, in denen normale Administratorkonten nicht verwendet werden können. Organisationen müssen sicherstellen, dass die Verwendung der Notfallkonten ausschließlich auf Zeiten beschränkt wird, in denen dies notwendig ist.

Eine Organisation könnte beispielsweise in den folgenden Situationen auf ein Konto für den Notfallzugriff zurückgreifen:

 - Die Benutzerkonten befinden sich in einem Verbund, und der Verbund ist aktuell nicht verfügbar, weil ein Mobilfunknetz oder ein Identitätsanbieter ausgefallen ist. Wenn beispielsweise der Identitätsanbieterhost in Ihrer Umgebung nicht erreichbar ist, können sich die Benutzer möglicherweise nicht anmelden, wenn sie von Azure AD an ihren Identitätsanbieter umgeleitet werden. 
 - Die Administratoren sind über Azure MFA (Multi-Factor Authentication) registriert, und keines der von ihnen verwendeten Geräte ist verfügbar. Benutzer können möglicherweise keine mehrstufige Authentifizierung durchführen, um eine Rolle zu aktivieren. Beispielsweise können bei einem Ausfall des Mobilfunknetzes keine Anrufe entgegengenommen oder SMS empfangen werden, womit die einzigen registrierten Authentifizierungsmechanismen für ihr Gerät wegfallen. 
 - Die Person, die zuletzt über globalen Administratorzugriff verfügte, hat die Organisation verlassen. Azure AD verhindert, dass das letzte *globale Administratorkonto* gelöscht wird, aber das lokale Löschen oder Deaktivieren des Kontos kann nicht verhindert werden. Beide Situationen können dazu führen, dass die Organisation nicht in der Lage ist, das Konto wiederherzustellen.

## <a name="initial-configuration"></a>Anfängliche Konfiguration

Erstellen Sie mindestens zwei Konten für den Notfallzugriff. Dies sollten reine Cloudkonten sein, die die Domäne \*.onmicrosoft.com verwenden, und bei denen es sich nicht um Verbundkonten oder um Konten handelt, die über eine lokale Umgebung synchronisiert werden. 

Die Konten dürfen keinem Einzelbenutzer in der Organisation zugeordnet werden. Organisationen müssen sicherstellen, dass die Anmeldeinformationen für diese Konten sicher aufbewahrt werden und ausschließlich den Personen bekannt sind, die zu ihrer Verwendung autorisiert sind. 

> [!NOTE]
> Das Kennwort eines Kontos für den Notfallzugriff wird in der Regel in zwei oder drei Teile unterteilt, die separat auf Papier festgehalten und in geschützten, feuerfesten Tresoren hinterlegt werden, die sich an sicheren, getrennten Standorten befinden. 
>
> Stellen Sie sicher, dass Ihre Konten für den Notfallzugriff nicht an Mobiltelefone von Mitarbeitern, Hardwaretoken einzelner Mitarbeitern oder andere mitarbeiterspezifische Anmeldeinformationen gebunden sind. Durch diese Vorsichtsmaßnahme werden Fälle abgedeckt, in denen einzelne Mitarbeiter nicht erreichbar sind, wenn die Anmeldeinformationen benötigt werden. 

### <a name="initial-configuration-with-permanent-assignments"></a>Anfängliche Konfiguration mit dauerhaften Zuweisungen

Eine Option besteht darin, Benutzer dauerhaft als Mitglieder der *globalen Administratorrolle* festzulegen. Diese Option eignet sich für Organisationen, die über keine Azure AD Premium P2-Abonnements verfügen.

Um das Risiko von Angriffen zu verringern, die aus kompromittierten Kennwörtern resultieren, empfiehlt das Azure AD-Team, Multi-Factor Authentication für alle Einzelbenutzer als verbindlich festzulegen. Diese Gruppe sollte Administratoren und alle weiteren Benutzer einschließen (z.B. Mitarbeiter der Finanzabteilung), bei denen ein kompromittiertes Konto erhebliche Auswirkungen haben kann. 

Wenn Ihre Organisation allerdings über keine freigegebenen Geräte verfügt, ist Multi-Factor Authentication für die Konten für den Notfallzugriff möglicherweise keine Option. Wenn Sie eine Richtlinie für den bedingten Zugriff konfigurieren, um die [Multi-Factor Authentication-Registrierung für alle Administratoren](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) für Azure AD und andere verbundene SaaS-Apps (Software-as-a-Service) als verbindlich festzulegen, müssen Sie möglicherweise Richtlinienausschlüsse konfigurieren, um die Konten für den Notfallzugriff von dieser Anforderung auszunehmen.

### <a name="initial-configuration-with-approvals"></a>Anfängliche Konfiguration mit Genehmigungen

Eine weitere Option besteht darin, Ihre Benutzer als berechtigt und Genehmigende zum Aktivieren der Rolle *globalen Administratorrolle* zu konfigurieren. Für diese Option benötigt Ihre Organisation Azure AD Premium P2-Abonnements. Darüber hinaus ist eine Multi-Factor Authentication-Option erforderlich, die für eine gemeinsame Verwendung durch verschiedene Personen und für die Netzwerkumgebung geeignet ist. Diese Anforderungen gelten, weil eine Aktivierung der *globalen Administratorrolle* erfordert, dass die Benutzer zuvor eine mehrstufige Authentifizierung durchlaufen haben. Weitere Informationen finden Sie unter [Erfordern von MFA in Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Bei Konten für den Notfallzugriff wird davon abgeraten, Multi-Factor Authentication mit persönlichen Geräten zu nutzen. Bei einem echten Notfall ist die Person, die Zugriff auf ein für MFA registriertes Gerät benötigt, möglicherweise nicht diejenige, die im Besitz des persönlichen Geräts ist. 

Berücksichtigen Sie außerdem die Bedrohungslandschaft. Beispielsweise könnten unvorhersehbare Ereignisse wie z.B. Naturkatastrophen dazu führen, dass ein Mobiltelefon oder andere Netzwerke nicht verfügbar sind. Es muss unbedingt sichergestellt werden, dass alle registrierten Geräte an einem bekannten, sicheren Ort aufbewahrt werden, die über verschiedene Wege mit Azure AD kommunizieren können.

## <a name="ongoing-monitoring"></a>Fortlaufende Überwachung

Überwachen Sie die [Azure AD-Anmeldungs- und Überwachungsprotokolle](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) auf Anmeldungen, und überwachen Sie die Aktivität der Konten für den Notfallzugriff. Normalerweise sollte über diese Konten keine Anmeldungen und keine Änderungen erfolgen, deshalb ist ihre Verwendung verdächtig und bedarf einer Sicherheitsuntersuchung.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Kontovalidierung in regelmäßigen Abständen

Für die Kontovalidierung sollten die folgenden Richtlinien eingehalten werden:
- Ausführung mindestens alle 90 Tage.
- Ausführung bei einem Wechsel bei den IT-Mitarbeitern, z.B. bei einem Positionswechsel, beim Ausscheiden eines Mitarbeiters oder bei einer Neueinstellung.
- Ausführung bei einer Änderung der Azure AD-Abonnements in der Organisation.

Führen Sie die folgenden Schritte aus, um die Mitarbeiter zur Verwendung der Konten für den Notfallzugriff zu schulen:

* Stellen Sie sicher, dass die für die Sicherheitsüberwachung verantwortlichen Mitarbeiter darüber informiert sind, dass eine Kontoüberprüfung stattfindet.
* Vergewissern Sie sich, dass die Cloudbenutzerkonten sich anmelden und ihre Rollen aktivieren können, und dass Benutzer, die diese Schritte bei einem Notfall ausführen müssen, entsprechend geschult sind.
* Stellen Sie sicher, dass Multi-Factor Authentication oder Self-Service-Kennwortzurücksetzung nicht mit persönlichen Benutzergeräten oder Informationen registriert wurden. 
* Wenn für die Konten eine MFA-Registrierung mit einem Gerät durchgeführt wurde, das zur Rollenaktivierung verwendet werden soll, müssen Sie sicherstellen, dass das Gerät für alle Administratoren zugänglich ist, die es im Notfall möglicherweise benötigen. Stellen Sie außerdem sicher, dass das Gerät über mindestens zwei Mechanismen registriert wurde, die nicht dasselbe Ausfallverhalten zeigen. Beispiel: Das Gerät kann über ein Drahtlosnetzwerk der Organisation und über das Netz eines Mobilfunkanbieters mit dem Internet kommunizieren.
* Aktualisieren Sie die Kontoanmeldeinformationen.

## <a name="next-steps"></a>Nächste Schritte
- [Fügen Sie einen cloudbasierten Benutzer hinzu](../fundamentals/add-users-azure-active-directory.md), und [weisen Sie dem neuen Benutzer die globale Administratorrolle zu](../fundamentals/active-directory-users-assign-role-azure-portal.md).
- [Registrieren Sie sich für Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md), sofern nicht bereits geschehen.
- [Erzwingen Sie Azure Multi-Factor Authentication für Einzelbenutzer, die als Administratoren fungieren](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Konfigurieren Sie zusätzlichen Schutz für globale Administratoren in Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), wenn Sie Office 365 verwenden.
- [Führen Sie eine Zugriffsüberprüfung für globale Administratoren durch](../privileged-identity-management/pim-how-to-start-security-review.md), und [stellen sie vorhandene globale Administratoren auf spezifischere Administratorrollen um](directory-assign-admin-roles.md).



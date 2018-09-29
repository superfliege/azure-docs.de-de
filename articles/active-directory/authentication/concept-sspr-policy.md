---
title: Richtlinien zur Self-Service-Kennwortzurücksetzung in Azure AD
description: Konfigurieren der Richtlinienoptionen zur Self-Service-Kennwortzurücksetzung in Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: ee30ee4fa89ce47e8441845b088919b26ce32b31
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434275"
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Kennwortrichtlinien und -einschränkungen in Azure Active Directory

In diesem Artikel sind die Kennwortrichtlinien und Komplexitätsanforderungen beschrieben, die zu Benutzerkonten gehören, die in Ihrem Azure Active Directory-Mandenten (Azure AD-Mandanten) gespeichert sind.

## <a name="administrator-reset-policy-differences"></a>Unterschiede zu Richtlinien zum Zurücksetzen von Administratorkennwörtern

**Microsoft erzwingt standardmäßig eine starke *Zwei-Gate*-Richtlinie zur Kennwortzurücksetzung für alle Azure-Administratorrollen**. Diese Richtlinie kann von der Richtlinie abweichen, die Sie für Ihre Benutzer definiert haben, und nicht geändert werden. Sie sollten die Funktion zum Zurücksetzen des Kennworts immer als Benutzer ohne zugewiesene Azure-Administratorrollen testen.

Mit einer Zwei-Gate-Richtlinie haben **Administratoren nicht die Möglichkeit, Sicherheitsfragen zu verwenden**.

 Eine Zwei-Gate-Richtlinie erfordert Authentifizierungsdaten, die aus zwei Elementen bestehen, z.B. eine E-Mail-Adresse *und* eine Telefonnummer. Eine Zwei-Gate-Richtlinie gilt in folgenden Situationen:

* Alle folgenden Administratorrollen sind betroffen:
  * Helpdesk-Administrator
  * Dienstunterstützungsadministrator
  * Rechnungsadministrator
  * Partnersupport der Ebene 1
  * Partnersupport der Ebene 2
  * Exchange-Dienstadministrator
  * Lync-Dienstadministrator
  * Benutzerkontoadministrator
  * Verzeichnis schreiben
  * Globaler Administrator oder Unternehmensadministrator
  * SharePoint-Dienstadministrator
  * Complianceadministrator
  * Anwendungsadministrator
  * Sicherheitsadministrator
  * Administrator für privilegierte Rollen
  * Microsoft Intune-Dienstadministrator
  * Anwendungsproxy-Dienstadministrator
  * CRM-Dienstadministrator
  * Power BI-Dienstadministrator

* Wenn 30 Tage in einem Testabonnement abgelaufen sind.

  oder

* Es ist eine benutzerdefinierte Domäne vorhanden, z.B. contoso.com.

  oder

* Azure AD Connect synchronisiert Identitäten aus Ihrem lokalen Verzeichnis.

### <a name="exceptions"></a>Ausnahmen

Eine Ein-Gate-Richtlinie erfordert Authentifizierungsdaten, die aus einem Element bestehen, z.B. eine E-Mail-Adresse *oder* eine Telefonnummer. Eine Ein-Gate-Richtlinie gilt in folgenden Situationen:

* Für ein Testabonnement sind noch keine 30 Tage vergangen.

  oder

* Es ist keine benutzerdefinierte Domäne vorhanden (*.onmicrosoft.com).

  and

  Azure AD Connect synchronisiert keine Identitäten.

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName-Richtlinien, die für alle Benutzerkonten gelten

Jedes Benutzerkonto, das sich bei Azure AD anmeldet, muss über ein eindeutiges, diesem Konto zugeordnetes Benutzerprinzipalnamen-Attribut (UPN, User Principal Name) verfügen. In der folgenden Tabelle sind die Richtlinien aufgeführt, die sowohl für lokale Active Directory-Benutzerkonten, die mit der Cloud synchronisiert werden, als auch für Benutzerkonten, die ausschließlich in der Cloud verwendet werden, gelten:

| Eigenschaft | UserPrincipalName-Richtlinien |
| --- | --- |
| Zulässige Zeichen |<ul> <li>A – Z</li> <li>a – z</li><li>0 – 9</li> <li> \. - \_ ! \# ^ \~</li></ul> |
| Unzulässige Zeichen |<ul> <li>Jedes\@\"-Zeichen, das nicht den Benutzernamen und die Domäne trennt.</li> <li>Darf keinen Punkt (.) unmittelbar vor dem \@\"-Symbol enthalten.</li></ul> |
| Längenbeschränkungen |<ul> <li>Die Gesamtlänge darf 113 Zeichen nicht überschreiten.</li><li>Vor dem \@\"-Symbol sind bis zu 64 Zeichen zulässig.</li><li>Nach dem \@\"-Symbol sind bis zu 48 Zeichen zulässig.</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Kennwortrichtlinien, die nur für Cloudbenutzerkonten gelten

In der folgenden Tabelle sind die verfügbaren Kennwortrichtlinieneinstellungen beschrieben, die auf die in Azure AD erstellten und verwalteten Benutzerkonten angewendet werden können:

| Eigenschaft | Requirements (Anforderungen) |
| --- | --- |
| Zulässige Zeichen |<ul><li>A – Z</li><li>a – z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ " ( ) ;</li></ul> |
| Unzulässige Zeichen |<ul><li>Unicode-Zeichen</li><li>Leerzeichen</li><li> Nur sichere Kennwörter</li></ul> |
| Kennworteinschränkungen |<ul><li>Mindestens 8 Zeichen und höchstens 16 Zeichen.</li><li>Nur für sichere Kennwörter: Muss drei der folgenden vier Elemente enthalten:<ul><li>Kleinbuchstaben</li><li>Großbuchstaben</li><li>Zahlen (0 bis 9)</li><li>Symbole (siehe die vorherigen Kennworteinschränkungen)</li></ul></li></ul> |
| Zeitraum bis zum Ablauf des Kennworts |<ul><li>Standardwert: **90** Tage</li><li>Der Wert kann im Azure Active Directory-Modul für Windows PowerShell mit dem Cmdlet `Set-MsolPasswordPolicy` konfiguriert werden.</li></ul> |
| Benachrichtigung bei Ablauf des Kennworts |<ul><li>Standardwert: **14** Tage (bevor das Kennwort abläuft)</li><li>Der Wert kann mit dem Cmdlet `Set-MsolPasswordPolicy` konfiguriert werden.</li></ul> |
| Ablauf des Kennworts |<ul><li>Standardwert: **false** Tage (gibt an, dass Kennwortablauf aktiviert ist)</li><li>Der Wert kann mit dem Cmdlet `Set-MsolUser` für einzelne Benutzerkonten konfiguriert werden.</li></ul> |
| Verlauf der Kennwortänderungen |Das letzte Kennwort *kann nicht* erneut verwendet werden, wenn der Benutzer ein Kennwort ändert. |
| Verlauf der Kennwortzurücksetzungen | Das letzte Kennwort *kann* erneut verwendet werden, wenn der Benutzer ein vergessenes Kennwort zurücksetzt. |
| Kontosperrung |Nach 10 nicht erfolgreichen Anmeldeversuchen mit einem falschen Kennwort wird der Benutzer für eine Minute gesperrt. Mit jeder weiteren fehlerhaften Anmeldeversuch verlängert sich die Dauer, die der Benutzer gesperrt ist. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Festlegen von Kennwortablaufrichtlinien in Azure AD

Ein globaler Administrator für einen Microsoft-Clouddienst kann mit dem Microsoft Azure Active Directory-Modul für Windows PowerShell festlegen, dass Benutzerkennwörter nicht ablaufen. Sie können auch Windows PowerShell-Cmdlets verwenden, um die Konfiguration zu entfernen, die angibt, dass Kennwörter nie ablaufen, oder um Benutzerkennwörter anzuzeigen, für die festgelegt ist, dass sie nie ablaufen. 

Diese Anleitung gilt für andere Anbieter wie Intune und Office 365, die ebenfalls auf Azure AD als Identitäts- und Verzeichnisdienste zurückgreifen. Kennwortablauf ist der einzige Teil der Richtlinie, der geändert werden kann.

> [!NOTE]
> Nur Kennwörter für Benutzerkonten, die nicht über die Verzeichnissynchronisierung synchronisiert werden, können so konfiguriert werden, dass sie nicht ablaufen. Weitere Informationen zur Verzeichnissynchronisierung finden Sie unter [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>Festlegen oder Überprüfen der Kennwortrichtlinien mithilfe von PowerShell

Zunächst müssen Sie [das Azure AD PowerShell-Modul herunterladen und installieren](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Nachdem Sie das Modul installiert haben, können Sie jedes Feld mit den folgenden Schritten konfigurieren.

### <a name="check-the-expiration-policy-for-a-password"></a>Überprüfen der Ablaufrichtlinie für ein Kennwort

1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Befehle aus:

   * Um festzustellen, ob für das Kennwort eines bestimmten Benutzers festgelegt ist, dass es nie abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN, z. B. *aprilr@contoso.onmicrosoft.com*) oder der Benutzer-ID des zu überprüfenden Benutzers aus: `Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Um die Einstellung **Kennwort läuft nie ab** für alle Benutzer anzuzeigen, führen Sie das folgende Cmdlet aus: `Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Festlegen, dass ein Kennwort abläuft

1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Befehle aus:

   * Um für das Kennwort eines Benutzers festzulegen, dass es abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN) oder der Benutzer-ID des Benutzers aus: `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Um für die Kennwörter aller Benutzer in der Organisation festzulegen, dass sie ablaufen, verwenden Sie das folgende Cmdlet: `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Festlegen, dass ein Kennwort nicht abläuft

1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Befehle aus:

   * Um für das Kennwort eines Benutzers festzulegen, dass es nie abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN) oder der Benutzer-ID des Benutzers aus: `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Um für die Kennwörter aller Benutzer in einer Organisation festzulegen, dass sie nie ablaufen, führen Sie das folgende Cmdlet aus: `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

   > [!WARNING]
   > Kennwörter, die auf `-PasswordNeverExpires $true` festgelegt sind, altern trotzdem entsprechend dem `pwdLastSet`-Attribut. Wenn Sie für die Benutzerkennwörter festlegen, dass sie nie ablaufen, und dann 90+ Tage vergangen sind, laufen die Kennwörter ab. Entsprechend dem `pwdLastSet`-Attribut ergibt sich, wenn Sie das Ablaufen in `-PasswordNeverExpires $false` ändern, dass jedes Kennwort, dessen `pwdLastSet` älter als 90 Tage ist, vom Benutzer bei seiner nächster Anmeldung geändert werden muss. Diese Änderung kann eine große Anzahl von Benutzern betreffen. 

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Informationen zur Kennwortzurücksetzung über Azure AD:

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md)
* [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](../user-help/active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](concept-sspr-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](howto-sspr-authenticationdata.md)
* [Welche Authentifizierungsmethoden sind für Benutzer verfügbar?](concept-sspr-howitworks.md#authentication-methods)
* [Worum handelt es sich beim Rückschreiben von Kennwörtern, und warum sollte ich mir Gedanken darüber machen?](howto-sspr-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](howto-sspr-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](concept-sspr-howitworks.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

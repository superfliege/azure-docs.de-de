---
title: Zugriffs- und Nutzungsberichte für Azure MFA| Microsoft Docs
description: Beschreibt, wie Sie das Berichte-Feature für Multi-Factor Authentication verwenden.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 92c319460d9412436eb1a474431c07766fa87176
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078898"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Berichte in Azure Multi-Factor Authentication

Azure Multi-Factor Authentication bietet verschiedene Berichte, die Sie und Ihre Organisation im Azure-Portal verwenden können. In der folgenden Tabelle sind die verfügbaren Berichte aufgeführt:

| Bericht | Standort | BESCHREIBUNG |
|:--- |:--- |:--- |
| Verlauf – gesperrte Benutzer | Azure AD > MFA-Server > Benutzer blockieren/entsperren | Zeigt die Liste der Anforderungen zum Blockieren und Entsperren von Benutzern an. |
| Nutzung und Betrugswarnungen | Azure AD > Anmeldungen | Bietet Informationen zur Gesamtnutzung, Übersichts- und Detailinformationen zu Benutzern sowie einen Verlauf von Betrugswarnungen, die im angegebenen Zeitraum gesendet wurden. |
| Nutzung für lokale Komponenten | Azure AD > MFA-Server > Aktivitätsbericht | Bietet Informationen zur Gesamtnutzung für MFA durch die NPS-Erweiterung, AD FS und den MFA-Server. |
| Verlauf – Umgangene Benutzer | Azure AD > MFA-Server > Einmalumgehung | Zeigt den Verlauf von Anforderungen zur Umgehung der Multi-Factor Authentication für einen Benutzer an. |
| Serverstatus | Azure AD > MFA-Server > Serverstatus | Zeigt den Status von Multi-Factor Authentication-Servern an, die mit Ihrem Konto verknüpft sind. |

## <a name="view-mfa-reports"></a>Anzeigen von MFA-Berichten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **MFA-Server** aus.
3. Wählen Sie den Bericht aus, den Sie anzeigen möchten.

   <center>![Cloud](./media/howto-mfa-reporting/report.png)</center>

## <a name="azure-ad-sign-ins-report"></a>Azure AD-Anmeldungenbericht

Mit dem **Anmeldungenaktivitätsbericht** im [Azure-Portal](https://portal.azure.com) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Der Anmeldungenbericht kann Informationen zur Nutzung von verwalteten Anwendungen und Benutzeranmeldeaktivitäten, z.B. auch Informationen zur Nutzung von MFA (Multi-Factor Authentication, mehrstufige Authentifizierung), enthalten. Die MFA-Daten liefern Ihnen Erkenntnisse zur Funktionsweise von MFA in Ihrer Organisation. Sie können dann beispielsweise folgende Fragen beantworten:

- Wurde bei der Anmeldung MFA verwendet?
- Wie hat der Benutzer den MFA-Vorgang durchgeführt?
- Warum konnte der Benutzer den MFA-Vorgang nicht durchführen?
- Wie viele Benutzer werden zur Durchführung des MFA-Vorgangs aufgefordert?
- Wie viele Benutzer können den MFA-Vorgang nicht durchführen?
- Welche MFA-Probleme treten für Benutzer häufig auf?

Diese Daten sind über das [Azure-Portal](https://portal.azure.com) und die [Berichterstellungs-API](../reports-monitoring/concept-reporting-api.md) verfügbar.

![Cloud](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Struktur des Anmeldungenberichts

Über die Berichte zu den Anmeldeaktivitäten für MFA erhalten Sie Zugriff auf die folgenden Informationen:

**MFA erforderlich:** Gibt an, ob MFA für die Anmeldung erforderlich ist. MFA kann aufgrund von MFA pro Benutzer, bedingtem Zugriff oder aus anderen Gründen obligatorisch sein. Mögliche Werte sind **Ja** oder **Nein**.

**MFA-Ergebnis:** Weitere Informationen dazu, ob der MFA-Vorgang erfolgreich durchgeführt wurde:

- Wenn der MFA-Vorgang erfolgreich war, enthält diese Spalte weitere Details dazu.
   - Azure Multi-Factor Authentication
      - completed in the cloud (in der Cloud durchgeführt)
      - has expired due to the policies configured on tenant (ist aufgrund der auf dem Mandanten konfigurierten Richtlinien abgelaufen)
      - registration prompted (zur Registrierung aufgefordert)
      - satisfied by claim in the token (per Anspruch im Token erfüllt)
      - satisfied by claim provided by external provider (per Anspruch vom externen Anbieter erfüllt)
      - satisfied by strong authentication (per strenger Authentifizierung erfüllt)
      - skipped as flow exercised was Windows broker logon flow (übersprungen, da es sich um einen Windows-Broker-Anmeldefluss gehandelt hat)
      - skipped due to app password (übersprungen aufgrund von App-Kennwort)
      - skipped due to location (übersprungen aufgrund von Standort)
      - skipped due to registered device (übersprungen aufgrund von registriertem Gerät)
      - skipped due to remembered device (übersprungen aufgrund von gespeichertem Gerät)
      - successfully completed (erfolgreich abgeschlossen)
   - Redirected to external provider for multi-factor authentication (umgeleitet an externen Anbieter zur mehrstufigen Authentifizierung)

- Wenn der MFA-Vorgang nicht erfolgreich war, ist in dieser Spalte der Grund angegeben.
   - Azure Multi-Factor Authentication denied; (Azure Multi-Factor Authentication verweigert;)
      - authentication in-progress (Authentifizierung in Bearbeitung)
      - duplicate authentication attempt (versuchte doppelte Authentifizierung)
      - entered incorrect code too many times (falschen Code zu häufig eingegeben)
      - invalid authentication (ungültige Authentifizierung)
      - invalid mobile app verification code (ungültiger Verifizierungscode der mobilen App)
      - misconfiguration (Fehlkonfiguration)
      - phone call went to voicemail (Voicemail bei Telefonanruf)
      - phone number has an invalid format (Telefonnummer hat ein ungültiges Format)
      - service error (Dienstfehler)
      - unable to reach the user’s phone (Telefon des Benutzers nicht erreichbar)
      - unable to send the mobile app notification to the device (Benachrichtigung über mobile App kann nicht an das Gerät gesendet werden)
      - unable to send the mobile app notification (Benachrichtigung über mobile App kann nicht gesendet werden)
      - user declined the authentication (Benutzer hat die Authentifizierung abgelehnt)
      - user did not respond to mobile app notification (Benutzer hat auf Benachrichtigung über mobile App nicht geantwortet)
      - user does not have any verification methods registered (für Benutzer sind keine Verifizierungsmethoden registriert)
      - user entered incorrect code (Benutzer hat falschen Code eingegeben)
      - user entered incorrect PIN (Benutzer hat falsche PIN eingegeben)
      - user hung up the phone call without succeeding the authentication (Benutzer hat Gespräch beendet, ohne die Authentifizierung durchzuführen)
      - user is blocked (Benutzer ist gesperrt)
      - user never entered the verification code (Benutzer hat den Verifizierungscode niemals eingegeben)
      - user not found (Benutzer wurde nicht gefunden)
      - verification code already used once (Verifizierungscode wurde bereits einmal verwendet)

**MFA-Authentifizierungsmethode:** Die Authentifizierungsmethode, die vom Benutzer zum Durchführen des MFA-Vorgangs verwendet wurde. Mögliche Werte sind:

- Textnachricht
- Benachrichtigung über eine mobile App
- Telefonanruf (Telefonnummer für Authentifizierung)
- Prüfcode in der mobilen App
- Telefonanruf (Geschäftliche Telefonnummer)
- Telefonanruf (alternative Telefonnummer für Authentifizierung)

**MFA-Authentifizierungsdetail:** Bereinigte Version der Telefonnummer, z.B. +X XXXXXXXX64.

**Bedingter Zugriff**: Finden Sie Informationen über Richtlinien für bedingten Zugriff, die den Anmeldeversuch beeinflusst haben, einschließlich:

- Richtlinienname
- Gewährungssteuerelemente
- Sitzungssteuerelemente
- Ergebnis

## <a name="powershell-reporting"></a>PowerShell-Berichterstellung

Identifizieren Sie mithilfe des folgenden PowerShell-Befehls Benutzer, die sich für MFA registriert haben.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifizieren Sie mithilfe des folgenden PowerShell-Befehls Benutzer, die sich nicht für MFA registriert haben.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Nächste Schritte

* [Für Benutzer](../user-help/multi-factor-authentication-end-user.md)
* [Bereitstellungsort](concept-mfa-whichversion.md)

---
title: Datenanforderungen von Azure AD SSPR | Microsoft-Dokumentation
description: "Datenanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung und deren Bereitstellung"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7936f47007285e3f7fa1d3220efa022a6e3881ca
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung

Für das Bereitstellen der Self-Service-Kennwortzurücksetzung (SSPR) müssen Authentifizierungsdaten vorhanden sein. Einige Organisationen verlangen von ihren Benutzern die Eingabe ihrer Authentifizierungsdaten, aber viele Organisationen bevorzugen eine Synchronisierung mit in Active Directory vorhandenen Daten. Wenn die Daten in Ihrem lokalen Verzeichnis ordnungsgemäß formatiert wurden und Sie [Azure AD Connect mit Expresseinstellungen](./connect/active-directory-aadconnect-get-started-express.md) konfigurieren, werden die Daten in Azure AD und SSPR ohne Benutzereingriff zur Verfügung gestellt.

Telefonnummern müssen im Format „+Landesvorwahl Telefonnummer“ eingegeben werden (Beispiel: +1 4255551234), damit sie ordnungsgemäß funktionieren.

> [!NOTE]
> Für die Kennwortzurücksetzung werden Nebenstellen nicht unterstützt. Selbst bei der Angabe im Format +1 4255551234X12345 werden Nebenstellen vor dem Anruf entfernt.

## <a name="fields-populated"></a>Ausgefüllte Felder

Wenn Sie die Standardeinstellungen in Azure AD Connect verwenden, werden die folgenden Zuordnungen vorgenommen.

| Lokales AD | Azure AD | Kontaktinformationen für die Azure AD-Authentifizierung |
| --- | --- | --- |
| telephoneNumber | Bürotelefon | Alternatives Telefon |
| mobile | Mobiltelefon | Phone |


## <a name="security-questions-and-answers"></a>Sicherheitsfragen und -antworten

Die Sicherheitsfragen und-antworten werden sicher in Ihrem Azure AD-Mandanten gespeichert und sind für Benutzer nur über das [SSPR-Registrierungsportal](https://aka.ms/ssprsetup) zugänglich. Administratoren können den Inhalt der Fragen und Antworten anderer Benutzer weder anzeigen noch ändern.

### <a name="what-happens-when-a-user-registers"></a>Was geschieht bei Registrierung eines Benutzers?

Wenn sich ein Benutzer registriert, zeigt die Registrierungsseite die folgenden Felder an:

* Telefonnummer für Authentifizierung
* E-Mail-Adresse für Authentifizierung
* Sicherheitsfragen und -antworten

Wenn Sie einen Wert für **Mobiltelefon** oder **Alternative E-Mail-Adresse** angegeben haben, können Benutzer diese Werte sofort verwenden, um ihre Kennwörter zurücksetzen, selbst wenn sie sich nicht für den Dienst registriert haben. Benutzer sehen diese Werte zudem bei der ersten Registrierung und können sie bei Bedarf ändern. Nachdem sie sich erfolgreich registriert haben, werden diese Werte in den Feldern **Authentifizierungstelefon** und **E-Mail-Adresse für Authentifizierung** beibehalten.

## <a name="set-and-read-authentication-data-using-powershell"></a>Festlegen und Lesen von Authentifizierungsdaten mithilfe von PowerShell

Die folgenden Felder können mithilfe von PowerShell festgelegt werden

* Alternative E-Mail-Adresse
* Mobiltelefon
* Bürotelefon: kann nur festgelegt werden, wenn keine Synchronisierung mit einem lokalen Verzeichnis erfolgt

### <a name="using-powershell-v1"></a>Mithilfe von PowerShell V1

Zunächst müssen Sie [das Azure AD PowerShell-Modul herunterladen und installieren](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Sobald Sie es installiert haben, können Sie anhand der folgenden Schritte die einzelnen Felder konfigurieren.

#### <a name="set-authentication-data-with-powershell-v1"></a>Festlegen von Authentifizierungsdaten mit PowerShell V1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a>Lesen von Authentifizierungsdaten mit PowerShell V1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-the-commands-that-follow"></a>Die Telefonnummer und die E-Mail-Adresse für die Authentifizierung können nur mithilfe von PowerShell V1 mit den folgenden Befehlen gelesen werden

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>Mithilfe von PowerShell V2

Zunächst müssen Sie [das Azure AD PowerShell V2-Modul herunterladen und installieren](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Sobald Sie es installiert haben, können Sie anhand der folgenden Schritte die einzelnen Felder konfigurieren.

Führen Sie die folgenden Befehle aus, um auf der Grundlage aktueller PowerShell-Versionen mit Install-Module-Unterstützung eine schnelle Installation durchzuführen. (Die erste Zeile überprüft, ob das Modul bereits installiert ist.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a>Festlegen von Authentifizierungsdaten mit PowerShell V2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a>Lesen von Authentifizierungsdaten mit PowerShell V2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Nächste Schritte

* [Wie führe ich ein erfolgreiches Rollout der SSPR durch?](active-directory-passwords-best-practices.md)
* [Setzen Sie Ihr Kennwort zurück bzw. ändern Sie es](active-directory-passwords-update-your-own-password.md).
* [Registrieren Sie sich für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md).
* [Haben Sie eine Frage zur Lizenzierung?](active-directory-passwords-licensing.md)
* [Welche Authentifizierungsmethoden sind für Benutzer verfügbar?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Welche Richtlinienoptionen stehen mit SSPR zur Verfügung?](active-directory-passwords-policy.md)
* [Worum handelt es sich beim Rückschreiben von Kennwörtern, und warum sollte ich mir Gedanken darüber machen?](active-directory-passwords-writeback.md)
* [Wie melde ich eine Aktivität bei SSPR?](active-directory-passwords-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

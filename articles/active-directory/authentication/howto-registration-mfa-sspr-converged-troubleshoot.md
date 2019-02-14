---
title: Deaktivieren der zusammengeführten Registrierung für die SSPR und MFA in Azure AD (Public Preview)
description: In diesem Artikel erfahren Sie, wie die Registrierung für die Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung in Azure AD (Public Preview) deaktiviert wird.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89423793dd7a93dfa1f324678fccb0b3ac17a8f3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186050"
---
# <a name="disable-azure-ad-converged-registration-public-preview"></a>Deaktivieren der zusammengeführten Azure AD-Registrierung (Public Preview)

Wenn ein Benutzer seine Telefonnummer und/oder mobile App in dem neuen Feature der zusammengefassten Registrierung registriert, versieht unser Dienst eine Gruppe von Flags (StrongAuthenticationMethods) mit einem Stempel für diese Methoden für diesen Benutzer. Über diese Funktionalität kann der Benutzer, immer die Azure Multi-Factor Authentication (MFA) mit diesen Methoden ausführen, wenn eine MFA erforderlich ist.

Für die Methoden, die Benutzer über die neue Oberfläche registrieren, ist die StrongAuthenticationMethods-Eigenschaft festgelegt. Dieses Verhalten tritt auch auf, sobald eine Public Preview verfügbar ist. Wenn ein Administrator die Vorschauversion aktiviert, registrieren Benutzer diese über die neue Oberfläche und der Administrator deaktiviert anschließend die Vorschauversion, da Benutzer unwissentlich auch für die MFA registriert werden könnten.

Wenn ein Benutzer, der eine zusammengeführte Registrierung durchgeführt hat, zur aktuellen SSPR-Registrierungsseite (Self-Service Password Reset) unter [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) navigiert, wird er dazu aufgefordert, die MFA auszuführen, bevor er auf diese Seite zugreifen kann. Dieser Schritt stellt aus technischer Sicht ein erwartetes Verhalten dar, doch für Benutzer, die vorher lediglich für die SSPR registriert waren, ist dieser Schritt ein neues Verhalten. Obwohl dieser zusätzliche Schritt das Sicherheitsniveau des Benutzers durch die Bereitstellung einer zusätzlichen Sicherheitsstufe verbessert, sollten Administratoren ein Rollback für ihre Benutzer ausführen, damit sie nicht mehr die MFA durchführen können.  

## <a name="how-to-roll-back-users"></a>Gewusst wie: Ausführen eines Rollbacks für Benutzer

Wenn Sie als Administrator die MFA-Einstellungen eines Benutzers zurücksetzen möchten, steht ihnen ein von uns erstelltes PowerShell-Skript zur Verfügung, das die StrongAuthenticationMethods-Eigenschaft für eine mobile App und/oder eine Telefonnummer des Benutzers löscht. Die Ausführung dieses Skripts für Ihre Benutzer bedeutet, dass sie sich bei Bedarf erneut für die MFA registrieren müssen. Es wird empfohlen, das Rollback vor der Ausführung eines Rollbacks für alle betroffenen Benutzer bei einem oder zwei Benutzern zu testen.

Die folgenden Schritte zeigen, wie Sie ein Rollback für einen Benutzer oder eine Gruppe von Benutzern ausführen:

### <a name="pre-requisites"></a>Voraussetzungen

1. Sie müssen die entsprechenden Azure AD PowerShell-Module installieren. Führen Sie in einem PowerShell-Fenster die folgenden Befehle zum Installieren der Module aus:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Speichern Sie die Liste der betroffenen Benutzerobjekt-ID(s) auf Ihrem Computer als Textdatei, in der jede ID in einer separaten Zeile aufgeführt wird. Notieren Sie sich den Speicherort der Datei.
1. Speichern Sie das folgende Skript auf Ihrem Computer, und notieren Sie sich den Speicherort des Skripts:

```powershell
<# 
//********************************************************
//*                                                      *
//*   Copyright (C) Microsoft. All rights reserved.      *
//*                                                      *
//********************************************************
#>

param($path)

# Define Remediation Fn
function RemediateUser {

    param  
    (
        $ObjectId
    )

    $user = Get-MsolUser -ObjectId $ObjectId

    Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

    $hasMfaRelyingParty = $false
    foreach($p in $user.StrongAuthenticationRequirements)
    {
        if ($p.RelyingParty -eq "*")
        {
            $hasMfaRelyingParty = $true
            Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
        }
    }

    if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
    {
        Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
        Write-Host "Rolling back user ..." -ForegroundColor Yellow
        Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
        Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
    }
    else
    {
        Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
    }

    Write-Host ""
    Start-Sleep -Milliseconds 750
}

# Connect
Import-Module MSOnline
Connect-MsolService

foreach($line in Get-Content $path)
{
    RemediateUser -ObjectId $line
}
```

### <a name="rollback"></a>Rollback

Führen Sie in einem PowerShell-Fenster den folgenden Befehl aus, nachdem Sie die hervorgehobenen Speicherorte aktualisiert haben. Geben Sie bei entsprechender Aufforderung die Anmeldeinformationen des globalen Administrators ein. Das Skript gibt das Ergebnis der einzelnen Benutzerupdatevorgänge aus.

`<script location> -path <user file location>`

## <a name="disable-preview-experience"></a>Deaktivieren der Oberfläche der Vorschauversion

Um die Oberfläche der Vorschauversion für Ihre Benutzer zu deaktivieren, führen Sie die folgenden Schritte durch:

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an.
2. Navigieren Sie zu **Azure Active Directory**, **Benutzereinstellungen**, **Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.
3. Legen Sie für **Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden** die Option **Keine** fest, und klicken Sie auf **Speichern**.

Benutzer werden nicht mehr aufgefordert, sich bei der Oberfläche der Vorschauversion zu registrieren.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über die Public Preview der zusammengeführten Registrierung für die Self-Service-Kennwortzurücksetzung und Azure Multi-Factor Authentication.](concept-registration-mfa-sspr-converged.md)

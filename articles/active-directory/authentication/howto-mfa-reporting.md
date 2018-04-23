---
title: Zugriffs- und Nutzungsberichte für Azure MFA| Microsoft Docs
description: Beschreibt, wie Sie das Berichte-Feature für Multi-Factor Authentication verwenden.
services: multi-factor-authentication
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 3a29d1b17924b519167f362e6116f405e182bf75
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Berichte in Azure Multi-Factor Authentication

Azure Multi-Factor Authentication bietet verschiedene Berichte, die Sie und Ihre Organisation im Azure-Portal verwenden können. In der folgenden Tabelle sind die verfügbaren Berichte aufgeführt:

| Bericht | Speicherort | BESCHREIBUNG |
|:--- |:--- |:--- |
| Verlauf – gesperrte Benutzer | Azure AD > MFA-Server > Benutzer blockieren/entsperren | Zeigt die Liste der Anforderungen zum Blockieren und Entsperren von Benutzern an. |
| Nutzung und Betrugswarnungen | Azure AD > Anmeldungen | Bietet Informationen zur Gesamtnutzung, Übersichts- und Detailinformationen zu Benutzern sowie einen Verlauf von Betrugswarnungen, die im angegebenen Zeitraum gesendet wurden. |
| Nutzung für lokale Komponenten | Azure AD > MFA-Server > Aktivitätsbericht | Bietet Informationen zur Gesamtnutzung für MFA durch die NPS-Erweiterung, AD FS und den MFA-Server. |
| Verlauf – Umgangene Benutzer | Azure AD > MFA-Server > Einmalumgehung | Zeigt den Verlauf von Anforderungen zur Umgehung der Multi-Factor Authentication für einen Benutzer an. |
| Serverstatus | Azure AD > MFA-Server > Serverstatus | Zeigt den Status von Multi-Factor Authentication-Servern an, die mit Ihrem Konto verknüpft sind. |

## <a name="view-reports"></a>Anzeigen von Berichten 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **MFA-Server** aus.
3. Wählen Sie den Bericht aus, den Sie anzeigen möchten.

   <center>![Cloud](./media/howto-mfa-reporting/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell-Berichterstellung

Identifizieren Sie mithilfe des folgenden PowerShell-Befehls Benutzer, die sich für MFA registriert haben.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifizieren Sie mithilfe des folgenden PowerShell-Befehls Benutzer, die sich nicht für MFA registriert haben.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Nächste Schritte

* [Für Benutzer](../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md)
* [Bereitstellungsort](concept-mfa-whichversion.md)

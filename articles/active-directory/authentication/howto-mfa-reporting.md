---
title: Zugriffs- und Nutzungsberichte für Azure MFA| Microsoft Docs
description: Beschreibt, wie Sie das Berichte-Feature für Multi-Factor Authentication verwenden.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: ead1c5a899057bb26154b45c75251e7d9e481147
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160891"
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

* [Für Benutzer](../user-help/multi-factor-authentication-end-user.md)
* [Bereitstellungsort](concept-mfa-whichversion.md)

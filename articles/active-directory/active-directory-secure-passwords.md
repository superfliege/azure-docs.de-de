---
title: Azure AD-Kennwortsicherheit mit Stufen | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure AD sichere Kennwörter erzwingt und Benutzerkennwörter vor Cyberkriminellen schützt.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/28/2017
ms.author: lizross
ms.openlocfilehash: e3e97a5a9b768b3b9d4a36627d28955be1f3d9df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056918"
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a>Ein mehrstufiger Ansatz für die Azure AD-Kennwortsicherheit

In diesem Artikel werden einige Best Practices beschrieben, die Sie als Benutzer oder Administrator verwenden können, um Ihre Azure Active Directory- (Azure AD) und Microsoft-Konten zu schützen.

 > [!NOTE]
 > **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](user-help/active-directory-passwords-update-your-own-password.md)weiter.
 >
 > Azure AD-Administratoren können Benutzerkennwörter mithilfe der Anleitungen im Artikel [Zurücksetzen des Kennworts für einen Benutzer in Azure Active Directory](fundamentals/active-directory-users-reset-password-azure-portal.md) zurücksetzen.
 >

## <a name="password-requirements"></a>Kennwortanforderungen

Azure AD verfügt über die folgenden gängigen Ansätze zum Schützen von Kennwörtern:

* Anforderungen an die Kennwortlänge
* Anforderungen an die Komplexität des Kennworts
* Regelmäßiger bzw. periodischer Kennwortablauf

Informationen zur Kennwortzurücksetzung in Azure Active Directory finden Sie im Thema [Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten](user-help/active-directory-passwords-update-your-own-password.md).

## <a name="azure-ad-password-protections"></a>Kennwortschutz in Azure AD

Azure AD und das Microsoft-Kontosystem nutzen in der Branche bewährte Ansätze, um den Schutz von Benutzer- und Administratorkennwörtern sicherzustellen. Dazu gehören u.a.:

* Dynamisch gesperrte Kennwörter
* Intelligente Kennwortsperrung

Informationen zur Kennwortverwaltung nach dem neuesten Forschungsstand finden Sie im Whitepaper [Password Guidance](https://aka.ms/passwordguidance) (Anleitung für Kennwörter).

### <a name="dynamically-banned-passwords"></a>Dynamisch gesperrte Kennwörter

Bei Azure AD und Microsoft-Konten werden Kennwörter geschützt, indem häufig verwendete Kennwörter dynamisch gesperrt werden. Das Azure AD Identity Protection-Team analysiert die Liste mit den gesperrten Kennwörtern routinemäßig, um zu verhindern, dass Benutzer häufig verwendete Kennwörter wählen. Dieser Dienst ist für Kunden von Azure AD und des Microsoft-Kontodiensts verfügbar.

Beim Erstellen von Kennwörtern ist es wichtig, dass Administratoren die Benutzer zur Wahl von Kennwörtern ermutigen, die eine eindeutige Kombination aus Buchstaben, Zahlen, Zeichen oder Wörtern enthalten. Dieser Ansatz macht die Gefährdung von Benutzerkennwörtern nahezu unmöglich, sorgt aber auch dafür, dass sie einfacher zu merken sind.

#### <a name="password-breaches"></a>Kennwortsicherheitsverletzungen

Bei Microsoft arbeiten wir ununterbrochen daran, den Cyberkriminellen immer einen Schritt voraus zu sein.

Das Azure AD Identity Protection-Team analysiert ständig, welche Kennwörter häufig verwendet werden. Auch Cyberkriminelle nutzen ähnliche Strategien, um die Chancen bei ihren Angriffen zu erhöhen, z.B. durch die Erstellung einer [Rainbow Table](https://en.wikipedia.org/wiki/Rainbow_table) (Regenbogentabelle) zum Cracken von Kennworthashes.

Microsoft führt ständig Analysen in Bezug auf [Datenverletzungen](https://www.privacyrights.org/data-breaches) durch, um eine Liste mit gesperrten Kennwörtern zu führen, die dynamisch aktualisiert wird. So wird sichergestellt, dass anfällige Kennwörter gesperrt werden, bevor sie für Azure AD-Kunden zu einer echten Bedrohung werden. Weitere Informationen zu unserer derzeitigen Arbeit in Bezug auf Sicherheitsmaßnahmen finden Sie im [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).

### <a name="smart-password-lockout"></a>Intelligente Kennwortsperrung

Wenn Azure AD einen potenziellen Cyberkriminellen erkennt, der versucht, ein Benutzerkennwort zu hacken, wird das Benutzerkonto per Smart Password Lockout gesperrt. Azure AD ist dafür ausgelegt, das Risiko zu ermitteln, das mit bestimmten Anmeldesitzungen verbunden ist. Dann wenden wir unter Nutzung der aktuellsten Sicherheitsdaten Sperrsemantiken auf Cyberbedrohungen an.

Beim Ausschluss eines Benutzers aus Azure AD wird auf dem Bildschirm etwa Folgendes angezeigt:

  ![Ausschluss aus Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

Für andere Microsoft-Konten sieht der Bildschirm in etwa wie folgt aus:

  ![Ausschluss aus einem Microsoft-Konto](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Informationen zur Kennwortzurücksetzung in Azure Active Directory finden Sie im Thema [Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten](user-help/active-directory-passwords-update-your-own-password.md).

  >[!NOTE]
  >Wenn Sie Azure AD-Administrator sind, ist die Verwendung von [Windows Hello](https://www.microsoft.com/windows/windows-hello) ratsam, um ganz zu verhindern, dass Ihre Benutzer herkömmliche Kennwörter erstellen müssen.
  >

## <a name="next-steps"></a>Nächste Schritte

* [Aktualisieren Ihres eigenen Kennworts](user-help/active-directory-passwords-update-your-own-password.md)
* [Grundlagen der Identitätsverwaltung in Azure](fundamentals-identity.md)
* [Bericht zur Aktivität „Zurücksetzen des Kennworts“](authentication/howto-sspr-reporting.md)

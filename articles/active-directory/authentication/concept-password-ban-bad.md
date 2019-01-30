---
title: Dynamisch gesperrte Kennwörter in Azure AD
description: Sperren Sie mit dynamisch gesperrten Kennwörtern in Azure AD unsichere Kennwörter aus Ihrer Umgebung aus.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.openlocfilehash: 7cb1acace3dd8605d7506013a6f1c0273dafa32f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54421435"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Beseitigen falscher Kennwörter in Ihrer Organisation

|     |
| --- |
| Azure AD-Kennwortschutz und die benutzerdefinierte Liste der gesperrten Kennwörter sind in der öffentlichen Vorschau befindliche Features von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Branchenführer schärfen Ihnen ein, dasselbe Kennwort nicht mehrmals zu verwenden, ein Kennwort komplex zu gestalten und nicht einfach Kennwörter wie „Kennwort123“ zu verwenden. Wie können Organisationen gewährleisten, dass ihre Benutzer Anleitungen folgen? Wie können sie sicherstellen, dass Benutzer keine gebräuchlichen Kennwörter verwenden, oder Kennwörter, die bekanntermaßen in neueste Datenschutzverstöße verwickelt sind?

## <a name="global-banned-password-list"></a>Liste global gesperrter Kennwörter

Bei Microsoft arbeiten wir ununterbrochen daran, den Cyberkriminellen immer einen Schritt voraus zu sein. Aus diesem Grund sucht das Azure AD Identity Protection-Team ständig nach häufig verwendeten und gefährdeten Kennwörtern. Dann werden Kennwörter, die als zu häufig verwendet eingestuft werden, durch Eintrag in die Liste global gesperrter Kennwörter blockiert. Weil Cyberkriminelle in ihren Angriffen ähnliche Strategien verwenden, veröffentlicht Microsoft die Inhalte dieser Liste nicht. Diese anfälligen Kennwörter werden blockiert, bevor sie für die Kunden von Microsoft ein echtes Risiko darstellen. Weitere Informationen zur derzeitigen Arbeit in Bezug auf Sicherheitsmaßnahmen finden Sie im [Microsoft Security Intelligence Report](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Vorschau: Benutzerdefinierte Liste gesperrter Kennwörter

Manche Organisationen möchten möglicherweise in puncto Sicherheit noch einen Schritt weiter gehen, indem sie die Liste global gesperrter Kennwörter mit ihren eigenen Anpassungen in der von Microsoft als benutzerdefinierte Liste gesperrter Kennwörter bezeichneten Liste ergänzen. Enterprisekunden wie Contoso könnten dann wählen, Varianten ihres Markennamens, unternehmensspezifische Begriffe oder andere Elemente zu blockieren.

Die benutzerdefinierte Liste gesperrter Kennwörter sowie die Möglichkeit, eine lokale Active Directory-Integration zu aktivieren, wird mit dem Azure-Portal verwaltet.

![Ändern der benutzerdefinierten Liste gesperrter Kennwörter unter Authentifizierungsmethoden im Azure-Portal](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Lokale Hybridszenarien

Schützen von ausschließlichen Cloudkonten ist hilfreich, aber viele Organisationen behalten Hybridszenarien einschließlich des lokalen Windows Server Active Directory bei. Es ist möglich, Azure AD-Kennwortschutz für Windows Server Active Directory-Agents (Vorschau) lokal zu installieren, um die Liste gesperrter Kennwörter auf Ihre bestehende Infrastruktur auszudehnen. Zurzeit müssen Benutzer und Administratoren, die Kennwörter lokal ändern, festlegen oder zurücksetzen, die gleiche Kennwortrichtlinie einhalten wie ausschließliche Cloudbenutzer.

## <a name="how-does-the-banned-password-list-work"></a>So funktioniert die Liste gesperrter Kennwörter

Um Übereinstimmungen von Kennwörtern mit Einträgen in der Liste gesperrter Kennwörter zu erkennen, wird die Zeichenfolge in Kleinbuchstaben umgewandelt und mit den bekannten gesperrten Kennwörtern innerhalb einer Bearbeitungsdistanz von 1 mit Fuzzyübereinstimmung verglichen.

Beispiel: Das Wort „password“ ist für eine Organisation blockiert.
   - Ein Benutzer versucht, sein Kennwort auf „P@ssword“ festzulegen, das in „password“ konvertiert und blockiert wird, da es sich um eine Variante von „password“ handelt.
   - Ein Administrator versucht, das Kennwort eines Benutzers auf „Password123!“ festzulegen,  das in „password123!“ konvertiert  und blockiert wird, da es sich um eine Variante von „password“ handelt.

Jedes Mal, wenn ein Benutzer sein Azure AD-Kennwort zurücksetzt oder ändert, durchläuft es diesen Prozess, um zu bestätigen, dass es nicht in der Liste gesperrter Kennwörter steht. Diese Überprüfung ist in Hybridszenarien mit Self-Service-Kennwortzurücksetzung, Kennworthash-Synchronisierung und Pass-Through-Authentifizierung enthalten.

## <a name="license-requirements"></a>Lizenzanforderungen

|   | Kennwortschutz für Azure AD mit globaler Liste gesperrter Kennwörter | Kennwortschutz für Azure AD mit benutzerdefinierter Liste gesperrter Kennwörter|
| --- | --- | --- |
| Reine Cloudbenutzer | Azure AD Free | Azure AD Basic |
| Über die lokale Windows Server Active Directory-Instanz synchronisierte Benutzer | Azure AD Premium P1 oder P2 | Azure AD Premium P1 oder P2 |

Weitere Informationen zur Lizenzierung einschließlich der Kosten finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Anzeige für Benutzer

Wenn ein Benutzer versucht, ein Kennwort auf einen Wert zurückzusetzen, der gesperrt würde, wird die folgende Fehlermeldung angezeigt:

Ihr Kennwort ist aufgrund eines enthaltenen Worts, Ausdrucks oder Musters leider leicht zu erraten. Wiederholen Sie den Vorgang mit einem anderen Kennwort.

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren der Liste benutzerdefinierter gesperrter Kennwörter](howto-password-ban-bad.md)
* [Vorschau: Bereitstellen von Azure AD-Kennwortschutz](howto-password-ban-bad-on-premises-deploy.md)

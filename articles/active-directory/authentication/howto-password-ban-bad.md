---
title: Sperren von Kennwörtern in Azure AD
description: Verbannen Sie unsichere Kennwörter aus Ihrer Umgebung – mithilfe von dynamisch gesperrten Kennwörtern in Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 30d8260d78b3b46a9f4caea63f6bed818935a9a1
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158742"
---
# <a name="configuring-the-custom-banned-password-list"></a>Konfigurieren der Liste benutzerdefinierter gesperrter Kennwörter

|     |
| --- |
| Azure AD-Kennwortschutz und die benutzerdefinierte Liste der gesperrten Kennwörter sind in der öffentlichen Vorschau befindliche Features von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Viele Organisationen stellen fest, dass ihre Benutzer für ihre Kennwörter allgemeine Wörter verwenden, wie z.B. ihre Schule, ein Sportteam oder eine berühmte Person, sodass die Kennwörter leicht zu erraten sind. Mit der benutzerdefinierten Liste gesperrter Kennwörter von Microsoft können Organisationen Zeichenfolgen festlegen, die zusätzlich zur globalen Liste gesperrter Kennwörter ausgewertet und blockiert werden, wenn Benutzer oder Administratoren Kennwörter ändern oder zurücksetzen.

## <a name="add-to-the-custom-list"></a>Hinzufügen zur benutzerdefinierten Liste

Zum Konfigurieren der benutzerdefinierten Liste gesperrter Kennwörter ist eine Azure Active Directory Premium P1- oder P2-Lizenz erforderlich. Weitere Informationen zur Azure Active Directory-Lizenzierung finden Sie in auf der Seite [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory**, **Authentifizierungsmethoden** und **Kennwortschutz (Vorschau)**.
1. Legen Sie die Option **Benutzerdefinierte Liste erzwingen** auf **Ja** fest.
1. Fügen Sie der **benutzerdefinierten Liste gesperrter Kennwörter** Zeichenfolgen hinzu. Geben Sie eine Zeichenfolge pro Zeile ein.
   * Die benutzerdefinierte Liste gesperrter Kennwörter kann bis zu 1000 Wörter umfassen.
   * Bei der benutzerdefinierten Liste gesperrter Kennwörter wird die Groß- und Kleinschreibung nicht beachtet.
   * Die benutzerdefinierte Liste gesperrter Kennwörter berücksichtigt die Ersetzung häufiger Zeichen.
      * Beispiel: „o“ und „0“ oder „a“ und „@“
   * Die Zeichenfolgen müssen mindestens vier und höchstens 16 Zeichen lang sein.
1. Wenn Sie alle Zeichenfolgen hinzugefügt haben, klicken Sie auf **Speichern**.

> [!NOTE]
> Es kann mehrere Stunden dauern, bis Updates der benutzerdefinierten Liste gesperrter Kennwörter angewendet werden.

![Ändern der benutzerdefinierten Liste gesperrter Kennwörter unter „Authentifizierungsmethoden“ im Azure-Portal](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>So funktioniert's

Jedes Mal, wenn ein Benutzer oder Administrator ein Azure AD-Kennwort zurücksetzt oder ändert, wird es mit den benutzerdefinierten Listen gesperrter Kennwörter abgeglichen, um zu bestätigen, dass es sich auf keiner Liste befindet. Diese Überprüfung wird bei allen mithilfe von Azure AD festgelegten oder geänderten Kennwörter durchgeführt.

## <a name="what-do-users-see"></a>Anzeige für Benutzer

Wenn ein Benutzer versucht, ein Kennwort auf einen Wert zurückzusetzen, der gesperrt würde, wird die folgende Fehlermeldung angezeigt:

Ihr Kennwort ist aufgrund eines enthaltenen Worts, Ausdrucks oder Musters leider leicht zu erraten. Wiederholen Sie den Vorgang mit einem anderen Kennwort.

## <a name="next-steps"></a>Nächste Schritte

[Konzeptionelle Übersicht über Listen gesperrter Kennwörter](concept-password-ban-bad.md)

[Konzeptionelle Übersicht über den Azure AD-Kennwortschutz](concept-password-ban-bad-on-premises.md)

[Aktivieren der lokalen Integration der Listen gesperrter Kennwörter](howto-password-ban-bad-on-premises.md)

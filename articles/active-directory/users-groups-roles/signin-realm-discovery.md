---
title: Nachschlagen des Benutzernamens während der Authentifizierung der Anmeldedaten – Azure Active Directory | Microsoft-Dokumentation
description: Informationen dazu, wie Meldungen auf dem Bildschirm das Nachschlagen des Benutzernamens bei der Anmeldung widerspiegeln
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/03/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91d0dbaf9b648f42ef535d8b491df04b2c7042d7
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007673"
---
# <a name="home-realm-discovery-during-sign-in-for-microsoft-365-services"></a>Startbereichsermittlung während der Anmeldung für Microsoft 365-Dienste

Wir ändern das Anmeldeverhalten in Azure Active Directory (Azure AD), um neue Authentifizierungsmethoden zuzulassen und die Benutzerfreundlichkeit zu verbessern. Azure AD ermittelt während der Anmeldung, wo ein Benutzer überall authentifiziert werden muss. Dabei trifft Azure AD intelligente Entscheidungen durch das Lesen der Organisations- und Benutzereinstellungen zum auf der Anmeldeseite eingegebenen Benutzernamen. Dies ist ein weiterer Schritt zu einer kennwortlosen Zukunft, in der zusätzliche Anmeldeinformationen wie FIDO 2.0 möglich sind.

## <a name="home-realm-discovery-behavior"></a>Verhalten bei der Startbereichsermittlung

In der Vergangenheit wurde die Startbereichsermittlung von der Domäne gesteuert, die bei der Anmeldung angegeben wird, oder bei einigen älteren Anwendungen von einer Richtlinie für die Startbereichsermittlung. Beim bisherigen Ermittlungsverhalten konnte ein Azure Active Directory-Benutzer seinen Benutzernamen falsch eingeben und wurde trotzdem zur Seite mit den Anmeldeinformationen seiner Organisation weitergeleitet. Dies erfolgt immer dann, wenn der Benutzer den Domänennamen der Organisation „contoso.com“ richtig angibt. Dieses Verhalten bietet jedoch nicht dieselben Möglichkeiten zum Anpassen der Umgebung für einzelne Benutzer.

Um eine größere Anzahl von Anmeldeinformationen zu unterstützen und die Benutzerfreundlichkeit zu erhöhen, wurde das Nachschlageverhalten von Azure Active Directory während des Anmeldeprozesses aktualisiert. Beim neuen Veralten werden intelligente Entscheidungen durch das Lesen von Einstellungen auf Mandanten- und Benutzerebene basierend auf dem Benutzernamen getroffen, der auf der Anmeldeseite eingegeben wird. Um dies zu ermöglichen, überprüft Azure Active Directory, ob der auf der Anmeldeseite eingegebene Benutzername in der angegebenen Domäne vorhanden ist, oder leitet den Benutzer um, damit er seine Anmeldeinformationen angeben kann. 

Ein weiterer Vorteil dieses Verhaltens sind bessere Fehlermeldungen. Es folgen einige Beispiele für die verbesserten Fehlermeldungen bei Anmeldung bei einer Anwendung, die nur Azure Active Directory-Benutzer unterstützt.

1. Der Benutzername wurde falsch eingegeben oder noch nicht mit Azure AD synchronisiert:
  
    ![Der Benutzername wurde falsch eingegeben oder nicht gefunden](./media/signin-realm-discovery/typo-username.png)
  
2. Der Domänenname wurde falsch eingegeben:
  
    ![Der Domänenname wurde falsch eingegeben oder nicht gefunden](./media/signin-realm-discovery/typo-domain.png)
  
3. Der Benutzer versucht, sich mit einer bekannten Consumerdomäne anzumelden:
  
    ![Anmeldung mit einem bekannten Consumerdomäne](./media/signin-realm-discovery/consumer-domain.png)
  
4. Das Kennwort wurde falsch eingegeben, aber der Benutzername ist richtig:  
  
    ![Kennwort bei richtigem Benutzernamen falsch eingegeben](./media/signin-realm-discovery/incorrect-password.png)
  
## <a name="additional-info"></a>Zusätzliche Informationen

Neben der verbesserten Anmeldeerfahrung umfasst die Änderung auch Verfahren zur Verhinderung von Missbrauch durch eine umfangreiche Enumeration von Benutzernamen.

Diese Änderung gilt zunächst für verwaltete Domänen und wird Mai 2019 eingeführt. Mit der Einführung für Verbunddomänen ist nicht vor Ende 2019 zu rechnen. Die genauen Einführungstermine für Verbunddomänen hängen vom Kundenfeedback ab.

> [!IMPORTANT]
> Dieses Feature wird erhebliche Auswirkungen auf Verbunddomänen haben, die sich auf die alte Startbereichsermittlung auf Domänenebene verlassen, um den Verbund zu erzwingen. Einige Organisationen haben ihre Mitarbeiter geschult, sich mit einem Benutzernamen anzumelden, der nicht in Azure Active Directory vorhanden ist, aber den richtigen Domänennamen enthält, da Benutzer derzeit über die Domänennamen zum Domänenendpunkt ihrer Organisation geleitet werden. Das neue Anmeldeverhalten lässt dies nicht zu. Der Benutzer wird aufgefordert, den Benutzernamen zu korrigieren, und darf sich nicht mit einem Benutzernamen anmelden, der nicht in Azure Active Directory vorhanden ist.
>
> Wenn Sie oder Ihre Organisation Verfahren nutzen, die das alte Verhalten erfordern, sollten Organisationsadministratoren unbedingt die Dokumentation zu Anmeldung und Authentifizierung für Ihre Mitarbeiter anpassen und Schulungen für die Anmeldung mit ihrem Azure Active Directory-Benutzernamen durchführen.
  
Wenn Sie Probleme mit dem neuen Verhalten haben, hinterlassen Sie Anmerkungen im Abschnitt **Feedback** dieses Artikels.  

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Brandings Ihrer Anmeldung](../fundamentals/add-custom-domain.md)
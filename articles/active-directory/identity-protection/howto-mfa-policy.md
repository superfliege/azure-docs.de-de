---
title: Konfigurieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure Active Directory Identity Protection | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure AD Identity Protection konfigurieren.
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 47c742e486c01d2861889419a56c252e74f1e316
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54469830"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Anleitung: Konfigurieren der Multi-Factor Authentication-Registrierungsrichtlinie

Azure AD Identity Protection unterstützt Sie beim Verwalten des Rollouts der Registrierung für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA), indem eine Richtlinie konfiguriert wird. In diesem Artikel wird beschrieben, wofür die Richtlinie verwendet werden kann und wie sie konfiguriert wird.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>Was ist die Registrierungsrichtlinie für die mehrstufige Authentifizierung?

Azure Multi-Factor Authentication ist eine Methode zur Identitätsüberprüfung, bei der mehr als nur ein Benutzername und ein Kennwort benötigt wird. Sie bietet eine zweite Sicherheitsebene für Benutzeranmeldungen und -transaktionen.  

Die Erzwingung von Azure Multi-Factor Authentication für Benutzeranmeldungen empfiehlt sich aus folgenden Gründen:

- Sie bietet eine leistungsfähige Authentifizierung mit einer Auswahl von einfachen Überprüfungsoptionen.

- Sie spielt eine wichtige Rolle bei der Vorbereitung Ihres Unternehmens zum Schutz und der Wiederherstellung von kompromittierten Konten.


Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)


## <a name="how-do-i-access-the-mfa-registration-policy"></a>Wie greife ich auf die Registrierungsrichtlinie für die mehrstufige Authentifizierung zu?
   
Die Registrierungsrichtlinie für die mehrstufige Authentifizierung befindet sich auf der [Azure AD Identity Protection-Seite](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) im Abschnitt **Konfigurieren**.
   
![MFA-Richtlinie](./media/howto-mfa-policy/1014.png)




## <a name="policy-settings"></a>Richtlinieneinstellungen

Sie müssen Folgendes festlegen, wenn Sie die Richtlinie zum Anmelderisiko konfigurieren:

- Die Benutzer und Gruppen, für die die Richtlinie gilt:

    ![Benutzer und Gruppen](./media/howto-mfa-policy/11.png)

- Die Art des Zugriffs, die erzwungen werden soll:  

    ![Benutzer und Gruppen](./media/howto-mfa-policy/12.png)

- Zustand Ihrer Richtlinie:

    ![Erzwingen der Richtlinie](./media/howto-mfa-policy/14.png)


Das Dialogfeld „Richtlinienkonfiguration“ enthält eine Option zum Schätzen der Auswirkungen, die sich aus Ihrer Konfiguration ergeben.

![Voraussichtliche Auswirkung](./media/howto-mfa-policy/15.png)




## <a name="user-experience"></a>Benutzererfahrung


Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

* [Registrierung für Multi-Factor Authentication](flows.md#multi-factor-authentication-registration)  
* [Anmeldeverfahren von Azure AD Identity Protection](flows.md)  



## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview.md).

---
title: Anfordern der mehrstufigen Authentifizierung für Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die mehrstufige Authentifizierung für Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) anfordern.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9777a7d2ed07ea2edf4bea661a5065c808de9f2b
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188352"
---
# <a name="require-multi-factor-authentication-for-azure-ad-directory-roles-in-pim"></a>Anfordern der mehrstufigen Authentifizierung für Azure AD-Verzeichnisrollen in PIM
Es wird empfohlen, von allen Administratoren Multi-Factor Authentication (MFA) zu fordern. Auf diese Weise sinkt das Risiko eines Angriffs aufgrund eines offengelegten Kennworts.

Sie können festlegen, dass Benutzer beim Anmelden eine mehrstufige Authentifizierung durchlaufen müssen. Im Blogbeitrag [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) (MFA für Office 365 und MFA für Azure) werden die in Office- und Azure-Abonnements enthaltenen Funktionen mit den Funktionen der Microsoft Azure Multi-Factor Authentication verglichen.

Sie können auch festlegen, dass Benutzer eine mehrstufige Authentifizierung durchlaufen müssen, wenn sie eine Rolle in Azure AD PIM aktivieren. Auf diese Weise wird ein Benutzer, der bei der Anmeldung keine mehrstufige Authentifizierung durchlaufen hat, von PIM aufgefordert, dies zu tun.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Erfordern von MFA in Azure AD Privileged Identity Management
Wenn Sie Identitäten in PIM als Administrator für privilegierte Rollen verwalten, werden möglicherweise Warnungen angezeigt, die MFA für privilegierte Konten empfehlen. Klicken Sie im PIM-Dashboard auf die Sicherheitswarnung. Anschließend wird ein neues Blatt mit einer Liste der Administratorkonten geöffnet, für die MFA erforderlich sein sollte.  Sie können MFA als erforderlich festlegen, indem Sie mehrere Rollen auswählen und dann auf die Schaltfläche **Korrigieren** klicken. Sie können aber auch auf die Auslassungspunkte neben einzelnen Rollen und dann auf die Schaltfläche **Korrigieren** klicken.

> [!IMPORTANT]
> Zurzeit funktioniert Azure MFA nur für Geschäfts- oder Schulkonten, aber nicht für Microsoft-Konten (normalerweise ein persönliches Konto, das zum Anmelden bei Microsoft-Diensten wie Skype, Xbox, Outlook.com usw. verwendet wird). Aus diesem Grund kann kein Benutzer mit einem Microsoft-Konto ein berechtigter Administrator sein, da zum Aktivieren der Rollen MFA verwendet werden muss. Wenn Benutzer weiterhin Workloads mit einem Microsoft-Konto verwalten müssen, stufen Sie sie vorerst zu permanenten Administratoren hoch.
> 
> 

Darüber hinaus können Sie die MFA-Anforderung für eine bestimmte Rolle ändern, indem Sie auf dem PIM-Dashboard im Abschnitt „Rollen“ auf die Rolle klicken. Klicken Sie dann auf dem Blatt der Rolle auf **Einstellungen**, und wählen Sie unter „Multi-Factor Authentication“ die Option **Aktivieren** aus.

## <a name="how-azure-ad-pim-validates-mfa"></a>Überprüfen der MFA durch Azure AD PIM
Wenn ein Benutzer eine Rolle aktiviert, stehen zwei Optionen zum Überprüfen von MFA zur Verfügung.

Die einfachste Methode besteht darin, sich bei Benutzern, die eine privilegierte Rolle aktivieren, auf Azure MFA zu verlassen. Überprüfen Sie dazu ggf. zunächst, ob diese Benutzer lizenziert sind und sich für Azure MFA registriert haben. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](../authentication/howto-mfa-getstarted.md). Es ist zwar nicht erforderlich, wird jedoch empfohlen, Azure AD so zu konfigurieren, dass MFA für diese Benutzer bei ihrer Anmeldung erzwungen wird. Der Grund dafür ist, dass die MFA-Überprüfungen von Azure AD PIM selbst durchgeführt werden.

Wenn sich Benutzer lokal authentifizieren, können Sie alternativ festlegen, dass der Identitätsanbieter für MFA verantwortlich ist. Wenn Sie beispielsweise AD-Verbunddienste so konfiguriert haben, dass vor dem Zugriff auf Azure AD eine smartcardbasierte Authentifizierung erforderlich ist, lesen Sie die Anweisungen zum Konfigurieren von AD FS für das Senden von Ansprüchen an Azure AD unter [Sichern von Cloudressourcen mit Azure Multi-Factor Authentication und AD FS](../authentication/howto-mfa-adfs.md) . Wenn ein Benutzer versucht, eine Rolle zu aktivieren, akzeptiert Azure AD PIM, dass MFA für den Benutzer bereits überprüft wurde, sobald die Anwendung die entsprechenden Ansprüche empfängt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-change-default-settings.md)
- [Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-configure-security-alerts.md)

---
title: Multi-Factor Authentication (MFA) und PIM – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure AD Privileged Identity Management (PIM) die Multi-Factor Authentication (MFA) überprüft.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: cfa092e8aebe92192ecca8aec2721282e603cc5b
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669273"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Multi-Factor Authentication (MFA) und PIM

Es wird empfohlen, von allen Administratoren Multi-Factor Authentication (MFA) zu verlangen. Auf diese Weise sinkt das Risiko eines Angriffs aufgrund eines offengelegten Kennworts.

Sie können festlegen, dass Benutzer beim Anmelden eine mehrstufige Authentifizierung durchlaufen müssen. Sie können auch festlegen, dass Benutzer eine mehrstufige Authentifizierung durchlaufen müssen, wenn sie eine Rolle in Azure AD Privileged Identity Management (PIM) aktivieren. Auf diese Weise wird ein Benutzer, der bei der Anmeldung keine mehrstufige Authentifizierung durchlaufen hat, von PIM aufgefordert, dies zu tun.

> [!IMPORTANT]
> Zurzeit funktioniert Azure MFA nur für Geschäfts- oder Schulkonten, aber nicht für Microsoft-Konten (normalerweise ein persönliches Konto, das zum Anmelden bei Microsoft-Diensten wie Skype, Xbox, Outlook.com usw. verwendet wird). Aus diesem Grund kann kein Benutzer mit einem Microsoft-Konto ein berechtigter Administrator sein, da zum Aktivieren der Rollen MFA verwendet werden muss. Wenn Benutzer weiterhin Workloads mit einem Microsoft-Konto verwalten müssen, stufen Sie sie vorerst zu permanenten Administratoren hoch.

## <a name="how-pim-validates-mfa"></a>Überprüfen der MFA durch PIM

Wenn ein Benutzer eine Rolle aktiviert, stehen zwei Optionen zum Überprüfen von MFA zur Verfügung.

Die einfachste Methode besteht darin, sich bei Benutzern, die eine privilegierte Rolle aktivieren, auf Azure MFA zu verlassen. Überprüfen Sie dazu ggf. zunächst, ob diese Benutzer lizenziert sind und sich für Azure MFA registriert haben. Weitere Informationen zum Bereitstellen von Azure MFA finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](../authentication/howto-mfa-getstarted.md). Es ist zwar nicht erforderlich, wird jedoch empfohlen, Azure AD so zu konfigurieren, dass MFA für diese Benutzer bei ihrer Anmeldung erzwungen wird. Der Grund dafür ist, dass die MFA-Überprüfungen von PIM selbst durchgeführt werden.

Wenn sich Benutzer lokal authentifizieren, können Sie alternativ festlegen, dass der Identitätsanbieter für MFA verantwortlich ist. Wenn Sie beispielsweise AD-Verbunddienste so konfiguriert haben, dass vor dem Zugriff auf Azure AD eine smartcardbasierte Authentifizierung erforderlich ist, lesen Sie die Anweisungen zum Konfigurieren von AD FS für das Senden von Ansprüchen an Azure AD unter [Sichern von Cloudressourcen mit Azure Multi-Factor Authentication und AD FS](../authentication/howto-mfa-adfs.md) . Wenn ein Benutzer versucht, eine Rolle zu aktivieren, akzeptiert PIM, dass MFA für den Benutzer bereits überprüft wurde, sobald die Anwendung die entsprechenden Ansprüche empfängt.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-change-default-settings.md)
- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-configure-role-settings.md)

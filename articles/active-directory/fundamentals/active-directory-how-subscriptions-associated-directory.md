---
title: Hinzufügen eines vorhandenen Abonnements zu Ihrem Azure AD-Verzeichnis | Microsoft-Dokumentation
description: Hinzufügen eines vorhandenen Abonnements zu Ihrem Azure AD-Verzeichnis
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: bcae3f51e2245928c8110d06f3514177d57ac883
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448961"
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory

In diesem Artikel erfahren Sie mehr über die Beziehung zwischen einem Azure-Abonnement und Azure Active Directory (Azure AD) sowie über das Hinzufügen eines Abonnements zu ihrem Azure Active Directory. Ihr Azure-Abonnement hat eine Vertrauensbeziehung mit Azure AD (Vertrauensstellung). Das bedeutet, es vertraut dem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, jedes Abonnement vertraut jedoch nur einem einzelnen Verzeichnis. 

Die Vertrauensstellung zwischen einem Abonnement und einem Verzeichnis unterscheidet sich von der Beziehung zwischen einem Abonnement und anderen Ressourcen in Azure (Websites, Datenbanken usw.). Mit dem Ablauf eines Abonnements endet auch der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind. Ein Azure AD Directory verbleibt jedoch in Azure, und Sie können diesem Verzeichnis ein anderes Abonnement zuordnen und das Verzeichnis über das neue Abonnement verwalten.

Alle Benutzer verfügen über ein einzelnes Basisverzeichnis für die Authentifizierung, können aber auch Gäste in anderen Verzeichnissen sein. In Azure AD sind für Sie nur die Verzeichnisse sichtbar, in denen Ihr Benutzerkonto Mitglied oder Gast ist.

## <a name="before-you-begin"></a>Voraussetzungen

* Sie müssen sich mit einem Konto anmelden, das über RBAC-Besitzerzugriff für das Abonnement verfügt.
* Sie müssen sich mit einem Konto anmelden, das sowohl in dem aktuellen Verzeichnis, dem das Abonnement zugeordnet ist, und in dem Verzeichnis, das Sie hinzufügen möchten, vorhanden ist. Weitere Informationen dazu, wie Sie Zugriff auf ein anderes Verzeichnis erlangen, finden Sie unter [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](../b2b/add-users-administrator.md).
* Diese Funktion ist für CSP-Abonnements (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P) und Microsoft Imagine-Abonnements (MS-AZR-0144P) nicht verfügbar.

## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>So weisen Sie Ihrem Azure AD-Verzeichnis ein vorhandenes Abonnement zu

1. Melden Sie sich an, und wählen Sie auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) im Azure-Portal ein Abonnement aus.
2. Klicken Sie auf **Verzeichnis ändern**.

    ![Screenshot mit der Schaltfläche „Verzeichnis ändern“](./media/active-directory-how-subscriptions-associated-directory/edit-directory-button.PNG)
3. Sehen Sie sich die Warnungen an. [RBAC](../../role-based-access-control/role-assignments-portal.md)-Benutzer (Role-Based Access Control, rollenbasierte Zugriffssteuerung) mit zugewiesenem Zugriff und Abonnementadministratoren haben keinen Zugriff mehr, wenn sich das Abonnementverzeichnis ändert.
4. Wählen Sie ein Verzeichnis aus.

    ![Screenshot mit der Benutzeroberfläche zum Ändern des Verzeichnisses](./media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.PNG)
5. Klicken Sie auf **Ändern**.
6. Erfolg! Rufen Sie mithilfe des Verzeichnisumschalters das neue Verzeichnis auf. Es kann bis zu zehn Minuten dauern, bis alles richtig angezeigt wird.

    ![Screenshot mit der Erfolgsmeldung zur Änderung des Verzeichnisses](./media/active-directory-how-subscriptions-associated-directory/edit-directory-success.PNG)

    ![Screenshot mit dem Umschalter](./media/active-directory-how-subscriptions-associated-directory/directory-switcher.PNG)


Alle Ihre Azure Key Vaults sind ebenfalls von einer Abonnementumstellung betroffen. [Ändern Sie daher die Key Vault-Mandanten-ID](../../key-vault/key-vault-subscription-move-fix.md), bevor Sie den Betrieb fortsetzen.

Die Änderung des Abonnementverzeichnisses erfolgt auf Dienstebene. Sie wirkt sich nicht auf die Abonnementabrechnung aus, und der Kontoadministrator kann über das [Kontocenter](https://account.azure.com/subscriptions) weiterhin den Dienstadministrator ändern. Wenn Sie das ursprüngliche Verzeichnis löschen möchten, müssen Sie die Abonnementabrechnung einem neuen Kontoadministrator übertragen. Weitere Informationen zum Übertragen der Abrechnung finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum kostenlosen Erstellen eines neuen Azure AD-Verzeichnisses finden Sie unter [Einrichten eines Azure Active Directory-Mandanten](../develop/active-directory-howto-tenant.md).
* Weitere Informationen zum Übertragen der Abrechnung für ein Azure-Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../../billing/billing-subscription-transfer.md).
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

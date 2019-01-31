---
title: Hinzufügen eines vorhandenen Azure-Abonnements zu Ihrem Mandanten – Azure Active Directory | Microsoft-Dokumentation
description: Anleitungen zum Hinzufügen eines vorhandenen Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: 86ba8f77121e6d4f6358887e13b8f86a5e020f1b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103699"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten
Azure-Abonnements haben eine Vertrauensstellung mit Azure Active Directory (Azure AD). Das bedeutet, sie vertrauen Azure AD beim Authentifizieren von Benutzern, Diensten und Geräten. Mehrere Abonnements können demselben Azure AD-Verzeichnis vertrauen, aber ein Abonnement vertraut immer nur einem einzigen Verzeichnis.

Wenn Ihr Abonnement abläuft, verlieren Sie auch den Zugriff auf alle Ressourcen, die dem Abonnement zugwiesen sind. Das Azure AD-Verzeichnis bleibt jedoch in Azure, sodass Sie das Verzeichnis mit einem anderen Azure-Abonnement zuweisen und verwalten können.

Alle Ihre Benutzer verfügen über ein einzelnes Basisverzeichnis für die Authentifizierung. Ihre Benutzer können aber auch Gäste in anderen Verzeichnissen sein. Ihnen werden sowohl das Basis- als auch das Gastverzeichnis für jeden Benutzer in Azure AD angezeigt.

>[!Important]
>[RBAC](../../role-based-access-control/role-assignments-portal.md)-Benutzer (rollenbasierte Zugriffssteuerung) mit zugewiesenem Zugriff und Abonnementadministratoren haben keinen Zugriff mehr, wenn sich das Abonnementverzeichnis ändert. Wenn Sie darüber hinaus über Schlüsseltresore verfügen, sind diese auch von der Verschiebung des Abonnements betroffen. Sie können dieses Problem beheben, indem Sie die [Schlüsseltresormandant-ID](../../key-vault/key-vault-subscription-move-fix.md) ändern, bevor Sie die Vorgänge fortsetzen.


## <a name="before-you-begin"></a>Voraussetzungen
Bevor Sie Ihr Abonnement zuweisen oder hinzufügen können, müssen Sie die folgenden Aufgaben ausführen:

- Melden Sie sich mit einem Konto an, das:
    - Über **RBAC-Besitzerzugriff** auf das Abonnement verfügt.

    - Sowohl im aktuellen Verzeichnis, das dem Abonnement zugewiesen ist, als auch im neuen Verzeichnis, dem Sie das Abonnement in Zukunft zuweisen möchten, existiert. Weitere Informationen zum Zugreifen auf ein anderes Verzeichnis finden Sie unter [Wie fügen Azure Active Directory-Administratoren Benutzer für die B2B-Zusammenarbeit hinzu?](../b2b/add-users-administrator.md).

- Achten Sie darauf, dass Sie kein Azure-Clouddienstanbieter-Abonnement (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-0146P, MS-AZR-159P), kein internes Microsoft-Abonnement (MS-AZR-0015P) und kein Microsoft Imagine-Abonnement (MS-AZR-0144P) verwenden.
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>So weisen Sie Ihrem Azure AD-Verzeichnis ein vorhandenes Abonnement zu
1. Melden Sie sich an, und wählen Sie im [Azure-Portal auf der Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ein Abonnement aus.

2. Wählen Sie **Verzeichnis ändern** aus.

    ![Seite „Abonnements“ mit hervorgehobener Option „Verzeichnis ändern“](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Überprüfen Sie alle angezeigten Warnungen, und wählen Sie dann **Ändern** aus.

    ![Ändern der Verzeichnisseite und Anzeigen des zu ändernden Verzeichnisses](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Das Verzeichnis wird für das Abonnement geändert, und eine Erfolgsmeldung wird angezeigt.

    ![Erfolgsmeldung](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    

4. Rufen Sie mithilfe des Verzeichnisumschalters Ihr neues Verzeichnis auf. Es kann bis zu zehn Minuten dauern, bis alles richtig angezeigt wird.

    ![Seite mit dem Verzeichnisumschalter](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Das Ändern des Abonnementverzeichnisses erfolgt auf Dienstebene, sodass sich der Vorgang nicht auf die Abonnementabrechnung auswirkt. Der Kontoadministrator kann den Dienstadministrator weiterhin über das [Kontocenter](https://account.azure.com/subscriptions) ändern. Um das ursprüngliche Verzeichnis zu löschen, müssen Sie die Abonnementabrechnung einem neuen Kontoadministrator übertragen. Weitere Informationen zum Übertragen der Abrechnung finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Nächste Schritte

- Wie Sie einen neuen Mandanten erstellen, erfahren Sie unter [Zugreifen auf Azure Active Directory zum Erstellen eines neuen Mandanten](active-directory-access-create-new-tenant.md).

- Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Weitere Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Verzeichnisrollen zu Benutzern in Azure Active Directory](active-directory-users-assign-role-azure-portal.md).

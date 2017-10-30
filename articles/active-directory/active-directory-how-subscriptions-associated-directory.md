---
title: "Hinzufügen eines vorhandenen Abonnements zu Ihrem Azure AD-Verzeichnis | Microsoft-Dokumentation"
description: "Hinzufügen eines vorhandenen Abonnements zu Ihrem Azure AD-Verzeichnis"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/17/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: abf207a3ceec708a828170936f7dc7948ccf34a9
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="how-to-add-an-azure-subscription-to-azure-active-directory"></a>Hinzufügen eines Azure-Abonnements zu Azure Active Directory
In diesem Artikel erfahren Sie mehr über die Beziehung zwischen einem Azure-Abonnement und Azure Active Directory (Azure AD) sowie über das Hinzufügen eines Abonnements zu ihrem Azure Active Directory. Ihr Azure-Abonnement hat eine Vertrauensbeziehung mit Azure AD (Vertrauensstellung). Das bedeutet, es vertraut dem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, jedes Abonnement vertraut jedoch nur einem einzelnen Verzeichnis. 

Die Vertrauensstellung zwischen einem Abonnement und einem Verzeichnis unterscheidet sich von der Beziehung zwischen einem Abonnement und anderen Ressourcen in Azure (Websites, Datenbanken usw.). Mit dem Ablauf eines Abonnements endet auch der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind. Ein Azure AD Directory verbleibt jedoch in Azure, und Sie können diesem Verzeichnis ein anderes Abonnement zuordnen und das Verzeichnis über das neue Abonnement verwalten.

Alle Benutzer verfügen über ein einzelnes Basisverzeichnis für die Authentifizierung, können aber auch Gäste in anderen Verzeichnissen sein. In Azure AD sind für Sie nur die Verzeichnisse sichtbar, in denen Ihr Benutzerkonto Mitglied oder Gast ist.

## <a name="to-add-an-existing-subscription-to-your-azure-ad-directory"></a>So fügen Sie Ihrem Azure AD-Verzeichnis ein vorhandenes Abonnement hinzu
Sie müssen sich mit einem Konto anmelden, das sowohl in dem aktuellen Verzeichnis, dem das Abonnement zugeordnet ist, und in dem Verzeichnis, das Sie hinzufügen möchten, vorhanden ist. 

1. Melden Sie sich beim [Azure-Kontocenter](https://account.azure.com/Subscriptions) mit einem Konto an, bei dem es sich um den Kontoadministrator für das Abonnement handelt, dessen Abonnementbesitz Sie übertragen möchten.
2. Achten Sie darauf, dass sich der Benutzer, der als der neue Abonnementbesitzer fungieren soll, im entsprechenden Verzeichnis befindet.
3. Klicken Sie auf **Abonnement übertragen**.
4. Geben Sie den Empfänger an. Der Empfänger erhält automatisch eine E-Mail mit einem Link für den Vorgang zum Akzeptieren.
5. Der Empfänger klickt auf den Link und folgt den Anweisungen. Außerdem gibt er seine Zahlungsinformationen ein. Wenn der Empfänger erfolgreich ist, wird das Abonnement übertragen. 
6. Das Standardverzeichnis des Abonnements wird in das Verzeichnis geändert, in dem sich der Benutzer befindet.

Weitere Informationen finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../billing/billing-subscription-transfer.md).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../billing/billing-subscription-transfer.md).
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](active-directory-understanding-resource-access.md)
* Informationen zum Zuweisen von Rollen in Azure AD finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

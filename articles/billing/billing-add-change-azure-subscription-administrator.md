---
title: Hinzufügen oder Ändern von Azure-Abonnementadministratorrollen | Microsoft Docs
description: Informationen zum Hinzufügen oder Ändern des Co-Administrators, Dienstadministrators und Kontoadministrators in Azure
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: genli
ms.openlocfilehash: b0e24e498acd823242b3613abb62df978466d56d
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918311"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Hinzufügen oder Ändern von Azure-Abonnementadministratoren

Für die Verwaltung von Azure-Ressourcen müssen Sie über die entsprechende Administratorrolle verfügen. In diesem Artikel wird beschrieben, wie die Administratorrolle für einen Benutzer auf Abonnementebene hinzugefügt oder geändert werden kann.

> [!div class="nextstepaction"]
> [Beitragen zur Verbesserung von Azure-Abrechnungsdokumenten](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="what-administrator-role-do-i-use"></a>Welche Administratorrolle sollte ich verwenden?

Azure verfügt über mehrere verschiedene Rollen. Für die Verwaltung des Zugriffs auf Ressourcen können Sie die klassischen Rollen für Abonnementadministratoren, wie z.B. „Dienstadministrator“ und „Co-Administrator“, oder ein neueres Autorisierungssystem, die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwenden. Um für eine bessere Kontrolle und eine Vereinfachung der Zugriffsverwaltung zu sorgen, empfehlen wir, RBAC für alle Anforderungen der Zugriffsverwaltung zu verwenden. Es ist ratsam, nach Möglichkeit vorhandene Zugriffsrichtlinien mit RBAC neu zu konfigurieren. Weitere Informationen finden Sie unter [Informationen zur rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/overview.md) und unter [Grundlegendes zu den verschiedenen Rollen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Hinzufügen eines Administrators mit der RBAC-Rolle „Besitzer“ für ein Abonnement im Azure-Portal 

Wenn Sie einen Benutzer als Administrator für ein Azure-Abonnement hinzufügen möchten, weisen Sie diesem im Abonnementbereich die Rolle [Besitzer](../role-based-access-control/built-in-roles.md#owner) (eine RBAC-Rolle) zu. Benutzer mit der Rolle „Besitzer“ können die Ressourcen in dem von Ihnen zugewiesenen Abonnement verwalten, verfügen aber über keine Zugriffsrechte für andere Abonnements.

1. Navigieren Sie im [ Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) zu **Abonnements**.
2. Wählen Sie das Abonnement aus, für das Sie Zugriff gewähren möchten.
3. Wählen Sie **Hinzufügen**.
   (Wenn die Schaltfläche „Hinzufügen“ nicht vorhanden ist, sind Sie nicht berechtigt, Berechtigungen hinzuzufügen.)
4. Wählen Sie in der Liste die Option **Zugriffssteuerung (IAM)** aus.
5. Wählen Sie im Feld **Rolle** die Option **Besitzer**. 
6. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung**. 
7. Geben Sie im Feld **Auswählen** die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten. Wählen Sie den Benutzer und dann **Speichern** aus.

    ![Screenshot mit ausgewählter Besitzerrolle](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Hierdurch erhält der Benutzer vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. Rufen Sie das Blatt **Zugriffssteuerung (IAM)** für diesen Bereich auf, um Zugriff für einen anderen Bereich zu gewähren, z.B. eine Ressourcengruppe.

## <a name="add-or-change-co-administrator"></a>Hinzufügen oder Ändern eines Co-Administrators

Nur ein [Besitzer](../role-based-access-control/built-in-roles.md#owner) kann als Co-Administrator hinzugefügt werden. Andere Benutzer mit Rollen wie [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) und [Leser](../role-based-access-control/built-in-roles.md#reader) können nicht als Co-Administratoren hinzugefügt werden.

> [!TIP]
> Sie müssen lediglich das Konto „Besitzer“ als Co-Administrator hinzufügen, wenn der Benutzer klassische Azure-Bereitstellungen verwalten soll. Wir empfehlen Ihnen, für alle anderen Zwecke RBAC zu nutzen.

1. Wenn noch nicht erfolgt, fügen Sie einen Benutzer entsprechend den Anweisungen oben als Besitzer hinzu.
2. **Klicken Sie mit der rechten Maustaste** auf den soeben hinzugefügten Benutzer mit der Rolle „Besitzer“, und wählen Sie dann **Als Co-Administrator hinzufügen** aus. Wenn die Option **Als Co-Administrator hinzufügen** nicht angezeigt wird, können Sie die Seite aktualisieren oder es mit einem anderen Internetbrowser versuchen. 

    ![Screenshot, auf dem der Co-Administrator hinzugefügt wird](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Klicken Sie zum Entfernen der Co-Administratorberechtigung **mit der rechten Maustaste** auf den Benutzer mit der Rolle „Co-Administrator“, und wählen Sie anschließend **Co-Administrator entfernen** aus.

    ![Screenshot, auf dem der Co-Administrator entfernt wird](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Ändern des Dienstadministrators für ein Azure-Abonnement

Nur der Kontoadministrator kann den Dienstadministrator für ein Abonnement ändern. Standardmäßig ist der Dienstadministrator bei der Registrierung mit dem Kontoadministrator identisch. Wenn der Dienstadministrator in einen anderen Benutzer geändert wird, verliert der Kontoadministrator den Zugriff auf das Azure-Portal. Der Kontoadministrator kann aber immer das Kontocenter verwenden, um den Dienstadministrator wieder auf sich selbst festzulegen.

1. Stellen Sie sicher, dass Ihr Szenario unterstützt wird, indem Sie die [Einschränkungen beim Ändern von Dienstadministratoren](#limits) überprüfen.
1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/subscriptions) als Kontoadministrator an.
1. Wählen Sie ein Abonnement aus.
1. Wählen Sie auf der rechten Seite die Option **Abonnementdetails bearbeiten** aus.

    ![Screenshot mit der Schaltfläche „Abonnementdetails bearbeiten“ im Kontocenter](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. Geben Sie im Feld **DIENSTADMINISTRATOR** die E-Mail-Adresse des neuen Dienstadministrators ein.

    ![Screenshot mit dem Feld zum Ändern der E-Mail-Adresse des Dienstadministrators](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Einschränkungen beim Ändern von Dienstadministratoren

* Jedes Abonnement ist einem Azure AD-Verzeichnis zugeordnet. Sie finden das Verzeichnis, dem das Abonnement zugeordnet ist, indem Sie zu [**Abonnements**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) navigieren und ein Abonnement auswählen, um das Verzeichnis anzuzeigen.
* Wenn Sie mit einem Geschäfts-, Schul- oder Unikonto angemeldet sind, können Sie andere Konten in Ihrer Organisation als Dienstadministrator hinzufügen. Beispielsweise kann abby@contoso.com das Konto bob@contoso.com als Dienstadministrator hinzufügen, aber nicht john@notcontoso.com, es sei denn, john@notcontoso.com ist im Verzeichnis „contoso.com“ enthalten. Mit einem Geschäfts-, Schul- oder Unikonto angemeldete Benutzer können Benutzer mit Microsoft-Konten weiterhin als Dienstadministrator hinzufügen.

  | Anmeldemethode | Hinzufügen Microsoft-Kontobenutzers als Dienstadministrator? | Hinzufügen eines Geschäfts-, Schul- oder Unikontos in der gleichen Organisation als Dienstadministrator? | Hinzufügen eines Geschäfts-, Schul- oder Unikontos in einer anderen Organisation als Dienstadministrator? |
  | --- | --- | --- | --- |
  |  Microsoft-Konto |JA |Nein  |Nein  |
  |  Geschäfts-, Schul- oder Unikonto |JA |JA |Nein  |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Ändern des Kontoadministrators für ein Azure-Abonnement

Der Kontoadministrator ist der Benutzer, der sich ursprünglich für das Azure-Abonnement registriert hat, und er ist der verantwortliche Besitzer des Abonnements (auch in Bezug auf die Abrechnung). Informationen zum Ändern des Kontoadministrators für ein Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Sie sind nicht sicher, wer der Kontoadministrator ist?** Folgen Sie diesen Schritten:

1. Navigieren Sie im [ Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) zu **Abonnements**.
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Weitere Informationen zu Ressourcenzugriffssteuerung und Active Directory

* Weitere Informationen zur rollenbasierte Zugriffssteuerung finden Sie unter [Informationen zur rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/overview.md)
* Weitere Informationen zu sämtlichen Rollen in Azure finden Sie unter [Grundlegendes zu den verschiedenen Rollen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Weitere Informationen zu Azure Active Directory finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) und [Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

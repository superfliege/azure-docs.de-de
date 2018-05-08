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
ms.date: 01/04/2018
ms.author: genli
ms.openlocfilehash: 117bf601383a2a1156f8f8dce721f45bd27f2f88
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="add-or-change-azure-subscription-administrators"></a>Hinzufügen oder Ändern von Azure-Abonnementadministratoren

Klassische Azure-Abonnementadministratoren und die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/overview.md) von Azure sind zwei Systeme zum Verwalten des Zugriffs auf Azure-Ressourcen:

* Klassische Abonnementadministratorrollen ermöglichen eine grundlegende Zugriffsverwaltung und umfassen den Kontoadministrator, Dienstadministrator sowie Co-Administratoren.
    * Wenn Sie sich für ein neues Azure-Abonnement registrieren, wird Ihr Konto standardmäßig sowohl als Kontoadministrator als auch als Dienstadministrator eingerichtet.
    * Co-Administratoren können nach der Registrierung hinzugefügt werden.
* Die rollenbasierte Zugriffssteuerung (RBAC) ist ein neueres System, das eine präzise Zugriffsverwaltung mit vielen integrierten Rollen, Flexibilität in Bezug auf den Umfang und benutzerdefinierte Rollen ermöglicht.
    * Benutzer, die nur über RBAC-Rollen und nicht über klassische Abonnementadministrator-Rollen verfügen, können aber keine klassischen Azure-Bereitstellungen verwalten.

Um für eine bessere Kontrolle und eine Vereinfachung der Zugriffsverwaltung zu sorgen, empfehlen wir, RBAC für alle Anforderungen der Zugriffsverwaltung zu verwenden. Es ist ratsam, nach Möglichkeit vorhandene Zugriffsrichtlinien mit RBAC neu zu konfigurieren. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Hinzufügen eines Administrators mit der RBAC-Rolle „Besitzer“ für ein Abonnement im Azure-Portal 

Wenn Sie eine Person als Administrator für die Dienstverwaltung von Azure-Abonnements hinzufügen möchten, sollten Sie ihr die RBAC-Rolle „Besitzer“ zuordnen. Benutzer mit der Rolle „Besitzer“ können die Ressourcen in dem von Ihnen zugewiesenen Abonnement verwalten, verfügen aber über keine Zugriffsrechte für andere Abonnements.

1. Navigieren Sie im [ Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) zu **Abonnements**.
2. Wählen Sie das Abonnement aus, für das Sie Zugriff gewähren möchten.
3. Wählen Sie **Hinzufügen** aus.  
   (Wenn die Schaltfläche „Hinzufügen“ nicht vorhanden ist, sind Sie nicht berechtigt, Berechtigungen hinzuzufügen.)
4. Wählen Sie im Menü die Option **Zugriffssteuerung (IAM)** aus.
5. Wählen Sie im Feld **Rolle** die Option **Besitzer**. 
6. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung**. 
7. Geben Sie im Feld **Auswählen** die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten. Wählen Sie den Benutzer und dann **Speichern** aus.

    ![Screenshot mit ausgewählter Besitzerrolle](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Hierdurch erhält der Benutzer vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. Verwenden Sie das Menü „IAM“ des Bereichs, um Zugriff für einen anderen Bereich zu gewähren, z.B. eine Ressourcengruppe. 

## <a name="add-or-change-co-administrator"></a>Hinzufügen oder Ändern eines Co-Administrators

Nur ein Besitzer kann als Co-Administrator hinzugefügt werden. Andere Benutzer mit Rollen wie „Mitwirkender“ oder „Leser“ können nicht als Co-Administratoren hinzugefügt werden.

> [!TIP]
> Sie müssen lediglich das Konto „Besitzer“ als Co-Administrator hinzufügen, wenn der Benutzer klassische Azure-Bereitstellungen verwalten soll. Wir empfehlen Ihnen, für alle anderen Zwecke RBAC zu nutzen.

1. Wenn noch nicht erfolgt, fügen Sie einen Benutzer entsprechend den Anweisungen oben als Besitzer hinzu.
2. **Klicken Sie mit der rechten Maustaste** auf den soeben hinzugefügten Benutzer mit der Rolle „Besitzer“, und wählen Sie dann **Als Co-Administrator hinzufügen** aus. Wenn die Option **Als Co-Administrator hinzufügen** nicht angezeigt wird, können Sie die Seite aktualisieren oder es mit einem anderen Internetbrowser versuchen. 

    ![Screenshot, auf dem der Co-Administrator hinzugefügt wird](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Klicken Sie zum Entfernen der Co-Administratorberechtigung **mit der rechten Maustaste** auf den Benutzer „Co-Administrator“, und wählen Sie dann **Co-Administrator entfernen** aus.

    ![Screenshot, auf dem der Co-Administrator entfernt wird](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Ändern des Dienstadministrators für ein Azure-Abonnement

Nur der Kontoadministrator kann den Dienstadministrator für ein Abonnement ändern. Standardmäßig ist der Dienstadministrator bei der Registrierung identisch mit dem Kontoadministrator. Wenn der Dienstadministrator in einen anderen Benutzer geändert wird, verliert der Kontoadministrator den Zugriff auf das Azure-Portal. Der Kontoadministrator kann aber immer das Kontocenter verwenden, um den Dienstadministrator wieder auf sich selbst festzulegen.

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

  | Anmeldemethode | Hinzufügen eines Microsoft-Kontobenutzers als Dienstadministrator? | Hinzufügen eines Geschäfts-, Schul- oder Unikontos in der gleichen Organisation als Dienstadministrator? | Hinzufügen eines Geschäfts-, Schul- oder Unikontos in einer anderen Organisation als Dienstadministrator? |
  | --- | --- | --- | --- |
  |  Microsoft-Konto |Ja |Nein  |Nein  |
  |  Geschäfts-, Schul- oder Unikonto |Ja |Ja |Nein  |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Ändern des Kontoadministrators für ein Azure-Abonnement

Der Kontoadministrator ist der Benutzer, der sich ursprünglich für das Azure-Abonnement registriert hat, und er ist der verantwortliche Besitzer des Abonnements (auch in Bezug auf die Abrechnung). Informationen zum Ändern des Kontoadministrators für ein Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Sie sind nicht sicher, wer der Kontoadministrator ist?** Folgen Sie diesen Schritten:

1. Navigieren Sie im [ Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) zu **Abonnements**.
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.  

## <a name="types-of-classic-subscription-admins"></a>Arten von klassischen Abonnementadministratoren

 Kontoadministrator, Dienstadministrator und Co-Administrator sind die drei Arten von klassischen Abonnementadministratoren in Azure. Das Konto, das zum Registrieren für Azure verwendet wird, wird automatisch sowohl als Kontoadministrator als auch als Dienstadministrator eingerichtet. Anschließend können zusätzliche Co-Administratoren hinzugefügt werden. In der folgenden Tabelle sind die genauen Unterschiede zwischen diesen drei Administratorrollen beschrieben. 

> [!TIP]
> Zur besseren Kontrolle und für eine präzise Zugriffsverwaltung empfehlen wir die Verwendung der rollenbasierten Zugriffssteuerung von Azure, bei der Benutzer mehreren Rollen hinzugefügt werden können. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../role-based-access-control/overview.md).

| Administrator für klassisches Abonnement | Begrenzung | BESCHREIBUNG |
| --- | --- | --- |
| Kontoadministrator (AA) |1 pro Azure-Konto |Dies ist der Benutzer, der sich für das Azure-Abonnement registriert hat und für den Zugriff auf das [Kontocenter](https://account.azure.com/Subscriptions) und zum Ausführen verschiedener Verwaltungsaufgaben berechtigt ist. Hierzu gehört beispielsweise die Möglichkeit zum Erstellen von neuen Abonnements, Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement und Ändern des Dienstadministrators. Der Kontoadministrator ist vom Konzept her der für die Abrechnung relevante Besitzer des Abonnements. Bei RBAC wird dem Kontoadministrator keine Rolle zugewiesen.|
| Dienstadministrator (SA) |1 pro Azure-Abonnement |Diese Rolle ist zum Verwalten von Diensten im [Azure-Portal](https://portal.azure.com)berechtigt. Standardmäßig ist der Kontoadministrator für ein neues Abonnement gleichzeitig auch der Dienstadministrator. Bei RBAC wird die Rolle „Besitzer“ dem Dienstadministrator für den Abonnementbereich zugeordnet.|
| Co-Administrator (CA) |200 pro Abonnement |Diese Rolle verfügt über die gleichen Zugriffsrechte wie der Dienstadministrator, kann jedoch die Zuordnung von Abonnements zu Azure-Verzeichnissen nicht ändern. Bei RBAC wird die Rolle „Besitzer“ dem Co-Administrator für den Abonnementbereich zugeordnet.|

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Weitere Informationen zu Ressourcenzugriffssteuerung und Active Directory

* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Weitere Informationen zu Azure Active Directory finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) und [Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

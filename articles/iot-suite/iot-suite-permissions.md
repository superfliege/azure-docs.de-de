---
title: Azure IoT Suite und Azure Active Directory | Microsoft Docs
description: Informationen, wie Azure Active Directory von Azure IoT Suite verwendet werden, um Berechtigungen zu verwalten.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: ee7acd393539277a5bc23a6fed40d07961974b9a
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2017
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Berechtigungen für die Website „azureiotsuite.com“

## <a name="what-happens-when-you-sign-in"></a>Was bei der Anmeldung geschieht

Bei Ihrer ersten Anmeldung bei [azureiotsuite.com][lnk-azureiotsuite] bestimmt die Website basierend auf dem derzeit ausgewählten Azure Active Directory-Mandanten (AAD) und dem Azure-Abonnement Ihre Berechtigungsstufen.

1. Um die Liste der Mandanten aufzufüllen, die neben Ihrem Benutzernamen angezeigt werden, ermittelt die Website zunächst mithilfe von Azure, zu welchen AAD-Mandanten Sie gehören. Derzeit kann die Website nur Benutzertoken für jeweils einen Mandanten abrufen. Daher werden Sie, wenn Sie über das Dropdownfeld rechts oben den Mandanten wechseln, bei diesem Mandanten angemeldet, um die Token für diesen Mandanten zu beziehen.

2. Als Nächstes ermittelt die Website mithilfe von Azure, welche Abonnements Sie dem ausgewählten Mandanten zugeordnet haben. Die verfügbaren Abonnements werden angezeigt, wenn Sie eine neue vorkonfigurierte Lösung erstellen.

3. Abschließend ruft die Website alle Ressourcen in den Abonnements und Ressourcengruppen ab, die als vorkonfigurierte Lösungen gekennzeichnet sind, und füllt die Kacheln auf der Startseite auf.

In den beiden folgenden Abschnitten werden die Rollen beschrieben, die den Zugriff auf die vorkonfigurierten Lösungen steuern.

## <a name="aad-roles"></a>AAD-Rollen

Die AAD-Rollen steuern die Fähigkeit, vorkonfigurierte Lösungen bereitzustellen und Benutzer in einer vorkonfigurierten Lösung zu verwalten.

Weitere Informationen zu Administratorrollen in AAD finden Sie unter [Zuweisen von Administratorrollen in Azure AD][lnk-aad-admin]. Der aktuelle Artikel konzentriert sich auf die Verzeichnisrollen **Globaler Administrator** und **Benutzer**, die von den vorkonfigurierten Lösungen verwendet werden.

### <a name="global-administrator"></a>Globaler Administrator

Pro AAD-Mandant kann es viele globale Administratoren geben:

* Wenn Sie einen AAD-Mandanten erstellen, sind Sie standardmäßig der globale Administrator dieses Mandanten.
* Der globale Administrator kann eine vorkonfigurierte Basic- und Standard-Lösung bereitstellen.

### <a name="domain-user"></a>Domänenbenutzer

Pro AAD-Mandant können zahlreiche Domänenbenutzer vorhanden sein:

* Domänenbenutzer können eine vorkonfigurierte Basic-Lösung über die Website [azureiotsuite.com][lnk-azureiotsuite] bereitstellen.
* Domänenbenutzer können eine vorkonfigurierte Basic-Lösung über die Befehlszeilenschnittstelle erstellen.

### <a name="guest-user"></a>Gastbenutzer

Pro AAD-Mandant kann es viele Gastbenutzer geben. Gastbenutzer verfügen im AAD-Mandanten über begrenzte Rechte. Daher können Gastbenutzer keine vorkonfigurierte Lösung im AAD-Mandanten bereitstellen.

Weitere Informationen zu Benutzern und Rollen in AAD finden Sie in den folgenden Ressourcen:

* [Erstellen von Benutzern in Azure AD][lnk-create-edit-users]
* [Zuweisen von Benutzern zu Apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Administratorrollen für Azure-Abonnements

Azure-Administratorrollen steuern die Fähigkeit, einem AD-Mandanten ein Azure-Abonnement zuzuordnen.

Weitere Informationen zu den Azure-Administratorrollen finden Sie im Artikel [Hinzufügen oder Ändern des Co-Administrators, Dienstadministrators und Kontoadministrators in Azure][lnk-admin-roles].

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ich bin Dienstadministrator und möchte die Verzeichniszuordnung zwischen meinem Abonnement und einen bestimmten AAD-Mandanten ändern. Wie führe ich diese Aufgabe aus?

Informationen dazu finden Sie unter [Hinzufügen eines vorhandenen Abonnements zu Ihrem Azure AD-Verzeichnis](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-add-an-existing-subscription-to-your-azure-ad-directory).

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ich möchte bei Anmeldung über ein Organisationskonto einen Dienstadministrator oder Co-Administrator ändern.

Lesen Sie den Artikel [Ändern des Dienstadministrators und des Co-Administrators bei Anmeldung über ein Organisationskonto][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Warum wird dieser Fehler angezeigt? „Ihr Konto hat nicht die erforderlichen Berechtigungen zum Erstellen einer Lösung. Wenden Sie sich an den Administrator Ihres Kontos, oder versuchen Sie es mit einem anderen Konto.“

Orientieren Sie sich an folgendem Diagramm:

![][img-flowchart]

> [!NOTE]
> Wenn Sie den Fehler auch nach der Überprüfung noch sehen und ein globaler Administrator des AAD-Mandanten und ein Co-Administrator des Abonnements sind, können Sie Ihren Kontoadministrator bitten, den Benutzer zu entfernen und die erforderlichen Berechtigungen in dieser Reihenfolge neu zuzuweisen: Fügen Sie zuerst den Benutzer als globalen Administrator hinzu, und fügen Sie den Benutzer dann als Co-Administrator für das Azure-Abonnement hinzu. Verwenden Sie [Hilfe und Support][lnk-help-support], falls die Probleme weiterhin bestehen.

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Weshalb wird dieser Fehler angezeigt, wenn ich ein Azure-Abonnement habe? „Zum Erstellen vorkonfigurierter Lösungen ist ein Azure-Abonnement erforderlich. In nur wenigen Minuten können Sie ein kostenloses Testkonto erstellen.“

Wenn Sie sicher sind, dass Sie über ein Azure-Abonnement verfügen, überprüfen Sie die Mandantenzuordnung für Ihr Abonnement, und stellen Sie sicher, dass der ordnungsgemäße Mandant in der Dropdownliste ausgewählt ist. Nachdem Sie überprüft haben, ob der gewünschte Mandant ordnungsgemäß ist, überprüfen Sie anhand des obigen Diagramms die Zuordnung Ihres Abonnements und dieses AAD-Mandanten.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie mehr über IoT Suite erfahren möchten, lesen Sie, wie Sie [eine vorkonfigurierte Lösung anpassen][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md

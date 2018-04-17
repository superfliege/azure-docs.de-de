---
title: Privileged Identity Management für Azure-Ressourcen – Aktivieren der Abonnementverwaltung | Microsoft-Dokumentation
description: Es wird beschrieben, wie globale Administratoren im Mandanten Abonnements verwalten können.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/27/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4c6ae3da34fe5157314b8ea422591f7ecbd2a667
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="enable-subscription-management"></a>Aktivieren der Abonnementverwaltung

Als globaler Administrator Ihres Verzeichnisses haben Sie unter Umständen keinen Standardzugriff auf alle Abonnementressourcen Ihres Mandanten. In diesem Artikel werden die Schritte beschrieben, mit denen Sie sich selbst Zugriff für alle Abonnements Ihres Mandanten gewähren. Außerdem wird der empfohlene Ansatz zum Einhalten der Konformität mit allen Sicherheitskontrollen, die für Ihre Organisation nach der Erteilung des Zugriffs erforderlich sind, beschrieben.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Wer kann die Verwaltung von Abonnements in meinem Verzeichnis aktivieren?

Jeder Benutzer, der der Rolle „Globaler Administrator“ zugewiesen ist, muss die unten angegebenen Schritte ausführen, um die Abonnementverwaltung zu ermöglichen. Nachdem Sie die Abonnementverwaltung für sich selbst aktiviert haben, können Sie weitere globale Administratoren hinzufügen, die ggf. ebenfalls Ressourcenzugriff benötigen. Es gibt keine Verzeichniseinstellung, mit der der Zugriff für alle Mitglieder der Rolle „Globaler Administrator“ aktiviert wird.

## <a name="log-on-to-the-azure-portal"></a>Anmelden am Azure-Portal

Melden Sie sich zunächst mit einem Konto am Azure-Portal an, das für die Rolle „Globaler Administrator“ ein berechtigtes oder aktives Mitglied ist. Wenn das Konto ein berechtigter „Globaler Administrator“ ist, müssen Sie zuerst die Rolle aktivieren, bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="access-the-azure-ad-admin-center"></a>Zugreifen auf das Azure AD Admin Center

Nachdem Sie sich als globaler Administrator am Azure-Portal angemeldet haben, können Sie die Einstellungen bearbeiten, die den Zugriff auf Azure-Abonnements ermöglichen. Navigieren Sie zum Azure AD Admin Center, und wählen Sie im linken Navigationsbereich die Registerkarte „Eigenschaften“.

![](media/azure-pim-resource-rbac/aad_properties.png)

Schalten Sie die Option „Ein globaler Administrator kann Azure-Abonnements verwalten“ in der Liste mit den Eigenschaften auf „Ja“ um.

![](media/azure-pim-resource-rbac/aad_properties_save.png)

## <a name="navigate-to-azure-ad-pim"></a>Navigieren zu Azure AD PIM

Wenn diese Option aktiviert ist, wird Ihr Konto automatisch der Rolle „Benutzerzugriffsadministrator“ für jede Abonnementressource des Mandanten hinzugefügt. Navigieren Sie von hier aus zu Azure AD PIM, und wählen Sie im linken Navigationsbereich im Abschnitt „Verwalten“ die Azure-Ressourcen aus.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Verwalten und Ermitteln von Ressourcen

Wenn Ihre Organisation Azure AD PIM bereits nutzt, um Administratoren in Azure Active Directory zu schützen, wird beim Laden des Blatts eine Liste mit Abonnements angezeigt.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Überprüfen Sie Folgendes, wenn keine Ressourcen angezeigt werden:
>- Ablauf der Rolle „Globaler Administrator“ 
>- Vorhandensein eines Azure-Abonnements für Ihre Organisation

![](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Konfigurieren von Zuweisungen

Wählen Sie in der Liste ein Abonnement aus, und weisen Sie sich selbst (oder eine Gruppe, in der Sie Mitglied sind) als verfügbaren Besitzer der Ressource zu. 
[Hier finden Sie weitere Informationen zum Zuweisen von Rollen](pim-resource-roles-assign-roles.md).

Wiederholen Sie diesen Vorgang für jede Ressource, bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="clean-up-standing-access"></a>Bereinigen des ständigen Zugriffs

Nachdem Sie nun über passende Zuweisungen für die wichtigen Abonnements Ihrer Organisation verfügen, können Sie den ständigen Zugriff bereinigen, indem Sie die Option in den Verzeichniseigenschaften deaktivieren:

![](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Nächste Schritte

[Ermitteln von Ressourcen](pim-resource-roles-discover-resources.md)

[Konfigurieren von Rolleneinstellungen](pim-resource-roles-configure-role-settings.md)









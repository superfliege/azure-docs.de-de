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
ms.openlocfilehash: e164d8adaf5df63dba31bb6aa8e56f768741479c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33200874"
---
# <a name="enable-subscription-management-in-your-tenant"></a>Aktivieren der Abonnementverwaltung in Ihrem Mandanten

Als globaler Administrator Ihres Verzeichnisses haben Sie unter Umständen keinen Standardzugriff auf alle Abonnementressourcen Ihres Mandanten. In diesem Artikel werden die Schritte beschrieben, mit denen Sie sich Zugriff auf alle Abonnements in Ihrem Mandanten gewähren. Darüber hinaus wird ein Ansatz zum Aufrechterhalten der Konformität mit allen Sicherheitssteuerungen empfohlen, die Ihre Organisation benötigt, nachdem Sie Zugriff erhalten.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Wer kann die Verwaltung von Abonnements in meinem Verzeichnis aktivieren?

Jeder Benutzer, der der Rolle „Globaler Administrator“ zugewiesen ist, muss die unten angegebenen Schritte ausführen, um die Abonnementverwaltung zu ermöglichen. Nachdem Sie die Abonnementverwaltung für sich selbst aktiviert haben, können Sie weitere globale Administratoren hinzufügen, die ggf. ebenfalls Ressourcenzugriff benötigen. Es gibt keine Verzeichniseinstellung, mit der der Zugriff für alle Mitglieder der Rolle „Globaler Administrator“ aktiviert wird.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit einem Konto beim Azure-Portal an, das für die Rolle „Globaler Administrator“ ein berechtigtes oder aktives Mitglied ist. Wenn das Konto ein berechtigter „Globaler Administrator“ ist, müssen Sie zuerst die Rolle aktivieren, bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="access-the-azure-active-directory-admin-center"></a>Zugreifen auf das Azure Active Directory Admin Center

Nachdem Sie sich als globaler Administrator beim Azure-Portal angemeldet haben, können Sie die Einstellungen bearbeiten, die den Zugriff auf Azure-Abonnements ermöglichen. Suchen Sie das Azure Active Directory Admin Center (Azure AD), und wählen Sie **Eigenschaften** aus.

![Screenshot des Azure AD Admin Center, wobei „Eigenschaften“ hervorgehoben ist](media/azure-pim-resource-rbac/aad_properties.png)

Wählen Sie in der Liste mit den Eigenschaften unter **Ein globaler Administrator kann Azure-Abonnements verwalten** die Option **Ja** aus.

![Screenshot der Eigenschaftenseite mit auf „Ja“ festgelegter Umschaltfläche](media/azure-pim-resource-rbac/aad_properties_save.png)

Jetzt wird Ihr Konto automatisch der Rolle Benutzerzugriffsadministrator für jede Abonnementressource des Mandanten hinzugefügt.

## <a name="browse-to-azure-ad-pim"></a>Navigieren zu Azure AD PIM

 Wechseln Sie von hier zu Azure AD Privileged Identity Management (PIM). Wählen Sie unter **Verwalten** die Option **Azure-Ressourcen** aus.

![Screenshot von PIM mit hervorgehobenen Azure-Ressourcen](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Verwalten und Ermitteln von Ressourcen

Wenn Ihre Organisation Azure AD PIM bereits nutzt, um Administratoren in Azure AD zu schützen, wird beim Laden des Blatts eine Liste mit Abonnements angezeigt.

![Screenshot von PIM mit im Blatt angezeigter Liste der Abonnements](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Überprüfen Sie Folgendes, wenn keine Ressourcen angezeigt werden:
>- Ihre Rolle „Globaler Administrator“ ist nicht abgelaufen. 
>- Für Ihre Organisation ist ein Azure-Abonnement vorhanden.

![Screenshot von PIM mit leerer Ressourcenliste](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Konfigurieren von Zuweisungen

Wählen Sie in der Liste ein Abonnement aus, und weisen Sie sich selbst (oder eine Gruppe, in der Sie Mitglied sind) als verfügbaren Besitzer der Ressource zu. 
[Hier finden Sie weitere Informationen zum Zuweisen von Rollen](pim-resource-roles-assign-roles.md).

Wiederholen Sie diesen Vorgang für jede Ressource, bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="clean-up-standing-access"></a>Bereinigen des ständigen Zugriffs

Nachdem Sie nun über passende Zuweisungen für die wichtigen Abonnements Ihrer Organisation verfügen, können Sie den ständigen Zugriff bereinigen, indem Sie die Option in den Verzeichniseigenschaften deaktivieren.

![Screenshot der Eigenschaftenseite mit auf „Nein“ festgelegter Umschaltfläche](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Nächste Schritte

[Ermitteln von Ressourcen](pim-resource-roles-discover-resources.md)

[Konfigurieren von Rolleneinstellungen](pim-resource-roles-configure-role-settings.md)









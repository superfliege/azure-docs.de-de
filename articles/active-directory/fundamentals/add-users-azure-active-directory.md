---
title: Hinzufügen oder Löschen von Benutzern – Azure Active Directory | Microsoft-Dokumentation
description: Anweisungen zum Hinzufügen neuer Benutzer oder Löschen vorhandener Benutzer mithilfe von Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3881026c1877059639b550f3d0342674b2f8b29
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187201"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Hinzufügen oder Löschen von Benutzern in Azure Active Directory
In Ihrem Azure Active Directory-Mandanten (Azure AD) können Sie neue Benutzer hinzufügen oder vorhandene Benutzer löschen.

## <a name="add-a-new-user"></a>Hinzufügen eines neuen Benutzers
Sie können einen neuen Benutzer im Azure Active Directory-Portal erstellen.

### <a name="to-add-a-new-user"></a>So fügen Sie einen neuen Benutzer hinzu
1. Melden Sie sich als globaler Administrator oder Benutzeradministrator für das Verzeichnis beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Azure Active Directory** aus, dann **Benutzer** und schließlich **Neuer Benutzer**.

    ![Seite „Benutzer – Alle Benutzer“ mit Hervorhebung von „Neuer Benutzer“](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. Geben Sie auf der Seite **Benutzer** die erforderlichen Informationen ein.

    ![„Neuen Benutzer hinzufügen“, Seite „Benutzer“ mit Benutzerinformationen](media/add-users-azure-active-directory/new-user-user-blade.png)

    - **Name (erforderlich).** Der Vor- und Nachname des neuen Benutzers. Beispielsweise Mary Parker.

    - **Benutzername (erforderlich).** Der Benutzername des neuen Benutzers. Beispiel: mary@contoso.com. 
    
        Der Domänenteil des Benutzernamens muss entweder der anfängliche Standarddomänenname sein, „<_IhrDomänenname_>.onmicrosoft.com“, oder ein benutzerdefinierter Domänennamen, z. B. „contoso.com“. Weitere Informationen zum Erstellen eines benutzerdefinierten Domänennamens finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](add-custom-domain.md).

    - **Profil.** Optional können Sie weitere Informationen zum Benutzer hinzufügen. Sie können Benutzerinformationen auch zu einem späteren Zeitpunkt hinzufügen. Weitere Informationen zum Hinzufügen von Benutzerinformationen finden Sie unter [Hinzufügen oder Ändern von Benutzerprofilinformationen](active-directory-users-profile-azure-portal.md).

    - **Gruppen.** Optional können Sie den Benutzer einer oder mehreren vorhandenen Gruppen hinzufügen. Sie können den Benutzer auch später zu Gruppen hinzufügen. Weitere Informationen zum Hinzufügen von Benutzern zu Gruppen finden Sie unter [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md).

    - **Verzeichnisrolle.** Optional können Sie den Benutzer einer Verzeichnisrolle hinzufügen. Sie können den Benutzer als globalen Administrator zuweisen oder ihn mindestens einer der anderen Administratorrollen in Azure AD zuweisen. Weitere Informationen zum Zuweisen von Rollen finden Sie unter [Zuweisen von Rollen zu Benutzern](active-directory-users-assign-role-azure-portal.md).

4. Kopieren Sie das automatisch generierte Kennwort aus dem Feld **Kennwort**. Sie müssen dieses Kennwort dem Benutzer für den ersten Anmeldeprozess bereitstellen.

5. Klicken Sie auf **Erstellen**.

    Der Benutzer wird erstellt und Ihrem Azure AD-Mandanten hinzugefügt.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Hinzufügen eines neuen Benutzers in einer Hybridumgebung
Wenn Sie eine Umgebung mit sowohl Azure Active Directory (Cloud) als auch Windows Server Active Directory (lokal) haben, können Sie neue Benutzer hinzufügen, indem Sie die vorhandenen Benutzerkontodaten synchronisieren. Weitere Informationen zu Hybridumgebungen und Benutzern finden Sie unter [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Löschen eines Benutzers
Sie können einen vorhandenen Benutzer im Azure Active Directory-Portal löschen.

### <a name="to-delete-a-user"></a>So löschen Sie einen Benutzer
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie zuerst **Azure Active Directory** und dann **Benutzer** aus, und suchen Sie dann nach dem Benutzer, den Sie aus dem Azure AD-Mandanten löschen möchten, und wählen Sie ihn aus. Beispielsweise _Mary Parker_.

3. Wählen Sie **Benutzer löschen** aus.

    ![Seite „Benutzer – Alle Benutzer“ mit Hervorhebung von „Benutzer löschen“](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    Der Benutzer wird gelöscht und nicht mehr auf der Seite **Benutzer – Alle Benutzer** angezeigt. Sie können den Benutzer für die nächsten 30 Tage auf der Seite **Gelöschte Benutzer** anzeigen und während dieses Zeitraums wiederherstellen. Weitere Informationen zum Wiederherstellen eines Benutzers finden Sie unter [Wiederherstellen oder endgültiges Entfernen eines kürzlich gelöschten Benutzers](active-directory-users-restore.md).

    >[!Note]
    >Sie müssen Windows Server Active Directory verwenden, um die Identität, Kontaktinformationen oder Jobinformationen für Benutzer zu aktualisieren, deren Quelle der Autorität Windows Server Active Directory ist. Nachdem Sie das Update abgeschlossen haben, müssen Sie bis zum Abschluss des nächsten Synchronisierungszyklus warten, bevor die Änderungen angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihre Benutzer hinzugefügt haben, können Sie die folgenden grundlegenden Prozesse ausführen:

- [Hinzufügen oder Ändern von Profilinformationen](active-directory-users-profile-azure-portal.md)

- [Zuweisen von Rollen zu Benutzern](active-directory-users-assign-role-azure-portal.md)

- [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)

- [Arbeiten mit dynamischen Gruppen und Benutzern](../users-groups-roles/groups-create-rule.md)

Oder Sie können andere Benutzerverwaltungsaufgaben durchführen wie das [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis](../b2b/what-is-b2b.md) oder das [Wiederherstellen eines gelöschten Benutzers](active-directory-users-restore.md). Weitere Informationen zu anderen verfügbaren Aktionen finden Sie unter [Dokumentation zur Azure Active Directory-Benutzerverwaltung](../users-groups-roles/index.yml).

---
title: Wiederherstellen oder dauerhaftes Löschen eines kürzlich gelöschten Benutzers – Azure Active Directory | Microsoft-Dokumentation
description: Vorgehensweise beim Anzeigen von wiederherstellbaren Benutzern, Wiederherstellen eines gelöschten Benutzers oder endgültigen Löschen eines Benutzers mithilfe von Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: a810ae13d9cfb68d11293ba883c52858aa4a2deb
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164752"
---
# <a name="restore-or-remove-a-recently-deleted-user-using-azure-active-directory"></a>Wiederherstellen oder Entfernen eines kürzlich gelöschten Benutzers mit Azure Active Directory
Nachdem Sie einen Benutzer gelöscht haben, verbleibt das Konto 30 Tage lang im angehaltenen Zustand. Während dieses Zeitfensters von 30 Tagen kann das Benutzerkonto mit allen zugehörigen Eigenschaften wiederhergestellt werden. Nachdem die 30 Tage verstrichen sind, wird der Benutzer automatisch und endgültig gelöscht.

Sie können Ihre wiederherstellbaren Benutzer anzeigen, einen gelöschten Benutzer wiederherstellen oder einen Benutzer endgültig löschen, indem Sie im Azure-Portal Azure Active Directory (Azure AD) verwenden.

>[!Important]
>Weder Sie selbst noch der Microsoft-Kundendienst kann einen endgültig gelöschten Benutzer wiederherstellen.

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Sie müssen über eine der folgenden Rollen verfügen, um Benutzer wiederherstellen und endgültig löschen zu können.

- Unternehmensadministrator

- Partnersupport der Ebene 1

- Partnersupport der Ebene 2

- Benutzerkontoadministrator

## <a name="view-your-restorable-users"></a>Anzeigen Ihrer wiederherstellbaren Benutzer
Sie können alle Benutzer anzeigen, die vor weniger als 30 Tagen gelöscht wurden. Diese Benutzer können wiederhergestellt werden.

### <a name="to-view-your-restorable-users"></a>So zeigen Sie Ihre wiederherstellbaren Benutzer an
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Azure Active Directory** und dann **Benutzer** und **Gelöschte Benutzer**.

    Überprüfen Sie die Liste mit den Benutzern, die wiederhergestellt werden können.

    ![Seite „Benutzer – Gelöschte Benutzer“ mit Benutzern, für die die Wiederherstellung noch möglich ist](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Wiederherstellen eines kürzlich gelöschten Benutzers
Während sich das Konto eines Benutzers im angehaltenen Zustand befindet, werden alle zugehörigen Verzeichnisinformationen beibehalten. Wenn Sie einen Benutzer wiederherstellen, werden diese Verzeichnisinformationen ebenfalls wiederhergestellt.

### <a name="to-restore-a-user"></a>So stellen Sie einen Benutzer wieder her
1. Suchen Sie auf der Seite **Benutzer – Gelöschte Benutzer** nach einem verfügbaren Benutzer, und wählen Sie ihn aus. Beispielsweise _Mary Parker_.

2. Wählen Sie **Benutzer wiederherstellen** aus.

    ![Seite „Benutzer – Gelöschte Benutzer“ mit hervorgehobener Option „Endgültig löschen“](media/active-directory-users-restore/users-deleted-users-restore-user.png)

## <a name="permanently-delete-a-user"></a>Dauerhaftes Löschen eines Benutzers
Sie können einen Benutzer auch endgültig aus Ihrem Verzeichnis löschen, ohne darauf zu warten, dass der Löschvorgang nach Ablauf der 30 Tage durchgeführt wird. Ein endgültig gelöschter Benutzer kann weder von Ihnen selbst, noch von einem anderen Administrator oder vom Microsoft-Kundendienst wiederhergestellt werden.

>[!Note]
>Wenn Sie einen Benutzer versehentlich endgültig löschen, müssen Sie einen neuen Benutzer erstellen und alle Informationen manuell eingeben, die zuvor vorhanden waren. Weitere Informationen zum Erstellen eines neuen Benutzers finden Sie unter [Vorgehensweise: Hinzufügen oder Löschen von Benutzern in Azure Active Directory](add-users-azure-active-directory.md).

### <a name="to-permanently-delete-a-user"></a>So löschen Sie einen Benutzer endgültig

1. Suchen Sie auf der Seite **Benutzer – Gelöschte Benutzer** nach einem verfügbaren Benutzer, und wählen Sie ihn aus. Beispiel: _Rae Huff_.

2. Wählen Sie **Endgültig löschen** aus.

    ![Seite „Benutzer – Gelöschte Benutzer“ mit hervorgehobener Option „Endgültig löschen“](media/active-directory-users-restore/users-deleted-users-permanent-delete-user.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihre Benutzer wiederhergestellt oder gelöscht haben, können Sie die folgenden grundlegenden Prozesse ausführen:

- [Hinzufügen oder Löschen von Benutzern](add-users-azure-active-directory.md)

- [Zuweisen von Rollen zu Benutzern](active-directory-users-assign-role-azure-portal.md)

- [Hinzufügen oder Ändern von Profilinformationen](active-directory-users-profile-azure-portal.md)

- [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis](../b2b/what-is-b2b.md) 

Weitere Informationen zu anderen verfügbaren Aufgaben der Benutzerverwaltung finden Sie unter [Dokumentation zur Azure Active Directory-Benutzerverwaltung](../users-groups-roles/index.yml).

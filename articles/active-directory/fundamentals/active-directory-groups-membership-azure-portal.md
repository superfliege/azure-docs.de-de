---
title: Hinzufügen oder Entfernen einer Gruppe zu bzw. aus einer anderen Gruppe in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Gruppe in Azure Active Directory zu einer anderen Gruppe hinzufügen bzw. die Gruppe entfernen.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: c28fe5ef226fac993fde221b16bfa875ba4845ca
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579767"
---
# <a name="how-to-add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Gewusst wie: Hinzufügen oder Entfernen einer Gruppe zu bzw. aus einer anderen Gruppe in Azure Active Directory
In diesem Artikel erfahren Sie mehr zum Hinzufügen oder Entfernen einer Gruppe zu bzw. aus einer anderen Gruppe in Azure Active Directory.

>[!Note]
>Wenn Sie die übergeordnete Gruppe löschen möchten, finden Sie Informationen dazu unter [Aktualisieren oder Löschen einer Gruppe und ihre Mitglieder](active-directory-groups-delete-group.md).

## <a name="add-a-group-as-a-member-to-another-group"></a>Hinzufügen einer Gruppe als Mitglied zu einer anderen Gruppe
Sie können eine bestehende Gruppe zu einer anderen bestehenden Gruppe hinzufügen, indem Sie eine Mitgliedergruppe (Untergruppe) und eine übergeordnete Gruppe erstellen. Die Mitgliedergruppe übernimmt die Attribute und Eigenschaften der übergeordneten Gruppe, wodurch Sie Zeit bei der Konfiguration sparen.

### <a name="to-add-a-group-as-a-member-to-another-group"></a>Hinzufügen einer Gruppe als Mitglied zu einer anderen Gruppe

1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Azure Active Directory** und dann **Gruppen** aus.

3. Suchen und wählen Sie auf der Seite **Gruppen – Alle Gruppen** die Gruppe aus, die Mitglied einer anderen Gruppe werden soll. In dieser Übung verwenden wir die Gruppe **MDM-Richtlinie – Westen**.

    >[!Note]
    >Sie können Ihre Gruppe als Mitglied jeweils nur zu einer Gruppe hinzufügen. Darüber hinaus filtert das Feld **Gruppe auswählen** die Anzeige basierend auf einem Abgleich Ihrer Eingabe mit einem beliebigen Teil eines Benutzer- oder Gerätenamens. Platzhalterzeichen werden jedoch nicht unterstützt.

    ![Seite „Gruppen – Alle Gruppen“ mit ausgewählter „MDM-Richtlinie – Westen“](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. Wählen Sie auf der Seite **MDM-Richtlinie – Westen – Gruppenmitgliedschaften** die Option **Gruppenmitgliedschaften**, klicken Sie auf **Hinzufügen**, suchen Sie die Gruppe, zu der Ihre Gruppe gehören soll, und wählen Sie dann **Auswählen**. In dieser Übung verwenden wir die Gruppe **MDM-Richtlinie – Alle Org**.

    Die Gruppe **MDM-Richtlinie – Westen** ist nun Mitglied der Gruppe **MDM-Richtlinie – Alle Org.** und übernimmt alle Eigenschaften und Konfigurationen der Gruppe „MDM-Richtlinie – Alle Org.“.

    ![Erstellen einer Gruppenmitgliedschaft durch Hinzufügen einer Gruppe zu einer anderen Gruppe](media/active-directory-groups-membership-azure-portal/add-group-membership.png)

5. Lesen Sie die Seite **MDM-Richtlinien – Westen – Gruppenmitgliedschaften**, um die Gruppe-Mitglied-Beziehung anzuzeigen.

    ![Seite „MDM-Richtlinie – Westen – Gruppenmitgliedschaften“ mit Anzeige der übergeordneten Gruppe](media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Für eine detailliertere Ansicht der Gruppe-Mitglied-Beziehung wählen Sie die Gruppe (**MDM-Richtlinie – Alle Org.**) und schauen Sie sich die Seitendetails **MDM-Richtlinie – Westen** an.

    ![Seite „Gruppenmitgliedschaft“, die sowohl das Mitglied als auch die Gruppendetails anzeigt](media/active-directory-groups-membership-azure-portal/group-membership-review.png)

## <a name="remove-a-member-group-from-another-group"></a>Entfernen einer Mitgliedergruppe aus einer anderen Gruppe
Sie können eine vorhandene Mitgliedergruppe aus einer anderen Gruppe entfernen. Das Entfernen der Mitgliedschaft entfernt jedoch auch alle übernommenen Attribute und Eigenschaften für Ihre Benutzer.

### <a name="to-remove-a-member-group-from-another-group"></a>Entfernen einer Mitgliedergruppe aus einer anderen Gruppe
1. Suchen und wählen Sie auf der Seite **Gruppen – Alle Gruppen** die Gruppe aus, die als Mitglied einer anderen Gruppe entfernt werden soll. In dieser Übung verwenden wir wieder die Gruppe **MDM-Richtlinie – Westen**.

2. Klicken Sie auf der Übersichtsseite für **MDM-Richtlinie – Westen** auf **Gruppenmitgliedschaften**.

    ![Übersichtsseite „MDM-Richtlinie – Westen“](media/active-directory-groups-membership-azure-portal/group-membership-overview.png)

3. Wählen Sie die Gruppe **MDM-Richtlinie – Alle Org.** auf der Seite **MDM-Richtlinie – Westen – Gruppenmitgliedschaften** aus, und wählen Sie dann **Entfernen** auf der Detailseite **MDM-Richtlinie – Westen**.

    ![Seite „Gruppenmitgliedschaft“, die sowohl das Mitglied als auch die Gruppendetails anzeigt](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)


## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

- [Anzeigen Ihrer Gruppen und Mitglieder](active-directory-groups-view-azure-portal.md)

- [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)

- [Hinzufügen oder Entfernen von Mitgliedern zu/aus einer Gruppe](active-directory-groups-members-azure-portal.md)

- [Bearbeiten Ihrer Gruppeneinstellungen](active-directory-groups-settings-azure-portal.md)

- [Zuweisen von Lizenzen zu Benutzern nach Gruppe](../users-groups-roles/licensing-groups-assign.md)

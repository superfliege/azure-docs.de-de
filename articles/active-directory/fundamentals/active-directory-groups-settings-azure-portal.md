---
title: Bearbeiten Ihrer Gruppeninformationen mit Azure Active Directory| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Informationen einer Gruppe mit Azure Active Directory bearbeiten.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a02987fdce3a15cd5d416234e3717df6d33622ec
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731341"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>Vorgehensweise: Bearbeiten Ihrer Gruppeninformationen mit Azure Active Directory

Mit Azure Active Directory, können Sie die Einstellungen einer Gruppe bearbeiten, einschließlich der Aktualisierung ihres Namens, der Beschreibung oder des Mitgliedschaftstyps.

## <a name="to-edit-your-group-settings"></a>So bearbeiten Sie Ihre Gruppeneinstellungen
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Azure Active Directory** und dann **Gruppen** aus.

    Die Seite **Gruppen – Alle Gruppen** wird mit allen Ihren aktiven Gruppen angezeigt.

3. Geben Sie auf der Seite **Gruppen – Alle Gruppen** so viel von dem Gruppennamen in das Feld **Suche** ein, wie möglich. Für die Zwecke dieses Artikels suchen wir nach der Gruppe **MDM policy – West** (MDM-Richtlinie – Westen).

    Die Suchergebnisse werden unterhalb des Felds **Suche** angezeigt. Sie werden aktualisiert, während Sie mehr Zeichen eingeben.

    ![Seite „Alle Gruppen“ mit Suchtext im Suchfeld](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Wählen Sie die Gruppe **MDM policy – West** (MDM-Richtlinie – Westen) aus, und wählen Sie dann **Eigenschaften** im Bereich **Verwalten** aus.

    ![Seite „Gruppenübersicht“ mit Anzahl und Mitgliedern sowie hervorgehobener Option „Mitglied“](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Aktualisieren Sie die Informationen unter **Allgemeine Einstellungen** nach Bedarf, einschließlich:

    ![Einstellungen für die Eigenschaften einer Gruppe](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Gruppenname.** Bearbeiten Sie den vorhandenen Gruppennamen.
    
    - **Gruppenbeschreibung.** Bearbeiten Sie die vorhandene Gruppenbeschreibung.

    - **Gruppentyp.** Sie können den Typ der Gruppe nach deren Erstellung nicht mehr ändern. Um den **Gruppentyp** zu ändern, müssen Sie die Gruppe löschen und eine neue erstellen.
    
    - **Mitgliedschaftstyp.** Ändern Sie den Typs der Mitgliedschaft. Weitere Informationen zu den verschiedenen verfügbaren Mitgliedschaftstypen finden Sie unter [Vorgehensweise: Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern über das Azure Active Directory-Portal](active-directory-groups-create-azure-portal.md).
    
    - **Objekt-ID.** Sie können die Objekt-ID nicht ändern, aber Sie können sie kopieren, um sie in Ihren PowerShell-Befehlen für die Gruppe zu verwenden. Weitere Informationen zur Verwendung von PowerShell-Cmdlets finden Sie unter [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

- [Anzeigen Ihrer Gruppen und Mitglieder](active-directory-groups-view-azure-portal.md)

- [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)

- [Hinzufügen oder Entfernen von Mitgliedern zu/aus einer Gruppe](active-directory-groups-members-azure-portal.md)

- [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](../users-groups-roles/groups-create-rule.md)

- [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)

- [Verwalten des Zugriffs auf Ressourcen mittels Gruppen](active-directory-manage-groups.md)

- [Zuordnen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

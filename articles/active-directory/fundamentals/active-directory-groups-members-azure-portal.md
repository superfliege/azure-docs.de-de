---
title: Hinzufügen oder Entfernen von Gruppenmitgliedern – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Anleitungen zum Hinzufügen oder Entfernen von Mitgliedern zu bzw. aus einer Gruppe in Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.openlocfilehash: c92ac805fe9c8f3554044aa85306ecc43464411b
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445343"
---
# <a name="add-or-remove-group-members-using-azure-active-directory"></a>Hinzufügen oder Entfernen von Gruppenmitgliedern mithilfe von Azure Active Directory
Mit Azure Active Directory können weiterhin Gruppenmitglieder hinzugefügt und entfernt werden.

## <a name="to-add-group-members"></a>So fügen Sie Gruppenmitglieder hinzu

1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Azure Active Directory** und anschließend auf **Gruppen**.

3. Suchen Sie auf der Seite **Gruppen – Alle Gruppen** nach der Gruppe, der Sie Mitglieder hinzufügen möchten, und wählen Sie die Gruppe aus. Verwenden Sie in diesem Beispiel die zuvor erstellte Gruppe **MDM policy - West**.

    ![Seite „Gruppen – Alle Gruppen“ mit hervorgehobenem Gruppennamen](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. Wählen Sie auf der Übersichtsseite für **MDM policy - West** im Bereich **Verwalten** die Option **Mitglieder** aus.

    ![Übersichtsseite für „MDM policy - West“ mit hervorgehobener Option „Mitglieder“](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. Klicken Sie auf **Mitglieder hinzufügen**, suchen Sie nach den einzelnen Mitgliedern, die Sie der Gruppe hinzufügen möchten, wählen Sie sie aus, und klicken Sie anschließend auf **Auswählen**.

    Sie erhalten eine Nachricht mit dem Hinweis, dass die Mitglieder erfolgreich hinzugefügt wurden.

    ![Seite zum Hinzufügen von Mitgliedern mit Anzeige des gesuchten Mitglieds](media/active-directory-groups-members-azure-portal/update-members.png)

6. Aktualisieren Sie den Bildschirm, um alle Namen der Mitglieder anzuzeigen, die der Gruppe hinzugefügt wurden.

## <a name="to-remove-group-members"></a>So entfernen Sie Gruppenmitglieder

1. Suchen Sie auf der Seite **Gruppen – Alle Gruppen** nach der Gruppe, aus der Sie Mitglieder entfernen möchten, und wählen Sie die Gruppe aus. Wir verwenden hier erneut **MDM policy - West**.

2. Klicken Sie im Bereich **Verwalten** auf **Mitglieder**, suchen Sie nach dem Namen des Mitglieds, das Sie entfernen möchten, wählen Sie das Mitglied aus, und klicken Sie anschließend auf **Entfernen**.

    ![Seite mit den Mitgliedsinformationen und der Option „Entfernen“](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen Ihrer Gruppen und Mitglieder](active-directory-groups-view-azure-portal.md)

- [Bearbeiten Ihrer Gruppeneinstellungen](active-directory-groups-settings-azure-portal.md)

- [Verwalten des Zugriffs auf Ressourcen mittels Gruppen](active-directory-manage-groups.md)

- [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](../users-groups-roles/groups-create-rule.md)

- [Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

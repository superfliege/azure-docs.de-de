---
title: Nächste Schritte für die Zugriffsverwaltung mithilfe von Gruppen – Azure AD | Microsoft-Dokumentation
description: Erweiterte Vorgehensweisen zum Verwalten von Sicherheitsgruppen und Verwenden dieser Gruppen zum Verwalten des Zugriffs auf eine Ressource.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/12/2017
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: bdc8754253ce2567d957b4d6240fe52242aea2ea
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448944"
---
# <a name="managing-owners-for-a-group"></a>Verwalten von Besitzern einer Gruppe
Wenn ein Ressourcenbesitzer einer Ressource den Zugriff auf eine Azure AD-Gruppe zugewiesen hat, wird die Mitgliedschaft der Gruppe durch den Besitzer der Gruppe verwaltet. Tatsächlich delegiert der Besitzer der Ressource die Berechtigung, dieser Ressource Benutzer zuzuweisen, an den Besitzer der Gruppe.

## <a name="add-an-owner-to-a-group"></a>Hinzufügen eines Besitzers zu einer Gruppe

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Benutzer und Gruppen**.
2. Wählen Sie **Alle Gruppen** aus, und öffnen Sie dann die Gruppe, der Besitzer hinzugefügt werden sollen.
3. Wählen Sie **Besitzer hinzufügen**aus.
4. Wählen Sie auf der Seite **Besitzer hinzufügen** den Benutzer aus, den Sie als Besitzer dieser Gruppe hinzufügen möchten, und stellen Sie sicher, dass dieser Name dem Bereich **Ausgewählt** hinzugefügt wird.

## <a name="remove-an-owner-from-a-group"></a>Entfernen eines Besitzers aus einer Gruppe

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Benutzer und Gruppen**.
2. Wählen Sie **Alle Gruppen**, und öffnen Sie dann die Gruppe, aus der Sie Besitzer entfernen möchten.
3. Wählen Sie die Registerkarte **Besitzer** aus.
4. Wählen Sie den Besitzer aus, der aus der Gruppe entfernt werden soll, und wählen Sie dann **Entfernen**aus.

## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](../users-groups-roles/groups-settings-cmdlets.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](../active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](../connect/active-directory-aadconnect.md)

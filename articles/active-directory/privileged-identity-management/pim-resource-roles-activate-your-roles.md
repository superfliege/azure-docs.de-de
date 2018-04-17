---
title: Privileged Identity Management für Azure-Ressourcen – Aktivieren von Rollen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie in PIM Rollen aktivieren.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 3e5456e7a632639cb82d7ba2b2e073938b1798ef
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---activate"></a>Privileged Identity Management – Ressourcenrollen – Aktivieren
Zur Aktivierung von Rollen für Azure-Ressourcen wird eine neue Umgebung eingeführt, in der geeignete Rollenmitglieder die Aktivierung für einen späteren Zeitpunkt planen und eine bestimmte Aktivierungsdauer auswählen können, die innerhalb des (von Administratoren konfigurierten) Bereichs liegt. Erfahren Sie mehr über das [Aktivieren von Azure AD-Rollen](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Aktivieren von Rollen
Navigieren Sie in der linken Navigationsleiste zum Abschnitt „Meine Rollen“. Klicken Sie für die Rolle, die aktiviert werden soll, auf „Aktivieren“.
![](media/azure-pim-resource-rbac/rbac-roles.png)

Geben Sie im Aktivierungsmenü den gewünschten Startzeitpunkt (Datum und Uhrzeit) für die Rollenaktivierung ein. Verringern Sie optional die Aktivierungsdauer (also den Zeitraum, für den die Rolle aktiv ist), geben Sie ggf. eine Begründung ein, und klicken Sie anschließend auf „Aktivieren“.

Wenn Startdatum und -uhrzeit nicht geändert werden, wird die Rolle innerhalb der nächsten Sekunden aktiviert. Auf der Seite mit Ihren Rollen wird eine Bannermeldung mit dem Hinweis angezeigt, dass die zu aktivierende Rolle der Warteschlange hinzugefügt wurde. Klicken Sie auf die Aktualisierungsschaltfläche, um diese Meldung zu löschen.

![](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Wenn die Aktivierung für einen späteren Zeitpunkt geplant wurde, wird die ausstehende Anforderung im linken Navigationsmenü auf der Registerkarte für ausstehende Anforderungen angezeigt. Sollte die Rollenaktivierung nicht mehr benötigt werden, kann der Benutzer die Anforderung rechts auf der Seite durch Klicken auf die Schaltfläche „Abbrechen“ abbrechen.

![](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="just-enough-administration"></a>Minimale Administration

PIM für Azure-Ressourcen ermöglicht die problemlose Anwendung bewährter Methoden für minimale Administration (Just Enough Administration, JEA) in Verbindung mit Rollenzuweisungen. Benutzer und Gruppenmitglieder mit Zuweisungen in Azure-Abonnements oder Ressourcengruppen können ihre vorhandene Rollenzuweisung in einem eingeschränkten Bereich aktivieren. 

Suchen Sie über die Suchseite die untergeordnete Ressource, die Sie verwalten möchten.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Klicken Sie im linken Navigationsmenü auf „My roles“ (Meine Rollen), und wählen Sie die zu aktivierende Rolle aus. Beachten Sie, dass der Zuweisungstyp übernommen wird, da die Rolle nicht auf der Ressourcengruppenebene, sondern auf der Abonnementebene zugewiesen wurde, wie hier zu sehen:

![](media/azure-pim-resource-rbac/my-roles-02.png)

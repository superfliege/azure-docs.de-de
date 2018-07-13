---
title: Aktivieren von Rollen für Azure-Ressourcen mit Privileged Identity Management | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie in PIM Rollen aktivieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 483f30a6546cf21d93b96b08eede6afae828d9e4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38548303"
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Aktivieren von Rollen für Azure-Ressourcen mit Privileged Identity Management
Privileged Identity Management (PIM) bietet eine neue Umgebung zur Aktivierung von Rollen für Azure-Ressourcen. Berechtigte Rollenmitglieder können die Aktivierung für einen späteren Zeitpunkt planen. Zudem können sie eine bestimmte Aktivierungsdauer innerhalb des (von Administratoren konfigurierten) Bereichs auswählen. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren von Rollen in Azure AD Privileged Identity Management](pim-how-to-activate-role.md).

## <a name="activate-roles"></a>Aktivieren von Rollen
Navigieren Sie im linken Bereich zum Abschnitt **My roles** (Meine Rollen). Wählen Sie **Aktivieren** für die Rolle aus, die Sie aktivieren möchten.

![Registerkarte „Berechtigte Rollen“ im Bereich „My roles“ (Meine Rollen)](media/azure-pim-resource-rbac/rbac-roles.png)

Geben Sie im Menü **Aktivierungen** den Startzeitpunkt (Datum und Uhrzeit) für die Rollenaktivierung ein. Verringern Sie optional die Aktivierungsdauer (also den Zeitraum, für den die Rolle aktiv ist), und geben Sie gegebenenfalls eine Begründung ein. Wählen Sie dann **Aktivieren** aus.

Wenn Startdatum und -uhrzeit nicht geändert werden, wird die Rolle innerhalb der nächsten Sekunden aktiviert. Im Bereich **My roles** (Meine Rollen) wird in einer Bannermeldung angezeigt, dass eine Rolle zur Aktivierung in die Warteschlange gestellt wurde. Wählen Sie die Aktualisierungsschaltfläche aus, um diese Meldung zu löschen.

![Bereich „My roles“ (Meine Rollen) mit einer Bannermeldung und einer Benachrichtigung zu einer ausstehenden Genehmigung](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Wenn die Aktivierung für einen späteren Zeitpunkt geplant wurde, wird die ausstehende Anforderung im linken Bereich auf der Registerkarte **Ausstehende Anforderungen** angezeigt. Wenn die Rollenaktivierung nicht mehr benötigt wird, können Sie die Anforderung durch Auswählen der Schaltfläche **Abbrechen** abbrechen.

![Liste der ausstehenden Anforderungen mit den Schaltflächen „Abbrechen“](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Methoden zur minimalen Verwaltung

PIM für Azure-Ressourcen ermöglicht die problemlose Anwendung bewährter Methoden für minimale Verwaltung (Just Enough Administration, JEA) in Verbindung mit Rollenzuweisungen. Benutzer und Gruppenmitglieder mit Zuweisungen in Azure-Abonnements oder Ressourcengruppen können ihre vorhandene Rollenzuweisung in einem eingeschränkten Bereich aktivieren. 

Suchen Sie über die Suchseite die untergeordnete Ressource, die Sie verwalten möchten.

![Auswählen einer Ressource](media/azure-pim-resource-rbac/azure-resources-02.png)

Wählen Sie **My roles** (Meine Rollen) im linken Bereich und dann die zu aktivierende Rolle aus. Der Zuweisungstyp lautet **Geerbt**, da die Rolle nicht auf der Ressourcengruppenebene, sondern auf der Abonnementebene zugewiesen wurde.

![Liste der berechtigten Rollenzuweisungen mit dem hervorgehobenen Zuweisungstyp](media/azure-pim-resource-rbac/my-roles-02.png)

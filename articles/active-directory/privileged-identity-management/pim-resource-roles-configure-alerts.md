---
title: Konfigurieren von Sicherheitswarnungen für Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Sicherheitswarnungen für Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) konfigurieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 564517052796ee5dbc022ff92afcaa0216bdf8ea
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196837"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Konfigurieren von Sicherheitswarnungen für Azure-Ressourcenrollen in PIM
Privileged Identity Management (PIM) für Azure-Ressourcen generiert bei verdächtigen oder nicht sicheren Aktivitäten in Ihrer Umgebung Warnungen. Wenn eine Warnung ausgelöst wird, wird sie auf der Seite „Warnungen“ angezeigt. 

![Seite „Warnungen“](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Überprüfen von Warnungen
Wählen Sie eine Warnung aus, um einen Bericht mit den Benutzern oder Rollen, die die Warnung ausgelöst haben, zusammen mit Ratschlägen zur Lösung anzuzeigen.

![Warnungsbericht](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Alerts
| Warnung | Severity | Trigger | Empfehlung |
| --- | --- | --- | --- |
| **Zu viele Besitzer zu Ressource zugewiesen** |Mittel |Zu viele Benutzer verfügen über die Rolle „Besitzer“. |Überprüfen Sie die Benutzer in der Liste, und weisen Sie einigen Benutzern Rollen mit weniger Rechten zu. |
| **Zu viele dauerhafte Besitzer zu Ressource zugewiesen** |Mittel |Zu viele Benutzer sind dauerhaft einer Rolle zugewiesen. |Überprüfen Sie die Benutzer in der Liste, und weisen Sie einige neu zu, damit für die Rollennutzung die Aktivierung erzwungen wird. |
| **Doppelte Rolle erstellt** |Mittel |Mehrere Rollen verfügen über die gleichen Kriterien. |Verwenden Sie nur eine dieser Rollen. |


### <a name="severity"></a>Severity
* **Hoch**: Aufgrund einer Richtlinienverletzung ist eine sofortige Aktion erforderlich. 
* **Mittel:** Es ist keine sofortige Aktion erforderlich, aber es wird eine potenzielle Richtlinienverletzung angezeigt.
* **Niedrig**: Es ist keine sofortige Aktion erforderlich, aber es wird eine bevorzugte Richtlinienänderung vorgeschlagen.

## <a name="configure-security-alert-settings"></a>Konfigurieren der Einstellungen für Sicherheitswarnungen
Navigieren Sie auf der Seite „Warnungen“ zu **Einstellungen**.
![Einstellungen](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Passen Sie die Einstellungen für die verschiedenen Warnungen so an, dass sie für Ihre Umgebung und Sicherheitsziele funktionieren.
![Anpassen von Einstellungen](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Sicherheitswarnungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-configure-alerts.md)

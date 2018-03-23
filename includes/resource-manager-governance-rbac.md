---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 24863e239620f84a57c631b3ecf5b08997de31c5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
Sie sollten sicherstellen, dass Benutzer in Ihrer Organisation einen geeigneten Zugriff auf diese Ressourcen haben. Den Benutzern sollte kein uneingeschränkter Zugriff erteilt werden, aber Sie müssen auch sicherstellen, dass sie ihre Arbeit erledigen können. Mit der rollenbasierten Zugriffssteuerung (Role-based Access Control, RBAC) können Sie verwalten, welche Benutzer die Berechtigung zum Ausführen bestimmter Aktionen in einem Bereich erhalten. Eine Rolle definiert einen Satz von zulässigen Aktionen. Sie weisen die Rolle einem Bereich zu und legen fest, welche Benutzer dieser Rolle für den Bereich angehören.

Bei der Planung Ihrer Strategie für die Zugriffssteuerung erteilen Sie Benutzern die geringsten Rechte zum Ausführen ihrer Aufgaben. Die folgende Abbildung zeigt ein vorgeschlagenes Muster für die Zuweisung von RBAC.

![Umfang](./media/resource-manager-governance-rbac/role-examples.png)

Drei Rollen gelten für alle Ressourcen: Besitzer, Mitwirkender und Leser. Alle Konten, die der Rolle „Besitzer“ zugewiesen sind, müssen streng kontrolliert und selten verwendet werden. Benutzern, die nur den Status von Lösungen beobachten müssen, sollte die Leserrolle gewährt werden.

Den meisten Benutzern werden [ressourcenspezifische](../articles/active-directory/role-based-access-built-in-roles.md) oder [benutzerdefinierte Rollen](../articles/active-directory/role-based-access-control-custom-roles.md) entweder auf Abonnement- oder Ressourcengruppenebene zugeteilt. Diese Rollen legen die zulässigen Aktionen präzise fest. Durch das Zuweisen von Benutzern an diese Rollen erteilen Sie Benutzern die erforderlichen Zugriffsrechte ohne zu viel Kontrolle zu gewähren. Sie können ein Konto mehreren Rollen zuweisen, und der entsprechende Benutzer erhält die kombinierten Berechtigungen der Rollen. Das Gewähren des Zugriffs auf Ressourcenebene ist für Benutzer zwar häufig zu restriktiv, kann jedoch für einen automatisierten Prozess funktionieren, der für eine bestimmten Aufgabe konzipiert wurde.

### <a name="who-can-assign-roles"></a>Wer kann Rollen zuweisen?

Um Rollenzuweisungen erstellen und entfernen zu können, benötigen Benutzer den Zugriff `Microsoft.Authorization/roleAssignments/*`. Dieser Zugriff wird über die Rolle „Besitzer“oder „Benutzerzugriffsadministrator“ gewährt.
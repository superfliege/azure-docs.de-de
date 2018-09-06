---
title: Anzeigen der Zuweisungen von Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Zeigen Sie in Azure AD Privileged Identity Management (PIM) an, welche Benutzer über Azure-Ressourcenrollen verfügen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: aee172bc6fc77aaac8d2d52037a481fdb976d308
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188967"
---
# <a name="view-who-has-azure-resource-roles-in-pim"></a>Anzeigen der Zuweisungen von Azure-Ressourcenrollen in PIM

Mit Azure Active Directory Privileged Identity Management (PIM) können Sie den Zugriff auf Azure-Ressourcen innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dies schließt Abonnements, Ressourcengruppen und sogar virtuelle Computer ein. Alle Ressourcen im Azure-Portal, die die Azure-Funktion für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) nutzen, können von den Azure AD PIM-Funktionen für Sicherheit und Lebenszyklusverwaltung profitieren. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>Unterstützung von Ressourcenadministratoren durch PIM für Azure-Ressourcen

- Sehen Sie, welchen Benutzern und Gruppen Rollen für die von Ihnen verwalteten Azure-Ressourcen zugewiesen sind.
- Aktivieren Sie bedarfsgesteuerten Just-In-Time-Zugriff zur Verwaltung von Ressourcen wie Abonnements und Ressourcengruppen
- Lassen Sie mithilfe neuer, zeitgebundener Zuweisungseinstellungen den Ressourcenzugriff zugewiesener Benutzer/Gruppen automatisch ablaufen.
- Weisen Sie temporären Ressourcenzugriff für kurze Aufgaben oder Bereitschaftszeitpläne zu.
- Erzwingen Sie die Verwendung der mehrstufigen Authentifizierung für den Ressourcenzugriff jeder beliebigen integrierten oder benutzerdefinierten Rolle. 
- Rufen Sie Berichte zu ressourcenzugriffsbezogenen Ressourcenaktivitäten während der aktiven Sitzung eines Benutzers ab.
- Empfangen von Benachrichtigungen, wenn neuen Benutzern oder Gruppen Ressourcenzugriff zugewiesen wird und sie geeignete Zuweisungen aktivieren

## <a name="view-activation-and-azure-resource-activity"></a>Anzeigen der Aktivierung und der Azure-Ressourcenaktivität

Falls Sie ermitteln möchten, welche Aktionen ein bestimmter Benutzer für verschiedene Ressourcen ausgeführt hat, können Sie die Azure-Ressourcenaktivität für einen bestimmten Aktivierungszeitraum prüfen (für geeignete Benutzer). Wählen Sie in der Mitgliederansicht zunächst einen Benutzer aus der Liste mit den Mitgliedern in einer bestimmten Rolle aus. Daraufhin erscheint eine grafische Darstellung der Aktionen (nach Datum), die der Benutzer für Azure-Ressourcen ausgeführt hat, und der kürzlich erfolgten Rollenaktivierungen im gleichen Zeitraum.

![](media/azure-pim-resource-rbac/user-details.png)

Wenn Sie auf eine bestimmte Rollenaktivierung klicken, werden Details zu der Rollenaktivierung sowie die entsprechende Azure-Ressourcenaktivität angezeigt, die erfolgt ist, während der Benutzer aktiv war.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="review-who-has-access-in-a-subscription"></a>Überprüfen, wer in einem Abonnement Zugriff hat

Rufen Sie zum Überprüfen der Rollenzuweisungen in Ihrem Abonnement über die linke Navigationsleiste die Registerkarte „Mitglieder“ auf, oder klicken Sie auf „Rollen“, und wählen Sie eine bestimmte Rolle aus, deren Mitglieder Sie überprüfen möchten. 

Klicken Sie auf der Aktionsleiste auf „Überprüfen“, um vorhandene Zugriffsüberprüfungen anzuzeigen, und klicken Sie anschließend auf „Hinzufügen“, um eine neue Überprüfung hinzuzufügen.

![](media/azure-pim-resource-rbac/owner.png)

Weitere Informationen zu Zugriffsüberprüfungen finden Sie [hier](pim-how-to-perform-security-review.md).

>[!NOTE]
Überprüfungen werden derzeit nur für Abonnementressourcentypen unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
- [Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-approval-workflow.md)
- [Integrierte Rollen in Azure](../../role-based-access-control/built-in-roles.md)

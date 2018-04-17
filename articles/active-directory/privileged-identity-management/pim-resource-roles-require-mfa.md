---
title: Privileged Identity Management für Azure-Ressourcen – MFA | Microsoft-Dokumentation
description: In diesem Dokument wird beschrieben, wie Sie die mehrstufige Authentifizierung für PIM-Ressourcen aktivieren.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8d1c05e7f61ed76c47613bfab7bb8afd9b66cbe7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---mfa"></a>Privileged Identity Management – Ressourcenrollen – MFA

Mit PIM für Azure-Ressourcenrollen können Ressourcenadministratoren und Identitätsadministratoren wichtige Azure-Infrastruktur mit zeitgebundenen Mitgliedschaften und Just-in-Time-Zugriff schützen. Außerdem ist in PIM die optionale Erzwingung der Azure Multi-Factor Authentication (MFA) für zwei bestimmte Szenarien möglich.

## <a name="require-mfa-to-activate"></a>Erzwingen von MFA für die Aktivierung

Ressourcenadministratoren können erzwingen, dass berechtigte Mitglieder einer Rolle zuerst erfolgreich einen Azure-MFA-Vorgang absolvieren müssen, bevor sie aktiviert werden können. Mit diesem Prozess wird sichergestellt, dass es sich bei dem Benutzer, der die Aktivierung anfordert, auch wirklich um den angegebenen Benutzer handelt. Durch die Erzwingung dieser Option werden wichtige Ressourcen in Situationen geschützt, in denen das Benutzerkonto unter Umständen kompromittiert wurde. 

Wählen Sie zum Erzwingen dieser Anforderung in der Liste mit den verwalteten Ressourcen eine Ressource aus. Wählen Sie im [Übersichtsdashboard](pim-resource-roles-overview-dashboards.md) unten rechts auf dem Bildschirm in der Liste mit den Rollen eine Rolle aus.

Außerdem können Sie auf Rolleneinstellungen im linken Navigationsmenü entweder über die Registerkarte „Rollen“ oder „Rolleneinstellungen“ zugreifen.

>[!Note]
>Wenn die Optionen im linken Navigationsmenü ausgegraut sind und oben auf der Seite ein Banner mit einem Hinweis der Art „Sie verfügen über berechtigte Rollen, die aktiviert werden können“ angezeigt wird, sind Sie kein aktiver Administrator und müssen vor dem Fortfahren die [Aktivierung](pim-resource-roles-activate-your-roles.md) durchführen.

![](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Wählen Sie beim Anzeigen der Mitgliedschaft einer Rolle in der Leiste am oberen Rand des Bildschirms die Option „Rolleneinstellungen“, um die Option „Details zur Rolleneinstellung“ zu öffnen.

Klicken Sie oben auf die Schaltfläche **Bearbeiten**, um die Rolleneinstellungen zu ändern.

Klicken Sie im Abschnitt unter **Aktivieren** auf das Kontrollkästchen **Require Multi-Factor Authentication to activate** (Multi-Factor Authentication für Aktivierung erzwingen) und dann auf „Speichern“.

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-mfa-on-assignment"></a>Erzwingen von MFA bei der Zuweisung

Es kann vorkommen, dass ein Ressourcenadministrator ein Mitglied einer Rolle für einen kurzen Zeitraum (z.B. einen Tag) zuweisen möchte, ohne dass die zugewiesenen Mitglieder die Aktivierung anfordern müssen. In diesem Szenario kann MFA von PIM nicht erzwungen werden, wenn das Mitglied seine Rollenzuweisung verwendet, da die Rolle ab dem Moment der Zuweisung bereits aktiv ist.

Um sicherzustellen, dass der Administrator, der die Zuweisung bearbeitet, auch wirklich die angegebene Person ist, können Sie für die Zuweisung MFA erzwingen.

Aktivieren Sie auf dem Bildschirm „Details zur Rolleneinstellung“ das Kontrollkästchen „Require Multi-Factor Authentication on assignment“ (Multi-Factor Authentication bei Zuweisung erzwingen).

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Nächste Schritte

[Erzwingen einer Genehmigung für die Aktivierung](pim-resource-roles-approval-workflow.md)

[Verwenden des Überwachungsprotokolls](pim-resource-roles-use-the-audit-log.md)




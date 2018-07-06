---
title: Erzwingen der Azure Multi-Factor Authentication in Azure-Ressourcen anhand von Privileged Identity Management | Microsoft-Dokumentation
description: In diesem Dokument wird beschrieben, wie Sie die mehrstufige Authentifizierung für PIM-Ressourcen aktivieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 21f6fef214f27630ff0eadc39e1e26c9c344f353
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444731"
---
# <a name="enforce-azure-multi-factor-authentication-in-azure-resources-by-using-privileged-identity-management"></a>Erzwingen der Azure Multi-Factor Authentication in Azure-Ressourcen anhand von Privileged Identity Management

Mit Privileged Identity Management (PIM) für Azure-Ressourcenrollen können Ressourcenadministratoren und Identitätsadministratoren wichtige Azure-Infrastruktur mit zeitgebundenen Mitgliedschaften und Just-in-Time-Zugriff schützen. Außerdem ist in PIM die optionale Erzwingung der Azure Multi-Factor Authentication (MFA) für zwei bestimmte Szenarien möglich.

## <a name="require-multi-factor-authentication-to-activate"></a>Erfordern von Multi-Factor Authentication zur Aktivierung

Ressourcenadministratoren können erfordern, dass berechtigte Mitglieder einer Rolle zuerst einen Azure-Multi-Factor Authentication-Vorgang ausführen müssen, bevor sie aktiviert werden können. Mit diesem Prozess wird sichergestellt, dass es sich bei dem Benutzer, der die Aktivierung anfordert, auch wirklich um den angegebenen Benutzer handelt. Durch die Erzwingung dieser Option werden wichtige Ressourcen in Situationen geschützt, in denen das Benutzerkonto unter Umständen kompromittiert wurde. 

Wählen Sie zum Erzwingen dieser Anforderung in der Liste mit den verwalteten Ressourcen eine Ressource aus. Wählen Sie im [Übersichtsdashboard](pim-resource-roles-overview-dashboards.md) unten rechts auf dem Bildschirm in der Liste mit den Rollen eine Rolle aus.

Außerdem können Sie auf Rolleneinstellungen im linken Bereich entweder über die Registerkarte **Rollen** oder **Rolleneinstellungen** zugreifen.

>[!Note]
>Wenn die Optionen im linken Bereich ausgegraut sind und oben auf der Seite ein Banner mit einem Hinweis der Art „Sie verfügen über berechtigte Rollen, die aktiviert werden können“ angezeigt wird, sind Sie kein aktiver Administrator. Das bedeutet, dass Sie vor dem Fortfahren die [Aktivierung](pim-resource-roles-activate-your-roles.md) durchführen müssen.

![Registerkarten „Rollen“ und „Rolleneinstellungen“ ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Wählen Sie beim Anzeigen der Mitgliedschaft einer Rolle in der Leiste am oberen Rand des Bildschirms die Option **Rolleneinstellungen**, um die Option **Details zur Rolleneinstellung** zu öffnen.

Klicken Sie oben auf die Schaltfläche **Bearbeiten**, um die Rolleneinstellungen zu bearbeiten.

Klicken Sie im Abschnitt unter **Aktivieren** auf das Kontrollkästchen **Bei Aktivierung Multi-Factor Authentication anfordern**. Klicken Sie dann auf **Speichern**.

![Bei Aktivierung Multi-Factor Authentication anfordern](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Bei Zuweisung Multi-Factor Authentication anfordern

Es kann vorkommen, dass ein Ressourcenadministrator ein Mitglied einer Rolle für einen kurzen Zeitraum (z.B. einen Tag) zuweisen möchte. In diesem Fall müssen die zugewiesenen Mitglieder die Aktivierung nicht anfordern. In diesem Szenario kann Multi-Factor Authentication von PIM nicht erzwungen werden, wenn das Mitglied seine Rollenzuweisung verwendet, da die Rolle ab dem Moment der Zuweisung bereits aktiv ist.

Um sicherzustellen, dass der Ressourcenadministrator, der die Zuweisung bearbeitet, auch wirklich die angegebene Person ist, können Sie für die Zuweisung Multi-Factor Authentication erzwingen.

Aktivieren Sie auf dem Bildschirm „Details zur Rolleneinstellung“ das Kontrollkästchen **Multi-Factor Authentication bei direkter Zuweisung erforderlich**.

![Multi-Factor Authentication bei direkter Zuweisung erforderlich](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Nächste Schritte

[Erzwingen einer Genehmigung für die Aktivierung](pim-resource-roles-approval-workflow.md)

[Verwenden des Überwachungsprotokolls](pim-resource-roles-use-the-audit-log.md)




---
title: Privileged Identity Management für Azure-Ressourcen – Verwenden von benutzerdefinierten Rollen für PIM-Einstellungen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie benutzerdefinierte Rollen in PIM für Azure-Ressourcen verwenden.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 6336d99df1bbdd71c66a9757af1d9fb356a91bf6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="use-custom-roles-to-target-pim-settings"></a>Verwenden von benutzerdefinierten Rollen für PIM-Einstellungen

Es kann erforderlich sein, strikte PIM-Einstellungen auf einige Mitglieder einer Rolle anzuwenden, während anderen mehr Freiheiten gewährt werden. Stellen Sie sich ein Szenario vor, bei dem eine Organisation mehrere Vertragspartner nutzt, um Unterstützung bei der Entwicklung einer Anwendung zu erhalten, die in einem Azure-Abonnement ausgeführt wird. 

Als Ressourcenadministrator möchten Sie, dass die Mitarbeiter Ihrer Organisation über eine Zugriffsberechtigung ohne erforderliche Genehmigung verfügen, während alle Vertragspartner beim Anfordern der Aktivierung zuerst eine Genehmigung erhalten müssen. Führen Sie die unten angegebenen Schritte aus, um PIM-Zieleinstellungen für Azure-Ressourcenrollen zu aktivieren.

## <a name="create-the-custom-role"></a>Erstellen der benutzerdefinierten Rolle

[Verwenden Sie diesen Leitfaden, um für eine Ressource eine benutzerdefinierte Rolle zu erstellen](../../role-based-access-control/custom-roles.md).

Fügen Sie einen beschreibenden Namen hinzu, damit Sie sich leicht merken können, welche integrierte Rolle dupliziert werden soll.

>[!NOTE]
>Stellen Sie sicher, dass die benutzerdefinierte Rolle ein Duplikat der gewünschten Rolle ist und dass der Bereich der integrierten Rolle entspricht.

## <a name="apply-pim-settings"></a>Anwenden von PIM-Einstellungen

Nachdem die Rolle in Ihrem Mandanten erstellt wurde, können Sie zu PIM navigieren und die Ressource auswählen, für die der Bereich der Rolle festgelegt ist.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[Konfigurieren Sie PIM-Rolleneinstellungen](pim-resource-roles-configure-role-settings.md), die für diese Mitglieder gelten sollen.

Führen Sie abschließend das [Zuweisen von Rollen](pim-resource-roles-assign-roles.md) für einzelne Gruppen mit Mitgliedern durch, für die diese Einstellungen gelten sollen.

## <a name="next-steps"></a>Nächste Schritte

[Überprüfen der Besitzer des Abonnements](pim-resource-roles-perform-access-review.md)

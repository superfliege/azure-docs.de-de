---
title: Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anforderungen für Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) genehmigen oder ablehnen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e5cda31af5ac95e7ebe2b858b1d7355ea3f2a6bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189803"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM

Sie müssen ein Mitglied der Liste mit den genehmigenden Personen sein, um eine Anforderung genehmigen oder ablehnen zu können. 

1. Wählen Sie in PIM im linken Menü auf der Registerkarte die Option **Anforderungen genehmigen** aus, und suchen Sie nach der Anforderung.

   ![Bereich „Anforderungen genehmigen“](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Wählen Sie die Anforderung aus, geben Sie eine Begründung für die Entscheidung an, und wählen Sie **Genehmigen** oder **Ablehnen** aus. Die Anforderung wird dann abgeschlossen.

   ![Ausgewählte Anforderung mit detaillierten Informationen](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Workflowbenachrichtigungen

Einige Fakten zu Workflowbenachrichtigungen:

- Alle Mitglieder der Liste mit den genehmigenden Personen werden per E-Mail benachrichtigt, wenn für die Anforderung einer Rolle die Überprüfung aussteht. E-Mail-Benachrichtigungen enthalten einen direkten Link zur Anforderung, damit diese von der genehmigenden Person genehmigt oder abgelehnt werden kann.
- Anforderungen werden von dem Mitglied bearbeitet, das in der Liste der genehmigenden Personen an erster Stelle steht. 
- Wenn eine genehmigende Person auf die Anforderung reagiert, werden alle Mitglieder der Liste über die Aktion informiert. 
- Ressourcenadministratoren werden benachrichtigt, wenn ein genehmigtes Mitglied in seiner Rolle aktiv wird. 

>[!Note]
>Wenn ein Ressourcenadministrator der Meinung ist, dass ein genehmigtes Mitglied nicht aktiv sein sollte, kann er die aktive Rollenzuweisung in PIM entfernen. Ressourcenadministratoren werden zwar nur über ausstehende Anforderungen benachrichtigt, wenn sie Mitglieder der Liste mit den genehmigenden Personen sind, aber sie können ausstehende Anforderungen aller Benutzer anzeigen und abbrechen, indem sie diese in PIM aufrufen. 

## <a name="next-steps"></a>Nächste Schritte

- [Genehmigen oder Ablehnen von Anforderungen für Azure AD-Verzeichnisrollen in PIM](azure-ad-pim-approval-workflow.md)
- [E-Mail-Benachrichtigungen in PIM](pim-email-notifications.md)

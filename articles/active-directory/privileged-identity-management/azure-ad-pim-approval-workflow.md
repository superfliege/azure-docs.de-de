---
title: Genehmigen oder Ablehnen von Anforderungen für Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anforderungen für Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) genehmigen oder ablehnen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7bf1e437e97fdb4d929af23bd7b2a9abb49268df
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189157"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Genehmigen oder Ablehnen von Anforderungen für Azure AD-Verzeichnisrollen in PIM

Mit Privileged Identity Management können Sie Rollen konfigurieren, um die Genehmigung für die Aktivierung anzufordern, und einen oder mehrere Benutzer oder Gruppen als delegierte genehmigende Personen auswählen.

## <a name="view-pending-approvals-requests"></a>Anzeigen ausstehender Genehmigungen (Anforderungen)

Als delegierte genehmigende Person erhalten Sie E-Mail-Benachrichtigungen, wenn Ihre Genehmigung einer Anforderung aussteht. Wählen Sie zum Anzeigen dieser Anforderungen im PIM-Portal im Dashboard (in der neuen Navigation) in der linken Navigationsleiste die Registerkarte „Ausstehende Genehmigungsanforderungen“.

![](media/azure-ad-pim-approval-workflow/image023.png)

Dort sehen Sie eine Liste der Anforderungen mit ausstehender Genehmigung:

![](media/azure-ad-pim-approval-workflow/image024.png)

## <a name="approve-or-deny-requests-for-role-elevation-single-andor-bulk"></a>Anforderungen von Rechteerweiterungen für Rollen (einzeln und/oder mehrere) genehmigen oder ablehnen

Wählen Sie die Anforderungen aus, die Sie genehmigen oder ablehnen möchten, und klicken Sie auf die entsprechende Schaltfläche in der Aktionsleiste:

![](media/azure-ad-pim-approval-workflow/image025.png)

## <a name="provide-justification-for-my-approvaldenial"></a>Eine Begründung für die Genehmigung/Ablehnung abgeben

Dadurch wird ein neues Blatt zum gleichzeitigen Genehmigen oder Ablehnen mehrerer Anforderungen geöffnet. Geben Sie eine Begründung für Ihre Entscheidung ein, und klicken Sie am unteren Rand des Blatts auf „Genehmigen“ (oder „Ablehnen“):

![](media/azure-ad-pim-approval-workflow/image029.png)

Nach Abschluss des Anforderungsprozesses spiegelt das Statussymbol die Entscheidung wider, die Sie getroffen haben (in diesem Beispiel die Genehmigung):

![](media/azure-ad-pim-approval-workflow/image031.png)

## <a name="next-steps"></a>Nächste Schritte

- [Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-approval-workflow.md)
- [E-Mail-Benachrichtigungen in PIM](pim-email-notifications.md)

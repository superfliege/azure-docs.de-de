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
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9402824540f965cb89aa00791d093bd87712a89a
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665841"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Genehmigen oder Ablehnen von Anforderungen für Azure AD-Verzeichnisrollen in PIM

Mit Azure AD Privileged Identity Management (PIM) können Sie Rollen konfigurieren, um die Genehmigung für die Aktivierung anzufordern, und einen oder mehrere Benutzer oder Gruppen als delegierte genehmigende Personen auswählen. Führen Sie die in diesem Artikel beschriebenen Schritte aus, um Anforderungen für Azure AD-Verzeichnisrollen zu genehmigen oder abzulehnen.

## <a name="view-pending-requests"></a>Anzeigen ausstehender Anforderungen

Als delegierte genehmigende Person erhalten Sie eine E-Mail-Benachrichtigung, wenn Ihre Genehmigung einer Anforderung für eine Azure AD-Verzeichnisrolle aussteht. Sie können diese ausstehenden Anforderungen in PIM anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Anforderungen genehmigen**.

    ![Azure AD-Verzeichnisrollen in PIM – Rollen](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Es wird eine Liste der Anforderungen angezeigt, die Sie noch genehmigen müssen.

## <a name="approve-requests"></a>Genehmigen von Anforderungen

1. Wählen Sie die Anforderungen aus, die Sie genehmigen möchten, und klicken Sie dann auf **Genehmigen**, um den Bereich „Ausgewählte Anforderungen genehmigen“ zu öffnen.

    ![Liste im Bereich „Anforderungen genehmigen“ in PIM](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. Geben Sie im Feld **Genehmigungsgrund** einen Grund ein.

    ![„Ausgewählte Anforderungen genehmigen“ in PIM](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Klicken Sie auf **Genehmigen**.

    Das Statussymbol wird mit Ihrer Genehmigung aktualisiert.

    ![„Ausgewählte Anforderungen genehmigen“ in PIM](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Ablehnen von Anforderungen

1. Wählen Sie die Anforderungen aus, die Sie ablehnen möchten, und klicken Sie dann auf **Ablehnen**, um den Bereich „Ausgewählte Anforderungen ablehnen“ zu öffnen.

    ![Liste im Bereich „Anforderungen genehmigen“ in PIM](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. Geben Sie im Feld **Ablehnungsgrund** einen Grund ein.

    ![„Ausgewählte Anforderungen ablehnen“ in PIM](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Klicken Sie auf **Ablehnen**.

    Das Statussymbol wird mit Ihrer Ablehnung aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

- [E-Mail-Benachrichtigungen in PIM](pim-email-notifications.md)
- [Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-approval-workflow.md)

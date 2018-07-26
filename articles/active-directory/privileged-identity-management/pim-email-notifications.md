---
title: E-Mail-Benachrichtigungen in Azure AD PIM | Microsoft-Dokumentation
description: Beschreibt E-Mail-Benachrichtigungen in Azure AD Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 07/14/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 6c329554b5854f113fb216f874fa5a918110f9c5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39058092"
---
# <a name="email-notifications-in-azure-ad-pim"></a>E-Mail-Benachrichtigungen in Azure AD PIM

Wenn in Azure AD Privileged Identity Management (PIM) wichtige Ereignisse auftreten, werden E-Mail-Benachrichtigungen an den entsprechenden Administrator oder Benutzer gesendet. PIM sendet E-Mail-Benachrichtigungen z.B. bei folgenden Ereignissen:

- Wenn die Genehmigung für die Aktivierung einer privilegierten Rolle aussteht
- Wenn die Aktivierung einer privilegierten Rolle genehmigt wird
- Wenn eine privilegierte Rolle aktiviert wird
- Wenn eine privilegierte Rolle zugewiesen wird
- Wenn Azure AD PIM aktiviert wird

Ab Ende Juli 2018 weisen über PIM versendete E-Mail-Benachrichtigungen eine neue Absender-E-Mail-Adresse und ein neues Layout auf. Dieses Update wirkt sich sowohl auf PIM für Azure AD als auch auf PIM für Azure-Ressourcen aus. Bei allen Ereignissen, die zuvor eine E-Mail-Benachrichtigung ausgelöst haben, wird auch weiterhin eine gesendet. Dieses Update enthält nur optische Änderungen, keine funktionellen.

## <a name="sender-email-address"></a>E-Mail-Adresse des Absenders

Ab Ende Juli 2018 weisen E-Mail-Benachrichtigungen die folgende Adresse auf:

- E-Mail-Adresse: **azure-noreply@microsoft.com**
- Anzeigename: Microsoft Azure

Zuvor wurden E-Mail-Benachrichtigungen über die folgende Adresse versendet:

- E-Mail-Adresse: **azureadnotifications@microsoft.com**
- Anzeigename: Microsoft Azure AD-Benachrichtigungsdienst

## <a name="pim-emails-for-azure-ad-roles"></a>PIM-E-Mails für Azure AD-Rollen

Ab Ende Juli 2018 weisen die PIM-E-Mail-Benachrichtigungen für Azure AD-Rollen ein aktualisiertes Layout auf. Nachstehend finden Sie eine Beispiel-E-Mail, die gesendet wird, wenn ein Benutzer eine privilegierte Rolle für die fiktive Organisation Contoso aktiviert.

![Neue PIM-E-Mail für Azure AD-Rollen](./media/pim-email-notifications/email-directory-new.png)

Wenn ein Benutzer früher eine privilegierte Rolle aktivierte, erhielt er eine Nachricht wie die folgende.

![Alte PIM-E-Mail für Azure AD-Rollen](./media/pim-email-notifications/email-directory-old.png)

## <a name="pim-emails-for-azure-resource-roles"></a>PIM-E-Mails für Azure-Ressourcenrollen

Ab Ende Juli 2018 weisen die PIM-E-Mail-Benachrichtigungen für Azure-Ressourcenrollen ein aktualisiertes Layout auf. Nachstehend finden Sie eine Beispiel-E-Mail, die gesendet wird, wenn einem Benutzer eine privilegierte Rolle für die fiktive Organisation Contoso zugewiesen wird.

![Neue PIM-E-Mails für Azure-Ressourcenrollen](./media/pim-email-notifications/email-resources-new.png)

Wenn einem Benutzer früher eine privilegierte Rolle zugewiesen wurde, erhielt er eine Nachricht wie die folgende.

![Alte PIM-E-Mail für Azure-Ressourcenrollen](./media/pim-email-notifications/email-resources-old.png)

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten der Einstellungen für die Rollenaktivierung in Azure AD Privileged Identity Management](pim-how-to-change-default-settings.md)
- [Genehmigungen](azure-ad-pim-approval-workflow.md)

---
title: E-Mail-Benachrichtigungen in PIM – Azure | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Beschreibung der E-Mail-Benachrichtigungen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 09/07/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: de1d29d3ab1b370257c3a2d6b6ff9f677197fc2a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303063"
---
# <a name="email-notifications-in-pim"></a>E-Mail-Benachrichtigungen in PIM

Wenn in Azure AD Privileged Identity Management (PIM) wichtige Ereignisse auftreten, werden E-Mail-Benachrichtigungen gesendet. PIM sendet E-Mail-Benachrichtigungen z.B. bei folgenden Ereignissen:

- Wenn die Genehmigung für die Aktivierung einer privilegierten Rolle aussteht
- Wenn die Aktivierungsanforderung einer privilegierten Rolle abgeschlossen wird
- Wenn eine privilegierte Rolle aktiviert wird
- Wenn eine privilegierte Rolle zugewiesen wird
- Wenn Azure AD PIM aktiviert wird

E-Mail-Benachrichtigungen werden an die folgenden Administratoren gesendet:

- Administrator für privilegierte Rollen
- Sicherheitsadministrator

E-Mail-Benachrichtigungen werden auch an den Endbenutzer gesendet, der die privilegierte Rolle für die folgenden Ereignisse hat:

- Wenn die Aktivierungsanforderung einer privilegierten Rolle abgeschlossen wird
- Wenn eine privilegierte Rolle zugewiesen wird

Ab Ende Juli 2018 weisen über PIM versendete E-Mail-Benachrichtigungen eine neue Absender-E-Mail-Adresse und ein neues Layout auf. Dieses Update wirkt sich sowohl auf PIM für Azure AD als auch auf PIM für Azure-Ressourcen aus. Bei allen Ereignissen, die zuvor eine E-Mail-Benachrichtigung ausgelöst haben, wird auch weiterhin eine gesendet. Einige E-Mails werden über aktualisierten Inhalt verfügen, der gezieltere Informationen bietet.

## <a name="sender-email-address"></a>E-Mail-Adresse des Absenders

Ab Ende Juli 2018 weisen E-Mail-Benachrichtigungen die folgende Adresse auf:

- E-Mail-Adresse: **azure-noreply@microsoft.com**
- Anzeigename: Microsoft Azure

Zuvor wurden E-Mail-Benachrichtigungen über die folgende Adresse versendet:

- E-Mail-Adresse: **azureadnotifications@microsoft.com**
- Anzeigename: Microsoft Azure AD-Benachrichtigungsdienst

## <a name="email-subject-line"></a>E-Mail-Betreffzeile

Ab Ende Juli 2018 werden E-Mail-Benachrichtigungen für Azure AD- und Azure-Ressourcenrollen in der Betreffzeile ein **PIM**-Präfix aufweisen. Hier sehen Sie ein Beispiel:

- PIM: Alain Charon wurde permanent die Rolle des Benutzers mit Leseberechtigung für Sicherungsfunktionen zugewiesen.

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

- [Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-change-default-settings.md)
- [Genehmigen oder Ablehnen von Anforderungen für Azure AD-Verzeichnisrollen in PIM](azure-ad-pim-approval-workflow.md)

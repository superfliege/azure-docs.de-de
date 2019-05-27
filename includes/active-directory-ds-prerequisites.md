---
title: Includedatei
description: Includedatei mit erforderlichen Komponenten
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 66b5f8e2-e08d-43c8-b460-6204aecda8f7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.custom: include file
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: 1fba8cc9ae40cf5539016bbd73de65f557a64136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66111384"
---
> [!IMPORTANT]
> **Aktivieren Sie die Kennworthashsynchronisierung mit Azure AD Domain Services, bevor Sie die Aufgaben in diesem Artikel ausführen.**
>
> Befolgen Sie die Anweisungen unten gemäß des Typs der Benutzer in Ihrem Azure AD-Verzeichnis. Führen Sie beide Gruppen von Anweisungen aus, wenn in Ihrem Azure AD-Verzeichnis eine Mischung aus reinen Cloudbenutzerkonten und synchronisierten Benutzerkonten vorliegt. Sie können die folgenden Vorgänge möglicherweise nicht ausführen, falls Sie versuchen, ein B2B-Gastkonto zu verwenden (z. B. Ihr Gmail-Konto oder MSA von einem anderen Identitätsanbieter, was wir zulassen), weil wird das Kennwort dieser Benutzer nicht besitzen, die mit der verwalteten Domäne synchronisiert werden, weil es sich dabei um Gastkonten im Verzeichnis handelt. Die vollständige Informationen zu diesen Konten, einschließlich ihrer Kennwörter, befänden sich außerhalb von Azure AD, und da sich diese Informationen nicht in Azure AD befinden, werden sie noch nicht einmal in die verwaltete Domäne synchronisiert. 
> - [Anweisungen für reine Cloudbenutzerkonten](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
> - [Anweisungen für synchronisierte Benutzerkonten aus einem lokalen Verzeichnis](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)

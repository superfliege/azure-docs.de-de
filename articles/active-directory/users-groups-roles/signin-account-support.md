---
title: Ermitteln, ob auf einer Azure AD-Anmeldeseite Microsoft-Konten akzeptiert werden | Microsoft-Dokumentation
description: Informationen dazu, wie Meldungen auf dem Bildschirm das Nachschlagen des Benutzernamens bei der Anmeldung widerspiegeln
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a62c4d56fbfca34ff6291863149b078f7ddc6680
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59287573"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Anmeldeoptionen für Microsoft-Konten in Azure Active Directory

Die Anmeldeseite von Microsoft 365 für Azure Active Directory (Azure AD) unterstützt je nach Fall Geschäfts-, Schul- oder Unikonten und/oder Microsoft-Konten für Folgendes:

* Apps, bei denen die Anmeldung über beide Kontotypen möglich ist
* Organisationen, die Gäste akzeptieren

## <a name="identification"></a>Identifikation
Sie können feststellen, ob die Anmeldeseite Ihrer Organisation Microsoft-Konten unterstützt, indem Sie sich den Hinweistext im Feld für den Benutzernamen ansehen. Wenn im Hinweistext „E-Mail, Telefon oder Skype“ angezeigt wird, unterstützt die Anmeldeseite Microsoft-Konten.

![Unterschied zwischen Konten für Anmeldeseiten](./media/signin-account-support/ui-prompt.png)

[Zusätzliche Anmeldeoptionen können nur für persönliche Microsoft-Konten](index.yml), jedoch nicht für die Anmeldung bei Geschäfts-, Schul- oder Unikonten verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Brandings Ihrer Anmeldung](../fundamentals/add-custom-domain.md)
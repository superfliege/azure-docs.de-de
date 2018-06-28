---
title: Bedrohungsverwaltung in Azure Active Directory B2C | Microsoft-Dokumentation
description: Informieren Sie sich über Verfahren zur Erkennung und Abwehr von Denial-of-Service- und Kennwortangriffen in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/27/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0a439a2bb485829b863d6aa666938ab58c90c61f
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750776"
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Bedrohungsverwaltung

Die Bedrohungsverwaltung umfasst die Planung des Schutzes vor Angriffen auf Ihr System und Ihre Netzwerke. Denial-of-Service-Angriffe können dazu führen, dass Ressourcen nicht mehr für die vorgesehenen Benutzer zur Verfügung stehen. Kennwortangriffe führen zu nicht autorisiertem Zugriff auf Ressourcen. Azure Active Directory B2C (Azure AD B2C) verfügt über integrierte Features, die Sie auf verschiedene Weise dabei unterstützen, Ihre Daten vor diesen Bedrohungen zu schützen.

## <a name="denial-of-service-attacks"></a>Denial-of-Service-Angriffe

Azure AD B2C verwendet Erkennungs- und Abwehrmaßnahmen wie SYN-Cookies und Raten-/Verbindungsbeschränkungen, um die zugrunde liegenden Ressourcen vor Denial-of-Service-Angriffen zu schützen.

## <a name="password-attacks"></a>Kennwortangriffe

Azure AD B2C umfasst auch Abwehrmaßnahmen für Kennwortangriffe. Diese decken unter anderem Brute-Force-Kennwortangriffe und Wörterbuchangriffe ab. Benutzerkennwörter müssen ausreichend komplex sein. Anhand verschiedener Signale analysiert Azure AD B2C die Integrität von Anforderungen. Azure AD B2C kann auf intelligente Weise zwischen vorgesehenen Benutzern und Hackern oder Botnetzen unterscheiden. Azure AD B2C bietet eine ausgeklügelte Strategie, um Konten auf der Grundlage der eingegebenen Kennwörter und der Wahrscheinlichkeit eines Angriffs zu sperren.

Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/default.aspx).

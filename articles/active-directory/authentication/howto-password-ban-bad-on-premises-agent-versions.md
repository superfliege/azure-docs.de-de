---
title: Verlauf der Versionsveröffentlichungen des lokalen Azure AD-Kennwortschutz-Agents
description: Dokumentiert den Verlauf der Versionsveröffentlichungen und Verhaltensänderungen.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 11/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 89d64a28d2fe43464995e434c9f3807047b29492
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913635"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Vorschau: Versionsverlauf des lokalen Azure AD-Kennwortschutz-Agents

|     |
| --- |
| Azure AD-Kennwortschutz ist eine öffentliche Vorschaufunktion für Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="12250"></a>1.2.25.0

Veröffentlichungsdatum: 01.11.2018

Fehlerbehebungen:

* DC-Agent- und Proxydienst sollten nicht mehr aufgrund von Zertifikatvertrauensfehlern fehlschlagen.
* DC-Agent- und Proxydienst weisen zusätzliche Fixes für FIPS-konforme Computer auf.
* Der Proxydienst funktioniert jetzt in einer reinen TLS 1.2-Netzwerkumgebung ordnungsgemäß.
* Kleinere Fehlerbehebungen bei Leistung und Stabilität.
* Verbesserte Protokollierung.

Änderungen:

* Die mindestens erforderliche Betriebssystemebene für den Proxydienst ist jetzt Windows Server 2012 R2. Die mindestens erforderliche Betriebssystemebene für den DC-Agent-Dienst bleibt Windows Server 2012.
* Der Algorithmus für die Kennwortüberprüfung verwendet eine erweiterte Zeichennormalisierungstabelle. Dies kann dazu führen, dass Kennwörter abgelehnt werden, die in früheren Versionen akzeptiert wurden.

## <a name="12100"></a>1.2.10.0

Herausgabedatum: 17.8.2018

Fehlerbehebungen:

* Register-AzureADPasswordProtectionProxy und Register-AzureADPasswordProtectionForest unterstützen jetzt mehrstufige Authentifizierung.
* Register-AzureADPasswordProtectionProxy erfordert eine WS2012-Domänencontroller oder höher in der Domäne, um Verschlüsselungsfehler zu vermeiden.
* Der DC-Agent-Dienst ist zuverlässiger beim Anfordern einer neuen Kennwortrichtlinie von Azure beim Starten.
* Der DC-Agent-Dienst fordert jede Stunde eine neue Kennwortrichtlinie von Azure an, falls erforderlich, führt dies aber jetzt zu einer zufällig ausgewählten Startzeit aus.
* Der DC-Agent-Dienst verursacht keine unbestimmte Verzögerung mehr bei einer neuen DC-Ankündigung, wenn er vor seiner Höherstufung als Replikat auf einem Server installiert wurde.
* Der DC-Agent-Dienst respektiert jetzt die Konfigurationseinstellung „Kennwortschutz für Windows Server Active Directory aktivieren“.
* Beide DC-Agent- und Proxy-Installationsprogramme unterstützen jetzt ein direktes Upgrade beim Upgrade auf zukünftige Versionen.

> [!WARNING]
> Ein direktes Upgrade von Version 1.1.10.3 wird nicht unterstützt und führt zu einem Installationsfehler. Für ein Upgrade auf Version 1.2.10 oder höher müssen Sie zuerst die DC-Agent- und Proxydienst-Software vollständig deinstallieren und dann die neue Version von Grund auf neu installieren. Eine erneute Registrierung des Azure AD-Kennwortschutz-Proxydiensts ist erforderlich.  Es ist nicht erforderlich, die Gesamtstruktur erneut zu registrieren.

> [!NOTE]
> Direkte Upgrades der DC-Agent-Software machen einen Neustart erforderlich.

* Der DC-Agent und Proxy-Dienst unterstützen nun die Ausführung auf einem Server, der für die ausschließliche Verwendung FIPS-konformer Algorithmen konfiguriert ist.
* Kleinere Fehlerbehebungen bei Leistung und Stabilität.
* Verbesserte Protokollierung.

## <a name="11103"></a>1.1.10.3

Herausgabedatum: 15.6.2018

Erste öffentliche Vorschauversion

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises-deploy.md)

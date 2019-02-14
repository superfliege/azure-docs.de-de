---
title: Häufig gestellte Fragen zum lokalen Azure AD-Kennwortschutz
description: Häufig gestellte Fragen zum lokalen Azure AD-Kennwortschutz
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a4528cde92c5f738fa3fa7f4a649d84b1e79431
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175947"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Vorschau: Lokaler Azure AD-Kennwortschutz – häufig gestellte Fragen

|     |
| --- |
| Azure AD-Kennwortschutz ist eine öffentliche Vorschaufunktion für Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="general-questions"></a>Allgemeine Fragen

**F: Wann erreicht der Azure AD-Kennwortschutz die allgemeine Verfügbarkeit (General Availability, GA)?**

Die allgemeine Verfügbarkeit ist für das 1. Quartal 2019 (vor Ende März 2019) geplant. Vielen Dank an alle Benutzer, die uns bisher Feedback zu dieser Funktion haben zukommen lassen. Wir wissen das zu schätzen!

**F: Wird der lokale Azure AD-Kennwortschutz in nicht öffentlichen Clouds unterstützt?**

Nein. Der lokale Azure AD-Kennwortschutz wird nur in der öffentlichen Cloud unterstützt.

**F: Wie kann ich die Vorteile des Azure AD-Kennwortschutzes auf eine Untergruppe meiner lokalen Benutzer anwenden?**

Nicht unterstützt. Sobald der Azure AD-Kennwortschutz einmal bereitgestellt und aktiviert ist, trifft er keine Unterscheidungen mehr – alle Benutzer genießen dieselben Sicherheitsvorteile.

**F: Wird die Parallelinstallation des Azure AD-Kennwortschutzes mit anderen kennwortfilterbasierten Produkten unterstützt?**

Ja. Unterstützung für mehrere registrierte Kennwortfilter-DLLs ist ein Hauptmerkmal von Windows und nicht spezifisch für den Azure AD-Kennwortschutz. Alle registrierten Kennwortfilter-DLLs müssen zustimmen, bevor ein Kennwort akzeptiert wird.

**F: Warum ist DFSR für die sysvol-Replikation erforderlich?**

FRS (die Vorgängertechnologie zu DFSR) verfügt über viele bekannte Probleme und die Unterstützung wird in neueren Versionen von Windows Server Active Directory vollständig eingestellt. Zero-Tests des Azure AD-Kennwortschutzes erfolgen in FRS-konfigurierten Domänen.

Weitere Informationen finden Sie in den folgenden Artikeln:

[Was für die Migration der sysvol-Replikation zu DFSR spricht](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Das Ende ist nahe für FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**F: Warum ist ein Neustart erforderlich, um die DC-Agent-Software zu installieren oder zu aktualisieren?**

Diese Anforderung wird durch das Kernverhalten von Windows verursacht.

**F: Gibt es eine Möglichkeit, einen DC-Agent so zu konfigurieren, dass er einen bestimmten Proxyserver verwendet?**

 Nein.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie eine Frage zum lokalen Azure AD-Kennwortschutz haben, die hier nicht beantwortet wird, senden Sie uns unten einen Feedbackkommentar. Vielen Dank!

[Bereitstellen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises-deploy.md)

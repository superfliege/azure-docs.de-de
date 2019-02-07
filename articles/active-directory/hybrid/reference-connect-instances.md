---
title: 'Azure AD Connect: Synchronisierungsdienstinstanzen | Microsoft-Dokumentation'
description: Diese Seite beschreibt besondere Überlegungen für Azure AD-Instanzen.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/18/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 0a3cbb352774fa319ad8ea6ad77f0cf089f75ab7
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489596"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Besondere Überlegungen zu Instanzen
Azure AD Connect wird am häufigsten mit der weltweiten Instanz von Azure AD und Office 365 verwendet. Es gibt jedoch noch weitere Instanzen, und für diese gelten andere Anforderungen an URLs sowie weitere besondere Überlegungen.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Deutschland
Die [Microsoft Cloud Deutschland](https://www.microsoft.de/cloud-deutschland) ist eine unabhängige Cloud, die von einem deutschen Datentreuhänder betrieben wird.

| URLs, die im Proxyserver geöffnet werden müssen |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| +Zertifikatsperrlisten |

Wenn Sie sich bei Ihrem Azure AD-Mandanten anmelden, müssen Sie ein Konto in der Domäne „onmicrosoft.de“ verwenden.

In der Microsoft Cloud Deutschland derzeit nicht enthaltene Features:

* **Kennwortrückschreiben** ist für die Vorschauversion mit Azure AD Connect-Version 1.1.570.0 und höher verfügbar.
* Andere Azure AD Premium-Dienste sind nicht verfügbar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government-Cloud
Die [Microsoft Azure Government-Cloud](https://azure.microsoft.com/features/gov/) ist eine Cloud für US-Regierungsbehörden.

Diese Cloud wurde von früheren Versionen von DirSync unterstützt. Ab Build 1.1.180 von Azure AD Connect wird die nächste Generation der Cloud unterstützt. Diese Generation verwendet ausschließlich in den USA basierte Endpunkte und weist eine andere Liste von URLs auf, die Sie in Ihrem Proxyserver öffnen müssen.

| URLs, die im Proxyserver geöffnet werden müssen |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*.windows.net (erforderlich für die automatische Erkennung von Azure AD-Behördenmandanten) |
| \*.gov.us.microsoftonline.com |
| +Zertifikatsperrlisten |

> [!NOTE]
> Wie bei Version 1.1.647.0 von AAD Connect ist die Festlegung des AzureInstance-Werts in der Registrierung nicht mehr erforderlich, sofern *. windows.net auf Ihren Proxyservern geöffnet ist.

In der Microsoft Azure Government-Cloud derzeit nicht enthaltene Features:

* **Kennwortrückschreiben** ist für die Vorschauversion mit Azure AD Connect-Version 1.1.570.0 und höher verfügbar.
* Andere Azure AD Premium-Dienste sind nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).

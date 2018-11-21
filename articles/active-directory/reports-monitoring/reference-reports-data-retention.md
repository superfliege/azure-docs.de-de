---
title: Aufbewahrungsrichtlinien für Azure Active Directory-Berichte | Microsoft Docs
description: Aufbewahrungsrichtlinien für Berichtdaten in Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 601169cc62a99438f661adc06ab166b545606edb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624589"
---
# <a name="azure-active-directory-report-retention-policies"></a>Aufbewahrungsrichtlinien für Azure Active Directory-Berichte

Dieser Artikel enthält Informationen zu den Datenaufbewahrungsrichtlinien für die unterschiedlichen Aktivitätsberichte in Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Wann beginnt Azure AD mit der Datensammlung?

| Azure AD-Edition | Start der Erfassung |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Beim Registrieren für ein Abonnement |
| Azure AD Free | Beim ersten Öffnen des Blatts [Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oder bei der ersten Verwendung der [Berichterstellungs-APIs](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Wann stehen die Aktivitätsdaten im Azure-Portal zur Verfügung?

- **Sofort**, wenn Sie bereits Berichte im Azure-Portal verwendet haben.
- **Innerhalb von 2 Stunden**, wenn Sie die Berichterstellung im Azure-Portal nicht aktiviert haben.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Wann beginnt Azure AD mit der Sammlung von Sicherheitssignaldaten?  

Die Erfassung von Sicherheitssignalen beginnt, wenn Sie sich für die Verwendung von **Identity Protection Center** entscheiden. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Wie lange speichert Azure AD die Daten?

**Aktivitätsberichte**    

| Bericht                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Verzeichnisprüfbericht        | 7 Tage        | 30 Tage             | 30 Tage             |
| Benutzeranmeldeaktivität       | N/V           | 30 Tage             | 30 Tage             |
| Azure MFA-Nutzung        | 30 Tage       | 30 Tage             | 30 Tage             |

**Sicherheitssignale**

| Bericht         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Gefährdete Benutzer  | 7 Tage        | 30 Tage             | 90 Tage             |
| Riskante Anmeldungen | 7 Tage        | 30 Tage             | 90 Tage             |

---

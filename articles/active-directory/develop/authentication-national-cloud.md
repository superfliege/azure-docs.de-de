---
title: Authentifizierung über Azure Active Directory in nationalen Clouds
description: Hier erhalten Sie Informationen zu App-Registrierungs- und Authentifizierungsendpunkten für nationale Clouds.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: negoe,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0d4586df23548854f4acbfefd32081a36906097
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067896"
---
# <a name="national-clouds"></a>Nationale Clouds

Nationale Clouds (auch als Sovereign Clouds bezeichnet) sind physisch isolierte Azure-Instanzen. Diese Regionen von Azure sollen dafür sorgen, dass Anforderungen an Datenresidenz, Datenhoheit und Compliance innerhalb geografischer Grenzen erfüllt werden.

Azure Active Directory wird in den folgenden nationalen Clouds bereitgestellt (globale Cloud eingeschlossen):  

- Azure US Government
- Azure Deutschland
- Azure China 21Vianet

Nationale Clouds sind eindeutig und eine andere Umgebung als globales Azure. Darum müssen Sie bei der Entwicklung Ihrer Anwendungen für diese Umgebungen, wie z.B. Registrierungsanwendungen und Anwendungen zum Abrufen von Token und Konfigurieren von Endpunkten, einige wesentliche Unterschiede beachten.

## <a name="app-registration-endpoints"></a>App-Registrierungsendpunkte

Es gibt für jede nationale Cloud ein separates Azure-Portal. Um Anwendungen mit der Microsoft Identity Platform in eine nationale Cloud zu integrieren, müssen Sie Ihre Anwendung separat jeweils im spezifischen Azure-Portal der Umgebung registrieren.

Die folgende Tabelle enthält die Basis-URLs für die Azure Active Directory-Endpunkte (Azure AD), die zum Registrieren einer Anwendung für die einzelnen nationalen Clouds verwendet werden:

| Nationale Cloud | Azure AD-Portalendpunkt |
|----------------|--------------------------|
| Azure AD für US-Regierungsbehörden | `https://portal.azure.us` |
| Azure AD Deutschland | `https://portal.microsoftazure.de` |
| Azure AD China, betrieben von 21Vianet | `https://portal.azure.cn` |
| Azure AD (globaler Dienst) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-Authentifizierungsendpunkte

Alle nationalen Clouds authentifizieren Benutzer in jeder Umgebung separat und verfügen über separate Authentifizierungsendpunkte.

Die folgende Tabelle enthält die Basis-URLs für die Azure Active Directory-Endpunkte (Azure AD), die zum Abrufen von Token für die einzelnen nationalen Clouds verwendet werden.

| Nationale Cloud | Azure AD-Authentifizierungsendpunkt |
|----------------|-------------------------|
| Azure AD für US-Regierungsbehörden | `https://login.microsoftonline.us` |
| Azure AD Deutschland| `https://login.microsoftonline.de` |
| Azure AD China, betrieben von 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (globaler Dienst)| `https://login.microsoftonline.com` |

- Anforderungen an die Autorisierungs- oder Tokenendpunkte von Azure AD können mit der entsprechenden regionsspezifischen Basis-URL generiert werden. Z.B. für Azure Deutschland:

  - Der allgemeine Autorisierungsendpunkt ist `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Der allgemeine Tokenendpunkt ist `https://login.microsoftonline.de/common/oauth2/token`.

- Ersetzen Sie bei Anwendungen mit einem Mandanten in den oben genannten URLs „common“ durch die ID oder den Namen Ihres Mandanten, z.B. `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> Die Endpunkte für die [Azure AD v2.0-Autorisierung]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) und -Token sind nur für den globalen Dienst verfügbar. Für Bereitstellungen nationaler Clouds werden sie noch nicht unterstützt.

## <a name="microsoft-graph-api"></a>Microsoft Graph-API

Informationen zum Aufrufen der Microsoft Graph-APIs in der Umgebung der nationalen Cloud finden Sie unter [Nationale Cloudbereitstellungen](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Bestimmte Dienste und Funktionen, die in bestimmten Regionen des globalen Diensts vorhanden sind, sind möglicherweise nicht in allen nationalen Clouds verfügbar. Um herauszufinden, welche Dienste verfügbar sind, wechseln Sie zu [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Führen Sie dieses [Tutorial zur Microsoft Authentication Library (MSAL)](msal-national-cloud.md) durch, um zu erfahren, wie Sie eine Anwendung mit Microsoft Identity Platform erstellen können. Mit dieser App wird ein Benutzer angemeldet und ein Zugriffstoken zum Aufrufen der Microsoft Graph-API abgerufen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Deutschland](https://docs.microsoft.com/azure/germany/)
- [Azure AD-Authentifizierungsgrundlagen](authentication-scenarios.md)

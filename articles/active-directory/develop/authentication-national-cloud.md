---
title: Authentifizierung mit Azure AD in nationalen Clouds
description: Hier erhalten Sie Informationen zu App-Registrierungs- und Authentifizierungsendpunkten für nationale Clouds.
services: active-directory
documentationcenter: ''
author: negoe
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/20/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a487c233e7bb71f05c6f2181d9c822ca508a583c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56192947"
---
# <a name="national-clouds"></a>Nationale Clouds

Nationale Clouds (auch Sovereign Clouds bezeichnet) sind physisch isolierte Azure-Instanzen. Diese Regionen von Azure sollen dafür sorgen, dass Anforderungen an Datenresidenz, Datenhoheit und Compliance innerhalb geografischer Grenzen erfüllt werden.

Azure Active Directory wird in den folgenden nationalen Clouds bereitgestellt (globale Cloud eingeschlossen):  

- Azure US Government
- Azure Deutschland
- Azure China 21Vianet

Nationale Clouds sind eindeutig und eine andere Umgebung als globales Azure. Darum müssen Sie bei der Entwicklung Ihrer Anwendungen für diese Umgebungen, wie z.B. Registrierungsanwendungen und Anwendungen zum Abrufen von Token und Konfigurieren von Endpunkten, einige wesentliche Unterschiede beachten.

## <a name="app-registration-endpoints"></a>App-Registrierungsendpunkte

Es gibt für jede nationale Cloud ein separates Azure-Portal. Um Anwendungen mit der Microsoft Identity Platform in eine nationale Cloud zu integrieren, müssen Sie Ihre Anwendung separat jeweils im spezifischen Azure-Portal der Umgebung registrieren.

Die folgende Tabelle enthält die Basis-URLs für die Azure Active Directory-Endpunkte (Azure AD), die zum Registrieren einer Anwendung für die einzelnen nationalen Clouds verwendet werden:

| Nationale Cloud | Azure AD-Portalendpunkt
| --- | --- |
| Azure AD für US-Regierungsbehörden |https://portal.azure.us
|Azure AD Deutschland |https://portal.microsoftazure.de
|Azure AD China, betrieben von 21Vianet |https://portal.azure.cn
|Azure AD (globaler Dienst)|https://portal.azure.com 

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-Authentifizierungsendpunkte

Alle nationalen Clouds authentifizieren Benutzer in jeder Umgebung separat und verfügen über separate Authentifizierungsendpunkte.

Die folgende Tabelle enthält die Basis-URLs für die Azure Active Directory-Endpunkte (Azure AD), die zum Abrufen von Token für die einzelnen nationalen Clouds verwendet werden.

| Nationale Cloud | Azure AD-Authentifizierungsendpunkt
| --- | --- |
| Azure AD für US-Regierungsbehörden |`https://login.microsoftonline.us`
|Azure AD Deutschland| `https://login.microsoftonline.de`
|Azure AD China, betrieben von 21Vianet | `https://login.chinacloudapi.cn`
|Azure AD (globaler Dienst)|`https://login.microsoftonline.com`

- Anforderungen an die Autorisierungs- oder Tokenendpunkte von Azure AD können mit der entsprechenden regionsspezifischen Basis-URL generiert werden. Z.B. für Azure Deutschland:

  - Der allgemeine Autorisierungsendpunkt ist `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Der allgemeine Tokenendpunkt ist `https://login.microsoftonline.de/common/oauth2/token`.

- Ersetzen Sie bei Anwendungen mit einem Mandanten in den oben genannten URLs „common“ durch die ID oder den Namen Ihres Mandanten, z.B. `https://login.microsoftonline.de/contoso.com`.

>[!NOTE]
> Die Endpunkte für die [Azure AD v2.0-Autorisierung]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) und -Token sind nur für den globalen Dienst verfügbar. Für Bereitstellungen nationaler Clouds werden sie noch nicht unterstützt.

## <a name="microsoft-graph-api"></a>Microsoft Graph-API

Informationen zum Aufrufen der Microsoft Graph-APIs in der Umgebung der nationalen Cloud finden Sie unter [Nationale Cloudbereitstellungen](https://developer.microsoft.com/graph/docs/concepts/deployments).



>[!IMPORTANT]
Bestimmte Dienste und Funktionen, die in bestimmten Regionen des globalen Diensts vorhanden sind, sind möglicherweise nicht in allen nationalen Clouds verfügbar. Um herauszufinden, welche Dienste verfügbar sind, wechseln Sie zu [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure Government](https://docs.microsoft.com/azure/azure-government/).
- Weitere Informationen zu [Azure China 21Vianet](https://docs.microsoft.com/azure/china/).
- Weitere Informationen zu [Azure Deutschland](https://docs.microsoft.com/azure/germany/).
- Weitere Informationen zu den [Authentifizierungsgrundlagen von Azure AD](authentication-scenarios.md).

---
title: Authentifizierung mit Azure AD in nationalen Clouds
description: Hier erhalten Sie Informationen zu App-Registrierungs- und Authentifizierungsendpunkten für nationale Clouds.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda
ms.custom: aaddev
ms.openlocfilehash: 866a86178d66b7b4af069d684e4eb56c12db47ca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982000"
---
# <a name="national-clouds"></a>Nationale Clouds

Nationale Clouds (auch Sovereign Clouds bezeichnet) sind physisch isolierte Azure-Instanzen. Diese Regionen von Azure sollen dafür sorgen, dass Anforderungen an Datenresidenz, Datenhoheit und Compliance innerhalb geografischer Grenzen erfüllt werden.

Azure Active Directory wird in den folgenden nationalen Clouds bereitgestellt (globale Cloud eingeschlossen):  

- Azure US Government
- Azure Deutschland
- Azure China 21Vianet

## <a name="app-registration-endpoints"></a>App-Registrierungsendpunkte

Die folgende Tabelle enthält die Basis-URLs für die Azure Active Directory-Endpunkte (Azure AD), die zum Registrieren einer Anwendung für die einzelnen nationalen Clouds verwendet werden:

| Nationale Cloud | Azure AD-Portalendpunkt
| --- | --- |
| Azure AD für US-Regierungsbehörden |https://portal.azure.us
|Azure AD Deutschland |https://portal.microsoftazure.de
|Azure AD China, betrieben von 21Vianet |https://portal.azure.cn
|Azure AD (globaler Dienst)|https://portal.azure.com

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-Authentifizierungsendpunkte

Die folgende Tabelle enthält die Basis-URLs für die Azure Active Directory-Endpunkte (Azure AD), die zum Abrufen von Token für den Aufruf von Microsoft Graph für die einzelnen nationalen Clouds verwendet werden:

| Nationale Cloud | Azure AD-Authentifizierungsendpunkt
| --- | --- |
| Azure AD für US-Regierungsbehörden |`https://login.microsoftonline.us`
|Azure AD Deutschland| `https://login.microsoftonline.de`
|Azure AD China, betrieben von 21Vianet | `https://login.chinacloudapi.cn`
|Azure AD (globaler Dienst)|`https://login.microsoftonline.com`

Anforderungen an die Autorisierungs- oder Tokenendpunkte von Azure AD können mit der entsprechenden regionsspezifischen Basis-URL generiert werden. Beispiel für Deutschland:

- Der allgemeine Autorisierungsendpunkt ist `https://login.microsoftonline.de/common/oauth2/authorize`.
- Der allgemeine Tokenendpunkt ist `https://login.microsoftonline.de/common/oauth2/token`. 

Ersetzen Sie bei Anwendungen mit einem Mandanten in den oben genannten URLs „common“ durch die ID oder den Namen Ihres Mandanten. Beispiel: `https://login.microsoftonline.de/contoso.com`

>[!NOTE]
> Die Endpunkte für die [Azure AD v2.0-Autorisierung]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) und -Token sind nur für den globalen Dienst verfügbar. Für Bereitstellungen nationaler Clouds werden sie noch nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- Weitere Informationen zu [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- Weitere Informationen zu [Azure Deutschland](https://docs.microsoft.com/azure/germany/)
- Weitere Informationen zu den [Authentifizierungsszenarien für Azure AD](authentication-scenarios.md)
- Weitere Informationen zur [Microsoft Graph-Bereitstellung in nationalen Clouds](https://developer.microsoft.com/graph/docs/concepts/deployments)
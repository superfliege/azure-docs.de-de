---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: a6fcbc0e8adac75f17d7379ff512ba650d0bb118
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203309"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Aufrufen der Microsoft Graph-API aus einer JavaScript-SPA

Diese Anleitung veranschaulicht, wie eine JavaScript-Single-Page-Webanwendung (SPA) eine Anmeldung für private Konten und Geschäfts-, Schul- oder Unikonten durchführen, ein Zugriffstoken abrufen und die Microsoft Graph-API oder andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2.0-Endpunkt benötigen.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funktionsweise der über diesen Leitfaden generierten Beispiel-App

![Zeigt, wie die in diesen Tutorials generierte Beispiel-App funktioniert](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro-updated.png)

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Die in dieser Anleitung erstellte Beispiel-App ermöglicht einer JavaScript-SPA das Abfragen der Microsoft Graph-API oder einer Web-API, die Token von einem Azure Active Directory v2.0-Endpunkt akzeptiert. In diesem Szenario wird nach der Benutzeranmeldung ein Zugriffstoken angefordert und über den Autorisierungsheader den HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft Authentication Library für JavaScript-Vorschau|

> [!NOTE]
> *msal.js* hat als Ziel den *Azure Active Directory v2.0-Endpunkt*, der privaten und Geschäfts-, Schul- oder Unikonten das Anmelden sowie das Abrufen von Token ermöglicht. Der *Azure Active Directory v2.0-Endpunkt* hat [einige Einschränkungen](../articles/active-directory/develop/active-directory-v2-limitations.md).
> Die Unterschiede zwischen v1.0- und v2.0-Endpunkten werden unter [Endpunktvergleich](../articles/active-directory/develop/azure-ad-endpoint-comparison.md) erläutert.

<!--end-collapse-->

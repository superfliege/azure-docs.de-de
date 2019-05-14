---
title: Web-API, die Web-APIs aufruft (Übergang in die Produktion) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-API erstellen, die Downstream-Web-APIs aufruft (Übergang in die Produktion).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7de892a0851e556c5046fe4b3691f39b42a9d237
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080084"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>Web-API, die Web-APIs aufruft (Übergang in die Produktion)

Sobald Sie ein Token zum Aufrufen von Web-APIs abgerufen haben, können Sie Ihre App in die Produktionsumgebung verschieben.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Weitere Informationen

Nachdem Sie sich mit den Grundlagen des Aufrufens von Web-APIs aus Ihrer eigenen Web-API vertraut gemacht haben, könnte Sie dieses Tutorial über den Code interessieren, der zum Erstellen einer geschützten Web-API verwendet wird, die Web-APIs aufruft.

| Beispiel | Plattform | BESCHREIBUNG |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2-Web-API, Desktop (WPF) | Die ASP.NET Core 2.2 Web-API ruft Microsoft Graph auf, wird selbst aber von einer WPF-Anwendung über Microsoft Identity Platform (v2.0) aufgerufen |

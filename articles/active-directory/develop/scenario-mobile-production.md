---
title: Mobile App, die Web-APIs aufruft (Übergang in die Produktion) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Übergang in die Produktion).
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d37d2de561a6f5841bf17a47fef86ad7639750d5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080110"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Mobile App, die Web-APIs aufruft (Übergang in die Produktion)

In diesem Artikel wird beschrieben, wie Sie die Qualität und Zuverlässigkeit Ihrer Anwendung erhöhen, damit der Übergang in die Produktion gelingt.

## <a name="handling-errors-in-mobile-applications"></a>Behandeln von Fehlern in mobilen Anwendungen

Für die bisher beschriebenen Flüssen wurden verschiedene Fehlerzustände aufgeführt, die eintreten können. Das Hauptszenario besteht in der Behandlung von Fehlern, die beim automatischen Abrufen eines Tokens auftreten, und dem Ausweichen auf eine Interaktion. Für Produktionsumgebungen sollten Sie außerdem Fehlerzustände berücksichtigen, die auf Netzwerkstörungen, Dienstausfälle, erforderliche Administratoreinwilligungen oder szenariobezogene Fälle zurückzuführen sind.

Für jede MSAL-Bibliothek sind Beispielcode und Wiki-Inhalte verfügbar, in denen ausführlich auf die Behandlung dieser Fehler eingegangen wird.

- [Android-Wiki für MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [iOS-Wiki für MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [Wiki für MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Untersuchen und Beheben von Problemen

Durch das Sammeln von Daten kann Ihre App leichter Probleme diagnostizieren. Weitere Informationen zu Arten von Daten, die Sie sammeln können, finden Sie im Wiki der entsprechenden MSAL-Plattform.

- Wenn bei Benutzern ein Problem auftritt, fordern sie möglicherweise Hilfe an. Als Best Practices empfehlen sich die temporäre Erfassung und Speicherung von Protokollen. Zusätzliche sollte für diese eine Uploadmöglichkeit für Benutzer bereitgestellt werden. MSAL stellt Protokollierungserweiterungen zur Verfügung, mit denen sich detaillierte Authentifizierungsinformationen erfassen lassen.
- Wenn eine Funktion für das Sammeln von Telemetriedaten verfügbar ist, können Sie diese mit MSAL nutzen und damit Daten erfassen, die Aufschluss darüber geben, wie sich Benutzer bei Ihrer App anmelden.

## <a name="testing-your-app"></a>Testen der App

Testen Sie Ihre App unbedingt mithilfe der [Checkliste zur Integration](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

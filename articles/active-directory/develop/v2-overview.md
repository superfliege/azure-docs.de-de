---
title: Informationen zu v2.0 | Azure
description: Informationen zum v2.0-Endpunkt und der -Plattform.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: adcb1b15ed52e6954846ea09be0a87a118222c10
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989583"
---
# <a name="about-v20"></a>Informationen zu v2.0

Der v2.0-Endpunkt und die -Plattform wurden in einer Vorschau vorgestellt und kontinuierlich verbessert. Heute sind die JavaScript Single-Page-Webanwendungsszenarien (SPA) vollständig, und wir laden Sie ein, „MSAL.js“ zu verwenden, um browserbasierte Anwendungen zu erstellen und uns Feedback zu geben, damit wir den Status aus der Vorschau in die allgemeine Verfügbarkeit (GA, General Availability) aktualisieren können.

> [!NOTE]
> Für MSAL Android, iOS und .NET befinden sich noch Features in der Entwicklung. Sie können sie verwenden, um Anwendungen zu erstellen und uns Feedback zu geben.

Die Entwicklerbenutzeroberfläche des Azure Portals wurde erheblich aktualisiert, um nun alle Ihre mit ADAL oder MSAL erstellten Anwendungen zu integrieren und die Benutzerfreundlichkeit zu verbessern.

In der Vergangenheit benötigten Anwendungsentwickler, die sowohl persönliche Microsoft-Konten als auch Geschäftskonten über Azure Active Directory (Azure AD) unterstützen wollten, eine Integration für zwei separate Systeme. Der v2.0-Endpunkt und die -Plattform bieten eine Version der Authentifizierungs-API, die diesen Vorgang vereinfacht. Mit dem v2.0-Endpunkt ist die Anmeldung über beide Arten von Konten mit einer einzelnen Integration möglich. Anwendungen, die den v2.0-Endpunkt verwenden, können auch die REST-APIs aus der [Microsoft Graph-API](https://graph.microsoft.io) mit beiden Kontotypen nutzen.

## <a name="getting-started"></a>Erste Schritte

Wählen Sie in der Liste weiter unten Ihre bevorzugte Plattform aus, um eine Anwendung mit den Open Source-Bibliotheken und -Frameworks von Microsoft zu erstellen. Sie können auch das OAuth 2.0-Protokoll und das OpenID Connect-Protokoll verwenden, um Protokollnachrichten direkt ohne Verwendung einer Authentifizierungsbibliothek zu senden und zu empfangen.

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>Weitere Informationen zum v2.0-Endpunkt und der -Plattform

Lernen Sie die Möglichkeiten kennen, die Ihnen der Azure AD v2.0-Endpunkt bietet:

* Informieren Sie sich über die [Arten von Anwendungen, die Sie mit dem Azure AD v2.0-Endpunkt erstellen können](v2-app-types.md).
* Machen Sie sich mit den [Einschränkungen](active-directory-v2-limitations.md) des Azure AD v2.0-Endpunkts vertraut.
* Das folgende Video bietet einen Überblick über den Azure AD v2.0-Endpunkt:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Hier erhalten Sie detaillierte Informationen zu v2.0:

* [v2.0-Protokollreferenz](active-directory-v2-protocols.md)
* [Zugriffstokenreferenz](access-tokens.md)
* [`id_tokens`-Referenz](id-tokens.md)
* [Referenz zu den v2.0-Authentifizierungsbibliotheken](reference-v2-libraries.md)
* [Bereiche und Zustimmung in v2. 0](v2-permissions-and-consent.md)
* [Microsoft Graph-API](https://graph.microsoft.io)

> [!NOTE]
> Wenn Sie nur Geschäfts-, Schul- und Unikonten aus Azure Active Directory anmelden müssen, lesen Sie das [Azure AD-Entwicklerhandbuch](azure-ad-developers-guide.md). Der v2.0-Endpunkt ist für die Verwendung durch Entwickler vorgesehen, die sich explizit bei persönlichen Microsoft-Konten anmelden müssen.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

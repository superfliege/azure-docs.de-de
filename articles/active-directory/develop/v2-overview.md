---
title: Informationen zu v2.0 | Azure
description: Informationen zum v2.0-Endpunkt und der -Plattform.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fb269df035bcc11583ebb7cff7d1ee2c3f6d8bca
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208298"
---
# <a name="about-v20"></a>Informationen zu v2.0

Der v2.0-Endpunkt und die -Plattform wurden in einer Vorschau vorgestellt und kontinuierlich verbessert. Heute sind die JavaScript Single-Page-Webanwendungsszenarien (SPA) vollständig, und wir laden Sie ein, „MSAL.js“ zu verwenden, um browserbasierte Anwendungen zu erstellen und uns Feedback zu geben, damit wir den Status aus der Vorschau in die allgemeine Verfügbarkeit (GA, General Availability) aktualisieren können.

> [!NOTE]
> Für MSAL Android, iOS und .NET befinden sich noch Features in der Entwicklung. Sie können sie verwenden, um Anwendungen zu erstellen und uns Feedback zu geben.

Die Benutzeroberfläche [App-Registrierungen (Vorschau)](quickstart-register-app.md) des Azure Portals wurde erheblich aktualisiert, um nun alle Ihre mit ADAL oder MSAL erstellten Anwendungen zu integrieren und die Benutzerfreundlichkeit zu verbessern.

In der Vergangenheit benötigten Anwendungsentwickler, die sowohl persönliche Microsoft-Konten als auch Geschäftskonten über Azure Active Directory (Azure AD) unterstützen wollten, eine Integration für zwei separate Systeme. Der v2.0-Endpunkt und die -Plattform bieten eine Version der Authentifizierungs-API, die diesen Vorgang vereinfacht. Mit dem v2.0-Endpunkt ist die Anmeldung über beide Arten von Konten mit einer einzelnen Integration möglich. Anwendungen, die den v2.0-Endpunkt verwenden, können auch die REST-APIs aus der [Microsoft Graph-API](https://developer.microsoft.com/graph) mit beiden Kontotypen nutzen.

## <a name="getting-started"></a>Erste Schritte

Wählen Sie in folgenden Liste Ihre bevorzugte Plattform aus, um eine Anwendung mit den Open-Source-Bibliotheken und -Frameworks von Microsoft zu erstellen:

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>Weitere Informationen zum v2.0-Endpunkt und der -Plattform

Lernen Sie die Möglichkeiten kennen, die Ihnen der Azure AD v2.0-Endpunkt bietet:

* Informieren Sie sich über die [Arten von Anwendungen, die Sie mit dem Azure AD v2.0-Endpunkt erstellen können](v2-app-types.md).
* Machen Sie sich mit den [Einschränkungen](active-directory-v2-limitations.md) des Azure AD v2.0-Endpunkts vertraut.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Hier erhalten Sie detaillierte Informationen zu v2.0:

* [Informationen zu Microsoft Identity Platform](about-microsoft-identity-platform.md)
* [v2.0-Protokollreferenz](active-directory-v2-protocols.md)
* [Zugriffstokenreferenz](access-tokens.md)
* [ID-Tokenreferenz](id-tokens.md)
* [Referenz zu den v2.0-Authentifizierungsbibliotheken](reference-v2-libraries.md)
* [Berechtigungen und Zustimmung in v2.0](v2-permissions-and-consent.md)
* [Microsoft Graph-API](https://developer.microsoft.com/graph)

> [!NOTE]
> Wenn Sie nur Geschäfts-, Schul- und Unikonten aus Azure Active Directory anmelden müssen, lesen Sie das [Azure AD-Entwicklerhandbuch](v1-overview.md). Der v2.0-Endpunkt ist für die Verwendung durch Entwickler vorgesehen, die sich explizit bei persönlichen Microsoft-Konten anmelden müssen.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

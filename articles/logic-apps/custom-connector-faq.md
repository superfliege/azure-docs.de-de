---
title: "Häufig gestellte Fragen zu benutzerdefinierten Connectors – Azure Logic Apps | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Anforderungen, Triggern und Ähnlichem im Zusammenhang mit der Erstellung benutzerdefinierter Connectors"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 225e56de3985acae871ddec447b763e7de61cb80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="faq-custom-connectors"></a>Häufig gestellte Fragen zu benutzerdefinierten Connectors

## <a name="requirements"></a>Requirements (Anforderungen)

**F:** Kann ich einen Connector ohne REST-APIs erstellen? </br>
**A:** Nein. Für die Connectorerstellung müssen Sie stabile HTTP-REST-APIs für Ihren Dienst unterstützen. 

**F:** Mit welchen Tools kann ich einen Connector erstellen? </br>
**A:** Azure bietet Funktionen und Dienste, mit denen Sie einen beliebigen Dienst als API verfügbar machen können. Hierzu zählen beispielsweise Azure App Service (Hosting) und API Management.

**F:** Welche Authentifizierungstypen werden unterstützt? </br>
**A:** Folgende unterstützte Authentifizierungsstandards stehen zur Verfügung:

* [OAuth 2.0](https://oauth.net/2/) (einschließlich [Azure Active Directory](https://azure.microsoft.com/develop/identity/)) oder spezifische Dienste, wie Dropbox, GitHub und Salesforce
* Generisches OAuth 2.0
* [Standardauthentifizierung](https://swagger.io/docs/specification/authentication/basic-authentication/)
* [API-Schlüssel](https://swagger.io/docs/specification/authentication/api-keys/)

## <a name="triggers"></a>Trigger

**F:** Kann ich Trigger ohne Webhooks erstellen? </br>
**A:** Nein. Benutzerdefinierte Connectors für Azure Logic Apps und Microsoft Flow unterstützen nur Webhook-basierte Trigger. Wenn Sie die Implementierung anderer Muster anfordern möchten, wenden Sie sich mit weiteren Details zu Ihrer API an [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com).

## <a name="certification"></a>Zertifizierung

**F:** Ich bin kein Microsoft-Partner oder unabhängiger Softwarehersteller (Independent Software Vendor, ISV). Kann ich trotzdem Connectors erstellen? </br>
**A**: Ja. Sie können diese Connectors für den internen Gebrauch in Ihrer Organisation registrieren. Wenn Sie einen Connector allerdings zertifizieren und öffentlich freigeben möchten, müssen Sie entweder den zugrunde liegenden Dienst besitzen oder ausdrücklich zur Nutzung der API berechtigt sein und dies belegen.

## <a name="other"></a>Sonstige

**F:** Meine APIs verwenden einen dynamischen Host. Wie kann ich sie mit OpenAPI implementieren? </br>
**A:** Benutzerdefinierte Connectors unterstützen keine dynamischen Hosts. Verwenden Sie zu Entwicklungs- und Testzwecken einen statischen Host. Wenn Sie Ihren Connector zertifizieren möchten, erkundigen Sie sich bei Ihrem Microsoft-Ansprechpartner nach der dynamischen Implementierung.

**F:** Wird Postman Collection V2 unterstützt? </br>
**A:** Nein. Derzeit wird nur Postman Collection V1 unterstützt.

**F:** Wird OpenAPI 3.0 unterstützt? </br>
**A:** Nein. Derzeit wird nur OpenAPI 2.0 unterstützt.
---
title: "Support- und Hilfeoptionen für Azure-Identitätsentwickler | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Hilfe und Support zu Problemen und Fragen im Zusammenhang mit der Entwicklung beim Erstellen einer Anwendung, die in Microsoft Azure-Identitäten (Azure Active Directory und MSA) integriert wird, erhalten."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/27/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 7c382da9bd9032b30f7c620e839c41c1756ba3f6
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="support-and-help-options-for-developers"></a>Support- und Hilfeoptionen für Entwickler 

Ungeachtet dessen, ob Sie gerade mit der Integration in Azure Active Directory, Microsoft-Identitäten oder Microsoft Graph-API beginnen oder ein neues Feature in Ihrer Anwendung implementieren, benötigen Sie manchmal Hilfe von der Community oder müssen die Supportoptionen kennen, die Ihnen als Entwickler zur Verfügung stehen. Dieser Artikel hilft Ihnen, diese Möglichkeiten zu kennen – hier folgt eine Zusammenfassung:

> [!div class="checklist"]
> * Versuchen Sie, durch Suchen herauszufinden, ob Ihr Problem noch nicht von der Community beantwortet wurde, oder ob für das Feature, das Sie zu implementieren versuchen, bereits Dokumentation vorhanden ist.
> * In einigen Fällen möchten Sie vielleicht einfach nur unsere Supporttools nutzen, um das Debuggen eines bestimmten Problems zu vereinfachen.
> * Wenn Sie die gesuchte Antwort nicht finden können, sollten Sie auf *Stack Overflow* eine Frage stellen.
> * Wenn Sie auf ein Problem mit einer unserer Authentifizierungsbibliotheken stoßen, erstellen Sie ein *GitHub*-Problem.
> * Wenn Sie schließlich mit einer Person sprechen müssen, stellen Sie eine Supportanfrage.


## <a name="search"></a>Suche

Wenn Sie eine auf die Entwicklung bezogene Frage haben, finden Sie die gewünschte Antwort möglicherweise in unserer Dokumentation, unseren [Github-Beispielen](https://github.com/azure-samples) oder in Antworten auf Fragen in [Stack Overflow](https://www.stackoverflow.com).

### <a name="scoped-search"></a>Bereichsbezogene Suche
Um schnellere Ergebnisse zu erzielen, beschränken Sie Ihre Suche auf Stack Overflow, unsere Dokumentation und unsere Codebeispiele, indem Sie in [Bing](https://bing.com) folgende Syntax verwenden:
```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/en-us/graph)
```
Hierbei entspricht *{Your Search Terms}* Ihren Schlüsselwörtern für die Suche.
<br/>

## <a name="use-our-development-support-tools"></a>Verwenden unserer Supporttools für die Entwicklung

|Tool  |Beschreibung  |
|---------|---------|
|[jwt.ms](https://jwt.ms)| Fügen Sie eine ID oder Zugriffstoken zum Entschlüsseln von Anspruchsnamen und Werten ein. |
|[Error Code Analyzer](https://apps.dev.microsoft.com/portal/tools/errors)| Fügen Sie einen Fehlercode ein, den Sie bei der Anmeldung oder von Zustimmungsseiten erhalten haben, um mögliche Ursachen und Wiederherstellungsmaßnahmen anzuzeigen. |
|[Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer)| Ein Tool, mit dem Sie Anforderungen an die Microsoft Graph-API richten und deren Antworten anzeigen können.|

<br/>

[![Stack Overflow](media/active-directory-develop-help-support/stackoverflow-logo.png)](https://www.stackoverflow.com)
## <a name="post-a-question-to-stack-overflow"></a>Richten einer Frage an Stack Overflow

Stack Overflow ist der bevorzugte Kanal für Fragen im Zusammenhang mit der Entwicklung, wobei sowohl Mitglieder der Community als auch des Microsoft-Teams Ihnen direkt bei der Problemlösung helfen können.

Wenn Sie mithilfe der Suche keine Lösung Ihres Problems finden, senden Sie eine neue Frage an Stack Overflow: Verwenden Sie eines der folgenden Tags beim Fragenstellen, damit die Community den Bereich der Frage identifizieren und Ihnen bald antworten kann:

|Komponente/Bereich  |Tags  |
|---------|---------|
|ADAL-Bibliothek |[[adal]](http://stackoverflow.com/questions/tagged/adal)|
|MSAL-Bibliothek     |[[msal]](http://stackoverflow.com/questions/tagged/msal)|
|OWIN-Middleware  |[[azure-active-directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)|
|[Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  |[[azure-ad-b2b]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  |[[azure-ad-b2c]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Microsoft Graph-API](https://developer.microsoft.com/graph/) |[[microsoft-graph]](http://stackoverflow.com/questions/tagged/microsoft-graph)
|Alle anderen Bereiche im Zusammenhang mit Authentifizierungs- oder Autorisierungsthemen |[[azure-active-directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)
<br/>
> [!TIP]
> Die folgenden Beiträge aus Stack Overflow enthalten Tipps zum Stellen von Fragen sowie zum Hinzufügen von Quellcode – Einhaltung dieser Richtlinien kann die Chancen erhöhen, dass Mitglieder der Community Ihre Frage schnell beantworten:  
> - [How do I ask a good question?](https://stackoverflow.com/help/how-to-ask) (Wie stelle ich meine Frage optimal?)
> - [How to create a Minimal, Complete, and Verifiable example](https://stackoverflow.com/help/mcve) (Erstellen eines minimalen, vollständigen und überprüfbaren Beispiels)

<br/>


[![Stack Overflow](media/active-directory-develop-help-support/github-logo.png)](https://www.github.com)
## <a name="create-a-github-issue"></a>Erstellen eines GitHub-Problems

 Wenn Sie einen Fehler oder ein Problem im Zusammenhang mit unserem Bibliotheken gefunden haben, erstellen Sie in unseren GitHub-Repositorys ein Problem. Da unsere Bibliotheken Open Source sind, können Sie auch einen Pull Request übermitteln. Der folgende Artikel enthält eine Liste der Bibliotheken und ihrer GitHub-Repositorys:

- [ADAL-, MSAL- und Owin-Middleware](active-directory-authentication-libraries.md)-Bibliotheken und GitHub-Repositorys

<br/>

## <a name="open-a-support-request"></a>Stellen einer Supportanfrage

Wenn Sie mit einer Person sprechen müssen, stellen Sie eine Supportanfrage. Wenn Sie ein Azure-Kunde sind, stehen Ihnen mehrere Supportoptionen zur Verfügung. Um Pläne zu vergleichen, lesen Sie [diese Seite](https://azure.microsoft.com/support/plans/). Support für Entwickler ist auch für Azure-Kunden verfügbar. Informationen zum Erwerb von Support für Entwickler finden Sie auf [dieser Seite](https://azure.microsoft.com/support/plans/developer/).

- Wenn Sie bereits einen Azure-Supportplan haben, [stellen Sie hier eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

- Wenn Sie kein Azure-Kunde sind, können Sie auch über [unseren kommerziellen Support](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial) eine Supportanfrage an Microsoft stellen.

Sie können auch [unseren virtuellen Agent](https://support.microsoft.com/contactus/?ws=support) ausprobieren, um Support zu erhalten oder Fragen zu stellen.

### <a name="free-chat-support-for-a-limited-time"></a>Kostenloser Chatsupport für begrenzte Zeit

Sie können auch unseren Chatsupport nutzen – der für eine begrenzte Zeit für Microsoft Partner kostenlos ist. Wenn Ihr Unternehmen kein Microsoft Partner ist, können Sie es kostenlos registrieren und erhalten weitere Vorteile – klicken Sie [hier](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Nach der Registrierung Ihres Unternehmens können Sie die Chatanfrage [hier](https://aka.ms/devchat) starten.
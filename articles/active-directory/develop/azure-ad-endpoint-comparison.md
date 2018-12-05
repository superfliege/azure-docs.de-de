---
title: 'Vergleich: Azure AD v2.0-Endpunkt und v1.0-Endpunkt | Microsoft-Dokumentation'
description: Machen Sie sich mit den Unterschieden zwischen dem Azure AD v2.0-Endpunkt und dem v1.0-Endpunkt vertraut
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2018
ms.author: celested
ms.reviewer: hirsin, andret, jmprieur, sureshja, jesakowi, lenalepa, kkrishna, dadobali, negoe
ms.custom: aaddev
ms.openlocfilehash: 6c11062dbae18eed618fe7dde95daa55024d2ddd
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444850"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Vergleich: Azure AD v2.0-Endpunkt und v1.0-Endpunkt

Beim Entwickeln einer neuen Anwendung ist es wichtig, dass Sie die Unterschiede zwischen Azure Active Directory (Azure AD) v1.0- und v2.0-Endpunkten kennen. Dieser Artikel behandelt die wichtigsten Unterschiede zwischen den Endpunkten und einige bestehende Einschränkungen für den v2.0-Endpunkt.

> [!NOTE]
> Der v2.0-Endpunkt unterstützt nicht alle Szenarien und Funktionen von Azure AD. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](#limitations), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.

## <a name="who-can-sign-in"></a>Wer kann sich anmelden?

![Wer kann sich mit v1.0- und v2.0-Endpunkten anmelden?](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* Beim v1.0-Endpunkt ist die Anmeldung an Ihrer Anwendung (Azure AD) nur mit Geschäfts-, Schul- und Unikonten möglich.
* Mit dem v2.0-Endpunkt können sich Geschäfts-, Schul- und Unikonten über Azure AD und persönliche Microsoft-Konten (MSA) (z.B. hotmail.com, outlook.com und msn.com) anmelden.
* Sowohl für v1.0- als auch für v2.0-Endpunkte werden außerdem Anmeldungen von *[Gastbenutzern](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* eines Azure AD-Verzeichnisses für Anwendungen akzeptiert, die für *[einen Mandanten](single-and-multi-tenant-apps.md)* konfiguriert sind, oder für Anwendungen mit *mehreren Mandanten*, für die ein Verweis auf den mandantenspezifischen Endpunkt (`https://login.microsoftonline.com/{TenantId_or_Name}`) konfiguriert ist.

Mit dem v2.0-Endpunkt können Sie Anwendungen schreiben, die Anmeldungen von persönlichen Microsoft-Konten sowie Geschäfts-, Schul- und Unikonten akzeptieren. Dies gibt Ihnen die Möglichkeit, Ihre Anwendung vollständig kontounabhängig zu schreiben. Wenn Ihre Anwendung beispielsweise [Microsoft Graph](https://graph.microsoft.io) aufruft, sind einige zusätzliche Funktionen und Daten für Geschäftskonten verfügbar, z.B. SharePoint-Websites oder Verzeichnisdaten. Aber für viele Aktionen, z.B. das [Lesen der E-Mail eines Benutzers](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_messages), kann mit demselben Code auf E-Mails für persönliche Konten und auch für Geschäfts-, Schul- und Unikonten zugegriffen werden.

Für den v2.0-Endpunkt können Sie die Microsoft Authentication Library (MSAL) nutzen, um Zugriff auf die Bereiche für Konsumenten, Weiterbildung und Unternehmen zu erhalten. Der Azure AD v1.0-Endpunkt akzeptiert nur Anmeldungen von Geschäfts-, Schul- und Unikonten.

## <a name="incremental-and-dynamic-consent"></a>Inkrementelle und dynamische Zustimmung

Für Apps, für die der Azure AD v1.0-Endpunkt verwendet wird, müssen vorher die erforderlichen OAuth 2.0-Berechtigungen angegeben werden, z.B.:

![Benutzeroberfläche für die Registrierung von Berechtigungen](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

Die Berechtigungen, die direkt in der Anwendungsregistrierung festgelegt werden, sind **statisch**. Statische Berechtigungen der Anwendung werden im Azure-Portal definiert und sorgen dafür, dass der Code klar und einfach ist, aber für Entwickler kann dies ggf. zu Problemen führen:

* Die Anwendung muss alle Berechtigungen anfordern, die nach der ersten Anmeldung des Benutzers jemals benötigt werden. Dies kann zu einer langen Liste von Berechtigungen führen, was Endbenutzer davon abhielt, bei der ersten Anmeldung den Zugriff der Anwendung zu genehmigen.

* Die Anwendung muss frühzeitig alle Ressourcen kennen, auf die sie jemals zugreifen muss. Es war schwierig, Apps zu erstellen, die auf eine beliebige Anzahl von Ressourcen zugreifen konnten.

Mit dem v2.0-Endpunkt können Sie die in den App-Registrierungsinformationen im Azure-Portal definierten statischen Berechtigungen ignorieren und stattdessen Berechtigungen schrittweise anfordern, was bedeutet, dass Sie im Voraus nach einem absoluten Minimum an Berechtigungen fragen müssen und im Laufe der Zeit mehr anfallen, da der Kunde zusätzliche Anwendungsfunktionen verwendet. Hierzu können Sie die Bereiche angeben, die für Ihre Anwendung zu beliebigen Zeitpunkten benötigt werden, indem Sie die neuen Bereiche in den Parameter `scope` einfügen, wenn Sie ein Zugriffstoken anfordern. Es ist nicht erforderlich, sie in den Informationen der Anwendungsregistrierung vorab zu definieren. Wenn der Benutzer noch nicht seine Zustimmung zu den neuen Bereichen gegeben hat, die der Anforderung hinzugefügt wurden, wird er aufgefordert, seine Zustimmung nur für die neuen Berechtigungen zu erteilen. Weitere Informationen dazu finden Sie unter [Berechtigungen, Zustimmung und Bereiche](v2-permissions-and-consent.md).

Wenn es für eine App zugelassen wird, Berechtigungen dynamisch über den `scope`-Parameter anzufordern, erhalten Entwickler die vollständige Kontrolle über die Erfahrung des Benutzers. Sie können die Zustimmungserfahrung auch vorziehen und alle Berechtigungen in einer anfänglichen Autorisierungsanforderung erfragen. Wenn Ihre Anwendung eine große Anzahl von Berechtigungen erfordert, können Sie die Berechtigungen des Benutzers inkrementell erfassen, während er über die Zeit bestimmte Features der Anwendung verwendet.

Die Administratorzustimmung im Namen einer Organisation erfordert weiterhin die für die Anwendung registrierten statischen Berechtigungen verwendet werden. Sie sollten diese Berechtigungen für die Anwendungen im Anwendungsregistrierungsportal festzulegen, falls ein Administrator die Zustimmung im Namen der gesamten Organisation erteilen muss. Hierdurch werden die Zyklen reduziert, die der Administrator der Organisation zum Einrichten der Anwendung benötigt.

## <a name="scopes-not-resources"></a>Bereiche, keine Ressourcen

Für Apps mit dem v1.0-Endpunkt kann sich eine App wie eine **Ressource** oder ein Empfänger von Token verhalten. Eine Ressource kann eine Anzahl von **Bereichen** oder **OAuth 2.0-Berechtigungen** definieren, die sie versteht, sodass Client-Apps Token für diese Ressource für einen bestimmten Satz von Bereichen anfordern können. Betrachten Sie als Beispiel für eine Ressource die Azure AD Graph-API:

* Ressourcenbezeichner oder `AppID URI`: `https://graph.windows.net/`
* Bereiche oder `oAuth2Permissions`: `Directory.Read`, `Directory.Write` usw.

Das gilt auch für den v2.0-Endpunkt. Eine Anwendung kann sich immer noch als Ressource verhalten, Bereiche definieren und durch einen URI identifiziert werden. Clientanwendungen können immer noch den Zugriff auf diese Bereiche anfordern. Allerdings hat sich die Art und Weise geändert, auf die ein Client solche Berechtigungen anfordert. 

Für den v1.0-Endpunkt kann eine an Azure AD gesendete OAuth 2.0-Autorisierungsanforderung beispielsweise wie folgt ausgesehen haben:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Hier gibt der **resource**-Parameter an, für welche Ressource die Clientanwendung eine Autorisierung anfordert. Azure AD hat die von der App benötigten Berechtigungen berechnet, und zwar basierend auf einer statischen Konfiguration im Azure-Portal. Die Token wurden entsprechend ausgestellt. 

Für Anwendungen mit dem v2.0-Endpunkt sieht die gleiche OAuth 2.0-Autorisierungsanforderung wie folgt aus:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Hierbei wird mit dem Parameter **scope** angegeben, für welche Ressourcen und Berechtigungen die Anwendung eine Autorisierung anfordert. Die gewünschte Ressource ist noch in der Anforderung vorhanden – sie ist von den einzelnen Werten des Bereichsparameters umgeben. Wenn der Bereichsparameter auf diese Weise verwendet wird, entspricht der v2.0-Endpunkt eher der OAuth 2.0-Spezifikation und richtet sich mehr nach den gängigen Methoden der Branche. Damit können Anwendungen auch [inkrementelle Zustimmung](#incremental-and-dynamic-consent) ausführen – d.h. nur dann, wenn die Anwendung sie benötigt, und nicht im Voraus.

## <a name="well-known-scopes"></a>Bekannte Bereiche

### <a name="offline-access"></a>Offlinezugriff

Für Apps, die den v2.0-Endpunkt verwenden, muss möglicherweise eine neue bekannte Berechtigung für Apps verwendet werden: der Bereich `offline_access`. Alle Apps müssen diese Berechtigung anfordern, wenn sie im Auftrag eines Benutzers für einen längeren Zeitraum auf Ressourcen zugreifen wollen, selbst, wenn der Benutzer die App nicht aktiv verwendet. Der `offline_access`-Bereich wird dem Benutzer in den Zustimmungsdialogfeldern als **Jederzeit auf Ihre Daten zugreifen** angezeigt, wofür der Benutzer seine Zustimmung gewähren muss. Durch Anfordern der `offline_access`-Berechtigung kann Ihre Web-App OAuth 2.0-Aktualisierungstoken vom v2. 0-Endpunkt erhalten. Aktualisierungstoken sind langlebig und können durch neue OAuth 2.0-Zugriffstoken für längere Zugriffszeiten ausgetauscht werden.

Wenn die Anwendung den `offline_access`-Bereich nicht anfordert, werden auch keine Aktualisierungstoken empfangen. Dies bedeutet, dass Sie beim Einlösen eines Autorisierungscodes im OAuth 2.0-Autorisierungscodefluss nur ein Zugriffstoken vom `/token`-Endpunkt erhalten. Dieses Zugriffstoken bleibt für einen kurzen Zeitraum (in der Regel eine Stunde) gültig, läuft aber anschließend ab. Zu diesem Zeitpunkt muss Ihre App den Benutzer zurück an den `/authorize`-Endpunkt leiten, um einen neuen Autorisierungscode abzurufen. Während dieser Umleitung muss der Benutzer möglicherweise seine Anmeldeinformationen erneut eingeben oder den Berechtigungen erneut zustimmen, je nach Anwendungstyp.

Weitere Informationen zu OAuth 2.0, `refresh_tokens` und `access_tokens` finden Sie in der [v2.0-Protokollreferenz](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, Profil und E-Mail

Bislang wurden im grundlegenden OpenID Connect-Anmeldefluss mit Azure AD zahlreiche Benutzerinformationen im resultierenden *id_token* bereitgestellt. Die Ansprüche in einem ID-Token können u. a. Name, bevorzugten Benutzernamen, E-Mail-Adresse, Objekt-ID eines Benutzers enthalten.

Die Informationen, auf die der Bereich `openid` Ihrer App Zugriff gewährt, sind nun beschränkt. Der Bereich `openid` erlaubt Ihrer App nur das Anmelden des Benutzers und das Empfangen eines App-spezifischen Bezeichners für den Benutzer. Falls Sie personenbezogene Daten über den Benutzer in Ihrer Anwendung erhalten möchten, müssen Sie über Ihre App zusätzliche Berechtigungen vom Benutzer einholen. Über zwei neue Bereiche `email` und `profile` können Sie weitere Berechtigungen anfordern.

* Der Bereich `email` gewährt Ihrer Anwendung mithilfe des Anspruchs `email` im „id_token“ Zugriff auf die primäre E-Mail-Adresse des Benutzers, vorausgesetzt, der Benutzer hat eine adressierbare E-Mail-Adresse. 
* Der Bereich `profile` ermöglicht Ihrer App den Zugriff auf alle anderen grundlegenden Informationen zum Benutzer: Name, bevorzugter Benutzername, Objekt-ID usw auf dem „id_token“.

Diese Bereiche können Sie für Ihre Anwendung den Weg der minimalen Offenlegung einschlagen. Sie können Benutzer nur um die Daten bitten, die für die Funktionsweise der Anwendung erforderlich sind. Weitere Informationen zu diesen Bereichen finden Sie in der [v2.0-Bereichsreferenz](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Tokenansprüche

Der v2.0-Endpunkt gibt standardmäßig einen kleineren Satz von Ansprüchen in seinen Token aus, um die Nutzlasten klein zu halten. Wenn Sie Anwendungen und Dienste haben, die von einem bestimmten Anspruch in einem v1.0-Token abhängig sind, der nicht mehr standardmäßig in einem v2.0-Token bereitgestellt wird, sollten Sie die Funktion [optionale Ansprüche](active-directory-optional-claims.md) verwenden, um diesen Anspruch einzuschließen.

## <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen, die Sie bei Verwendung von v2.0 kennen sollten.

Beim Erstellen von Anwendungen, die in die Microsoft Identity Platform integriert werden, müssen Sie entscheiden, ob der v2.0-Endpunkt und die Authentifizierungsprotokolle Ihren Anforderungen entsprechen. Der v1.0-Endpunkt und die Plattform werden weiterhin vollständig unterstützt und umfassen in gewisser Hinsicht mehr Funktionen als v2.0. Mit der Einführung von v2.0 ergeben sich allerdings [erhebliche Vorteile](azure-ad-endpoint-comparison.md) für Entwickler.

Folgendes wird Entwicklern derzeit empfohlen (vereinfacht dargestellt):

* Wenn Sie persönliche Microsoft-Konten in Ihrer Anwendung unterstützen möchten oder müssen oder eine neue Anwendung schreiben, verwenden Sie v2.0. Zuvor sollten Sie sich jedoch mit den in diesem Artikel behandelten Einschränkungen vertraut machen.
* Wenn Sie eine Anwendung migrieren oder aktualisieren, die auf SAML basiert, können Sie v2.0 nicht verwenden. Informieren Sie sich stattdessen im [Leitfaden zu v1.0](v1-overview.md).

Der v2.0-Endpunkt wird weiterentwickelt, sodass die hier aufgeführten Einschränkungen beseitigt werden. Daher können Sie in Zukunft nur noch den v2.0-Endpunkt verwenden. Bis dahin können Sie mithilfe dieses Artikels feststellen, ob der v2.0-Endpunkt für Sie geeignet ist. Wir werden diesen Artikel immer wieder aktualisieren, damit er den aktuellen Status des v2.0-Endpunkts widerspiegelt. Vergleichen Sie nochmals Ihre Anforderungen mit den v2.0-Funktionen.

### <a name="restrictions-on-app-registrations"></a>Einschränkungen für App-Registrierungen

Sie müssen für jede Anwendung, die Sie mit dem v2.0-Endpunkt integrieren möchten, eineApp-Registrierung im [Microsoft Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen. Alternativ können Sie eine Anwendung mithilfe der [**App-Registrierungen (Vorschauversion)**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) im Azure-Portal registrieren. Bestehende Microsoft-Kontoanwendungen sind nicht mit dem Vorschauportal kompatibel, aber alle AAD-Anwendungen sind es, unabhängig davon, wo und wann sie registriert wurden. 

App-Registrierungen, die Geschäfts-, Schul- und Unikonten und persönliche Konten unterstützen, haben die folgenden Einschränkungen:

* Pro Anwendungs-ID sind nur zwei Anwendungsgeheimnisse zulässig.
* Eine Anwendung, die nicht in einem Mandanten registriert war, kann nur von dem Konto verwaltet werden, das sie registriert hat. Sie kann nicht für andere Entwicklern freigegeben werden. Dies ist bei den meisten Anwendungen der Fall, die mit einem persönlichen Microsoft-Konto im App-Registrierungsportal registriert wurden. Wenn Sie Ihre App-Registrierung für mehrere Entwickler freigeben möchten, registrieren Sie die Anwendung in einem Mandanten unter Verwendung des Abschnitts **App-Registrierungen (Vorschauversion)** des Azure-Portals.
* Es gibt mehrere Einschränkungen, was das zulässige Format der Umleitungs-URL angeht. Weitere Informationen zur Umleitungs-URL finden Sie im nächsten Abschnitt.

### <a name="restrictions-on-redirect-urls"></a>Einschränkungen für Umleitungs-URLs

Für v2.0 registrierte Anwendungen sind auf eine begrenzte Zahl von Umleitungs-URL-Werten beschränkt. Die Umleitungs-URL für Web-Apps und -Dienste muss mit dem Schema `https` beginnen, und alle Umleitungs-URL-Werte müssen dieselbe DNS-Domäne verwenden.  Das Registrierungssystem vergleicht den gesamten DNS-Namen der vorhandenen Umleitungs-URL mit dem DNS-Namen der von Ihnen hinzugefügten Umleitungs-URL. `http://localhost` wird auch als Umleitungs-URL unterstützt.  

Die Anforderung zum Hinzufügen des DNS-Namens schlägt fehl, wenn eine der folgenden Bedingungen erfüllt ist:  

* Der gesamte DNS-Name der neuen Umleitungs-URL entspricht nicht dem DNS-Namen der vorhandenen Umleitungs-URL.
* Der gesamte DNS-Name der neuen Umleitungs-URL ist keine Unterdomäne der vorhandenen Umleitungs-URL.

#### <a name="example-1"></a>Beispiel 1

Wenn die Anwendung eine Umleitungs-URL von `https://login.contoso.com` hat, können Sie eine Umleitungs-URL hinzufügen, bei der der DNS-Name genau übereinstimmt, wie im folgenden Beispiel gezeigt:

`https://login.contoso.com/new`

Oder Sie können sich auf eine DNS-Unterdomäne von login.contoso.com beziehen, wie im folgenden Beispiel gezeigt:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Beispiel 2

Wenn Sie eine Anwendung verwenden möchten, für die `login-east.contoso.com` und `login-west.contoso.com` als Umleitungs-URLs angegeben sind, müssen Sie diese URLs in der folgenden Reihenfolge hinzufügen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Die beiden letztgenannten URLs können hinzugefügt werden, da es sich hierbei um Unterdomänen der ersten Umleitungs-URL „contoso.com“ handelt. Diese Einschränkung wird in einer zukünftigen Version aufgehoben.

Beachten Sie auch, dass Sie nur 20 Antwort-URLs für eine bestimmte Anwendung haben können – diese Begrenzung gilt für alle Anwendungstypen, die die Registrierung unterstützt (SPA, nativer Client, Web-App und Dienst).  

Um zu erfahren, wie Sie eine Anwendung für die Verwendung mit v2.0 registrieren, lesen Sie diese Schnellstarts:

* [Registrieren einer Anwendung mit dem Anwendungsregistrierungsportal](quickstart-v2-register-an-app.md)
* [Registrieren einer Anwendung mit den App-Registrierungen (Vorschauversion)](quickstart-register-app.md)

### <a name="restrictions-on-libraries-and-sdks"></a>Einschränkungen für Bibliotheken und SDKs

Die Bibliotheksunterstützung für den v2.0-Endpunkt ist gegenwärtig eingeschränkt. Wenn Sie den v2.0-Endpunkt in einer Produktionsanwendung verwenden möchten, stehen Ihnen folgende Möglichkeiten offen:

* Wenn Sie eine Webanwendung erstellen, können Sie die allgemein verfügbare serverseitige Middleware bedenkenlos für die Anmeldung und die Tokenüberprüfung einsetzen. Dazu gehören die OWIN Open ID Connect-Middleware für ASP.NET und das NodeJS Passport-Plug-In. Codebeispiele, die Microsoft-Middleware verwenden, finden Sie im Abschnitt zu den [ersten Schritten für v2.0](v2-overview.md#getting-started).
* Wenn Sie eine Anwendung für Desktop- oder Mobilgeräte entwickeln, können Sie eine der Microsoft Authentication Libraries verwenden (MSAL, derzeit in der Vorschauphase). Diese Bibliotheken gibt es in der für die Produktion unterstützten Vorschauversion, die Sie gefahrlos in Produktionsanwendungen einsetzen können. In der [Referenz zu Authentifizierungsbibliotheken](reference-v2-libraries.md) erfahren Sie mehr zu den Nutzungsbedingungen der Vorschauversion und verfügbaren Bibliotheken.
* Für Plattformen, die nicht von den Microsoft-Bibliotheken abgedeckt sind, können Sie die Integration mit dem v2.0-Endpunkt auch erreichen, indem Sie Protokollnachrichten direkt in Ihrem Anwendungscode senden und empfangen. Die v2.0-Protokolle OpenID Connect und OAuth [wurden explizit dokumentiert](active-directory-v2-protocols.md), um Sie bei einer solchen Integration zu unterstützen.
* Zudem können Sie die Open ID Connect- und OAuth-Open Source-Bibliotheken für die Integration in den v2.0-Endpunkt verwenden. Der v2.0-Endpunkt sollte ohne Änderungen mit vielen Open Source-Protokollbibliotheken kompatibel sein. Die Verfügbarkeit dieser Bibliotheksarten variiert je nach Sprache und Plattform. Die [Open ID Connect](https://openid.net/connect/)- und die [OAuth 2.0](https://oauth.net/2/)-Websites enthalten eine Liste gängiger Implementierungen. Weitere Informationen und die Liste der Open Source-Clientbibliotheken und -Beispiele, die mit dem v2.0-Endpunkt getestet wurden, finden Sie unter [Azure Active Directory (AD) v2.0 und Authentifizierungsbibliotheken](reference-v2-libraries.md).
* Als Referenz: Der `.well-known`-Endpunkt für den gemeinsamen v2.0-Endpunkt ist `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`. Ersetzen Sie `common` durch Ihre Mandanten-ID, um mandantenspezifische Daten abzurufen.  

### <a name="protocol-changes"></a>Protokolländerungen

Der v2.0-Endpunkt unterstützt keinen SAML- oder WS-Verbund, sondern nur Open ID Connect und OAuth 2.0.  Die wesentlichen Änderungen an den OAuth 2.0-Protokollen gegenüber dem v1.0-Endpunkt sind: 

* Der Anspruch `email` wird zurückgegeben, wenn ein optionaler Anspruch konfiguriert **oder** „scope=email“ in der Anforderung angegeben wurde. 
* Der `scope`-Parameter wird jetzt anstelle des `resource`-Parameters unterstützt.  
* Viele Antworten wurden geändert, um die Kompatibilität mit der OAuth 2.0-Spezifikation zu erhöhen, z.B. die korrekte Rückgabe von `expires_in` als int-Wert anstelle einer Zeichenfolge.  

Informationen zum Umfang der vom v2.0-Endpunkt unterstützten Protokollfunktionen finden Sie in der Referenz [OpenID Connect- und OAuth 2.0-Protokolle](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>SAML-Einschränkungen

Wenn Sie die Active Directory Authentication Library (ADAL) in Windows-Anwendungen verwendet haben, haben Sie möglicherweise die integrierte Windows-Authentifizierung genutzt, die mit der SAML-Assertionsgewährung (Security Assertion Markup Language) arbeitet. Diese Gewährung erlaubt Benutzern von Azure AD-Partnermandanten, sich im Hintergrund bei ihrer lokalen Active Directory-Instanz zu authentifizieren, ohne ihre Anmeldeinformationen eingeben zu müssen. Die SAML-Assertionsgewährung wird vom v2.0-Endpunkt nicht unterstützt.

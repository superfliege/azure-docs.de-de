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
ms.date: 09/27/2018
ms.author: andret
ms.reviewer: hirsin, andret
ms.custom: aaddev
ms.openlocfilehash: b75b31ddfc77be5ed651e7b8484e41a4ae73d8d8
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406531"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Vergleich: Azure AD v2.0-Endpunkt und v1.0-Endpunkt

Beim Entwickeln einer neuen Anwendung ist es wichtig, dass Sie die Unterschiede zwischen v1.0- und v2.0-Endpunkten kennen. Unten sind die Hauptunterschiede sowie einige vorhandene Einschränkungen für den v2.0-Endpunkt aufgeführt.

> [!NOTE]
> Nicht alle Szenarien und Funktionen von Azure Active Directory (Azure D) werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](#limitations), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.

## <a name="who-can-sign-in"></a>Wer kann sich anmelden?

![Wer kann sich mit v1.0- und v2.0-Endpunkten anmelden?](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* Beim v1.0-Endpunkt ist die Anmeldung an Ihrer Anwendung (Azure AD) nur mit Geschäfts-, Schul- und Unikonten möglich.

* Mit dem v2.0-Endpunkt können sich Geschäfts-, Schul- und Unikonten über Azure AD und persönliche Konten (MSA) (hotmail.com, outlook.com, msn.com) anmelden.

* Sowohl für v1.0- als auch für v2.0-Endpunkte werden außerdem Anmeldungen von *[Gastbenutzern](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* eines Azure AD-Verzeichnisses für Anwendungen akzeptiert, die für *[einen Mandanten](single-and-multi-tenant-apps.md)* konfiguriert sind, oder für Anwendungen mit *mehreren Mandanten*, für die ein Verweis auf den mandantenspezifischen Endpunkt (`https://login.microsoftonline.com/{TenantId_or_Name}`) konfiguriert ist.

Mit dem v2.0-Endpunkt können Sie Apps schreiben, die eine Anmeldung sowohl für persönliche Konten als auch für Geschäfts-, Schul- und Unikonten akzeptieren, sodass Sie Ihre App vollständig „kontoagnostisch“ schreiben können. Wenn Ihre App beispielsweise [Microsoft Graph](https://graph.microsoft.io) aufruft, sind einige zusätzliche Funktionen und Daten für Geschäftskonten verfügbar, z.B. SharePoint-Websites oder Verzeichnisdaten. Aber für viele Aktionen, z.B. das [Lesen der E-Mail eines Benutzers](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), kann mit demselben Code auf E-Mails für persönliche Konten und auch für Geschäfts-, Schul- und Unikonten zugegriffen werden.

Für den v2.0-Endpunkt können Sie eine einzelne Bibliothek (MSAL) nutzen, um Zugriff auf die Bereiche für Konsumenten, Weiterbildung und Unternehmen zu erhalten.

 Der Azure AD v1.0-Endpunkt akzeptiert nur Anmeldungen von Geschäfts-, Schul- und Unikonten.

## <a name="incremental-and-dynamic-consent"></a>Inkrementelle und dynamische Zustimmung

Für Apps, für die der Azure AD v1.0-Endpunkt verwendet wird, müssen vorher die erforderlichen OAuth 2.0-Berechtigungen angegeben werden, z.B.:

![Benutzeroberfläche für die Registrierung von Berechtigungen](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

Die Berechtigungen, die direkt in der Anwendungsregistrierung festgelegt werden, sind **statisch**. Statische Berechtigungen der App werden im Azure-Portal definiert und sorgen dafür, dass der Code klar und einfach ist, aber für Entwickler kann dies ggf. zu Problemen führen:

* Die App muss zum Erstellungszeitpunkt alle Berechtigungen kennen, die jemals von der App benötigt werden. Das Hinzufügen von Berechtigungen mit der Zeit war ein schwieriger Prozess.

* Die App muss frühzeitig alle Ressourcen kennen, auf die sie jemals zugreifen muss. Es war schwierig, Apps zu erstellen, die auf eine beliebige Anzahl von Ressourcen zugreifen konnten.

* Die App muss alle Berechtigungen anfordern, die nach der ersten Anmeldung des Benutzers jemals benötigt werden. In einigen Fällen führte dies zu einer langen Liste von Berechtigungen, was Endbenutzer davon abhielt, bei der ersten Anmeldung den Zugriff der App zu genehmigen.

Beim v2.0-Endpunkt können Sie die statisch definierten Berechtigungen ignorieren, die in den Informationen der Anwendungsregistrierung im Azure-Portal definiert sind, und die für die App erforderlichen Berechtigungen **dynamisch** zur Laufzeit angeben, während die App normal genutzt wird. Dies erfolgt unabhängig von den statisch definierten Berechtigungen in den Informationen der Anwendungsregistrierung.

Hierzu können Sie die Bereiche angeben, die für Ihre App zu unterschiedlichen Zeitpunkten benötigt werden, indem Sie die neuen Bereiche in den Parameter `scope` einfügen, wenn Sie ein Zugriffstoken anfordern. Es ist nicht erforderlich, sie in den Informationen der Anwendungsregistrierung vorab zu definieren.

Wenn der Benutzer noch nicht seine Zustimmung zu den neuen Bereichen gegeben hat, die der Anforderung hinzugefügt wurden, wird er aufgefordert, seine Zustimmung nur für die neuen Berechtigungen zu erteilen. Weitere Informationen dazu finden Sie unter [Berechtigungen, Zustimmung und Bereiche](v2-permissions-and-consent.md).

Wenn es für eine App zugelassen wird, Berechtigungen dynamisch über den `scope`-Parameter anzufordern, erhalten Entwickler die vollständige Kontrolle über die Erfahrung des Benutzers. Falls gewünscht, können Sie die Zustimmungserfahrung auch vorziehen und alle Berechtigungen in einer anfänglichen Autorisierungsanforderung erfragen. Wenn Ihre App eine große Anzahl von Berechtigungen erfordert, können Sie auch die Berechtigungen des Benutzers inkrementell erfassen, während er über die Zeit bestimmte Features Ihrer App verwendet.

Beachten Sie, dass für die Administratorzustimmung im Namen einer Organisation weiterhin die für die App registrierten statischen Berechtigungen verwendet werden. Es wird also empfohlen, diese Berechtigungen für Apps mit dem v2.0-Endpunkt festzulegen, falls ein Administrator die Zustimmung im Namen der gesamten Organisation erteilen muss. Hierdurch werden die Zyklen reduziert, die der Administrator der Organisation zum Einrichten der Anwendung benötigt.

## <a name="scopes-not-resources"></a>Bereiche, keine Ressourcen

Für Apps mit dem v1.0-Endpunkt kann sich eine App wie eine **Ressource** oder ein Empfänger von Token verhalten. Eine Ressource kann eine Anzahl von **Bereichen** oder **OAuth 2.0-Berechtigungen** definieren, die sie versteht, sodass Client-Apps Token für diese Ressource für einen bestimmten Satz von Bereichen anfordern können. Betrachten Sie als Beispiel für eine Ressource die Azure AD Graph-API:

* Ressourcenbezeichner, oder `AppID URI`: `https://graph.windows.net/`

* Bereiche oder `OAuth2Permissions`: `Directory.Read`, `Directory.Write` usw.

Das alles gilt auch für den v2.0-Endpunkt. Eine App kann sich immer noch als Ressource verhalten, Bereiche definieren und durch einen URI identifiziert werden. Clientanwendungen können immer noch den Zugriff auf diese Bereiche anfordern. Allerdings hat sich die Art und Weise geändert, auf die ein Client solche Berechtigungen anfordert. Für den v1.0-Endpunkt kann eine an Azure AD gesendete OAuth 2.0-Autorisierungsanforderung beispielsweise wie folgt ausgesehen haben:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Dabei gibt der **resouce**-Parameter an, für welche Ressource die Client-App eine Autorisierung anfordert. Azure AD hat die von der App benötigten Berechtigungen berechnet, und zwar basierend auf einer statischen Konfiguration im Azure-Portal. Die Token wurden entsprechend ausgestellt. Für Anwendungen mit dem v2.0-Endpunkt sieht die gleiche OAuth 2.0-Autorisierungsanforderung wie folgt aus:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Hierbei wird mit dem Parameter **Bereich** angegeben, für welche Ressourcen und Berechtigungen die App eine Autorisierung anfordert. Die gewünschte Ressource ist noch in der Anforderung vorhanden – sie ist nun einfach von den einzelnen Werten des Bereichsparameters umgeben. Wenn der Bereichsparameter auf diese Weise verwendet wird, entspricht der v2.0-Endpunkt eher der OAuth 2.0-Spezifikation und richtet sich mehr nach den gängigen Methoden der Branche. Dadurch können Apps auch eine [inkrementelle Zustimmung](#incremental-and-dynamic-consent) durchführen, die weiter oben beschrieben wurde.

## <a name="well-known-scopes"></a>Bekannte Bereiche

### <a name="offline-access"></a>Offlinezugriff

Für Apps, die den v2.0-Endpunkt verwenden, muss möglicherweise eine neue bekannte Berechtigung für Apps verwendet werden: der Bereich `offline_access`. Alle Apps müssen diese Berechtigung anfordern, wenn sie im Auftrag eines Benutzers für einen längeren Zeitraum auf Ressourcen zugreifen wollen, selbst, wenn der Benutzer die App nicht aktiv verwendet. Der `offline_access`-Bereich wird dem Benutzer in den Zustimmungsdialogfeldern als **Jederzeit auf Ihre Daten zugreifen** angezeigt, wofür der Benutzer seine Zustimmung gewähren muss. Durch Anfordern der `offline_access`-Berechtigung kann Ihre Web-App OAuth 2.0-Aktualisierungstoken vom v2. 0-Endpunkt erhalten. Aktualisierungstoken sind langlebig und können durch neue OAuth 2.0-Zugriffstoken für längere Zugriffszeiten ausgetauscht werden.

Wenn die App den `offline_access`-Bereich nicht anfordert, werden auch keine Aktualisierungstoken empfangen. Dies bedeutet, dass Sie beim Einlösen eines Autorisierungscodes im OAuth 2.0-Autorisierungscodefluss nur ein Zugriffstoken vom `/token`-Endpunkt erhalten. Dieses Zugriffstoken bleibt für einen kurzen Zeitraum (in der Regel eine Stunde) gültig, läuft aber anschließend ab. Zu diesem Zeitpunkt muss Ihre App den Benutzer zurück an den `/authorize`-Endpunkt leiten, um einen neuen Autorisierungscode abzurufen. Während dieser Umleitung muss der Benutzer möglicherweise seine Anmeldeinformationen erneut eingeben oder den Berechtigungen erneut zustimmen, je nach App-Typ.

Weitere Informationen zu OAuth 2.0, `refresh_tokens` und `access_tokens` finden Sie in der [v2.0-Protokollreferenz](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, Profil und E-Mail

Bislang wurden im grundlegenden OpenID Connect-Anmeldefluss mit Azure AD zahlreiche Benutzerinformationen im resultierenden *id_token* bereitgestellt. Die Ansprüche in einem *id_token* können für einen Benutzer beispielsweise Name, den bevorzugten Benutzernamen, E-Mail-Adresse, Objekt-ID und mehr enthalten.

Die Informationen, auf die der Bereich `openid` Ihrer App Zugriff gewährt, sind nun beschränkt. Der Bereich `openid` erlaubt Ihrer App nur das Anmelden des Benutzers und das Empfangen eines App-spezifischen Bezeichners für den Benutzer. Falls Sie personenbezogene Daten über den Benutzer in Ihrer App erhalten möchten, müssen Sie über Ihre App zusätzliche Berechtigungen vom Benutzer einholen. Über zwei neue Bereiche – die Bereiche `email` und `profile` – können Sie weitere Berechtigungen anfordern.

Der Bereich `email` gewährt Ihrer App mithilfe des Anspruchs `email` im „id_token“ Zugriff auf die primäre E-Mail-Adresse des Benutzers. Der Bereich `profile` ermöglicht Ihrer App den Zugriff auf alle anderen grundlegenden Informationen zum Benutzer: Name, bevorzugter Benutzername, Objekt-ID usw.

Hiermit können Sie für Ihre App den Weg der minimalen Offenlegung einschlagen. Sie können Benutzer nur um die Daten bitten, die für die Funktionsweise der App erforderlich sind. Weitere Informationen zu diesen Bereichen finden Sie in der [v2.0-Bereichsreferenz](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Tokenansprüche

Die Ansprüche in Token, die vom v2.0-Endpunkt ausgestellt werden, sind nicht mit den Token identisch, die von den allgemein verfügbaren Azure AD-Endpunkten ausgestellt werden. Apps, die zum neuen Dienst migriert werden und keinen bestimmten Anspruch erfordern, sind in „id_tokens“ oder „access_tokens“ vorhanden. Weitere Informationen zu verschiedenen Tokentypen, die im v2.0-Endpunkt verwendet werden, finden Sie in der Referenz zu [Zugriffstoken](access-tokens.md) und zu [`id_token`](id-tokens.md).

## <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen, die Sie bei Verwendung von v2.0 kennen sollten.

Beim Erstellen von Anwendungen, die in die Microsoft Identity Platform integriert werden, müssen Sie entscheiden, ob der v2.0-Endpunkt und die Authentifizierungsprotokolle Ihren Anforderungen entsprechen. Der v1.0-Endpunkt und die Plattform werden weiterhin vollständig unterstützt und umfassen in gewisser Hinsicht mehr Funktionen als v2.0. Mit der Einführung von v2.0 ergeben sich allerdings [erhebliche Vorteile](azure-ad-endpoint-comparison.md) für Entwickler.

Folgendes wird Entwicklern derzeit empfohlen (vereinfacht dargestellt):

* Wenn Sie in Ihrer Anwendung persönliche Microsoft-Konten unterstützen müssen, sollten Sie v2.0 verwenden. Zuvor sollten Sie sich jedoch mit den in diesem Artikel behandelten Einschränkungen vertraut machen.

* Wenn Ihre Anwendung nur Microsoft-Geschäfts-, Schul- und Unikonten unterstützen muss, sollten Sie v2. 0 nicht verwenden. Informieren Sie sich stattdessen im [Leitfaden zu v1.0](azure-ad-developers-guide.md).

Der v2.0-Endpunkt wird weiterentwickelt, sodass die hier aufgeführten Einschränkungen beseitigt werden. Daher können Sie in Zukunft nur noch den v2.0-Endpunkt verwenden. Bis dahin können Sie mithilfe dieses Artikels feststellen, ob der v2.0-Endpunkt für Sie geeignet ist. Wir werden diesen Artikel immer wieder aktualisieren, damit er den aktuellen Status des v2.0-Endpunkts widerspiegelt. Vergleichen Sie nochmals Ihre Anforderungen mit den v2.0-Funktionen.

### <a name="restrictions-on-app-types"></a>Einschränkungen bei App-Typen

Die folgenden App-Typen werden derzeit nicht vom v2.0-Endpunkt unterstützt. Eine Beschreibung der unterstützten App-Typen finden Sie unter [App-Typen in v2.0](v2-app-types.md).

#### <a name="standalone-web-apis"></a>Eigenständige Web-APIs

Sie können den v2.0-Endpunkt verwenden, um [eine Web-API zu erstellen, die mit OAuth 2.0 gesichert wird](v2-app-types.md#web-apis). Allerdings kann die Web-API nur Token von einer Anwendung empfangen, die dieselbe Anwendungs-ID aufweist. Sie können nicht von einem Client, der über eine andere Anwendungs-ID verfügt, auf eine Web-API zugreifen. Diesem Client ist es nicht möglich, Berechtigungen für Ihre Web-API anzufordern oder zu erhalten.

Wenn Sie wissen möchten, wie eine Web-API erstellt wird, die Token von einem Client mit derselben Anwendungs-ID akzeptiert, helfen Ihnen die Web-API-Beispiele im Abschnitt zu den [ersten Schritten für v2.0](v2-overview.md#getting-started) weiter.

### <a name="restrictions-on-app-registrations"></a>Einschränkungen für App-Registrierungen

Derzeit müssen Sie für jede App, die Sie mit dem v2.0-Endpunkt integrieren möchten, eine App-Registrierung im neuen [Microsoft Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen. Vorhandene Azure AD- oder Microsoft-Konto-Apps sind nicht mit dem v2.0-Endpunkt kompatibel. Apps, die in einem Portal registriert sind, bei dem es sich nicht um das Anwendungsregistrierungsportal handelt, sind nicht mit dem v2.0-Endpunkt kompatibel.

Darüber hinaus gelten für App-Registrierungen, die Sie im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen, die folgenden Einschränkungen:

* Pro Anwendungs-ID sind nur zwei App-Geheimnisse zulässig.

* Eine von einem Benutzer mit einem persönlichen Microsoft-Konto vorgenommene App-Registrierung kann nur von einem einzelnen Entwicklerkonto angezeigt und verwaltet werden. Sie kann nicht von mehreren Entwicklern gemeinsam genutzt werden. Wenn Sie Ihre App-Registrierung mit mehreren Entwicklern gemeinsam nutzen möchten, können Sie die Anwendung erstellen, indem Sie sich mit einem Azure AD-Konto beim Registrierungsportal anmelden.

* Es gibt mehrere Einschränkungen, was das zulässige Format der Umleitungs-URL angeht. Weitere Informationen zur Umleitungs-URL finden Sie im nächsten Abschnitt.

### <a name="restrictions-on-redirect-urls"></a>Einschränkungen für Umleitungs-URLs

Im App-Registrierungsportal registrierte Apps sind auf eine begrenzte Zahl von Umleitungs-URL-Werten beschränkt. Die Umleitungs-URL für Web-Apps und -Dienste muss mit dem Schema `https` beginnen, und alle Umleitungs-URL-Werte müssen dieselbe DNS-Domäne verwenden. Es ist beispielsweise nicht möglich, eine Web-App zu registrieren, die eine der folgenden Umleitungs-URLs aufweist:

* `https://login-east.contoso.com`  
* `https://login-west.contoso.com`

Das Registrierungssystem vergleicht den gesamten DNS-Namen der vorhandenen Umleitungs-URL mit dem DNS-Namen der von Ihnen hinzugefügten Umleitungs-URL. Die Anforderung zum Hinzufügen des DNS-Namens schlägt fehl, wenn eine der folgenden Bedingungen erfüllt ist:  

* Der gesamte DNS-Name der neuen Umleitungs-URL entspricht nicht dem DNS-Namen der vorhandenen Umleitungs-URL.

* Der gesamte DNS-Name der neuen Umleitungs-URL ist keine Unterdomäne der vorhandenen Umleitungs-URL.

Ein Beispiel: Die App hat die folgende Umleitungs-URL:

`https://login.contoso.com`

Eine Hinzufügung ist wie folgt möglich:

`https://login.contoso.com/new`

In diesem Fall stimmt der DNS-Name exakt überein. Alternativ haben Sie folgende Möglichkeit:

`https://new.login.contoso.com`

In diesem Fall verweisen Sie auf eine DNS-Unterdomäne von login.contoso.com. Wenn Sie eine App verwenden möchten, für die `login-east.contoso.com` und `login-west.contoso.com` als Umleitungs-URLs angegeben sind, müssen Sie diese URLs in der hier angegebenen Reihenfolge hinzufügen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Die beiden letztgenannten URLs können hinzugefügt werden, da es sich hierbei um Unterdomänen der ersten Umleitungs-URL „contoso.com“ handelt. Diese Einschränkung wird in einer zukünftigen Version aufgehoben.

Beachten Sie zudem, dass Sie nur 20 Antwort-URLs für eine bestimmte Anwendung verwenden können.

Informationen zum Registrieren einer App im Anwendungsregistrierungsportal finden Sie unter [Registrieren einer App mit dem v2.0-Endpunkt](quickstart-v2-register-an-app.md).

### <a name="restrictions-on-libraries-and-sdks"></a>Einschränkungen für Bibliotheken und SDKs

Die Bibliotheksunterstützung für den v2.0-Endpunkt ist gegenwärtig eingeschränkt. Wenn Sie den v2.0-Endpunkt in einer Produktionsanwendung verwenden möchten, stehen Ihnen folgende Möglichkeiten offen:

* Wenn Sie eine Webanwendung erstellen, können Sie die allgemein verfügbare serverseitige Middleware von Microsoft bedenkenlos für die Anmeldung und Tokenüberprüfung einsetzen. Dazu gehören die OWIN Open ID Connect-Middleware für ASP.NET und das NodeJS Passport-Plug-In. Codebeispiele, die Microsoft-Middleware verwenden, finden Sie im Abschnitt zu den [ersten Schritten für v2.0](v2-overview.md#getting-started).

* Wenn Sie eine Anwendung für Desktop- oder Mobilgeräte entwickeln, können Sie eine der Microsoft Authentication Libraries verwenden (MSAL, derzeit in der Vorschauphase). Diese Bibliotheken gibt es in der für die Produktion unterstützten Vorschauversion, die Sie gefahrlos in Produktionsanwendungen einsetzen können. In der [Referenz zu Authentifizierungsbibliotheken](reference-v2-libraries.md) erfahren Sie mehr zu den Nutzungsbedingungen der Vorschauversion und verfügbaren Bibliotheken.

* Für Plattformen, die nicht von den Microsoft-Bibliotheken abgedeckt sind, können Sie die Integration mit dem v2.0-Endpunkt auch erreichen, indem Sie Protokollnachrichten direkt in Ihrem Anwendungscode senden und empfangen. Die v2.0-Protokolle OpenID Connect und OAuth [wurden explizit dokumentiert](active-directory-v2-protocols.md), um Sie bei einer solchen Integration zu unterstützen.

* Zudem können Sie die Open ID Connect- und OAuth-Open Source-Bibliotheken für die Integration in den v2.0-Endpunkt verwenden. Das v2.0-Protokoll sollte ohne wesentliche Änderungen mit vielen Open Source-Protokollbibliotheken kompatibel sein. Die Verfügbarkeit dieser Bibliotheksarten variiert je nach Sprache und Plattform. Die [Open ID Connect](http://openid.net/connect/)- und die [OAuth 2.0](http://oauth.net/2/)-Websites enthalten eine Liste gängiger Implementierungen. Weitere Informationen und die Liste der Open Source-Clientbibliotheken und -Beispiele, die mit dem v2.0-Endpunkt getestet wurden, finden Sie unter [Azure Active Directory (AD) v2.0 und Authentifizierungsbibliotheken](reference-v2-libraries.md).

* Als Referenz: Der `.well-known`-Endpunkt für den gemeinsamen v2.0-Endpunkt ist `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`.  Ersetzen Sie `common` durch Ihre Mandanten-ID, um mandantenspezifische Daten abzurufen.  

### <a name="restrictions-on-protocols"></a>Einschränkungen für Protokolle

Der v2.0-Endpunkt unterstützt keinen SAML- oder WS-Verbund, sondern nur Open ID Connect und OAuth 2.0. Nicht alle Features und Funktionen von OAuth-Protokollen wurden in den v2.0-Endpunkt integriert.

Die folgenden Protokollfeatures und -funktionen sind derzeit für den v2. 0-Endpunkt *nicht verfügbar* oder werden *nicht unterstützt*:

* Der Anspruch `email` wird nur zurückgegeben, wenn ein optionaler Anspruch konfiguriert und „scope=email“ in der Anforderung angegeben wurde. Allerdings wird sich dieses Verhalten ändern, wenn der v2.0-Endpunkt aktualisiert wird, um mehr Kompatibilität mit den Standards Open ID Connect und OAuth 2.0 herzustellen.

* Die Ausgabe von Rollen- oder Gruppenansprüchen in ID-Token wird vom v2.0-Endpunkt nicht unterstützt.

* [OAuth 2.0 Resource Owner Password Credentials Grant](https://tools.ietf.org/html/rfc6749#section-4.3) (OAuth 2.0: Gewährung der Kennwortanmeldeinformationen des Ressourcenbesitzers) wird vom v2.0-Endpunkt nicht unterstützt.

Informationen zum Umfang der vom v2.0-Endpunkt unterstützten Protokollfunktionen finden Sie in der Referenz [v2.0-Protokolle – OAuth 2.0 und OpenID Connect](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>SAML-Einschränkungen

Wenn Sie die Active Directory Authentication Library (ADAL) in Windows-Anwendungen verwendet haben, haben Sie möglicherweise die integrierte Windows-Authentifizierung genutzt, die mit der SAML-Assertionsgewährung (Security Assertion Markup Language) arbeitet. Diese Gewährung erlaubt Benutzern von Azure AD-Partnermandanten, sich im Hintergrund bei ihrer lokalen Active Directory-Instanz zu authentifizieren, ohne ihre Anmeldeinformationen eingeben zu müssen. Die SAML-Assertionsgewährung wird vom v2.0-Endpunkt derzeit nicht unterstützt.

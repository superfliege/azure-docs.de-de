---
title: "Grundlegendes zu den impliziten \"oauth2\" Datenfluss in Azure AD gewähren | Microsoft Docs"
description: "Erfahren Sie mehr über Azure Active Directory-Implementierung, der die implizite OAuth2-Flow erteilen und gibt an, ob es für Ihre Anwendung geeignet ist."
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 241c744737515ee0c8d5d833a51121808877e559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Grundlegendes zu den impliziten Grant-Datenfluss von "oauth2" in Azure Active Directory (AD)
Die implizite Gewährung von "oauth2" ist oftmals die Gewährung für die mit der längsten Liste von Sicherheitsrisiken in der Spezifikation "oauth2". Und noch, ist der Ansatz von ADAL JS und die es wird, beim Schreiben von SPA-Anwendungen empfohlen implementiert. Was gibt? Es wird die alle durch die vor-und Nachteile: und wie sich herausstellt, ist die implizite Gewährung der beste Ansatz kann jedoch auch, für Anwendungen, die eine Web-API über JavaScript in einem Browser zu nutzen.

## <a name="what-is-the-oauth2-implicit-grant"></a>Was ist die implizite Gewährung von "oauth2"?
Die ultimative [OAuth2-autorisierungscodeerteilung](https://tools.ietf.org/html/rfc6749#section-1.3.1) die autorisierungserteilung der zwei verschiedene Endpunkte verwendet wird. Der autorisierungsendpunkt wird für die Benutzer Interaktion Phase verwendet einen Autorisierungscode vortäuschen. Die token-Endpunkt wird dann vom Client für den Austausch von der Code für ein Zugriffstoken und häufig auch ein Aktualisierungstoken verwendet. Webanwendungen sind erforderlich, um ihre eigenen Anmeldeinformationen an den tokenendpunkt hinsichtlich vorhanden, sodass der autorisierungsserver den Client authentifizieren kann.

Die [implizite Gewährung von "oauth2"](https://tools.ietf.org/html/rfc6749#section-1.3.2) ist eine Variante des anderen Autorisierung gewährt. Es kann ein Client ein Zugriffstoken abrufen (und Id_token, bei Verwendung [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) direkt vom autorisierungsendpunkt, ohne wenden Sie sich an den tokenendpunkt hinsichtlich noch die Authentifizierung des Clients. Diese Variante wurde speziell für JavaScript-basierte Anwendungen, die in einem Webbrowser ausführen entwickelt: Token werden in der ursprünglichen OAuth2-Spezifikation in einem URI-Fragment zurückgegeben. Die das token Bits zur Verfügung gestellt den JavaScript-Code auf dem Client, jedoch es wird sichergestellt, dass sie in umleitungen auf dem Server nicht enthalten sind. Zurückgeben von Token über Browser leitet direkt vom autorisierungsendpunkt. Es wurde auch die Vorteile des Entfernens von allen Anforderungen für die cross-Origin-Aufrufe, die sind erforderlich, wenn das JavaScript-Anwendung erforderlich ist, wenden Sie sich an den tokenendpunkt hinsichtlich.

Ein wichtiges Merkmal der die implizite Gewährung von "oauth2" ist die Tatsache, dass etwa nie return Aktualisierungstoken an den Client übergibt. Wie wir im nächsten Abschnitt sehen werden, wird, ist nicht wirklich erforderlich und wäre tatsächlich ein Sicherheitsproblem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geeignete Szenarien für die implizite Gewährung von "oauth2"
Wie die OAuth2-Spezifikation selbst deklariert wird, wurde die implizite Gewährung entwickelt wurden, damit Benutzer-Agent-Anwendungen – das heißt, JavaScript-Anwendungen, die in einem Browser ausführen können. Das definierende Merkmal solche Anwendung ist JavaScript-Code verwendet wird, für den Zugriff auf Serverressourcen (in der Regel in einer Web-API) und für die Anwendung UX entsprechend zu aktualisieren. Betrachten Sie z. B. Gmail oder Outlook Web Access-Anwendungen: Bei Auswahl eine Nachricht aus dem Posteingang nur Visualisierung Meldungsbereich geändert wird, um die neue Auswahl anzuzeigen, während der Rest der Seite unverändert bleibt. Dies ist im Gegensatz zu herkömmlichen / Umleitung basierende Web-apps führt, in dem alle Benutzerinteraktionen in eine vollständige Seite Postback und eine vollständige Seite Rendern der Antwort des neuen Servers.

Anwendungen, die die JavaScript-basiertes an dessen Extreme beansprucht werden Single Page Applications oder SPAs genannt: die Idee dabei ist, dass diese Anwendungen dienen nur ein HTML-Startseite und die zugeordneten JavaScript, mit der alle folgenden Interaktionen wird gesteuert durch Web-API-Aufrufe über JavaScript ausgeführt. Allerdings Hybrid Ansätze, bei denen die Anwendung größtenteils Postback-driven aber gelegentliche JS-Aufrufe ausführt, sind nicht ungewöhnlich, dass – die Diskussion zur Verwendung von impliziter Datenfluss ist auch für diejenigen relevant.

/ Umleitung basierende Anwendungen sichern Sie ihre Anforderungen mithilfe von Cookies, jedoch, die Vorgehensweise auch nicht für JavaScript-Anwendungen funktioniert in der Regel. Cookies können nur für die Domäne, der sie für generiert wurden während der JavaScript-Aufrufe in anderen Domänen Richtung weitergeleitet werden können. Tatsächlich werden, die häufig der Fall: Denken Sie an Anwendungen Aufruf Microsoft Graph-API, Office-API, Azure-API – alle Wohnsitz außerhalb der Domäne aus, in dem die Anwendung bereitgestellt wird. Ein wachsender Trend für JavaScript-Anwendungen ist keine Back-End-überhaupt der vertrauenden Seite 100 % 3. Partei Web-APIs, um ihre Business-Funktion zu implementieren.

Derzeit ist die bevorzugte Methode zum Schutz der Aufrufe von einer Web-API für die "oauth2" Träger token Ansatz, in dem jedem Aufruf von "oauth2" Zugriffstoken begleitet. Die Web-API überprüft das eingehende Zugriffstoken und, wenn sich die erforderlichen Bereiche gefunden werden, er gewährt Zugriff auf den angeforderten Vorgang. Implizite Datenfluss bietet eine bequeme JavaScript-Anwendungen zum Abrufen von Zugriffstoken für eine Web-API bietet zahlreiche Vorteile in Bezug auf Cookies:

* Token können zuverlässig ohne dass die Notwendigkeit für domänenübergreifende Aufrufe – obligatorische Eintragung des umleitungs-abgerufen werden, der URI, der Token zurückgegeben werden, garantiert, dass das Token nicht ersetzt werden
* JavaScript-Anwendungen können so viele Zugriffstoken, die sie für so viele Web-APIs sie Zielen auf – ohne Einschränkung Domänen benötigen, Abrufen
* HTML5-Features wie z. B. Sitzung oder lokaler Speicher vollständige Kontrolle über tokenzwischenspeicherung und Verwaltung der Objektlebensdauer erteilen, während der Verwaltung von Cookies für die app nicht transparent ist
* Zugriffstoken sind nicht anfällig gegenüber Angriffen von Cross-Site Request Websiteübergreifender anforderungsfälschung

Die implizite Grant-Datenfluss wird Aktualisierungstoken hauptsächlich aus Sicherheitsgründen nicht ausgegeben werden. Ein Aktualisierungstoken wird nicht als Zugriffstoken, erteilen jedoch weitaus höher Power erster daher wesentlich mehr Schäden aus, für den Fall, dass es Verlust vorliegt wie eng begrenzt. Klicken Sie in der impliziter Datenfluss Token in der URL übermittelt werden, daher das Risiko abfangen ist höher als in der Authorization Code Grant.

Beachten Sie jedoch, dass eine JavaScript-Anwendung einen anderen Mechanismus bereit, für die Erneuerung Zugriffstoken ohne wiederholt den Benutzer zum Eingeben von Anmeldeinformationen aufzufordern verfügt. Die Anwendung kann mithilfe ein versteckten IFRAMES neue token Anfragen an den autorisierungsendpunkt von Azure AD ausführen: solange der Browser wird eine aktive Sitzung noch (lesen: verfügt über ein Sitzungscookie) anhand der Azure AD-Domäne die Authentifizierungsanforderung erfolgreich ist möglich, ohne Eingreifen des Benutzers werden müssen.

Dieses Modell bietet der JavaScript-Anwendung die Möglichkeit, unabhängig voneinander erneuern Zugriffstoken und sogar neue für eine neue API abrufen, (vorausgesetzt, dass der Benutzer zuvor für sie freigegeben. Dadurch wird vermieden, die zusätzliche Last der abrufen, verwalten und schützen eine wertvolle-Element, z. B. ein Aktualisierungstoken. Das Element, das automatische Verlängerung möglich ist, macht das Sitzungscookie von Azure AD wird außerhalb der Anwendung verwaltet. Ein weiterer Vorteil dieses Ansatzes ist, dass ein Benutzer von Azure AD, verwenden eine der Anwendungen in Azure Active Directory ausgeführt wird, in einer der Registerkarten "Browser" signiert abmelden kann. Dadurch wird das Löschen des Sitzungscookie Azure AD, und der JavaScript-Anwendung verlieren automatisch die Möglichkeit, erneuern Sie Token für den Benutzer, mit Vorzeichen.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Eignet sich die implizite Gewährung für meine app?
Die implizite Gewährung zeigt Weitere Risiken als andere gewährt, und die Bereiche, denen Sie geachtet müssen, werden angemessen dokumentiert wird. Deutet, [Missbrauch des Zugriffstokens an imitieren Ressourcenbesitzer in impliziten Flow] [ OAuth2-Spec-Implicit-Misuse] und [OAuth 2.0-Gefahrenmodells und Sicherheitsaspekte][OAuth2-Threat-Model-And-Security-Implications]). Die höhere Risikoprofil ist jedoch größtenteils auf den Umstand zurückzuführen, dass es Anwendungen ermöglichen, die von einer Remoteressource an einen Browser bedient active Code auszuführen, soll. Wenn Sie beabsichtigen, eine Architektur SPA verfügen über keine Back-End-Komponenten oder eine Web-API über JavaScript aufrufen möchten, Verwendung impliziter Datenfluss für tokenabruf wird empfohlen.

Wenn Ihre Anwendung ein systemeigener Client ist, ist die implizite Datenfluss hervorragend nicht. Das Fehlen der Azure AD-Sitzungscookie im Kontext eines systemeigenen Clients ärztlichen Ihre Anwendung über die Möglichkeiten zum Verwalten einer langlebige Sitzungs. Dies bedeutet, dass die Anwendung fordert wiederholt den Benutzer beim Abrufen von Zugriffstoken nach neuen Ressourcen.

Wenn Sie eine Webanwendung eine Back-End- und Verarbeiten von einer API von den Back-End-Code enthält entwickeln, ist die implizite Datenfluss auch nicht gut geeignet. Andere gewährt Ihnen dabei helfen weit mehr Leistung. Beispielsweise enthält die "oauth2" erteilungsfluss Token abrufen, die über die Berechtigungen für die Anwendung selbst im Gegensatz zu Delegierungen für Benutzer widerspiegeln. Dies bedeutet, dass der Client hat die Möglichkeit, die programmgesteuerten Zugriff auf Ressourcen auch verwalten, wenn ein Benutzer nicht aktiv in einer Sitzung, und So weiter involviert ist. Nicht nur, dass aber solche gewährt bieten höhere Sicherheit gewährleistet. Z. B. Zugriffstoken für die Datenübertragung nie über den Browser des Benutzers, nicht in den Browserverlauf gespeichert werden, besteht das Risiko, so weiter. Die Clientanwendung kann beim Anfordern eines Tokens starke Authentifizierung durchführen.

## <a name="next-steps"></a>Nächste Schritte
* Eine vollständige Liste der Ressourcen für Entwickler, einschließlich Referenzinformationen für die Protokolle und die "oauth2" Autorisierung Flüsse Unterstützung von Azure AD gewähren finden Sie in der [Azure AD-Entwicklerhandbuch][AAD-Developers-Guide]
* Finden Sie unter [wie eine Anwendung in Azure AD integrieren] [ ACOM-How-To-Integrate] für zusätzliche Tiefe auf den Prozess der Integration.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

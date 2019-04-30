---
title: Erneutes Generieren von HTTP-Headern in Azure Application Gateway | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie eine Übersicht über die Möglichkeit, HTTP-Header in Azure Application Gateway erneut zu generieren.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/11/2019
ms.author: absha
ms.openlocfilehash: 20c484779e7ffe74ae01e33472b4cf8761d81b66
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682679"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Erneutes Generieren von HTTP-Headern mit Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP-Header ermöglichen dem Client und dem Server das Übergeben von zusätzlichen Informationen mit der Anforderung oder der Antwort. Durch das erneute Generieren dieser HTTP-Header können Sie verschiedene wichtige Szenarios umsetzen, z.B. das Hinzufügen von sicherheitsbezogenen Headerfeldern wie HSTS/X-XSS-Protection, das Entfernen von Antwortheaderfeldern, über die sensible Informationen offengelegt werden können oder das Entfernen von Portinformationen aus X-Forwarded-For-Headern. Application Gateway unterstützt das Hinzufügen, Entfernen oder Aktualisieren von HTTP-Anforderungs- und -Antwortheadern, während die Anforderungs- und Antwortpakete zwischen dem Client und den Back-End-Pools übertragen werden. Es bietet Ihnen die Möglichkeit, Bedingungen hinzuzufügen, um sicherzustellen, dass die angegebenen Header nur dann neu geschrieben werden, wenn bestimmte Bedingungen erfüllt sind. Die Funktion unterstützt außerdem mehrere [Servervariablen](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables), mit denen zusätzliche Informationen über die Anforderungen und Antworten gespeichert werden, wodurch Sie leistungsstarke Neuschreibungsregeln erstellen können.
> [!NOTE]
>
> Die Unterstützung für das erneute Generieren von HTTP-Headern ist nur für die [neue SKU [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) verfügbar.

![Erneutes Generieren von Headern](media/rewrite-http-headers/rewrite-headers.png)

## <a name="headers-supported-for-rewrite"></a>Header, für die das erneute Generieren unterstützt wird

Die Funktion ermöglicht Ihnen das erneute Generieren sämtlicher Header in der Anforderung und Antwort, abgesehen von Host-, Verbindungs- und Upgradeheadern. Sie können auch über das Anwendungsgateway benutzerdefinierte Header erstellen und der Anforderung und den Antworten hinzufügen, die über das Gateway weitergeleitet werden. 

## <a name="rewrite-conditions"></a>Bedingungen für das erneute Generieren

Anhand der Bedingungen für das erneute Generieren können Sie den Inhalt der HTTP(S)-Anforderungen und -Antworten auswerten und nur dann eine Neugenerierung ausführen, wenn eine oder mehrere Bedingungen erfüllt sind. Anhand der folgenden drei Typen von Variablen wertet das Anwendungsgateway den Inhalt von HTTP(S)-Anforderungen und -Antworten aus:

- HTTP-Header in der Anforderung
- HTTP-Header in der Antwort
- Servervariablen im Application Gateway

Mit einer Bedingung kann ermittelt werden, ob die angegebene Variable vorhanden ist, ob die angegebene Variable genau mit einem bestimmten Wert übereinstimmt oder ob die angegebene Variable genau einem bestimmten Muster entspricht. Mit der [PCRE (Perl Compatible Regular Expressions)-Bibliothek](https://www.pcre.org/) werden Muster für reguläre Ausdrücke implementiert, mit denen die Bedingungen übereinstimmen sollen. Weitere Informationen zur Syntax von regulären Ausdrücken finden Sie auf der [Perl-Manpage für reguläre Ausdrücke](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Aktionen für das erneute Generieren

Mit Aktionen für das erneute Generieren geben Sie die Anforderungs- und Antwortheader an, die erneut generiert werden sollen, sowie den neuen Wert, mit dem die ursprünglichen Header erneut generiert werden müssen. Sie können entweder einen neuen Header erstellen, den Wert eines vorhandenen Headers ändern oder einen vorhandene Header löschen. Der Wert eines neuen Headers oder eines vorhandenen Headers kann auf die folgenden Typen von Werten festgelegt werden:

- Text 
- Anforderungsheader: Wenn Sie einen Anforderungsheader bestimmen möchten, müssen Sie die Syntax {http_req_*headerName*} verwenden.
- Antwortheader: Wenn Sie einen Antwortheader bestimmen möchten, müssen Sie die Syntax {http_resp_*headerName*} verwenden.
- Servervariable: Wenn Sie eine Servervariable bestimmen möchten, müssen Sie die Syntax {var_*serverVariable*} verwenden.
- Die Kombination aus Text, Anforderungsheader, Antwortheader und Servervariablen.

## <a name="server-variables"></a>Servervariablen

Das Application Gateway speichert mit Servervariablen nützliche Informationen zum Server, zur Verbindung mit dem Client und zur momentanen Anforderung an die Verbindung, wie die IP-Adresse des Clients oder den Webbrowsertyp. Diese Variablen ändern sich dynamisch, z.B. wenn eine neue Seite geladen oder ein Formular gesendet wird. Anhand dieser Servervariablen können Sie Bedingungen für das erneute Generieren und Header für das erneute Generieren auswerten. 

Das Application Gateway unterstützt die folgenden Servervariablen:

| Unterstützte Servervariablen | BESCHREIBUNG                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | Enthält das Clientanforderung-Headerfeld „X-Forwarded-For“ mit der Variablen `client_ip` (in dieser Tabelle unten erläutert), die im Format (IP1, IP2, IP3,...) angefügt ist. Ist das Feld „X-Forwarded-For“ im Header der Clientanforderung nicht vorhanden, ist die Variable `add_x_forwarded_for_proxy` gleich der Variablen `$client_ip`. Diese Variable ist besonders in Szenarien hilfreich, in denen Kunden den X-Forwarded-For-Header, der von Application Gateway festgelegt wurde, neu generieren möchten, sodass der Header nur die IP-Adresse und keine Portinformationen enthält. |
| ciphers_supported          | Gibt die Liste mit den Verschlüsselungen zurück, die vom Client unterstützt werden.          |
| ciphers_used               | Gibt die Verschlüsselungszeichenfolge zurück, die für eine eingerichtete SLL-Verbindung verwendet wird. |
| client_ip                  | IP-Adresse des Clients, von dem das Anwendungsgateway die Anforderung empfangen hat. Befindet sich ein Reverseproxy vor dem Anwendungsgateway und dem ursprünglichen Client, gibt *client_ip* die IP-Adresse des Reverseproxys zurück. |
| client_port                | Port des Clients                                                  |
| client_tcp_rtt             | Informationen über die TCP-Verbindung des Clients. Verfügbar auf Systemen, die die Socketoption „TCP_INFO“ unterstützen. |
| client_user                | Wenn HTTP-Authentifizierung verwendet wird: der Benutzername, der bei der Authentifizierung angegeben wurde. |
| host                       | In dieser Reihenfolge: Hostname aus der Anforderungszeile oder Hostname aus dem Anforderungsheaderfeld „Host“ oder der Servername, der mit einer Anforderung übereinstimmt. |
| cookie_*Name*              | Das *Name*-Cookie                                            |
| http_method                | Die Methode, die für die URL-Anforderung verwendet wird, z. B. GET, POST etc. |
| http_status                | Sitzungsstatus, z. B. 200, 400, 403 etc.                       |
| http_version               | Anforderungsprotokoll, normalerweise „HTTP/1.0“, „HTTP/1.1“ oder „HTTP/2.0“. |
| query_string               | Die Liste der Variablenwertpaare nach dem „?“ in der angeforderten URL. |
| received_bytes             | Angeforderte Länge (einschließlich Anforderungszeile, Header und Anforderungstext) |
| request_query              | Argumente in der Anforderungszeile                                |
| request_scheme             | Anforderungsschema: „http“ oder „https“                            |
| request_uri                | Die vollständige ursprüngliche Anforderungs-URI (mit Argumenten)                   |
| sent_bytes                 | Anzahl der an einen Client gesendeten Bytes                             |
| server_port                | Port des Servers, der die Anforderung akzeptiert hat.                 |
| ssl_connection_protocol    | Gibt ein Protokoll einer hergestellten SLL-Verbindung zurück.        |
| ssl_enabled                | „On“, wenn die Verbindung im SSL-Modus arbeitet, andernfalls eine leere Zeichenfolge |

## <a name="rewrite-configuration"></a>Konfiguration für das erneute Generieren

Folgendes ist erforderlich, um das erneute Generieren eines HTTP-Headers zu konfigurieren:

1. Erstellen Sie die neuen Objekte, die benötigt werden, um die HTTP-Header neu zu generieren:

   - **Aktion für das erneute Generieren**: Dient dazu, die Felder für Anforderung und Anforderungsheader, die Sie erneut generieren möchten, und den neuen Wert anzugeben, in den die ursprünglichen Header erneut generiert werden müssen. Sie können auswählen, ob eine oder mehrere Bedingungen für das erneute Generieren mit einer Aktion für das erneute Generieren verknüpft werden sollen.

   - **Bedingung für das erneute Generieren**: Dies ist eine optionale Konfiguration. Wenn eine Bedingung für das erneute Generieren hinzugefügt wird, wertet sie den Inhalt der HTTP(S)-Anforderungen und -Antworten aus. Die Entscheidung, die mit der Bedingung für das erneute Generieren verbundene Aktion zum erneuten Generieren auszuführen, hängt davon ab, ob die HTTP(S)-Anforderung oder -Antwort mit der Bedingung für das erneute Generieren übereinstimmt. 

     Wenn einer Aktion mehr als eine Bedingung zugeordnet ist, wird die Aktion nur ausgeführt, wenn alle Bedingungen erfüllt sind, d.h., es wird eine logische UND-Operation durchgeführt.

   - **Regel zum erneuten Generieren**: Die Regel zum erneuten Generieren enthält mehrere Kombinationen aus Aktion und Bedingung zum erneuten Generieren.

   - **Regelreihenfolge**: Bestimmt die Reihenfolge, in der die verschiedenen Regeln zum erneuten Generieren ausgeführt werden. Dies ist hilfreich, wenn es mehrere Regeln zum erneuten Generieren in einem Satz zum erneuten Generieren gibt. Die Regel zum erneuten Generieren mit einem niedrigeren Wert für die Regelreihenfolge wird zuerst ausgeführt. Wenn Sie dieselbe Regelreihenfolge für zwei Regeln zum erneuten Generieren festlegen, ist die Reihenfolge der Ausführung unbestimmt.

   - **Satz zum erneuten Generieren**: Enthält mehrere Regeln zum erneuten Generieren, die einer Anforderungsroutingregel zugeordnet werden.

2. Der Satz für das erneute Generieren (*rewriteRuleSet*) muss an eine Routingregel angefügt werden. Dies liegt daran, dass die erstellte Konfiguration der erneuten Generierung anhand der Routingregel an den Quelllistener angefügt wird. Bei Verwendung einer einfachen Routingregel wird die Konfiguration der erneuten Generierung eines Headers einem Quelllistener zugeordnet und fungiert als erneute Generierung eines globalen Headers. Wenn eine pfadbasierte Routingregel verwendet wird, wird die Konfiguration der erneuten Generierung eines Headers in der URL-Pfadzuordnung definiert. Daher gilt sie nur für den bestimmten Pfadbereich einer Site.

Sie können mehrere solche Sätze für das erneute Generieren eines HTTP-Headers erstellen, und jeder dieser Sätze kann auf mehrere Listener angewendet werden. Allerdings können Sie auf einen bestimmten Listener nur einen Satz für das erneute Generieren anwenden.

## <a name="common-scenarios"></a>Häufige Szenarios

Einige gängige Szenarien, in denen das erneute Generieren von Headern erforderlich ist, werden unten erwähnt.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Entfernen von Portinformationen aus dem X-Forwarded-For-Header

Application Gateway fügt X-Forwarded-For-Header in alle Anforderungen ein, bevor es die Anforderungen an das Back-End weiterleitet. Das Format für diesen Header ist eine durch Trennzeichen getrennte Liste von IP:Port. Möglicherweise gibt es jedoch Szenarien, in denen die Back-End-Server erfordern, dass der Header ausschließlich IP-Adressen enthält. Zur Umsetzung derartiger Szenarien können durch das erneute Generieren von Headern die Portinformationen aus dem X-Forwarded-For-Header entfernt werden. Dies kann beispielsweise dadurch erreicht werden, indem der Header auf die Servervariable add_x_forwarded_for_proxy festgelegt wird. 

![Entfernen des Ports](media/rewrite-http-headers/remove-port.png)

### <a name="modify-the-redirection-url"></a>Ändern der Umleitungs-URL

Wenn eine Back-End-Anwendung eine Umleitungsantwort sendet, empfiehlt es sich u.U., den Client an eine andere als die von der Back-End-Anwendung angegebene URL umzuleiten. Ein mögliches Szenario: Ein App Service wird hinter einem Anwendungsgateway gehostet, und der Client muss eine Umleitung zu seinem relativen Pfad ausführen (Umleitung von „contoso.azurewebsites.net/path1“ zu „contoso.azurewebsites.net/path2“). 

Da App Service ein mehrinstanzenfähiger Dienst ist, nutzt er den Hostheader in der Anforderung zur Weiterleitung an den richtigen Endpunkt. App Services weisen den Standarddomänennamen „*.azurewebsites.net“ auf (z. B. „contoso.azurewebsites.net“), der sich vom Domänennamen des Anwendungsgateways (z.B. „contoso.com“) unterscheidet. Da die ursprüngliche Anforderung vom Client den Domänennamen des Anwendungsgateways „contoso.com“ als Hostnamen aufweist, ändert das Anwendungsgateway nun den Hostnamen in „contoso.azurewebsites.net“, damit der App Service sie an den richtigen Endpunkt weiterleiten kann. Wenn der App Service eine Umleitungsantwort sendet, verwendet er den gleichen Hostnamen im Adressheader seiner Antwort wie in der Anforderung, die er vom Anwendungsgateway empfängt. Daher sendet der Client die Anforderung direkt an „contoso.azurewebsites.net/path2“ und durchläuft nicht das Anwendungsgateway („contoso.com/path2“). Das Umgehen des Anwendungsgateways ist nicht wünschenswert. 

Dieses Problem kann durch Festlegen des Hostnamens im Adressheader des Anwendungsgateway-Domänennamens behoben werden. Hierzu können Sie eine Regel zum erneuten Generieren mit einer Bedingung erstellen, die auswertet, ob der Adressheader in der Antwort „azurewebsites.net“ enthält. Dabei wird `(https?):\/\/.*azurewebsites\.net(.*)$` als Muster eingegeben, und es wird eine Aktion zum erneuten Generieren des Adressheaders ausgeführt, sodass dieser den Hostnamen des Anwendungsgateways aufweist, indem `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als Headerwert eingegeben wird.

![Ändern des Adressheaders](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Implementieren von HTTP-Sicherheitsheadern, um Sicherheitsrisiken zu verhindern

Verschiedene Sicherheitsrisiken können behoben werden, indem die erforderlichen Header in der Anwendungsantwort implementiert werden. Beispiele für solche Sicherheitsheader sind X-XSS-Protection, Strict-Transport-Security und Content-Security-Policy. Über das Anwendungsgateway können Sie diese Header für alle Antworten festlegen.

![Sicherheitsheader](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>Löschen von unerwünschten Headern

Möglicherweise möchten Sie die Header aus der HTTP-Antwort entfernen, die sensible Informationen wie Back-End-Servername, Betriebssystem, Bibliotheksdetails usw. offenlegen. Diese können über das Anwendungsgateway entfernt werden.

![Löschen des Headers](media/rewrite-http-headers/remove-headers.png)

### <a name="check-presence-of-a-header"></a>Überprüfen des Vorhandenseins eines Headers

Sie können den HTTP-Anforderungs- oder -Antwortheader auf das Vorhandensein einer Header- oder Servervariablen untersuchen. Dies ist hilfreich, wenn das erneute Generieren von Headern nur dann erfolgen soll, wenn der betreffende Header auch vorhanden ist.

![Überprüfen des Vorhandenseins eines Headers](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Einschränkungen

- Das erneute Generieren von Verbindungs-, Upgrade- und Hostheadern wird noch nicht unterstützt.

- Wie in [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27) festgelegt, können Headernamen sämtliche alphanumerische Zeichen und Sonderzeichen beinhalten. Derzeit wird allerdings das Sonderzeichen „Unterstrich“ (\_) in Headernamen nicht unterstützt. 

## <a name="need-help"></a>Sie brauchen Hilfe?

Kontaktieren Sie uns unter [AGHeaderRewriteHelp@microsoft.com](mailto:AGHeaderRewriteHelp@microsoft.com), falls Sie Hilfe benötigen.

## <a name="next-steps"></a>Nächste Schritte

Eine Anleitung zum erneuten Generieren von HTTP-Header finden Sie in folgenden Artikeln:

-  [Erneutes Generieren von HTTP-Headern über das Azure-Portal](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
-  [Erneutes Generieren von HTTP-Headern über Azure PowerShell](add-http-header-rewrite-rule-powershell.md)

---
title: Erneutes Generieren von HTTP-Headern in Azure Application Gateway | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie eine Übersicht über die Möglichkeit, HTTP-Header in Azure Application Gateway erneut zu generieren.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: e89fe10768331f5b4099ce9a9e2204dd72aa0bff
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793463"
---
# <a name="rewrite-http-headers-with-application-gateway-public-preview"></a>Erneutes Generieren von HTTP-Headern in Application Gateway (Public Preview)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP-Header ermöglichen dem Client und dem Server das Übergeben von zusätzlichen Informationen mit der Anforderung oder der Antwort. Durch das erneute Generieren dieser HTTP-Header können Sie verschiedene wichtige Szenarios erzielen, z. B. das Hinzufügen von sicherheitsbezogenen Headerfeldern wie HSTS/X-XSS-Protection oder das Entfernen von Antwortheaderfeldern, über die sensible Informationen wie der Back-End-Servername offengelegt werden.

Application Gateway unterstützt jetzt die Möglichkeit, die Header der eingehenden HTTP-Anforderungen sowie der ausgehenden HTTP-Antworten erneut zu generieren. Sie können HTTP-Anforderungs- und -Antwortheader hinzufügen, entfernen oder aktualisieren, während die Anforderung/Antwort-Pakete zwischen dem Client und den Back-End-Pools verschoben werden. Sie können sowohl Standardheaderfelder als auch andere Headerfelder erneut generieren.

> [!NOTE]
> 
> Die Unterstützung für das erneute Generieren von HTTP-Headern ist nur für die [neue SKU [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) verfügbar.

Die Unterstützung für das erneute Generieren von Headern in Application Gateway bietet Folgendes:

- **Erneutes Generieren von globalen Headern:** Sie können bestimmte Header für alle Anforderungen und Antworten, die für eine Website gelten, neu generieren.
- **Erneutes Generieren von pfadbasierten Headern:** Diese Art des erneuten Generierens ermöglicht es, Header nur für die Anforderungen und Antworten neu zu generieren, die nur zu einem bestimmten Websitebereich gehören, z. B. der Bereich „Warenkorb“ mit der Bezeichnung /cart/\* (Warenkorb).

Diese Änderungen bedeuten für Sie:

1. Erstellen Sie die neuen Objekte, die benötigt werden, um die HTTP-Header neu zu generieren: 
   - **RequestHeaderConfiguration:** Dieses Objekt wird dazu verwendet, die Anforderungsheaderfelder zu bestimmen, die Sie neu generieren möchten, sowie den neuen Wert, den die ursprünglichen Header benötigen, um neu generiert werden zu können.
   - **ResponseHeaderConfiguration:** Dieses Objekt wird dazu verwendet, die Antwortheaderfelder, die Sie neu generieren möchten, und den neuen Wert zu bestimmen, den die ursprünglichen Header benötigen, um neu generiert werden zu können.
   - **ActionSet:** Dieses Objekt beinhaltet die Konfigurationen der Anforderungs- und Antwortheader, die oben bestimmt wurden. 
   - **RewriteRule:** Dieses Objekt beinhaltet alle *actionSets*, die oben bestimmt wurden. 
   - **RewriteRuleSet:** Dieses Objekt beinhaltet alle *rewriteRules* und muss an die Anforderungsroutingregel angefügt werden (Standard oder pfadbasiert).
2. Der Regelsatz für das erneute Generieren („RewriteRuleSet“) muss dann an eine Routingregel angefügt werden. Die erstellte Konfiguration der erneuten Generierung wird anhand der Routingregel an den Quelllistener angefügt. Bei Verwendung einer einfachen Routingregel wird die Konfiguration der erneuten Generierung eines Headers einem Quelllistener zugeordnet und fungiert als erneute Generierung eines globalen Headers. Wenn eine pfadbasierte Routingregel verwendet wird, wird die Konfiguration der erneuten Generierung eines Headers in der URL-Pfadzuordnung definiert. Daher gilt sie nur für den bestimmten Pfadbereich einer Site.

Sie können mehrere solche Regelsätze für das erneute Generieren eines HTTP-Headers erstellen, und jeder dieser Regelsätze kann auf mehrere Listener angewendet werden. Allerdings können Sie auf einen bestimmten Listener nur einen Regelsatz für das erneute Generieren eines HTTP-Headers anwenden.

Den Wert eines Headers können Sie neu generieren als:

- Textwert. 

  *Beispiel:* 

  ```azurepowershell-interactive
  $responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -  HeaderValue "max-age=31536000")
  ```

- Wert eines anderen Headers. 

  *Beispiel 1:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-RequestHeader" -HeaderValue {http_req_oldHeader}
  ```

  > [!Note] 
  > Wenn Sie einen Anforderungsheader bestimmen möchten, müssen Sie folgende Syntax verwenden: {http_req_headerName}.

  *Beispiel 2:*

  ```azurepowershell-interactive
  $responseHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-ResponseHeader" -HeaderValue {http_resp_oldHeader}
  ```

  > [!Note] 
  > Wenn Sie einen Antwortheader bestimmen möchten, müssen Sie folgende Syntax verwenden: {http_resp_headerName}.

- Wert aus unterstützten Servervariablen.

  *Beispiel:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Ciphers-Used" -HeaderValue "{var_ciphers_used}"
  ```

  > [!Note] 
  > Wenn Sie eine Servervariable bestimmen möchten, müssen Sie folgende Syntax verwenden: {var_serverVariable}.

- Eine Kombination aus den obenstehenden Möglichkeiten.

## <a name="server-variables"></a>Servervariablen

Mit Servervariablen werden nützliche Informationen auf einem Webserver gespeichert. Diese Variablen liefern Informationen zum Server, zur Verbindung mit dem Client und zur momentanen Anforderung an die Verbindung, wie die IP-Adresse des Clients oder den Webbrowsertyp. Sie ändern sich dynamisch, z.B. wenn eine neue Seite geladen oder ein Formular gesendet wird.  Mithilfe dieser Variablen können Benutzer Anforderungsheader sowie Antwortheader festlegen. 

Diese Möglichkeit unterstützt die erneute Generierung von Headern in Form der folgenden Servervariablen:

| Unterstützte Servervariablen | BESCHREIBUNG                                                  |
| -------------------------- | :----------------------------------------------------------- |
| ciphers_supported          | Gibt die Liste mit den Verschlüsselungen zurück, die vom Client unterstützt werden.          |
| ciphers_used               | Gibt die Verschlüsselungszeichenfolge zurück, die für eine eingerichtete SLL-Verbindung verwendet wird. |
| client_ip                  | IP-Adresse des Clients, von dem das Anwendungsgateway die Anforderung empfangen hat. Befindet sich ein Reverseproxy vor dem Anwendungsgateway und dem ursprünglichen Client, gibt *client_ip* die IP-Adresse des Reverseproxys zurück. Diese Variable ist besonders in Szenarios hilfreich, in denen Kunden den X-Forwarded-For-Header, der von Application Gateway festgelegt wurde, neu generieren möchten, sodass der Header nur die IP-Adresse und keine Portinformationen enthält. |
| client_port                | Port des Clients                                                  |
| client_tcp_rtt             | Informationen über die TCP-Verbindung des Clients. Verfügbar auf Systemen, die die Socketoption „TCP_INFO“ unterstützen. |
| client_user                | Wenn HTTP-Authentifizierung verwendet wird: der Benutzername, der bei der Authentifizierung angegeben wurde. |
| host                       | In dieser Reihenfolge: Hostname aus der Anforderungszeile oder Hostname aus dem Anforderungsheaderfeld „Host“ oder der Servername, der mit einer Anforderung übereinstimmt. |
| cookie_*Name*              | Das *Name*-Cookie |
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

## <a name="limitations"></a>Einschränkungen

- Die Möglichkeit, HTTP-Header erneut zu generieren, ist momentan nur über Azure PowerShell, die Azure-API und Azure SDK verfügbar. Die Unterstützung über das Portal und die Azure-Befehlszeilenschnittstelle folgt in Kürze.

- Die Unterstützung für das erneute Generieren von HTTP-Headern ist nur für die neue SKU [Standard_V2](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) verfügbar. Auf der alten SKU wird diese Möglichkeit nicht unterstützt.

- Das erneute Generieren von Verbindungs-, Upgrade- und Hostheadern wird noch nicht unterstützt.

- Die Möglichkeit, HTTP-Header bedingt erneut zu generieren, ist bald verfügbar.

- Wie in [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27) festgelegt, können Headernamen sämtliche alphanumerische Zeichen und Sonderzeichen beinhalten. Derzeit wird allerdings das Sonderzeichen „Unterstrich“ (\_) in Headernamen nicht unterstützt. 

## <a name="need-help"></a>Sie brauchen Hilfe?

Kontaktieren Sie uns unter [AGHeaderRewriteHelp@microsoft.com](mailto:AGHeaderRewriteHelp@microsoft.com), falls Sie Hilfe benötigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr zur Möglichkeit erfahren haben, HTTP-Header neu zu generieren, finden Sie unter [Create an autoscaling and zone-redundant application gateway that rewrites HTTP headers (Erstellen eines automatisch skalierenden und zonenredundanten Application Gateway-Diensts, der HTTP-Header erneut generiert)](tutorial-http-header-rewrite-powershell.md) oder [Rewrite HTTP headers in existing autoscaling and zone-redundant application gateway (Erneutes Generieren von HTTP-Headern in einem vorhandenen automatisch skalierenden und zonenredundanten Application Gateway-Diensts)](add-http-header-rewrite-rule-powershell.md) weitere Informationen.

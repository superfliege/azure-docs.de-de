---
title: Azure Application Gateway-Komponenten
description: Dieser Artikel bietet Informationen zu den verschiedenen Komponenten in Application Gateway.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 44c8b331ebb258c39a003c91e0711e6dfb87cb12
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58076292"
---
# <a name="application-gateway-components"></a>Application Gateway-Komponenten

 Ein Anwendungsgateway dient als zentraler Kontaktpunkt für Clients. Es verteilt eingehenden Anwendungsdatenverkehr über mehrere Back-End-Pools hinweg, z.B. virtuelle Azure-Computer, VM-Skalierungsgruppen, App Services oder lokale bzw. externe Server. Zu diesem Zweck verwendet ein solches Gateway verschiedene Komponenten, wie in diesem Artikel beschrieben.

![application-gateway-components](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-address"></a>Front-End-IP-Adresse

Eine Front-End-IP-Adresse ist die IP-Adresse, die dem Anwendungsgateway zugeordnet ist. Sie können das Anwendungsgateway mit einer öffentlichen IP-Adresse, mit einer privaten IP-Adresse oder mit beidem konfigurieren. Es werden nur jeweils eine öffentliche oder private IP-Adresse unterstützt. Ihr virtuelles Netzwerk und die öffentliche IP-Adresse müssen sich am gleichen Standort befinden wie Ihr Anwendungsgateway.

Eine Front-End-IP-Adresse wird nach dem Erstellen einem *Listener* zugeordnet. 

### <a name="static-vs-dynamic-public-ip-address"></a>Statische und dynamische öffentliche IP-Adresse im Vergleich

Die V1-SKU unterstützt statische interne IP-Adressen, nicht aber statische öffentliche IP-Adressen. Die VIP kann sich ändern, wenn das Application Gateway beendet und gestartet wird. Der zugeordnete DNS-Name von Application Gateway ändert sich während des Lebenszyklus des Gateways nicht. Daher wird empfohlen, einen CNAME-Alias zu verwenden und damit auf die DNS-Adresse von Application Gateway zu verweisen.

Die Application Gateway V2-SKU unterstützt statische öffentliche IP-Adressen sowie statische interne IP-Adressen. 

## <a name="listeners"></a>Listener

Bevor Sie Ihr Anwendungsgateway verwenden können, müssen Sie mindestens einen Listener hinzufügen. Ein Listener ist eine logische Entität, die nach eingehenden Verbindungsanforderungen sucht und diese akzeptiert, wenn das Protokoll, der Port, der Host und die IP-Adresse der Anforderung mit dem Protokoll, dem Port, dem Host und der IP-Adresse der Listenerkonfiguration übereinstimmen. An ein Anwendungsgateway können mehrere Listener angefügt sein, und sie können für dasselbe Protokoll verwendet werden. Nachdem der Listener eingehende Anforderungen von Clients erkannt hat, leitet das Anwendungsgateway diese an die Mitglieder des Back-End-Pools weiter. Dabei werden die Routingregeln für Anforderungen verwendet, die Sie für den Listener definiert haben, der die eingehende Anforderung empfängt.

Listener unterstützen die folgenden Ports und Protokolle:

### <a name="ports"></a>Ports

Dies ist der Port, an dem der Listener auf Clientanforderungen lauscht. Sie können Ports von 1 bis 65502 für die V1-SKU und 1 bis 65199 für die V2-SKU konfigurieren.

### <a name="protocols"></a>Protokolle

Das Anwendungsgateway unterstützt die folgenden vier Protokolle: HTTP, HTTPS, HTTP/2 und WebSocket.

Sie geben die genaue Option für HTTP- und HTTPS-Protokolle in der Listenerkonfiguration an. Die [Unterstützung für die Protokolle WebSocket und HTTP/2](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) wird nativ bereitgestellt. Die [WebSocket-Unterstützung](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) ist standardmäßig aktiviert. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. Sie können das WebSocket-Protokoll sowohl mit HTTP- als auch mit HTTPS-Listenern verwenden. Die Unterstützung des HTTP/2-Protokolls ist nur für Clients verfügbar, die mit Application Gateway-Listenern verbunden sind. Die Kommunikation mit Back-End-Serverpools erfolgt über HTTP/1.1. Die HTTP/2-Unterstützung ist standardmäßig deaktiviert. Sie können es aktivieren.

Sie können einen HTTPS-Listener für die SSL-Terminierung verwenden. Ein HTTPS-Listener lagert die Ver- und Entschlüsselung an Ihr Anwendungsgateway aus, sodass Ihre Webserver mit diesem Overhead nicht belastet werden. Ihre Anwendungen können sich ganz auf die Geschäftslogik konzentrieren.

### <a name="custom-error-pages"></a>Benutzerdefinierte Fehlerseiten

Mit einem Anwendungsgateway können Sie benutzerdefinierte Fehlerseiten erstellen, anstatt Standardfehlerseiten anzuzeigen. Sie können für eine benutzerdefinierte Fehlerseite Ihr eigenes Branding und Layout verwenden. Application Gateway kann eine benutzerdefinierte Fehlerseite anzeigen, wenn eine Anforderung das Back-End nicht erreichen kann. Weitere Informationen finden Sie unter [ Benutzerdefinierte Application Gateway-Fehlerseiten](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Arten von Listenern

Es gibt zwei Arten von Listenern:

- **Basic**: Diese Listener lauschen an einer einzelnen Domänenwebsite, wo eine einzelne DNS-Zuordnung zur IP-Adresse des Anwendungsgateways vorliegt. Diese Listenerkonfiguration ist erforderlich, wenn Sie eine einzelne Website hinter einem Anwendungsgateway hosten.
- **Mehrere Websites**: Diese Listenerkonfiguration ist erforderlich, wenn Sie mehrere Webanwendungen in der gleichen Anwendungsgateway-Instanz konfigurieren. So können Sie eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie bis zu 100 Websites zu einem einzigen Anwendungsgateway hinzufügen. Jede Website kann an ihren eigenen Back-End-Pool weitergeleitet werden. Beispiel:   Für drei Unterdomänen – abc.contoso.com, xyz.contoso.com und pqr.contoso.com –, die auf die IP-Adresse des Anwendungsgateway zeigen. Erstellen Sie drei Listener vom Typ *Mehrere Websites*, und konfigurieren Sie jeden Listener für den jeweiligen Port und die jeweilige Protokolleinstellung. Weitere Informationen finden Sie unter [Hosten von mehreren Websites](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Nachdem Sie einen Listener erstellt haben, ordnen Sie ihm eine Anforderungsroutingregel zu, die bestimmt, wie die am Listener empfangene Anforderung an das Back-End weitergeleitet werden soll.

Listener werden gemäß der Anzeigereihenfolge verarbeitet. Wenn also ein einfacher Listener zu einer eingehenden Anforderung passt, wird diese zuerst verarbeitet. Listener für mehrere Standorte müssen vor einem grundlegenden Listener konfiguriert werden, um zu gewährleisten, dass der Datenverkehr an das richtige Back-End weitergeleitet wird.

## <a name="request-routing-rule"></a>Anforderungsroutingregel

Dies ist die wichtigste Komponente des Anwendungsgateways und legt fest, wie der Datenverkehr auf dem Listener, der mit dieser Regel verknüpft ist, weitergeleitet wird. Diese Regel bindet den Listener, den Back-End-Serverpool und die Back-End-HTTP-Einstellungen. Sobald ein Listener eine Anforderung akzeptiert hat, legt die Anforderungsroutingregel fest, ob die Anforderung an das Back-End weitergeleitet oder an eine andere Stelle umgeleitet werden soll. Wenn die Anforderung an das Back-End weitergeleitet werden soll, definiert die Anforderungsroutingregel, an welchen Back-End-Server die Weiterleitung erfolgen soll. Darüber hinaus legt die Anforderungsroutingregel auch fest, ob die Header in der Anforderung neu geschrieben werden sollen. Ein Listener kann nur mit einer Regel verknüpft werden.

Es gibt zwei Arten von Routingregeln für Anforderungen:

- **Basic:** Alle Anforderungen für den zugeordneten Listener (Beispiel: blog.contoso.com/*) werden mit der zugeordneten HTTP-Einstellung an den zugeordneten Back-End-Pool weitergeleitet.
- **Pfadbasiert**: Mit Regeln dieses Typs können Sie Anforderungen für den zugeordneten Listener basierend auf der URL in der Anforderung an einen bestimmten Back-End-Pool weiterleiten. Wenn der Pfad der URL in einer Anforderung dem Pfadmuster in einer pfadbasierten Regel entspricht, wird die Anforderung anhand dieser Regel geroutet. Das Pfadmuster wird nur auf den Pfad der URL angewendet, nicht auf ihre Abfrageparameter. Wenn der URL-Pfad einer Anforderung für einen Listener keiner der pfadbasierten Regeln entspricht, wird die Anforderung an mit den *Standard-HTTP-Einstellungen* an den *Standard-Back-End-Pool* geroutet. Weitere Informationen finden Sie unter [URL-basiertes Routing](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Umleitungsunterstützung

Die Routingregel für Anforderungen ermöglicht es Ihnen auch, Datenverkehr im Anwendungsgateway umzuleiten. Vielmehr handelt es sich um einen generischen Umleitungsmechanismus, sodass Sie Umleitungen für jeden Port durchführen können, den Sie mithilfe von Regeln definieren. Sie können als Umleitungsziel eine externe Website oder einen anderen Listener auswählen (die letztere Option ermöglicht eine automatische HTTP- oder HTTPS-Umleitung). Sie können festlegen, ob die Umleitung temporär oder permanent ist, und Sie können den URI-Pfad und die Abfragezeichenfolge an die umgeleitete URL anfügen. Weitere Informationen finden Sie unter [Umleiten von Datenverkehr im Application Gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="rewrite-http-headers"></a>Erneutes Generieren von HTTP-Headern

Mit Anwendungsroutingregeln können Sie HTTP(S)-Anforderungs- und -Antwortheader hinzufügen, entfernen oder aktualisieren, während die Anforderungs- und Antwortpakete über das Anwendungsgateway zwischen dem Client und den Back-End-Pools weitergeleitet werden. Die Header können nicht nur auf statische Werte, sondern auch auf andere Header und wichtige Servervariablen festgelegt werden. So können Sie verschiedene wichtige Anwendungsfälle abdecken – z.B. das Extrahieren der IP-Adressen der Clients, das Entfernen vertraulicher Informationen zum Back-End, das Hinzufügen von Sicherheitsmaßnahmen und vieles mehr. Weitere Informationen finden Sie unter [Erneutes Generieren von HTTP-Headern im Application Gateway](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP-Einstellungen

Das Anwendungsgateway routet Datenverkehr an die Back-End-Server (diese sind in der Anforderungsroutingregel angegeben, an die die HTTP-Einstellungen angefügt sind). Dabei werden Portnummer, Protokoll sowie weitere in dieser Komponente angegebene Einstellungen verwendet. Die Angaben für Port und Protokoll in den HTTP-Einstellungen bestimmen, ob der Datenverkehr zwischen dem Anwendungsgateway und den Back-End-Servern verschlüsselt wird, um End-to-End-SSL zu erzielen, oder ob er unverschlüsselt bleibt. Diese Komponente wird auch für Folgendes verwendet: Mithilfe der [cookiebasierten Sitzungsaffinität](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity) kann festgelegt werden, ob eine Benutzersitzung auf dem gleichen Server beibehalten werden soll. Mit einem [Verbindungsausgleich](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining) können Mitglieder des Back-End-Pools ordnungsgemäß entfernt werden. Benutzerdefinierte Tests ermöglichen die Überwachung der Integrität des Back-Ends. Außerdem können Sie das Timeoutintervall für Anforderungen festlegen, den Hostnamen und -pfad in der Anforderung überschreiben und mit wenigen Mausklicks die Back-End-Einstellungen für das App Service-Back-End festlegen. 

## <a name="backend-pool"></a>Back-End-Pool

Der Back-End-Pool wird zum Weiterleiten von Anforderungen an die Back-End-Server verwendet, die die Anforderung verarbeiten. Back-End-Pools können Netzwerkkarten, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDN) und mehrinstanzenfähige Back-Ends wie Azure App Service umfassen. Mitglieder des Anwendungsgateway-Back-End-Pools sind nicht an eine Verfügbarkeitsgruppe gebunden. Das Anwendungsgateway kann mit Instanzen außerhalb des virtuellen Netzwerks kommunizieren, zu dem es gehört. Daher können sich die Mitglieder des Back-End-Pools in verschiedenen Clustern und Rechenzentren oder sogar außerhalb von Azure befinden, solange eine IP-Verbindung besteht. Wenn Sie die Verwendung interner IP-Adressen als Back-End-Pool-Mitglieder planen, ist [VNet-Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) oder [VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) erforderlich. VNET-Peering wird unterstützt und bietet Vorteile für den Lastenausgleich von Datenverkehr in anderen virtuellen Netzwerken. Das Anwendungsgateway kann auch mit lokalen Servern kommunizieren, wenn diese über ExpressRoute oder VPN-Tunnel verbunden sind und Datenverkehr zulässig ist.

Sie können verschiedene Back-End-Pools für verschiedene Arten von Anforderungen erstellen. Sie können z.B. einen Back-End-Pool für allgemeine Anforderungen und einen anderen für Anforderungen an die Microservices Ihrer Anwendung erstellen.

## <a name="health-probes"></a>Integritätstests

Das Anwendungsgateway überwacht standardmäßig die Integrität aller Ressourcen in seinem Back-End-Pool und entfernt automatisch alle als fehlerhaft geltenden Ressourcen aus dem Pool. Es überwacht die fehlerhaften Instanzen weiterhin und fügt sie dem fehlerfreien Back-End-Pool hinzu, sobald sie verfügbar sind und auf Integritätsüberprüfungen reagieren. Zusätzlich zur Nutzung der standardmäßigen Überwachung der Integritätsüberprüfung können Sie die Integritätsüberprüfung auch an die Anforderungen Ihrer Anwendung anpassen. Benutzerdefinierte Überprüfungen ermöglichen Ihnen eine präzisere Kontrolle über die Überwachung des Systemzustands. Bei Verwendung von benutzerdefinierten Überprüfungen können Sie das Überprüfungsintervall, die URL und den zu überprüfenden Pfad konfigurieren und festlegen, wie viele fehlerhafte Antworten akzeptiert werden, bevor die Back-End-Pool-Instanz als fehlerhaft gekennzeichnet wird. Es empfiehlt sich dringend, benutzerdefinierte Überprüfungen zu konfigurieren, um die Integrität jedes Back-End-Pools zu überwachen. Weitere Informationen finden Sie unter [Überwachen der Integrität Ihres Application Gateways](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über Application Gateway-Komponenten informiert haben, können Sie folgende Aktionen ausführen:
* [Erstellen eines Anwendungsgateways im Azure-Portal](quick-create-portal.md)
* [Erstellen eines Anwendungsgateways mit Azure PowerShell](quick-create-powershell.md)
* [Erstellen eines Anwendungsgateways mithilfe der Azure CLI](quick-create-cli.md)

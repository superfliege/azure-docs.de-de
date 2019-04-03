---
title: Azure Application Gateway – Konfigurationsübersicht
description: Dieser Artikel enthält Informationen zur Konfiguration der verschiedenen Komponenten in Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: absha
ms.openlocfilehash: 61b3a9e066a3ee20effa97f1c6c7a0bd1ae90ac0
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58285837"
---
# <a name="application-gateway-configuration-overview"></a>Application Gateway – Konfigurationsübersicht

Application Gateway besteht aus mehreren Komponenten, die in verschiedener Hinsicht konfiguriert werden können, um verschiedenen Szenarien Rechnung zu tragen. Dieser Artikel führt Sie durch die Konfiguration der einzelnen Komponenten.

![application-gateway-components](./media/configuration-overview/configuration-overview1.png)

Die Beispielabbildung oben veranschaulicht die Konfiguration einer Anwendung mit 3 Listenern. Die ersten zwei sind Listener für mehrere Standorte für `http://acme.com/*` bzw. `http://fabrikam.com/*`. Beide lauschen an Port 80. Der dritte Listener ist ein einfacher Listener mit End-to-End-SSL-Terminierung. 

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Virtuelles Azure-Netzwerk und dediziertes Subnetz

Application Gateway ist eine dedizierte Bereitstellung in Ihrem virtuellen Netzwerk. Innerhalb Ihres virtuellen Netzwerks ist ein dediziertes Subnetz für Ihr Application Gateway erforderlich. Sie können in diesem Subnetz mehrere Instanzen einer bestimmten Application Gateway-Bereitstellung betreiben. Sie können außerdem weitere Application Gateways im Subnetz bereitstellen, jedoch können Sie keine andere Ressource im Application Gateway-Subnetz bereitstellen.  

> [!NOTE]   
> Das Mischen von Standard_v2 und Standardanwendungsgateway im selben Subnetz wird nicht unterstützt.

#### <a name="size-of-the-subnet"></a>Subnetzgröße

Application Gateway nutzt eine private IP-Adresse pro Instanz sowie eine weitere private IP-Adresse, wenn eine private Front-End-IP-Konfiguration konfiguriert ist. Darüber hinaus reserviert Azure die ersten vier IP-Adressen sowie die letzte IP-Adresse in jedem Subnetz für die interne Verwendung. Wenn ein Application Gateway beispielsweise auf drei Instanzen ohne private Front-End-IP festgelegt ist, werden im Subnetz mindestens acht IP-Adressen benötigt – fünf IP-Adressen für den internen Gebrauch und drei IP-Adressen für die drei Instanzen des Application Gateways. Daher ist in diesem Fall mindestens ein Subnetz der Größe /29 erforderlich. Wenn Sie drei Instanzen und eine IP-Adresse für die Konfiguration der privaten Front-End-IP haben, sind neun IP-Adressen erforderlich – drei IP-Adressen für die drei Instanzen des Application Gateways, eine IP-Adresse als private Front-End-IP und fünf IP-Adressen für den internen Gebrauch. Daher ist in diesem Fall mindestens ein Subnetz der Größe /28 erforderlich.

Als bewährte Methode sollten Sie eine Subnetzgröße von mindestens /28 verwenden. Dadurch erhalten Sie 11 nutzbare Adressen. Wenn für Ihre Anwendungslast mehr als 10 Instanzen erforderlich sind, sollten Sie eine Subnetzgröße von /27 oder /26 in Betracht ziehen.

#### <a name="network-security-groups-supported-on-the-application-gateway-subnet"></a>Im Application Gateway-Subnetz unterstützte Netzwerksicherheitsgruppen

Netzwerksicherheitsgruppen (NSGs) werden im Application Gateway-Subnetz mit folgenden Einschränkungen unterstützt: 

- Ausnahmen müssen für eingehenden Datenverkehr an den Ports 65503 bis 65534 für die Application Gateway v1-SKU und an den Ports 65200 bis 65535 für die v2-SKU eingerichtet werden. Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich. Sie werden von Azure-Zertifikaten geschützt (gesperrt). Ohne entsprechende Zertifikate können externe Entitäten, einschließlich der Kunden dieser Gateways, keine Änderungen an diesen Endpunkten vornehmen.

- Die ausgehende Internetverbindung kann nicht blockiert sein. Standard-Ausgangsregeln in der NSG ermöglichen bereits Internetkonnektivität. Es empfiehlt sich, die Standard-Ausgangsregeln nicht zu entfernen und keine weiteren Ausgangsregeln zu erstellen, die keine ausgehende Internetkonnektivität zulassen.

- Datenverkehr vom AzureLoadBalancer-Tag muss zulässig sein.

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>Aufnehmen des Application Gateway-Zugriffs auf einige wenige Quell-IPs in eine Positivliste

Dieses Szenario können Sie mithilfe von Netzwerksicherheitsgruppen im Application Gateway-Subnetz umsetzen. Die folgenden Einschränkungen sollten für das Subnetz festgelegt werden (nach ihrer Priorität geordnet):

1. Zulassen des eingehenden Datenverkehrs von Quell-IP/-IP-Adressbereich.
2. Zulassen von eingehenden Anforderungen aus allen Quellen an den Ports 65503 65534 für die [Back-End-Integrität-Kommunikation](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich. Sie werden von Azure-Zertifikaten geschützt (gesperrt). Ohne entsprechende Zertifikate können externe Entitäten, einschließlich der Kunden dieser Gateways, keine Änderungen an diesen Endpunkten vornehmen.
3. Zulassen eingehender Azure Load Balancer-Tests (AzureLoadBalancer-Tag) und von eingehendem virtuellem Netzwerkdatenverkehr (VirtualNetwork-Tag) für die [Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Blockieren des gesamten übrigen eingehenden Datenverkehrs mit einer Alle-verweigern-Regel.
5. Zulassen von ausgehendem Datenverkehr an das Internet für alle Ziele.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Im Application Gateway-Subnetz unterstützte benutzerdefinierte Routen

Im Fall der v1 SKU werden benutzerdefinierte Routen (User-Defined Routes, UDRs) im Application Gateway-Subnetz unterstützt, solange sie die End-to-End-Anforderung/Antwort-Kommunikation nicht ändern. Beispielsweise können Sie eine UDR im Application Gateway-Subnetz einrichten, um für die Paketüberprüfung auf eine Firewallappliance zu verweisen, aber Sie müssen sicherstellen, dass das Paket die vorgesehene Zielnachprüfung erreichen kann. Ein Unterlassen kann zu einem falschen Integritätstest oder Datenverkehrsrouting-Verhalten führen. Dies schließt gelernte Routen oder standardmäßige 0.0.0.0/0-Routen ein, die durch ExpressRoute oder VPN-Gateways im virtuellen Netzwerk verteilt werden.

Im Fall der v2 SKU werden UDRs im Application Gateway-Subnetz nicht unterstützt. Weitere Informationen finden Sie unter [Automatische Skalierung und zonenredundantes Application Gateway (öffentliche Vorschau)](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#known-issues-and-limitations).

> [!NOTE]
> Die Verwendung von UDRs im Application Gateway-Subnetz führt dazu, dass der Integritätsstatus in der [Back-End-Integritätsansicht](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) als **Unbekannt** dargestellt wird und verhindert außerdem die Erstellung von Application Gateway-Protokollen und -Metriken. Es wird empfohlen, im Application Gateway-Subnetz keine UDRs zu verwenden, um die Back-End-Integrität, Protokolle und Metriken anzeigen zu können.

## <a name="frontend-ip"></a>Front-End-IP

Sie können das Application Gateway entweder mit einer öffentlichen IP-Adresse oder mit einer privaten IP-Adresse oder beidem konfigurieren. Öffentliche IP ist erforderlich, wenn Sie ein Back-End hosten, für das Clientzugriff über das Internet mithilfe einer VIP für Internetzugriff erforderlich ist. Öffentliche IP ist für interne Endpunkte, die nicht im Internet verfügbar gemacht werden und auch als interne Load Balancer-Endpunkte (ILB) bezeichnet werden, nicht erforderlich. Das Konfigurieren des Gateways mit einem ILB ist für interne Branchenanwendungen nützlich, die nicht für das Internet verfügbar gemacht werden. Es ist auch hilfreich für die Dienste und Ebenen in einer Anwendung mit mehreren Ebenen, die sich innerhalb einer Sicherheitsgrenze befinden und nicht für das Internet verfügbar gemacht werden, aber dennoch eine Round-Robin-Lastverteilung, Sitzungsbindungen oder SSL-Beendigung erfordern.

Es werden nur eine öffentliche IP-Adresse und eine private IP-Adresse unterstützt. Sie wählen die Front-End-IP beim Erstellen des Application Gateways aus. 

- Im Fall einer öffentlichen IP-Adresse können Sie wählen, ob Sie eine neue öffentliche IP-Adresse erstellen oder eine vorhandene öffentliche IP-Adresse am gleichen Speicherort wie das Application Gateway verwenden. Wenn Sie eine neue öffentliche IP-Adresse erstellen, kann der gewählte Typ der IP-Adresse (statisch oder dynamisch) später nicht mehr geändert werden. Weitere Informationen finden Sie unter [Static vs Dynamic Public IP](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-vs-dynamic-public-ip) (Gegenüberstellung von statischer und dynamischer öffentlicher IP-Adresse) 

- Im Fall einer privaten IP-Adresse können Sie sich für die Angabe einer privaten IP-Adresse aus dem Subnetz entscheiden, in dem das Application Gateway erstellt wird. Ohne explizite Angabe wird automatisch eine beliebige IP-Adresse aus dem Subnetz ausgewählt. Weitere Informationen finden Sie unter [Erstellen eines Application Gateways mit einem internen Lastenausgleichsendpunkt (ILB).](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm)

Eine Front-End-IP ist dem *Listener* zugeordnet, der auf eingehende Anforderungen über die Front-End-IP prüft.

## <a name="listeners"></a>Listener

Ein Listener ist eine logische Entität, die mithilfe von Port, Protokoll, Host und IP-Adresse auf eingehende Verbindungsanforderungen prüft. Beim Konfigurieren des Listeners müssen Sie daher diese Werte für Port, Protokoll, Host und IP-Adresse eingeben, die mit den entsprechenden Werten der eingehenden Anforderung im Gateway übereinstimmen. Wenn Sie ein Application Gateway mithilfe des Azure-Portals erstellen, erstellen Sie außerdem einen Standardlistener durch Auswählen von Protokoll und Port für den Listener. Sie können darüber hinaus wählen, ob Sie HTTP2-Unterstützung auf dem Listener aktivieren möchten. Nach der Erstellung des Application Gateways können Sie die Einstellung dieses Standardlisteners (*appGatewayHttpListener*/*appGatewayHttpsListener*) bearbeiten und/oder neue Listener erstellen.

### <a name="listener-type"></a>Listenertyp

Beim Erstellen eines neuen Listeners können Sie zwischen [einfachen Listenern oder Listenern für mehrere Standorte](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners) wählen. 

- Wenn Sie hinter einem Application Gateway eine einzelne Website hosten, wählen Sie den einfachen Listener. Mehr Informationen zum [Erstellen eines Application Gateways mit einem einfachen Listener](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Wenn Sie mehr als eine Webanwendung oder mehrere Unterdomänen der gleichen übergeordneten Domäne auf der gleichen Application Gateway-Instanz konfigurieren, wählen Sie den Listener für mehrere Standorte. Für einen Listener für mehrere Standorte müssen Sie außerdem einen Hostnamen eingeben. Das hat den Grund, dass Application Gateway HTTP 1.1-Hostheader verwendet, um mehrere Websites an der gleichen öffentlichen IP-Adresse und dem gleichen Port zu hosten.

#### <a name="order-of-processing-listeners"></a>Verarbeitungsreihenfolge von Listenern

Im Fall von v1 SKUs werden Listener gemäß der Anzeigereihenfolge verarbeitet. Wenn also ein grundlegender Listener zu einer eingehenden Anforderung passt, wird diese zuerst verarbeitet. Daher müssen Listener für mehrere Standorte vor einem einfachen Listener konfiguriert werden, um zu gewährleisten, dass der Datenverkehr an das richtige Back-End weitergeleitet wird.

Im Fall von v2 SKUs werden Listener für mehrere Standorte vor einfachen Listenern verarbeitet.

### <a name="frontend-ip"></a>Front-End-IP

Wählen Sie die Front-End-IP-Adresse, die Sie diesem Listener zuordnen möchten. Der Listener lauscht nach eingehenden Anforderungen unter dieser IP.

### <a name="frontend-port"></a>Front-End-Port

Wählen Sie den Front-End-Port aus. Sie können unter den vorhandenen Ports wählen oder einen neuen erstellen. Innerhalb des [zulässigen Portbereichs](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports) können Sie jeden beliebigen Wert auswählen. Dadurch können Sie nicht nur die bekannten Ports wie 80 und 443 verwenden, sondern jeden zulässigen benutzerdefinierten Port, der sich für Ihren Zweck eignet. Ein Port kann entweder für öffentlich zugängliche Listener oder für nur privat zugängliche Listener verwendet werden.

### <a name="protocol"></a>Protokoll

Sie müssen zwischen dem HTTP- und dem HTTPS-Protokoll wählen. 

- Wenn Sie sich für HTTP entscheiden, fließt der Datenverkehr zwischen dem Client und dem Application Gateway unverschlüsselt.

- Wählen Sie HTTPS, wenn Sie ein Interesse an [SSL-Terminierung (Secure Sockets Layer)](https://docs.microsoft.com/azure/application-gateway/overview#secure-sockets-layer-ssl-terminationl) oder [End-to-End-SSL-Verschlüsselung](https://docs.microsoft.com/azure/application-gateway/ssl-overview) haben. Wenn Sie HTTPS auswählen, ist der Datenverkehr zwischen dem Client und dem Application Gateway verschlüsselt, und die SSL-Verbindung ist beim Application Gateway terminiert.  Wenn Sie End-to-End-SSL-Verschlüsselung wünschen, müssen Sie das HTTPS-Protokoll darüber hinaus beim Konfigurieren der *Back-End-HTTP-Einstellung* wählen. Dadurch wird sichergestellt, dass der Datenverkehr auf dem Weg vom Application Gateway zum Back-End erneut verschlüsselt wird.

  Um SSL-Terminierung (Secure Sockets Layer) und End-to-End-SSL-Verschlüsselung zu konfigurieren, ist ein Zertifikat erforderlich, das dem Listener hinzugefügt wird, um dem Application Gateway die Ableitung eines symmetrischen Schlüssels gemäß SSL-Protokollspezifikation zu ermöglichen. Der symmetrische Schlüssel wird dann zum Verschlüsseln und Entschlüsseln des Datenverkehrs mit dem Gateway verwendet. Das Gatewayzertifikat muss im PFX-Format (Personal Information Exchange, privater Informationsaustausch) vorliegen. In diesem Dateiformat können Sie den privaten Schlüssel exportieren. Das ist erforderlich, damit das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs durchführen kann. 

#### <a name="supported-certificates"></a>Unterstützte Zertifikate

Informationen finden Sie unter [Unterstützte Zertifikate für die SSL-Terminierung](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Unterstützung zusätzlicher Protokolle

#### <a name="http2-support"></a>HTTP2-Unterstützung

Die Unterstützung des HTTP/2-Protokolls ist nur für Clients verfügbar, die mit Application Gateway-Listenern verbunden sind. Die Kommunikation mit Back-End-Serverpools erfolgt über HTTP/1.1. Die HTTP/2-Unterstützung ist standardmäßig deaktiviert. Das folgende Beispiel eines Azure PowerShell-Codeausschnitts zeigt, wie Sie sie aktivieren:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>WebSocket-Unterstützung

WebSocket-Unterstützung ist standardmäßig aktiviert. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. Sie können WebSockets sowohl mit HTTP- als auch mit HTTPS-Listenern verwenden. 

### <a name="custom-error-page"></a>Benutzerdefinierte Fehlerseite

Benutzerdefinierte Fehlerseiten können sowohl auf der globalen als auch auf der Listenerebene definiert werden, allerdings wird das Erstellen von benutzerdefinierten Fehlerseiten aus dem Azure-Portal zurzeit nicht unterstützt. Sie können auf Listenerebene eine benutzerdefinierte Fehlerseite für einen 403 WAF-Fehler oder eine 502-Wartungsseite konfigurieren. Sie müssen außerdem eine öffentlich zugängliche Blob-URL für den bestimmten Fehlerstatuscode angeben. Weitere Informationen finden Sie unter [Create custom error page](https://docs.microsoft.com/azure/application-gateway/custom-error) (Erstellen einer benutzerdefinierten Fehlerseite).

![Application Gateway-Fehlercodes](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Verwenden Sie zum Konfigurieren einer benutzerdefinierten globalen Fehlerseite [Azure PowerShell-Konfiguration](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration) 

### <a name="ssl-policy"></a>SSL-Richtlinie

Sie können die SSL-Zertifikatverwaltung zentralisieren sowie den Ver- und Entschlüsselungsaufwand für eine Back-End-Serverfarm verringern. Diese zentralisierte SSL-Behandlung ermöglicht auch die Angabe einer zentralen SSL-Richtlinie, die auf die Sicherheitsanforderungen Ihrer Organisation abgestimmt ist.  Sie können zwischen einer standardmäßigen, einer vordefinierten und einer benutzerdefinierten SSL-Richtlinie wählen. 

Sie können SSL-Richtlinien zum Steuern von SSL-Protokollversionen konfigurieren. Sie können Application Gateway zum Abweisen von TLS1.0, TLS1.1 und TLS1.2 konfigurieren. SSL 2.0 und 3.0 sind bereits in der Standardeinstellung deaktiviert und nicht konfigurierbar. Weitere Informationen finden Sie unter [Application Gateway SSL policy overview](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview) (Application Gateway SSL-Richtlinie – Übersicht).

Nach dem Erstellen eines Listeners ordnen Sie ihm eine Anforderungsroutingregel zu, die bestimmt, wie die am Listener empfangene Anforderung an das Back-End geroutet werden soll.

## <a name="request-routing-rule"></a>Anforderungsroutingregel

Beim Erstellen des Application Gateways mithilfe des Azure-Portals erstellen Sie eine Standardregel (*Regel1*), die den Standardlistener (*appGatewayHttpListener*) an den Standard-Back-End-Pool (*appGatewayBackendPool*) und die HTTP-Einstellungen des Standard-Back-Ends (*appGatewayBackendHttpSettings*) bindet. Nachdem das Application Gateway erstellt wurde, können Sie die Einstellung dieser Standardregel bearbeiten und/oder neue Regeln erstellen.

### <a name="rule-type"></a>Regeltyp

Beim Erstellen einer neuen Regel können Sie zwischen [einfachen oder pfadbasierten Regeln](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rule) wählen. 

- Wenn Sie alle Anforderungen am zugeordneten Listener (z.B.: blog.contoso.com/*) an einen einzelnen Back-End-Pool weiterleiten möchten, wählen Sie einen einfachen Listener. 
- Wählen Sie den pfadbasierten Listener, wenn Sie Anforderungen mit bestimmten URL-Pfaden an bestimmte Back-End-Pools routen möchten. Das Pfadmuster wird nur auf den Pfad der URL angewendet, nicht auf ihre Abfrageparameter.


#### <a name="order-of-processing-rules"></a>Verarbeitungsreihenfolge von Regeln

Im Fall von v1 SKUs wird die Musterzuordnung eingehender Anforderungen in der Reihenfolge verarbeitet, in der die Pfade in der URL-Pfadkarte der pfadbasierten Regel aufgelistet sind. Daher wird, wenn eine Anforderung den Mustern in zwei oder mehr Pfaden der URL-Pfadkarte entspricht, der zuerst aufgeführte Pfad zugeordnet, und die Anforderung wird an das diesem Pfad zugeordnete Back-End weitergeleitet.

Im Fall von v2 SKUs hat eine exakte Übereinstimmung eine höhere Priorität als die Reihenfolge, in der die Pfade in der URL-Pfadkarte aufgelistet sind. Daher wird eine Anforderung, wenn sie mit dem Muster in mehr als einem Pfad übereinstimmt, an das Back-End weitergeleitet, das dem Pfad zugeordnet ist, der exakt mit der Anforderung übereinstimmt. Wenn der Pfad in der eingehenden Anforderung mit keinem Pfad in der URL-Pfadkarte exakt übereinstimmt, dann erfolgt die Zuordnung des Musters der eingehenden Anforderung in der Reihenfolge, in der die Pfade in der URL-Pfadkarte der pfadbasierten Regel aufgelistet sind.

### <a name="associated-listener"></a>Zugeordneter Listener

Sie müssen der Regel einen Listener zuordnen, damit die *Anforderungsroutingregel*, die dem *Listener* zugeordnet ist, ausgewertet wird, um den *Back-End-Pool* zu bestimmen, an den die Anforderung geroutet werden soll.

### <a name="associated-backend-pool"></a>Zugeordneter Back-End-Pool

Ordnen Sie den Back-End-Pool zu, der die Back-End-Ziele enthält, die die vom Listener empfangenen Anforderungen bedienen sollen. Im Fall einer einfachen Regel ist nur ein Back-End-Pool zulässig, da alle Anforderungen am zugeordneten Listener an diesen Back-End-Pool weitergeleitet werden. Fügen Sie im Fall einer pfadbasierten Regel mehrere Back-End-Pools hinzu, die den einzelnen URL-Pfaden entsprechen. Die Anforderungen, die mit dem hier eingegebenen URL-Pfad übereinstimmen, werden an den entsprechenden Back-End-Pool weitergeleitet. Fügen Sie außerdem einen Standard-Back-End-Pool hinzu, da die Anforderungen, die mit keinem der in diese Regel eingegebenen URL-Pfade übereinstimmen, an ihn weitergeleitet werden.

### <a name="associated-backend-http-setting"></a>Zugeordnete Back-End-HTTP-Einstellung

Fügen Sie für jede Regel eine Back-End-HTTP-Einstellung hinzu. Die Anforderungen werden vom Application Gateway mithilfe der Portnummer, des Protokolls und der anderen in dieser Einstellung festgelegten Einstellungen an die Back-End-Ziele geroutet. Im Fall einer einfachen Regel ist nur eine Back-End-HTTP-Einstellung zulässig, da alle Anforderungen am zugeordneten Listener mithilfe dieser HTTP-Einstellung an die entsprechenden Back-End-Ziele weitergeleitet werden. Fügen Sie im Fall einer pfadbasierten Regel mehrere Back-End-HTTP-Einstellungen hinzu, die den einzelnen URL-Pfaden entsprechen. Die Anforderungen, die mit dem hier eingegebenen URL-Pfad übereinstimmen, werden mithilfe der den einzelnen URL-Pfaden entsprechenden HTTP-Einstellungen an die entsprechenden Back-End-Ziele weitergeleitet. Fügen Sie außerdem eine Standard-HTTP-Einstellung hinzu, da die Anforderungen, die mit keinem der in diese Regel eingegebenen URL-Pfade übereinstimmen, mithilfe der Standard-HTTP-Einstellung an den Standard-Back-End-Pool weitergeleitet werden.

### <a name="redirection-setting"></a>Umleitungseinstellung

Wenn die Umleitung für eine einfache Regel konfiguriert ist, werden alle Anforderungen am zugeordneten Listener zum Umleitungsziel umgeleitet, was eine globale Umleitung ermöglicht. Wenn die Umleitung für eine pfadbasierte Regel konfiguriert ist, werden nur die Anforderungen in einem bestimmten Websitebereich, beispielsweise einem Einkaufskorb, der durch /cart/* bezeichnet ist, an das Umleitungsziel umgeleitet, was pfadbasierte Umleitung ermöglicht. 

Informationen zur Umleitungsfunktionalität finden Sie unter [Umleitung – Übersicht](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

- #### <a name="redirection-type"></a>Umleitungstyp

  Wählen Sie den erforderlichen Typ der Umleitung unter diesen Optionen: Dauerhaft(301), Temporär(307), Gefunden(302) oder Verweis(303).

- #### <a name="redirection-target"></a>Umleitungsziel

  Sie können zwischen einem anderen Listener oder einer externen Website als Umleitungsziel wählen. 

  - ##### <a name="listener"></a>Listener

    Die Wahl eines Listeners als Umleitungsziel hilft dabei, von einem Listener auf dem Gateway zu einem anderen umzuleiten. Diese Einstellung ist erforderlich, wenn Sie die Umleitung von HTTP zu HTTPS aktivieren möchten, d.h. Datenverkehr vom Quelllistener, der auf eingehende HTTP-Anforderungen prüft, zum Ziellistener umleiten möchten, der auf eingehende HTTPS-Anforderungen prüft. Sie können auch festlegen, dass die Abfragezeichenfolge und der Pfad der ursprünglichen Anforderung in die Anforderung eingeschlossen werden, die an das Umleitungsziel weitergeleitet wird.![application-gateway-components](./media/configuration-overview/configure-redirection.png)

    Weitere Informationen zur HTTP-zu-HTTPS-Umleitung finden Sie unter [HTTP-zu-HTTP-Umleitung mithilfe des Portals](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal), [HTTP-zu-HTTP-Umleitung mithilfe von PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell), [HTTP-zu-HTTP-Umleitung mithilfe der CLI](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

  - ##### <a name="external-site"></a>Externe Website

    Wählen Sie „Externe Website“ aus, wenn Sie den Datenverkehr am Listener, der dieser Regel zugeordnet ist, auf eine externe Website umleiten möchten. Sie können festlegen, dass die Abfragezeichenfolge der ursprünglichen Anforderung in die Anforderung eingeschlossen werden, die an das Umleitungsziel weitergeleitet wird. Den Pfad der ursprünglichen Anforderung können Sie nicht an die externe Website weiterleiten.

    Weitere Informationen zur Umleitung an externe Websites finden Sie unter [redirect traffic to external site using PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell) (Umleiten von Datenverkehr an externe Websites mithilfe von PowerShell) und [https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-http-header-setting"></a>Umschreiben der HTTP-Headereinstellung

Diese Funktion ermöglicht Ihnen das Hinzufügen, Entfernen oder Aktualisieren von HTTP-Anforderungs- und -Antwortheadern, während die Anforderung/Antwort-Pakete zwischen dem Client und den Back-End-Pools verschoben werden.    Diese Funktion lässt sich nur mithilfe von PowerShell konfigurieren. Unterstützung für Portal und CLI ist noch nicht verfügbar. Weitere Informationen finden Sie unter [Erneutes Generieren von HTTP-Headern](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) (Übersicht) und [Konfigurieren des erneuten Schreibens von HTTP-Headern](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-your-http-header-rewrite-rule-configuration).

## <a name="http-settings"></a>HTTP-Einstellungen

Das Application Gateway routet Datenverkehr mithilfe der in dieser Komponente festgelegten Konfiguration an die Back-End-Server. Nachdem Sie eine HTTP-Einstellung erstellt haben, müssen Sie sie einer oder mehreren Regeln für das Routing von Anforderungen zuordnen.

### <a name="cookie-based-affinity"></a>Cookiebasierte Affinität

Dieses Feature ist nützlich, wenn eine Benutzersitzung auf dem gleichen Server bleiben soll. Mithilfe von durch das Gateway verwalteten Cookies kann das Anwendungsgateway weiteren Datenverkehr einer Benutzersitzung zur Verarbeitung an den gleichen Server weiterleiten. Dies ist hilfreich, wenn der Sitzungsstatus für eine Benutzersitzung lokal auf dem Server gespeichert wird. Wenn die Anwendung cookiebasierte Affinität nicht verarbeiten kann, können Sie diese Funktion nicht verwenden. Um cookiebasierte Sitzungsaffinität zu verwenden, müssen Sie sicherstellen, dass die Clients Cookies unterstützen. 

### <a name="connection-draining"></a>Verbindungsausgleich

Mit dem Verbindungsausgleich können Sie eine korrekte Entfernung von Mitgliedern des Back-End-Pools bei geplanten Dienstupdates erzielen. Diese Einstellung kann bei der Erstellung einer Regel auf alle Mitglieder eines Back-End-Pools angewendet werden. Nach der Aktivierung stellt Application Gateway sicher, dass alle Instanzen eines Back-End-Pools, deren Registrierung aufgehoben wird, keine neuen Anforderungen mehr erhalten, während vorhandene Anforderungen innerhalb eines konfigurierten Zeitlimits abgeschlossen werden können. Dies gilt sowohl für Back-End-Instanzen, die per API-Aufruf explizit aus dem Back-End-Pool entfernt werden, als auch für Back-End-Instanzen, die von den Integritätstests als fehlerhaft gemeldet werden.

### <a name="protocol"></a>Protokoll

Application Gateway unterstützt sowohl HTTP- als auch HTTPS-Protokolle für das Routing von Anforderungen an die Back-End-Server. Wenn das HTTP-Protokoll gewählt wird, fließt der Datenverkehr unverschlüsselt an die Back-End-Server. In Fällen, in denen die unverschlüsselte Kommunikation mit den Back-End-Servern keine akzeptable Option darstellt, sollten Sie das HTTPS-Protokoll wählen. Diese Einstellung ermöglicht Ihnen in Kombination mit der Wahl des HTTPS-Protokolls im Listener das Aktivieren von [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Damit können Sie vertrauliche Daten sicher verschlüsselt an das Back-End übertragen. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-SSL muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

### <a name="port"></a>Port

Dies ist der Port, an dem die Back-End-Server auf den vom Application Gateway kommenden Datenverkehr lauschen. Sie können Ports im Bereich von 1 bis 65535 konfigurieren.

### <a name="request-timeout"></a>Anforderungszeitlimit

Die Anzahl der Sekunden, die das Application Gateway auf Antwort vom Back-End-Pool wartet, bevor es einen „Connection timed out“-Fehler zurückgibt.

### <a name="override-backend-path"></a>Back-End-Pfad außer Kraft setzen

Mit dieser Einstellung können Sie einen optionalen benutzerdefinierten Weiterleitungspfad konfigurieren, der für die Weiterleitung der Anforderung an das Back-End verwendet werden soll. Damit wird ein beliebiger Teil des Eingangspfads, der mit dem benutzerdefinierten Pfad übereinstimmt, der im Feld **Back-End-Pfad außer Kraft setzen** angegeben ist, in den weitergeleiteten Pfad kopiert. Aus der Tabelle unten können Sie ersehen, wie diese Funktion arbeitet.

- Wenn die HTTP-Einstellung einer einfachen Routingregel für Anforderungen zugeordnet ist:

  | Ursprüngliche Anforderung  | Back-End-Pfad außer Kraft setzen | An das Back-End weitergeleitete Anforderung |
  | ----------------- | --------------------- | ---------------------------- |
  | /home/            | /override/            | /override/home/              |
  | /home/secondhome/ | /override/            | /override/home/secondhome/   |

- Wenn die HTTP-Einstellung einer pfadbasierten Routingregel für Anforderungen zugeordnet ist:

  | Ursprüngliche Anforderung           | Pfadregel       | Back-End-Pfad außer Kraft setzen | An das Back-End weitergeleitete Anforderung |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home/            | /pathrule*      | /override/            | /override/home/              |
  | /pathrule/home/secondhome/ | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /home/                     | /pathrule*      | /override/            | /override/home/              |
  | /home/secondhome/          | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /pathrule/home/            | /pathrule/home* | /override/            | /override/                   |
  | /pathrule/home/secondhome/ | /pathrule/home* | /override/            | /override/secondhome/        |

### <a name="use-for-app-service"></a>Für App Service verwenden

Dies ist eine Verknüpfung der Benutzeroberfläche, die die zwei erforderlichen Einstellungen für das App Service-Back-End auswählt – der Hostname kann aus der Back-End-Adresse ausgewählt werden, und es wird ein neuer benutzerdefinierter Test erstellt. Der Grund für die erste Aktion wird im Abschnitt zur Einstellung **Hostnamen aus Back-End-Adresse auswählen** erläutert. Es wird ein neuer Test erstellt, dessen Header ebenfalls aus der Adresse des Back-End-Mitglieds ausgewählt wird.

### <a name="use-custom-probe"></a>Benutzerdefinierten Test verwenden

Diese Einstellung dient dazu, der HTTP-Einstellung einen [benutzerdefinierten Test](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) zuzuordnen. Einer HTTP-Einstellung kann nur ein benutzerdefinierter Test zugeordnet werden. Wenn Sie nicht explizit einen benutzerdefinierten Test zuordnen, wird der [Standardtest](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) zur Überwachung der Integrität des Back-Ends verwendet. Es empfiehlt sich, einen benutzerdefinierten Test zu erstellen, um eine feiner abgestufte Kontrolle über die Integritätsüberwachung Ihrer Back-Ends zu haben.

> [!NOTE]   
> Der benutzerdefinierte Test beginnt nicht mit der Überwachung der Integrität des Back-End-Pools, sofern die entsprechende HTTP-Einstellung nicht explizit einem Listener zugeordnet ist.

### <a name="pick-host-name-from-backend-address"></a>Hostnamen aus Back-End-Adresse auswählen

Diese Funktion legt den *Host*-Header in der Anforderung auf den Hostnamen des Back-End-Pools fest und verwendet dazu eine IP-Adresse oder einen vollqualifizierten Domänennamen (FQDN). Dies ist in Szenarien nützlich, in denen der Domänenname des Back-Ends vom DNS-Namen des Application Gateways abweicht und sich das Back-End für die Auflösung des richtigen Endpunks auf einen bestimmten Hostheader oder eine SNI-Erweiterung stützt, wie es etwa bei mandantenübergreifenden Diensten auf dem Back-End der Fall ist. Da es sich bei App Service um einen mehrmandantenfähigen Dienst mit einem gemeinsamen Bereich und einer einzelnen IP-Adresse handelt, kann auf einen App Service nur mithilfe der Hostnamen zugegriffen werden, die in den benutzerdefinierten Domäneneinstellungen konfiguriert sind. Standardmäßig ist der benutzerdefinierte Domänenname *example.azurewebsites.net*. Wenn Sie daher mithilfe von Application Gateway auf Ihren App Service zugreifen möchten und der Hostname nicht explizit beim App Service registriert ist oder der Zugriff über den FQDN des Application Gateways erfolgen soll, müssen Sie den Hostnamen in der ursprünglichen Anforderung mit dem Hostnamen des App Service überschreiben, indem Sie die Einstellung **Hostnamen aus Back-End-Adresse auswählen** aktivieren.

Wenn Sie eine benutzerdefinierte Domäne besitzen und den vorhandenen benutzerdefinierten DNS-Namen dem App-Service zugeordnet haben, brauchen Sie diese Einstellung nicht zu aktivieren.

> [!NOTE]   
> Diese Einstellung ist für die App Service-Umgebung (App Service Environment, ASE) nicht erforderlich, da es sich bei ASE um eine dedizierte Bereitstellung handelt. 

### <a name="host-name-override"></a>Hostnamen außer Kraft setzen

Diese Funktion ersetzt den *Host*-Header in der beim Application Gateway eingehenden Anforderung durch den Hostnamen, den Sie hier angeben. Wenn beispielsweise www\.contoso.com als Einstellung für den **Hostnamen** angegeben ist, wird die ursprüngliche Anforderung https://appgw.eastus.cloudapp.net/path1 beim Weiterleiten der Anforderung an den Back-End-Server in https://www.contoso.com/path1 geändert. 

## <a name="backend-pool"></a>Back-End-Pool

Ein Back-End-Pool kann auf vier Typen von Back-End-Elementen verweisen: einen bestimmten virtuellen Computer, eine VM-Skalierungsgruppe, eine IP-Adresse/einen FQDN oder einen App Service. Jeder Back-End-Pool kann auf mehrere Elemente des gleichen Typs verweisen. Das Verweisen auf Elemente verschiedenen Typs im gleichen Back-End-Pool wird nicht unterstützt. 

Nach dem Erstellen eines Back-End-Pools müssen Sie ihn einer oder mehreren Routingregeln für Anforderungen zuordnen. Ferner müssen Sie Integritätstests für jeden Back-End-Pool in Ihrem Application Gateway konfigurieren. Wenn eine Bedingung einer Anfoderungsroutingregel erfüllt ist, leitet das Application Gateway den Datenverkehr an die fehlerfreien Server (die durch die Integritätstests bestimmt werden) im entsprechenden Back-End-Pool weiter.

## <a name="health-probes"></a>Integritätstests

Obwohl Application Gateway die Integrität aller Ressourcen in seinem Back-End standardmäßig überwacht, wird dringend empfohlen, einen benutzerdefinierten Test für die HTTP-Einstellung jedes einzelnen Back-Ends zu erstellen, um eine feiner abgestufte Kontrolle über die Integritätsüberwachung zu erhalten. Informationen zum Konfigurieren eines benutzerdefinierten Integritätstests finden Sie unter [Einstellungen für die benutzerdefinierte Integritätsüberprüfung](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]   
> Nachdem Sie einen benutzerdefinierten Integritätstest erstellt haben, müssen Sie ihn einer Back-End-HTTP-Einstellung zuordnen. Der benutzerdefinierte Test beginnt nicht mit der Überwachung der Integrität des Back-End-Pools, sofern die entsprechende HTTP-Einstellung nicht explizit einem Listener zugeordnet ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über Application Gateway-Komponenten informiert haben, können Sie folgende Aktionen ausführen:

- [Erstellen eines Anwendungsgateways im Azure-Portal](quick-create-portal.md)
- [Erstellen eines Anwendungsgateways mit PowerShell](quick-create-powershell.md)
- [Erstellen eines Anwendungsgateways mithilfe der Azure CLI](quick-create-cli.md)

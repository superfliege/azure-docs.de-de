---
title: "Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten"
description: "Hier erfahren Sie, wie Sie Azure Stack-Endpunkte in Ihrem Datencenter veröffentlichen."
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/18/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: 0d15252079b62f6a74a1279309fb9b1b3ed5711e
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten

*Gilt für: Integrierte Azure Stack-Systeme*

Azure Stack richtet für die eigenen Infrastrukturrollen verschiedene Endpunkte (virtuelle IP-Adressen, VIPs) ein. Diese VIPs stammen aus dem öffentlichen IP-Adresspool. Jede VIP wird durch eine Zugriffssteuerungsliste (Access Control List, ACL) auf der softwaredefinierten Netzwerkebene geschützt. Für zusätzlichen Schutz werden außerdem übergreifende ACLs für die physischen Switches (TORs und BMC) verwendet. Für jeden Endpunkt in der externen DNS-Zone, die zum Zeitpunkt der Bereitstellung angegeben wurde, wird ein DNS-Eintrag erstellt.


Das folgende Architekturdiagramm zeigt die verschiedenen Netzwerkebenen und ACLs:

![Architekturdiagramm](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Ports und Protokolle (eingehend)

Die folgende Tabelle gibt Aufschluss über die Infrastruktur-VIPs, die für die Veröffentlichung von Azure Stack-Endpunkten für externe Netzwerke benötigt werden. Die Liste enthält jeweils den Endpunkt, den erforderlichen Port und das Protokoll. Informationen zu erforderlichen Endpunkten für zusätzliche Ressourcenanbieter (etwa für den SQL-Ressourcenanbieter) finden Sie in der Bereitstellungsdokumentation des jeweiligen Ressourcenanbieters.

Interne Infrastruktur-VIPs sind nicht aufgeführt, da sie zum Veröffentlichen von Azure Stack nicht benötigt werden.

> [!NOTE]
> Benutzer-VIPs sind dynamisch und werden von den Benutzern selbst definiert. Der Azure Stack-Betreiber hat darauf keinen Einfluss.


|Endpunkt (VIP)|A-Eintrag des DNS-Hosts|Protocol|Ports|
|---------|---------|---------|---------|
|AD FS|`Adfs.[Region].[External FQDN]`|HTTPS|443|
|Portal (Administrator)|`Adminportal.[Region].[External FQDN]`|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (Administrator)|`Adminmanagement.[Region].[External FQDN]`|HTTPS|443<br>30024|
|Portal (Benutzer)|`Portal. [Region].[External FQDN]`|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (Benutzer)|`Management.[Region].[External FQDN]`|HTTPS|443<br>30024|
|Graph|`Graph.[Region].[External FQDN]`|HTTPS|443|
|Zertifikatsperrliste|`Crl.[Region].[External FQDN]`|HTTP|80|
|DNS|`*.[Region].[External FQDN]`|TCP und UDP|53|
|Key Vault (Benutzer)|`*.vault.[Region].[External FQDN]`|TCP|443|
|Key Vault (Administrator)|`*.adminvault.[Region].[External FQDN]`|TCP|443|
|Speicherwarteschlange|`*.queue.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|
|Speichertabelle|`*.table.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|
|Speicherblob|`*.blob.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|

## <a name="ports-and-urls-outbound"></a>Ports und URLs (ausgehend)

Azure Stack unterstützt nur transparente Proxyserver. In einer Bereitstellung mit einem Uplink zwischen einem transparenten Proxy und einem herkömmlichen Proxyserver müssen für die ausgehende Kommunikation die folgenden Ports und URLs zugelassen werden:


|Zweck|URL|Protocol|Ports|
|---------|---------|---------|---------|
|Identity|`login.windows.net`<br>`login.microsoftonline.com`<br>`graph.windows.net`|HTTP<br>HTTPS|80<br>443|
|Marketplace-Syndikation|`https://management.azure.com`<br>`https://*.blob.core.windows.net`<br>`https://*.azureedge.net`<br>`https://*.microsoftazurestack.com`|HTTPS|443|
|Patches und Updates|`https://*.azureedge.net`|HTTPS|443|
|Registrierung|`https://management.azure.com`|HTTPS|443|
|Verwendung|`https://*.microsoftazurestack.com`<br>`https://*.trafficmanager.com`|HTTPS|443|

## <a name="firewall-publishing"></a>Firewall-Veröffentlichung

Beim Veröffentlichen von Azure Stack-Diensten durch eine vorhandene Firewall gelten die Ports aus dem vorherigen Abschnitt für die eingehende Kommunikation.

Es empfiehlt sich, Azure Stack mit einem Firewallgerät zu schützen. Dies ist jedoch nicht zwingend erforderlich. Firewalls können zwar bei DDoS-Angriffen und bei der Inhaltsuntersuchung hilfreich sein, andererseits können sie sich aber auch als Engpass für den Durchsatz von Azure-Speicherdiensten wie Blobs, Tabellen und Warteschlangen erweisen.

Ob eine Veröffentlichung des AD FS-Endpunkts erforderlich ist, hängt vom Identitätsmodell (Azure AD oder AD FS) ab. Bei Verwendung eines getrennten Bereitstellungsmodus muss der AD FS-Endpunkt veröffentlicht werden. (Weitere Informationen finden Sie im Thema zur Datencenter-Integrationsidentität.)

Für die Endpunkte „Azure Resource Manager (Administrator)“, „Portal (Administrator)“ und „Schlüsseltresor (Administrator)“ ist nicht zwingend eine externe Veröffentlichung erforderlich. Dies hängt vom Szenario ab. Als Dienstanbieter können Sie beispielsweise die Angriffsfläche verkleinern und Azure Stack nur innerhalb Ihres Netzwerks (und nicht über das Internet) verwalten.

Bei einem Unternehmen kann das Unternehmensnetzwerk als externes Netzwerk fungieren. In einem solchen Szenario müssen Sie die Endpunkte veröffentlichen, um Azure Stack über das Unternehmensnetzwerk bedienen zu können.

## <a name="edge-firewall-scenario"></a>Szenario mit Edgefirewall

In einer Edgebereitstellung wird Azure Stack direkt hinter dem (vom ISP bereitgestellten) Edgerouter bereitgestellt. Bei Bedarf kann davor noch eine Firewall platziert werden.

![Architekturdiagramm einer Edgebereitstellung von Azure Stack](media/azure-stack-integrate-endpoints/Integrate-Endpoints-02.png)

In der Regel werden in einer Edgebereitstellung zum Zeitpunkt der Bereitstellung routingfähige IP-Adressen für den öffentlichen VIP-Pool angegeben. Dadurch erhalten Benutzer eine vollständig selbstgesteuerte Cloudumgebung – genau wie bei einer öffentlichen Cloud (beispielsweise Azure).

### <a name="using-nat"></a>Verwenden der NAT

Sie können zwar eine Netzwerkadressenübersetzung (Network Address Translation, NAT) zur Veröffentlichung von Endpunkten verwenden, dies wird jedoch aufgrund des Zusatzaufwands nicht empfohlen. Für eine vollständig benutzergesteuerte Endpunktveröffentlichung benötigt jede Benutzer-VIP eine NAT-Regel, die alle Ports enthält, die ein Benutzer möglicherweise verwendet.

Ein weiterer Punkt: Die Einrichtung eines VPN-Tunnels zu einem Endpunkt mit NAT in einem Hybrid Cloud-Szenario mit Azure wird nicht unterstützt.

## <a name="enterpriseintranetperimeter-network-firewall-scenario"></a>Firewallszenario mit Unternehmensnetzwerk/Intranet/Umkreisnetzwerk

In einer Bereitstellung mit Unternehmensnetzwerk/Intranet/Umkreisnetzwerk wird Azure Stack jenseits einer zweiten Firewall bereitgestellt, die üblicherweise einem Umkreisnetzwerk (auch DMZ genannt) angehört.

![Azure Stack-Firewallszenario](media/azure-stack-integrate-endpoints/Integrate-Endpoints-03.png)

Wenn für den öffentlichen VIP-Pool von Azure Stack öffentliche routingfähige IP-Adresse angegeben wurden, gehören diese Adressen logisch zum Umkreisnetzwerk und erfordern Veröffentlichungsregeln in der primären Firewall.

### <a name="using-nat"></a>Verwenden der NAT

Wenn für den öffentlichen VIP-Pool von Azure Stack nicht öffentliche routingfähige IP-Adresse verwendet werden, wird zur Veröffentlichung von Azure Stack-Endpunkten die NAT an der sekundären Firewall verwendet. In diesem Szenario müssen Sie die Veröffentlichungsregeln in der primären Firewall jenseits der Edge sowie in der sekundären Firewall konfigurieren. Wenn Sie die NAT verwenden möchten, berücksichtigen Sie Folgendes:

- Durch die NAT erhöht sich der Verwaltungsaufwand für Firewallregeln, da Benutzer ihre eigenen Endpunkte und ihre eigenen Veröffentlichungsregeln im SDN-Stapel (Software Defined Networking) steuern. Benutzer müssen sich zur Veröffentlichung ihrer VIPs sowie zur Aktualisierung der Portliste an den Azure Stack-Betreiber wenden.
- Die Verwendung der NAT beeinträchtigt zwar die Benutzerfreundlichkeit, gibt dem Betreiber jedoch die volle Kontrolle über Veröffentlichungsanforderungen.
- In Hybrid Cloud-Szenarien mit Azure wird die Einrichtung eines VPN-Tunnels zu einem Endpunkt mit NAT nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte

[Integration des Azure Stack-Datencenters: Sicherheit](azure-stack-integrate-security.md)
---
title: Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Stack-Endpunkte in Ihrem Datencenter veröffentlichen.
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 07/16/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: ''
ms.openlocfilehash: bfb50a3e1b2276a63d79f5a6b8b1ab721f039d9d
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090549"
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten
Azure Stack richtet für die eigenen Infrastrukturrollen virtuelle IP-Adressen (VIPs) ein. Diese VIPs stammen aus dem öffentlichen IP-Adresspool. Jede VIP wird durch eine Zugriffssteuerungsliste (Access Control List, ACL) auf der softwaredefinierten Netzwerkebene geschützt. Für zusätzlichen Schutz werden außerdem übergreifende ACLs für die physischen Switches (TORs und BMC) verwendet. Für jeden Endpunkt in der externen DNS-Zone, die zum Zeitpunkt der Bereitstellung angegeben wurde, wird ein DNS-Eintrag erstellt.


Das folgende Architekturdiagramm zeigt die verschiedenen Netzwerkebenen und ACLs:

![Strukturbild](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Ports und Protokolle (eingehend)

Eine Reihe von Infrastruktur-VIPs wird für die Veröffentlichung von Azure Stack-Endpunkten in externen Netzwerken benötigt. Die Tabelle *Endpunkt (VIP)* enthält jeweils den Endpunkt, den erforderlichen Port und das Protokoll. Informationen zu Endpunkten, für die zusätzliche Ressourcenanbieter (etwa der SQL-Ressourcenanbieter) erforderlich sind, finden Sie in der Bereitstellungsdokumentation des jeweiligen Ressourcenanbieters.

Interne Infrastruktur-VIPs sind nicht aufgeführt, da sie zum Veröffentlichen von Azure Stack nicht benötigt werden.

> [!NOTE]
> Benutzer-VIPs sind dynamisch und werden von den Benutzern selbst definiert. Der Azure Stack-Betreiber hat darauf keinen Einfluss.


|Endpunkt (VIP)|A-Eintrag des DNS-Hosts|Protokoll|Ports|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (Administrator)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (Administrator)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (Benutzer)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (Benutzer)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graph|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Zertifikatsperrliste|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP und UDP|53|
|Key Vault (Benutzer)|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Key Vault (Administrator)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Speicherwarteschlange|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Speichertabelle|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Speicherblob|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|SQL-Ressourcenanbieter|sqladapter.dbadapter.*&lt;Region>.&lt;FQDN>*|HTTPS|44300-44304|
|MySQL-Ressourcenanbieter|mysqladapter.dbadapter.*&lt;Region>.&lt;FQDN>*|HTTPS|44300-44304|
|App Service|&#42;.appservice.*&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)<br>44300 (Azure Resource Manager)|
|  |ftp.appservice.*&lt;region>.&lt;fqdn>*|TCP, UDP|21, 1021, 10001-10100 (FTP)<br>990 (FTPS)|
|VPN-Gateways|     |     |Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability).|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>Ports und URLs (ausgehend)

Azure Stack unterstützt nur transparente Proxyserver. In einer Bereitstellung mit einem Uplink zwischen einem transparenten Proxy und einem herkömmlichen Proxyserver müssen für die ausgehende Kommunikation die folgenden Ports und URLs zugelassen werden:


|Zweck|URL|Protokoll|Ports|
|---------|---------|---------|---------|
|Identity|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net<br>https://secure.aadcdn.microsoftonline-p.com|HTTP<br>HTTPS|80<br>443|
|Marketplace-Syndikation|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patches und Updates|https://&#42;.azureedge.net|HTTPS|443|
|Registrierung|https://management.azure.com|HTTPS|443|
|Verwendung|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.net|HTTPS|443|
|Windows Defender|.wdcp.microsoft.com<br>.wdcpalt.microsoft.com<br>*.updates.microsoft.com<br>*.download.microsoft.com<br>https://msdl.microsoft.com/download/symbols<br>http://www.microsoft.com/pkiops/crl<br>http://www.microsoft.com/pkiops/certs<br>http://crl.microsoft.com/pki/crl/products<br>http://www.microsoft.com/pki/certs<br>https://secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|
|NTP|     |UDP|123|
|DNS|     |TCP<br>UDP|53|
|     |     |     |     |



## <a name="next-steps"></a>Nächste Schritte

[PKI-Anforderungen für Azure Stack](azure-stack-pki-certs.md)

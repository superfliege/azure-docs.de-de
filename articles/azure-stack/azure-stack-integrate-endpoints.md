---
title: "Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten"
description: "Hier erfahren Sie, wie Sie Azure Stack-Endpunkte in Ihrem Datencenter veröffentlichen."
services: azure-stack
author: jeffgilb
ms.service: azure-stack
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: 
ms.openlocfilehash: e368109adc7db4c589ac37b28c4891cb3ec5346f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
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


|Endpunkt (VIP)|A-Eintrag des DNS-Hosts|Protokoll|Ports|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (Administrator)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (Administrator)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (Benutzer)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
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
|MySQL-Ressourcenanbieter|mysqladapter.dbadapter.*&lt;Region>.&lt;FQDN>*|HTTPS|44300-44304

## <a name="ports-and-urls-outbound"></a>Ports und URLs (ausgehend)

Azure Stack unterstützt nur transparente Proxyserver. In einer Bereitstellung mit einem Uplink zwischen einem transparenten Proxy und einem herkömmlichen Proxyserver müssen für die ausgehende Kommunikation die folgenden Ports und URLs zugelassen werden:


|Zweck|URL|Protokoll|Ports|
|---------|---------|---------|---------|
|Identity|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Marketplace-Syndikation|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patches und Updates|https://&#42;.azureedge.net|HTTPS|443|
|Registrierung|https://management.azure.com|HTTPS|443|
|Verwendung|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|


## <a name="next-steps"></a>Nächste Schritte
[PKI-Anforderungen für Azure Stack](azure-stack-pki-certs.md)
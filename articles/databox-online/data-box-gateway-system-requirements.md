---
title: Systemanforderungen für Microsoft Azure Data Box Gateway | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Software- und Netzwerkanforderungen für Azure Data Box Gateway.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 915190f6f2773d22d14c75be1140f96b7dd40559
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991571"
---
# <a name="azure-data-box-gateway-system-requirements-preview"></a>Systemanforderungen für Microsoft Azure Data Box Gateway (Vorschauversion)

In diesem Artikel werden wichtige Systemanforderungen für Ihre Microsoft Azure Data Box Gateway-Lösung und die Clients beschrieben, die mit Azure Data Box Gateway verbunden sind. Sie sollten die Informationen sorgfältig lesen, bevor Sie Data Box Gateway bereitstellen. Auch später sollten Sie während der Bereitstellung und beim nachfolgenden Betrieb bei Bedarf als Referenz darauf zurückgreifen.

Für das virtuelle Data Box Gateway-Gerät gelten die folgenden Systemanforderungen:

- **Softwareanforderungen für Hosts**: Beschreibt die unterstützten Plattformen, Browser für die lokale Webbenutzeroberfläche, SMB-Clients und alle zusätzlichen Anforderungen an Hosts, die sich mit dem Gerät verbinden.
- **Netzwerkanforderungen für das Gerät**: Liefert Informationen zu den Netzwerkanforderungen für den Betrieb des Geräts.

> [!IMPORTANT]
> Data Box Gateway ist in der Vorschauphase. Lesen Sie [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen. 

## <a name="supported-os-for-clients-connected-to-device"></a>Unterstütztes Betriebssystem für Clients, die mit dem Gerät verbunden sind

Im Folgenden finden Sie eine Liste der unterstützten Betriebssysteme für Clients, die mit Data Box Gateway verbunden sind.

| **Betriebssystem/-plattform** | **Versionen** |
| --- | --- |
| Windows Server |2012 R2 <br> 2016 |
| Windows |8, 10 |
| SUSE Linux |Enterprise Server 12 (x86_64)|
| Ubuntu |16.04.3 LTS|
| CentOS | 7.0 |

## <a name="supported-protocols-for-clients-accessing-device"></a>Unterstützte Protokolle für Clients, die auf das Gerät zugreifen

|**Protokoll** |**Versionen**   |**Hinweise**  |
|---------|---------|---------|
|SMB    | 2.X, 3.X      | SMB 1 wird nicht unterstützt.|
|NFS     | V3 und V4        |         |

## <a name="supported-virtualization-platforms-for-device"></a>Unterstützte Virtualisierungsplattformen für das Gerät

| **Betriebssystem/-plattform**  |**Versionen**   |**Hinweise**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6,0 <br> 6,5        |VMware-Tools werden nicht unterstützt.         |


## <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

Im Folgenden finden Sie eine Liste der unterstützten Speichertypen für Data Box Gateway.

| **Speicherkonto** | **Hinweise** |
| --- | --- |
| Klassisch | Standard |
| Allgemeiner Zweck  |Standard: sowohl V1 als auch V2 werden unterstützt. Es werden sowohl „heiße“ als auch „kalte“ Ebenen unterstützt. |


## <a name="supported-storage-types"></a>Unterstützte Speichertypen

Im Folgenden finden Sie eine Liste der unterstützten Speichertypen für Data Box Gateway.

| **Dateiformat** | **Hinweise** |
| --- | --- |
| Azure-Blockblob | |
| Azure-Seitenblob  | |
| Azure Files | |

## <a name="supported-browsers-for-local-web-ui"></a>Unterstützte Browser für die lokale Webbenutzeroberfläche

Es folgt eine Liste der für die lokale Webbenutzeroberfläche des virtuellen Geräts unterstützten Webbrowser.

|Browser  |Versionen  |Weitere Anforderungen/Hinweise  |
|---------|---------|---------|
|Google Chrome   |Aktuelle Version         |         |
|Microsoft Edge    | Aktuelle Version        |         |
|Internet Explorer     | Aktuelle Version        |         |
|Firefox    |Aktuelle Version         |         |


## <a name="networking-requirements"></a>Netzwerkanforderungen

In der folgenden Tabelle sind die Ports aufgeführt, die in der Firewall für SMB-, Cloud- oder Verwaltungsdatenverkehr geöffnet werden müssen. In dieser Tabelle bezieht sich *ein* oder *eingehend* auf die Richtung, aus der eingehende Clientanforderungen auf das Gerät zugreifen. Entsprechend bezieht sich *aus* oder *ausgehend* auf die Richtung, in der das Data Box Gateway-Gerät Daten über die Bereitstellung hinaus an externe Ziele sendet: z.B. ausgehende Verbindungen mit dem Internet.

| Port-Nr.| ein oder aus | Portbereich| Erforderlich|   Notizen                                                             |                                                                                     |
|--------|---------|----------|--------------|----------------------|---------------|
| TCP 80 (HTTP)|aus|WAN |Nein |Der ausgehende Port wird für den Internetzugriff zum Abrufen von Updates verwendet. <br>Der ausgehende Webproxy kann vom Benutzer konfiguriert werden. |                          
| TCP 443 (HTTPS)|aus|WAN|JA|Der ausgehende Port wird für den Zugriff auf Daten in der Cloud verwendet.<br>Der ausgehende Webproxy kann vom Benutzer konfiguriert werden.|   
| UDP 53 (DNS)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen internetbasierten DNS-Server verwenden.<br>Es wird empfohlen, den lokalen DNS-Server zu verwenden. |
| UDP 123 (NTP)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen internetbasierten NTP-Server verwenden.  |
| UDP 67 (DHCP)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen DHCP-Server verwenden.  |
| TCP 80 (HTTP)|Geben Sie in|LAN|JA|Dies ist der eingehende Port für die lokale Benutzeroberfläche auf dem Gerät für die lokale Verwaltung. <br>Beim Zugriff auf die lokale Benutzeroberfläche über HTTP erfolgt automatisch eine Umleitung auf HTTPS.  | 
| TCP 443 (HTTPS)|Geben Sie in|LAN|JA|Dies ist der eingehende Port für die lokale Benutzeroberfläche auf dem Gerät für die lokale Verwaltung. | 

## <a name="url-patterns-for-firewall-rules"></a>URL-Muster für Firewallregeln

Netzwerkadministratoren können häufig erweiterte, auf den URL-Mustern basierende Firewallregeln konfigurieren, die zum Filtern des eingehenden und ausgehenden Verkehrs verwendet werden. Ihr Data Box Gateway-Gerät und der Data Box Gateway-Dienst hängen von anderen Microsoft-Anwendungen wie Azure Service Bus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern ab. Die URL-Muster, die diesen Anwendungen zugeordnet sind, können verwendet werden, um Firewallregeln zu konfigurieren. Es ist wichtig, zu verstehen, dass sich diese den Anwendungen zugeordneten URL-Muster ändern können. Das bedeutet, dass der Netzwerkadministrator die Firewallregeln für Ihr Data Box Gateway bei Bedarf überwachen und aktualisieren muss.

Es empfiehlt sich, die Firewallregeln für den ausgehenden Verkehr basierend auf den festen IP-Adressen für Data Box Gateway in den meisten Fällen recht locker festzulegen. Sie können jedoch die folgenden Informationen verwenden, um erweiterte Firewallregeln festzulegen, die erforderlich sind, um sichere Umgebungen zu erstellen.

> [!NOTE]
> - Die Geräte-IPs (Quell-IPs) sollten immer für alle cloudaktivierten Netzwerkschnittstellen eingerichtet sein.
> - Die Ziel-IPs sollten auf die [IP-Bereiche des Azure-Datencenters](https://www.microsoft.com/download/confirmation.aspx?id=41653) festgelegt werden.

|     URL-Muster                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Komponente/Funktionalität                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.accesscontrol.windows.net/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Azure Data Box Gateway-Dienst<br>Zugriffssteuerungsdienst (ACS)<br>Azure-Servicebus<br>Authentifizierungsdienst    |
|    http://*.backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Geräteaktivierung                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Zertifikatswiderruf                                                                               |
|    https://*.core.windows.net/*   https://*.data.microsoft.com   http://*.msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Azure-Speicherkonten und Überwachung                                                                |
|    http://windowsupdate.microsoft.com<br>http://*.windowsupdate.microsoft.com<br>https://*.windowsupdate.microsoft.com<br>http://*.update.microsoft.com<br>https://*.update.microsoft.com<br>http://*.windowsupdate.com<br>http://download.microsoft.com<br>http://*.download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*.ws.microsoft.com<br>https://*.ws.microsoft.com<br>http://*.mp.microsoft.com        |    Microsoft Update-Server                                                                             |
|    http://*.deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    Akamai CDN                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Supportpaket                                                                                      |
|    http://*.data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    Telemetriedienst in Windows. Informationen finden Sie im „Update für Kundenzufriedenheit und Diagnosetelemetrie“.      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |



## <a name="internet-bandwidth"></a>Internetbandbreite

Die folgenden Anforderungen gelten für die minimale Internetbandbreite, die für Ihre Data Box Gateway-Geräte verfügbar ist.

- Für Ihr Data Box Gateway steht jederzeit eine feste Internetbandbreite von 20 MBit/s (oder mehr) zur Verfügung. Diese Bandbreite sollte nicht gemeinsam mit anderen Anwendungen genutzt werden. 
- Für Ihr Data Box Gateway steht jederzeit eine feste Internetbandbreite von 32 MBit/s (oder mehr) zur Verfügung, wenn die Einschränkung der Netzwerkbandbreite aktiviert ist.

## <a name="next-step"></a>Nächster Schritt

* [Bereitstellen Ihres Azure Data Box-Gateways](data-box-gateway-deploy-prep.md)


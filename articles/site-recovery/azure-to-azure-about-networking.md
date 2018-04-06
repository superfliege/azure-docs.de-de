---
title: Netzwerkkonzepte für die Notfallwiederherstellung zwischen Azure-Standorten mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Netzwerkkonzepte für die Replikation von Azure-VMs mithilfe von Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/26/2018
ms.author: sujayt
ms.openlocfilehash: 48be55632d9c1bece3f1a6e4f9ac12a68f9cb7ab
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Netzwerkkonzepte für die Replikation zwischen Azure-Standorten

>[!NOTE]
> Die Site Recovery-Replikation für virtuelle Azure-Computer ist derzeit als Vorschauversion verfügbar.

Dieser Artikel bietet einen Netzwerkleitfaden für die Replikation und Wiederherstellung von Azure-VMs zwischen verschiedenen Regionen mithilfe von [Azure Site Recovery](site-recovery-overview.md). 

## <a name="before-you-start"></a>Vorbereitung

Erfahren Sie, wie Site Recovery die Notfallwiederherstellung für [dieses Szenario](azure-to-azure-architecture.md) bereitstellt.

## <a name="typical-network-infrastructure"></a>Typische Netzwerkinfrastruktur

Das folgende Diagramm zeigt eine typische Azure-Umgebung für Anwendungen, die auf Azure-VMs ausgeführt werden:

![Kundenumgebung](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Wenn Ihr lokales Netzwerk über eine Azure ExpressRoute- oder VPN-Verbindung mit Azure verbunden ist, sieht die Umgebung wie folgt aus:

![Kundenumgebung](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normalerweise werden Netzwerke durch Firewalls und Netzwerksicherheitsgruppen (NSGs) geschützt. Firewalls verwenden eine URL- oder IP-basierte Whitelist zum Steuern der Netzwerkkonnektivität. NSGs stellen Regeln bereit, die IP-Adressbereiche zum Steuern der Netzwerkkonnektivität verwenden.

>[!IMPORTANT]
> Die Verwendung eines authentifizierten Proxys zum Steuern der Netzwerkkonnektivität wird von Site Recovery nicht unterstützt. In diesem Fall kann die Replikation nicht aktiviert werden.


## <a name="outbound-connectivity-for-urls"></a>Ausgehende Konnektivität für URLs

Lassen Sie die folgenden Site Recovery-URLs zu, wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden:


**URL** | **Details**  
--- | ---
*.blob.core.windows.net | Erforderlich, damit Daten in das Cachespeicherkonto in der Quellregion über die VM geschrieben werden können.
login.microsoftonline.com | Erforderlich für die Autorisierung und Authentifizierung bei den Site Recovery-Dienst-URLs.
*.hypervrecoverymanager.windowsazure.com | Erforderlich, um die Kommunikation mit dem Site Recovery-Dienst über die VM zu ermöglichen.
*.servicebus.windows.net | Erforderlich, damit die Site Recovery-Überwachungs- und -Diagnosedaten über die VM geschrieben werden können.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Ausgehende Konnektivität für IP-Adressbereiche

Wenn Sie einen IP-basierten Firewallproxy oder NSG-Regeln zum Steuern der ausgehenden Konnektivität verwenden, müssen die folgenden IP-Adressbereiche zugelassen werden.

- Alle IP-Adressbereiche, die dem Quellstandort entsprechen.
    - Sie können die [IP-Adressbereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) herunterladen.
    - Sie müssen diese Adressen zulassen, damit Daten von der VM in das Cachespeicherkonto geschrieben werden können.
- Alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.
    - Wenn den Office 365-Bereichen später neue Adressen hinzugefügt werden, müssen Sie neue NSG-Regeln erstellen.
- IP-Adressen des Site Recovery-Dienstendpunkts. Diese IP-Adressen sind in einer [XML-Datei](https://aka.ms/site-recovery-public-ips) verfügbar und hängen vom Zielstandort ab.
-  Sie können [dieses Skript herunterladen und verwenden](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702), um die erforderlichen Regeln in der Netzwerksicherheitsgruppe automatisch zu erstellen. 
- Wir empfehlen, die erforderlichen NSG-Regeln in einer Test-Netzwerksicherheitsgruppe zu erstellen und sicherzustellen, dass keine Probleme vorliegen, bevor Sie die Regeln in einer Netzwerksicherheitsgruppe in der Produktionsumgebung erstellen.
- Um die erforderliche Anzahl von NSG-Regeln zu erstellen, stellen Sie sicher, dass Ihr Abonnement in der Whitelist enthalten ist. Wenden Sie sich an den Azure-Support, um das Limit für NSG-Regeln in Ihrem Abonnement zu erhöhen.

Die IP-Adressbereiche lauten wie folgt:

>
   **Ziel** | **IP-Adressen für Site Recovery** |  **IP-Adressen zur Site Recovery-Überwachung**
   --- | --- | ---
   Asien, Osten | 52.175.17.132 | 13.94.47.61
   Asien, Südosten | 52.187.58.193 | 13.76.179.223
   Indien, Mitte | 52.172.187.37 | 104.211.98.185
   Indien (Süden) | 52.172.46.220 | 104.211.224.190
   USA Nord Mitte | 23.96.195.247 | 168.62.249.226
   Nordeuropa | 40.69.212.238 | 52.169.18.8
   Europa, Westen | 52.166.13.64 | 40.68.93.145
   USA (Ost) | 13.82.88.226 | 104.45.147.24
   USA (Westen) | 40.83.179.48 | 104.40.26.199
   USA Süd Mitte | 13.84.148.14 | 104.210.146.250
   USA (Mitte) | 40.69.144.231 | 52.165.34.144
   USA (Ost) 2 | 52.184.158.163 | 40.79.44.59
   Japan, Osten | 52.185.150.140 | 138.91.1.105
   Japan, Westen | 52.175.146.69 | 138.91.17.38
   Brasilien Süd | 191.234.185.172 | 23.97.97.36
   Australien (Osten) | 104.210.113.114 | 191.239.64.144
   Australien, Südosten | 13.70.159.158 | 191.239.160.45
   Kanada, Mitte | 52.228.36.192 | 40.85.226.62
   Kanada, Osten | 52.229.125.98 | 40.86.225.142
   USA, Westen-Mitte | 52.161.20.168 | 13.78.149.209
   USA, Westen 2 | 52.183.45.166 | 13.66.228.204
   UK, Westen | 51.141.3.203 | 51.141.14.113
   UK, Süden | 51.140.43.158 | 51.140.189.52
   Großbritannien, Süden 2 | 13.87.37.4| 13.87.34.139
   Großbritannien, Norden | 51.142.209.167 | 13.87.102.68
   Korea, Mitte | 52.231.28.253 | 52.231.32.85
   Korea, Süden | 52.231.298.185 | 52.231.200.144
   
   
  

## <a name="example-nsg-configuration"></a>Beispielkonfiguration für eine Netzwerksicherheitsgruppe

Dieses Beispiel zeigt, wie NSG-Regeln für eine zu replizierende VM konfiguriert werden. 

- Wenn Sie die ausgehende Konnektivität mit NSG-Regeln steuern, verwenden Sie für alle erforderlichen IP-Adressbereiche Regeln zum Zulassen ausgehender HTTPS-Verbindungen.
- Im Beispiel wird davon ausgegangen, dass der VM-Quellstandort „USA, Osten“ ist und der Zielstandort „USA, Mitte“.

### <a name="nsg-rules---east-us"></a>NSG-Regeln – USA, Osten

1. Erstellen Sie Regeln, die [IP-Adressbereichen für die Region „USA, Osten“](https://www.microsoft.com/download/confirmation.aspx?id=41653) entsprechen. Dies ist erforderlich, damit über die VM Daten in das Cachespeicherkonto geschrieben werden können.
2. Erstellen Sie Regeln für alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.
3. Erstellen Sie Regeln, die dem Zielstandort entsprechen:

   **Location** | **IP-Adressen für Site Recovery** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Mitte) | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG-Regeln – USA, Mitte 

Diese Regeln sind erforderlich, damit nach dem Failover die Replikation von der Zielregion zur Quellregion aktiviert werden kann:

* Regeln, die [IP-Adressbereichen für die Region „USA, Mitte“](https://www.microsoft.com/download/confirmation.aspx?id=41653) entsprechen. Diese sind erforderlich, damit über die VM Daten in das Cachespeicherkonto geschrieben werden können.

* Regeln für alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.

* Regeln, die dem Quellstandort entsprechen:
    - USA (Ost)
    - IP-Adresse für Site Recovery: 13.82.88.226
    - IP-Adresse zur Site Recovery-Überwachung: 104.45.147.24


## <a name="expressroutevpn"></a>ExpressRoute/VPN 

Wenn eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen Standort und dem Azure-Standort besteht, befolgen Sie die Richtlinien in diesem Abschnitt.

### <a name="forced-tunneling"></a>Tunnelerzwingung

Üblicherweise definieren Sie eine Standardroute (0.0.0.0/0), die die Übermittlung von ausgehendem Internetdatenverkehr durch den lokalen Standort erzwingt. Hiervon wird abgeraten. Der Replikationsdatenverkehr und die Kommunikation mit dem Site Recovery-Dienst sollten nicht die Grenzen von Azure verlassen. Die Lösung besteht darin, benutzerdefinierte Routen (User-Defined Routes, UDRs) für [diese IP-Adressbereiche](#outbound-connectivity-for-azure-site-recovery-ip-ranges) hinzuzufügen, damit der Replikationsdatenverkehr nicht über den lokalen Standort weitergeleitet wird.

### <a name="connectivity"></a>Konnektivität 

Befolgen Sie die folgenden Richtlinien für Verbindungen zwischen dem Zielstandort und dem lokalen Standort:
- Wenn Ihre Anwendung eine Verbindung mit lokalen Computern herstellen muss oder Clients von lokalen Standorten aus über VPN/ExpressRoute eine Verbindung mit der Anwendung herstellen, stellen Sie sicher, dass mindestens eine [Standort-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) zwischen Ihrer Azure-Zielregion und dem lokalen Rechenzentrum besteht.

- Wenn Sie mit einem hohen Datenverkehrsaufkommen zwischen Ihrer Azure-Zielregion und dem lokalen Rechenzentrum rechnen, sollten Sie eine weitere [ExpressRoute-Verbindung](../expressroute/expressroute-introduction.md) zwischen der Azure-Zielregion und dem lokalen Rechenzentrum erstellen.

- Wenn Sie nach einem Failover IP-Adressen für virtuelle Computer beibehalten möchten, lassen Sie die Standort-zu-Standort-/ExpressRoute-Verbindung der Zielregion getrennt. Dadurch wird sichergestellt, dass keine Bereichskonflikte zwischen den IP-Adressbereichen der Quellregion und denen der Zielregion entstehen.

### <a name="expressroute-configuration"></a>ExpressRoute-Konfiguration
Befolgen Sie die folgenden Best Practices für die ExpressRoute-Konfiguration:

- Erstellen Sie eine ExpressRoute-Verbindung in der Quell- und Zielregion. Dann müssen Sie eine Verbindung zwischen Folgendem herstellen:
    - Virtuelles Quellnetzwerk und lokales Netzwerk (über die ExpressRoute-Verbindung in der Quellregion)
    - Virtuelles Zielnetzwerk und lokales Netzwerk (über die ExpressRoute-Verbindung in der Zielregion)


- Im Rahmen des ExpressRoute-Standards können Sie Verbindungen in derselben geopolitischen Region erstellen. Für die Erstellung von ExpressRoute-Verbindungen in verschiedenen geopolitischen Regionen ist Azure ExpressRoute Premium erforderlich. Hierfür werden zusätzliche Kosten erhoben. (Wenn Sie bereits ExpressRoute Premium verwenden, fallen keine zusätzlichen Kosten an.) Weitere Informationen finden Sie im [Dokument zu ExpressRoute-Standorten](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) und [ExpressRoute-Preisen](https://azure.microsoft.com/pricing/details/expressroute/).

- Es wird empfohlen, verschiedene IP-Adressbereiche in Quell- und Zielregionen zu verwenden. Die ExpressRoute-Verbindung kann nicht gleichzeitig eine Verbindung mit zwei virtuellen Azure-Netzwerke mit denselben IP-Adressbereichen herstellen.

- Sie können virtuelle Netzwerke mit denselben IP-Adressbereichen in beiden Regionen erstellen und dann ExpressRoute-Verbindungen in beiden Regionen erstellen. Trennen Sie im Falle eines Failovers die Verbindung vom virtuellen Quellnetzwerk und stellen Sie eine Verbindung mit dem virtuellen Zielnetzwerk her.

 >[!IMPORTANT]
 > Wenn die primäre Region nicht vollständig deaktiviert ist, kann ein Fehler beim Trennvorgang auftreten. Hierdurch kann das virtuelle Zielnetzwerk keine ExpressRoute-Verbindung herstellen.

## <a name="next-steps"></a>Nächste Schritte
Schützen Sie Ihre Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md).

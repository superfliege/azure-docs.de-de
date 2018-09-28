---
title: Azure Virtual WAN – Standorte von Partnern | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Liste der Standorte von Azure Virtual WAN-Partnern und -Hubs.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: 76542392522ff14642ed7ccc6de8ed543d66513f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984992"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Standorte von Virtual WAN-Partnern und -Hubs

Dieser Artikel enthält Informationen über die von Virtual WAN unterstützten Regionen und bevorzugten Partner für Konnektivität mit Virtual Hub.

Der Netzwerkdienst Azure Virtual WAN bietet optimierte und automatisierte Konnektivität zwischen Zweigstellen über Azure. Per Virtual WAN können Sie Zweigstellengeräte für die Kommunikation mit Azure verbinden und konfigurieren. Dies ist entweder manuell oder durch die Nutzung von Geräten eines bevorzugten Anbieters über einen bevorzugten Virtual WAN-Partner möglich. Die Nutzung von Geräten eines bevorzugten Partners ermöglicht eine einfache Verwendung, die Vereinfachung der Verbindungsherstellung und die Konfigurationsverwaltung. Konnektivität auf dem lokalen Gerät wird auf automatisierte Weise mit dem virtuellen Hub hergestellt. Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk. Der Hub enthält verschiedene Dienstendpunkte zum Aktivieren der Konnektivität über Ihr lokales Netzwerk (vpnsite). Pro Region ist nur ein Hub möglich.

## <a name="regions"></a>Regionen

Die Liste der unterstützten und verfügbaren Regionen ist wie folgt:

|Geopolitische Region | Azure-Regionen|
|---|---|
|Nordamerika | USA, Osten; USA, Westen; USA, Osten 2; USA, Westen 2; USA, Mitte; USA, Süden-Mitte; USA, Norden-Mitte; USA, Westen-Mitte; Kanada, Mitte; Kanada, Osten |
|Südamerika |Brasilien Süd |
| Europa | „Frankreich, Mitte“, „Frankreich, Süden“, „Europa, Norden“, „Europa, Westen“, „Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“ |
| Asien | Ostasien, Südostasien |
| Japan  | Japan West, Japan Ost |
| Australien | Südostaustralien, Ostaustralien | 
| Australische Behörden | Australien, Mitte; Australien, Mitte 2 |
| Indien | Indien, Westen, Indien, Mitte, Indien, Süden |
| Südkorea | Korea, Mitte, Korea, Süden | 

## <a name="automation-from-connectivity-partners"></a>Automatisierung von Konnektivitätspartnern

Dieser Abschnitt beschreibt die allgemeinen Details der Automatisierung von Konnektivitätsanbietern.

Geräte, die sich mit Azure Virtual WAN verbinden, verfügen für die Verbindungsherstellung über eine integrierte Automatisierung. Diese wird typischerweise auf der Geräteverwaltungsoberfläche (o.ä.) eingerichtet. Dadurch wird die Verbindungs- und Konfigurationsverwaltung zwischen dem VPN-Zweigstellengerät und einem Azure Virtual Hub VPN-Endpunkt (VPN Gateway) eingerichtet.

Die folgende allgemeine Automatisierung wird in der Gerätekonsole bzw. im Verwaltungscenter eingerichtet:

* Entsprechende Berechtigungen für das Gerät, um auf die Azure Virtual WAN-Ressourcengruppe zugreifen zu können
* Hochladen des Zweigstellengeräts in Azure Virtual WAN
* Automatischer Download der Azure-Konnektivitätsinformationen 
* Konfiguration des lokalen Zweigstellengeräts 

Einige Konnektivitätspartner können die Automatisierung erweitern, indem sie das Azure Virtual Hub-VNET und das VPN-Gateway erstellen. Wenn Sie mehr über Automatisierung erfahren möchten, siehe [Konfigurieren der Automatisierung – WAN-Partner](virtual-wan-configure-automation-providers.md).

## <a name="connectivity-through-preferred-partners"></a>Konnektivität über bevorzugte Partner

Wenn Ihr Partner für Zweigstellengeräte nicht im folgenden Abschnitt aufgeführt ist, wenden Sie sich an Ihren Anbieter von Zweigstellengeräten, und bitten Sie ihn, uns zu kontaktieren, indem Sie eine E-Mail an azurevirtualwan@microsoft.com senden.

Sie können auch die folgenden Links überprüfen, um weitere Informationen über die von den bevorzugten Partnern angebotenen Dienste zu erhalten. 

|Bevorzugte Partner|
|---|
|[Barracuda Networks](https://www.barracuda.com/AzurevWAN)|
| [Check Point](https://www.checkpoint.com/solutions/microsoft-azure-virtual-wan/) |
| [Citrix](https://www.citrix.com/global-partners/microsoft/sd-wan-for-azure-virtual-wan.html)|
|[Netfoundry](https://netfoundry.io/solutions/netfoundry-for-microsoft-azure-virtual-wan/)|
|[Palo Alto Networks](https://researchcenter.paloaltonetworks.com/2018/09/azure-vwan-integration/) |
|[Riverbed Technology](https://www.riverbed.com/go/steelconnect-azurewan.html)|
|[128 Technology](https://www.128technology.com/partners/azure) |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie unter [Virtual WAN – Häufig gestellte Fragen](virtual-wan-faq.md).

Weitere Informationen zur Automatisierung der Konnektivität mit Azure Virtual WAN finden Sie unter [Virtual WAN-Automatisierung durch Partner](virtual-wan-configure-automation-providers.md).

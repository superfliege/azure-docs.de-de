---
title: Azure Relay-Porteinstellungen | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu Azure Relay-Portwerten.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 875f00064f94b37ab5efdde54ca3e6cbda779654
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-relay-port-settings"></a>Azure Relay-Porteinstellungen

Die folgende Tabelle beschreibt die erforderliche Konfiguration für Portwerte für Azure Relay.

## <a name="hybrid-connections"></a>Hybridverbindungen
Hybrid Connections verwendet WebSockets als zugrunde liegenden Transportmechanismus, der ausschließlich **HTTPS** verwendet. 

## <a name="wcf-relays"></a>WCF-Relays
  
|Bindung|Transportsicherheit|Port|  
|-------------|------------------------|----------|  
|[BasicHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (Client)|Ja|HTTPS| 
| |" |Nein|HTTP|  
|[BasicHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (Dienste)|Sie können das|9351/HTTP|  
|[NetEventRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (Client)|Ja|9351/HTTPS|  
||" |Nein|9350/HTTP|  
|[NetEventRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (Dienst)|Sie können das|9351/HTTP|  
|[NetTcpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (Client/Dienst)|Sie können das|5671/9352/HTTP (9352/9353, wenn eine Hybridverbindung verwendet)|  
|[NetOnewayRelayBinding Class](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (Client)|Ja|9351/HTTPS|  
||" |Nein|9350/HTTP|  
|[NetOnewayRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (Dienst)|Sie können das|9351/HTTP|  
|[WebHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (Client)|Ja|HTTPS|  
||" |Nein|HTTP|  
|[WebHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (Client)|Sie können das|9351/HTTP|  
|[WS2007HttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (Client)|Ja|HTTPS|  
||" |Nein|HTTP|  
|[WS2007HttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (Dienst)|Sie können das|9351/HTTP|

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Relay finden Sie unter diesen Links:
* [Was ist Azure Relay?](relay-what-is-it.md)
* [Relay – Häufig gestellte Fragen](relay-faq.md)
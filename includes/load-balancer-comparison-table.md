---
title: Includedatei
description: Includedatei
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 8/8/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 6514bcdbe79db4a22b2a4bf13e5808bbb9abbb06
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40026280"
---
| | Standard-SKU | Basic-SKU |
| --- | --- | --- |
| Größe des Back-End-Pools | Unterstützt bis zu 1.000 Instanzen | Unterstützt bis zu 100 Instanzen |
| Back-End-Pool-Endpunkte | Jeder virtuelle Computer in einem einzelnen virtuellen Netzwerk, einschließlich Kombinationen aus virtuellen Computern, Verfügbarkeitsgruppen, VM-Skalierungsgruppen | Virtuelle Computer in einer einzelnen Verfügbarkeitsgruppe oder VM-Skalierungsgruppe |
| [Integritätstests](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Verhalten bei Ausfall während Integritätstest](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP-Verbindungen bleiben bei Ausfällen während Instanztests __und__ bei Ausfällen aller Tests bestehen. | TCP-Verbindungen bleiben bei Ausfällen während Instanztests bestehen. Alle TCP-Verbindungen werden bei Ausfällen während Tests beendet. |
| Verfügbarkeitszonen | In Basic-SKU, zonenredundante und zonale Front-Ends für eingehende und ausgehende Datenflüsse, Zuordnungen für ausgehende Datenflüsse bleiben bei Zonenfehlern erhalten, zonenübergreifender Lastenausgleich | –|
| Diagnose | Azure Monitor, mehrdimensionale Metriken einschließlich Byte- und Paketleistungsindikatoren, Integritätsteststatus, Verbindungsversuche (TCP SYN), Integrität ausgehender Verbindungen (erfolgreiche und fehlgeschlagene SNAT-Datenströme), Messungen für die aktive Datenebene | Azure Log Analytics nur für öffentlichen Lastenausgleich, SNAT-Überlastungswarnung, Integritätsanzahl für Back-End-Pool |
| HA-Ports | Interner Lastenausgleich | – |
| Standardmäßig sicher | Standardmäßig geschlossen für öffentliche IP- und Load Balancer-Endpunkte, und es muss eine Netzwerksicherheitsgruppe verwendet werden, über die explizit eine Whitelist verwendet wird, damit Datenverkehr möglich ist | Standardmäßig geöffnet, Netzwerksicherheitsgruppe ist optional |
| [Ausgehende Verbindungen](../articles/load-balancer/load-balancer-outbound-connections.md) | Mehrere Front-Ends mit Deaktivierung über Lastenausgleichsregel. Es _muss_ explizit ein Szenario für ausgehenden Datenverkehr erstellt werden, damit der virtuelle Computer ausgehende Verbindungen verwenden kann.  Dienstendpunkte für virtuelle Netzwerke können ohne ausgehende Verbindungen erreicht werden und werden nicht für verarbeitete Daten angerechnet.  Alle öffentlichen IP-Adressen, einschließlich der Azure-PaaS-Dienste, die nicht als VNet-Dienstendpunkte verfügbar sind, müssen über ausgehende Verbindungen erreicht werden und werden als verarbeitete Daten angerechnet. Wenn nur ein interner Load Balancer einen virtuellen Computer versorgt, sind ausgehende Verbindungen nicht über Standard-SNAT verfügbar. Ausgehende SNAT-Programmierung ist transportprotokollspezifisch entsprechend dem Protokoll der Regel für eingehenden Lastenausgleich. | Einzelnes Front-End, ausgewählt nach dem Zufallsprinzip, wenn mehrere Front-Ends vorhanden sind.  Wenn Sie nur ein interner Load Balancer einen virtuellen Computer versorgt, wird Standard-SNAT verwendet. |
| [Mehrere Front-Ends](../articles/load-balancer/load-balancer-multivip-overview.md) | Eingehend und [ausgehend](../articles/load-balancer/load-balancer-outbound-connections.md) | Nur eingehend |
| Verwaltungsvorgänge | Die meisten Vorgänge < 30 Sekunden | Meist 60 bis 90 (oder mehr) Sekunden |
| SLA | 99,99 % für Datenpfad mit zwei fehlerfreien virtuellen Computern | Implizit in VM-SLA | 
| Preise | Berechnet anhand der Anzahl der Regeln, Daten, die eingehend oder ausgehend verarbeitet werden, werden der Ressource zugeordnet  | Keine Berechnung |
|  |  |  |

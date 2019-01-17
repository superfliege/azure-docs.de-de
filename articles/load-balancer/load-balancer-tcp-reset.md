---
title: Load Balancer mit TCP-Rücksetzung bei Leerlauf
titlesuffix: Azure Load Balancer
description: Load Balancer mit bidirektionalen TCP-RST-Paketen bei Leerlauftimeout
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/03/2018
ms.author: kumud
ms.openlocfilehash: 0b3e8fc72eb22a67c0672be19f60d4956d3377b7
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257290"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer mit TCP-Rücksetzung bei Leerlauf (öffentliche Vorschau)

Sie können [Load Balancer Standard](load-balancer-standard-overview.md) verwenden, um ein besser vorhersagbares Anwendungsverhalten für Ihre Szenarios mit bidirektionalen TCP-Rücksetzungen (TCP-RST-Paketen) für jedes konfigurierbare Leerlauftimeout zu erhalten.  Das Standardverhalten von Load Balancer besteht darin, Flows ohne Rückmeldung zu verwerfen, wenn das Leerlauftimeout eines Flows erreicht ist.

![Load Balancer mit TCP-Rücksetzung](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>Die Funktion von Load Balancer mit TCP-Rücksetzung bei Leerlauftimeout ist zurzeit in einer begrenzten Anzahl von [Regionen](#regions) als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
Sie ändern dieses Standardverhalten und ermöglichen das Senden von TCP-Rücksetzungen bei Leerlauftimeout in NAT-Eingangsregeln, Lastenausgleichsregeln und [Ausgangsregeln](https://aka.ms/lboutboundrules).  Wenn dies durch eine Regel aktiviert ist, sendet Load Balancer zum Zeitpunkt des Leerlauftimeouts für alle passenden Flows bidirektionale TCP-Rücksetzungen (TCP-RST-Pakete) sowohl an den Client- als auch an den Serverendpunkt.

Endpunkte, die TCP-RST-Pakete empfangen, schließen den entsprechenden Socket sofort. Damit werden Endpunkte sofort darüber benachrichtigt, dass die Verbindung getrennt wurde und jegliche weitere Kommunikation über die gleiche TCP-Verbindung zu einem Fehler führt.  Anwendungen können Verbindungen bereinigen, wenn der Socket geschlossen wird, und Verbindungen bei Bedarf erneut herstellen, ohne auf das Timeout der TCP-Verbindung zu warten.

In vielen Szenarien müssen damit weniger TCP-Keepalives (bzw. Keepalives auf der Anwendungsschicht) gesendet werden, um das Leerlauftimeout eines Flows zu aktualisieren. 

Wenn die Leerlaufzeiträume die in der Konfiguration erlaubten Werte überschreiten oder Ihre Anwendung mit aktivierten TCP-Rücksetzungen ein unerwünschtes Verhalten zeigt, müssen Sie diese TCP-Keepalives (bzw. Keepalives auf der Anwendungsschicht) möglicherweise weiterhin verwenden, um die Verfügbarkeit der TCP-Verbindungen zu überwachen.  Keepalives (insbesondere Keepalives auf der Anwendungsschicht) sind zudem weiterhin nützlich, wenn eine Verbindung an irgendeiner Stelle im Pfad über einen Proxy geleitet wird.  

Überprüfen Sie das gesamte Szenario sorgfältig, um zu entscheiden, ob Sie – ggf. bei angepassten Leerlauftimeouts – von TCP-Rücksetzungen profitieren und ob zusätzliche Schritte erforderlich sind, um das gewünschte Anwendungsverhalten sicherzustellen.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Aktivieren der TCP-Rücksetzung bei Leerlauftimeout

Mithilfe der API-Version 2018-07-01 können Sie das Senden von bidirektionalen TCP-Rücksetzungen bei Leerlauftimeout für die einzelnen Regeln aktivieren:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a> Regionale Verfügbarkeit

Dieser Parameter gilt zurzeit in den folgenden Regionen.  In hier nicht aufgeführten Regionen hat der Parameter keinerlei Auswirkungen.

| Region |
|---|
| Asien, Südosten |
| Brasilien Süd |
| Kanada, Mitte |
| Europa, Westen |
| Indien, Mitte |
| Indien, Westen |
| Japan, Westen |
| Korea, Mitte |
| Korea, Süden |
| Vereinigtes Königreich, Norden |
| Vereinigtes Königreich, Süden 2 |
| USA, Osten |
| USA (Ost 2) |
| Vereinigtes Königreich, Norden |
| USA, Westen |

Diese Tabelle wird aktualisiert, sobald die Vorschau auf weitere Regionen ausgeweitet wird.  

## <a name="limitations"></a>Einschränkungen

- Eingeschränkte [regionale Verfügbarkeit](#regions).
- Sie können TCP-Rücksetzungen im Portal weder konfigurieren noch ansehen.  Verwenden Sie stattdessen Vorlagen, die REST-API, die Azure CLI 2.0 oder PowerShell.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Load Balancer Standard](load-balancer-standard-overview.md).
- Erfahren Sie mehr über [Ausgangsregeln](load-balancer-outbound-rules-overview.md).

---
title: "Übersicht: Azure Load Balancer Standard | Microsoft Docs"
description: "Übersicht: Features von Azure Load Balancer Standard"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2017
ms.author: kumud
ms.openlocfilehash: 0ed8d3432a988c468260589cfe12090529c403d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-load-balancer-standard-overview-preview"></a>Übersicht: Azure Load Balancer Standard (Preview)

Die SKUs Azure Load Balancer Standard und Public IP Standard ermöglichen Ihnen zusammen das Erstellen hoch skalierbarer und zuverlässiger Architekturen.  Anwendungen, die Load Balancer Standard verwenden, bieten nicht nur geringe Latenz, hohen Durchsatz und Skalierung für Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen, sondern können auch neue Funktionen nutzen.

>[!NOTE]
> Die Load Balancer Standard-SKU ist zurzeit als Vorschauversion verfügbar. Während der Vorschauphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Verwenden Sie die allgemein verfügbare [Load Balancer Basic-SKU](load-balancer-overview.md) für Ihre Produktionsdienste.  Funktionen in dieser Vorschauversion wie z.B. [Verfügbarkeitszonen](https://aka.ms/availabilityzones) und [Ports mit Hochverfügbarkeit](https://aka.ms/haports) erfordern derzeit eine getrennte Anmeldung. Befolgen Sie zusätzlich zur Load Balancer [Standard-Vorschauversion](#preview-sign-up) die entsprechenden Hinweise zur Anmeldung.

## <a name="why-use-load-balancer-standard"></a>Gründe für die Verwendung von Load Balancer Standard

Verwenden Sie Load Balancer-Standard für das gesamte Spektrum virtueller Rechenzentren von kleinen Bereitstellungen bis hin zu großen und komplexen Architekturen mit mehreren Zonen, um die folgenden Funktionen zu nutzen:

- [Skalierung auf Unternehmensniveau](#enterprisescale) kann mit Load Balancer-Standard erreicht werden.  Das Produkt kann mit jeder Virtual Machine-Instanz mit Virtual Network und mit bis zu 1.000 VM-Instanzen verwendet werden.

- [Neue Diagnoseerkenntnisse](#diagnosticinsights) ermöglichen Ihnen das Verstehen, das Verwalten und die Problembehandlung für diese wichtige Komponente des virtuellen Rechenzentrums. Verwenden Sie Azure Monitor (Preview) zum Anzeigen, Filtern und Gruppieren von neuen mehrdimensionalen Metriken für kontinuierliche Datenpfad-Integritätsmessungen vom Front-End bis zum virtuellen Computer anhand von Endpunktintegritätsüberprüfungen, TCP-Verbindungsversuchen und ausgehenden Verbindungen.

- [Netzwerksicherheitsgruppen](#nsg) sind nun für alle VM-Instanzen erforderlich, die Standard-SKUs von Load Balancer oder Public IP zugeordnet sind, und bieten mehr Sicherheit.

- [HA-Ports bieten hohe Zuverlässigkeit](#highreliability) und Skalierung für virtuelle Netzwerkgeräte und andere Anwendungsszenarien. HA-Ports führen den Lastenausgleich für alle Ports auf einem internen Load Balancer-Front-End in einem Pool von VM-Instanzen aus.

- [Ausgehende Verbindungen](#outboundconnections) verwenden nun ein neues SNAT-Portzuweisungsmodell, das bessere Stabilität und Skalierung bereitstellt.

- [Load Balancer Standard mit Verfügbarkeitszonen](#availabilityzones) kann verwendet werden, um zonenredundante und zonale Architekturen mit zonenübergreifendem Lastenausgleich zu nutzen. Sie können Zonenredundanz ohne Abhängigkeit von DNS-Einträgen erzielen: Eine einzelne IP-Adresse ist einfach standardmäßig zonenredundant und kann jeden virtuellen Computer in einem VNet einer Region in allen Verfügbarkeitszonen erreichen.


Load Balancer Standard können Sie in einer öffentlichen oder in einer internen Konfiguration verwenden. Dabei werden die folgenden grundlegenden Szenarien weiterhin unterstützt:

- Lastenausgleich für eingehenden Datenverkehr mit integren Back-End-Instanzen.
- Portweiterleitung von eingehendem Datenverkehr an eine einzelne Back-End-Instanz.
- Übersetzen von ausgehendem Datenverkehr aus einer privaten IP-Adresse im VNet in eine öffentliche IP-Adresse.

### <a name = "enterprisescale"></a>Skalierung auf Unternehmensniveau

 Verwenden Sie Load Balancer Standard, um Ihr leistungsstarkes virtuelles Rechenzentrum zu entwerfen. Sie können eigenständige VM-Instanzen oder bis zu 1.000 VM-Skalierungsgruppeninstanzen in einem Back-End-Pool verwenden und jede TCP- oder UDP-Anwendung unterstützen. Mit Load Balancer Standard kann Ihre Anwendung weiterhin Weiterleitung mit niedriger Latenz, hohe Durchsatzleistung und Skalierung auf Millionen von Datenflüssen für einen vollständig verwalteten Azure-Dienst nutzen.
 
Load Balancer Standard kann Datenverkehr an eine beliebige VM-Instanz in einem VNet in einer Region weiterleiten. Die Größe des Back-End-Pools kann bis zu 1.000 Instanzen mit einer beliebigen Kombination der folgenden Komponenten betragen:

- Eigenständige virtuelle Computer ohne Verfügbarkeitsgruppen
- Eigenständige virtuelle Computer mit Verfügbarkeitsgruppen
- VM-Skalierungsgruppen (VM-Skalierungsgruppe) mit bis zu 1.000 Instanzen
- Mehrere VM-Skalierungsgruppen
- Kombination aus virtuellen Computern und VM-Skalierungsgruppen

Verfügbarkeitsgruppen sind nicht mehr erforderlich. Sie können aber wahlweise Verfügbarkeitsgruppen verwenden, um deren Vorteile zu nutzen.

### <a name = "diagnosticinsights"></a>Diagnoseerkenntnisse

Load Balancer Standard bietet neue mehrdimensionale Diagnosefunktionen für öffentliche und interne Load Balancer-Konfigurationen. Diese neuen Metriken werden über Azure Monitor (Preview) bereitgestellt und nutzen alle zugehörigen Funktionen einschließlich der Möglichkeit für die Integration mit verschiedenen Downstreamconsumern.

| Metrik | Beschreibung |
| --- | --- |
| VIP-Verfügbarkeit | Load Balancer Standard wendet kontinuierlich den Datenpfad aus einer Region auf das Load Balancer-Front-End und schließlich auf den SDN-Stapel an, der Ihren virtuellen Computer unterstützt. Solange integre Instanzen verbleiben, folgt die Messung demselben Pfad wie der Datenverkehr mit Lastenausgleich Ihrer Anwendungen, und überprüft den Datenpfad, den Ihre Kunden möglicherweise verwenden. Die Messung ist für Ihre Anwendung nicht sichtbar und bewirkt keine Beeinträchtigung.|
| DIP-Verfügbarkeit | Load Balancer Standard verwendet einen verteilten Integritätsprüfungsdienst, der die Integrität Ihres Anwendungsendpunkts gemäß Ihrer Konfiguration überwacht.  Diese Metrik stellt eine Aggregat- oder nach Endpunkt gefilterte Ansicht jedes einzelnen Instanzendpunkts im Load Balancer-Pool bereit.  Sie können sehen, wie Load Balancer die Integrität Ihrer Anwendung gemäß Ihrer Integritätsprüfungskonfiguration beurteilt.
| SYN-Pakete | Load Balancer-Standard beendet keine TCP-Verbindungen und interagiert nicht mit TCP- oder UDP-Paketdatenflüssen. Datenflüsse und deren-Handshakes erfolgen immer zwischen der Quelle und der VM-Instanz. Damit die Problembehandlung aller Ihrer TCP-Protokollszenarien auf einfachere Weise ausgeführt werden kann, können Sie diese Metrik verwenden, um zu verstehen, wie viele TCP-Verbindungsversuche stattgefunden haben. Diese Metrik meldet die Anzahl der TCP-SYN-Pakete, die empfangen wurden, und kann auf Clients hinweisen, die versuchen, Verbindungen mit Ihrem Dienst herzustellen.|
| SNAT-Verbindungen | Load Balancer-Standard meldet die Anzahl der maskierten ausgehenden Verbindungen an das Front-End mit der öffentlichen IP-Adresse.  SNAT-Ports sind eine erschöpfbare Ressource, und diese Metrik kann einen Hinweis darauf geben, in welchem Umfang Ihre Anwendung SNAT für ausgehende Verbindungen verwendet.|
| Byteleistungsindikatoren | Load Balancer-Standard meldet die pro Front-End verarbeiteten Daten.|
| Paketleistungsindikatoren | Load Balancer-Standard meldet die pro Front-End verarbeiteten Pakete.|

### <a name = "highreliability"></a>Hohe Zuverlässigkeit

Konfigurieren Sie Lastenausgleichsregeln, damit die Anwendung skaliert werden kann und hohe Zuverlässigkeit bietet.  Sie können Regeln für einzelne Ports konfigurieren, oder Sie können die neuen HA-Ports verwenden, um einen Lastenausgleich des gesamten Datenverkehrs unabhängig von der TCP- oder UDP-Portnummer auszuführen.  

Sie können das neue Feature „HA-Ports“ zum Ermöglichen einer Vielzahl von Szenarien verwenden, z. B. hohe Verfügbarkeit und Skalierung für interne virtuelle Netzwerkgeräte und andere Szenarien, in denen es unpraktisch oder unerwünscht ist, einzelne Ports anzugeben. HA-Ports bietet Redundanz und Skalierung, da die benötigte Anzahl von Instanzen zulässig ist und keine Einschränkung auf Aktiv-/Passivszenarien erfolgt. Ihre Integritätsprüfungskonfigurationen schützen Ihren Dienst, indem Datenverkehr nur an integre Instanzen weitergeleitet wird.

Die Hersteller virtueller Netzwerkgeräte können vollständig herstellerunterstützte, robuste Szenarien durch Entfernen einer einzelnen Fehlerquelle für ihre Kunden bereitstellen und es möglichen, dass mehrere aktive Instanzen für die Skalierung vorhanden sind. Sie können auf mindestens zwei Instanzen skalieren, wenn Ihr Gerät solche Konfigurationen zulässt. Der Hersteller Ihres virtuellen Netzwerkgeräts muss für diese Szenarien weitere Anleitungen bereitstellen.

### <a name = "availabilityzones"></a>Verfügbarkeitszonen

[!INCLUDE [availability-zones-preview-statement](../../includes/availability-zones-preview-statement.md)]

Vergrößern Sie die Anwendungsresilienz durch die Verwendung von Verfügbarkeitszonen in unterstützten Regionen. Verfügbarkeitszonen sind zurzeit als Vorschau in bestimmten Regionen verfügbar und erfordern ein zusätzliches Abonnement.

### <a name="automatic-zone-redundancy"></a>Automatische Zonenredundanz

Sie können auswählen, ob Load Balancer ein zonenredundantes oder zonales Front-End für jede Ihrer Anwendungen bereitstellen soll.  Es ist einfach, mit Load Balancer Standard Zonenredundanz zu erstellen.  Eine einzelne Front-End-IP-Adresse ist automatisch zonenredundant.  Ein zonenredundantes Front-End wird von allen Verfügbarkeitszonen in einer Region gleichzeitig versorgt. Auf diese Weise entsteht ein zonenredundanter Datenpfad für eingehende und ausgehende Verbindungen. Für Zonenredundanz in Azure sind nicht mehrere IP-Adressen und DNS-Einträge erforderlich. 

Diese Zonenredundanz ist für öffentliche und interne Front-Ends verfügbar. Ihre Public IP-Adresse und die private IP-Adresse Ihres internen Load Balancer-Front-Ends können zonenredundant sein.

Erstellen Sie eine zonenredundante Public IP-Adresse mit dem folgenden Code (fügen Sie allen ggf. vorhandenen Resource Manager-Vorlagen „sku“ hinzu):

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

Erstellen Sie eine interne, zonenredundante Load Balancer-Front-End-IP-Konfiguration mit dem folgenden Code (fügen Sie allen ggf. vorhandenen Resource Manager-Vorlagen „sku“ hinzu):

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

Wenn Ihr Public IP-Front-End zonenredundant ist, werden alle ausgehenden Verbindungen, die von VM-Instanzen erfolgen, automatisch zonenredundant und sind vor Zonenfehlern geschützt.  Ihre SNAT-Portzuordnung überdauert Zonenfehler.

#### <a name="cross-zone-load-balancing"></a>Zonenübergreifender Lastenausgleich

Zonenübergreifender Lastenausgleich steht innerhalb einer Region für den Back-End-Pool zur Verfügung und ermöglicht maximale Flexibilität für Ihre VM-Instanzen.  Ein Front-End-kann Datenflüsse an jeden virtuellen Computer im VNet unabhängig von der Verfügbarkeitszone der VM-Instanz übermitteln.

Sie können darüber hinaus auch eine bestimmte Zone für Ihre Front-End- und Back-End-Instanzen angeben, um Ihren Datenpfad und Ihre Ressourcen mit einer bestimmten Zone zu verbinden.

Da VNets und Subnetze niemals zoneneingeschränkt sind, müssen Sie einfach nur einen Back-End-Pool mit den gewünschten VM-Instanzen definieren, und die Konfiguration ist abgeschlossen.

#### <a name="zonal-deployments"></a>Zonale Bereitstellungen

Optional können Sie für das Front-End eine bestimmte Zone verwenden, indem Sie ein zonales Front-End definieren.  Ein zonales Front-End wird nur von der festgelegten einzelne Verfügbarkeitszone versorgt. In Kombination mit zonalen VM-Instanzen können Sie Ressourcen mit bestimmten Zonen verbinden.

Eine öffentliche IP-Adresse, die in einer bestimmten Zone erstellt wurde, ist immer nur in dieser Zone vorhanden.  Es ist nicht möglich, die Zone einer öffentlichen IP-Adresse zu ändern.  Wenn Sie eine öffentliche IP-Adresse wünschen, die Ressourcen in mehreren Zonen zugeordnet werden kann, sollten Sie stattdessen eine zonenredundante öffentliche IP-Adresse erstellen.

Erstellen Sie eine zonale Public IP-Adresse in Verfügbarkeitszone 1 mit dem folgenden Code (fügen Sie allen ggf. vorhandenen Resource Manager-Vorlagen „zones“ und „sku“ hinzu):

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Erstellen Sie ein internes Load Balancer-Front-End in der Verfügbarkeitszone 1 mit den folgenden Code (fügen Sie allen vorhandenen Resource Manager-Vorlagen „sku“ hinzu, und fügen Sie „zones“ in die untergeordnete Ressource mit der Front-End-IP-Konfiguration ein):

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

Darüber hinaus können Sie zonenübergreifenden Lastenausgleich für Ihr Back-End verwenden, indem Sie Ihre VM-Instanzen in einem VNet in einem Back-End-Pool platzieren.

Die Load Balancer Standard-Ressource selbst ist immer regional und zonenredundant, wenn Verfügbarkeitszonen unterstützt werden. Eine Public IP-Adresse oder ein internes Load Balancer-Front-End, der bzw. dem keine Zone zugewiesen wurde, kann in einer beliebigen Region unabhängig von der Unterstützung für Verfügbarkeitszonen bereitgestellt werden. Wenn einer Region später Verfügbarkeitszonen hinzugefügt werden, wird eine bereits bereitgestellte Public IP-Adresse oder ein internes Load Balancer-Front-End automatisch zonenredundant. Ein zonenredundanter Datenpfad bedeutet nicht Paketverluste von 0 %.

### <a name = "nsg"></a>Netzwerksicherheitsgruppen

Load Balancer Standard und Public IP Standard müssen nun vollständig in das VNet und Netzwerksicherheitsgruppen (NSG) integriert werden. NSG ermöglicht das Whitelisting von Datenflüssen sowie Konfigurationen, bei denen Kunden die vollständige Kontrolle mit einer NSG darüber besitzen, wann Datenverkehr an ihre Bereitstellung zulässig ist. Andere Konfigurationen müssen nicht zuvor abgeschlossen werden.

Ordnen Sie Subnetzen oder NICs von VM-Instanzen im Back-End-Pool eine NSG zu.  Dies gilt für Load Balancer Standard und Public IP Standard, wenn die Verwendung als öffentliche IP-Adresse auf Instanzebene erfolgt. Sie müssen eine explizite Whitelist erstellen, welcher Datenverkehr mit der NSG erlaubt ist, damit dieser erfolgen kann.

Weitere Informationen zu Netzwerksicherheitsgruppen und ihrer Anwendung in Ihrem Szenario finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

### <a name ="outboundconnections"></a>Ausgehende Verbindungen

 Load Balancer Standard stellt ausgehende Verbindungen für virtuelle Computer innerhalb des VNets bereit, wenn die Zuordnung zu einem Lastenausgleich mit portmaskierender SNAT (Source Network Address Translation) erfolgt.

Wenn eine öffentliche Load Balancer-Ressource VM-Instanzen zugeordnet ist, wird die Quelle jeder ausgehenden Verbindung aus dem privaten IP-Adressbereich des VNets an die öffentliche IP-Adresse des Load Balancer Front-Ends erneut geschrieben.  Load Balancer Standard verwendet einen neuen portmaskierenden SNAT-Algorithmus (Source Network Address Translation) für höhere Stabilität und Skalierung.  

Darüber hinaus sind ausgehende Verbindungen bei Verwendung mit einem zonenredundanten Front-End auch zonenredundant, und SNAT-Portzuordnungen überstehen Zonenfehler.

Der neue Algorithmus von Load Balancer Standard reserviert SNAT-Ports für jede Netzwerkschnittstelle des virtuellen Computers, wenn diese dem Pool gemäß den folgenden Ebenen hinzugefügt werden:

| Größe des Back-End-Pools | Reservierte SNAT-Ports |
| --- | --- |
| 1-50 | 1024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 |

SNAT Ports werden nicht direkt in die Anzahl der Verbindungen übersetzt. Ein SNAT-Port kann für mehrere eindeutige Ziele wiederverwendet werden.  Weitere Informationen finden Sie im Artikel zu [ausgehenden Verbindungen](load-balancer-outbound-connections.md).

Wenn der Back-End-Pool erhöht wird und von der Größe einer Ebene in die nächstgrößere Ebene übergeht, wird die Hälfte der zugewiesenen Ports freigegeben. Für jede Verbindung, die einem freigegebenen Port zugeordnet ist, erfolgt ein Timeout, und die Verbindung muss erneut hergestellt werden. Alle neuen Verbindungsversuche sind sofort erfolgreich. Wenn der Back-End-Pool aus der Größe einer Ebene auf den nächstkleineren Größenwert verringert wird, nehmen die verfügbaren SNAT-Ports zu. Vorhandene Verbindungen sind nicht betroffen.

Load Balancer Standard verfügt auch über eine zusätzliche Konfigurationsoption auf der Basis einzelner Regeln, damit Kunden steuern können, welches Front-End für die portmaskierende SNAT verwendet wird, wenn mehrere Front-Ends verfügbar sind.

Wenn nur Load Balancer Standard VM-Instanzen verarbeitet, sind ausgehende SNAT-Verbindungen nicht verfügbar. Sie können diese Funktion explizit wiederherstellen, indem Sie die VM-Instanzen auch einem öffentlichen Load Balancer oder die Public IP-Adressen auch als öffentliche IP-Adressen auf Instanzebene direkt jeder VM-Instanz zuweisen. Dies kann für einige Betriebssystem- und Anwendungsszenarien erforderlich sein. 

### <a name="port-forwarding"></a>Portweiterleitung

 Load Balancer vom Typ „Basic“ und „Standard“ bieten die Möglichkeit, NAT-Regeln für eingehenden Datenverkehr zum Zuordnen eines Front-End-Ports zu einer einzelnen Back-End-Instanz zu konfigurieren.  Es gibt viele Verwendungsmöglichkeiten für diese Funktion, z. B. das Bereitstellen von Remotedesktopprotokoll-Endpunkten oder SSH-Endpunkten. Auch eine Vielzahl anderer Anwendungsszenarien kommt in Betracht.

Load Balancer Standard stellt weiterhin Portweiterleitung über NAT-Regeln für eingehenden Datenverkehr bereit.  Bei Verwendung mit zonenredundanten Front-Ends werden NAT-Regeln für eingehenden Datenverkehr zonenredundant und bleiben bei Zonenfehlern erhalten.

### <a name="multiple-frontends"></a>Mehrere Front-Ends

Konfigurieren Sie mehrere Front-Ends für Entwurfsflexibilität, wenn Anwendungen das Bereitstellen mehrerer einzelner IP-Adressen erfordern (z. B. TLS-Websites oder Endpunkte von SQL AlwaysOn-Verfügbarkeitsgruppen).  Weitere Informationen finden Sie [hier](load-balancer-multivip-overview.md).

Load Balancer Standard bietet weiterhin mehrere Front-Ends, wenn es wünschenswert ist, einen bestimmten Anwendungsendpunkt für eine eindeutige IP-Adresse zur Verfügung zu stellen.

 Weitere Informationen finden Sie [hier](load-balancer-multivip-overview.md).

## <a name = "sku"></a>Informationen zu SKUs

SKUs sind nur im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar.  Diese Vorschau führt zwei SKUs („Basic“ und „Standard“) für Load Balancer- und Public IP-Ressourcen ein.  Die SKUs unterscheiden sich hinsichtlich der Funktionen und Leistungsmerkmale, Einschränkungen sowie einigen systeminternen Verhaltensweisen. Virtual Machines kann mit beiden SKUs verwendet werden. Für Load Balancer- und Public IP-Ressourcen bleiben die SKUs optionale Attribute. Wenn diese ausgelassen werden, wird standardmäßig „Basic“ verwendet.

>[!IMPORTANT]
>Die SKU einer Ressource kann nicht geändert werden.  Sie können die SKU einer vorhandenen Ressource nicht ändern.  

### <a name="load-balancer"></a>Lastenausgleichsmodul

Die [vorhandene Load Balancer-Ressource](load-balancer-overview.md) wird zur SKU „Basic“ und bleibt allgemein verfügbar und unverändert.

Die SKU „Load Balancer Standard“ ist ein neues Angebot und zurzeit als Vorschauversion verfügbar. Die API-Version 2017-08-01 für „Microsoft.Network/loadBalancers“ führt SKUs für die Ressource ein.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```
Bei Verwendung in einer Region, die auch Verfügbarkeitszonen anbietet, ist Load Balancer Standard automatisch zonenstabil, sofern keine Deklaration als zonal erfolgt ist.

### <a name="public-ip"></a>Öffentliche IP-Adresse

Die [vorhandene Public IP-Ressource](../virtual-network/virtual-network-ip-addresses-overview-arm.md) wird zur SKU „Basic“ und bleibt mit allen Funktionen, Leistungsmerkmalen und Einschränkungen allgemein verfügbar.

Die SKU „Public IP Standard“ ist ein neues Angebot und zurzeit als Vorschauversion verfügbar. Die API-Version 2017-08-01 für „Microsoft.Network/publicIPAddresses“ führt SKUs für die Ressource ein.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

Im Gegensatz zu Public IP Basic mit mehreren Zuordnungsmethoden verwendet Public IP Standard immer statische Zuordnung.

Bei Verwendung in einer Region, die auch Verfügbarkeitszonen anbietet, ist Public IP Standard automatisch zonenstabil, sofern keine Deklaration als zonal erfolgt ist.  Eine zonale öffentliche IP kann nicht von einer Zone in eine andere geändert werden.

## <a name="migration-between-skus"></a>Migration zwischen SKUs

SKUs sind nicht änderbar.  Gehen Sie folgendermaßen vor, wenn Sie von einer Ressourcen-SKU zu einer anderen wechseln möchten:

### <a name="migrating-from-basic-to-standard-sku"></a>Migrieren von der SKU „Basic“ zu „Standard“

1. Erstellen Sie eine neue Standard-Ressource (Load Balancer bzw. Public IP nach Bedarf), und erstellen Sie Ihre Regeln und Prüfdefinitionen erneut.
2. Entfernen Sie SKU-Ressourcen vom Typ „Basic“ (Public IP und LB) aus allen VM-Instanzen (dies schließt alle Instanzen einer Verfügbarkeitsgruppe ein).
3. Fügen Sie alle VM-Instanzen an die neuen SKU-Ressourcen vom Typ „Standard“ an.

### <a name="migrating-from-standard-to-basic-sku"></a>Migrieren von der SKU „Standard“ zu „Basic“:

1. Erstellen Sie eine neue Basic-Ressource (Load Balancer bzw. Public IP nach Bedarf), und erstellen Sie Ihre Regeln und Prüfdefinitionen erneut. 
2. Entfernen Sie SKU-Ressourcen vom Typ „Standard“ (Public IP und LB) aus allen VM-Instanzen (dies schließt alle Instanzen einer Verfügbarkeitsgruppe ein).
3. Fügen Sie alle VM-Instanzen an die neuen SKU-Ressourcen vom Typ „Basic“ an.

  >[!NOTE]
  >HA-Ports und Diagnose der Standard-SKU sind nur in der Standard-SKU verfügbar. Sie können nicht von Standard zu Basic migrieren und diese Funktionalität beibehalten.

Für Load Balancer- und Public IP-Ressourcen müssen übereinstimmende SKUs verwendet werden.  Es ist nicht möglich, Basic- und Standard-SKU-Ressourcen zu kombinieren oder einen virtuellen Computer, virtuelle Computer in einer Verfügbarkeitsgruppe oder eine VM-Skalierungsgruppe an beide gleichzeitig anzufügen.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Load Balancer Standard ist zurzeit in den folgenden Regionen verfügbar:
- USA (Ost) 2
- USA (Mitte)
- Nordeuropa
- USA, Westen-Mitte
- Europa, Westen
- Asien, Südosten

## <a name="service-limits--abilities-comparison"></a>Diensteinschränkungen und -funktionen im Vergleich

Azure-[Diensteinschränkungen für Netzwerke](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits#networking-limits) gelten pro Region und pro Abonnement. 

Die folgende Tabelle enthält einen Vergleich der Einschränkungen und Funktionen zwischen den Basic- und Standard-SKUs für Load Balancer:

| Lastenausgleichsmodul | Basic | Standard |
| --- | --- | --- |
| Größe des Back-End-Pools | Bis zu 100 | Bis zu 1.000 |
| Grenze des Back-End-Pools | Verfügbarkeitsgruppe | VNet, Region |
| Entwurf des Back-End-Pools | Virtuelle Computer in Verfügbarkeitsgruppe oder VM-Skalierungsgruppe in Verfügbarkeitsgruppe | Beliebige VM-Instanz im VNet |
| HA-Ports | Nicht unterstützt | Verfügbar |
| Diagnose | Eingeschränkt, nur öffentlich | Verfügbar |
| VIP-Verfügbarkeit  | Nicht unterstützt | Verfügbar |
| Schnelle IP-Mobilität | Nicht unterstützt | Verfügbar |
|Szenarien für Verfügbarkeitszonen | Nur zonal | Zonal, zonenredundant, zonenübergreifender Lastenausgleich |
| SNAT-Algorithmus für ausgehenden Datenverkehr | Bei Bedarf | Reserviert |
| Auswahl des SNAT-Front-Ends für ausgehenden Datenverkehr | Nicht konfigurierbar, mehrere Kandidaten | Optionale Konfiguration zum Verringern von Kandidaten |
| Netzwerksicherheitsgruppen (NSG) | Optional für NIC/Subnetz | Erforderlich |

Die folgende Tabelle enthält einen Vergleich der Einschränkungen und Funktionen zwischen den Basic- und Standard-SKUs für Public IP:

| Öffentliche IP-Adresse | Basic | Standard |
| --- | --- | --- |
| Szenarien für Verfügbarkeitszonen | Nur zonal | Zonenredundant (Standard), zonal (optional) | 
| Schnelle IP-Mobilität | Nicht unterstützt | Verfügbar |
| VIP-Verfügbarkeit | Nicht unterstützt | Verfügbar |
| Counters | Nicht unterstützt | Verfügbar |
| Netzwerksicherheitsgruppen (NSG) | Optional für NIC | Erforderlich |


## <a name="preview-sign-up"></a>Registrierung für die Vorschauversion

Für die Teilnahme an der Vorschau der Load Balancer Standard-SKU und der zugehörigen Public IP Standard-SKU registrieren Sie Ihr Abonnement mithilfe von PowerShell oder Azure CLI 2.0, um Zugriff zu erhalten.

- Registrierung mithilfe von PowerShell

    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

- Registrierung mithilfe von Azure CLI 2.0

    ```cli
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```

>[!NOTE]
>Registrierung der Load Balancer-Standardfeatures kann bis zu einer Stunde dauern.

>[!NOTE]
>Wenn Sie Load Balancer Standard mit [Verfügbarkeitszonen](https://aka.ms/availabilityzones) und [Ports mit Hochverfügbarkeit](https://aka.ms/haports) verwenden möchten, ist für diese Vorschauversionen eine separate Anmeldung erforderlich.  Befolgen Sie die entsprechenden Anweisungen.

## <a name="pricing"></a>Preise

Die Load Balancer Standard-SKU wird basierend auf den konfigurierten Regeln und verarbeiteten Daten abgerechnet.  Während des Vorschauzeitraums fallen keine Kosten an.  Weitere Informationen zu Preisen finden Sie auf den Seiten zu [Load Balancer](https://aka.ms/lbpreviewpricing) und [Public IP](https://aka.ms/lbpreviewpippricing).

Kunden können die Load Balancer-SKU vom Typ „Basic“ weiterhin kostenlos nutzen.

## <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten für die Preview und können jederzeit geändert werden:

- Load Balancer-Back-End-Instanzen dürfen sich zurzeit nicht in Peer-VNets befinden. Alle Back-End-Instanzen müssen sich in der gleichen Region befinden.
- SKUs sind nicht änderbar. Sie können die SKU einer vorhandenen Ressource nicht ändern.
- Sie können die Basic- oder Standard-SKU mit einem eigenständigen virtuellen Computer, allen VM-Instanzen in einer Verfügbarkeitsgruppe oder einer VM-Skalierungsgruppe verwenden. Ein eigenständiger virtueller Computer, alle VM-Instanzen in einer Verfügbarkeitsgruppe oder einer VM-Skalierungsgruppe können nicht mit beiden SKUs gleichzeitig verwendet werden. Das Kombinieren von SKUs ist unzulässig.
- Durch Verwenden eines internen Load Balancer Standard mit einer VM-Instanz (oder einen beliebigen Teil einer Verfügbarkeitsgruppe) werden [ausgehende SNAT-Standardverbindungen](load-balancer-outbound-connections.md) deaktiviert.  Sie können diese Funktion für einen eigenständigen virtuellen Computer oder VM-Instanzen in einer Verfügbarkeitsgruppe oder einer VM-Skalierungsgruppe wiederherstellen und ausgehende Verbindungen herstellen, indem Sie gleichzeitig eine öffentliche Load Balancer Standard- oder Public IP-Standard-SKU als öffentliche IP-Adresse auf Instanzebene derselben VM-Instanz zuweisen. Nachdem dieser Vorgang ausgeführt wurde, wird portmaskierende SNAT an eine öffentliche IP-Adresse erneut bereitgestellt.
- VM-Instanzen müssen möglicherweise in Verfügbarkeitsgruppen gruppiert werden, um die volle Skalierung des Back-End-Pools zu erreichen. Bis zu 150 Verfügbarkeitsgruppen und eigenständige virtuelle Computer können in einem einzelnen Back-End-Pool platziert werden.
- IPv6 wird nicht unterstützt.
- Im Kontext von Verfügbarkeitszonen ist ein Front-End nicht aus zonal in zonenredundant änderbar (und umgekehrt). Nachdem die Erstellung zonenredundant erfolgt ist, ist das Front-End immer zonenredundant. Wenn es als zonal erstellt wird, ist es immer zonal.
- Im Kontext von Verfügbarkeitszonen kann eine zonale öffentliche IP-Adresse nicht aus einer Zone in eine andere verschoben werden.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Load Balancer Basic](load-balancer-overview.md)
- Weitere Informationen zu [Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.


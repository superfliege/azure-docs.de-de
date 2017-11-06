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
ms.openlocfilehash: 08e4e22ae7e5d6f6efad458b4240a6d57090e865
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="azure-load-balancer-standard-overview-preview"></a>Übersicht: Azure Load Balancer Standard (Preview)

Die SKUs Azure Load Balancer Standard und Public IP Standard ermöglichen Ihnen zusammen das Erstellen hoch skalierbarer und zuverlässiger Architekturen. Anwendungen, die Load Balancer Standard verwenden, können neue Funktionen nutzen. Geringe Latenz, hoher Durchsatz und Skalierung sind für Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen verfügbar.

>[!NOTE]
> Die Load Balancer Standard-SKU ist zurzeit als Preview verfügbar. Während der Previewphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Verwenden Sie die allgemein verfügbare [Load Balancer Basic-SKU](load-balancer-overview.md) für Ihre Produktionsdienste. Die Features in dieser Preview wie z.B. [Verfügbarkeitszonen](https://aka.ms/availabilityzones) und [HA-Ports](https://aka.ms/haports) erfordern zurzeit eine getrennte Anmeldung. Befolgen Sie die entsprechenden Anweisungen für die Registrierung für diese Features, zusätzlich zur Registrierung für Load Balancer [Standard Preview](#preview-sign-up).

## <a name="why-use-load-balancer-standard"></a>Gründe für die Verwendung von Load Balancer Standard

Sie können Load Balancer Standard für das gesamte Spektrum virtueller Rechenzentren verwenden. Setzen Sie Load Balancer Standard von kleinen Bereitstellungen bis hin zu großen und komplexen Architekturen mit mehreren Zonen ein, um die folgenden Funktionen zu nutzen:

- [Skalierung auf Unternehmensniveau](#enterprisescale) kann mit Load Balancer-Standard erreicht werden. Dieses Feature kann mit jeder VM-Instanz (Virtual Machine, virtueller Computer) in einem virtuellen Netzwerk und mit bis zu 1.000 VM-Instanzen verwendet werden.

- [Neue Diagnoseerkenntnisse](#diagnosticinsights) unterstützen Sie beim Verstehen, beim Verwalten und bei der Problembehandlung dieser wichtigen Komponente des virtuellen Rechenzentrums. Verwenden Sie Azure Monitor (Preview), um neue mehrdimensionale Metriken für kontinuierliche Datenpfad-Integritätsmessungen anzuzeigen, zu filtern und zu gruppieren. Überwachen Sie Ihre Daten vom Front-End bis zum virtuellen Computer, und erfassen Sie Endpunktintegritätsüberprüfungen für TCP-Verbindungsversuche und ausgehende Verbindungen.

- [Netzwerksicherheitsgruppen](#nsg) sind nun für alle VM-Instanzen erforderlich, die Load Balancer Standard- oder Public IP Standard-SKUs zugeordnet sind. Netzwerksicherheitsgruppen (NSGs) bieten erweiterte Sicherheit für Ihr Szenario.

- [Hochverfügbarkeitsports (HA-Ports) bieten hohe Zuverlässigkeit](#highreliability) und Skalierung für virtuelle Netzwerkgeräte (NVAs) und andere Anwendungsszenarien. HA-Ports führen den Lastenausgleich für alle Ports auf einem internen Azure Load Balancer-Front-End in einem Pool von VM-Instanzen aus.

- [Ausgehende Verbindungen](#outboundconnections) verwenden nun ein neues SNAT-Portzuweisungsmodell (Source Network Address Translation), das bessere Stabilität und Skalierung bereitstellt.

- [Load Balancer Standard mit Verfügbarkeitszonen](#availabilityzones) kann verwendet werden, um zonenredundante und zonale Architekturen zu nutzen. Beide Architekturen können zonenübergreifenden Lastenausgleich enthalten. Sie können Zonenredundanz ohne Abhängigkeit von DNS-Einträgen erzielen. Eine einzelne IP-Adresse ist standardmäßig zonenredundant.  Eine einzelne IP-Adresse kann jeden virtuellen Computer in einem virtuellen Netzwerk innerhalb einer Region erreichen, die für alle Verfügbarkeitszonen gilt.


Load Balancer Standard können Sie in einer öffentlichen oder in einer internen Konfiguration verwenden. Dabei werden die folgenden grundlegenden Szenarien unterstützt:

- Lastenausgleich für eingehenden Datenverkehr an integre Back-End-Instanzen.
- Portweiterleitung von eingehendem Datenverkehr an eine einzelne Back-End-Instanz.
- Übersetzen von ausgehendem Datenverkehr aus einer privaten IP-Adresse im virtuellen Netzwerk in eine öffentliche IP-Adresse.

### <a name = "enterprisescale"></a>Skalierung auf Unternehmensniveau

 Mit Load Balancer Standard entwerfen Sie Ihr virtuelles Hochleistungsrechenzentrum und unterstützen alle TCP- oder UDP-Anwendungen. Verwenden Sie eigenständige VM-Instanzen oder bis zu 1.000 Instanzen von VM-Skalierungsgruppen in einem Back-End-Pool. Nutzen Sie weiterhin Weiterleitung mit niedriger Latenz, hohe Durchsatzleistung und Skalierung auf Millionen von Datenflüssen für einen vollständig verwalteten Azure-Dienst.
 
Load Balancer Standard kann Datenverkehr an eine beliebige VM-Instanz in einem virtuellen Netzwerk in einer Region weiterleiten. Die Größe des Back-End-Pools kann bis zu 1.000 Instanzen mit einer beliebigen Kombination der folgenden VM-Szenarien betragen:

- Eigenständige virtuelle Computer ohne Verfügbarkeitsgruppen
- Eigenständige virtuelle Computer mit Verfügbarkeitsgruppen
- VM-Skalierungsgruppen mit bis zu 1.000 Instanzen
- Mehrere VM-Skalierungsgruppen
- Kombination aus virtuellen Computern und VM-Skalierungsgruppen

Eine Anforderung für Verfügbarkeitsgruppen besteht nicht mehr. Sie können aber wahlweise Verfügbarkeitsgruppen verwenden, um die anderen Vorteile zu nutzen, die sie bereitstellen.

### <a name = "diagnosticinsights"></a>Diagnoseerkenntnisse

Load Balancer Standard bietet neue mehrdimensionale Diagnosefunktionen für öffentliche und interne Load Balancer-Konfigurationen. Diese neuen Metriken werden über Azure Monitor (Preview) bereitgestellt und nutzen alle zugehörigen Funktionen einschließlich der Möglichkeit für die Integration in verschiedene Downstreamconsumer.

| Metrik | Beschreibung |
| --- | --- |
| VIP-Verfügbarkeit | Load Balancer Standard wendet kontinuierlich den Datenpfad aus einer Region auf das Load Balancer-Front-End bis hin zum SDN-Stapel an, der Ihren virtuellen Computer unterstützt. Solange integre Instanzen verbleiben, folgt die Messung demselben Pfad wie der Datenverkehr mit Lastenausgleich Ihrer Anwendungen. Der Datenpfad, der von Ihren Kunden verwendet wird, wird ebenfalls überprüft. Die Messung ist für Ihre Anwendung nicht sichtbar und bewirkt keine Beeinträchtigung bei anderen Vorgängen.|
| DIP-Verfügbarkeit | Load Balancer Standard verwendet einen verteilten Integritätsprüfungsdienst, der die Integrität Ihres Anwendungsendpunkts gemäß Ihren Konfigurationseinstellungen überwacht. Diese Metrik stellt eine Aggregat- oder nach Endpunkt gefilterte Ansicht jedes einzelnen Instanzendpunkts im Load Balancer-Pool bereit.  Sie können sehen, wie Load Balancer die Integrität Ihrer Anwendung gemäß Ihrer Integritätsprüfungskonfiguration beurteilt.
| SYN-Pakete | Load Balancer Standard beendet keine TCP-Verbindungen und interagiert nicht mit TCP- oder UDP-Paketdatenflüssen. Datenflüsse und deren Handshakes erfolgen immer zwischen der Quelle und der VM-Instanz. Damit die Problembehandlung Ihrer TCP-Protokollszenarien auf einfachere Weise ausgeführt werden kann, können Sie SYN-Pakete verwenden, um zu verstehen, wie viele TCP-Verbindungsversuche vorgenommen werden. Die Metrik gibt die Anzahl der TCP-SYN-Pakete an, die empfangen wurden. Die Metrik kann ggf. auch Clients angeben, die versuchen, eine Verbindung mit Ihrem Dienst herzustellen.|
| SNAT-Verbindungen | Load Balancer Standard meldet die Anzahl der maskierten ausgehenden Verbindungen an das Front-End mit der öffentlichen IP-Adresse. SNAT Ports stellen eine erschöpfbare Ressource dar. Diese Metrik kann einen Hinweis darauf geben, in welchem Umfang Ihre Anwendung SNAT für ausgehende Verbindungen verwendet.|
| Byteleistungsindikatoren | Load Balancer Standard meldet die pro Front-End verarbeiteten Daten.|
| Paketleistungsindikatoren | Load Balancer Standard meldet die pro Front-End verarbeiteten Pakete.|

### <a name = "highreliability"></a>Hohe Zuverlässigkeit

Konfigurieren Sie Lastenausgleichsregeln, damit die Anwendung skaliert werden kann und hohe Zuverlässigkeit bietet. Sie können Regeln für einzelne Ports konfigurieren, oder Sie können die HA-Ports verwenden, um einen Lastenausgleich des gesamten Datenverkehrs unabhängig von der TCP- oder UDP-Portnummer auszuführen.  

Sie können das neue Feature „HA-Ports“ verwenden, um eine Vielzahl von Szenarien zu erschließen, z.B. Hochverfügbarkeit und Skalierung für interne NVAs. Das Feature ist auch für andere Szenarien hilfreich, in denen es unpraktisch oder nicht erwünscht ist, einzelne Port anzugeben. HA-Ports stellen Redundanz und Skalierung bereit, indem die erforderliche Anzahl von Instanzen ermöglicht wird. Ihre Konfiguration ist nicht mehr auf Aktiv/Passiv-Szenarien beschränkt. Ihre Integritätsprüfungskonfigurationen schützen Ihren Dienst, indem Datenverkehr nur an integre Instanzen weitergeleitet wird.

NVA-Anbieter können vollständig vom Anbieter unterstützte, robuste Szenarien für ihre Kunden bereitstellen. Die einzelne Fehlerquelle wird entfernt, und mehrere aktive Instanzen werden für die Skalierung unterstützt. Sie können auf zwei oder mehr Instanzen abhängig von den Funktionen Ihres Geräts skalieren. Wenden Sie sich an den NVA-Anbieter, um weitere Anleitungen für diese Szenarien zu erhalten.

### <a name = "availabilityzones"></a>Verfügbarkeitszonen

[!INCLUDE [availability-zones-preview-statement](../../includes/availability-zones-preview-statement.md)]

Vergrößern Sie die Anwendungsresilienz durch die Verwendung von Verfügbarkeitszonen in unterstützten Regionen. Verfügbarkeitszonen sind zurzeit als Preview in bestimmten Regionen verfügbar und erfordern ein zusätzliches Abonnement.

### <a name="automatic-zone-redundancy"></a>Automatische Zonenredundanz

Sie können auswählen, ob Load Balancer ein zonenredundantes oder zonales Front-End für jede Ihrer Anwendungen bereitstellen soll. Es ist einfach, mit Load Balancer Standard Zonenredundanz zu erstellen. Eine einzelne Front-End-IP-Adresse ist automatisch zonenredundant. Ein zonenredundantes Front-End wird von allen Verfügbarkeitszonen in einer Region gleichzeitig versorgt. Ein zonenredundanter Datenpfad wird für eingehende und ausgehende Verbindungen erstellt. Für Zonenredundanz in Azure sind nicht mehrere IP-Adressen und DNS-Einträge erforderlich. 

Zonenredundanz ist für öffentliche und interne Front-Ends verfügbar. Ihre öffentliche IP-Adresse und die private IP-Adresse des Front-Ends für Ihren internen Load Balancer können zonenredundant sein.

Verwenden Sie das folgende Skript zum Erstellen einer zonenredundanten öffentlichen IP-Adresse für Ihren internen Load Balancer. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu.

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

Verwenden Sie das folgende Skript zum Erstellen einer zonenredundanten Front-End-IP-Adresse für Ihren internen Load Balancer. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu.

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

Wenn Ihr Public IP-Front-End zonenredundant ist, werden ausgehenden Verbindungen, die aus VM-Instanzen erfolgen, automatisch zonenredundant. Das Front-End ist vor Zonenfehlern geschützt. Ihre SNAT-Portzuordnung überdauert auch Zonenfehler.

#### <a name="cross-zone-load-balancing"></a>Zonenübergreifender Lastenausgleich

Zonenübergreifender Lastenausgleich steht innerhalb einer Region für den Back-End-Pool zur Verfügung und bietet maximale Flexibilität für Ihre VM-Instanzen. Ein Front-End übermittelt unabhängig von der Verfügbarkeitszone der VM-Instanz Datenflüsse an jeden virtuellen Computer im virtuellen Netzwerk.

Sie können auch eine bestimmte Zone für Ihre Front-End- und Back-End-Instanzen angeben, um Ihren Datenpfad und Ihre Ressourcen mit einer bestimmten Zone zu verbinden.

Virtuelle Netzwerke und Subnetze werden nie durch eine Zone eingeschränkt. Definieren Sie einfach einen Back-End-Pool mit den gewünschten VM-Instanzen, und schon ist Ihre Konfiguration abgeschlossen.

#### <a name="zonal-deployments"></a>Zonale Bereitstellungen

Optional können Sie Ihr Lastenausgleichs-Front-End mit einer bestimmten Zone verbinden, indem Sie ein zonales Front-End definieren. Eine zonales Front-End wird ausschließlich von einer designierten einzelnen Verfügbarkeitszone versorgt. Wenn das Front-End mit zonalen VM-Instanzen kombiniert wird, können Sie Ressourcen mit bestimmten Zonen verbinden.

Eine öffentliche IP-Adresse, die in einer bestimmten Zone erstellt wird, ist immer nur in dieser Zone vorhanden. Es ist nicht möglich, die Zone einer öffentlichen IP-Adresse zu ändern. Erstellen Sie für eine öffentliche IP-Adresse, die an Ressourcen in mehreren Zonen angefügt werden kann, stattdessen eine zonenredundante öffentliche IP-Adresse.

Verwenden Sie das folgende Skript, um eine öffentliche IP-Adresse in der Verfügbarkeitszone 1 zu erstellen. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu.

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

Verwenden Sie das folgende Skript, um eine öffentliche IP-Adresse in der Verfügbarkeitszone 1 zu erstellen.

Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu. Definieren Sie außerdem die Eigenschaft **zones** in der Front-End-IP-Konfiguration für die untergeordnete Ressource.

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

Fügen Sie Zonenlastenausgleich für den Back-End-Pool hinzu, indem Sie die VM-Instanzen, die sich in einem virtuellen Netzwerk befinden, in den Pool einfügen.

Die Load Balancer Standard-Ressource ist immer regional und zonenredundant, wenn Verfügbarkeitszonen unterstützt werden. Sie können eine öffentliche IP-Adresse oder das interne Load Balancer-Front-End bereitstellen, das keine zugewiesene Zone in einer beliebigen Region aufweist. Unterstützung für Verfügbarkeitszonen wirkt sich nicht negativ auf die Bereitstellungsfunktion aus. Wenn einer Region später Verfügbarkeitszonen hinzugefügt werden, werden bereits bereitgestellte öffentliche IP-Adressen oder interne Load Balancer-Front-Ends automatisch zonenredundant. Ein zonenredundanter Datenpfad bedeutet nicht Paketverluste von 0 %.

### <a name = "nsg"></a>Netzwerksicherheitsgruppen

Load Balancer Standard und Public IP Standard müssen nun vollständig in das virtuelle Netzwerk integriert werden, das die Verwendung von Netzwerksicherheitsgruppen (NSGs) erfordert. NSGs ermöglichen eine Whitelist des Datenverkehrsflusses. Die können NSGs verwenden, um vollständige Kontrolle über den Datenverkehr an Ihre Bereitstellung zu erlangen. Sie müssen nicht mehr warten, bis andere Datenverkehrsflüsse abgeschlossen wurden.

Ordnen Sie NSGs Subnetzen oder den Netzwerkschnittstellen (NICs) von VM-Instanzen im Back-End-Pool zu. Verwenden Sie diese Konfiguration mit Load Balancer Standard und Public IP Standard, wenn die Verwendung als öffentliche IP-Adresse auf Instanzebene erfolgt. Die NSG muss den Datenverkehr, der zulässig sein soll, explizit in einer Whitelist erfassen, damit dieser Datenverkehr fließen kann.

Weitere Informationen zu NSGs und ihrer Verwendung in Ihrem Szenario finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

### <a name ="outboundconnections"></a>Ausgehende Verbindungen

Load Balancer Standard bietet ausgehende Verbindungen für virtuelle Computer, die sich innerhalb des virtuellen Netzwerks befinden, wenn der Lastenausgleich SNAT mit Portmaskierung verwendet. Der Portmaskierungs-SNAT-Algorithmus bietet höhere Stabilität und Skalierung.

Wenn eine öffentliche Load Balancer-Ressource mit VM-Instanzen verknüpft ist, wird jede ausgehende Verbindungsquelle erneut geschrieben. Die Quelle wird aus dem Adressraum der privaten Adresse des Front-Ends in die öffentliche IP-Adresse des Front-Ends des Lastenausgleichsmoduls erneut geschrieben.

Wenn ausgehende Verbindungen mit einem zonenredundanten Front-End verwendet werden, sind die Verbindungen auch zonenredundant, und SNAT-Portzuordnungen überstehen Zonenfehler.

Der neue Algorithmus in Load Balancer Standard nimmt Vorabzuordnungen von SNAT-Ports für die NIC jedes virtuellen Computers vor. Wenn dem Pool eine NIC hinzugefügt wird, sind die SNAT-Ports vorab basierend auf der Größe des Pools zugeordnet. Die folgende Tabelle zeigt die Port-Vorabzuordnungen für sechs Ebenen der Back-End-Poolgrößen:

| Poolgröße (VM-Instanzen) | SNAT-Ports mit Vorabzuordnungen |
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1.000 | 32 |

SNAT Ports werden nicht direkt in die Anzahl der ausgehenden Verbindungen übersetzt. Ein SNAT-Port kann für mehrere eindeutige Ziele wiederverwendet werden. Weitere Informationen finden Sie im Artikel [Ausgehenden Verbindungen](load-balancer-outbound-connections.md).

Wenn die Back-End-Poolgröße zunimmt und in eine höheren Ebene übergeht, wird die Hälfte der zugeordneten Ports freigegeben. Für Verbindungen, die einem freigegebenen Port zugeordnet sind, erfolgt ein Timeout, und sie müssen erneut hergestellt werden. Neue Verbindungsversuche sind sofort erfolgreich. Wenn sich die Back-End-Poolgröße verringert und in eine niedrigere Dienstebene übergeht, nimmt die Anzahl der verfügbaren SNA-Ports zu. In diesem Fall sind vorhandene Verbindungen nicht betroffen.

Load Balancer Standard weist eine zusätzliche Konfigurationsoption auf, die pro Regel verwendet werden kann. Sie können steuern, welches Front-End-für Portmaskierung und SNAT verwendet wird, wenn mehrere-Front-Ends verfügbar sind.

Wenn nur Load Balancer Standard VM-Instanzen verarbeitet, sind ausgehende SNAT-Verbindungen nicht verfügbar. Sie können diese Fähigkeit explizit wiederherstellen, indem Sie die VM-Instanzen auch einem öffentlichen Lastenausgleich zuweisen. Sie können jeder VM-Instanz auch direkt öffentliche IP-Adressen als öffentliche IP-Adressen auf Instanzebene zuweisen. Diese Konfigurationsoption kann für einige Betriebssystem- und Anwendungsszenarien erforderlich sein. 

### <a name="port-forwarding"></a>Portweiterleitung

Load Balancer vom Typ „Basic“ und „Standard“ bieten die Möglichkeit, NAT-Regeln für eingehenden Datenverkehr zum Zuordnen eines Front-End-Ports zu einer einzelnen Back-End-Instanz zu konfigurieren. Indem Sie diese Regeln konfigurieren, können Sie Remotedesktopprotokoll-Endpunkte und SSH-Endpunkte bereitstellen oder andere Anwendungsszenarien ausführen.

Load Balancer Standard stellt weiterhin Portweiterleitung über NAT-Regeln für eingehenden Datenverkehr bereit. Bei Verwendung mit zonenredundanten Front-Ends werden NAT-Regeln für eingehenden Datenverkehr zonenredundant und bleiben bei Zonenfehlern erhalten.

### <a name="multiple-front-ends"></a>Mehrere Front-Ends

Konfigurieren Sie mehrere Front-Ends für Entwurfsflexibilität, wenn Anwendungen das Bereitstellen mehrerer einzelner IP-Adressen erfordern, z.B. für TLS-Websites oder Endpunkte von SQL AlwaysOn-Verfügbarkeitsgruppen. 

Load Balancer Standard bietet weiterhin mehrere Front-Ends, wenn es erforderlich ist, einen bestimmten Anwendungsendpunkt für eine eindeutige IP-Adresse zur Verfügung zu stellen.

Weitere Informationen zum Konfigurieren von mehreren Front-End-IP-Adressen finden Sie unter [Konfiguration mit mehreren IP-Adressen](load-balancer-multivip-overview.md).

## <a name = "sku"></a>Informationen zu SKUs

SKUs sind nur im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar. Diese Preview führt zwei SKUs für Load Balancer- und Public IP-Ressourcen ein: „Basic“ und „Standard“. Die SKUs unterscheiden sich hinsichtlich der Funktionen und Leistungsmerkmale, Einschränkungen sowie einigen systeminternen Verhaltensweisen. Virtual Machines kann mit beiden SKUs verwendet werden. Für Load Balancer- und Public IP-Ressourcen bleiben die SKUs optionale Attribute. Wenn SKUs in einer Szenariodefinition ausgelassen werden, wird standardmäßig die SKU „Basic“ verwendet.

>[!IMPORTANT]
>Die SKU einer Ressource kann nicht geändert werden. Sie können die SKU einer vorhandenen Ressource nicht ändern.  

### <a name="load-balancer"></a>Lastenausgleichsmodul

Die [vorhandene Load Balancer-Ressource](load-balancer-overview.md) wird zur SKU „Basic“ und bleibt allgemein verfügbar und unverändert.

Die Load Balancer Standard-SKU ist neu und zurzeit als Preview verfügbar. Die API-Version für Microsoft.Network/loadBalancers vom 1. August 2017 fügt der Ressourcendefinition die Eigenschaft **sku** hinzu:

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
Load Balancer Standard ist in Regionen, die Verfügbarkeitszonen bieten, automatisch zonenstabil. Wenn der Load Balancer als zonal deklariert wurde, ist er nicht automatisch zonenstabil.

### <a name="public-ip"></a>Öffentliche IP-Adresse

Die [vorhandene Public IP-Ressource](../virtual-network/virtual-network-ip-addresses-overview-arm.md) wird zur SKU „Basic“ und bleibt mit allen Funktionen, Leistungsmerkmalen und Einschränkungen allgemein verfügbar.

Die Public IP Standard-SKU ist neu und zurzeit als Preview verfügbar. Die API-Version für Microsoft.Network/publicIPAddresses vom 1. August 2017 fügt der Ressourcendefinition die Eigenschaft **sku** hinzu:

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

Public IP Standard ist in Regionen, die Verfügbarkeitszonen bieten, automatisch zonenstabil. Wenn die Public IP als zonal deklariert wurde, ist sie nicht automatisch zonenstabil. Eine zonale öffentliche IP-Adresse kann nicht aus einer Zone in eine andere geändert werden.

## <a name="migration-between-skus"></a>Migration zwischen SKUs

SKUs sind nicht änderbar. Führen Sie die Schritte in diesem Abschnitt aus, um aus einer Ressourcen-SKU in eine andere zu wechseln.

### <a name="migrate-from-basic-to-standard-sku"></a>Migrieren von der SKU „Basic“ zu „Standard“

1. Erstellen Sie eine neue Standard-Ressource (Load Balancer bzw. Public IP nach Bedarf). Erstellen Sie Ihre Regeln und Prüfdefinitionen erneut.

2. Entfernen Sie die Basic-SKU-Ressourcen (Load Balancer und Public IPs nach Bedarf) aus allen VM-Instanzen. Stellen Sie sicher, dass auch alle VM-Instanzen einer Verfügbarkeitsgruppe entfernt werden.

3. Fügen Sie alle VM-Instanzen an die neuen SKU-Ressourcen vom Typ „Standard“ an.

### <a name="migrate-from-standard-to-basic-sku"></a>Migrieren von der SKU „Standard“ zu „Basic“

1. Erstellen Sie eine neue Basic-Ressource (Load Balancer bzw. Public IP nach Bedarf). Erstellen Sie Ihre Regeln und Prüfdefinitionen erneut. 

2. Entfernen Sie die Standard-SKU-Ressourcen (Load Balancer und Public IPs nach Bedarf) aus allen VM-Instanzen. Stellen Sie sicher, dass auch alle VM-Instanzen einer Verfügbarkeitsgruppe entfernt werden.

3. Fügen Sie alle VM-Instanzen an die neuen SKU-Ressourcen vom Typ „Basic“ an.

>[!IMPORTANT]
>
>Es bestehen Einschränkungen hinsichtlich der Nutzung der SKUs „Basic“ und „Standard“.
>
>HA-Ports und Diagnose der Standard-SKU sind nur in der SKU „Standard“ verfügbar. Sie können nicht von der SKU „Standard“ zur SKU „Basic“ migrieren und diese Funktionen beibehalten.
>
>Für Load Balancer- und Public IP-Ressourcen müssen übereinstimmende SKUs verwendet werden. Eine Kombination von Ressourcen vom SKU-Typ „Basic“ und „Standard“ ist nicht möglich. Sie können einen virtuellen Computer, virtuelle Computer in der gleichen Verfügbarkeitsgruppe oder eine VM-Skalierungsgruppe nicht gleichzeitig an beide SKUs anfügen.
>

## <a name="region-availability"></a>Regionale Verfügbarkeit

Load Balancer Standard ist zurzeit in den folgenden Regionen verfügbar:
- USA (Ost) 2
- USA (Mitte)
- Nordeuropa
- USA, Westen-Mitte
- Europa, Westen
- Asien, Südosten

## <a name="sku-service-limits-and-abilities"></a>SKU-Diensteinschränkungen und -Funktionen

Azure-[Diensteinschränkungen für Netzwerke](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits#networking-limits) gelten pro Region und pro Abonnement. 

Die folgende Tabelle vergleicht die Einschränkungen und Funktionen der Load Balancer-SKUs „Basic“ und „Standard“:

| Lastenausgleichsmodul | Basic | Standard |
| --- | --- | --- |
| Größe des Back-End-Pools | Bis zu 100 | Bis zu 1.000 |
| Begrenzung des Back-End-Pools | Verfügbarkeitsgruppe | Virtuelles Netzwerk, Region |
| Entwurf des Back-End-Pools | Virtuelle Computer in Verfügbarkeitsgruppe, VM-Skalierungsgruppe in Verfügbarkeitsgruppe | Eine beliebige VM-Instanz im virtuellen Netzwerk |
| HA-Ports | Nicht unterstützt | Verfügbar |
| Diagnose | Eingeschränkt, nur öffentlich | Verfügbar |
| VIP-Verfügbarkeit  | Nicht unterstützt | Verfügbar |
| Schnelle IP-Mobilität | Nicht unterstützt | Verfügbar |
|Szenarien für Verfügbarkeitszonen | Nur zonal | Zonal, zonenredundant, zonenübergreifender Lastenausgleich |
| SNAT-Algorithmus für ausgehenden Datenverkehr | Bei Bedarf | Reserviert |
| Auswahl des SNAT-Front-Ends für ausgehenden Datenverkehr | Nicht konfigurierbar, mehrere Kandidaten | Optionale Konfiguration zum Verringern von Kandidaten |
| Netzwerksicherheitsgruppen (NSG) | Optional für NIC/Subnetz | Erforderlich |

Die folgende Tabelle vergleicht die Einschränkungen und Funktionen der Public IP-SKUs „Basic“ und „Standard“:

| Öffentliche IP-Adresse | Basic | Standard |
| --- | --- | --- |
| Szenarien für Verfügbarkeitszonen | Nur zonal | Zonenredundant (Standard), zonal (optional) | 
| Schnelle IP-Mobilität | Nicht unterstützt | Verfügbar |
| VIP-Verfügbarkeit | Nicht unterstützt | Verfügbar |
| Counters | Nicht unterstützt | Verfügbar |
| Netzwerksicherheitsgruppen (NSG) | Optional für NIC | Erforderlich |


## <a name="preview-sign-up"></a>Registrierung für die Vorschauversion

Für die Teilnahme an der Preview der Load Balancer Standard-SKU und der zugehörigen Public IP Standard-SKU registrieren Sie Ihr Abonnement.  Wenn Sie Ihr Abonnement registrieren, erhalten Sie aus PowerShell oder Azure CLI 2.0 Zugriff. Führen Sie zum Registrieren die folgenden Schritte aus:

>[!NOTE]
>Es kann bis zu eine Stunde in Anspruch nehmen, bis die Registrierung der Load Balancer-Standardfeatures global wirksam wird. Wenn Sie Load Balancer Standard mit [Verfügbarkeitszonen](https://aka.ms/availabilityzones) und [Ports mit Hochverfügbarkeit](https://aka.ms/haports) verwenden möchten, ist für diese Vorschauversionen eine separate Anmeldung erforderlich. Befolgen Sie die entsprechenden Anweisungen für die Registrierung für diese Funktionen.

### <a name="sign-up-by-using-azure-cli-20"></a>Registrierung mithilfe von Azure CLI 2.0

1. Registrieren des Features beim Anbieter:

    ```cli
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. Dieser Vorgang kann bis zu 10 Minuten dauern. Sie können den Status des Vorgangs mithilfe des folgenden Befehls überprüfen:

    ```cli
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```
    
    Fahren Sie mit dem nächsten Schritt fort, wenn für die Featureregistrierung der Status „Registriert“ zurückgegeben wird:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowLBPreview",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Schließen Sie die Registrierung für die Preview ab, indem Sie Ihr Abonnement erneut beim Ressourcenanbieter registrieren:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-by-using-powershell"></a>Registrierung mithilfe von PowerShell

1. Registrieren des Features beim Anbieter:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. Dieser Vorgang kann bis zu 10 Minuten dauern. Sie können den Status des Vorgangs mithilfe des folgenden Befehls überprüfen:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    Fahren Sie mit dem nächsten Schritt fort, wenn für die Featureregistrierung der Status „Registriert“ zurückgegeben wird:
   
    ```
    FeatureName      ProviderName        RegistrationState
    -----------      ------------        -----------------
    AllowLBPreview   Microsoft.Network   Registered
    ```
    
3. Schließen Sie die Registrierung für die Preview ab, indem Sie Ihr Abonnement erneut beim Ressourcenanbieter registrieren:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```
 
## <a name="pricing"></a>Preise

Die Abrechnung für die Load Balancer-Standard-SKU basiert auf den konfigurierten Regeln und den verarbeiteten Daten. Während des Previewzeitraums fallen keine Kosten an. Weitere Informationen zu Preisen finden Sie auf den Seiten zu [Load Balancer](https://aka.ms/lbpreviewpricing) und [Public IP](https://aka.ms/lbpreviewpippricing).

Kunden können die Load Balancer-SKU vom Typ „Basic“ weiterhin kostenlos nutzen.

## <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten für die Preview und können jederzeit geändert werden:

- Load Balancer-Back-End-Instanzen dürfen sich zurzeit nicht in virtuellen Peernetzwerken befinden. Alle Back-End-Instanzen müssen sich in der gleichen Region befinden.
- SKUs sind nicht änderbar. Sie können die SKU einer vorhandenen Ressource nicht ändern.
- Beide SKUs können mit einem eigenständigen virtuellen Computer, VM-Instanzen in einer Verfügbarkeitsgruppe oder einer VM-Skalierungsgruppe verwendet werden. VM-Kombinationen dürfen nicht mit beiden SKUs gleichzeitig verwendet werden. Eine Konfiguration, die eine Mischung von SKUs enthält, ist unzulässig.
- Durch Verwenden eines internen Load Balancer Standard mit einer VM-Instanz (oder einem beliebigen Teil einer Verfügbarkeitsgruppe) werden [ausgehende SNAT-Standardverbindungen](load-balancer-outbound-connections.md) deaktiviert. Sie können diese Funktion auf einem eigenständigen virtuellen Computer, in VM-Instanzen in einer Verfügbarkeitsgruppe oder in einer VM-Skalierungsgruppe wiederherstellen. Sie können auch die Funktion zum Herstellen ausgehender Verbindungen wiederherstellen. Um diese Funktionen wiederherzustellen, weisen Sie der gleichen VM-Instanz gleichzeitig eine öffentliche Load Balancer Standard- oder öffentliche IP-Standard-SKU als eine öffentliche IP-Adresse auf Instanzebene zu. Nachdem diese Zuweisung abgeschlossen wurde, wird erneut portmaskierendes SNAT an eine öffentliche IP-Adresse bereitgestellt.
- VM-Instanzen müssen möglicherweise in Verfügbarkeitsgruppen gruppiert werden, um die volle Skalierung des Back-End-Pools zu erreichen. Bis zu 150 Verfügbarkeitsgruppen und eigenständige virtuelle Computer können in einem einzelnen Back-End-Pool platziert werden.
- IPv6 wird nicht unterstützt.
- Im Kontext von Verfügbarkeitszonen ist ein Front-End nicht aus zonal in zonenredundant änderbar (und umgekehrt). Nachdem ein Front-End als zonenredundant erstellt wurde, bleibt es zonenredundant. Nachdem ein Front-End als zonal erstellt wurde, bleibt es zonal.
- Im Kontext von Verfügbarkeitszonen kann eine zonale öffentliche IP-Adresse nicht aus einer Zone in eine andere verschoben werden.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Load Balancer Basic](load-balancer-overview.md).
- Weitere Informationen zu [Verfügbarkeitszonen](../availability-zones/az-overview.md).
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) in Azure.


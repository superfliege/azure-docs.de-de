---
title: "Übersicht über Azure-Netzwerksicherheit | Microsoft-Dokumentation"
description: "Enthält Informationen zu den Sicherheitsoptionen zum Steuern des Netzwerkdatenverkehr-Flusses zwischen Azure-Ressourcen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial
ms.openlocfilehash: c5b5d79a18d8c4d370b1deb506285519fdbfbcf8
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2017
---
# <a name="network-security"></a>Netzwerksicherheit

Sie können den Netzwerkdatenverkehr auf Ressourcen in einem virtuellen Netzwerk beschränken, indem Sie eine Netzwerksicherheitsgruppe verwenden. Eine Netzwerksicherheitsgruppe enthält eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen. 

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Jeder Netzwerkschnittstelle ist entweder keine oder eine Netzwerksicherheitsgruppe zugeordnet. Jede Netzwerkschnittstelle ist in einem Subnetz des [virtuellen Netzwerks](virtual-networks-overview.md) enthalten. Einem Subnetz kann auch keine oder eine Netzwerksicherheitsgruppe zugeordnet sein. 

Bei Anwendung auf ein Subnetz gelten Sicherheitsregeln für alle Ressourcen des Subnetzes. Zusätzlich zu Netzwerkschnittstellen verfügen Sie unter Umständen über Instanzen von anderen Azure-Diensten, z.B. HDInsight, VM-Skalierungsgruppen und Anwendungsdienstumgebungen, die im Subnetz bereitgestellt werden.

Wenn eine Netzwerksicherheitsgruppe sowohl einer Netzwerkschnittstelle als auch dem Subnetz der Netzwerkschnittstelle zugeordnet ist, werden Netzwerksicherheitsgruppen wie folgt angewendet:

- **Eingehender Datenverkehr**: Die Netzwerksicherheitsgruppe, die dem Subnetz der Netzwerkschnittstelle zugeordnet ist, wird zuerst ausgewertet. Der gesamte Datenverkehr, der über die dem Subnetz zugeordnete Netzwerksicherheitsgruppe zugelassen wird, wird dann von der Netzwerksicherheitsgruppe ausgewertet, die der Netzwerkschnittstelle zugeordnet ist. Es kann beispielsweise sein, dass Sie Zugriff in eingehender Richtung auf einen virtuellen Computer über Port 80 aus dem Internet benötigen. Wenn Sie eine Netzwerksicherheitsgruppe sowohl der Netzwerkschnittstelle als auch dem Subnetz der Netzwerkschnittstelle zuordnen, muss für die Netzwerksicherheitsgruppe, die dem Subnetz und der Netzwerkschnittstelle zugeordnet ist, Port 80 zugelassen werden. Falls Sie Port 80 nur für die Netzwerksicherheitsgruppe, die dem Subnetz zugeordnet ist, oder der Netzwerkschnittstelle des Subnetzes zugelassen haben, tritt für die Kommunikation aufgrund der Standardsicherheitsregeln ein Fehler auf. Ausführliche Informationen finden Sie unter [Standardsicherheitsregeln](#default-security-rules). Wenn Sie eine Netzwerksicherheitsgruppe nur auf das Subnetz oder die Netzwerkschnittstelle angewendet haben und die Netzwerksicherheitsgruppe eine Regel enthalten hat, für die beispielsweise der eingehende Datenverkehr über Port 80 zugelassen ist, ist die Kommunikation erfolgreich. 
- **Ausgehender Datenverkehr**: Die Netzwerksicherheitsgruppe, die der Netzwerkschnittstelle zugeordnet ist, wird zuerst ausgewertet. Der gesamte Datenverkehr, der über die Netzwerksicherheitsgruppe der Netzwerkschnittstelle zugelassen wird, wird dann von der Netzwerksicherheitsgruppe ausgewertet, die dem Subnetz zugeordnet ist.

Es ist für Sie unter Umständen nicht immer klar erkennbar, wenn Netzwerksicherheitsgruppen sowohl auf eine Netzwerkschnittstelle als auch auf ein Subnetz angewendet werden. Sie können die Aggregatregeln, die auf eine Netzwerkschnittstelle angewendet werden, leicht prüfen, indem Sie die [effektiven Sicherheitsregeln](virtual-network-manage-nsg-arm-portal.md) für eine Netzwerkschnittstelle anzeigen. Sie können auch in Azure Network Watcher die Funktion [Überprüfen des IP-Flusses](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verwenden, um zu ermitteln, ob die Kommunikation für eine Netzwerkschnittstelle in ein- oder ausgehender Richtung zulässig ist. Das Tool gibt an, ob die Kommunikation zugelassen ist und für welche Netzwerksicherheitsregel Datenverkehr zugelassen ist oder abgelehnt wird.
 
> [!NOTE]
> Netzwerksicherheitsgruppen sind Subnetzen oder virtuellen Computern und Clouddiensten zugeordnet, die im Rahmen des klassischen Bereitstellungsmodells bereitgestellt wurden, und nicht Netzwerkschnittstellen gemäß dem Resource Manager-Bereitstellungsmodell. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Eine Netzwerksicherheitsgruppe kann auf beliebig viele einzelne Netzwerkschnittstellen und Subnetze angewendet werden.

## <a name="security-rules"></a>Sicherheitsregeln

Eine Netzwerksicherheitsgruppe kann – innerhalb der [Grenzwerte](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) des Azure-Abonnements – null oder mehr Regeln enthalten. Für jede Regel werden die folgenden Eigenschaften angegeben:

|Eigenschaft  |Erklärung  |
|---------|---------|
|Name|Ein eindeutiger Name in der Netzwerksicherheitsgruppe.|
|Priority | Eine Zahl zwischen 100 und 4.096. Regeln werden in der Reihenfolge ihrer Priorität verarbeitet. Regeln mit niedrigeren Zahlen werden vor Regeln mit höheren Zahlen verarbeitet, weil die Priorität für niedrigere Zahlen höher ist. Nachdem sich für den Datenverkehr eine Übereinstimmung mit einer Regel ergibt, wird die Verarbeitung angehalten. Daher werden alle Regeln mit niedrigerer Priorität (höherer Zahl), die über die gleichen Attribute wie Regeln mit höheren Prioritäten verfügen, nicht verarbeitet.|
|Quelle oder Ziel| Beliebiges Element oder eine einzelne IP-Adresse, ein CIDR-Block (z.B. 10.0.0.0/24), ein Diensttag oder eine Anwendungssicherheitsgruppe. Informieren Sie sich über [Diensttags](#service-tags) und [Anwendungssicherheitsgruppen](#application-security-groups). Durch das Angeben eines Bereichs, eines Diensttags oder einer Anwendungssicherheitsgruppe haben Sie die Möglichkeit, weniger Sicherheitsregeln zu erstellen. Die Option zum Angeben mehrerer einzelner IP-Adressen und Bereiche (die Angabe mehrerer Diensttags oder Anwendungsgruppen ist nicht zulässig) in einer Regel befindet sich in der Vorschauphase und wird als „Ergänzte Sicherheitsregeln“ bezeichnet. Ergänzte Sicherheitsregeln können nur in Netzwerksicherheitsgruppen erstellt werden, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Es ist nicht möglich, mehrere IP-Adressen und IP-Adressbereiche in Netzwerksicherheitsgruppen anzugeben, die mit dem klassischen Bereitstellungsmodell erstellt wurden.|
|Protocol     | TCP, UDP oder „Any“ (Alle), also z.B. TCP, UDP und ICMP. Das Angeben von ICMP allein ist unzulässig. Wenn Sie ICMP benötigen, müssen Sie also „Any“ (Alle) verwenden. |
|Richtung| Gibt an, ob die Regel für ein- oder ausgehenden Datenverkehr gilt.|
|Portbereich     |Sie können einen einzelnen Port oder einen Bereich mit Ports angeben. Mögliche Angaben sind beispielsweise „80“ oder „10.000 - 10.005“. Das Angeben von Bereichen ermöglicht Ihnen die Erstellung von weniger Sicherheitsregeln. Die Option zum Angeben mehrerer einzelner Ports und Portbereiche in einer Regel befindet sich in der Vorschauphase und wird als „ergänzte Sicherheitsregeln“ bezeichnet. Lesen Sie sich vor dem Verwenden von ergänzten Sicherheitsregeln die wichtigen Informationen unter [Vorschaufeatures](#preview-features) durch. Ergänzte Sicherheitsregeln können nur in Netzwerksicherheitsgruppen erstellt werden, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. In Netzwerksicherheitsgruppen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können Sie in derselben Sicherheitsregel nicht mehrere Ports oder Portbereiche angeben.   |
|Aktion     | Zulassen oder Verweigern        |

**Überlegungen**

- **Virtuelle IP des Hostknotens**: Grundlegende Infrastrukturdienste wie DHCP, DNS und die Systemüberwachung werden über die virtualisierten Host-IP-Adressen 168.63.129.16 und 169.254.169.254 bereitgestellt. Diese öffentlichen IP-Adressen gehören Microsoft und sind die einzigen virtuellen IP-Adressen, die in allen Regionen zu diesem Zweck verwendet werden. Die Adressen werden der physischen IP-Adresse des Servercomputers (Hostknoten) zugeordnet, der den virtuellen Computer hostet. Der Hostknoten fungiert als DHCP-Relay, als rekursiver DNS-Resolver und als Integritätstestquelle für den Load Balancer und den Computer. Bei der Kommunikation mit diesen IP-Adressen handelt es sich nicht um einen Angriff. Wenn Sie den Datenverkehr an bzw. von diesen IP-Adressen blockieren, funktioniert ein virtueller Computer unter Umständen nicht richtig.
- **Lizenzierung (Schlüsselverwaltungsdienst)**: Die auf virtuellen Computern ausgeführten Windows-Images müssen lizenziert werden. Zum Sicherstellen der Lizenzierung wird eine entsprechende Anforderung an die Hostserver des Schlüsselverwaltungsdiensts gesendet, die solche Abfragen verarbeiten. Die Anforderung wird in ausgehender Richtung über Port 1688 gesendet.
- **Virtuelle Computer in Pools mit Lastenausgleich**: Der angewendete Quellport und -adressbereich stammen vom Ausgangscomputer, nicht vom Lastenausgleich. Der Zielport und -adressbereich sind für den Zielcomputer bestimmt, nicht für den Lastenausgleich.
- **Azure-Dienstinstanzen**: Instanzen mehrerer Azure-Dienste, z.B. HDInsight, Anwendungsdienstumgebungen und VM-Skalierungsgruppen, werden in Subnetzen virtueller Computer bereitgestellt. Machen Sie sich auf jeden Fall mit den Portanforderungen für die einzelnen Dienste vertraut, bevor Sie eine Netzwerksicherheitsgruppe auf das Subnetz anwenden, in dem die Ressource bereitgestellt wurde. Wenn Sie den Datenverkehr für Ports verweigern, die für den Dienst benötigt werden, funktioniert der Dienst nicht richtig. 

Sicherheitsregeln sind zustandsbehaftet. Wenn Sie beispielsweise eine Sicherheitsregel für Datenverkehr in ausgehender Richtung an eine beliebige Adresse über Port 80 angeben, ist es nicht erforderlich, für die Antwort auf den ausgehenden Datenverkehr eine Sicherheitsregel für Datenverkehr in eingehender Richtung anzugeben. Sie müssen nur dann eine Sicherheitsregel für Datenverkehr in eingehender Richtung angeben, wenn die Kommunikation extern initiiert wird. Dies gilt auch für den umgekehrten Fall. Wenn eingehender Datenverkehr über einen Port zugelassen wird, ist es nicht erforderlich, für die Beantwortung des Datenverkehrs über den Port eine Sicherheitsregel für Datenverkehr in ausgehender Richtung anzugeben. Informationen zu den Grenzwerten beim Erstellen von Sicherheitsregeln finden Sie unter [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Ergänzte Sicherheitsregeln

Mit ergänzten Sicherheitsregeln wird die Sicherheitsdefinition für virtuelle Netzwerke vereinfacht, da Sie umfangreichere und komplexe Netzwerksicherheitsregeln mit weniger Regeln definieren können. Sie können mehrere Ports, mehrere explizite IP-Adressen, Diensttags und Anwendungssicherheitsgruppen zu einer einzelnen Sicherheitsregel zusammenfassen, die leicht verständlich ist. Verwenden Sie ergänzte Regeln in den Feldern für die Quelle, das Ziel und den Port einer Regel. Beim Erstellen einer Regel können Sie mehrere explizite IP-Adressen, CIDR-Bereiche und Ports angeben. Kombinieren Sie ergänzte Sicherheitsregeln mit Diensttags oder Anwendungssicherheitsgruppen, um die Wartung Ihrer Sicherheitsregeldefinition zu vereinfachen. 

Ergänzte Sicherheitsregeln sind als Vorschauversion verfügbar. Informationen zu den Grenzwerten beim Erstellen von ergänzten Sicherheitsregeln finden Sie unter [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Features, die sich in der Vorschauphase befinden, weisen nicht den gleichen Grad an Verfügbarkeit und Zuverlässigkeit wie bereits allgemein verfügbare Features auf. Die Features sind nur in den folgenden Regionen verfügbar: „USA, Osten“, „USA, Westen“, „USA, Westen 2“, „USA, Westen-Mitte“, „Australien, Osten“, „Australien, Südosten“ und „Vereinigtes Königreich, Süden“.

## <a name="default-security-rules"></a>Standardsicherheitsregeln

Wenn eine Netzwerksicherheitsgruppe keinem Subnetz und keiner Netzwerkschnittstelle zugeordnet ist, wird der gesamte Datenverkehr für das Subnetz bzw. die Netzwerkschnittstelle in ein- und ausgehender Richtung zugelassen. Nachdem eine Netzwerksicherheitsgruppe erstellt wurde, erstellt Azure darin die folgenden Standardregeln:

### <a name="inbound"></a>Eingehend

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priority|Quelle|Quellports|Ziel|Zielports|Protocol|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0 - 65535|VirtualNetwork|0 - 65535|Alle|ZULASSEN|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priority|Quelle|Quellports|Ziel|Zielports|Protocol|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0 - 65535|0.0.0.0/0|0 - 65535|Alle|ZULASSEN|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priority|Quelle|Quellports|Ziel|Zielports|Protocol|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0 - 65535|0.0.0.0/0|0 - 65535|Alle|VERWEIGERN|

### <a name="outbound"></a>Ausgehend

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priority|Quelle|Quellports| Ziel | Zielports | Protocol | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0 - 65535 | VirtualNetwork | 0 - 65535 | Alle | ZULASSEN |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Priority|Quelle|Quellports| Ziel | Zielports | Protocol | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0 - 65535 | Internet | 0 - 65535 | Alle | ZULASSEN |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priority|Quelle|Quellports| Ziel | Zielports | Protocol | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0 - 65535 | 0.0.0.0/0 | 0 - 65535 | Alle | VERWEIGERN |

In den Spalten **Quelle** und **Ziel** handelt es sich bei *VirtualNetwork*, *AzureLoadBalancer* und *Internet* um [Diensttags](#tags) und nicht um IP-Adressen. In der Protokollspalte steht **Alle** für TCP, UDP und ICMP. Beim Erstellen einer Regel können Sie „TCP“, „UDP“ oder „Alle“ angeben, aber das Angeben von ICMP allein ist nicht möglich. Falls für Ihre Regel ICMP benötigt wird, müssen Sie als Protokoll daher *Alle* auswählen. *0.0.0.0/0* in den Spalten **Quelle** und **Ziel** steht für alle Adressen.
 
Sie können die Standardregeln nicht entfernen, aber Sie können sie außer Kraft setzen, indem Sie Regeln mit höheren Prioritäten erstellen.

## <a name="service-tags"></a>Diensttags

 Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern. Sie können kein eigenes Diensttag erstellen und auch nicht angeben, welche IP-Adressen in einem Tag enthalten sind. Microsoft verwaltet die Adresspräfixe, die mit dem Diensttag abgedeckt werden, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Die folgenden Diensttags können zum Definieren von Sicherheitsregeln verwendet werden. Ihre Namen unterscheiden sich je nach [Azure-Bereitstellungsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) leicht.

* **VirtualNetwork** (*Resource Manager) (**VIRTUAL_NETWORK** für klassisch): Dieses Tag enthält den VM-Adressraum (alle für das virtuelle Netzwerk definierten CIDR-Bereiche), alle verbundenen lokalen Adressräume und [per Peering verknüpfte](virtual-network-peering-overview.md) virtuelle Netzwerke oder virtuelle Netzwerke, die mit einem [VM-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verbunden sind.
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** für klassisch): Dieses Tag symbolisiert den Lastenausgleich der Azure-Infrastruktur. Das Tag wird in eine [Azure-Datencenter-IP-Adresse](https://www.microsoft.com/download/details.aspx?id=41653) umgewandelt, die als Ausgangspunkt für die Integritätstests von Azure fungiert. Sie können diese Regel außer Kraft setzen, wenn Sie den Lastenausgleich von Azure nicht verwenden.
* **Internet** (Resource Manager) (**INTERNET** klassisch): Dieses Tag stellt den IP-Adressraum dar, der außerhalb des virtuellen Netzwerks liegt und über das öffentliche Internet erreichbar ist. Der Adressbereich schließt den [Azure-eigenen öffentlichen IP-Adressraum](https://www.microsoft.com/download/details.aspx?id=41653) ein.
* **AzureTrafficManager** (nur Resource Manager): Mit diesem Tag wird der IP-Adressraum für den Azure Traffic Manager-Dienst angegeben.
* **Storage** (nur Resource Manager): Mit diesem Tag wird der IP-Adressraum für den Azure Storage-Dienst angegeben. Wenn Sie *Storage* als Wert angeben, wird der Datenverkehr für den Speicher zugelassen oder verweigert. Falls Sie den Zugriff auf den Speicher nur für eine bestimmte [Region](https://azure.microsoft.com/regions) zulassen möchten, können Sie die Region angeben. Wenn Sie den Zugriff auf Azure Storage beispielsweise nur für die Region „USA, Osten“ zulassen möchten, können Sie *Storage.EastUS* als Diensttag angeben. Zusätzliche verfügbare regionale Diensttags: Storage.AustraliaEast, Storage.AustraliaSoutheast, Storage.EastUS, Storage.UKSouth, Storage.WestCentralUS, Storage.WestUS und Storage.WestUS2. Das Tag steht für den Dienst, aber nicht für bestimmte Instanzen des Diensts. Beispielsweise steht das Tag für den Azure Storage-Dienst, aber nicht für ein bestimmtes Azure Storage-Konto.
* **Sql** (nur Resource Manager): Dieses Tag steht für die Adresspräfixe der Azure SQL-Datenbank- und Azure SQL Data Warehouse-Dienste. Sie können für dieses Diensttag nur bestimmte Regionen angeben. Wenn Sie den Zugriff auf Azure SQL-Datenbank beispielsweise nur für die Region „USA, Osten“ zulassen möchten, können Sie *Sql.EastUS* als Diensttag angeben. Es ist nicht möglich, nur Sql für alle Azure-Regionen anzugeben, sondern Sie müssen die Regionen einzeln angeben. Andere verfügbare regionale Diensttags: Sql.AustraliaEast, Sql.AustraliaSoutheast, Sql.EastUS, Sql.UKSouth, Sql.WestCentralUS, Sql.WestUS und Sql.WestUS2. Das Tag steht für den Dienst, aber nicht für bestimmte Instanzen des Diensts. Beispielsweise steht das Tag für den Azure SQL-Datenbank-Dienst, aber nicht für eine bestimmte Azure SQL-Datenbank.

> [!WARNING]
> Die Diensttags „AzureTrafficManager“, „Storage“ und „Sql“ sind als Vorschauversionen verfügbar. Features, die sich in der Vorschauphase befinden, weisen nicht den gleichen Grad an Verfügbarkeit und Zuverlässigkeit wie bereits allgemein verfügbare Features auf. Die Diensttags sind nur in den folgenden Regionen verfügbar: „USA, Osten“, „USA, Westen“, „USA, Westen 2“, „USA, Westen-Mitte“, „Australien, Osten“, „Australien, Südosten“ und „Vereinigtes Königreich, Süden“.

> [!NOTE]
> Wenn Sie einen Dienstendpunkt für ein virtuelles Netzwerk für einen Dienst implementieren, z.B. Azure Storage oder Azure SQL-Datenbank, fügt Azure eine Route zu einem Subnetz des virtuellen Netzwerks für den Dienst hinzu. Die Adresspräfixe für die Route sind die gleichen Adresspräfixe bzw. CIDR-Bereiche wie für das entsprechende Diensttag.

## <a name="application-security-groups"></a>Anwendungssicherheitsgruppen

Mit Anwendungssicherheitsgruppen können Sie die Netzwerksicherheit als natürliche Erweiterung einer Anwendungsstruktur konfigurieren und virtuelle Computer gruppieren und basierend auf diesen Gruppen Netzwerksicherheitsrichtlinien definieren. Dieses Feature ermöglicht Ihnen die bedarfsabhängige Wiederverwendung Ihrer Sicherheitsrichtlinie, ohne dass Sie explizite IP-Adressen manuell warten müssen. Die Plattform übernimmt die komplexe Verarbeitung von expliziten IP-Adressen und mehreren Regelsätzen, damit Sie sich auf Ihre Geschäftslogik konzentrieren können.

Sie können eine Anwendungssicherheitsgruppe in einer Sicherheitsregel als Quelle und Ziel angeben. Nachdem Sie Ihre Sicherheitsrichtlinie definiert haben, können Sie virtuelle Computer erstellen und die Netzwerkschnittstellen auf dem virtuellen Computer einer Anwendungssicherheitsgruppe zuweisen. Die Richtlinie wird in Abhängigkeit davon angewendet, ob eine Netzwerkschnittstelle eines virtuellen Computers Mitglied der Anwendungssicherheitsgruppe ist. Im folgenden Beispiel wird veranschaulicht, wie Sie eine Anwendungssicherheitsgruppe für alle Webserver in Ihrem Abonnement nutzen können:

1. Erstellen Sie eine Anwendungssicherheitsgruppe mit dem Namen *WebServers*.
2. Erstellen Sie eine Netzwerksicherheitsgruppe mit dem Namen *MyNSG*.
3. Erstellen Sie in der Netzwerksicherheitsgruppe eine Sicherheitsregel für eingehenden Datenverkehr. Geben Sie das Diensttag *Internet* als Quelladresse und die Anwendungssicherheitsgruppe *WebServers* als Zieladresse an, und lassen Sie die Ports 80 und 443 zu.
4. Stellen Sie einen virtuellen Computer bereit, auf dem eine Webserveranwendung ausgeführt wird. Geben Sie an, dass die Netzwerkschnittstelle auf dem virtuellen Computer Mitglied der Anwendungssicherheitsgruppe *WebServers* ist. Die Ports 80 und 443 sind für den virtuellen Computer dann zugelassen. Die Ports sind außerdem für alle nachfolgend erstellten Webserver zugelassen, die Sie zu Mitgliedern der Anwendungssicherheitsgruppe *WebServers* machen. 

Wenn Sie andere Regeln erstellen und andere Anwendungssicherheitsgruppen als Ziel angeben, werden diese Regeln nicht auf die Webserver des vorherigen Beispiels angewendet. Regeln, in denen eine Anwendungssicherheitsgruppe angegeben ist, werden nur auf Netzwerkschnittstellen angewendet, bei denen es sich um Mitglieder der Anwendungssicherheitsgruppe handelt. Anwendungssicherheitsgruppen ermöglichen zusammen mit ergänzten Sicherheitsregeln und Diensttags die Erstellung einer minimalen Anzahl von Netzwerksicherheitsgruppen zum Verwalten der Netzwerksicherheit in Ihrem Abonnement.
 
Informationen zu den Grenzwerten beim Erstellen von Anwendungssicherheitsgruppen und zum Angeben in Sicherheitsregeln finden Sie unter [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Anwendungssicherheitsgruppen sind als Vorschauversion verfügbar. Vor der Verwendung von Anwendungssicherheitsgruppen müssen Sie diese entsprechend registrieren, indem Sie die Schritte 1 bis 5 unter [Create a network security group with application security groups](create-network-security-group-preview.md#powershell) (Erstellen einer Netzwerksicherheitsgruppe mit Anwendungssicherheitsgruppen) ausführen und die Informationen unter [Vorschaufeatures](#preview-features) lesen. Für Anwendungssicherheitsgruppen gelten folgende Einschränkungen:

-   Alle Netzwerkschnittstellen innerhalb einer Anwendungssicherheitsgruppe müssen sich im gleichen virtuellen Netzwerk befinden. Einer Anwendungssicherheitsgruppe können keine Netzwerkschnittstellen aus verschiedenen virtuellen Netzwerken hinzugefügt werden. Das virtuelle Netzwerk, in dem sich die erste Netzwerkschnittstelle befindet, die der Anwendungssicherheitsgruppe zugewiesen wurde, definiert das virtuelle Netzwerk, in dem sich alle später zugewiesenen Netzwerkschnittstellen befinden müssen.
- Wenn Sie Anwendungssicherheitsgruppen als Quelle und Ziel in einer Sicherheitsregel angeben, müssen sich die Netzwerkschnittstellen in beiden Anwendungssicherheitsgruppen im gleichen virtuellen Netzwerk befinden. Wenn also beispielsweise ASG1 Netzwerkschnittstellen aus VNet1 und ASG2 Netzwerkschnittstellen aus VNet2 enthält, kann ASG1 nicht als Quelle und ASG2 nicht als Ziel in einer Regel zugewiesen werden, da sich alle Netzwerkschnittstellen in VNet1 befinden müssen. 

Features, die sich in der Vorschauphase befinden, weisen nicht den gleichen Grad an Verfügbarkeit und Zuverlässigkeit wie bereits allgemein verfügbare Features auf. Vor der Verwendung von Anwendungssicherheitsgruppen müssen Sie diese zuerst entsprechend registrieren. Die Features sind nur in der folgenden Region verfügbar: USA, Westen-Mitte.

## <a name="next-steps"></a>Nächste Schritte

* Arbeiten Sie das Tutorial [Erstellen einer Netzwerksicherheitsgruppe](virtual-networks-create-nsg-arm-pportal.md) durch.
* Arbeiten Sie das Tutorial [Create a network security group with application security groups](create-network-security-group-preview.md) (Erstellen einer Netzwerksicherheitsgruppe mit Anwendungssicherheitsgruppen) durch.

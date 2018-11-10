---
title: Einrichten der Konnektivität zwischen virtuellen Computern und SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Einrichten der Konnektivität von virtuellen Computern für die Nutzung von SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90d920a501d27ca21b1c2b7b7f5491cebb2c2822
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233717"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Verbinden von Azure-VMs mit HANA (große Instanzen)

Wie bereits unter [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) beschrieben, sieht die minimale Bereitstellung von HANA (große Instanzen) mit der SAP-Anwendungsebene in Azure wie folgt aus:

![Mit SAP HANA in Azure (große Instanzen) und lokal verbundenes Azure-VNET](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Wenn wir uns das virtuelle Azure-Netzwerk genauer ansehen, wird klar, dass Folgendes benötigt wird:

- Die Definition eines virtuellen Azure-Netzwerks, in dem Sie die VMs der SAP-Anwendungsebene bereitstellen.
- Die Definition eines Standardsubnetzes im virtuellen Azure-Netzwerk, in dem die VMs tatsächlich bereitgestellt werden.
- Das erstellte virtuelle Azure-Netzwerk muss mindestens über ein VM-Subnetz und ein Azure ExpressRoute-Subnetz mit einem Gateway für virtuelle Netzwerke verfügen. Diese Subnetze sollten den IP-Adressbereichen so zugewiesen werden, wie dies in den folgenden Abschnitten angegeben und beschrieben ist.

Wir sehen uns die Erstellung von virtuellen Azure-Netzwerken für HANA (große Instanzen) jetzt etwas genauer an.

## <a name="create-the-azure-virtual-network-for-hana-large-instances"></a>Erstellen des virtuellen Azure-Netzwerks für HANA (große Instanzen)

>[!Note]
>Das virtuelle Azure-Netzwerk für HANA (großen Instanzen) muss mit dem Azure Resource Manager-Bereitstellungsmodell erstellt werden. Das ältere Azure-Bereitstellungsmodell, üblicherweise als klassisches Bereitstellungsmodell bezeichnet, wird von der Lösung HANA (großen Instanzen) nicht unterstützt.

Sie können das Azure-Portal, PowerShell, eine Azure-Vorlage oder die Azure CLI nutzen, um das virtuelle Netzwerk zu erstellen. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). Im folgenden Beispiel betrachten wir ein virtuelles Netzwerk, das mit dem Azure-Portal erstellt wird.

Wir sehen, wie die Definitionen des virtuellen Netzwerks mit den unterschiedlichen IP-Adressbereichen zusammenhängen. Mit **Adressraum** ist der Adressraum gemeint, der für das virtuelle Azure-Netzwerk verwendet werden kann. Dieser Adressraum ist auch der Adressbereich, der im virtuellen Netzwerk für die BGP-Routenweitergabe verwendet wird. Dieser **Adressraum** ist hier dargestellt:

![Adressraum eines virtuellen Azure-Netzwerks im Azure-Portal](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Im vorherigen Beispiel mit 10.16.0.0/16 wurde für das virtuelle Azure-Netzwerk ein relativ großer und breiter IP-Adressbereich bereitgestellt. Aus diesem Grund können alle IP-Adressbereiche nachfolgender Subnetze dieses virtuellen Netzwerks innerhalb dieses Adressraums liegen. Normalerweise empfehlen wir für einzelne virtuelle Netzwerke in Azure keine Adressbereiche dieser Größe. Wir betrachten nun aber die Subnetze, die im virtuellen Azure-Netzwerk definiert sind:

![Subnetze des virtuellen Azure-Netzwerks und zugehörige IP-Adressbereiche](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Wir sehen uns ein virtuelles Netzwerk mit einem ersten VM-Subnetz (hier als „default“ bezeichnet) und einem Subnetz mit dem Namen „GatewaySubnet“ an.

Im folgenden Abschnitt wird der IP-Adressbereich des Subnetzes, das in der Abbildung den Namen „default“ hat, als **IP-Adressbereich des Azure-VM-Subnetzes** bezeichnet. Wir bezeichnen den IP-Adressbereich des Gatewaysubnetzes als **IP-Adressbereich des Gatewaysubnetzes für virtuelle Netzwerke**. 

In den beiden obigen Grafiken umfasst der **Adressraum des virtuellen Netzwerks** sowohl den **IP-Adressbereich des Azure-VM-Subnetzes** als auch den **IP-Adressbereich des Gatewaysubnetzes für virtuelle Netzwerke**. 

Um Ihre IP-Adressbereiche beizubehalten oder spezifisch festzulegen, können Sie den **VNET-Adressraum** eines virtuellen Netzwerks auf die spezifischen Bereiche beschränken, die von den einzelnen Subnetzen verwendet werden. Sie können den **VNET-Adressraum** eines virtuellen Netzwerks wie hier gezeigt als mehrere spezifische Bereiche definieren:

![Adressraum für virtuelles Azure-Netzwerk mit zwei Bereichen](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In diesem Fall hat der **VNET-Adressraum** zwei definierte Bereiche. Dies sind die gleichen IP-Adressbereiche, die für den **IP-Adressbereich des Azure-VM-Subnetzes** und den **IP-Adressbereich des Gatewaysubnetzes für virtuelle Netzwerke** definiert wurden.

Für diese Mandantensubnetze (VM-Subnetze) können Sie einen beliebigen Benennungsstandard verwenden. Es darf aber **immer nur ein einziges Gatewaysubnetz für jedes virtuelle Netzwerk vorhanden sein**, für das eine Verbindung mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen) hergestellt wird. Und **dieses Gatewaysubnetz muss den Namen „GatewaySubnet“ haben**, um sicherzustellen, dass das ExpressRoute-Gateway richtig angeordnet ist.

> [!WARNING] 
> Es ist wichtig, dass das Gatewaysubnetz immer „GatewaySubnet“ heißt.

Sie können mehrere VM-Subnetze und nicht zusammenhängende Adressbereiche verwenden. Diese Adressbereiche müssen durch den **VNET-Adressraum** des virtuellen Netzwerks abgedeckt sein. Sie können in aggregierter Form oder als Liste mit den genauen Bereichen der VM-Subnetze und des Gatewaysubnetzes vorliegen.

Hier ist eine Zusammenfassung der wichtigen Fakten zu einem virtuellen Azure-Netzwerk angegeben, für das eine Verbindung mit HANA (große Instanzen) hergestellt wird:

- Sie müssen den **VNET-Adressraum** an Microsoft übermitteln, wenn Sie die erste Bereitstellung von HANA (große Instanzen) durchführen. 
- Der **VNET-Adressraum** kann ein größerer Bereich sein, der den Bereich für die IP-Adressbereiche des Azure-VM-Subnetzes und den IP-Adressbereich des VNET-Gatewaysubnetzes abdeckt.
- Sie können aber auch mehrere Bereiche übermitteln, die die unterschiedlichen Bereiche der IP-Adressbereiche des Azure-VM-Subnetzes und des IP-Adressbereichs des Gateways für virtuelle Netzwerke abdecken.
- Der definierte **VNET-Adressraum** wird für die BGP-Routingweitergabe verwendet.
- Das Gatewaysubnetz muss den Namen **GatewaySubnet** haben.
- Der Adressraum wird auf der HANA (große Instanzen)-Seite als Filter verwendet, um Datenverkehr zu den HANA (große Instanz)-Einheiten von Azure zuzulassen oder zu unterbinden. Stellen Sie sicher, dass die Informationen der BGP-Routingweitergabe des virtuellen Azure-Netzwerks und der IP-Adressbereiche, die für die Filterung auf der HANA (große Instanzen)-Seite konfiguriert sind, übereinstimmen. Andernfalls können Verbindungsprobleme auftreten.
- Der Abschnitt **Verbinden eines virtuellen Netzwerks mit großen HANA-Instanzen** enthält einige Details zum Gatewaysubnetz.



## <a name="different-ip-address-ranges-to-be-defined"></a>Andere zu definierende IP-Adressbereiche 

Einige der IP-Adressbereiche, die für die Bereitstellung von HANA (große Instanzen) erforderlich sind, wurden bereits beschrieben. Es gibt aber noch weitere IP-Adressbereiche, die ebenfalls wichtig sind. Wir sehen uns nun einige weitere Details hierzu an. Die folgenden IP-Adressen müssen nicht alle an Microsoft übermittelt werden. Sie müssen sie aber definieren, bevor Sie eine Anforderung zur ersten Bereitstellung senden:

- **VNET-Adressraum**: Wie bereits beschrieben, ist der **VNET-Adressraum** der IP-Adressbereich (bzw. mehrere), den Sie Ihrem Adressraumparameter im virtuellen Azure-Netzwerk zuweisen, für das eine Verbindung mit der Umgebung für SAP HANA (große Instanzen) hergestellt wird.

 Dieser Adressraumparameter sollte ein mehrzeiliger Wert sein, der aus den Azure-VM-Subnetzbereichen und dem Azure-Gatewaysubnetzbereich besteht (siehe obige Abbildungen). Dieser Bereich darf weder mit dem Adressbereich Ihrer lokalen Umgebung oder Server-IP-Pools noch mit ER-P2P-Adressbereichen überlappen. 
 
Wie erhalten Sie diese IP-Adressbereiche? 

Das Netzwerkteam Ihres Unternehmens oder Ihr Dienstanbieter sollte einen oder mehrere IP-Adressbereiche bereitstellen, die in Ihrem Netzwerk nicht verwendet werden. Wenn Ihr Azure-VM-Subnetz beispielsweise 10.0.1.0/24 und Ihr Azure-Gatewaysubnetz 10.0.2.0/28 lautet, empfehlen wir Ihnen, für Ihren Adressraum des virtuellen Azure-Netzwerks zwei Zeilen zu verwenden: 10.0.1.0/24 und 10.0.2.0/28. 

Die Adressraumwerte können zwar aggregiert werden, aber die Empfehlung lautet, sie an die Subnetzbereiche anzupassen. Hierdurch vermeiden Sie die zukünftige versehentliche Wiederverwendung von nicht genutzten IP-Adressbereichen in größeren Adressräumen an einer anderen Stelle Ihres Netzwerks. **Der VNET-Adressraum ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird**.

- **IP-Adressbereich des Azure-VM-Subnetzes:** Wie bereits weiter oben erläutert, ist dies der IP-Adressbereich, den Sie dem Azure-VNET-Subnetzparameter für das virtuelle Azure-Netzwerk zugewiesen haben (oder später zuweisen möchten), für das mit der Umgebung von SAP HANA (große Instanzen) eine Verbindung hergestellt wird. Dieser IP-Adressbereich wird dazu verwendet, Ihren Azure-VMs IP-Adressen zuzuweisen. Die IP-Adressen dieses Bereichs können eine Verbindung mit Ihren Servern für SAP HANA (große Instanzen) herstellen. Bei Bedarf können Sie mehrere Azure-VM-Subnetze verwenden. Wir empfehlen Ihnen, für jedes Azure-VM-Subnetz einen /24-CIDR-Block zu nutzen. Dieser Adressbereich muss Teil der Werte sein, die im Adressraum des virtuellen Azure-Netzwerks verwendet werden. 

Wie erhalten Sie diesen IP-Adressbereich? 

Das Netzwerkteam Ihres Unternehmens oder Ihr Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der nicht in Ihrem Netzwerk verwendet wird.

- **IP-Adressbereich des VNET-Gatewaysubnetzes:** Je nachdem, welche Features Sie verwenden möchten, wird die folgende Größe empfohlen:
   - Höchstleistungs-ExpressRoute-Gateway: /26-Adressblock – erforderlich für Typ II-Klasse von SKUs.
   - Koexistenz mit VPN und ExpressRoute über ein Hochleistungs-ExpressRoute-Gateway für virtuelle Netzwerke (oder kleiner): /27-Adressblock.
   - Alle anderen Fälle: /28-Adressblock. Dieser Adressbereich muss Bestandteil der Werte sein, die in den Werten von „VNET-Adressraum“ verwendet werden. Dieser Adressbereich muss Bestandteil der Werte sein, die als Werte für den Adressraum des virtuellen Netzwerks verwendet werden, den Sie an Microsoft übermitteln. Wie erhalten Sie diesen IP-Adressbereich? Das Netzwerkteam Ihres Unternehmens oder Ihr Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. 

- **Adressbereich für ER-P2P-Konnektivität:** Dieser Adressbereich ist der IP-Adressbereich für die ExpressRoute-P2P-Verbindung von SAP HANA (große Instanzen) (ExpressRoute = ER). Dieser Bereich von IP-Adressen muss ein /29-CIDR-IP-Adressbereich sein. Dieser Bereich darf weder mit dem lokalen noch mit anderen Azure-IP-Adressbereichen überlappen. Dieser IP-Adressbereich wird verwendet, um die ER-Konnektivität von Ihrem virtuellen ExpressRoute-Gateway zu den Servern von SAP HANA (große Instanzen) einzurichten. Wie erhalten Sie diesen IP-Adressbereich? Das Netzwerkteam Ihres Unternehmens oder Ihr Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. **Dieser Bereich ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird**.
  
- **Adressbereich des Server-IP-Pools:** Dieser IP-Adressbereich wird verwendet, um jedem SAP HANA (große Instanzen)-Server seine eigene IP-Adresse zuzuweisen. Die empfohlene Subnetzgröße ist ein /24-CIDR-Block. Bei Bedarf kann auch eine geringere Größe mit nur 64 IP-Adressen genutzt werden. Aus diesem Bereich werden die ersten 30 IP-Adressen für die Verwendung durch Microsoft reserviert. Stellen Sie sicher, dass Sie dies berücksichtigen, wenn Sie die Größe des Bereichs auswählen. Dieser Bereich darf weder mit den lokalen noch mit anderen Azure-IP-Adressen überlappen. Wie erhalten Sie diesen IP-Adressbereich? Das Netzwerkteam Ihres Unternehmens oder Ihr Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. Ein eindeutiger /24-CIDR-Block (empfohlen) für die Zuweisung bestimmter IP-Adressen ist für SAP HANA in Azure (große Instanzen) erforderlich. **Dieser Bereich ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird**.
 
Sie müssen zwar die IP-Adressbereiche definieren und planen, die zuvor beschrieben wurden, aber Sie müssen nicht alle an Microsoft übertragen. Sie müssen die folgenden IP-Adressbereiche für Microsoft angeben:

- Adressräume des virtuellen Azure-Netzwerks
- Adressbereich für ER-P2P-Konnektivität
- Adressbereich des Server-IP-Pools

Wenn Sie zusätzliche virtuelle Netzwerke hinzufügen, für die eine Verbindung mit HANA (große Instanzen) hergestellt werden muss, müssen Sie den Adressraum des neuen virtuellen Azure-Netzwerks an Microsoft übermitteln. 

Es folgen ein Beispiel für die verschiedenen Bereiche sowie einige Beispielbereiche dafür, wie Sie diese konfigurieren und schließlich an Microsoft übermitteln müssen. Der Wert für den Azure-VNET-Adressraum wird im ersten Beispiel nicht aggregiert, sondern aus den Bereichen des ersten IP-Adressbereichs des Azure-VM-Subnetzes und des IP-Adressbereichs des VNET-Gatewaysubnetzes definiert. 

Sie können mehrere VM-Subnetze im virtuellen Azure-Netzwerk verwenden, wenn Sie die zusätzlichen IP-Adressbereiche der zusätzlichen VM-Subnetze als Teil des Adressraums des virtuellen Azure-Netzwerks konfigurieren und übermitteln.

![In einer minimalen Bereitstellung von SAP HANA in Azure (große Instanzen) erforderliche IP-Adressbereiche](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Sie können die Daten, die Sie an Microsoft übermitteln, auch aggregieren. In diesem Fall enthält der Adressraum des virtuellen Azure-Netzwerks nur einen Bereich. Bei Verwendung der IP-Adressbereiche aus dem obigen Beispiel kann der aggregierte Adressraum des virtuellen Netzwerks beispielsweise wie in der folgenden Abbildung aussehen:

![Zweite Möglichkeit für in einer minimalen Bereitstellung von SAP HANA in Azure (große Instanzen) erforderliche IP-Adressbereiche](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Wie Sie im Beispiel sehen können, wird nun anstelle von zwei kleineren Bereichen, die den Adressraum des virtuellen Azure-Netzwerks definieren, ein größerer Bereich verwendet, der 4.096 IP-Adressen abdeckt. Eine solch große Definition des Adressraums bewirkt, dass einige ziemlich große Bereiche nicht verwendet werden. Da die Adressraumwerte des virtuellen Netzwerks für die BGP-Routenweitergabe verwendet werden, können Routingprobleme verursacht werden, wenn ungenutzte Bereiche lokal oder an anderer Stelle in Ihrem Netzwerk verwendet werden. 

Deshalb empfiehlt es sich, den Adressraum eng am tatsächlich verwendeten Subnetzadressraum auszurichten. Sie können bei Bedarf später neue Adressraumwerte hinzufügen, ohne dass dies zu Ausfallzeiten im virtuellen Netzwerk führt.
 
> [!IMPORTANT] 
> Die einzelnen IP-Adressbereiche in ER-P2P, im Server-IP-Pool und im Adressraum des virtuellen Azure-Netzwerks dürfen sich **NICHT** gegenseitig oder mit anderen Bereichen überlappen, die in Ihrem Netzwerk verwendet werden. Jeder Bereich muss eindeutig sein. Wie in den beiden obigen Grafiken dargestellt, kann es sich dabei auch um ein Subnetz eines anderen Bereichs handeln. Wenn Überlappungen zwischen Bereichen bestehen, kann es passieren, dass keine Verbindung zwischen dem virtuellen Azure-Netzwerk und der ExpressRoute-Leitung hergestellt werden kann.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Nächste Schritte, nachdem Adressbereiche definiert wurden
Nachdem die IP-Adressbereiche definiert wurden, müssen die folgenden Schritte ausgeführt werden:

1. Übermitteln Sie die IP-Adressbereiche für den Adressraum des virtuellen Azure-Netzwerks, den Adressbereich für ER-P2P-Konnektivität und den Adressbereich des Server IP-Pools zusammen mit anderen Daten, die am Anfang des Dokuments aufgelistet sind. An diesem Punkt können Sie auch damit beginnen, das virtuelle Netzwerk und die VM-Subnetze zu erstellen. 
2. Eine ExpressRoute-Leitung wird von Microsoft zwischen Ihrem Azure-Abonnement und dem Stempel für SAP HANA (große Instanzen) erstellt.
3. Microsoft erstellt ein Mandantennetzwerk auf dem Stempel für SAP HANA (große Instanzen).
4. Microsoft konfiguriert die Netzwerkfunktionen in der Infrastruktur von SAP HANA in Azure (große Instanzen), damit IP-Adressen aus Ihrem Adressraum des virtuellen Azure-Netzwerks akzeptiert werden, über die mit SAP HANA (große Instanzen) kommuniziert werden soll.
5. Je nach der jeweils erworbenen SKU von SAP HANA in Azure (große Instanzen) weist Microsoft eine Compute-Einheit in einem Mandantennetzwerk zu, reserviert und stellt Speicher bereit und installiert das Betriebssystem (SUSE oder RedHat Linux). IP-Adressen für diese Einheiten werden dem Adressbereich des Server IP-Pools entnommen, den Sie an Microsoft übermittelt haben.

Am Ende des Bereitstellungsprozesses stellt Microsoft Ihnen die folgenden Daten zur Verfügung:
- Informationen, die benötigt werden, um Ihre virtuellen Azure-Netzwerke mit der ExpressRoute-Leitung zu verbinden, mit der die Verbindung zwischen virtuellen Azure-Netzwerken und HANA (große Instanzen) hergestellt wird:
     - Autorisierungsschlüssel
     - ExpressRoute-Peer-ID
- Daten für den Zugriff auf HANA (große Instanzen), nachdem Sie die ExpressRoute-Leitung und das virtuelle Azure-Netzwerk eingerichtet haben.

Die Sequenz für das Verbinden von HANA (große Instanzen) finden Sie auch im Dokument [SAP HANA on Azure (Large Instances) Setup](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/) (SAP HANA in Azure (große Instanzen) – Setup). Viele der folgenden Schritte werden in diesem Dokument in einer Beispielbereitstellung dargestellt. 

**Nächste Schritte**

- Informieren Sie sich unter [Verbinden eines virtuellen Netzwerks mit großen HANA-Instanzen](hana-connect-vnet-express-route.md).

---
title: Einrichten der Konnektivität zwischen virtuellen Computern und SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Einrichten der Konnektivität zwischen virtuellen Computern und SAP HANA in Azure (große Instanzen)
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
ms.openlocfilehash: f0e09e6dfce5d0ede525f461193866219b7f9429
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167622"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Verbinden von Azure-VMs mit HANA (große Instanzen)

Wie bereits unter [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) beschrieben, sieht die minimale Bereitstellung von HANA (große Instanzen) mit der SAP-Anwendungsebene in Azure wie folgt aus:

![Mit SAP HANA in Azure (große Instanzen) und lokal verbundenes Azure-VNET](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Bei genauerer Betrachtung der Azure-VNet-Seite wird klar, dass Folgendes erforderlich ist:

- Die Definition eines Azure-VNets, das verwendet wird, um darin die virtuellen Computer der SAP-Anwendungsschicht bereitzustellen.
- Dies bedeutet automatisch, dass im Azure-VNet ein Standardsubnetz definiert ist, das tatsächlich dasjenige ist, in dem die virtuellen Computer bereitgestellt werden.
- Das Azure-VNet, das erstellt wird, muss mindestens ein VM-Subnetz und ein ExpressRoute-Gatewaysubnetz haben. Diese Subnetze sollten den IP-Adressbereichen so zugewiesen werden, wie dies in den folgenden Abschnitten angegeben und beschrieben ist.

Nun soll ein genauerer Blick auf das Erstellen des Azure-VNets für HANA (große Instanzen) geworfen werden.

## <a name="creating-the-azure-vnet-for-hana-large-instances"></a>Erstellen des Azure-VNets für HANA (großen Instanzen)

>[!Note]
>Das Azure-VNet für HANA (großen Instanzen) muss entsprechend dem Azure Resource Manager-Bereitstellungsmodells erstellt werden. Das ältere Azure-Bereitstellungsmodell, üblicherweise als klassisches Bereitstellungsmodell bezeichnet, wird für die Lösung HANA (großen Instanzen) nicht unterstützt.

Das VNet kann mithilfe des Azure-Portals, mit PowerShell, der Azure-Vorlage oder Azure CLI erstellt werden (siehe [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). Im folgenden Beispiel wird ein VNet erläutert, das über das Azure-Portal erstellt wurde.

Bei Betrachtung der Definitionen eines Azure-VNets über das Azure-Portal sollen einige Definitionen untersucht werden. Außerdem soll ermittelt werden, welche Beziehung sie zu den nachstehend aufgelisteten verschiedenen IP-Adressbereichen haben. Mit **Adressraum** ist der Adressraum gemeint, der für das Azure-VNet verwenden werden kann. Dieser Adressraum ist auch der Adressbereich, der im VNet für die BGP-Routenweitergabe verwendet wird. Dieser **Adressraum** ist hier zu sehen:

![Im Azure-Portal angezeigter Adressraum eines Azure-VNets](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Im obigen Fall wurde für das Azure-VNet mit „10.16.0.0/16“ ein relativ großer und umfangreicher verwendbarer IP-Adressbereich bereitgestellt. Dies bedeutet, dass alle IP-Adressbereiche nachfolgender Subnetze in diesem VNet ihre Bereiche in diesem „Adressraum“ haben können. Üblicherweise ist es nicht empfehlenswert, einen solch großen Adressbereich für ein einzelnes VNet in Azure zu haben. Im nächsten Schritt werden nun die Subnetze untersucht, die im Azure-VNet definiert sind:

![Azure-VNet-Subnetze und deren IP-Adressbereiche](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Wie Sie sehen, geht es um ein VNet mit einem ersten VM-Subnetz (hier als „default“ bezeichnet) und ein Subnetz namens „GatewaySubnet“.
Im folgenden Abschnitt wird auf den IP-Adressbereich des Subnetzes, das in der Abbildung den Namen „default“ hat, mit **IP-Adressbereich des Azure-VM-Subnetzes** verwiesen. In den folgenden Abschnitten wird auf den IP-Adressbereich des Gatewaysubnetzes mit **IP-Adressbereich des VNet-Gatewaysubnetzes** verwiesen. 

Für den Fall, der in den beiden obigen Abbildungen veranschaulicht ist, sehen Sie, dass der **VNet-Adressraum** sowohl den **IP-Adressbereich des Azure-VM-Subnetzes** als auch den **IP-Adressbereich des VNet-Gatewaysubnetzes** abdeckt. 

In anderen Fällen, in denen Sie IP-Adressbereiche sparsam oder spezifisch verwenden müssen, können Sie den **VNet-Adressraum** eines VNets auf die jeweiligen Bereiche einschränken, die für die einzelnen Subnetze verwendet werden. Im vorliegenden Fall können Sie den **VNet-Adressraum** eines VNets als mehrere bestimmte Bereiche definieren:

![Azure-VNet-Adressraum mit zwei Bereichen](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In diesem Fall hat der **VNet-Adressraum** zwei definierte Bereiche. Diese beiden Adressräume sind mit den IP-Adressbereichen identisch, die für den **IP-Adressbereich des Azure-VM-Subnetzes** und den **IP-Adressbereich des VNet-Gatewaysubnetzes** definiert sind.

Für diese Mandantensubnetze (VM-Subnetze) können Sie einen beliebigen Benennungsstandard verwenden. Es muss aber **immer nur ein einziges Gatewaysubnetz für jedes VNet vorhanden sein**, für das eine Verbindung mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen) hergestellt wird. Außerdem **muss dieses Gatewaysubnetz immer den Namen „GatewaySubnet“** haben, um dafür zu sorgen, dass das ExpressRoute-Gateway richtig angeordnet ist.

> [!WARNING] 
> Es ist sehr wichtig, dass das Gatewaysubnetz immer „GatewaySubnet“ heißt.

Es können mehrere VM-Subnetze verwendet werden, in denen sogar nicht zusammenhängende Adressbereiche genutzt werden können. Allerdings müssen diese Adressbereiche, wie bereits erwähnt, durch den **VNet-Adressraum** des VNets in aggregierter Form oder in einer Liste der genauen Bereiche der VM-Subnetze und des Gatewaysubnetzes abgedeckt werden.

Es folgt die Zusammenfassung der wichtigen Fakten zu einem Azure-VNet, das mit HANA (große Instanzen) verbunden ist:

- Sie müssen den **VNet-Adressraum** an Microsoft übermitteln, wenn Sie eine Erstbereitstellung von HANA (große Instanzen) durchführen. 
- Der **VNet-Adressraum** kann ein größerer Bereich sein, der den Bereich für die IP-Adressbereiche des Azure-VM-Subnetzes und den IP-Adressbereich des VNet-Gatewaysubnetzes abdeckt.
- Sie können als **VNet-Adressraum** aber auch mehrere Bereiche übermitteln, die die unterschiedlichen IP-Adressbereiche der IP-Adressbereiche des Azure-VM-Subnetzes und den IP-Adressbereich des VNet-Gatewaysubnetzes abdecken.
- Der definierte **VNet-Adressraum** wird zur BGP-Routingweitergabe verwendet.
- Das Gatewaysubnetz muss den Namen **GatewaySubnet** haben.
- Der **VNet-Adressraum** wird auf der HANA (große Instanzen)-Seite als Filter verwendet, um Datenverkehr zu den HANA (große Instanz)-Einheiten von Azure zuzulassen oder zu unterbinden. Wenn die BGP-Routinginformationen des Azure-VNets und die IP-Adressbereiche, die für das Filtern auf der Seite von HANA (große Instanzen) konfiguriert sind, nicht zueinander passen, können Verbindungsprobleme auftreten.
- Es gibt einige Details zu dem Gatewaysubnetz, die weiter unten im Abschnitt „Verbinden eines VNets mit HANA (große Instanzen)-ExpressRoute“ erläutert sind.



## <a name="different-ip-address-ranges-to-be-defined"></a>Andere zu definierende IP-Adressbereiche 

Einige IP-Adressbereiche, die zum Bereitstellen von HANA (große Instanzen) erforderlich sind, wurden bereits in den Abschnitten weiter oben vorgestellt. Es gibt aber noch einige weitere IP-Adressbereiche, die wichtig sind. Dazu folgen nun weitere Details. Die folgenden IP-Adressen, von denen nicht alle an Microsoft zu übermitteln sind, müssen definiert sein, bevor eine Anforderung zur Erstbereitstellung gesendet wird:

- **VNet-Adressraum:** Dies sind, wie bereits oben vorgestellt, der bzw. die IP-Adressbereiche, die Sie dem Adressraum-Parameter in den Azure Virtual Networks (VNets) zugewiesen haben (oder später zuweisen möchten), die mit der Umgebung von SAP HANA (große Instanzen) verbunden werden. Dieser Adressraum-Parameter sollte ein mehrzeiliger Wert sein, der aus den Azure-VM-Subnetzbereichen und dem Azure-Gatewaysubnetzbereich besteht (siehe Abbildungen weiter oben). Dieser Bereich darf weder mit dem Adressbereich Ihres lokalen oder Server-IP-Pools noch mit ER P2P-Adressbereichen überlappen. Wie erhalten Sie diese IP-Adressbereiche? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen oder mehrere IP-Adressbereiche bereitstellen, die in Ihrem Netzwerk nicht verwendet werden. Beispiel: Wenn Ihr Azure-VM-Subnetz (siehe oben) als „10.0.1.0/24“ und Ihr Azure-Gatewaysubnetz (siehe unten) als „10.0.2.0/28“ angegeben ist, sollte Ihr Azure-VNet-Adressraum aus zwei Zeilen bestehen: „10.0.1.0/24“ und „10.0.2.0/28“. Die Adressraumwerte können zwar aggregiert werden, aber es wird empfohlen, sie an die Subnetzbereiche anzupassen. So wird eine versehentliche spätere Wiederverwendung von ungenutzten IP-Adressbereichen in größeren Adressräumen an einer anderen Stelle Ihres Netzwerks vermieden. **Der VNet-Adressraum ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird.**

- **IP-Adressbereich des Azure-VM-Subnetzes:** Wie bereits weiter oben erläutert, ist dies der IP-Adressbereich, den Sie dem Azure-VNet-Subnetzparameter für das Azure-VNet zugewiesen haben (oder später zuweisen möchten), das mit der Umgebung von SAP HANA (große Instanzen) verbunden wird. Dieser IP-Adressbereich wird dazu verwendet, Ihren Azure-VMs IP-Adressen zuzuweisen. Die IP-Adressen dieses Bereichs können eine Verbindung mit Ihren Servern für SAP HANA (große Instanzen) herstellen. Bei Bedarf können mehrere Azure-VM-Subnetze verwendet werden. Microsoft empfiehlt für jedes Azure-VM-Subnetz einen /24-CIDR-Block. Dieser Adressbereich muss Bestandteil der Werte sein, die im Azure-VNet-Adressraum verwendet werden. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird.

- **IP-Adressbereich des VNet-Gatewaysubnetzes:** Je nachdem, welche Features Sie verwenden möchten, wird die folgende Größe empfohlen:
   - Höchstleistungs-ExpressRoute-Gateway: /26-Adressblock – erforderlich für Typ II-Klasse von SKUs
   - Koexistenz mit VPN und ExpressRoute über ein Hochleistungs-ExpressRoute-Gateway (oder kleiner): /27-Adressblock
   - Alle anderen Fälle: /28-Adressblock. Dieser Adressbereich muss Bestandteil der Werte sein, die in den Werten von „Azure-VNET-Adressraum“ verwendet werden. Dieser Adressbereich muss Bestandteil der Werte sein, die in dem Azure-VNet-Adressraum verwendet werden, den Sie an Microsoft übermitteln müssen. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. 

- **Adressbereich für ER-P2P-Konnektivität:** Dieser Adressbereich ist der IP-Adressbereich für die ExpressRoute-P2P-Verbindung von SAP HANA (große Instanzen) (ExpressRoute = ER). Dieser Bereich von IP-Adressen muss ein /29-CIDR-IP-Adressbereich sein. Dieser Bereich darf weder mit dem lokalen noch mit anderen Azure-IP-Adressbereichen überlappen. Dieser IP-Adressbereich wird verwendet, um die ER-Konnektivität von Ihrem Azure-VNet ExpressRoute-Gateway zu den Servern von SAP HANA (große Instanzen) einzurichten. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. **Dieser Bereich ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird.**
  
- **Adressbereich des Server-IP-Pools:** Dieser IP-Adressbereich wird verwendet, um jedem SAP HANA (große Instanzen)-Server seine eigene IP-Adresse zuzuweisen. Als Subnetzgröße wird ein /24-CIDR-Block empfohlen – bei Bedarf kann aber auch ein kleineres Subnetz verwendet werden, mindestens 64 IP-Adressen müssen jedoch bereitgestellt werden. Aus diesem Bereich werden die ersten 30 IP-Adressen für die Verwendung durch Microsoft reserviert. Dieser Punkt ist zu berücksichtigen, wenn Sie die Größe des Bereichs festlegen. Dieser Bereich darf weder mit den lokalen noch mit anderen Azure-IP-Adressen überlappen. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. Ein eindeutiger /24-CIDR-Block (empfohlen) für die Zuweisung bestimmter IP-Adressen, die für SAP HANA in Azure (große Instanzen) erforderlich sind. **Dieser Bereich ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird.**
 
Sie müssen zwar die oben aufgeführten IP-Adressbereiche planen und definieren, es ist aber nicht erforderlich, dass sie alle dieser Bereich an Microsoft übermitteln. Zusammengefasst ergibt sich, dass Sie Microsoft die folgenden IP-Adressbereiche benennen müssen:

- Azure-VNet-Adressraum oder -Adressräume
- Adressbereich für ER-P2P-Konnektivität
- Adressbereich des Server-IP-Pools

Wenn Sie weitere VNets hinzufügen, die mit SAP HANA (große Instanzen) verbunden werden sollen, müssen Sie Microsoft den neuen Azure-VNet-Adressraum übermitteln, den Sie hinzufügen. 

Es folgen ein Beispiel für die verschiedenen Bereiche sowie einige Beispielbereiche dafür, wie Sie diese konfigurieren und schließlich an Microsoft übermitteln müssen. Wie Sie sehen können, wird der Wert für den Azure-VNet-Adressraum im ersten Beispiel nicht aggregiert, sondern aus den Bereichen des ersten IP-Adressbereichs des Azure-VM-Subnetzes und des IP-Adressbereichs des VNet-Gatewaysubnetzes definiert. Ein Verwenden mehrerer VM-Subnetze im Azure-VNet würde entsprechend funktionieren, indem die weiteren IP-Adressbereiche der zusätzlichen VM-Subnetze als Bestandteil des Azure-VNet-Adressraums konfiguriert und übermittelt werden.

![In einer minimalen Bereitstellung von SAP HANA in Azure (große Instanzen) erforderliche IP-Adressbereiche](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Sie können die Daten, die Sie an Microsoft übermitteln, auch aggregieren. In diesem Fall umfasst der Adressraum des Azure-VNets nur einen Bereich. Es werden die IP-Adressbereiche aus dem obigen Beispiel verwendet. Dieser aggregierte VNet-Adressraum kann wie folgt aussehen:

![Zweite Möglichkeit für in einer minimalen Bereitstellung von SAP HANA in Azure (große Instanzen) erforderliche IP-Adressbereiche](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Wie Sie oben sehen können, wird nun anstelle von zwei kleineren Bereichen, die den Adressraum des Azure-VNets definieren, ein größerer Bereich verwendet, der 4096 IP-Adressen abdeckt. Eine solche große Definition des Adressraums bewirkt, dass einige ziemlich große Bereiche nicht verwendet werden. Da die VNet-Adressraumwerte für die BGP-Routenweitergabe verwendet werden, können Routingprobleme verursacht werden, wenn ungenutzte Bereiche lokal oder an anderer Stelle in Ihrem Netzwerk verwendet werden. Deshalb empfiehlt es sich, den Adressraum eng an dem tatsächlich verwendeten Subnetzadressraum auszurichten. Sie können bei Bedarf später neue Adressraumwerte hinzufügen, ohne dass dies zu Ausfallzeiten im VNet führt.
 
> [!IMPORTANT] 
> Die IP-Adressbereiche für ER-P2P, Server-IP-Pool und Azure-VNet-Adressraum dürfen sich **NICHT** überlappen, und zwar weder untereinander noch mit irgendeinem anderen Bereich, der an anderer Stelle in Ihrem Netzwerk verwendet wird. Für jeden IP-Adressbereich gilt, dass er diskret sein muss und – wie die beiden obigen Abbildungen veranschaulichen – kein Subnetz eines anderen Bereichs sein darf. Gibt es Überlappungen zwischen Bereichen, kann es passieren, dass keine Verbindung zwischen dem Azure-VNet und der ExpressRoute-Leitung hergestellt werden kann.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Nächste Schritte, nachdem Adressbereiche definiert wurden
Nachdem die IP-Adressbereiche definiert wurden, müssen die folgenden Schritte ausgeführt werden:

1. Übermitteln Sie die IP-Adressbereiche für Azure VNet-Adressraum, den Adressbereich für ER-P2P-Konnektivität und den Adressbereich des Server IP-Pools zusammen mit anderen Daten, die am Anfang des Dokuments aufgelistet sind. Zu diesem Zeitpunkt können Sie auch beginnen, das virtuelle Netzwerk (VNet) und die VM-Subnetze zu erstellen. 
2. Eine ExpressRoute-Leitung wird von Microsoft zwischen Ihrem Azure-Abonnement und dem Stempel für SAP HANA (große Instanzen) erstellt.
3. Microsoft erstellt ein Mandantennetzwerk auf dem Stempel für SAP HANA (große Instanzen).
4. Microsoft konfiguriert die Netzwerkfunktionen in der Infrastruktur von SAP HANA in Azure (große Instanzen), damit IP-Adressen aus Ihrem Azure-VNet-Adressraum akzeptiert werden, über die mit SAP HANA (große Instanzen) kommuniziert werden soll.
5. Je nach der speziellen erworbenen SKU von SAP HANA in Azure (große Instanzen) weist Microsoft eine Computeeinheit in einem Mandantennetzwerk zu, reserviert Speicher, bindet diesen ein und installiert das Betriebssystem (SUSE oder RedHat Linux). IP-Adressen für diese Einheiten werden dem Adressbereich des Server IP-Pools entnommen, den Sie an Microsoft übermittelt haben.

Am Ende des Bereitstellungsprozesses stellt Microsoft Ihnen die folgenden Daten zur Verfügung:
- Informationen, die benötigt werden, um Ihre Azure-VNets mit der ExpressRoute-Leitung zu verbinden, über die Azure-VNets mit SAP HANA (große Instanzen) verbunden werden:
     - Autorisierungsschlüssel
     - ExpressRoute-Peer-ID
- Daten, um auf SAP HANA (große Instanzen) zuzugreifen, nachdem Sie die ExpressRoute-Leitung und das Azure-VNet eingerichtet haben

Die Sequenz für das Verbinden von HANA (große Instanzen) finden Sie auch im Dokument [End to End Setup for SAP HANA Large Instances](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/) (End-to-End-Setup für SAP HANA (große Instanzen)). Viele der folgenden Schritte werden in diesem Dokument in einer Beispielbereitstellung dargestellt. 

**Nächste Schritte**

- Lesen Sie [Herstellen einer ExpressRoute-Verbindung zwischen einem VNET und HANA (große Instanzen)](hana-connect-vnet-express-route.md).
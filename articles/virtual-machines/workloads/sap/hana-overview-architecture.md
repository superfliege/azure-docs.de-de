---
title: Übersicht und Architektur von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Architekturübersicht zur Bereitstellung von SAP HANA in Azure (große Instanzen).
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
ms.date: 06/04/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfcab5a84d9e8b0bf164c666162636ede2e1b06f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763781"
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Übersicht und Architektur von SAP HANA in Azure (große Instanzen)

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Was ist SAP HANA in Azure (große Instanzen)?

SAP HANA in Azure (große Instanzen) ist eine einzigartige Lösung in Azure. Neben VMs zum Bereitstellen und Ausführen von SAP HANA bietet Azure Ihnen die Möglichkeit, SAP HANA auf Ihnen zugeordneten Bare-Metal-Servern auszuführen und bereitzustellen. Die Lösung „SAP HANA in Azure (große Instanzen)“ beruht auf nicht freigegebener Host- bzw. Bare-Metal-Hardware, die Ihnen zugewiesen ist. Die Serverhardware ist in größere Stempel eingebettet, die Infrastruktur für Compute/Server, Netzwerke und Speicher enthalten. Als Kombination ist die Lösung für HANA Tailored Data Center Integration (TDI) zertifiziert. Für SAP HANA in Azure (große Instanzen) sind verschiedene Server-SKUs oder -größen verfügbar. Die Einheiten können von 72 CPUs und 768 GB Arbeitsspeicher bis zu 960 CPUs und 20 TB Arbeitsspeicher umfassen.

Die Kundenisolation innerhalb des Infrastrukturumfelds wird in Mandanten wie folgt durchgeführt:

- **Netzwerke**: Isolation von Kunden innerhalb des Infrastrukturstapels durch virtuelle Netzwerke pro dem Kunden zugewiesenem Mandanten. Ein Mandant wird einem einzelnen Kunden zugewiesen. Ein Kunde kann mehrere Mandanten haben. Die Netzwerkisolation von Mandanten verhindert die Netzwerkkommunikation zwischen Mandanten auf der Ebene des Infrastrukturumfelds. Dies ist auch dann der Fall, wenn die Mandanten zum selben Kunden gehören.
- **Speicherkomponenten**: Isolation durch Speicher-VMs, denen Speichervolumes zugewiesen sind. Speichervolumen können nur einem virtuellen Speichercomputer zugewiesen werden. Ein virtueller Speichercomputer wird nur einem einzigen Mandanten im SAP HANA-TDI-zertifizierten Infrastrukturstapel zugewiesen. Folglich kann nur in einem bestimmten verknüpften Mandanten auf Speichervolumes zugegriffen werden, die einer Speicher-VM zugewiesen sind. Sie sind zwischen den verschiedenen bereitgestellten Mandanten nicht sichtbar.
- **Server oder Host**: Eine Server- oder Hosteinheit wird nicht zwischen Kunden oder Mandanten freigegeben. Ein Server oder Host, der einem Kunden bereitgestellt wird, ist eine unteilbare Bare-Metal-Computeeinheit, die einem einzigen Mandanten zugewiesen wird. Es wird *keine* Hard- oder Softpartitionierung verwendet, die dazu führen könnte, dass Sie einen Host oder Server für einen anderen Kunden freigeben. Speichervolumen, die dem virtuellen Speichercomputer des spezifischen Kunden zugewiesen sind, werden in einen derartigen Server eingebunden. Einem Mandant kann eine oder mehrere Servereinheiten verschiedener SKUs exklusiv zugewiesen werden.
- In einem Infrastrukturumfeld von SAP HANA in Azure (große Instanzen) werden viele verschiedene Mandanten anhand der Mandantenkonzepte auf Netzwerk-, Speicher- und Computeebene bereitgestellt und voneinander isoliert. 


Diese Bare-Metal-Servereinheiten unterstützen nur das Ausführen von SAP HANA. Die SAP-Anwendungsschicht oder Middlewareschicht der Workload wird auf VMs ausgeführt. Die Infrastrukturumfelder, die die Einheiten von SAP HANA in Azure (große Instanzen) ausführen, sind mit den Backbones der Azure-Netzwerkdienste verbunden. Auf diese Weise wird eine Konnektivität mit geringer Wartezeit zwischen Einheiten von SAP HANA in Azure (große Instanzen) und VMs bereitgestellt.

Dieses Dokument ist eines von mehreren Dokumenten, die sich mit SAP HANA in Azure (große Instanzen) befassen. Es bietet eine Einführung in die grundlegende Architektur, die Zuständigkeiten und die von der Lösung bereitgestellten Dienste. Allgemeine Funktionen der Lösung werden ebenfalls erläutert. Die meisten anderen Bereiche (beispielsweise Netzwerk und Konnektivität) werden in vier weiteren Dokumenten ausführlich behandelt. Die Installation oder Bereitstellung von SAP NetWeaver auf VMs wird in der Dokumentation zu SAP HANA in Azure (große Instanzen) nicht behandelt. SAP NetWeaver in Azure wird in separaten Dokumenten abgedeckt, die sich im selben Azure-Dokumentationscontainer befinden. 


Die anderen Dokumente mit Anleitungen zu großen HANA-Instanzen behandeln die folgenden Bereiche:

- [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Problembehandlung und Überwachung von SAP HANA in Azure (große Instanzen)](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hochverfügbarkeitskonfiguration unter SUSE mit STONITH](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Sichern und Wiederherstellen des Betriebssystems für Typ-II-SKUs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definitionen

Im Handbuch zur Architektur und technischen Bereitstellung werden mehrere allgemeine Definitionen häufig verwendet. Beachten Sie die folgenden Begriffe und ihre Bedeutung:

- **IaaS:** Infrastructure-as-a-Service.
- **PaaS:** Platform-as-a-Service.
- **SaaS:** Software-as-a-Service.
- **SAP-Komponente**: Eine einzelne SAP-Anwendung, beispielsweise ERP Central Component (ECC), Business Warehouse (BW), Solution Manager oder Enterprise Portal (EP). SAP-Komponenten können auf herkömmlichen ABAP- oder Java-Technologien oder auf einer Nicht-NetWeaver-basierten Anwendung wie Business Objects basieren.
- **SAP-Umgebung:** Eine oder mehrere SAP-Komponenten, die logisch gruppiert sind, um eine Geschäftsfunktion wie Entwicklung, Qualitätssicherung, Schulung, Notfallwiederherstellung oder Produktion auszuführen.
- **SAP-Landschaft:** Bezieht sich auf alle SAP-Ressourcen in Ihrer IT-Landschaft. Die SAP-Landschaft umfasst alle Produktions- und anderen Umgebungen.
- **SAP-System**: Die Kombination von DBMS-Schicht und Anwendungsschicht, die beispielsweise aus einem SAP ERP-Entwicklungssystem, einem SAP BW-Testsystem und einem SAP CRM-Produktionssystem bestehen kann. Azure-Bereitstellungen unterstützen die Aufteilung dieser beiden Schichten zwischen lokalen Systemen und Azure nicht. Ein SAP-System wird entweder lokal oder in Azure bereitgestellt. Sie können die verschiedenen Systeme einer SAP-Landschaft in Azure oder lokal bereitstellen. Beispielsweise können Sie die SAP CRM-Entwicklungs- und Testsysteme in Azure und das SAP CRM-Produktionssystem lokal bereitstellen. Im Fall von SAP HANA in Azure (große Instanzen) sollen Sie die SAP-Anwendungsschicht von SAP-Systemen auf VMs und die zugehörige SAP HANA-Instanz auf einer Einheit im Umfeld von SAP HANA in Azure (große Instanzen) hosten.
- **Umfeld der großen Instanz:** Ein Hardwareinfrastrukturstapel, der SAP HANA TDI-zertifiziert und für die Ausführung von SAP HANA-Instanzen in Azure zugeordnet ist.
- **SAP HANA in Azure (große Instanzen):** Offizieller Name für das Angebot in Azure, HANA-Instanzen auf SAP HANA TDI-zertifizierter Hardware auszuführen, die in Umfeldern der großen Instanz in verschiedenen Azure-Regionen bereitgestellt wird. Der Begriff *HANA (große Instanz)* ist die Kurzform für *SAP HANA in Azure (große Instanzen)* und wird in diesem Bereitstellungshandbuch gemeinhin verwendet.
- **Standortübergreifend:** Beschreibt ein Szenario, in dem VMs in einem Azure-Abonnement bereitgestellt werden, das Site-to-Site-, Multisite- oder ExpressRoute-Konnektivität zwischen lokalen Rechenzentren und Azure umfasst. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als standortübergreifende Szenarios bezeichnet. Die Verbindung dient dazu, lokale Domänen, lokale Azure Active Directory-/OpenLDAP-Instanzen und den lokalen DNS-Dienst auf Azure zu erweitern. Die lokale Landschaft wird auf die Azure-Ressourcen der Azure-Abonnements erweitert. Durch diese Erweiterung können die VMs Teil der lokalen Domäne sein. 

   Domänenbenutzer der lokalen Domäne können auf die Server zugreifen und Dienste auf den VMs ausführen (beispielsweise DBMS-Dienste). Die Kommunikation und Namensauflösung zwischen lokal bereitgestellten VMs und in Azure bereitgestellten VMs ist möglich. Dieses typische Szenario wird für die Bereitstellung der meisten SAP-Ressourcen genutzt. Weitere Informationen finden Sie unter [Planung und Entwurf für VPN Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und [Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-Verbindung im Azure-Portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Mandant**: Ein im Umfeld von HANA (große Instanz) bereitgestellter Kunde wird in einem *Mandanten* isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen sind, können einander auf der Ebene des Umfelds von HANA (große Instanz) nicht sehen und nicht miteinander kommunizieren. Ein Kunde kann Bereitstellungen in unterschiedlichen Mandanten durchführen. Auch dann ist die Kommunikation auf Ebene der großen HANA-Instanz nicht möglich.
- **SKU-Kategorie:** Für HANA (große Instanz) werden die folgenden beiden SKU-Kategorien angeboten:
    - **Typ I-Klasse**: S72, S72m, S144, S144m, S192, S192m und S192xm
    - **Type II-Klasse**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm und S960m


Es sind viele weitere Ressourcen zum Bereitstellen einer SAP-Workload in der Cloud verfügbar. Die Planung einer Bereitstellung von SAP HANA in Azure erfordert, dass Sie mit den Prinzipien von Azure-IaaS und der Bereitstellung von SAP-Workloads in Azure-IaaS vertraut sind. Bevor Sie fortfahren, sollten Sie den Artikel zur [Verwendung von SAP-Lösungen auf Azure-VMs](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lesen. 

## <a name="certification"></a>Zertifizierung

Neben der NetWeaver-Zertifizierung erfordert SAP eine spezielle Zertifizierung für SAP HANA, damit SAP HANA auf bestimmten Infrastrukturen wie Azure IaaS unterstützt wird.

Der wichtigste SAP-Hinweis zu NetWeaver und in gewissem Maße zur SAP HANA-Zertifizierung ist [SAP-Hinweis 1928533 – SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen](https://launchpad.support.sap.com/#/notes/1928533).

Der [SAP-Hinweis 2316233 – SAP HANA in Microsoft Azure (große Instanzen) ist ebenfalls relevant](https://launchpad.support.sap.com/#/notes/2316233/E). Darin wird die in diesem Handbuch beschriebene Lösung behandelt. Darüber hinaus werden Sie bei der Ausführung von SAP HANA auf dem Azure-VM-Typ GS5 unterstützt. Informationen für diesen Fall sind auf der [SAP-Website](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) verfügbar.

Die Lösung „SAP HANA in Azure (große Instanzen)“, auf die im SAP-Hinweis 2316233 Bezug genommen wird, bietet Microsoft- und SAP-Kunden die Möglichkeit, große SAP Business Suite-, SAP BW-, S/4 HANA-, BW/4HANA- oder andere SAP HANA-Workloads in Azure bereitzustellen. Die Lösung beruht auf dem dedizierten SAP HANA-zertifizierten Hardwareumfeld ([SAP HANA Tailored Datacenter Integration – TDI](https://scn.sap.com/docs/DOC-63140)). Wenn Sie eine für SAP HANA TDI konfigurierte Lösung ausführen, werden alle SAP HANA-basierten Anwendungen (beispielsweise SAP Business Suite unter SAP HANA, SAP BW unter SAP HANA, S4/HANA und BW4/HANA) auf der Hardwareinfrastruktur ausgeführt.

Diese Lösung bietet im Vergleich zur Ausführung von SAP HANA auf VMs einen Vorteil: Es sind deutlich größere Speichervolumes verfügbar. Um diese Lösung nutzen zu können, müssen Sie die folgenden wichtigen Aspekte verstehen:

- Die SAP-Anwendungsschicht und Nicht-SAP-Anwendungen werden auf VMs ausgeführt, die in den üblichen Azure-Hardwareumfeldern gehostet werden.
- Die lokale(n) Infrastruktur, Rechenzentren und Anwendungsbereitstellungen des Kunden werden über ExpressRoute (empfohlen) oder ein virtuelles privates Netzwerk (VPN) mit der Cloudplattform verbunden. Auch Active Directory und DNS werden auf Azure erweitert.
- Die SAP HANA-Datenbankinstanz für HANA-Workload wird auf SAP HANA in Azure (große Instanzen) ausgeführt. Das Umfeld von HANA (große Instanz) ist mit dem Azure-Netzwerk verbunden, sodass auf VMs ausgeführte Software mit der HANA-Instanz interagieren kann, die in HANA (große Instanz) ausgeführt wird.
- Hardware von SAP HANA in Azure (große Instanzen) ist dedizierte Hardware, die in einer IaaS mit vorinstalliertem SUSE Linux Enterprise Server oder Red Hat Enterprise Linux bereitgestellt wird. Wie bei VMs sind Sie für weitere Updates und die Wartung des Betriebssystems zuständig.
- Die Installation von HANA oder zusätzlichen Komponenten, die zum Ausführen von SAP HANA auf Einheiten von HANA (große Instanz) erforderlich sind, liegt in Ihrer Verantwortung. Sie sind auch für den gesamten laufenden Betrieb und die laufende Verwaltung von SAP HANA in Azure zuständig.
- Zusätzlich zu den hier beschriebenen Lösungen können Sie andere Komponenten in Ihrem Azure-Abonnement installieren, das eine Verbindung mit SAP HANA in Azure (große Instanzen) herstellt. Hierzu zählen beispielsweise Komponenten, die die Kommunikation oder direkte Verbindung mit der SAP HANA-Datenbank ermöglichen (beispielsweise Jumpserver, RDP-Server, SAP HANA Studio, SAP Data Services für SAP BI-Szenarien oder Netzwerküberwachungslösungen).
- Wie in Azure bietet HANA (große Instanz) Unterstützung für Funktionen für Hochverfügbarkeit und Notfallwiederherstellung.

## <a name="architecture"></a>Architecture

Allgemein betrachtet, befindet sich die SAP-Anwendungsschicht bei SAP HANA in Azure (große Instanzen) auf VMs. Die Datenbankschicht befindet sich auf SAP TDI-konfigurierter Hardware in einem Umfeld der großen Instanz in derselben mit Azure-IaaS verbundenen Azure-Region.

> [!NOTE]
> Stellen Sie die SAP-Anwendungsschicht in derselben Azure-Region bereit wie die SAP-DBMS-Schicht. Diese Regel ist in veröffentlichten Informationen zu SAP-Workloads in Azure umfassend dokumentiert. 

Die Gesamtarchitektur von SAP HANA in Azure (große Instanzen) bietet eine SAP TDI-zertifizierte Hardwarekonfiguration, bei der es sich um einen nicht virtualisierten Bare-Metal-Hochleistungsserver für die SAP HANA-Datenbank handelt. Darüber hinaus bietet sie die Möglichkeit und Flexibilität von Azure, Ressourcen für die SAP-Anwendungsschicht nach Bedarf zu skalieren.

![Architekturübersicht über SAP HANA in Azure (große Instanzen)](./media/hana-overview-architecture/image1-architecture.png)

Die dargestellte Architektur ist in drei Abschnitte unterteilt:

- **Rechts**: Zeigt eine lokale Infrastruktur, die verschiedene Anwendungen in Rechenzentren ausführt, sodass Endbenutzer auf branchenspezifische Anwendungen wie SAP zugreifen können. Im Idealfall wird diese lokale Infrastruktur dann über [ExpressRoute](https://azure.microsoft.com/services/expressroute/) mit Azure verbunden.

- **Mitte:** Zeigt Azure-IaaS und (in diesem Fall) die Verwendung von VMs zum Hosten von SAP- oder anderen Anwendungen, die SAP HANA als DBMS-System nutzen. Kleinere HANA-Instanzen, die mit dem von VMs bereitgestellten Arbeitsspeicher arbeiten, werden zusammen mit ihrer Anwendungsschicht auf VMs bereitgestellt. Weitere Informationen zu VMs finden Sie unter [Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/).

   Azure-Netzwerkdienste werden verwendet, um SAP-Systeme mit anderen Anwendungen in virtuellen Netzwerken zu gruppieren. Diese virtuellen Netzwerke stellen sowohl eine Verbindung mit lokalen Systemen als auch mit SAP HANA in Azure (große Instanzen) her.

   Informationen zu SAP NetWeaver-Anwendungen und Datenbanken, deren Ausführung in Azure unterstützt wird, finden Sie im [SAP-Supporthinweis 1928533 – SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen](https://launchpad.support.sap.com/#/notes/1928533). Eine Dokumentation zur Bereitstellung von SAP-Lösungen in Azure finden Sie in den Artikeln zu folgenden Themen:

  -  [Verwenden von SAP auf Windows-VMs](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Verwenden von SAP-Lösungen auf Azure-VMs](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Links:** Zeigt die SAP HANA TDI-zertifizierte Hardware im Azure-Umfeld der großen Instanz. Die Einheiten von HANA (große Instanz) sind mit den virtuellen Netzwerken Ihres Abonnements verbunden. Für diese Verbindung wird dieselbe Technologie verwendet wie für die Konnektivität zwischen lokalen Systemen und Azure.

Im Azure-Stapel für große Instanzen selbst werden die folgenden Komponenten kombiniert:

- **Computing:** Server, die auf Intel Xeon E7-8890v3- oder Intel Xeon E7-8890v4-Prozessoren basieren, die die erforderliche Computingleistung bereitstellen und SAP HANA-zertifiziert sind.
- **Netzwerk:** Ein einheitliches Hochgeschwindigkeitsnetzwerk-Fabric, das die Computing-, Speicher- und LAN-Komponenten miteinander verbindet.
- **Speicher:** Eine Speicherinfrastruktur, auf die über ein einheitliches Netzwerkfabric zugegriffen wird. Die verfügbare spezifische Speicherkapazität hängt von der jeweiligen Konfiguration von SAP HANA in Azure (große Instanzen) ab, die bereitgestellt wird. Mehr Speicherkapazität ist gegen eine zusätzliche monatliche Gebühr erhältlich.

Innerhalb der mehrinstanzenfähigen Infrastruktur des Stapels für große Instanzen werden Kunden als isolierte Mandanten bereitgestellt. Bei der Bereitstellung des Mandanten benennen Sie ein Azure-Abonnement in Ihrer Azure-Registrierung. Diesem Azure-Abonnement werden die Kosten von HANA (große Instanz) in Rechnung gestellt. Diese Mandanten weisen eine 1:1-Beziehung mit dem Azure-Abonnement auf. In einem Netzwerk ist der Zugriff auf eine Einheit von HANA (große Instanz), die in einem Mandanten in einer Azure-Region bereitgestellt ist, über verschiedene, zu unterschiedlichen Azure-Abonnements gehörende virtuelle Netzwerke möglich. Diese Azure-Abonnements müssen zur selben Azure-Registrierung gehören. 

Wie bei VMs wird SAP HANA in Azure (große Instanzen) in mehreren Azure-Regionen angeboten. Funktionen zur Notfallwiederherstellung können wahlweise abonniert werden. Die verschiedenen Stapel für große Instanzen sind innerhalb einer geopolitischen Azure-Region miteinander verbunden. Umfelder von HANA (große Instanz) in „USA, Westen“ und „USA, Osten“ sind beispielsweise zur Replikation zwecks Notfallwiederherstellung über eine dedizierte Netzwerkverbindung verbunden. 

Ebenso, wie Sie bei Azure Virtual Machines zwischen verschiedenen VM-Typen wählen können, stehen Ihnen verschiedene SKUs von HANA (große Instanz) zur Auswahl, die auf verschiedene Workloadtypen von SAP HANA zugeschnitten sind. Für variierende Workloads wendet SAP basierend auf den Intel-Prozessorgenerationen ein geeignetes Verhältnis zwischen Arbeitsspeicher und Prozessorsockets an. Die folgende Tabelle zeigt die angebotenen SKU-Typen.

Ab Juli 2017 ist SAP HANA in Azure (große Instanzen) in verschiedenen Konfigurationen in den folgenden Azure-Regionen verfügbar: „USA, Westen“, „USA, Osten“, „Australien, Osten“, „Australien, Südosten“, „Europa, Westen“ und „Europa, Norden“.

| SAP-Lösung | CPU | Arbeitsspeicher | Speicher | Verfügbarkeit |
| --- | --- | --- | --- | --- |
| Optimiert für OLAP: SAP BW, BW/4HANA<br /> oder SAP HANA für generische OLAP-Workload | SAP HANA in Azure S72<br /> – 2 x Intel® Xeon®-Prozessor E7-8890 v3<br /> 36 CPU-Kerne und 72 CPU-Threads |  768 GB |  3 TB | Verfügbar |
| --- | SAP HANA in Azure S144<br /> – 4 x Intel® Xeon®-Prozessor E7-8890 v3<br /> 72 CPU-Kerne und 144 CPU-Threads |  1,5 TB |  6 TB | Wird nicht mehr angeboten |
| --- | SAP HANA in Azure S192<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-Kerne und 192 CPU-Threads |  2,0 TB |  8 TB | Verfügbar |
| --- | SAP HANA in Azure S384<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-Kerne und 384 CPU-Threads |  4,0 TB |  16 TB | Verfügbar |
| Optimiert für OLTP: SAP Business Suite<br /> auf SAP HANA oder S/4HANA (OLTP),<br /> generisches OLTP | SAP HANA in Azure S72m<br /> – 2 x Intel® Xeon®-Prozessor E7-8890 v3<br /> 36 CPU-Kerne und 72 CPU-Threads |  1,5 TB |  6 TB | Verfügbar |
|---| SAP HANA in Azure S144m<br /> – 4 x Intel® Xeon®-Prozessor E7-8890 v3<br /> 72 CPU-Kerne und 144 CPU-Threads |  3,0 TB |  12TB | Wird nicht mehr angeboten |
|---| SAP HANA in Azure S192m<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-Kerne und 192 CPU-Threads  |  4,0 TB |  16 TB | Verfügbar |
|---| SAP HANA in Azure S384m<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-Kerne und 384 CPU-Threads |  6,0 TB |  18 TB | Verfügbar |
|---| SAP HANA in Azure S384xm<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-Kerne und 384 CPU-Threads |  8,0 TB |  22 TB |  Verfügbar |
|---| SAP HANA in Azure S576m<br /> – 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU-Kerne und 576 CPU-Threads |  12,0 TB |  28 TB | Verfügbar |
|---| SAP HANA in Azure S768m<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-Kerne und 768 CPU-Threads |  16, 0 TB |  36 TB | Verfügbar |
|---| SAP HANA in Azure S960m<br /> – 20 x Intel® Xeon® Processor E7-8890 v4<br /> 480 CPU-Kerne und 960 CPU-Threads |  20,0 TB |  46 TB | Verfügbar |
| Optimiert für OLTP **TDIv5**: SAP Business Suite<br /> auf SAP HANA oder S/4HANA (OLTP),<br /> generisches OLTP | SAP HANA in Azure S192xm<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-Kerne und 192 CPU-Threads |  6,0 TB |  16 TB | Verfügbar |
|---| SAP HANA in Azure S384xxm<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-Kerne und 384 CPU-Threads |  12,0 TB |  28 TB | Verfügbar |
|---| SAP HANA in Azure S576xm<br /> – 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU-Kerne und 576 CPU-Threads |  18 TB |  41 TB | Verfügbar |
|---| SAP HANA in Azure S768xm<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-Kerne und 768 CPU-Threads |  24 TB |  56 TB | Verfügbar |
- CPU-Kerne = Summe der CPU-Kerne ohne Hyperthreading aus der Gesamtsumme der Prozessoren der Servereinheit.
- CPU-Threads = Summe der Computethreads von den Hyperthreading-CPU-Kernen aus der Gesamtsumme der Prozessoren der Servereinheit. Alle Einheiten sind standardmäßig zur Verwendung der Hyperthreading-Technologie konfiguriert.


Die jeweils gewählten Konfigurationen hängen von der Workload, den CPU-Ressourcen und dem gewünschten Arbeitsspeicher ab. Die OLTP-Workload kann die SKUs nutzen, die für die OLAP-Workload optimiert sind. 

Die Hardwarebasis für alle Angebote ist SAP HANA TDI-zertifiziert. Die SKUs werden durch zwei unterschiedliche Hardwareklassen wie folgt unterteilt:

- S72, S72m, S144, S144m, S192, S192m und S192xm, die als „Typ-I-Klasse“ von SKUs bezeichnet werden.
- S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm und S960m, die wir als „Typ II-Klasse“ von SKUs bezeichnen.

Ein vollständiges Umfeld von HANA (große Instanz) ist nicht ausschließlich zur Verwendung durch einen einzelnen Kunden zugeordnet. Dies gilt für Racks mit Compute- und Speicherressourcen, die über ein in Azure bereitgestelltes Netzwerkfabric verbunden sind. Die Infrastruktur von HANA (große Instanz) stellt wie Azure verschiedene &quot;Kundenmandanten&quot; bereit, die in den drei folgenden Ebenen voneinander isoliert sind:

- **Netzwerk**: Isolation durch virtuelle Netzwerke innerhalb des Umfelds von HANA (große Instanz).
- **Speicher**: Isolation durch Speicher-VMs, denen Speichervolumes zugewiesen sind und die Speichervolumes zwischen Mandanten isolieren.
- **Compute**: Dedizierte Zuweisung von Servereinheiten zu einem einzelnen Mandanten. Keine Hard- oder Softpartitionierung von Servereinheiten. Keine Freigabe einer einzelnen Server- oder Hosteinheit zwischen Mandanten. 

Die Bereitstellungen von Einheiten von HANA (große Instanz) zwischen verschieden Mandanten sind füreinander nicht sichtbar. In verschiedenen Mandanten bereitgestellte Einheiten von HANA (große Instanz) können nicht direkt auf der Ebene des Umfelds von HANA (große Instanz) miteinander kommunizieren. Nur Einheiten von HANA (große Instanz) innerhalb eines Mandanten können auf der Ebene des Umfelds von HANA (große Instanz) miteinander kommunizieren.

Ein bereitgestellter Mandant im Umfeld der großen Instanz wird zur Abrechnung einem Azure-Abonnement zugewiesen. In einem Netzwerk kann über virtuelle Netzwerke anderer Azure-Abonnements in derselben Azure-Registrierung darauf zugegriffen werden. Bei Bedarf können Sie auch einen getrennten HANA-Mandanten (große Instanz) beantragen, wenn Sie mit einem anderen Azure-Abonnement in derselben Azure-Region Bereitstellungen durchführen.

Es gibt jedoch bedeutende Unterschiede zwischen der Ausführung von SAP HANA unter HANA (große Instanz) und der Ausführung von SAP HANA auf in Azure bereitgestellten VMs:

- Es gibt keine Virtualisierungsschicht für SAP HANA in Azure (große Instanzen). Sie profitieren von der Leistung der zugrunde liegenden Bare-Metal-Hardware.
- Im Gegensatz zu Azure ist der Server für SAP HANA in Azure (große Instanzen) für einen bestimmten Kunden vorgesehen. Die Hard- oder Softpartitionierung einer Servereinheit oder eines Hosts ist nicht möglich. Deshalb wird eine Einheit von HANA (große Instanz) als Ganzes einem Mandanten und somit Ihnen zugewiesen und verwendet. Ein Neustart oder das Herunterfahren des Servers führt nicht automatisch dazu, dass das Betriebssystem und SAP HANA auf einem anderen Server bereitgestellt werden. (Die einzige Ausnahme für Typ-I-Klasse-SKUs ist das Auftreten von Problemen auf einem Server, die eine erneute Bereitstellung auf einem anderen Server erfordern.)
- Im Gegensatz zu Azure, wo die Auswahl der Hostprozessortypen nach dem besten Preis-Leistungs-Verhältnis erfolgt, werden für SAP HANA in Azure (große Instanzen) die Prozessortypen mit der höchsten Leistung der Intel E7v3- und E7v4-Prozessorserie ausgewählt.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Ausführen mehrerer SAP HANA-Instanzen auf einer Einheit von HANA (große Instanz)
Es ist möglich, mehrere aktive SAP HANA-Instanzen auf Einheiten von HANA (große Instanz) zu hosten. Um Funktionen für Speichermomentaufnahmen und die Notfallwiederherstellung bereitzustellen, erfordert eine solche Konfiguration ein pro Instanz festgelegtes Volume. Zurzeit können Einheiten von HANA (große Instanz) wie folgt unterteilt werden:

- **S72, S72m, S144, S192**: In Inkrementen von 256 GB, wobei 256 GB die kleinste Einheit ist. Andere Inkremente wie 256 GB und 512 GB können bis zum Maximum des Arbeitsspeichers der Einheit kombiniert werden.
- **S144m und S192m**: In Inkrementen von 256 GB, wobei 512 GB die kleinste Einheit ist. Andere Inkremente wie 512 GB und 768 GB können bis zum Maximum des Arbeitsspeichers der Einheit kombiniert werden.
- **Typ-II-Klasse**: In Inkrementen von 512 GB, wobei 2 TB die kleinste Einheit ist. Andere Inkremente wie 512 GB, 1 TB und 1,5 TB können bis zum Maximum des Arbeitsspeichers der Einheit kombiniert werden.

Beispiele für die Ausführung mehrerer SAP HANA-Instanzen könnten wie folgt aussehen.

| SKU | Arbeitsspeichergröße | Speichergröße | Größen mit mehreren Datenbanken |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA-Instanz<br /> oder 1 x 512 GB-Instanz + 1 x 256 GB-Instanz<br /> oder 3 x 256 GB-Instanzen | 
| S72m | 1,5 TB | 6 TB | 3 x 512 GB HANA-Instanzen<br />oder 1 x 512 GB-Instanz + 1 x 1 TB-Instanz<br />oder 6 x 256 GB-Instanzen<br />oder 1x1,5 TB-Instanzen | 
| S192m | 4 TB | 16 TB | 8 x 512 GB-Instanzen<br />oder 4 x 1 TB-Instanzen<br />oder 4 x 512 GB-Instanzen + 2 x 1 TB-Instanzen<br />oder 4 x 768 GB-Instanzen + 2 x 512 TB-Instanzen<br />oder 1 x 4 TB-Instanz |
| S384xm | 8 TB | 22 TB | 4 x 2 TB-Instanzen<br />oder 2 x 4 TB-Instanzen<br />oder 2 x 3 TB-Instanzen + 1 x 2 TB-Instanzen<br />oder 2 x 2,5 TB-Instanzen + 1 x 3 TB-Instanzen<br />oder 1 x 8 TB-Instanz |


Andere Variationen sind ebenfalls möglich. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>Verwenden von SAP HANA Data Tiering und Extension Nodes
SAP unterstützt ein Data Tiering-Modell für SAP BW für verschiedene Releases von SAP NetWeaver und SAP BW/4HANA. Weitere Informationen zum Data Tiering-Modell finden Sie im SAP-Dokument [SAP BW/4HANA and SAP BW on HANA with SAP HANA extension nodes](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#) (SAP BW/4HANA und SAP BW unter HANA mit SAP HANA Extension Nodes).
Mit HANA (große Instanz) können Sie die Option-1-Konfiguration von SAP HANA Extension Nodes wie in den häufig gestellten Fragen und Blogdokumenten von SAP beschrieben verwenden. Option-2-Konfigurationen können mit folgenden HANA-SKUs (große Instanzen) eingerichtet werden: S72m, S192, S192m, S384 und S384m. 

In der Dokumentation ist der Vorteil möglicherweise nicht sofort offensichtlich. Wenn Sie jedoch einen Blick in die SAP-Dimensionierungsrichtlinien werfen, werden Sie sehen, welche Vorteile die Verwendung von Option-1- und Option-2-SAP HANA Extension Nodes bieten. Im Folgenden sind einige Beispiele aufgeführt:

- SAP HANA-Dimensionierungsrichtlinien erfordern in der Regel das Doppelte des Datenvolumens als Arbeitsspeicher. Wenn Sie Ihre SAP HANA-Instanz mit den heißen Daten ausführen, sind höchstens 50 % des Arbeitsspeichers mit Daten gefüllt. Der restliche Arbeitsspeicher wird im Idealfall für die Arbeit von SAP HANA reserviert.
- Das bedeutet, dass in einer HANA-S192-Einheit (große Instanz) mit 2 TB Arbeitsspeicher, in der eine SAP BW-Datenbank ausgeführt wird, nur 1 TB als Datenvolumen verfügbar ist.
- Wenn Sie einen zusätzlichen Option-1-SAP HANA Extension Node verwenden, würde eine S192-SKU von HANA (große Instanz) zusätzliche 2 TB Kapazität für das Datenvolumen bieten. Bei der Option-2-Konfiguration erhalten Sie ein zusätzliches Volumen von 4 TB für warme Daten. Gegenüber dem heißen Knoten kann die vollständige Speicherkapazität des „warmen“ Extension Node zur Datenspeicherung für die Option-1 verwendet werden. Bei der Option-2-SAP HANA Extension Node-Konfiguration kann die doppelte Menge an Arbeitsspeicher für das Datenvolumen genutzt werden.
- Bei Option-1 haben Sie letztlich eine Kapazität von 3 TB für Ihre Daten und ein Heiß/Warm-Verhältnis von 1:2. Bei der Option-2-Extension Node-Konfiguration verfügen Sie über 5 TB an Daten und ein Verhältnis von 1:4.

Je höher das Datenvolumen im Vergleich zum Arbeitsspeicher ist, desto wahrscheinlicher ist es, dass die von Ihnen angeforderten warmen Daten auf dem Datenträger gespeichert werden.


## <a name="operations-model-and-responsibilities"></a>Funktionsprinzipien und Zuständigkeiten

Der mit SAP HANA in Azure (große Instanzen) bereitgestellte Dienst ist auf Azure-IaaS-Dienste ausgerichtet. Sie erhalten eine Instanz von HANA (große Instanz) mit einem installierten Betriebssystem, das für SAP HANA optimiert ist. Wie bei Azure-IaaS-VMs sind Sie zuständig für die meisten Aufgaben im Zusammenhang mit dem Härten des Betriebssystems, Installieren zusätzlicher Software, Installieren von HANA, Ausführen des Betriebssystems und von HANA sowie dem Aktualisieren des Betriebssystems und von HANA. Microsoft erzwingt keine Betriebssystemupdates oder HANA-Updates.

![Zuständigkeiten von SAP HANA in Azure (große Instanzen)](./media/hana-overview-architecture/image2-responsibilities.png)

Wie im Diagramm dargestellt, ist SAP HANA in Azure (große Instanzen) ein IaaS-Angebot mit mehreren Mandanten. Die Trennung der Zuständigkeiten erfolgt zum größten Teil an der Infrastrukturgrenze zum Betriebssystem. Microsoft ist für alle Aspekte des Diensts unterhalb der Linie des Betriebssystems verantwortlich. Sie sind für alle Aspekte des Diensts oberhalb der Linie zuständig. Das Betriebssystem liegt in Ihrer Verantwortung. Sie können somit weiterhin die meisten aktuellen lokalen Methoden verwenden, die Sie für Konformität, Sicherheit, Anwendungsverwaltung, Basis und Betriebssystemverwaltung einsetzen. Die Systeme werden in jeder Hinsicht so dargestellt, als befänden sie sich in Ihrem Netzwerk.

Dieser Dienst ist für SAP HANA optimiert. Daher gibt es Bereiche, in denen Sie mit Microsoft zusammenarbeiten müssen, um die Funktionen der zugrunde liegenden Infrastruktur optimal zu nutzen.

Die folgende Liste enthält weitere Details zu den einzelnen Schichten und Ihren Zuständigkeiten:

**Netzwerk**: Alle internen Netzwerke für das Umfeld der großen Instanz unter SAP HANA. Sie sind verantwortlich für den Zugriff auf den Speicher, die Konnektivität zwischen den Instanzen (für horizontale Skalierung und andere Funktionen), die Konnektivität mit der Landschaft und die Konnektivität mit Azure, wenn die SAP-Anwendungsschicht auf VMs gehostet wird. Darüber hinaus sind Sie für die WAN-Konnektivität zwischen Azure-Rechenzentren zur Replikation für die Notfallwiederherstellung zuständig. Alle Netzwerke sind nach Mandanten partitioniert und unterliegen der Quality of Service (QoS).

**Speicher:** Der virtualisierte partitionierte Speicher für alle Volumes, die von den SAP HANA-Servern sowie für Momentaufnahmen benötigt werden. 

**Server:** Die dedizierten physischen Server zum Ausführen der SAP HANA-Datenbanken, die den Mandanten zugewiesen sind. Der Server der SKU-Klasse Typ I sind hardwareabstrahiert. Mit diesen Servertypen wird die Serverkonfiguration erfasst und in Profilen verwaltet, die von einer physischen Hardware auf eine andere physische Hardware verschoben werden können. Eine solche (manuelle) Verschiebung eines Profils durch Vorgänge ist mit der Azure-Dienstreparatur vergleichbar. Die Server der Typ-II-Klasse-SKU bieten keine solchen Funktionen.

**SDDC:** Die Verwaltungssoftware, die zum Verwalten von Rechenzentren als softwaredefinierte Entitäten verwendet wird. Sie ermöglicht Microsoft das Zusammenfassen von Ressourcen in Pools aus Skalierungs-, Verfügbarkeits- und Leistungsgründen.

**Betriebssystem:** Das von Ihnen ausgewählte Betriebssystem (SUSE Linux oder Red Hat Linux), das auf den Servern ausgeführt wird. Die Betriebssystemimages, die Ihnen zur Verfügung gestellt werden, wurden Microsoft von den einzelnen Linux-Anbietern zur Ausführung von SAP HANA bereitgestellt. Sie benötigen ein Abonnement des Linux-Anbieters für das jeweilige für SAP HANA optimierte Image. Sie sind für die Registrierung der Images beim Betriebssystemanbieter zuständig. 

Ab dem Zeitpunkt der Übergabe durch Microsoft sind Sie für das weitere Patchen des Linux-Betriebssystems verantwortlich. Dazu zählen auch zusätzliche Pakete, die ggf. für eine erfolgreiche SAP HANA-Installation erforderlich sind und nicht in den für SAP HANA optimierten Betriebssystemimages des jeweiligen Linux-Anbieters enthalten waren. (Weitere Informationen finden Sie in der Installationsdokumentation für SAP HANA und SAP-Hinweisen.) 

Sie sind verantwortlich für das Patchen des Betriebssystems im Fall von Fehlfunktionen oder zur Optimierung des Betriebssystems und der zugehörigen Treiber im Zusammenhang mit der spezifischen Serverhardware. Sie sind auch für das sicherheits- oder funktionsbezogene Patchen des Betriebssystems zuständig. 

In Ihrer Verantwortung liegt auch die Überwachung und Kapazitätsplanung für Folgendes:

- CPU-Ressourcennutzung
- Arbeitsspeichernutzung
- Datenträgervolumes im Zusammenhang mit freiem Speicherplatz, IOPS und Wartezeit
- Netzwerkvolume-Datenverkehr zwischen HANA (große Instanz) und der SAP-Anwendungsschicht

Die zugrunde liegende Infrastruktur von HANA (große Instanz) bietet Funktionen zur Sicherung und Wiederherstellung des Betriebssystemvolumes. Auch die Nutzung dieser Funktionalität liegt in Ihrer Verantwortung.

**Middleware:** Hauptsächlich die SAP HANA-Instanz. Verwaltung, Betrieb und Überwachung liegen in Ihrer Verantwortung. Sie können die bereitgestellten Funktionen zur Verwendung von Speichermomentaufnahmen für die Sicherung und Wiederherstellung sowie die Notfallwiederherstellung nutzen. Diese Funktionen werden von der Infrastruktur bereitgestellt. Zu Ihren Zuständigkeiten gehören auch das Entwickeln einer Hochverfügbarkeits- oder Notfallwiederherstellungslösung mit diesen Funktionen, deren Nutzung und die Überwachung der erfolgreichen Ausführung von Speichermomentaufnahmen.

**Daten:** Ihre von SAP HANA verwalteten Daten und andere Daten, beispielsweise Sicherungsdateien auf Volumes oder Dateifreigaben. Zu Ihren Zuständigkeiten zählen die Überwachung des freien Speicherplatzes und die Verwaltung des Inhalts auf den Volumes. Sie sind auch für die Überwachung der erfolgreichen Ausführung von Sicherungen von Datenträgervolumes und Speichermomentaufnahmen verantwortlich. Die erfolgreiche Ausführung der Datenreplikation an Notfallwiederherstellungsstandorten liegt in der Verantwortung von Microsoft.

**Anwendungen:** Die SAP-Anwendungsinstanzen oder bei Nicht-SAP-Anwendungen die Anwendungsschicht dieser Anwendungen. Ihre Zuständigkeiten umfassen die Bereitstellung, Verwaltung, Ausführung und Überwachung dieser Anwendungen. Sie sind verantwortlich für die Kapazitätsplanung für den CPU-Ressourcenverbrauch, die Arbeitsspeichernutzung, die Azure Storage-Nutzung und die Netzwerk-Bandbreitennutzung innerhalb virtueller Netzwerke. Sie sind auch für die Kapazitätsplanung für den Ressourcenverbrauch zwischen virtuellen Netzwerken und SAP HANA in Azure (große Instanzen) zuständig.

**WANs:** Die Verbindungen, die Sie zwischen lokalen Systemen und Azure-Bereitstellungen für Workloads herstellen. Alle Kunden mit HANA (große Instanz) verwenden Azure ExpressRoute für die Konnektivität. Diese Verbindung ist nicht Teil von SAP HANA in Azure (große Instanzen). Sie sind für die Einrichtung dieser Verbindung zuständig.

**Archiv:** Möglicherweise möchten Sie Datenkopien mit Ihren eigenen Methoden in Speicherkonten archivieren. Die Archivierung erfordert Verwaltungsaufwand, Konformität, Kosten und Vorgänge. Sie sind für die Erstellung von Archivkopien und Sicherungen in Azure sowie für die konforme Speicherung verantwortlich.

Informationen hierzu finden Sie in der [SLA für SAP HANA in Azure (große Instanzen)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Festlegen der Größe

Die Größenanpassung für HANA (große Instanz) unterscheidet sich nicht von der Größenanpassung für HANA im Allgemeinen. Für vorhandene und bereitgestellte Systeme, die Sie von anderen RDBMS auf HANA umstellen möchten, bietet SAP eine Reihe von Berichten, die auf Ihren vorhandenen SAP-Systemen ausgeführt werden können. Wenn die Datenbank zu HANA migriert wird, überprüfen diese Berichte die Daten und berechnen Arbeitsspeicheranforderungen für die HANA-Instanz. Weitere Informationen zur Ausführung dieser Berichte und zum Abrufen der aktuellen Patches oder Versionen finden Sie in den folgenden SAP-Hinweisen:

- [SAP-Hinweis 1793345 – Größenanpassung für die SAP-Suite für HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-Hinweis 1872170 – Bericht zur Größenanpassung für die Suite für HANA und S/4 HANA](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-Hinweis 2121330 – Häufig gestellte Fragen: Bericht zur Größenanpassung für SAP BW unter HANA](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-Hinweis 1736976 – Bericht zur Größenanpassung für BW unter HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-Hinweis 2296290 – Neuer Bericht zur Größenanpassung für BW unter HANA](https://launchpad.support.sap.com/#/notes/2296290)

Für Greenfield-Implementierungen steht der SAP Quick Sizer zur Verfügung, um Arbeitsspeicheranforderungen für die Implementierung von SAP-Software zusätzlich zu HANA zu berechnen.

Der Arbeitsspeicherbedarf für HANA nimmt mit zunehmender Datenmenge zu. Beobachten Sie Ihre aktuelle Arbeitsspeichernutzung, um den zukünftigen Bedarf zu prognostizieren. Basierend auf den Arbeitsspeicheranforderungen können Sie Ihren Bedarf dann einer der SKUs von HANA (große Instanz) zuordnen.

## <a name="requirements"></a>Requirements (Anforderungen)

Diese Liste führt die Anforderungen zum Ausführen von SAP HANA in Azure (große Instanzen) auf.

**Microsoft Azure**

- Ein Azure-Abonnement, das mit SAP HANA in Azure (große Instanzen) verknüpft werden kann.
- Microsoft Premier Support-Vertrag. Spezifische Informationen im Zusammenhang mit der Ausführung von SAP in Azure finden Sie im [SAP-Supporthinweis 2015553 – SAP in Microsoft Azure: Supportvoraussetzungen](https://launchpad.support.sap.com/#/notes/2015553). Wenn Sie Einheiten von HANA (große Instanz) mit 384 und mehr CPUs verwenden, muss der Premier Support-Vertrag Azure Rapid Response beinhalten.
- Kenntnis der SKUs von HANA (große Instanz), die Sie nach dem Ausführen einer Größenanpassung mit SAP benötigen.

**Netzwerkkonnektivität**

- ExpressRoute zwischen lokalen Systemen und Azure: Um Ihr lokales Rechenzentrum mit Azure zu verbinden, benötigen Sie mindestens eine 1-GBit/s-Verbindung von Ihrem Internetdienstanbieter (ISP). 

**Betriebssystem**

- Lizenzen für SUSE Linux Enterprise Server 12 für SAP-Anwendungen.

   > [!NOTE] 
   > Das von Microsoft bereitgestellte Betriebssystem ist nicht bei SUSE registriert. Es ist nicht mit einer Subscription Management Tool-Instanz verbunden.

- Bereitstellung von SUSE Linux Subscription Management Tool in Azure auf einer VM. Dieses Tool bietet die Möglichkeit, SAP HANA in Azure (große Instanzen) zu registrieren und entsprechend von SUSE aktualisieren zu lassen. (Im Rechenzentrum für HANA (große Instanz) besteht kein Internetzugriff.) 
- Lizenzen für Red Hat Enterprise Linux 6.7 oder 7.2 für SAP HANA.

   > [!NOTE]
   > Das von Microsoft bereitgestellte Betriebssystem ist nicht bei Red Hat registriert. Es ist nicht mit einer Red Hat Subscription Manager-Instanz verbunden.

- Bereitstellung von Red Hat Subscription Manager in Azure auf einer VM. Der Red Hat Subscription Manager bietet die Möglichkeit, SAP HANA in Azure (große Instanzen) zu registrieren und entsprechend von Red Hat aktualisieren zu lassen. (In dem Mandanten, der im Azure-Umfeld der großen Instanz bereitgestellt wird, besteht kein direkter Internetzugriff.)
- SAP setzt voraus, dass Sie auch einen Support-Vertrag mit Ihrem Linux-Anbieter haben. Diese Anforderung gilt auch bei der Verwendung von HANA (große Instanz) und trotz der Tatsache, dass Sie Linux in Azure ausführen. Anders als bei einigen der Images im Linux Azure-Katalog ist die Servicegebühr *nicht* im Lösungsangebot von HANA (große Instanz) enthalten. Es liegt in Ihrer Verantwortung, die Anforderungen von SAP bezüglich der Supportverträge mit dem Linux-Vertreiber zu erfüllen. 
   - Die Anforderungen bezüglich der Supportverträge für SUSE Linux finden Sie in [SAP-Hinweis 1984787 – SUSE Linux Enterprise Server 12: Installationshinweise](https://launchpad.support.sap.com/#/notes/1984787) und [SAP-Hinweis 1056161 – SUSE-Prioritätssupport für SAP-Anwendungen](https://launchpad.support.sap.com/#/notes/1056161).
   - Für Red Hat Linux benötigen Sie die richtigen Abonnementebenen, die Support und Dienstupdates für die Betriebssysteme von HANA (große Instanz) umfassen. Red Hat empfiehlt das Abonnement „Red Hat Enterprise Linux für [SAP-Lösungen]“ (https://access.redhat.com/solutions/3082481). 

Die Supportmatrix der anderen SAP HANA-Versionen mit den verschiedenen Linux-Versionen finden Sie im [SAP-Hinweis 2235581](https://launchpad.support.sap.com/#/notes/2235581).

Die Kompatibilitätsmatrix des Betriebssystems und der HLI-Firmware-/Treiberversionen finden Sie unter [Operating System Upgrade](os-upgrade-hana-large-instance.md) (Betriebssystemupgrade).


**Datenbank**

- Lizenzen und Softwareinstallationskomponenten für SAP HANA (Plattform oder Enterprise Edition).

**Anwendungen**

- Lizenzen und Softwareinstallationskomponenten für alle SAP-Anwendungen, die eine Verbindung mit SAP HANA herstellen, sowie zugehörige SAP-Supportverträge.
- Lizenzen und Softwareinstallationskomponenten für alle Nicht-SAP-Anwendungen, die in Zusammenhang mit Umgebungen für SAP HANA in Azure (große Instanzen) verwendet werden, sowie zugehörige Supportverträge.

**Fähigkeiten**

- Erfahrung und Kenntnisse in Bezug auf IaaS und die zugehörigen Komponenten
- Erfahrung und Kenntnisse in Bezug auf die Bereitstellung von SAP-Workloads in Azure
- Für die Installation von SAP HANA zertifizierte Mitarbeiter
- Kenntnisse der SAP-Architektur zum Entwerfen der Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA

**SAP**

- Es wird vorausgesetzt, das Sie ein SAP-Kunde sind und über einen Supportvertrag mit SAP verfügen.
- Insbesondere für Implementierungen der Typ-II-Klasse-SKUs von HANA (große Instanz) sollten Sie SAP bezüglich der SAP HANA-Versionen und möglichen Konfigurationen auf großer, zentral hochskalierter Hardware zurate ziehen.


## <a name="storage"></a>Speicher

Das Speicherlayout für SAP HANA in Azure (große Instanzen) wird von SAP HANA nach dem klassischen Bereitstellungsmodell anhand von Richtlinien konfiguriert, die von SAP empfiehlt. Die Richtlinien sind im Whitepaper [SAP HANA storage requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (SAP HANA-Speicheranforderungen) dokumentiert.

Bei HANA (große Instanz) der Typ-I-Klasse ist das Arbeitsspeichervolume viermal so groß wie das Speichervolume. Bei Einheiten von HANA (große Instanz) der Typ-II-Klasse ist dies nicht der Fall. Die Einheiten verfügen über ein Volume zum Speichern von HANA-Transaktionsprotokollsicherungen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Informationen zur Speicherzuordnung finden Sie in der folgenden Tabelle. Die Tabelle enthält die ungefähre Kapazität für die verschiedenen Volumes, die mit den unterschiedlichen Einheiten von HANA (große Instanz) bereitgestellt werden.

| HANA-SKU (große Instanzen) | hana/data | hana/log | hana/shared | hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1.280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3.328 GB | 768 GB |1.280 GB | 768 GB |
| S192 | 4.608 GB | 1.024 GB | 1.536 GB | 1.024 GB |
| S192m | 11.520 GB | 1.536 GB | 1.792 GB | 1.536 GB |
| S192xm |  12.000 GB |  2.050 GB |  2.050 GB |  2040 GB |
| S384 | 11.520 GB | 1.536 GB | 1.792 GB | 1.536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2.040 GB |
| S384xm | 16.000 GB | 2.050 GB | 2.050 GB | 2.040 GB |
| S384xxm |  20.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S576m | 20.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S576xm | 31744 GB | 4096 GB | 2048 GB | 4096 GB |
| S768m | 28.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S768xm | 40960 GB | 6144 GB | 4096 GB | 6144 GB |
| S960m | 36.000 GB | 4.100 GB | 2.050 GB | 4.100 GB |


Die tatsächlich bereitgestellten Volumes können je nach Bereitstellung und Tool, das zum Anzeigen der Volumegrößen verwendet wird, variieren.

Wenn Sie eine SKU von HANA (große Instanz) unterteilen, könnten die Unterteilungen beispielsweise wie folgt aussehen:

| Speicherpartition in GB | hana/data | hana/log | hana/shared | hana/log/backup |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1.280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1.792 GB | 640 GB | 1.024 GB | 640 GB |
| 1.536 | 3.328 GB | 768 GB | 1.280 GB | 768 GB |


Diese Größen sind ungefähre Zahlen, die je nach Bereitstellung und den zum Anzeigen der Volumes verwendeten Tools geringfügig variieren können. Es sind auch weitere Partitionsgrößen wie 2,5 TB verfügbar. Diese Speichergrößen werden mit einer Formel berechnet, die der für die vorherigen Partitionen verwendeten Formel ähnelt. Der Begriff „Partitionen“ bedeutet nicht, dass das Betriebssystem, der Arbeitsspeicher oder die CPU-Ressourcen in irgendeiner Weise partitioniert werden. Er verweist nur auf Speicherpartitionen für die verschiedenen HANA-Instanzen, die Sie auf einer einzelnen Einheit von HANA (große Instanz) bereitstellen können. 

Möglicherweise benötigen Sie mehr Speicher. Sie können Speicher hinzufügen, indem Sie zusätzlichen Speicher in Einheiten von 1 TB erwerben. Dieser zusätzliche Speicher kann als zusätzliches Volume hinzugefügt werden. Er kann auch verwendet werden, um vorhandene Volumes zu erweitern. Es ist nicht möglich, die Größe der ursprünglich bereitgestellten Volumes (größtenteils in den obigen Tabellen aufgeführt) zu verringern. Ebenso ist es nicht möglich, die Namen der Volumes oder Bereitstellungsnamen zu ändern. Die oben beschriebenen Speichervolumes werden als NFS4-Volumes an die Einheiten von HANA (große Instanz) angefügt.

Sie können Speichermomentaufnahmen zur Sicherung und Wiederherstellung sowie zur Notfallwiederherstellung verwenden. Weitere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten
Der für HANA (große Instanz) genutzte Speicher ermöglicht eine transparente Verschlüsselung der Daten, die auf den Datenträgern gespeichert werden. Sie können diese Verschlüsselung beim Bereitstellen einer Einheit von HANA (große Instanz) aktivieren. Die Verschlüsselung von Volumes kann auch nach der Bereitstellung aktiviert werden. Der Wechsel von unverschlüsselten zu verschlüsselten Volumes erfolgt transparent und erfordert keine Downtime. 

Bei SKUs der Typ-I-Klasse wird das Volume verschlüsselt, auf dem die Start-LUN gespeichert ist. Bei der Typ-II-Klasse der SKUs von HANA (große Instanz) müssen Sie die Start-LUN mit Methoden des Betriebssystems verschlüsseln. Für weitere Informationen wenden Sie sich an das Microsoft Service Management-Team.


## <a name="networking"></a>Netzwerk

Die Architektur von Azure-Netzwerkdiensten ist eine wichtige Komponente der erfolgreichen Bereitstellung von SAP-Anwendungen unter HANA (große Instanz). In der Regel weisen Bereitstellungen von SAP HANA in Azure (große Instanzen) eine größere SAP-Landschaft mit mehreren verschiedenen SAP-Lösungen mit Datenbanken unterschiedlicher Größe sowie variierender CPU-Ressourcen- und Arbeitsspeichernutzung auf. Wahrscheinlich basieren nicht all diese SAP-Systeme auf SAP HANA. Ihre SAP-Landschaft ist wahrscheinlich eine hybride Landschaft, die Folgendes umfasst:

- Lokal bereitgestellte SAP-Systeme. Aufgrund ihrer Größe können diese Systeme derzeit nicht in Azure gehostet werden. Ein Beispiel ist ein SAP ERP-Produktionssystem, das auf SQL Server (als Datenbank) ausgeführt wird und mehr CPU- oder Arbeitsspeicherressourcen erfordert als VMs bereitstellen können.
- Lokal bereitgestellte, auf SAP HANA basierende SAP-Systeme.
- Auf VMs bereitgestellte SAP-Systeme. Bei diesen Systemen kann es sich um Entwicklungs-, Test-, Sandbox- oder Produktionsinstanzen beliebiger SAP NetWeaver-basierter Anwendungen handeln, die je nach Ressourcennutzung und Arbeitsspeicherbedarf erfolgreiche in Azure (auf VMs) bereitgestellt werden können. Diese Systeme können auch auf Datenbanken wie SQL Server basieren. Weitere Informationen finden Sie im [SAP-Hinweis 1928533 – SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen](https://launchpad.support.sap.com/#/notes/1928533/E). Diese Systeme können auch auf Datenbanken wie SAP HANA basieren. Weitere Informationen finden Sie unter [SAP HANA certified IaaS platforms](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) (SAP HANA zertifizierte IaaS-Plattformen).
- In Azure (auf VMs) bereitgestellte SAP-Anwendungsserver, die SAP HANA in Azure (große Instanzen) in Azure-Umfeldern der großen Instanz nutzen.

Eine hybride SAP-Landschaft mit vier oder mehr unterschiedlichen Bereitstellungsszenarien ist typisch. Es gibt auch viele Kunden, die vollständige SAP-Landschaften in Azure ausführen. Da VMs immer leistungsstärker werden, nimmt die Anzahl von Kunden zu, die ihre sämtlichen SAP-Lösungen zu Azure migrieren.

Das Azure-Netzwerk im Kontext von in Azure bereitgestellten SAP-Systemen ist nicht kompliziert. Es beruht auf den folgenden Prinzipien:

- Virtuelle Azure-Netzwerke müssen mit der ExpressRoute-Leitung verbunden sein, die eine Verbindung mit dem lokalen Netzwerk herstellt.
- Eine ExpressRoute-Leitung, die lokale Systeme mit Azure verbindet, sollte in der Regel über eine Bandbreite von mindestens 1 GBit/s verfügen. Diese minimale Bandbreite ist ausreichend für die Übertragung von Daten zwischen lokalen Systemen und auf VMs ausgeführten Systemen. Sie bietet auch eine angemessene Bandbreite für die Verbindung von lokalen Benutzern mit Azure-Systemen.
- Alle SAP-Systeme in Azure müssen in virtuellen Netzwerken eingerichtet werden, um miteinander kommunizieren zu können.
- Die lokal gehostete Active Directory-Instanz und der lokale DNS-Dienst werden über ExpressRoute von der lokalen Umgebung auf Azure ausgedehnt.


> [!NOTE] 
> Hinsichtlich der Abrechnung kann nur ein Azure-Abonnement mit einem einzigen Mandanten in einem Umfeld der großen Instanz in einer bestimmten Azure-Region verknüpft werden. Umgekehrt kann ein einziger Mandant im Umfeld der großen Instanz nur mit einem Azure-Abonnement verknüpft werden. Diese Anforderung gilt auch für andere abrechenbare Objekte in Azure.

Wenn SAP HANA in Azure (große Instanzen) in mehreren verschiedenen Azure-Regionen bereitgestellt wird, wird ein separater Mandant im Umfeld der großen Instanz bereitgestellt. Sie können beide Mandanten im gleichen Azure-Abonnement ausführen, solange diese Instanzen zur gleichen SAP-Landschaft gehören. 

> [!IMPORTANT] 
> Für SAP HANA in Azure (große Instanzen) wird nur die Azure Resource Manager-Bereitstellung unterstützt.

 

### <a name="additional-virtual-network-information"></a>Zusätzliche Informationen zum virtuellen Netzwerk

Zum Verbinden eines virtuellen Netzwerks mit ExpressRoute muss ein Azure-Gateway erstellt werden. Weitere Informationen finden Sie unter [Informationen zu Gateways für virtuelle Netzwerke für ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Ein Azure-Gateway kann mit einer ExpressRoute-Verbindung mit einer Infrastruktur außerhalb von Azure oder mit einem Azure-Umfeld der großen Instanz verwendet werden. Ein Azure-Gateway kann auch verwendet werden, um eine Verbindung zwischen virtuellen Netzwerken herzustellen. Weitere Informationen finden Sie im Artikel zum [Konfigurieren einer Verbindung zwischen Netzwerken für Resource Manager mithilfe von PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können das Azure-Gateway mit bis zu vier verschiedenen ExpressRoute-Verbindungen verbinden, solange diese von unterschiedlichen Microsoft-Unternehmensedgeroutern stammen. Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Der von einem Azure-Gateway bereitgestellte Durchsatz unterscheidet sich für die beiden Anwendungsfälle. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Der maximale Durchsatz, den Sie über eine ExpressRoute-Verbindung mit einem virtuellen Netzwerkgateway erreichen können, beträgt 10 GBit/s. Beim Kopieren von Dateien zwischen einer VM in einem virtuellen Netzwerk und einem lokalen System (als einzelner Kopierdatenstrom) wird nicht der volle Durchsatz der anderen Gateway-SKUs erzielt. Verwenden Sie mehrere Datenströme, um die vollständige Bandbreite des virtuellen Netzwerkgateways zu nutzen. Andernfalls müssen Sie verschiedene Dateien in parallelen Datenströmen einer einzelnen Datei kopieren.


### <a name="networking-architecture-for-hana-large-instance"></a>Netzwerkarchitektur für HANA (große Instanz)
Die Netzwerkarchitektur für HANA (große Instanz) kann in vier verschiedene Teile unterteilt werden:

- Lokales Netzwerk und ExpressRoute-Verbindung mit Azure. Dieser Teil ist die Domäne des Kunden und wird über ExpressRoute mit Azure verbunden. Er ist unten rechts in der folgenden Abbildung dargestellt.
- Azure-Netzwerkdienste (wie zuvor erläutert) mit virtuellen Netzwerken, die wiederum über Gateways verfügen. Dieser Teil ist ein Bereich, für den Sie die geeigneten Entwürfe für Ihre Anwendungs-, Sicherheits- und Konformitätsanforderungen finden müssen. Ob Sie HANA (große Instanz) verwenden, ist ein weiterer Aspekt, der im Hinblick auf die Auswahl der Anzahl virtueller Netzwerke und Azure-Gateway-SKUs zu berücksichtigen ist. Dieser Teil ist oben rechts in der Abbildung dargestellt.
- Konnektivität von HANA (große Instanz) über ExpressRoute-Technologie in Azure. Diesem Teil wird von Microsoft bereitgestellt und gehandhabt. Sie müssen lediglich nach der Bereitstellung Ihrer Ressourcen in HANA (große Instanz) einige IP-Adressbereiche angeben, um die ExpressRoute-Leitung mit den virtuellen Netzwerken zu verbinden. Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Netzwerk in HANA (große Instanz), das für Sie größtenteils transparent ist.

![Mit SAP HANA in Azure (große Instanzen) und lokal verbundenes virtuelles Netzwerk](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Die Anforderung, dass Ihre lokalen Ressourcen über ExpressRoute mit Azure verbunden werden müssen, ändert sich durch die Verwendung von HANA (große Instanz) nicht. Die Notwendigkeit von mindestens einem virtuellen Netzwerk zur Ausführung der VMs, die die Anwendungsschicht hosten, die mit den in Einheiten von HANA (große Instanz) gehosteten HANA-Instanzen verbunden sind, ändert sich ebenfalls nicht. 

Für SAP-Bereitstellungen in Azure gelten folgende Unterschiede:

- Die Einheiten von HANA (große Instanz) Ihres Kundenmandanten sind über eine andere ExpressRoute-Leitung mit Ihren virtuellen Netzwerken verbunden. Zur Trennung der Ladebedingungen nutzen die ExpressRoute-Verbindungen zwischen lokalen Systemen und virtuellen Netzwerken und die Verbindungen zwischen virtuellen Netzwerken und HANA (große Instanz) nicht die gleichen Router.
- Das Workloadprofil zwischen der SAP-Anwendungsschicht und HANA (große Instanz) unterscheidet sich insofern, dass viele kleine Anforderungen und Bursts wie Datenübertragungen (Resultsets) von SAP HANA an die Anwendungsschicht gesendet werden.
- Die SAP-Anwendungsarchitektur ist empfindlicher gegenüber der Netzwerklatenz als typische Szenarien, in denen Daten zwischen lokalen Systemen und Azure ausgetauscht werden.
- Das virtuelle Netzwerkgateway verfügt über mindestens zwei ExpressRoute-Verbindungen. Beide Verbindungen nutzen gemeinsam die maximale Bandbreite für eingehende Daten des virtuellen Netzwerkgateways.

Die Netzwerklatenz zwischen VMs und Einheiten von HANA (große Instanz) kann höher sein als die typische Roundtriplatenz in einem Netzwerk zwischen VMs. Je nach Azure-Region können die gemessenen Werte die Roundtriplatenz von 0,7 ms übersteigen, die im [SAP-Hinweis 1100926 – Häufig gestellte Fragen: Netzwerkleistung](https://launchpad.support.sap.com/#/notes/1100926/E) als unterdurchschnittlich klassifiziert ist. Trotzdem stellen Kunden SAP HANA-basierte SAP-Produktionsanwendungen erfolgreich unter SAP HANA (große Instanz) bereit. Diese Kunden berichten von großen Verbesserungen, die Sie durch die Ausführung ihrer SAP-Anwendungen unter SAP HANA mit Einheiten von HANA (große Instanz) erzielt haben. Sie sollten Ihre Geschäftsprozesse unbedingt gründlich in SAP HANA in Azure (große Instanzen) testen.
 
Die Auswahl der SKU für das virtuelle Netzwerkgateway ist entscheidend, um eine deterministische Netzwerklatenz zwischen VMs und HANA (große Instanz) bereitzustellen. Im Gegensatz zu den Datenverkehrsmustern zwischen lokalen Systemen und VMs kann das Datenverkehrsmuster zwischen VMs und HANA (große Instanz) kleine, aber hohe Bursts von zu übertragenden Anforderungen und Datenvolumen hervorbringen. Damit solche Bursts korrekt verarbeitet werden, empfehlen wir die Verwendung der UltraPerformance-Gateway-SKU. Für die Typ-II-Klasse-SKUs von HANA (große Instanz) muss die UltraPerformance-Gateway-SKU als virtuelles Netzwerkgateway verwendet werden.

> [!IMPORTANT] 
> Beim gesamten Netzwerkdatenverkehr zwischen den SAP-Anwendungs- und Datenbankschichten werden zum Herstellen einer Verbindung mit SAP HANA in Azure (große Instanzen) nur die HighPerformance- oder UltraPerformance-Gateway-SKUs für virtuelle Netzwerke unterstützt. Für Typ-II-SKUs von HANA (große Instanz) wird nur die UltraPerformance-Gateway-SKU als virtuelles Netzwerkgateway unterstützt.



### <a name="single-sap-system"></a>Einzelnes SAP-System

Die zuvor gezeigte lokale Infrastruktur ist über ExpressRoute mit Azure verbunden. Die ExpressRoute-Leitung ist mit einem Unternehmensedgerouter verbunden. Weitere Informationen finden Sie in der [technischen Übersicht über ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nachdem die Route eingerichtet wurde, stellt sie eine Verbindung mit dem Azure-Backbone her, und es kann auf alle Azure-Regionen zugegriffen werden.

> [!NOTE] 
> Stellen Sie zum Ausführen von SAP-Landschaften in Azure eine Verbindung mit dem Unternehmensedgerouter her, der der Azure-Region in der SAP-Landschaft am nächsten ist. Azure-Umfelder der großen Instanz werden über dedizierte Unternehmensedgerouter-Geräte verbunden, um die Netzwerklatenz zwischen VMs in Azure-IaaS und Umfeldern der großen Instanz zu minimieren.

Das virtuelle Netzwerkgateway für die VMs, die die SAP-Anwendungsinstanzen hosten, ist mit der ExpressRoute-Leitung verbunden. Dasselbe virtuelle Netzwerk ist mit einem separaten Unternehmensedgerouter verbunden, der zum Herstellen einer Verbindung mit Umfeldern der großen Instanz vorgesehen ist.

Dieses System ist ein einfaches Beispiel für ein einzelnes SAP-System. Die SAP-Anwendungsschicht wird in Azure gehostet. Die SAP HANA-Datenbank wird in SAP HANA in Azure (große Instanzen) ausgeführt. Dabei wird davon ausgegangen, dass die Bandbreite des virtuellen Netzwerkgateways mit einem Durchsatz von 2 GBit/s oder 10 GBit/s keinen Engpass darstellt.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Mehrere SAP-Systeme oder große SAP-Systeme

Wenn mehrere SAP-Systeme oder große SAP-Systeme zur Verbindung mit SAP HANA in Azure (große Instanzen) bereitgestellt werden, kann sich der Durchsatz des virtuellen Netzwerkgateways als Engpass erweisen. Teilen Sie in diesem Fall die Anwendungsschichten auf mehrere virtuelle Netzwerke auf. In Fällen wie den folgenden können Sie auch ein spezielles virtuelles Netzwerk erstellen, das mit HANA (große Instanz) verbunden wird:

- Direktes Ausführen von Sicherungen über die HANA-Instanzen in HANA (große Instanz) auf einer VM, die NFS-Freigaben hostet.
- Kopieren großer Sicherungen oder anderer Dateien aus Einheiten von HANA (große Instanzen) auf Festplattenspeicher, der in Azure verwaltet wird.

Verwenden Sie ein separates virtuelles Netzwerk zum Hosten von VMs, die Speicher verwalten. Dadurch vermeiden Sie, dass sich große Datei- oder Datenübertragungen von HANA (große Instanz) an Azure auf das virtuelle Netzwerkgateway für die VMs auswirken, die die SAP-Anwendungsschicht ausführen. 

Gehen Sie für eine stärker skalierbare Netzwerkarchitektur folgendermaßen vor:

- Nutzen Sie mehrere virtuelle Netzwerke für eine einzelne, größere SAP-Anwendungsschicht.
- Stellen Sie für jedes bereitgestellte SAP-System ein separates virtuelles Netzwerk bereit, anstatt diese SAP-Systeme in separaten Subnetzen unter demselben virtuellen Netzwerk zu kombinieren.

 Eine stärker skalierbare Netzwerkarchitektur für SAP HANA in Azure (große Instanzen):

![Bereitstellen einer SAP-Anwendungsschicht über mehrere virtuelle Netzwerke](./media/hana-overview-architecture/image4-networking-architecture.png)

Die Abbildung zeigt die Bereitstellung der SAP-Anwendungsschicht bzw. -Komponenten über mehrere virtuelle Netzwerke. Diese Konfiguration führt zu einer unvermeidbaren höheren Wartezeit während der Kommunikation zwischen den in diesen virtuellen Netzwerken gehosteten Anwendungen. Der Netzwerkdatenverkehr zwischen VMs in verschiedenen virtuellen Netzwerken wird in dieser Konfiguration standardmäßig über die Unternehmensedgerrouter geleitet. Seit September 2016 kann dieses Routing optimiert werden. 

Die Methode zum Optimieren und Verkürzen der Wartezeit bei der Kommunikation zwischen zwei virtuellen Netzwerken ist das Peering virtueller Netzwerke innerhalb derselben Region. Diese Methode funktioniert auch, wenn die virtuellen Netzwerke in verschiedenen Abonnements enthalten sind. Beim Peering virtueller Netzwerke kann für die Kommunikation zwischen VMs in zwei verschiedenen virtuellen Netzwerken der Azure-Netzwerkbackbone zur direkten Kommunikation verwendet werden. Die Wartezeit ist mit der bei VMs im selben virtuellen Netzwerk identisch. Datenverkehr an IP-Adressbereiche, die über das virtuelle Azure-Netzwerkgateway verbunden sind, wird über das individuelle virtuelle Netzwerkgateway des virtuellen Netzwerks weitergeleitet. 

Weitere Informationen zum Peering virtueller Netzwerke finden Sie unter [Peering in virtuellen Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routing in Azure

Es gibt drei wichtige Überlegungen zum Netzwerkrouting für SAP HANA in Azure (große Instanzen):

* SAP HANA in Azure (große Instanzen) ist nur über VMs und die dedizierte ExpressRoute-Verbindung und nicht direkt vom lokalen System aus zugänglich. Der direkte Zugriff vom lokalen System auf die Einheiten von HANA (große Instanz) ist in der von Microsoft bereitgestellten Konfiguration nicht sofort möglich. Die Einschränkungen beim transitiven Routing sind auf die aktuelle Azure-Netzwerkarchitektur zurückzuführen, die für SAP HANA (große Instanz) verwendet wird. Manche Verwaltungsclients und alle Anwendungen, die direkten Zugriff erfordern (beispielsweise eine lokale Instanz von SAP Solution Manager), können keine Verbindung mit der SAP HANA-Datenbank herstellen.

* Wenn Sie für die Notfallwiederherstellung Einheiten von HANA (große Instanz) in zwei verschiedenen Azure-Regionen bereitgestellt haben, gelten dieselben vorübergehenden Routingeinschränkungen. Mit anderen Worten: Die IP-Adressen einer Einheit von HANA (große Instanz) in einer Region (z. B. „USA, Westen“) werden nicht an eine Einheit von HANA (große Instanz) in einer anderen Region (z.B. „USA, Osten“) weitergeleitet. Diese Einschränkung gilt unabhängig von der Verwendung des Azure-Netzwerkpeerings zwischen Regionen oder der Querverbindung von ExpressRoute-Leitungen, die Einheiten von HANA (große Instanz) mit virtuellen Netzwerken verbinden. Eine grafische Darstellung finden Sie in der Abbildung im Abschnitt „Verwenden von Einheiten von HANA (große Instanz) in mehreren Regionen“. Diese Einschränkung, die auf die bereitgestellte Architektur zurückzuführen ist, verhindert die unmittelbare Verwendung der HANA-Systemreplikation als Funktionalität zur Notfallwiederherstellung.

* Einheiten von SAP HANA in Azure (große Instanzen) verfügen über eine zugewiesene IP-Adresse aus dem IP-Adressbereich des Serverpools, den Sie angegeben haben. Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Der Zugriff auf diese IP-Adresse ist über die Azure-Abonnements und die ExpressRoute-Verbindung möglich, die virtuelle Netzwerke mit HANA in Azure (große Instanzen) verbindet. Die zugewiesene IP-Adresse aus dem IP-Adressbereich des Serverpools wird der Hardwareeinheit direkt zugewiesen. Anders als bei den ersten Bereitstellungen dieser Lösung wird sie *nicht* mehr über NAT zugewiesen. 

> [!NOTE] 
> Verwenden Sie zusätzliche Komponenten für das Routing, um die in den ersten beiden Listeneinträgen erläuterte Einschränkung beim vorübergehenden Routing zu umgehen. Zu diesem Zweck können folgende Komponenten verwendet werden:

> * Ein Reverseproxy zum Weiterleiten von Daten in beide Richtungen. Beispiel: F5 BIG-IP, NGINX mit Traffic Manager, in Azure als virtuelle Firewall-/Datenverkehrsroutinglösung bereitgestellt.
> * [IPTables-Regeln](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) auf einer Linux-VM, um das Routing zwischen lokalen Standorten und Einheiten von HANA (große Instanz) oder zwischen Einheiten von HANA (große Instanz) in unterschiedlichen Regionen zu ermöglichen.

> Beachten Sie, dass Microsoft keine Implementierung und keinen Support für benutzerdefinierte Lösungen mit Netzwerkgeräten von Drittanbietern oder IPTables bietet. Der Support muss durch den Hersteller der verwendeten Komponente oder den Integrator bereitgestellt werden. 

### <a name="internet-connectivity-of-hana-large-instance"></a>Internetkonnektivität von HANA (große Instanz)
HANA (große Instanz) verfügt *nicht* über direkte Internetkonnektivität. Daher können Sie das Betriebssystemimage möglicherweise nicht direkt beim Hersteller des Betriebssystems registrieren. Gegebenenfalls müssen Sie dazu den lokalen SUSE Linux Enterprise Server Subscription Management Tool-Server oder Red Hat Enterprise Linux Subscription Manager verwenden.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>Datenverschlüsselung zwischen VMs und HANA (große Instanz)
Zwischen HANA (große Instanz) und VMs übertragene Daten werden nicht verschlüsselt. Sie können jedoch für den Austausch zwischen der HANA DBMS-Seite und JDBC-/ODBC-basierten Anwendungen die Verschlüsselung des Datenverkehrs aktivieren. Weitere Informationen finden Sie in [dieser Dokumentation von SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>Verwenden von Einheiten von HANA (große Instanz) in mehreren Regionen

Möglicherweise haben Sie neben der Notfallwiederherstellung auch andere Gründe, SAP HANA in Azure (große Instanzen) in mehreren Azure-Regionen bereitzustellen. Vielleicht möchten Sie von jeder der VMs, die in den verschiedenen virtuellen Netzwerken in den Regionen bereitgestellt werden, auf HANA (große Instanz) zugreifen. Die zugewiesenen IP-Adressen der verschiedenen Einheiten von HANA (große Instanz) werden nicht über die virtuellen Netzwerke, die direkt über ihr Gateway mit den Instanzen verbunden sind, hinaus verteilt. Dies führt zu einer geringfügigen Änderung im Entwurf des virtuellen Netzwerks. Ein virtuelles Netzwerkgateway kann vier verschiedene ExpressRoute-Leitungen von verschiedenen Unternehmensedgeroutern unterstützen. Jedes virtuelle Netzwerk, das mit einem der Umfelder von HANA (große Instanz) verbunden ist, kann mit dem Umfeld von HANA (große Instanz) in einer anderen Azure-Region verbunden werden.

![Mit Azure-Umfeldern der großen Instanz in verschiedenen Azure-Regionen verbundene virtuelle Netzwerke](./media/hana-overview-architecture/image8-multiple-regions.png)

Die Abbildung zeigt, wie die verschiedenen virtuellen Netzwerke in beiden Regionen mit zwei verschiedenen ExpressRoute-Leitungen verbunden sind, die zum Herstellen einer Verbindung mit SAP HANA in Azure (große Instanzen) in beiden Azure-Regionen verwendet werden. Die neu eingeführten Verbindungen werden durch die rechteckigen roten Linien dargestellt. Mit diesen ausgehenden Verbindungen der virtuellen Netzwerke können die in einem dieser virtuellen Netzwerke ausgeführten VMs auf jede der in den beiden Regionen bereitgestellten verschiedenen Einheiten von HANA (große Instanz) zugreifen. Wie die Abbildung zeigt, werden dabei zwei ExpressRoute-Verbindungen zwischen den lokalen Systemen und den beiden Azure-Regionen vorausgesetzt. Diese Konfiguration wird aus Gründen der Notfallwiederherstellung empfohlen.

> [!IMPORTANT] 
> Falls Sie mehrere ExpressRoute-Leitungen verwendet haben, sollten das Voranstellen von AS Path und lokale BGP-Einstellungen verwendet werden, um ein korrektes Routing des Datenverkehrs sicherzustellen.



---
title: Optimierung der TCP/IP-Leistung für Azure-VMs | Microsoft-Dokumentation
description: Lernen Sie verschiedene allgemeine Techniken zur TCP/IP-Leistungsoptimierung und ihre Beziehung zu Azure-VMs kennen.
services: virtual-network
documentationcenter: na
author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
manager: paragk
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/30/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: 664c8b659152a370d7fb31907b6cdbcd414dce31
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905098"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>Optimierung der TCP/IP-Leistung für Azure-VMs

In diesem Artikels werden allgemeine Techniken zur TCP/IP-Leistungsoptimierung und deren Überlegungen für virtuelle Maschinen, die auf Microsoft Azure ausgeführt werden, erläutert. Es ist wichtig, zuerst ein grundlegendes Verständnis der Konzepte zu haben und dann zu besprechen, wie sie abgestimmt werden können.

## <a name="common-tcpip-tuning-techniques"></a>Allgemeine Techniken zur TCP/IP-Leistungsoptimierung

### <a name="mtu-fragmentation-and-large-send-offload-lso"></a>Maximale Übertragungseinheit (MTU), Fragmentierung und Large Send Offload (LSO)

#### <a name="explanation-of-mtu"></a>Erläuterungen zu MTU

Die maximale Übertragungseinheit (MTU) ist der größte in Bytes angegebene Frame (Paket), der über eine Netzwerkschnittstelle gesendet werden kann. Die MTU ist eine konfigurierbare Einstellung und die Standard-MTU, die auf Azure-VMs verwendet wird, und die Standardeinstellung auf den meisten Netzwerkgeräten weltweit, beträgt 1.500 Bytes.

#### <a name="explanation-of-fragmentation"></a>Erläuterungen zu Fragmentierung

Fragmentierung tritt auf, wenn ein Paket gesendet wird, dessen Größe die MTU einer Netzwerkschnittstelle überschreitet. Der TCP/IP-Stapel zerlegt das Paket in kleinere Teile (Fragmente), die der MTU der Schnittstellen entsprechen. Die Fragmentierung erfolgt auf der IP-Ebene und ist unabhängig vom zugrunde liegenden Protokoll (z.B. TCP). Wenn ein 2000-Byte-Paket über eine Netzwerkschnittstelle mit einer MTU von 1.500 gesendet wird, wird es in ein 1.500-Byte-Paket und ein 500-Byte-Paket zerlegt.

Netzwerkgeräte im Pfad zwischen Quelle und Ziel haben die Möglichkeit, Pakete, die die MTU überschreiten, zu verwerfen oder das Paket in kleinere Teile zu zerlegen.

#### <a name="the-dont-fragment-df-bit-in-an-ip-packet"></a>Das Bit „Don't Fragment (DF)“ in einem IP-Paket

Das „Don't Fragment“-Bit ist ein Flag im IP-Protokoll-Header. Wenn das DF-Bit festgelegt ist, bedeutet dies, dass zwischengeschaltete Netzwerkgeräte auf dem Weg zwischen Sender und Empfänger das Paket nicht fragmentieren dürfen. Es gibt viele Gründe, warum dieses Bit festgelegt werden kann (ein Beispiel finden Sie im Abschnitt „Path Discovery“). Wenn eine Netzwerkgerät ein Paket mit dem „Don't Fragment“-Bit empfängt und dieses Paket die MTU der Geräteschnittstelle überschreitet, dann ist das Standardverhalten, dass das Gerät das Paket verwirft und ein „ICMP Fragmentation Needed“-Paket zurück an die ursprüngliche Quelle des Pakets sendet.

#### <a name="performance-implications-of-fragmentation"></a>Leistungsauswirkungen der Fragmentierung

Eine Fragmentierung kann sich negativ auf die Leistung auswirken. Einer der Hauptgründe für die Auswirkungen auf die Leistung ist die Auswirkung der Fragmentierung auf den CPU-Speicher und die erneute Zusammensetzung von Paketen. Wenn ein Netzwerkgerät ein Paket fragmentieren muss, muss es CPU-Speicherressourcen zuweisen, um eine Fragmentierung durchzuführen. Dasselbe muss passieren, wenn das Paket wieder zusammengesetzt wird. Das Netzwerkgerät muss alle Fragmente bis zum Empfang speichern, damit es sie wieder zum ursprünglichen Paket zusammensetzen kann. Dieser Prozess der Fragmentierung/Neuzusammensetzung kann auch zu Latenzzeiten aufgrund des Fragmentierungs-/Neuzusammensetzungsprozesses führen.

Die andere mögliche negative Auswirkung der Fragmentierung auf die Leistung ist, dass fragmentierte Pakete möglicherweise nicht mehr in der richtigen Reihenfolge ankommen. Diese Pakete können dazu führen, dass sie von bestimmten Netzwerkgeräten verworfen werden – was dann eine erneute Übertragung des gesamten Pakets erfordert. Typische Szenarien für das Verwerfen von Fragmenten sind Sicherheitsvorrichtungen wie Netzwerkfirewalls oder das die Empfangspuffer eines Netzwerkgeräts erschöpft sind. Wenn die Empfangspuffer eines Netzwerkgeräte erschöpft sind, versucht eine Netzwerkgerät, ein fragmentiertes Paket wieder zusammenzusetzen, verfügt aber nicht über die Ressourcen, um das Paket zu speichern und wieder aufzunehmen.

Fragmentierung kann als negativer Vorgang wahrgenommen werden, aber die Fragmentierung muss unterstützt werden, um verschiedene Netzwerke über das Internet zu verbinden.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Vorteile und Konsequenzen einer Änderung der MTU

Allgemein ausgedrückt kann eine Erhöhung der MTU ein effizienteres Netzwerk schaffen. Jedes übertragene Paket hat zusätzliche Headerinformationen, die dem ursprünglichen Paket hinzugefügt werden. Mehr Pakete bedeuten mehr Header-Overhead, und das Netzwerk ist dadurch weniger effizient.

So beträgt beispielsweise die Ethernet-Headergröße 14 Bytes plus eine 4-Byte-Blockprüfzeichenfolge (Frame Check Sequence, FCS), um die Konsistenz des Frames sicherzustellen. Wenn ein 2000-Byte-Paket gesendet wird, werden 18 Bytes an Ethernet-Overhead im Netzwerk hinzugefügt. Wenn das Paket in ein 1.500-Byte-Paket und ein 500-Byte-Paket zerlegt wird, dann hat jedes Paket einen Ethernet-Header von 18 Bytes – oder 36 Bytes. Ein einzelnes 2000-Byte-Paket hätte dagegen nur einen Ethernet-Header von 18 Bytes.

Es ist zu beachten, dass eine Erhöhung der MTU an sich nicht zwangsläufig zu einem effizienteren Netzwerk führt. Wenn eine Anwendung nur 500-Byte-Pakete sendet, dann entsteht der gleiche Header-Overhead, unabhängig davon, ob die MTU 1.500 Byte oder 9.000 Byte beträgt. Damit das Netzwerk effizienter wird, muss es auch größere Paketgrößen verwenden, die im Verhältnis zur MTU stehen.

#### <a name="azure-and-vm-mtu"></a>Azure und VM-MTU

Die Standard-MTU für Azure-VMs ist 1.500 Bytes. Der Stapel des virtuellen Azure-Netzwerks versucht, ein Paket auf 1.400 Bytes zu fragmentieren. Der Stapel des virtuellen Azure-Netzwerks erlaubt jedoch Pakete bis zu 2006 Bytes, wenn das „Don't Fragment“-Bit im IP-Header festgelegt ist.

Es ist zu beachten, dass diese Fragmentierung nicht bedeutet, dass der Stapel des virtuellen Azure-Netzwerks von Natur aus ineffizient ist, da er Pakete mit 1.400 Bytes fragmentiert, während VMs eine MTU von 1.500 haben. Tatsächlich ist ein großer Prozentsatz der Netzwerkpakete viel kleiner als 1.400 Bytes oder 1.500 Bytes.

#### <a name="azure-and-fragmentation"></a>Azure und Fragmentierung

Der Stapel des virtuellen Azure-Netzwerks ist heute so konfiguriert, dass er „Out of Order Fragments“ – also fragmentierte Pakete, die nicht in ihrer ursprünglichen fragmentierten Reihenfolge ankommen – verwirft. Diese Pakete werden hauptsächlich aufgrund einer im November 2018 angekündigten Sicherheitslücke namens FragmentStack verworfen.

FragmentSmack ist ein Fehler in der Art und Weise, wie der Linux-Kernel die Neuzusammensetzung von fragmentierten IPv4- und IPv6-Paketen verarbeitet. Ein externer Angreifer könnte diesen Fehler nutzen, um teure Vorgänge zur Neuzusammenstellung von Fragmenten auszulösen, was zu einer Erhöhung der CPU und einem Denial-of-Service auf dem Zielsystem führt.

#### <a name="tune-the-mtu"></a>Optimieren der MTU

Azure VMs unterstützen wie jedes andere Betriebssystem eine konfigurierbare MTU. Die Fragmentierung, die innerhalb von Azure auftritt und oben beschrieben ist, sollte jedoch bei der Konfiguration der MTU berücksichtigt werden.

Azure empfiehlt Kunden nicht, die MTU für ihre VM zu erhöhen. Diese Erklärung erläutert im Detail, wie Azure heute die MTU implementiert und die Fragmentierung durchführt.

> [!IMPORTANT]
>Die Erhöhung der MTU hat keine Leistungssteigerung gezeigt und könnte sich negativ auf die Anwendungsleistung auswirken.
>
>

#### <a name="large-send-offload-lso"></a>Large Send Offload (LSO)

Large Send Offload (LSO) kann die Netzwerkleistung verbessern, indem es die Segmentierung der Pakete an den Ethernetadapter weiterleitet. Wenn LSO aktiviert ist, erstellt der TCP/IP-Stapel ein großes TCP-Paket und sendet es dann zur Segmentierung an den Ethernetadapter, bevor es weitergeleitet wird. Der Vorteil von LSO ist, dass die CPU nicht mehr Pakete in Paketgrößen segmentieren muss, die der MTU entsprechen, und diese Verarbeitung an die Ethernetschnittstelle auslagern kann, wo sie in Hardware durchgeführt wird. Weitere Informationen über die Vorteile von LSO finden Sie in der [Dokumentation zur Leistung im Microsoft-Netzwerkadapter](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

Wenn LSO aktiviert ist, können Azure-Kunden bei der Paketerfassung große Framegrößen sehen. Diese großen Framegrößen können einige Kunden zu der Annahme veranlassen, dass Fragmentierung oder eine Jumbo-MTU verwendet wird, wenn dies jedoch nicht der Fall ist. Mit LSO kann der Ethernetadapter dem TCP/IP-Stapel eine größere MSS anbieten, um ein größeres TCP-Paket zu erstellen. Dieser gesamte nicht segmentierte Frame wird dann an den Ethernetadapter weitergeleitet und wäre in einer Paketerfassung auf der VM sichtbar. Das Paket wird jedoch durch den Ethernetadapter gemäß der MTU des Ethernetadapters in viele kleinere Frames zerlegt.

### <a name="tcpmss-window-scaling-and-pmtud"></a>TCP-MSS-Fensterskalierung und PMTUD

#### <a name="explanation-of-tcp-mss"></a>Erläuterungen zu TCP MSS

TCP Maximum Segment Size (MSS) ist eine Einstellung, die dazu dient, die maximale TCP-Segmentgröße einzustellen, um eine Fragmentierung von TCP-Paketen zu vermeiden. Betriebssysteme legen die MSS typischerweise so fest: MSS = MTU - IP & TCP-Headergröße (je 20 Bytes oder 40 Bytes insgesamt). Eine Schnittstelle mit einer MTU von 1.500 hat also eine MSS von 1.460. Die MSS ist jedoch konfigurierbar.

Diese Einstellung wird im TCP-Drei-Wege-Handschlag vereinbart, wenn eine TCP-Sitzung zwischen Quelle und Ziel eingerichtet wird. Beide Seiten senden einen MSS-Wert und der niedrigere der beiden wird für die TCP-Verbindung verwendet.

Zwischengeschaltete Netzwerkgeräte, wie VPN-Gateways, wie Azure VPN Gateway, haben die Möglichkeit, die MTU unabhängig von Quelle und Ziel anzupassen, um eine optimale Netzwerkleistung zu gewährleisten. Daher ist zu beachten, dass die MTU von Quelle und Ziel allein nicht der einzige Faktor für den tatsächlichen MSS-Wert ist.

#### <a name="explanation-of-path-mtu-discovery-pmtud"></a>Erläuterungen zu Path MTU Discovery (PMTUD)

Während MSS ausgehandelt wird, kann es sein, dass die tatsächliche MSS, die als andere Netzwerkgeräte im Pfad zwischen Quelle und Ziel verwendet werden kann, keinen niedrigeren MTU-Wert hat als die Quelle und das Ziel. In diesem Fall wird das Gerät, dessen MTU kleiner als das Paket ist, das Paket verwerfen und eine „Fragementation Needed“-Nachricht (Typ 3, Code 4) vom Internet Control Message-Protokoll mit seiner MTU zurücksenden. Diese ICMP-Nachricht ermöglicht es dem Quellhost, seine Path-MTU entsprechend zu reduzieren. Der Prozess wird als „Path MTU Discovery“ bezeichnet.

Der Prozess von PMTUD ist grundsätzlich ineffizient und hat Auswirkungen auf die Netzwerkleistung. Wenn Pakete gesendet werden, die die MTU des Netzwerkpfads überschreiten, dann müssen diese Pakete mit einer niedrigeren MSS erneut übertragen werden. Wenn der Sender das ICMP-Paket „Fragmentation Needed“ nicht erhält, vielleicht aufgrund einer Netzwerkfirewall im Pfad (allgemein als PMTUD Blackhole bezeichnet), dann weiß der Sender nicht, dass er die MSS senken muss und wird das Paket kontinuierlich erneut senden. Aus diesem Grund wird nicht empfohlen, die MTU der Azure-VM zu erhöhen.

#### <a name="vpn-considerations-with-mtu"></a>VPN-Überlegungen mit MTU

Bei Kunden, die VMs verwenden, die eine Kapselung durchführen (z.B. IPSec VPNs), kann es zu zusätzlichen Auswirkungen auf die Paketgröße und die MTU kommen. VPNs erhöhen die Anzahl der Header, die dem ursprünglichen Paket hinzugefügt werden, wodurch die Paketgröße erhöht wird und eine geringere MSS erforderlich ist.

Die aktuelle Empfehlung für Azure lautet, TCP-MSS-Clamping auf 1.350 Bytes und die MTU der Tunnelschnittstelle auf 1.400 einzustellen. Weitere Informationen finden Sie auf der Seite [VPN-Geräte und IPSec/IKE-Parameter](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Latenz, Paketumlaufzeit und TCP-Fensterskalierung

#### <a name="latency-and-round-trip-time"></a>Latenz und Paketumlaufzeit

Die Netzwerklatenz wird durch die Lichtgeschwindigkeit über ein Glasfasernetz bestimmt. Die Realität ist, dass der Netzwerkdurchsatz von TCP durch die Paketumlaufzeit zwischen zwei Netzwerkgeräten auch effektiv geregelt wird (praktische Maxima).

| | | | |
|-|-|-|-|
|Weiterleiten|Distance|Unidirektionale Zeit|Paketumlaufzeit (RTT)|
|New York nach San Francisco|4.148 km|21 ms|42 ms|
|New York nach London|5.585 km|28 ms|56 ms|
|New York nach Sydney|15.993 km|80 ms|160 ms|

Diese Tabelle zeigt die Luftlinie zwischen zwei Standorten, jedoch ist die Entfernung in Netzwerken typischerweise länger als die Luftlinie. Eine einfache Formel zur Berechnung von MINIMUM RTT, die sich nach der Lichtgeschwindigkeit richtet, lautet: Minimum RTT = 2 * (Entfernungskilometer / Ausbreitungsgeschwindigkeit).

Für die Ausbreitungsgeschwindigkeit kann ein Standardwert von 200 verwendet werden – der Wert ist die Entfernung in Metern, die Licht in 1 Millisekunde zurücklegt.

Beispiel: Die Entfernung zwischen New York und San Francisco beträgt 4.148 Kilometer Luftlinie. Minimum RTT = 2 * (4.148/20). Die Ausgabe der Formel wird in Millisekunden angegeben.

Da die räumliche Entfernung zwischen zwei Standorten eine feste Realität ist, ist für maximale Netzwerkleistung die logischste Option, Ziele mit dem geringsten Entfernung zwischen ihnen auszuwählen. Zweitens können Entwurfsentscheidungen innerhalb des virtuellen Netzwerks getroffen werden, um den Pfad des Datenverkehrs zu optimieren und die Latenz zu reduzieren. Diese Überlegungen zum virtuellen Netzwerk werden im Abschnitt „Überlegungen zum Netzwerkentwurf“ unten beschrieben.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>Auswirkungen von Latenz und Paketumlaufzeit auf TCP

Die Paketumlaufzeit (PTT) hat direkte Auswirkung auf den maximalen TCP-Durchsatz. Das TCP-Protokoll verfügt über ein Konzept der Fenstergröße. Die Fenstergröße ist die maximale Datenmenge, die über eine TCP-Verbindung gesendet werden kann, bevor der Sender eine Bestätigung vom Empfänger erhalten muss. Wenn die TCP-MSS auf 1.160 und die TCP Window Size auf 65.535 eingestellt ist, dann kann der Sender 45 Pakete senden, bevor er eine Bestätigung vom Empfänger empfangen muss. Wenn die Bestätigung nicht empfangen wird, sendet der Sender erneut. In diesem Beispiel: TCP-Fenstergröße/TCP-MSS = gesendete Pakete. Oder 65535/1460 wird auf 45 aufgerundet.

Aber durch diesen Zustand "Warten auf Bestätigung" als Mechanismus zur Gewährleistung einer zuverlässigen Datenübermittlung hat die RTT effektiv Einfluss auf den TCP-Durchsatz. Je länger der Sender auf die Bestätigung wartet, desto länger muss er auch warten, bevor er weitere Daten sendet.

Die Formel zur Berechnung des maximalen Durchsatzes einer einzelnen TCP-Verbindung lautet wie folgt: Fenstergröße/(RTT-Latenz in Millisekunden/1000) = maximale Bytes pro Sekunde. Die folgende Tabelle ist zur besseren Lesbarkeit in Megabyte formatiert und zeigt den maximalen Durchsatz in Megabyte/Sekunde einer einzelnen TCP-Verbindung.

| | | | |
|-|-|-|-|
|TCP-Fenstergröße in Bytes|RTT-Latenz<br/>in Millisekunden|Maximum<br/>Durchsatz in Megabytes pro Sekunde|Maximum<br/> Durchsatz in Megabit pro Sekunde|
|65.535|1|65,54|524,29|
|65.535|30|2,18|17,48|
|65.535|60|1,09|8,74|
|65.535|90|0,73|5,83|
|65.535|120|0,55|4.37|

Wenn ein Paketverlust aufgetreten ist, dann reduziert dies den maximalen Durchsatz einer TCP-Verbindung, während der Sender Daten, die er bereits gesendet hat, erneut sendet.

#### <a name="explanation-of-tcp-window-scaling"></a>Erläuterungen zu TCP-Fensterskalierung

TCP-Fensterskalierung ist ein Konzept, das die TCP-Fenstergröße dynamisch erhöht, sodass mehr Daten gesendet werden können, bevor eine Bestätigung erforderlich ist. In unserem vorherigen Beispiel wurden 45 Pakete gesendet, bevor eine Bestätigung erforderlich war. Wenn die Anzahl der Pakete, die vor einer Bestätigung gesendet werden, erhöht wird, dann wird auch der maximale TCP-Durchsatz erhöht, indem reduziert wird, wie oft der Sender auf Bestätigung wartet.

Der TCP-Durchsatz wird in einer einfachen Tabelle weiter unten veranschaulicht:

| | | | |
|-|-|-|-|
|TCP-Fenstergröße<br/>in Bytes|RTT-Latenz in Millisekunden|Maximum<br/>Durchsatz in Megabytes pro Sekunde|Maximum<br/> Durchsatz in Megabit pro Sekunde|
|65.535|30|2,18|17,48|
|131.070|30|4.37|34,95|
|262.140|30|8,74|69,91|
|524.280|30|17,48|139,81|

Der TCP-Headerwert für die TCP-Fenstergröße ist jedoch nur 2 Bytes lang, was bedeutet, dass der Maximalwert für ein Empfangsfenster 65.535 beträgt. Um die maximale Fenstergröße zu erhöhen, wurde ein TCP-Fensterskalierungsfaktor eingeführt.

Der Skalierungsfaktor ist auch eine Einstellung, die in einem Betriebssystem konfiguriert werden kann. Die Formel zur Berechnung der TCP-Fenstergröße aus Skalierungsfaktoren lautet wie folgt: TCP-Fenstergröße = TCP-Fenstergröße in Bytes \* (2 ^ Skalierungsfaktor). Wenn der Fensterskalierungsfaktor 3 und die Fenstergröße 65.535 ist, wird wie folgt berechnet: 65535 \* (2^3) = 262,140 Bytes. Ein Skalierungsfaktor von 14 ergibt eine TCP-Fenstergröße von 14 (der maximal zulässige Versatz), dann beträgt die TCP-Fenstergröße 1.073.725.440 Bytes (8,5 Gigabit).

#### <a name="support-for-tcp-window-scaling"></a>Unterstützung für TCP-Fensterskalierung

Windows hat die Möglichkeit, verschiedene Skalierungsfaktoren für jeden Verbindungstyp festzulegen – es gibt mehrere Klassen von Verbindungen (Rechenzentrum, Internet usw.). Sie können die Verbindungsklassifizierung der Fensterskalierung mit dem Befehl „Get-NetTCPConnection powershell“ anzeigen.

```powershell
Get-NetTCPConnection
```

Sie können die Werte jeder Klasse mit dem Befehl „Get-NetTCPSetting powershell“ anzeigen.

```powershell
Get-NetTCPSetting
```

Die ursprüngliche TCP-Fenstergröße und der TCP-Skalierungsfaktor können in Windows über den Befehl „Set-NetTCPSetting powershell“ eingestellt werden. Weitere Informationen finden Sie unter auf der [Set-NetTCPSetting-Seite](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps).

```powershell
Set-NetTCPSetting
```

Die effektiven TCP-Einstellungen für AutoTuningLevel sind wie folgt.

| | | | |
|-|-|-|-|
|AutoTuningLevel|Skalierungsfaktor|Skalierungsmultiplikator|Formel zum<br/>Berechnen der maximalen Fenstergröße|
|Deaktiviert|Keine|Keine|Fenstergröße|
|Eingeschränkt|4|2^4|Fenstergröße * (2^4)|
|Stark eingeschränkt|2|2^2|Fenstergröße * (2^2)|
|Normal|8|2^8|Fenstergröße * (2^8)|
|Experimentell|14|2^14|Fenstergröße * (2^14)|

Während diese Einstellungen die TCP-Leistung am ehesten beeinflussen, sollte beachtet werden, dass viele andere Faktoren im Internet, die außerhalb der Kontrolle von Azure liegen, ebenfalls die TCP-Leistung beeinträchtigen können.

#### <a name="increase-mtu-size"></a>Erhöhen der MTU-Größe

Eine logische Frage ist: „Kann die Erhöhung der MTU die TCP-Leistung erhöhen, da eine größere MTU gleichbedeutend mit einer größeren MSS ist“? Die einfache Antwort lautet: wahrscheinlich nicht. Wie besprochen, gibt es Vor- und Nachteile bezüglich der Paketgröße, die über den reinen TCP-Datenverkehr hinaus anwendbar sind. Wie bereits erwähnt, sind die wichtigsten Faktoren, die die TCP-Durchsatzleistung beeinflussen, TCP-Fenstergröße, Paketverlust und RTT.

> [!IMPORTANT]
> Azure empfiehlt nicht, dass Azure-Kunden den MTU-Standardwert für virtuelle Computer ändern.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Beschleunigter Netzwerkbetrieb und empfangsseitige Skalierung

#### <a name="accelerated-networking"></a>Beschleunigter Netzwerkbetrieb

Netzwerkfunktionen des virtuellen Computers waren in der Vergangenheit sowohl beim VM-Gast als auch beim Hypervisor/Host CPU-intensiv. Jedes Paket, das durch den Host übertragen wird, wird von der Host-CPU in Software verarbeitet – einschließlich der gesamten Kapselung und Entkapselung des virtuellen Netzwerks. Also, je mehr Datenverkehr durch den Host geleitet wird, desto höher ist die CPU-Last. Und wenn die Host-CPU damit beschäftigt ist, andere Vorgänge auszuführen, dann hat das auch Auswirkungen auf den Netzwerkdurchsatz und die Latenz. Dieses Problem wurde durch einen beschleunigten Netzwerkbetrieb gelöst.

Der beschleunigte Netzwerkbetrieb bietet eine konsistente, extrem niedrige Netzwerklatenz über Azure's hauseigene programmierbare Hardware und Technologien wie SR-IOV. Durch das Verschieben eines Großteils der von Azure softwaredefinierten Netzwerkstapel von den CPUs in FPGA-basierte SmartNICs werden Rechenzyklen von Endbenutzeranwendungen zurückgewonnen, wodurch die VM weniger belastet, Jitter und Inkonsistenzen in der Latenz verringert werden. Mit anderen Worten, die Leistung kann deterministischer sein.

Der beschleunigte Netzwerkbetrieb erzielt Leistungssteigerungen, indem er es der Gast-VM ermöglicht, den Host zu umgehen und einen Datenpfad direkt mit dem SmartNIC eines Hosts einzurichten. Die Vorteile des beschleunigten Netzwerkbetriebs sind:

- **Niedrigere Latenz/mehr Pakete pro Sekunde (pps)**: Durch das Entfernen des virtuellen Switch aus dem Datenpfad wird die Zeit reduziert, die Pakete auf dem Host für die Verarbeitung von Richtlinien warten müssen. Gleichzeitig wird die Anzahl von Paketen erhöht, die im virtuellen Computer verarbeitet werden können.

- **Reduzierte Jitter**: Die Verarbeitung im virtuellen Switch hängt vom Umfang der anzuwendenden Richtlinien und von der Workload der CPU ab, die die Verarbeitung durchführt. Das Auslagern der Richtlinienerzwingung auf die Hardware entfernt diesen Umweg, indem die Pakete direkt an den virtuellen Computer gesendet werden. Damit werden der Host und alle Softwareinterrupts und Kontextwechsel aus der VM-Kommunikation entfernt.

- **Verringerte CPU-Auslastung**: Das Umgehen des virtuellen Switch auf dem Host führt zu weniger CPU-Auslastung für die Verarbeitung des Netzwerkdatenverkehrs.

Der beschleunigte Netzwerkbetrieb muss für jede VM explizit aktiviert werden. Anweisungen zum Aktivieren des beschleunigten Netzwerkbetriebs auf einer VM finden Sie auf der Seite [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

#### <a name="receive-side-scaling-rss"></a>Empfangsseitige Skalierung (RSS)

Die empfangsseitige Skalierung ist eine Netzwerktreibertechnologie, die den Empfang von Netzwerkverkehr effizienter verteilt, indem sie die Empfangsverarbeitung auf mehrere CPUs in einem Mehrprozessorsystem verteilt. Einfach ausgedrückt ermöglicht es RSS einem System, eine größere Menge an empfangenem Datenverkehr zu verarbeiten, da es alle verfügbaren CPUs anstelle von nur einer verwendet. Eine weitere technische Erläuterung zu RSS finden Sie auf der Seite [Einführung zur empfangsseitigen Skalierung](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

RSS ist erforderlich, um maximale Leistung zu erzielen, wenn der beschleunigte Netzwerkbetrieb auf einer VM aktiviert ist. Es kann auch Vorteile bei der Verwendung von RSS auf VMs geben, bei denen der beschleunigte Netzwerkbetrieb nicht aktiviert ist. Eine Übersicht darüber, wie Sie feststellen können, ob RSS aktiviert ist, und eine Konfiguration zum Aktivieren finden Sie auf der Seite [Optimieren des Netzwerkdurchsatzes für virtuelle Azure-Computer](http://aka.ms/FastVM).

### <a name="tcp-time-wait-and-time-wait-assassination"></a>TCP-Wartezeit und Time Wait Assassination

Ein weiteres häufiges Problem, das die Netzwerk- und Anwendungsleistung beeinträchtigt, ist die Einstellung der TCP-Wartezeit. Bei ausgelasteten VMs, die viele Sockel öffnen und schließen, entweder als Client oder Server (Quell-IP: Quellport + Ziel-IP: Zielport), kann ein bestimmter Sockel während des normalen Betriebs von TCP für eine längere Zeit in einem Wartezustand übergehen. Dieser Zustand soll es ermöglichen, dass zusätzliche Daten an einen Sockel geliefert werden, bevor er geschlossen wird. Daher verhindern TCP/IP-Stapels im Allgemeinen die Wiederverwendung eines Sockeln, indem sie das TCP-SYN-Paket des Clients im Hintergrund ablegen.

Diese Zeitspanne, die sich ein Sockel im Wartezustand befindet, ist konfigurierbar, sollte aber zwischen 30 Sekunden und 240 Sekunden liegen. Sockel sind eine endliche Ressource, und die Anzahl der Sockel, die zu einem bestimmten Zeitpunkt verwendet werden können, ist konfigurierbar (die Anzahl liegt in der Regel bei etwa 30.000 potenziellen Sockeln). Wenn diese Anzahl erschöpft ist, oder die Wartezeiteinstellungen für Clients und Server nicht übereinstimmt und eine VM versucht, einen Sockel im Wartezustand wiederzuverwenden, schlagen neue Verbindungen fehl, da TCP-SYN-Pakete im Hintergrund gelöscht werden.

Normalerweise sind der Wert für den Portbereich für ausgehende Sockel sowie die Einstellungen für die TCP-Wartezeit und die Wiederverwendung von Sockeln innerhalb des TCP/IP-Stapels eines Betriebssystems konfigurierbar. Die Änderung dieser Werte kann die Skalierbarkeit potenziell verbessern, könnte aber je nach Szenario zu Interoperabilitätsproblemen führen. Daher sollte die Änderung mit Vorsicht vorgenommen werden.

Eine Funktion namens Time Wait Assassination wurde eingeführt, um diese Skalierungsbeschränkung zu umgehen. Time Wait Assassination ermöglicht die Wiederverwendung eines Sockets unter bestimmten Bedingungen, z.B. wenn die Sequenznummer im IP-Paket der neuen Verbindung die Sequenznummer des letzten Pakets der vorherigen Verbindung überschreitet. In diesem Fall ermöglicht das Betriebssystem den Aufbau der neuen Verbindung (Annahme des neuen SYN ACK) und erzwingt das Schließen der vorherigen Verbindung, die sich im Wartezustand befand. Diese Funktion wird heute auf Windows-VMs innerhalb von Azure unterstützt und die Unterstützung innerhalb anderer VMs sollte von Azure-Kunden mit den jeweiligen Betriebssystemanbietern überprüft werden.

Dokumentation zur Konfiguration der Einstellung für die TCP-Wartezeit und des Quellportbereichs finden Sie auf der Seite [Einstellungen, die zur Verbesserung der Netzwerkleistung geändert werden können](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Faktoren im virtuellen Netzwerk, die sich auf die Leistung auswirken können

### <a name="vm-maximum-outbound-throughput"></a>Maximaler ausgehender VM-Durchsatz

Azure bietet eine Vielzahl von VM-Größen und -Typen mit einer unterschiedlichen Kombination von Leistungsaspekten. Zu diesen Leistungsaspekten gehört der Netzwerkdurchsatz (oder Bandbreite), der in Megabit pro Sekunde (MBit/s) gemessen wird. Da virtuelle Computer auf gemeinsam genutzter Hardware gehostet werden, muss die Netzwerkkapazität zwischen den virtuellen Computern, die dieselbe Hardware verwenden, gerecht aufgeteilt werden. Größeren virtuellen Computern wird verhältnismäßig mehr Bandbreite als kleineren virtuellen Computern zugeordnet.

Die Netzwerkbandbreite, die dem jeweiligen virtuellen Computer zugeordnet ist, wird anhand des ausgehenden Datenverkehrs vom virtuellen Computer gemessen. Der gesamte Netzwerkdatenverkehr, der vom virtuellen Computer ausgeht, wird unabhängig vom Ziel auf den zugewiesenen Grenzwert angerechnet. Wenn der Grenzwert eines virtuellen Computers beispielsweise bei 1.000 MBit/s liegt, gilt dieser Grenzwert unabhängig davon, ob das Ziel des ausgehenden Datenverkehrs ein anderer virtueller Computer in demselben virtuellen Netzwerk ist oder ob es sich außerhalb von Azure befindet.
Der eingehende Datenverkehr wird nicht gemessen oder direkt beschränkt. Es gibt jedoch andere Faktoren, wie z. B. CPU- und Speichergrenzwerte, die sich auf die Fähigkeit eines virtuellen Computers zur Verarbeitung eingehender Daten auswirken können.

Der beschleunigte Netzwerkbetrieb ist ein Feature, das der Verbesserung der Netzwerkleistung, einschließlich Latenz, Durchsatz und CPU-Auslastung, dient. Zwar kann durch den beschleunigten Netzwerkbetrieb der Durchsatz eines virtuellen Computers verbessert werden, doch ist dies nur im Rahmen der Bandbreite möglich, die dem virtuellen Computer zugewiesen ist.

Virtuellen Azure-Computern muss eine Netzwerkschnittstelle angefügt sein, doch können es auch mehrere sein. Die einem virtuellen Computer zugewiesene Bandbreite umfasst die Summe des gesamten ausgehenden Datenverkehrs aller Netzwerkschnittstellen, die an einen virtuellen Computer angefügt sind. Anders ausgedrückt: Die zugewiesene Bandbreite gilt pro virtuellem Computer, unabhängig davon, wie viele Netzwerkschnittstellen an den virtuellen Computer angefügt sind.
 
Der erwartete ausgehende Durchsatz und die Anzahl der Netzwerkschnittstellen, die von der jeweiligen Größe des virtuellen Computers unterstützt werden, ist hier angegeben. Um den maximalen Durchsatz anzuzeigen, wählen Sie einen Typ aus (z. B. „Allgemeiner Zweck“), und wählen Sie dann auf der daraufhin angezeigten Seite eine Größenserie aus (z. B. die Dv2-Serie). Jede Serie weist eine Tabelle mit Netzwerkspezifikationen in der letzten Spalte mit dem Titel „Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbps)“ auf.

Die Durchsatzbegrenzung gilt für den virtuellen Computer. Der Durchsatz wird durch folgende Faktoren nicht beeinflusst:

- **Anzahl der Netzwerkschnittstellen**: Der Bandbreitengrenzwert ist ein kumulativer Wert des gesamten ausgehenden Datenverkehrs vom virtuellen Computer.

- **Beschleunigter Netzwerkbetrieb**: Obwohl das Feature beim Erreichen des veröffentlichten Grenzwerts hilfreich sein kann, ändert sich dadurch der Grenzwert nicht.

- **Datenverkehrsziel**: Alle Ziele werden auf den Grenzwert für ausgehenden Datenverkehr angerechnet.

- **Protokoll:** Der gesamte ausgehende Datenverkehr über alle Protokolle wird auf den Grenzwert angerechnet.

Eine [Tabelle der maximalen Bandbreite pro VM-Typ finden Sie, indem Sie diese Seite](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) besuchen und auf den jeweiligen VM-Typ klicken. Auf jeder Seite zeigt eine Tabelle die maximalen NICs und die maximal erwartete Netzwerkbandbreite an.

Weitere Informationen zur VM-Netzwerkbandbreite finden Sie unter [Netzwerkbandbreite des virtuellen Computers](http://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Überlegungen zur Internetleistung

Wie in diesem Artikel erläutert, können Faktoren im Internet und außerhalb der Kontrolle von Azure die Netzwerkleistung beeinträchtigen. Diese Faktoren sind:

- **Latenz**: Die Paketumlaufzeit zwischen zwei Zielen kann durch Probleme in Zwischennetzwerken, Datenverkehr, der nicht den „kürzesten“ Weg nimmt, und suboptimale Peering-Pfade beeinflusst werden.

- **Paketverlust**: Paketverluste können durch Netzwerküberlastung, Probleme mit dem physischen Pfad und leistungsschwache Netzwerkgeräte verursacht werden.

- **MTU-Größe/Fragmentierung**: Fragmentierung entlang des Pfades kann zu Verzögerungen bei der Datenankunft oder zu fehlerhaften Paketen führen, was die Übermittlung von Paketen beeinträchtigen kann.

Traceroute ist ein gutes Tool, um die Leistungsmerkmale des Netzwerks (wie Paketverlust und Latenz) entlang jedes Netzwerkpfades zwischen einem Quell- und einem Zielgerät zu messen.

### <a name="network-design-considerations"></a>Überlegungen zum Netzwerkentwurf

Neben den oben genannten Überlegungen kann die Topologie eines virtuellen Netzwerks dessen beeinflussen. Zum Beispiel ein Hub-and-Spoke-Entwurf, der den Datenverkehr global zu einem einzelnen virtuellen Hub zurückleitet, führt zu einer Netzwerklatenz und damit zu einer Beeinträchtigung der gesamten Netzwerkleistung. Ebenso kann die Anzahl der Netzwerkgeräte, die der Netzwerkdatenverkehr durchläuft, auch die Gesamtlatenz beeinflussen. Wenn beispielsweise bei einem Hub-and-Spoke-Entwurf der Datenverkehr über ein virtuelles Spoke-Netzwerkgerät und ein virtuelles Hub-Gerät läuft, bevor er ins Internet gelangt, dann kann die Latenzzeit durch das virtuelle Netzwerkgerät eingeleitet werden.

### <a name="azure-regions-virtual-networks-and-latency"></a>Azure-Regionen, virtuelle Netzwerke und Latenz

Azure-Regionen bestehen aus mehreren Rechenzentren innerhalb eines allgemeinen geografischen Gebiets. Diese Rechenzentren sind möglicherweise nicht physisch nebeneinander angeordnet und können in einigen Fällen bis zu 10 Kilometer voneinander entfernt sein. Das virtuelle Netzwerk ist eine logische Überlagerung des physischen Rechenzentrumsnetzwerks von Azure, und ein virtuelles Netzwerk impliziert keine spezifische Netzwerktopologie innerhalb des Rechenzentrums. So befinden sich beispielsweise VM A und VM B im gleichen virtuellen Netzwerk und Subnetz, können sich aber in verschiedenen Racks, Reihen oder sogar Rechenzentren befinden. Sie können mehrere Kilometer von Glasfaserkabeln getrennt sein. Diese Tatsache kann zu variablen Latenzen (wenige Millisekunden Differenz) zwischen verschiedenen VMs führen.

Diese geografische Platzierung und damit die Latenz zwischen zwei VMs kann durch die Konfiguration von Verfügbarkeitsgruppen und Verfügbarkeitszonen beeinflusst werden, jedoch ist der Abstand zwischen den Rechenzentren in einer Region regionsspezifisch und wird überwiegend durch die Topologie des Rechenzentrums in der Region bestimmt.

### <a name="source-nat-port-exhaustion"></a>Quell-NAT-Portauslastung

Eine Bereitstellung in Azure kann mit Endpunkten außerhalb von Azure im öffentlichen Internet und/oder öffentlichen IP-Adressraum kommunizieren. Wenn eine Instanz eine ausgehende Verbindung initiiert, ordnet Azure die private IP-Adresse dynamisch einer öffentlichen IP-Adresse zu. Nachdem diese Zuordnung erstellt wurde, kann der Antwortdatenverkehr für diesen ursprünglich ausgehenden Datenfluss auch die private IP-Adresse erreichen, von welcher der Datenfluss stammt.

Für jede ausgehende Verbindung muss der Azure Load Balancer diese Zuordnung für längere Zeit beibehalten. Aufgrund der Mehrmandantenfähigkeit von Azure kann die Aufrechterhaltung dieses Zuordnungen für jeden ausgehenden Datenverkehr für jede VM sehr ressourcenintensiv sein. Daher gibt es Einschränkungen, die auf der Grundlage der Konfiguration des virtuellen Azure-Netzwerks festgelegt werden. Oder genauer gesagt – eine Azure-VM kann jeweils nur eine bestimmte Anzahl von ausgehenden Verbindungen aufbauen. Wenn diese Grenzwerte erreicht sind, wird die Azure-VM daran gehindert, weitere ausgehende Verbindungen herzustellen.

Dieses Verhalten ist jedoch konfigurierbar. Weitere Informationen zu [SNAT- und SNAT-Portauslastung], finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>Messen der Netzwerkleistung in Azure

Eine Reihe der Leistungshöchstwerten in diesem Artikel beziehen sich auf die Netzwerklatenz/Paketumlaufzeit (RTT) zwischen zwei VMs. Dieser Abschnitt enthält einige Vorschläge zum Testen von Latenz/RTT sowie von TCP-Leistung und VM-Netzwerkleistung. Die oben beschriebenen TCP/IP- und Netzwerkwerte können mit den unten beschriebenen Techniken angepasst und auf ihre Leistung getestet werden. Die Werte für Latenz, MTU, MSS und Fenstergröße können in den oben genannten Berechnungen verwendet werden, und theoretische Höchstwerte können mit den während der Tests beobachteten tatsächlichen Werten verglichen werden.

### <a name="measure-round-trip-time-and-packet-loss"></a>Messen von Paketumlaufzeit (RTT) und Paketverlust

Die TCP-Leistung basiert stark auf RTT und Paketverlust. Der einfachste Weg, RTT und Paketverlust zu messen, ist die Verwendung des in Windows und Linux verfügbaren Ping-Hilfsprogramms. Die Ausgabe von ping zeigt die minimale/maximale/durchschnittliche Latenzzeit zwischen Quelle und Ziel sowie den Paketverlust an. Ping verwendet standardmäßig das ICMP-Protokoll. Um TCP-RTT zu testen, kann PsPing verwendet werden. Weitere Informationen zu PsPing finden Sie unter [diesem Link](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>Messen des tatsächlichen Durchsatzes für eine TCP-Verbindung

NTttcp ist ein Tool, mit dem die TCP-Leistung einer Linux- oder Windows-VM getestet werden kann. Mit NTttcp können verschiedene TCP-Einstellungen optimiert und die Vorteile getestet werden. Weitere Informationen zu NTttcp finden Sie unter den folgenden Links.

- [Testen der Bandbreite/des Durchsatzes (NTTTCP)](https://aka.ms/TestNetworkThroughput)

- [NTttcp-Hilfsprogramm](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Messen der tatsächlichen Bandbreite eines virtuellen Computers

Leistungstests verschiedener VM-Typen, beschleunigter Netzwerkbetrieb usw. können mit einem Tool namens Iperf getestet werden, das auch unter Linux und Windows verfügbar ist. Iperf kann mit TCP oder UDP den gesamten Netzwerkdurchsatz testen. TCP-Durchsatztests mit Iperf werden von den in diesem Artikel beschriebenen Faktoren beeinflusst (Latenz, RTT usw.). Daher kann UDP bei der einfachen Prüfung des maximalen Durchsatzes zu besseren Ergebnissen führen.

Weitere Informationen finden Sie unten:

- [Beheben von Problemen bei der Expressroute-Netzwerkleistung](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [Überprüfen des VPN-Durchsatzes zu einem virtuellen Netzwerk](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Erkennen von ineffizientem TCP-Verhalten

Azure-Kunden sehen möglicherweise TCP-Pakete mit TCP-Flags (SACK, DUP ACK, RETRANSMIT und FAST RETRANSMIT) in Paketerfassungen, die auf Probleme mit der Netzwerkleistung hinweisen können. Diese Pakete weisen speziell auf Netzwerkineffizienzen als Folge von Paketverlust hin. Grund für einen Paketverlust ist jedoch nicht unbedingt ein Problem mit der Azure-Leistung. Leistungsprobleme können auch auf Problemen bei Anwendungen oder Betriebssystem zurückzuführen sein oder durch andere Probleme verursacht werden, die nicht direkt mit der Azure-Plattform zusammenhängen. Es ist auch zu beachten, dass einige Neuübertragungen oder doppelte ACKs in einem Netzwerk normal sind – TCP-Protokolle wurden entwickelt, um zuverlässig zu sein. Und das Vorhandensein dieser TCP-Pakete in einer Paketerfassung deutet nicht unbedingt auf ein systemisches Netzwerkproblem hin, es sei denn, die Anzahl ist sehr hoch.

Es sollte jedoch klargestellt werden, dass diese Pakettypen Hinweise darauf sind, dass der TCP-Durchsatz nicht seine maximale Leistung erreicht. Die Gründe dafür werden in anderen Abschnitten erläutert.

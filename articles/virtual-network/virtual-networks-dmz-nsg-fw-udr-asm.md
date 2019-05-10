---
title: Umkreisnetzwerk-Beispiel – Schützen von Netzwerken mit einem Umkreisnetzwerk, das aus einer Firewall, UDR und NSGs besteht | Microsoft-Dokumentation
description: Erstellen eines Umkreisnetzwerks (auch als DMZ bezeichnet) mit einer Firewall, benutzerdefiniertem Routing (User Defined Routing, UDR) und Netzwerksicherheitsgruppen (Network Security Groups, NSGs)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 0a7927868a9a4bebc80ec995baefbae4c45d747f
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410471"
---
# <a name="example-3-build-a-perimeter-network-to-protect-networks-with-a-firewall-udr-and-nsgs"></a>Beispiel 3: Erstellen eines Umkreisnetzwerks zum Schutz von Netzwerken mit einer Firewall, UDR und NSGs

[Zurück zur Seite mit Best Practices zu Sicherheitsgrenzen][HOME]

In diesem Beispiel erstellen Sie ein Umkreisnetzwerk (auch als DMZ, demilitarisierte Zone, und überwachtes Subnetz bezeichnet). In diesem Beispiel werden eine Firewall, vier Windows-Server, benutzerdefiniertes Routing (User Defined Routing, UDR), IP-Weiterleitung und Netzwerksicherheitsgruppen (NSGs) implementiert. In diesem Artikel wird jeder der relevanten Befehle genau erläutert, um ein besseres Verständnis jedes einzelnen Schritts zu ermöglichen. Im Abschnitt „Datenverkehrsszenarien“ ist außerdem ausführlich beschrieben, wie Datenverkehr die Sicherheitsstufen im Umkreisnetzwerk durchläuft. Der Abschnitt „Referenzen“ schließlich enthält den gesamten Code sowie Anweisungen zum Aufbau dieser Umgebung, sodass Sie verschiedene Szenarien testen und ausprobieren können.

![Bidirektionale Umkreisnetzwerkverbindungen mit NVA, NSG und UDR][1]

## <a name="environment-setup"></a>Einrichten der Umgebung

In diesem Beispiel wird ein Abonnement verwendet, das die folgenden Komponenten enthält:

* Drei Clouddienste: SecSvc001, FrontEnd001 und BackEnd001
* Ein virtuelles Netzwerk (CorpNetwork) mit drei Subnetzen: SecNet, FrontEnd und BackEnd
* Ein virtuelles Netzwerkgerät: eine Firewall, die mit dem Subnetz „SecNet“ verbunden ist
* Ein Windows-Server, der einen Anwendungswebserver darstellt: IIS01
* Zwei Windows-Server, die Anwendungs-Back-End-Server darstellen: AppVM01, AppVM02
* Ein Windows-Server, der einen DNS-Server darstellt: DNS01

Der [Abschnitt „Referenzen“](#references) enthält ein PowerShell-Skript, mit dem der größte Teil der hier beschriebenen Umgebung erstellt wird. Dieser Artikel enthält ansonsten keine ausführlichen Anweisungen zum Erstellen der virtuellen Computer und virtuellen Netzwerke.

So erstellen Sie die Umgebung:

1. Speichern Sie die Netzwerkkonfigurations-XML-Datei, die im [Abschnitt „Referenzen“](#references) enthalten ist. Sie müssen diese Datei mit Namen, Speicherort und IP-Adressen aktualisieren, damit sie dem vorliegenden Szenario entspricht.
1. Aktualisieren Sie die Benutzervariablen im vollständigen Skript entsprechend Ihrer speziellen Umgebung (z. B. Abonnements, Dienstnamen usw.).
1. Führen Sie das Skript in PowerShell aus.

> [!NOTE]
> Die im PowerShell-Skript angegebene Region muss mit der Region übereinstimmen, die in der Netzwerkkonfigurations-XML-Datei angegeben ist.

Nachdem das Skript erfolgreich ausgeführt wurde, gehen Sie folgendermaßen vor:

1. Einrichten der Firewallregeln. Informationen hierzu finden Sie im Abschnitt [Firewallregeln](#firewall-rules).
1. Verwenden Sie optional die beiden Skripts im Abschnitt „Referenzen“, um eine Webanwendung auf dem Webserver und dem Anwendungsserver einzurichten, damit diese DMZ-Konfiguration getestet werden kann.

## <a name="user-defined-routing"></a>Benutzerdefiniertes Routing

Standardmäßig sind folgende Systemrouten definiert:

    Effective routes :
     Address Prefix    Next hop type    Next hop IP address Status   Source
     --------------    -------------    ------------------- ------   ------
     {10.0.0.0/16}     VNETLocal                            Active   Default
     {0.0.0.0/0}       internet                             Active   Default
     {10.0.0.0/8}      Null                                 Active   Default
     {100.64.0.0/10}   Null                                 Active   Default
     {172.16.0.0/12}   Null                                 Active   Default
     {192.168.0.0/16}  Null                                 Active   Default

VNETLocal ist immer das definierte Adresspräfix für dieses spezielle virtuelle Netzwerk. Dies ändert sich z. B. von virtuellem Netzwerk zu virtuellem Netzwerk, je nachdem, wie jedes spezielle virtuelle Netzwerk definiert ist. Die restlichen Systemrouten sind statisch und standardmäßig wie hier angegeben.

Hinsichtlich Priorität werden die Routen gemäß der LPM-Methode (Longest Prefix Match, längste Präfixübereinstimmung) verarbeitet. Daher gilt die speziellste Route in der Tabelle für eine bestimmte Zieladresse.

Aus diesem Grund wird Datenverkehr, der für einen Server wie DNS01 (10.0.2.4) im lokalen Netzwerk (10.0.0.0/16) vorgesehen ist, wegen der 10.0.0.0/16-Route über das virtuelle Netzwerk weitergeleitet.  Für 10.0.2.4 ist die 10.0.0.0/16-Route die speziellste Route. Diese Regel gilt, obwohl 10.0.0.0/8 und 0.0.0.0/0 ebenfalls anwendbar sein können. Sie sind jedoch weniger speziell, sodass sie sich nicht auf diesen Datenverkehr auswirken. Datenverkehr an 10.0.2.4 hat das lokale virtuelle Netzwerk als seinen nächsten Hop, wird also an das Ziel weitergeleitet.

Die 10.0.0.0/16-Route gilt z. B. nicht für Datenverkehr, der für 10.1.1.1 vorgesehen ist. Die 10.0.0.0/8-Systemroute ist die speziellste, somit wird der Datenverkehr verworfen oder „blockiert“, weil der nächste Hop gleich „Null“ ist.

Trifft das Ziel für keines der „Null“-Präfixe oder „VNETLocal“-Präfixe zu, folgt der Datenverkehr der am wenigsten speziellen Route (0.0.0.0/0). Er wird an das Internet als dem nächsten Hop und durch die Internetgrenze von Azure hindurch weitergeleitet.

Wenn die Routingtabelle zwei identische Präfixe enthält, ergibt sich die Präferenzreihenfolge aus dem Routenattribut „Source“:

1. VirtualAppliance: Eine benutzerdefinierte Route, die der Tabelle manuell hinzugefügt wurde.
1. VPNGateway: Eine dynamische Route (BGP bei Verwendung mit Hybridnetzwerken), die durch ein dynamisches Netzwerkprotokoll hinzugefügt wurde. Diese Routen können sich im Laufe der Zeit ändern, weil das dynamische Protokoll Änderungen im Peernetzwerk automatisch berücksichtigt.
1. Standardwert: Die Systemrouten, das lokale virtuelle Netzwerk und die statischen Einträge, wie in der Routingtabelle oben gezeigt.

> [!NOTE]
> Jetzt können Sie benutzerdefiniertes Routing (UDR) mit ExpressRoute und VPN-Gateways verwenden, damit ausgehender und eingehender standortübergreifender Datenverkehr an ein virtuelles Netzwerkgerät (NVA) weitergeleitet wird.

### <a name="create-local-routes"></a>Erstellen von lokalen Routen

In diesem Beispiel werden zwei Routingtabellen verwendet, jeweils eine für das Front-End- und das Back-End-Subnetz. Jede Tabelle enthält statische Routen, die sich für das jeweilige Subnetz eignen. In diesem Beispiel weist jede Tabelle drei Routen auf:

1. Lokaler Subnetzdatenverkehr ohne definierten nächsten Hop. Diese Route lässt es zu, dass lokaler Subnetzdatenverkehr an der Firewall vorbei weitergeleitet wird.
2. Virtueller Netzwerkdatenverkehr mit einem als Firewall definierten nächsten Hop. Diese Route setzt die Standardregel außer Kraft, die ein direktes Routing des lokalen virtuellen Netzwerkdatenverkehrs ermöglicht.
3. Der gesamte verbleibende Datenverkehr (0/0) mit einem als Firewall definierten nächsten Hop.

Nachdem die Routingtabellen erstellt wurden, sind sie an ihre Subnetze gebunden. Die Routingtabelle für das Front-End-Subnetz sollte so aussehen:

    Effective routes :
     Address Prefix    Next hop type       Next hop IP address  Status   Source
     --------------    ------------------  -------------------  ------   ------
     {10.0.1.0/24}     VNETLocal                                Active
     {10.0.0.0/16}     VirtualAppliance    10.0.0.4             Active
     {0.0.0.0/0}       VirtualAppliance    10.0.0.4             Active

In diesem Beispiel werden die folgenden Befehle verwendet, um die Routingtabelle zu erstellen, eine benutzerdefinierte Route hinzuzufügen und dann die Routingtabelle an ein Subnetz zu binden. Elemente, die mit `$` beginnen, etwa `$BESubnet`, sind benutzerdefinierte Variablen aus dem Skript im Abschnitt „Referenzen“.

1. Erstellen Sie zunächst die Basisroutingtabelle. Im folgenden Codeausschnitt wird die Tabelle für das Back-End-Subnetz erstellt. Im vollständigen Skript wird auch eine entsprechende Tabelle für das Front-End-Subnetz erstellt.

   ```powershell
   New-AzureRouteTable -Name $BERouteTableName `
       -Location $DeploymentLocation `
       -Label "Route table for $BESubnet subnet"
   ```

1. Nachdem Sie die Routingtabelle erstellt haben, können Sie bestimmte benutzerdefinierte Routen hinzufügen. Im folgenden Codeausschnitt ist angegeben, dass der gesamte Datenverkehr (0.0.0.0/0) über das virtuelle Gerät weitergeleitet wird. Die Variable `$VMIP[0]` wird verwendet, um die IP-Adresse zu übergeben, die zugewiesen wurde, als das virtuelle Gerät weiter oben im Skript erstellt wurde. Im vollständigen Skript wird auch eine entsprechende Regel für die Front-End-Tabelle erstellt.

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. Der vorherige Routeneintrag überschreibt die Standardroute „0.0.0.0/0“, aber die Standardroute „10.0.0.0/16“ lässt weiterhin zu, dass Datenverkehr im virtuellen Netzwerk direkt an das Ziel und nicht an das virtuelle Netzwerkgerät weitergeleitet wird. Um dieses Verhalten zu korrigieren, müssen Sie die folgende Regel hinzuzufügen:

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. An diesem Punkt müssen Sie eine Entscheidung treffen. Die beiden vorhergehenden Regeln leiten sämtlichen Datenverkehr an die Firewall zur Bewertung, so auch Datenverkehr innerhalb eines einzelnen Subnetzes. Es kann sei, dass Sie dieses Verhalten wünschen. Wenn nicht, können Sie jedoch zulassen, dass Datenverkehr in einem Subnetz lokal ohne Beteiligung der Firewall weitergeleitet wird. Fügen Sie eine dritte, spezielle Regel hinzu, in der jede Adresse, die für das lokale Subnetz bestimmt ist, direkt weiterleitet wird (NextHopType = VNETLocal).

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
           -NextHopType VNETLocal
   ```

1. Schließlich müssen Sie, nachdem Sie die Routingtabelle erstellt und mit benutzerdefinierten Routen aufgefüllt haben, die Tabelle an ein Subnetz binden. Im folgenden Codeausschnitt wird die Tabelle an das Back-End-Subnetz gebunden. Im vollständigen Skript wird auch die Front-End-Routingtabelle an das Front-End-Subnetz gebunden.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
       -SubnetName $BESubnet `
       -RouteTableName $BERouteTableName
   ```

## <a name="ip-forwarding"></a>IP-Weiterleitung

IP-Weiterleitung ist eine begleitende Funktion für das benutzerdefinierte Routing (UDR). Diese Einstellung für ein virtuelles Gerät ermöglicht, dass das Gerät Datenverkehr, der nicht an es adressierten ist, empfangen und an das endgültige Ziel weiterleiten kann.

Wenn beispielsweise Datenverkehr aus AppVM01 eine Anforderung an den Server DNS01 sendet, leitet UDR den Datenverkehr an die Firewall weiter. Wenn die IP-Weiterleitung aktiviert ist, wird Datenverkehr mit dem Ziel DNS01 (10.0.2.4) vom Firewallgerät (10.0.0.4) akzeptiert und dann an sein endgültige Ziel (10.0.2.4) weitergeleitet. Ist die IP-Weiterleitung in der Firewall nicht aktiviert, akzeptiert das Gerät keinen Datenverkehr, selbst wenn in der Routingtabelle die Firewall als nächster Hop angegeben ist.

> [!IMPORTANT]
> Denken Sie daran, die IP-Weiterleitung zusammen mit dem benutzerdefinierten Routing zu aktivieren.

Die IP-Weiterleitung kann bei der Erstellung eines virtuellen Computers mit einem einzigen Befehl aktiviert werden. Sie rufen Sie die VM-Instanz auf, die als Ihr virtuelles Firewallgerät fungiert, und aktivieren die IP-Weiterleitung. Beachten Sie, dass rot markierte Elemente, die mit `$` beginnen, z. B. `$VMName[0]`, benutzerdefinierte Variablen aus dem Skript im Abschnitt „Referenzen“ dieses Dokuments sind. Die 0 (null) in Klammern, `[0]`, stellt den ersten virtuellen Computer (VM) im Array der virtuellen Computer dar. Damit das Beispielskript ohne Änderung funktioniert, muss der erste virtuelle Computer (VM 0) die Firewall sein. Im vollständigen Skript ist der relevante Codeausschnitt mit den UDR-Befehlen in Nähe des Skriptendes gruppiert.

```powershell
Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
    Set-AzureIPForwarding -Enable
```

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

In diesem Beispiel erstellen Sie eine Netzwerksicherheitsgruppe (NSG) und laden diese anschließend mit einer einzigen Regel. Im Beispiel wird die NSG dann nur an das Front-End- und das Back-End-Subnetz (nicht an SecNet) gebunden. Die Regel, die Sie in die NSG laden, lautet wie folgt:

* Jeglicher Datenverkehr (alle Ports) aus dem Internet in das gesamte virtuelle Netzwerk (alle Subnetze) wird Abgelehnt.

Obwohl Netzwerksicherheitsgruppen in diesem Beispiel verwendet werden, dienen sie hauptsächlich als zweite Stufe zum Schutz gegen manuelle Fehlkonfigurationen. Sie möchten den gesamten eingehenden Datenverkehr aus dem Internet an das Front-End- oder das Back-End-Subnetz blockieren. Datenverkehr soll nur über das Subnetz „SecNet“ zur Firewall gelangen, nach der nur geeigneter Datenverkehr an das Front-End- oder Back-End-Subnetz weitergeleitet werden soll. Zusätzlich leiten die UDR-Regeln jeglichen Datenverkehr, der das Front-End- oder Back-End-Subnetz erreicht, an die Firewall weiter. Die Firewall erkennt diesen Datenverkehr als asymmetrischen Datenfluss und verwirft den ausgehenden Datenverkehr.

Drei Sicherheitsstufen schützen das Front-End- und das Back-End-Subnetz:

1. Keine geöffneten Endpunkte in den Clouddiensten „FrontEnd001“ und „BackEnd001“.
1. NSGs verweigern Datenverkehr aus dem Internet.
1. Die Firewall verwirft asymmetrischen Datenverkehr.

Ein interessanter Aspekt der Netzwerksicherheitsgruppe in diesem Beispiel ist, dass sie nur eine Regel (nachstehend aufgeführt) enthält. Diese Regel verweigert Internetdatenverkehr zum gesamten virtuellen Netzwerk, einschließlich des Sicherheitssubnetzes.

```powershell
Get-AzureNetworkSecurityGroup -Name $NSGName | `
    Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
    from the internet" `
    -Type Inbound -Priority 100 -Action Deny `
    -SourceAddressPrefix INTERNET -SourcePortRange '*' `
    -DestinationAddressPrefix VIRTUAL_NETWORK `
    -DestinationPortRange '*' `
    -Protocol *
```

Da die Netzwerksicherheitsgruppe nur an das Front-End- und Back-End-Subnetz gebunden ist, wird die Regel nicht auf eingehenden Datenverkehr angewendet, der das Sicherheitssubnetz als Ziel hat. Daher gelangt der Datenverkehr zum Sicherheitssubnetz.

```powershell
Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $FESubnet -VirtualNetworkName $VNetName

Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $BESubnet -VirtualNetworkName $VNetName
```

## <a name="firewall-rules"></a>Firewallregeln

Sie müssen die Weiterleitungsregeln für die Firewall erstellen. Weil die Firewall sämtlichen eingehenden und ausgehenden Datenverkehr sowie den Datenverkehr innerhalb des virtuellen Netzwerks blockiert oder weiterleitet, benötigen Sie viele Firewallregeln. Darüber hinaus muss die Firewall jeglichen eingehenden Datenverkehr an die öffentliche IP-Adresse des Sicherheitsdiensts (über unterschiedliche Ports) verarbeiten. Um spätere Überarbeitungen zu vermeiden, sollten Sie in bewährter Weise vorgehen, indem Sie die logischen Datenflüsse in Diagrammen abbilden, bevor Sie Subnetze und Firewallregeln einrichten. Die folgende Abbildung zeigt eine logische Ansicht der Firewallregeln für dieses Beispiel:

![Logische Ansicht der Firewallregeln][2]

> [!NOTE]
> Verwaltungsports variieren in Abhängigkeit vom virtuellen Netzwerkgerät. In diesem Beispiel wird eine Barracuda NextGen-Firewall verwendet, in der die Ports 22, 801 und 807 verwendet werden. Informationen zu den genauen Ports, über die das Gerät verwaltet wird, finden Sie in der Dokumentation des Geräteanbieters.

### <a name="logical-rule-description"></a>Beschreibung der logischen Regel

Im obigen logischen Diagramm ist das Sicherheitssubnetz nicht dargestellt, weil die Firewall die einzige Ressource in diesem Subnetz ist. Das Diagramm zeigt die Firewallregeln und die Art und Weise, wie sie Datenverkehrsflüsse logisch zulassen oder verweigern, zeigt aber nicht den tatsächlichen Weiterleitungspfad. Außerdem sind die externen Ports, die für den RDP-Datenverkehr (Remotedesktopprotokoll) ausgewählt sind, Ports im höheren Bereich (8014 – 8026), die zwecks besserer Lesbarkeit so ausgewählt wurden, dass sie an die letzten beiden Oktette der lokalen IP-Adressen angeglichen sind. Zum Beispiel ist die lokale Serveradresse 10.0.1.4 mit dem Port 8014 verknüpft. Sie können jedoch jeden höheren konfliktfreien Port verwenden.

Sie benötigen folgende Arten von Regeln für dieses Beispiel:

* Externe Regeln für eingehenden Datenverkehr:
  1. Firewallverwaltungsregel: Ermöglicht die Weiterleitung von Datenverkehr an die Verwaltungsports des virtuellen Netzwerkgeräts.
  2. RDP-Regeln für jeden Windows-Server: Ermöglichen die Verwaltung der einzelnen Server über RDP.  Abhängig von den Leistungsmerkmalen Ihres virtuellen Netzwerkgeräts können Sie die Regeln möglicherweise zu einer Regel zusammenfassen.
  3. Regeln für Anwendungsdatenverkehr: eine für Front-End-Datenverkehr und eine für Back-End-Datenverkehr (z. B. Webserver zur Datenschicht). Die Konfiguration dieser Regeln hängt von der Netzwerkarchitektur und den Datenverkehrsflüssen ab.

     * Die erste Regel lässt zu, dass tatsächlicher Anwendungsdatenverkehr den Anwendungsserver erreicht. Im Gegensatz zu Regeln für Sicherheit, Verwaltung usw. ermöglichen Anwendungsregeln es Benutzern oder Diensten, auf die Anwendung(en) zuzugreifen. In diesem Beispiel gibt es einen einzigen Webserver an Port 80, für den eine einzige Firewallanwendungsregel berechtigt ist, Datenverkehr umzuleiten, der für eine externe IP-Adresse vorgesehen ist, statt ihn an die interne IP-Adresse des Webservers weiterzuleiten. Die umgeleitete Datenverkehrssitzung wird per Netzwerkadressübersetzung (Network Address Translation, NAT) dem internen Server neu zugeordnet.
     * Die zweite Regel für Anwendungsdatenverkehr ist die Back-End-Regel, die es dem Webserver ermöglicht, über einen beliebigen Port Datenverkehr an den Server AppVM01, aber nicht an den Server AppVM02 weiterzuleiten.

* Interne Regeln für den Datenverkehr innerhalb des virtuellen Netzwerks:
  1. Regel für ausgehenden Datenverkehr zum Internet: Ermöglicht, dass Datenverkehr aus jedem Netzwerk an die ausgewählten Netzwerke weitergeleitet werden kann. Diese Regel ist üblicherweise standardmäßig in der Firewall vorhanden, jedoch deaktiviert. Aktivieren Sie diese Regel für dieses Beispiel.
  2. DNS-Regel: Lässt nur zu, dass DNS-Datenverkehr (Port 53) an den DNS-Server weitergeleitet werden kann. Für diese Umgebung wird der meiste Datenverkehr vom Front-End an das Back-End blockiert. Diese Regel lässt insbesondere DNS aus allen lokalen Subnetzen zu.
  3. Subnetz-zu-Subnetz-Regel: Ermöglicht es jedem Server im Back-End-Subnetz, eine Verbindung mit jedem Server im Front-End-Subnetz herzustellen (nicht jedoch umgekehrt).

* Ausfallsicherheitsregel für Datenverkehr, für den keine der obigen Regeln gilt:
  1. Regel zum Ablehnen jeglichen Datenverkehrs: Dies ist immer die letzte Regel hinsichtlich der Priorität. Passt Datenverkehrsfluss zu keiner der vorherigen Regeln, wird er von diese Regel blockiert. Sie ist eine Standardregel. Da sie üblicherweise aktiviert ist, sind keine Änderungen erforderlich.

> [!TIP]
> In der zweiten Regel für Anwendungsdatenverkehr ist jeder Port zulässig, damit dieses Beispiel einfach bleibt. In einem realen Szenario sollten Sie bestimmte Port- und Adressbereiche verwenden, um die Angriffsfläche dieser Regel zu reduzieren.


> [!IMPORTANT]
> Nachdem Sie die Regeln erstellt haben, ist es unverzichtbar, dass Sie die Priorität jeder einzelnen Regel prüfen, um sicherzustellen, dass der Datenverkehr wie gewünscht zugelassen oder verweigert wird. In diesem Beispiel wurden die Regeln nach Priorität geordnet. Es kann leicht passieren, dass Sie von der Firewall ausgesperrt werden, wenn die Regeln in falscher Reihenfolge vorliegen. Achten Sie darauf, dass Sie die Firewallverwaltungsregel mit der absolut höchsten Priorität festlegen.

### <a name="rule-prerequisites"></a>Voraussetzungen für Regeln

Öffentliche Endpunkte sind für den virtuellen Computer erforderlich, auf dem die Firewall ausgeführt wird. Diese öffentlichen Endpunkte müssen offen sein, damit die Firewall Datenverkehr verarbeiten kann. Es gibt drei Arten von Datenverkehr in diesem Beispiel:

1. Verwaltungsdatenverkehr zum Steuern der Firewall und der Firewallregeln
1. RDP-Datenverkehr zum Steuern der Windows-Server
1. Anwendungsdatenverkehr

Die Datenverkehrsarten sind in der oberen Hälfte des obigen Logikdiagramms für Firewallregeln aufgeführt.

> [!IMPORTANT]
> Beachten Sie, dass der *gesamte* Datenverkehr durch die Firewall läuft. Um über Remotedesktop eine Verbindung mit dem Server IIS01 herzustellen, müssen Sie eine Verbindung mit der Firewall über Port 8014 herstellen und der Firewall gestatten, die RDP-Anforderung intern an den RDP-Port von IIS01 weiterleiten zu können. Die Schaltfläche **Verbinden** im Azure-Portal funktioniert hier nicht, da es aus Sicht des Portals keinen direkten RDP-Pfad zu IIS01 gibt. Alle Verbindungen aus dem Internet werden an den Sicherheitsdienst und einen Port (z. B. secscv001.cloudapp.net:xxxx) weitergeleitet. Beachten Sie das obige Diagramm für die Zuordnung von externem Port und interner IP-Adresse und internem Port.

Sie können einen Endpunkt während der Erstellung des virtuellen Computers oder nach der Erstellung öffnen. Im Beispielskript und im folgenden Codeausschnitt wird ein Endpunkt geöffnet, nachdem der virtuelle Computer erstellt wurde.

Beachten Sie, dass Elemente, die mit `$` beginnen, etwa `$VMName[$i]`, benutzerdefinierte Variablen aus dem Skript im Abschnitt „Referenzen“ sind. `[$i]` stellt die Arraynummer eines bestimmten virtuellen Computers in einem Array virtueller Computer dar.

```powershell
Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
    -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
    Update-AzureVM
```

Obwohl dies hier wegen Variablen nicht eindeutig gezeigt ist, sollten Sie Endpunkte nur im Sicherheitsclouddienst öffnen. Durch diese Vorsichtsmaßnahme lässt sich sicherstellen, dass die Firewall sämtlichen eingehenden Datenverkehr verarbeitet, egal, ob dieser weitergeleitet, über NAT neu zugeordnet oder blockiert wurde.

Installieren Sie einen Verwaltungsclient auf einem PC, um die Firewall zu verwalten und die erforderlichen Konfigurationen zu erstellen. Informationen darüber, wie Sie Ihre Firewall oder ein anderes virtuelles Netzwerkgerät verwalten können, finden Sie in der Dokumentation des Herstellers. Im verbleibenden Teil dieses Abschnitts sowie im Abschnitt **Erstellen von Firewallregeln** ist die Konfiguration der Firewall beschrieben. Verwenden Sie den Verwaltungsclient des Anbieters, nicht das Azure-Portal oder PowerShell.

Wechseln Sie zu [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin), um den Verwaltungsclient herunterzuladen und zu erfahren, wie Sie eine Verbindung mit der Barracuda-Firewall herstellen.

Nachdem Sie sich bei der Firewall angemeldet haben, definieren Sie Netzwerk- und Dienstobjekte, bevor Sie die Firewallregeln erstellen. Diese beiden vorausgesetzten Objektklassen können die Erstellung der Regeln vereinfachen.

Definieren Sie für dieses Beispiel drei benannte Netzwerkobjekte für diese Elemente:

* Front-End-Subnetz
* Back-End-Subnetz
* IP-Adresse des DNS-Servers

So erstellen Sie ein benanntes Netzwerk über das Barracuda NG Admin-Clientdashboard:

1. Navigieren Sie zur **Registerkarte „Configuration“**.
1. Wählen Sie im Abschnitt **Operational Configuration** die Option **Ruleset** aus.
1. Wählen Sie im Menü **Firewall Objects** die Option **Networks** aus.
1. Wählen Sie im Menü **Edit Networks** den Befehl **New** aus.
1. Erstellen Sie das Netzwerkobjekt, indem Sie den Namen und das Präfix hinzufügen:

   ![Erstellen eines Front-End-Netzwerkobjekts][3]

In den vorherigen Schritten wurde ein benanntes Netzwerk für das Front-End-Subnetz erstellt. Erstellen Sie ein ähnliches Objekt für das Back-End-Subnetz. Jetzt können die Subnetze in den Firewallregeln leichter anhand ihrer Namen referenziert werden.

Für das DNS-Serverobjekt:

![Erstellen eines DNS-Serverobjekts][4]

Dieser Verweis auf eine einzelne IP-Adresse wird in einer DNS-Regel weiter unten in diesem Dokument verwendet.

Die andere Objektklasse enthält Dienstobjekte, die den RDP-Verbindungsports für jeden Server entsprechen. Das vorhandene RDP-Dienstobjekt ist an den RDP-Standardport 3389 gebunden. Daher können Sie neue Dienste erstellen, um Datenverkehr von den externen Ports (8014 bis 8026) zuzulassen. Sie können die neuen Ports auch zum vorhandenen RDP-Dienst hinzufügen. Um das Beispiel anschaulicher zu halten, können Sie jedoch für jeden Server eine eigene Regel festlegen. So erstellen Sie eine benannte RDP-Regel für einen Server über das Barracuda NG Admin-Clientdashboard:

1. Navigieren Sie zur **Registerkarte „Configuration“**.
1. Wählen Sie im Abschnitt **Operational Configuration** die Option **Ruleset** aus.
1. Wählen Sie im Menü **Firewall Objects** die Option **Services**  aus.
1. Scrolle Sie in der Liste der Dienste nach unten, und wählen **RDP** aus.
1. Klicken Sie mit der rechten Maustaste, und wählen Sie „Copy“ aus. Klicken Sie erneut mit der rechten Maustaste, und wählen Sie „Paste“ aus.
1. Es gibt nun ein Dienstobjekt namens „RDP-Copy1“, das bearbeitet werden kann. Klicken Sie mit der rechten Maustaste auf **RDP-Copy1**, und wählen Sie **Bearbeiten** aus.
1. Das Popupfenster **Edit/Create Service Object** wird wie folgt angezeigt:

   ![Kopie der RDP-Standardregel][5]

1. Bearbeiten Sie die Werte, um den RDP-Dienst für einen bestimmten Server darzustellen. Für AppVM01 sollte die RDP-Standardregel so bearbeitet werden, das sie dem **Namen** und der **Beschreibung** des neuen Diensts und dem externen RDP-Port entspricht, der in Abbildung 8 verwendet ist. Beachten Sie, dass die Ports von der RDP-Standardeinstellung 3389 in den externen Port für diesen speziellen Server geändert werden. Beispielsweise ist der externe Port für AppVM01 der Port 8025. Die geänderte Dienstregel sieht wie folgt aus:

   ![AppVM01-Regel][6]

Wiederholen Sie diesen Vorgang, um die RDP-Dienste für die verbleibenden Server zu erstellen: AppVM02, DNS01 und IIS01. Mit diesen Diensten wird es einfacher, die Regeln im nächsten Abschnitt zu erstellen, und werden die Regeln übersichtlicher.

> [!NOTE]
> Ein RDP-Dienst für die Firewall ist nicht erforderlich, weil die virtuelle Firewall-VM ein Linux-basiertes Image ist, sodass zur VM-Verwaltung nicht RDP, sondern SSH an Port 22 verwendet wird. Darüber hinaus Sind Port 22 und zwei weitere Ports für Verwaltungsverbindungen zulässig. Weitere Informationen hierzu finden Sie im nächsten Abschnitt in der **Firewallverwaltungsregel**.

### <a name="firewall-rules-creation"></a>Erstellen von Firewallregeln

In diesem Beispiel werden drei Arten von Firewallregeln verwendet, die jeweils unterschiedliche Symbole haben:

Die Regel zur Anwendungsumleitung: ![Symbol der Regel zur Anwendungsumleitung][7]

Die Ziel-NAT-Regel: ![Symbol der Ziel-NAT-Regel][8]

Die Übergaberegel: ![Symbol der Übergaberegel][9]

Weitere Informationen zu diesen Regeln finden Sie auf der Barracuda-Website.

So erstellen Sie die folgenden Regeln oder überprüfen Sie vorhandene Standardregeln:

1. Navigieren Sie aus dem Barracuda NG Admin-Clientdashboard zur Registerkarte **Configuration**.
1. Wählen Sie im Abschnitt **Operational Configuration** die Option **Ruleset** aus.
1. Im Raster **Main Rules** werden die vorhandenen aktiven und deaktivierten Regeln für diese Firewall angezeigt. Wählen Sie das grüne **+** in der oberen rechten Ecke aus, um eine neue Regel zu erstellen. Ist Ihre Firewall für Änderungen gesperrt, sehen Sie eine mit **Lock** gekennzeichnete Schaltfläche, und Sie können Regeln weder erstellen noch bearbeiten. Wählen Sie die Schaltfläche **Lock** aus, um den Regelsatz zu entsperren und Bearbeitung zuzulassen. Klicken Sie mit der rechten Maustaste auf die Regel, die Sie bearbeiten möchten, und wählen Sie **Edit Rule** aus.

Nachdem Sie eine Regel erstellt oder geändert haben, müssen Sie diese per Push an die Firewall senden und aktivieren. Andernfalls werden die Regeländerungen nicht wirksam. Der Push- und Aktivierungsprozess ist in [Regelaktivierung](#rule-activation) beschrieben.

Nachfolgend sind die Einzelheiten zu allen Regeln aufgeführt, die zum Vervollständigen dieses Beispiels erforderlich sind:

* **Firewallverwaltungsregel**: Diese Regel zur Anwendungsumleitung ermöglicht die Weiterleitung von Datenverkehr an die Verwaltungsports des virtuellen Netzwerkgeräts, bei dem es sich in diesem Fall um eine Barracuda NextGen-Firewall handelt. Die Verwaltungsports lauten 801, 807 und optional 22. Die externen und internen Ports sind identisch (es gibt keine Portübersetzung). Diese Regel hat den Namen SETUP-MGMT-ACCESS. Sie ist eine Standardregel, die standardmäßig in Barracuda NextGen Firewall, Version 6.1, aktiviert ist.
  
    ![Firewallverwaltungsregel][10]

  > [!TIP]
  > Der Quelladressraum in dieser Regel ist **Any**. Sind die IP-Verwaltungsadressbereiche bekannt, bewirkt eine Verkleinerung dieses Bereichs, dass auch die Angriffsfläche der Verwaltungsports verkleinert wird.

* **RDP-Regeln**:  Diese Ziel-NAT-Regeln ermöglichen die Verwaltung der einzelnen Server über RDP. Die wichtigsten Felder für diese Regeln sind:
  * Source. Um eine Verwaltung per RDP von beliebiger Stelle aus zu ermöglichen, verwenden Sie im Feld „Source“ den Verweis **Any**.
  * Dienst: Verwenden Sie das RDP-Dienstobjekt, das Sie zuvor erstellt haben: **AppVM01 RDP**. Für die externen Ports erfolgt eine Weiterleitung an die lokale IP-Adresse des Servers und den RDP-Standardport 3386. Diese spezifische Regel dient zum Zugriff auf AppVM01 über RDP.
  * Destination. Verwenden Sie den lokalen Port in der Firewall: **DCHP 1 Local IP** oder **eth0**, wenn Sie statische IP-Adressen verwenden. Die Ordnungszahl (eth0, eth1 usw.) kann abweichen, wenn Ihr Netzwerkgerät mehrere lokale Schnittstellen hat. Die Firewall verwendet diesen Port zum Senden, und der Port kann mit dem empfangenden Port identisch sein. Das tatsächliche Weiterleitungsziel befindet sich im Feld **Target List**.
  * Redirection. Konfigurieren Sie diesen Abschnitt, um dem virtuellen Gerät mitzuteilen, wohin es diesen Datenverkehr letztendlich umleiten soll. Die einfachste Umleitung besteht darin, die IP-Adresse in das Feld „Target List“ einzutragen. Sie können auch den Port angeben, und NAT leitet sowohl den Port als auch die IP-Adresse um. Wenn Sie keinen Port angeben, verwendet das virtuelle Gerät den Zielport aus der eingehenden Anforderung.

    ![RDP-Firewallregel][11]

    Erstellen Sie vier RDP-Regeln:

    | Regelname | Server | Dienst | Zielliste |
    | --- | --- | --- | --- |
    | RDP-to-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
    | RDP-to-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
    | RDP-to-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
    | RDP-to-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

  > [!TIP]
  > Indem Sie den Bereich in den Feldern „Source“ und „Service“ verkleinern, verringern Sie die Angriffsfläche. Verwenden Sie den am stärksten begrenzten Bereich, für den Funktionalität sichergestellt ist.

* **Regeln für den Anwendungsdatenverkehr**: Es gibt zwei Regeln für den Anwendungsdatenverkehr. Eine ist für den Front-End-Webdatenverkehr vorgesehen. Die andere betrifft den Back-End-Datenverkehr, etwa den Datenverkehr vom Webserver zur Datenschicht. Diese Regeln hängen von der Netzwerkarchitektur und den Datenverkehrsflüssen ab.
  
  * Die Front-End-Regel für Webdatenverkehr:
  
    ![Firewallwebregel][12]
  
    Mit dieser Ziel-NAT-Regel gelangt der tatsächliche Anwendungsdatenverkehr zum Anwendungsserver. Im Gegensatz zu Regeln für Sicherheit, Verwaltung usw. ermöglichen Anwendungsregeln es Benutzern oder Diensten, auf die Anwendung(en) zuzugreifen. In diesem Beispiel gibt es einen einzigen Webserver an Port 80, für den eine einzige Firewallanwendungsregel berechtigt ist, Datenverkehr umzuleiten, der für eine externe IP-Adresse vorgesehen ist, statt ihn an die interne IP-Adresse des Webservers weiterzuleiten. Die umgeleitete Datenverkehrssitzung wird per Netzwerkadressübersetzung (Network Address Translation, NAT) dem internen Server neu zugeordnet.

    > [!NOTE]
    > Im Feld **Target List** ist kein Port zugewiesen. Daher wird der eingehende Port 80 (oder 443 für den ausgewählten Dienst) in der Umleitung des Webservers verwendet. Wenn der Webserver an einem anderen Port lauscht, etwa 8080, können Sie das Feld „Target List“ auf „10.0.1.4:8080“ aktualisieren, um eine Umleitung auch an diesem Port zu ermöglichen.
  
  * Die Back-End-Regel ermöglicht es dem Webserver, über den **Any**-Dienst mit dem Server AppVM01, nicht jedoch mit dem Server AppVM02 zu kommunizieren:
  
    ![AppVM01-Firewallregel][13]
  
    Mit dieser Übergaberegel kann jeder IIS-Server im Front-End-Subnetz den Server AppVM01 (10.0.2.5) über jeden Port mit beliebigem Protokoll erreichen, sodass die Webanwendung auf Daten zugreifen kann.
  
    In diesem Screenshot wird `<explicit-dest>` im Feld **Destination** verwendet, um 10.0.2.5 als Ziel anzugeben. Sie können explizit die IP-Adresse angeben, wie dies im Screenshot gezeigt ist. Sie können auch ein benanntes Netzwerkobjekt verwenden, wie dies in den Voraussetzungen für den DNS-Server angegeben ist. Der Administrator der Firewall kann die zu verwendende Methode wählen. Um 10.0.2.5 als explizites Ziel hinzuzufügen, doppelklicken Sie auf die erste leere Zeile unter `<explicit-dest>`, und geben Sie die Adresse in dem Dialogfenster ein, das angezeigt wird.
  
    Mit dieser Übergaberegel ist keine Netzwerkadressübersetzung erforderlich, da die Regel internen Datenverkehr verarbeitet. Sie können die Verbindungsmethode (**Connection Method**) auf `No SNAT` festlegen.
  
    > [!NOTE]
    > Das Quellnetzwerk in dieser Regel ist jede Ressource im Front-End-Subnetz, wenn es nur eines gibt. Wenn in Ihrer Architektur eine bekannte Anzahl von Webservern angegeben ist, können Sie eine Netzwerkobjektressource erstellen, um eingegrenzter diese genauen IP-Adressen anstelle des gesamten Front-End-Subnetzes anzugeben.

    > [!TIP]
    > In dieser Regel wird der Dienst **Any** verwendet, um das Einrichten und Verwenden der Beispielanwendung zu vereinfachen. Dies ermöglicht ICMPv4 (ping) in einer einzigen Regel. Um jedoch die Angriffsfläche entlang dieser Grenze zu verringern, sollten Sie die Ports und Protokolle (Services) auf das Mindestmaß einschränken, mit dem ein Anwendungsbetrieb möglich ist.

    > [!TIP]
    > Obwohl in dieser Beispielregel der Verweis `<explicit-dest>` verwendet wird, Sie sollten in der gesamten Firewallkonfiguration einen konsistenten Ansatz verwenden. Es empfiehlt sich, ein benanntes Netzwerkobjekt zu verwenden, um die Lesbarkeit und Wartbarkeit zu vereinfachen. Der hier gezeigte Verweis `<explicit-dest>` dient nur dazu, eine alternative Verweismethode vorzustellen. Diese Methode ist grundsätzlich nicht zu empfehlen, insbesondere für komplexe Konfigurationen.

* **Regel für ausgehenden Datenverkehr zum Internet**: Diese Übergaberegel ermöglicht es, Datenverkehr aus jedem Quellnetzwerk an die ausgewählten Zielnetzwerke zu übergeben. In der Barracuda NextGen-Firewall ist diese Regel üblicherweise standardmäßig „eingeschaltet“, aber im deaktivierten Zustand. Klicken Sie mit der rechten Maustaste auf diese Regel, und wählen Sie den Befehl **Activate Rule** aus. Ändern Sie die im Screenshot gezeigte Regel, indem Sie die Netzwerkobjekte für das Back-End- und das Front-End-Subnetz zum „Source“-Attribut dieser Regel hinzufügen. Sie haben diese Netzwerkobjekte im Verlauf dieses Artikels im Abschnitt „Voraussetzungen für Regeln“ erstellt.
  
    ![Ausgehende Firewallregel][14]

* **DNS-Regel**: Mit dieser Übergaberegel kann nur DNS-Datenverkehr (Port 53) an den DNS-Server übergeben werden. Für diese Umgebung wird der größte Teil des Datenverkehrs aus dem Front-End an das Back-End blockiert, sodass diese Regel speziell DNS-Datenverkehr zulässt.
  
    ![DNS-Firewallregel][15]
  
    > [!NOTE]
    > **Connection Method** ist auf `No SNAT` festgelegt, weil diese Regel für Datenverkehr zwischen internen IP-Adressen vorgesehen und kein Umleiten über Netzwerkadressübersetzung erforderlich ist.

* **Subnetz-zu-Subnetz-Regel**: Diese Standardübergaberegel wurde aktiviert und bearbeitet, um jedem Server im Back-End-Subnetz die Herstellung einer Verbindung mit jedem Server im Front-End-Subnetz zu ermöglichen. Diese Regel gilt nur für internen Datenverkehr, sodass **Connection Method** auf `No SNAT` festgelegt werden kann.

    ![VNet-interne Firewallregel][16]
  
    > [!NOTE]
    > Das Kontrollkästchen **Bi-directional** ist nicht hier aktiviert, sodass diese Regel nur in eine Richtung gilt. Eine Verbindung kann nur vom Back-End-Subnetz zum Front-End-Netzwerk initiiert werden, nicht jedoch umgekehrt. Wäre dieses Kontrollkästchen aktiviert, würde diese Regel bidirektionalen Datenverkehr zulassen, der im logischen Diagramm als unerwünscht angegeben ist.

* **Regel zum Ablehnen jeglichen Datenverkehrs**: Diese Regel sollte immer die letzte Regel hinsichtlich der Priorität sein. Wenn der Datenverkehrsfluss zu keiner der vorherigen Regeln passt, bewirkt diese Regel, dass er verworfen wird. Diese Regel ist normalerweise standardmäßig aktiviert, sodass keine Änderungen erforderlich sind.
  
    ![Firewallablehnungsregel][17]

> [!IMPORTANT]
> Nachdem alle oben aufgeführten Regeln erstellt sind, sollten Sie die Priorität jeder Regel prüfen, um sicherzustellen, dass der Datenverkehr wie gewünscht zugelassen oder abgelehnt wird. In diesem Beispiel sind die Regeln in der Reihenfolge aufgeführt, in der sie im Barracuda-Verwaltungsclient in dessen Hauptraster für Weiterleitungsregeln angezeigt werden sollten.

## <a name="rule-activation"></a>Regelaktivierung

Nachdem Sie den Regelsatz so geändert haben, dass er den Spezifikationen des logischen Diagramms entspricht, müssen Sie den Regelsatz in die Firewall hochladen und aktivieren.

![Aktivierung der Firewallregeln][18]

Schauen Sie in die obere rechte Ecke des Clientverwaltungsfensters, und wählen Sie **Send Changes** aus, um die geänderten Regeln in die Firewall hochzuladen. Wählen Sie **Aktivieren**aus.

Wenn Sie den Regelsatz für die Firewall aktiviert haben, ist diese Beispielumgebung vollständig.

## <a name="traffic-scenarios"></a>Datenverkehrsszenarien

> [!IMPORTANT]
> Beachten Sie, dass der *gesamte* Datenverkehr durch die Firewall läuft. Um über Remotedesktop eine Verbindung mit dem Server IIS01 herzustellen, müssen Sie eine Verbindung mit der Firewall über Port 8014 herstellen und der Firewall gestatten, die RDP-Anforderung intern an den RDP-Port von IIS01 weiterleiten zu können. Die Schaltfläche **Verbinden** im Azure-Portal funktioniert hier nicht, da es aus Sicht des Portals keinen direkten RDP-Pfad zu IIS01 gibt. Alle Verbindungen aus dem Internet werden an den Sicherheitsdienst und einen Port (z. B. secscv001.cloudapp.net:xxxx) weitergeleitet. Beachten Sie das obige Diagramm für die Zuordnung von externem Port und interner IP-Adresse und internem Port.

Für diese Szenarien sollten folgende Firewallregeln eingerichtet sein:

1. Firewallverwaltung (FW-Verwaltung)
2. RDP an IIS01
3. RDP an DNS01
4. RDP an AppVM01
5. RDP an AppVM02
6. Datenverkehr aus Anwendung zum Web
7. Datenverkehr aus Anwendung an AppVM01
8. Ausgehend zum Internet
9. Front-End an DNS01
10. Subnetzinterner Datenverkehr (nur vom Back-End zum Front-End)
11. Alle ablehnen

Ihr tatsächlicher Regelsatz für die Firewall erfordert höchstwahrscheinlich mehr Regeln, als in diesem Beispiel aufgeführt sind. Außerdem haben diese wahrscheinlich andere Prioritätsnummern. Sie sollten anhand dieser Liste und den zugeordneten Nummern deren relative Priorität untereinander prüfen. Beispielsweise kann die Regel „RDP an IIS01“ die Nummer 5 in der tatsächlichen Firewall sein, aber solange diese Regel sich unterhalb der Regel für die Firewallverwaltung und oberhalb von „RDP an DNS01“ befindet, entspricht die der Regelsatz der Intention dieser Liste. Mit dieser Liste lassen sich auch die Anweisungen für die Szenarien vereinfachen, die folgen. Nehmen Sie als Beispiel die Firewallregel 9 (DNS). Beachten Sie, dass die vier RDP-Regeln kollektiv als „die RDP-Regeln“ bezeichnet werden, wenn es in einem Datenverkehrsszenario nicht um RDP geht.

Denken Sie auch daran, dass Netzwerksicherheitsgruppen (NSGs) für den eingehenden Internetdatenverkehr im Front-End- und im Back-End-Subnetz eingerichtet wurden.

### <a name="allowed-internet-to-web-server"></a>(Zulässig) Internet an Webserver

1. Ein Internetbenutzer fordert HTTP-Seite von SecSvc001.CloudApp.Net (Clouddienst mit Schnittstelle zum Internet) an.
1. Der Clouddienst übergibt Datenverkehr über einen offenen Endpunkt an Port 80 an die Firewallschnittstelle unter 10.0.0.4:80.
1. Dem Sicherheitssubnetz ist keine NSG zugewiesen, daher lassen die NSG-Regeln des Systems den Datenverkehr an die Firewall zu.
1. Der Datenverkehr trifft an einer internen IP-Adresse der Firewall (10.0.1.4) ein.
1. In der Firewall werden die Regeln verarbeitet:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 2 bis 5 (RDP-Regeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Firewallregel 6 (Anwendung: Web) trifft zu. Der Datenverkehr wird zugelassen. Die Firewall leitet den Datenverkehr über NAT an 10.0.1.4 (IIS01) um.
1. Im Front-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
   1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Die Firewall hat diesen Datenverkehr über NAT umgeleitet, sodass die Firewall nun die Quelladresse ist. Weil sich die Firewall im Sicherheitssubnetz befindet und von der Front-End-Subnetz-NSG als lokaler Datenverkehr angesehen wird, wird der Datenverkehr zugelassen. Weiter zur nächsten Regel.
   1. NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu, somit wird dieser Datenverkehr zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. IIS01 lauscht auf Webdatenverkehr. Er empfängt diese Anforderung und beginnt mit deren Verarbeitung.
1. IIS01 versucht, eine FTP-Sitzung mit AppVM01 im Back-End-Subnetz zu initiieren.
1. Aufgrund der UDR-Route im Front-End-Subnetz ist die Firewall der nächste Hop.
1. Es gibt keine Ausgangsregeln im Front-End-Subnetz, also wird der Datenverkehr zugelassen.
1. Firewall beginnt mit Regelverarbeitung:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 2 bis 5 (RDP-Regeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Firewallregel 6 (Anwendung: Web) trifft nicht zu. Weiter zur nächsten Regel.
   1. Firewallregel 7 (Anwendung: Back-End) trifft zu. Der Datenverkehr wird zugelassen. Die Firewall leitet den Datenverkehr an 10.0.2.5 (AppVM01) weiter.
1. Im Back-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
    1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Weiter zur nächsten Regel.
    1. NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu. Der Datenverkehr wird zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. AppVM01 empfängt die Anforderung, initiiert die Sitzung und antwortet.
1. Aufgrund der UDR-Route im Back-End-Subnetz ist die Firewall der nächste Hop.
1. Es gibt keine NSG-Ausgangsregeln für das Back-End-Subnetz, also wird die Antwort zugelassen.
1. Da es sich hier um zurückkommenden Datenverkehr in einer vorhandenen Sitzung handelt, übergibt die Firewall die Antwort an den Webserver (IIS01).
1. Im Front-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
    1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Weiter zur nächsten Regel.
    1. Die NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu, somit wird dieser Datenverkehr zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. Der IIS-Server empfängt die Antwort und schließt die Transaktion mit AppVM01 ab. Der Server schließt dann das Erstellen der HTTP-Antwort ab und sendet diese an die anfordernde Komponente.
1. Es gibt keine NSG-Ausgangsregeln für das Front-End-Subnetz, also wird die Antwort zugelassen.
1. Die HTTP-Antwort trifft an der Firewall ein. Weil sie eine Antwort an eine vorhandene NAT-Sitzung ist, wird die Antwort von der Firewall akzeptiert.
1. Die Firewall leitet die Antwort an den Internetbenutzer weiter.
1. Es gibt keine NSG-Ausgangsregeln oder UDR-Hops für das Front-End-Subnetz, also wird die Antwort zugelassen. Der Internetbenutzer empfängt die angeforderte Webseite.

### <a name="allowed-internet-rdp-to-back-end"></a>(Zulässig) Internet-RDP an Back-End

1. Ein Serveradministrator im Internet fordert eine RDP-Sitzung mit AppVM01 über SecSvc001.CloudApp.Net:8025 an. 8025 ist die Benutzern zugewiesene Portnummer für die Firewallregel 4 (RDP an AppVM01).
1. Der Clouddienst übergibt den Datenverkehr über den offenen Endpunkt an Port 8025 an die Firewallschnittstelle an 10.0.0.4:8025.
1. Dem Sicherheitssubnetz ist keine NSG zugewiesen, daher lassen die systemeigenen NSG-Regeln den Datenverkehr an die Firewall zu.
1. In der Firewall werden die Regeln verarbeitet:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Firewallregel 2 (RDP an IIS01) trifft nicht zu. Weiter zur nächsten Regel.
   1. Firewallregel 3 (RDP an DNS01) trifft nicht zu. Weiter zur nächsten Regel.
   1. Firewallregel 4 (RDP an AppVM01) trifft zu, also wird Datenverkehr zugelassen. Die Firewall leitet Datenverkehr über NAT an 10.0.2.5:3386 (RDP-Port auf AppVM01) um.
1. Im Back-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
   1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Die Firewall hat diesen Datenverkehr über NAT umgeleitet, sodass die Firewall, die sich im Sicherheitssubnetz befindet, nun die Quelladresse ist. Für die Back-End-Subnetz-NSG sieht dieser Datenverkehr wie lokaler Datenverkehr aus und wird zugelassen. Weiter zur nächsten Regel.
   1. NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu, somit wird dieser Datenverkehr zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. AppVM01 lauscht auf RDP-Datenverkehr und antwortet.
1. Es gibt keine NSG-Ausgangsregeln, also gelten die Standardregeln. Antwortdatenverkehr wird zugelassen.
1. UDR routet ausgehenden Datenverkehr an die Firewall, die den nächsten Hop darstellt.
1. Da es sich hier um zurückkommenden Datenverkehr in einer vorhandenen Sitzung handelt, übergibt die Firewall die Antwort an den Internetbenutzer.
1. Die RDP-Sitzung wird aktiviert.
1. AppVM01 fordert zur Eingabe von Benutzername und Kennwort auf.

### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Zugelassen) Webserver-DNS-Lookup auf DNS-Server

1. Webserver IIS01 fordert einen Datenfeed von „http\:\/\/www.data.gov“ an, muss jedoch die Adresse auflösen.
1. Die Netzwerkkonfiguration für das virtuelle Netzwerk listet DNS01 (10.0.2.4 im Back-End-Subnetz) als den primären DNS-Server auf. IIS01 sendet die DNS-Anforderung an DNS01.
1. UDR routet ausgehenden Datenverkehr an die Firewall, die den nächsten Hop darstellt.
1. Es sind keine NSG-Ausgangsregeln an das Front-End-Subnetz gebunden. Der Datenverkehr wird zugelassen.
1. In der Firewall werden die Regeln verarbeitet:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 2 bis 5 (RDP-Regeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 6 und 7 (Anwendungsregeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 8 (zum Internet) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 9 (DNS) trifft zu. Der Datenverkehr wird zugelassen. Die Firewall leitet den Datenverkehr an 10.0.2.4 (DNS01) weiter.
1. Im Back-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
   1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Weiter zur nächsten Regel.
   1. NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu. Der Datenverkehr wird zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. Der DNS-Server empfängt die Anforderung.
1. Die Adresse ist nicht im DNS-Server zwischengespeichert, daher fragt der DNS-Server die Adresse bei einem DNS-Stammserver im Internet ab.
1. UDR leitet den ausgehenden Datenverkehr an die Firewall weiter, die den nächsten Hop darstellt.
1. Es gibt keine NSG-Ausgangsregeln für das Back-End-Subnetz, also wird der Datenverkehr zugelassen.
1. In der Firewall werden die Regeln verarbeitet:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 2 bis 5 (RDP-Regeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 6 und 7 (Anwendungsregeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 8 (zum Internet) trifft zu. Der Datenverkehr wird zugelassen. Die Sitzung wird über SNAT an den DNS-Stammserver im Internet umgeleitet.
1. Der Internet-DNS-Server antwortet. Diese Sitzung wurde von der Firewall initiiert, also wird die Antwort von der Firewall akzeptiert.
1. Diese Sitzung ist bereits vorhanden, also leitet die Firewall die Antwort an den Ausgangsserver, DNS01, weiter.
1. Im Back-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
    1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Weiter zur nächsten Regel.
    1. NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu, somit wird dieser Datenverkehr zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. Der DNS-Server empfängt die Antwort, speichert sie zwischen und gibt dann die Antwort auf die ursprüngliche Anforderung zurück an IIS01.
1. Aufgrund der UDR-Route im Back-End-Subnetz ist die Firewall der nächste Hop.
1. Für das Back-End-Subnetz sind keine NSG-Ausgangsregeln vorhanden, also wird der Datenverkehr zugelassen.
1. Diese Sitzung ist bereits in der Firewall eingerichtet, also leitet die Firewall die Antwort an den IIS-Server weiter.
1. Im Front-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
    1. Es gibt keine NSG-Regel für eingehenden Datenverkehr vom Back-End- zum Front-End-Subnetz, somit trifft keine der NSG-Regeln zu.
    1. Die standardmäßige Systemregel lässt Datenverkehr zwischen Subnetzen zu. Der Datenverkehr wird zugelassen.
1. IIS01 empfängt die Antwort von DNS01.

### <a name="allowed-back-end-server-to-front-end-server"></a>(Zulässig) Back-End-Server an Front-End-Server

1. Ein über RDP bei AppVM02 angemeldeter Administrator fordert eine Datei über den Windows-Datei-Explorer direkt vom IIS01-Server an.
1. Aufgrund der UDR-Route im Back-End-Subnetz ist die Firewall der nächste Hop.
1. Es gibt keine NSG-Ausgangsregeln für das Back-End-Subnetz, also wird die Antwort zugelassen.
1. In der Firewall werden die Regeln verarbeitet:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 2 bis 5 (RDP-Regeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 6 und 7 (Anwendungsregeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 8 (zum Internet) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 9 (DNS) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 10 (subnetzintern) trifft zu. Der Datenverkehr wird zugelassen. Die Firewall übergibt Datenverkehr an 10.0.1.4 (IIS01).
1. Das Front-End-Subnetz beginnt damit, die Regel für eingehenden Datenverkehr zu verarbeiten:
   1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu, somit wird der Datenverkehr zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. Vorausgesetzt, Authentifizierung und Autorisierung sind ordnungsgemäß erfolgt, akzeptiert IIS01 die Anforderung und antwortet.
1. Aufgrund der UDR-Route im Front-End-Subnetz ist die Firewall der nächste Hop.
1. Es gibt keine NSG-Ausgangsregeln für das Front-End-Subnetz, also wird die Antwort zugelassen.
1. Diese Sitzung bereits ist in der Firewall vorhanden, sodass diese Antwort zugelassen wird. Die Firewall gibt die Antwort an AppVM02 zurück.
1. Im Back-End-Subnetz werden die Regeln für eingehenden Datenverkehr verarbeitet:
    1. NSG-Regel 1 (Internet blockieren) trifft nicht zu. Weiter zur nächsten Regel.
    2. Die NSG-Standardregeln lassen Datenverkehr zwischen Subnetzen zu, also wird der Datenverkehr zugelassen. Die Verarbeitung der NSG-Regeln wird beendet.
1. AppVM02 empfängt die Antwort.

### <a name="denied-internet-direct-to-web-server"></a>(Abgelehnt) Internet direkt an Webserver

1. Ein Internetbenutzer versucht, über den FrontEnd001.CloudApp.Net-Dienst auf Webserver IIS01 zuzugreifen.
1. Es sind keine Endpunkte für HTTP-Datenverkehr offen, also wird dieser Datenverkehr nicht durch den Clouddienst weitergeleitet. Der Datenverkehr erreicht den Server nicht.
1. Sind die Endpunkte aus irgendeinem Grund offen, blockiert die NSG-Regel (Internet blockieren) für das Front-End-Subnetz diesen Datenverkehr.
1. Schließlich sendet die UDR-Route im Front-End-Subnetz jeglichen ausgehenden Datenverkehr von IIS01 an die Firewall als nächsten Hop. Die Firewall erkennt diesen Datenverkehr als asymmetrischen Datenverkehr und verwirft die ausgehende Antwort.

>Somit gibt es mindestens drei unabhängige Sicherheitsstufen zwischen dem Internet und IIS01. Der Clouddienst verhindert nicht autorisierten oder zweckwidrigen Zugriff.

### <a name="denied-internet-to-back-end-server"></a>(Abgelehnt) Internet an Back-End-Server

1. Ein Internetbenutzer versucht, über den BackEnd001.CloudApp.Net-Dienst auf eine Datei auf AppVM01 zuzugreifen.
2. Es sind keine Endpunkte für Dateifreigaben offen, also wird diese Anforderung vom Clouddienst nicht weitergeleitet. Der Datenverkehr erreicht den Server nicht.
3. Sind die Endpunkte aus irgendeinem Grund offen, blockiert die NSG-Regel (Internet blockieren) diesen Datenverkehr.
4. Schließlich sendet die UDR-Route jeglichen ausgehenden Datenverkehr von AppVM01 an die Firewall als nächsten Hop. Die Firewall erkennt diesen Datenverkehr als asymmetrischen Datenverkehr und verwirft die ausgehende Antwort.

> Somit gibt es mindestens drei unabhängige Sicherheitsstufen zwischen dem Internet und AppVM01. Der Clouddienst verhindert nicht autorisierten oder zweckwidrigen Zugriff.

### <a name="denied-front-end-server-to-back-end-server"></a>(Abgelehnt) Front-End-Server an Back-End-Server

1. IIS01 ist kompromittiert, und auf dem Server wird Schadsoftware ausgeführt, die versucht, die Back-End-Subnetzserver zu scannen.
1. Die UDR-Route im Front-End-Subnetz sendet jeglichen ausgehenden Datenverkehr von IIS01 an die Firewall als nächsten Hop. Die komprommitierte virtuelle Computer kann dieses Routing nicht ändern.
1. Die Firewall verarbeitet den Datenverkehr. Geht die Anforderung an AppVM01 oder an den DNS-Server für DNS-Suchvorgänge, könnte die Firewall den Datenverkehr wegen der Firewallregeln 7 und 9 zulassen. Jeglicher sonstiger Datenverkehr wird durch Firewallregel 11 (Ablehnen jeglichen Datenverkehrs) blockiert.
1. Wenn Sie eine erweiterte Bedrohungserkennung für die Firewall aktivieren, könnte Datenverkehr, der bekannte Signaturen oder Muster enthält, die eine erweiterte Bedrohungsregel kennzeichnen, verhindert werden. Diese Maßnahme kann selbst dann funktionieren, wenn der Datenverkehr entsprechend den grundlegenden Weiterleitungsregeln, die in diesem Artikel erläutert sind, zulässig ist. Erweiterte Bedrohungserkennung wird in diesem Dokument nicht behandelt. Informationen zu den erweiterten Bedrohungserkennungsfunktionen Ihres speziellen Netzwerkgeräts finden Sie in der Dokumentation des Herstellers.

### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Abgelehnt) Internet-DNS-Lookup im DNS-Server

1. Ein Internetbenutzer versucht, über den BackEnd001.CloudApp.Net-Dienst auf DNS01 einen internen DNS-Eintrag nachzuschlagen.
1. Da keine Endpunkte für DNS-Datenverkehr offen sind, wird dieser Datenverkehr nicht durch den Clouddienst weitergeleitet. Er erreicht den Server nicht.
1. Sind die Endpunkte aus irgendeinem Grund offen, blockiert die NSG-Regel (Internet blockieren) für das Front-End-Subnetz diesen Datenverkehr.
1. Schließlich sendet die UDR-Route im Back-End-Subnetz jeglichen ausgehenden Datenverkehr von DNS01 an die Firewall als nächsten Hop. Die Firewall erkennt diesen Datenverkehr als asymmetrischen Datenverkehr und verwirft die ausgehende Antwort.

> Somit gibt es mindestens drei unabhängige Sicherheitsstufen zwischen dem Internet und DNS01. Der Clouddienst verhindert nicht autorisierten oder zweckwidrigen Zugriff.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Abgelehnt) Zugriff auf SQL aus dem Internet über die Firewall

1. Ein Internetbenutzer fordert SQL-Daten vom internetfähigen SecSvc001.CloudApp.Net-Clouddienst an.
1. Es sind keine Endpunkte für SQL offen, also wird dieser Datenverkehr nicht durch den Clouddienst weitergeleitet. Er erreicht die Firewall nicht.
1. Sind aus irgendeinem Grund SQL-Endpunkte offen, führt die Firewall die Verarbeitung der Regeln aus:
   1. Firewallregel 1 (FW-Verwaltung) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 2 bis 5 (RDP-Regeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregeln 6 und 7 (Anwendungsregeln) treffen nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 8 (zum Internet) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 9 (DNS) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 10 (subnetzintern) trifft nicht zu. Weiter zur nächsten Regel.
   1. Die Firewallregel 11 (Ablehnen jeglichen Datenverkehrs) trifft zu. Der Datenverkehr wird blockiert. Verarbeitung von Regeln beenden.

## <a name="references"></a>Referenzen

Dieser Abschnitt enthält die folgenden Elemente:

* Vollständiges Skript. Speichern Sie dieses Skript in einer PowerShell-Skriptdatei.
* Netzwerkkonfiguration. Speichern Sie die Konfiguration in einer Datei namens „NetworkConf2.xml“.

Bearbeiten Sie die benutzerdefinierten Variablen in der Datei nach Bedarf. Führen Sie das Skript aus, und gehen Sie dann gemäß den Anweisungen zum Einrichten der Firewallregeln vor, die weiter oben in diesem Artikel aufgeführt sind.

### <a name="full-script"></a>Vollständiges Skript

Nachdem Sie die benutzerdefinierten Variablen festgelegt haben, führen Sie dieses Skript aus zum:

1. Herstellen einer Verbindung mit einem Azure-Abonnement
1. Erstellen eines neuen Speicherkontos
1. Erstellen eines neuen virtuellen Netzwerks und dreier Subnetze, wie in der Netzwerkkonfigurationsdatei definiert
1. Erstellen von fünf virtuellen Computern: einen als Firewall und vier als Windows-Server
1. Konfigurieren von UDR:
   1. Erstellen von zwei neuen Routingtabellen
   1. Hinzufügen von Routen zu den Tabellen
   1. Binden der Tabellen an geeignete Subnetze
1. Aktivieren der IP-Weiterleitung auf dem virtuellen Netzwerkgerät
1. Konfigurieren der NSG:
   1. Erstellen einer NSG
   1. Hinzufügen einer Regel
   1. Binden Netzwerksicherheitsgruppe (NSG) an die geeigneten Subnetze

Führen Sie dieses PowerShell-Skript lokal auf einem mit dem Internet verbundenen PC oder Server aus.

> [!IMPORTANT]
> Bei der Ausführung des Skripts werden möglicherweise Warnungen oder Informationsmeldungen in PowerShell angezeigt. Nur in Rot angezeigte Fehlermeldungen müssen beachtet werden.

```powershell
    <#
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwarding from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Variables or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) {
            Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Associate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the FrontEnd and BackEnd subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install BackEnd resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
```

### <a name="network-config-file"></a>Netzwerkkonfigurationsdatei

Speichern Sie diese XML-Datei mit dem aktualisierten Speicherort. Ändern Sie die `$NetworkConfigFile`-Variable im vollständigen Skript so, dass sie mit der gespeicherten Netzwerkkonfigurationsdatei verknüpft ist.

```xml
    <NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>
```

## <a name="next-steps"></a>Nächste Schritte

Sie können eine Beispielanwendung installieren, um dieses Beispiel für ein Umkreisnetzwerk zu testen.

> [!div class="nextstepaction"]
> [Skript für Beispielanwendung](./virtual-networks-sample-app.md)

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bidirektionale DMZ mit virtuellem Netzwerkgerät, NSG und benutzerdefiniertem Routing"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische Ansicht der Firewallregeln"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Erstellen eines Front-End-Netzwerkobjekts"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Erstellen eines DNS-Serverobjekts"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopie der RDP-Standardregel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01-Regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Symbol der Regel zur Anwendungsumleitung"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Symbol der Ziel-NAT-Regel"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Symbol der Übergaberegel"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewallverwaltungsregel"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP-Firewallregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Firewallwebregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "AppVM01-Firewallregel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Ausgehende Firewallregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-Firewallregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "VNET-interne Firewallregel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Firewallregel „Alle ablehnen“"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivierung der Firewallregeln"

<!--Link References-->
[HOME]: ../best-practices-network-security.md

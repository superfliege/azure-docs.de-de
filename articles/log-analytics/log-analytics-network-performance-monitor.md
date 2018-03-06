---
title: "Netzwerkleistungsmonitor-Lösung in Azure | Microsoft-Dokumentation"
description: "Mit dem Netzwerkleistungsmonitor in Azure können Sie die Leistung Ihrer Netzwerke nahezu in Echtzeit überwachen, um Leistungsengpässe im Netzwerk zu erkennen und zu lokalisieren."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 399fe552d5c7d9a96cdabc2a1dfafe99635d4a61
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Netzwerkleistungsmonitor-Lösung in Azure

![Symbol des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Der Netzwerkleistungsmonitor (Network Performance Monitor, NPM) ist eine cloudbasierte Hybrid-Netzwerküberwachungslösung, die Sie bei der Überwachung der Netzwerkleistung zwischen verschiedenen Punkten in Ihrer Netzwerkinfrastruktur, bei der Überwachung der Netzwerkkonnektivität mit Dienst-/Anwendungsendpunkten sowie bei der Überwachung der Leistung Ihrer Azure ExpressRoute-Verbindung unterstützt.  

Der NPM erkennt Netzwerkprobleme wie ins Nichts führenden Datenverkehr (Blackholing), Routingfehler und Probleme, die mit herkömmlichen Netzwerküberwachungsmethoden nicht erkannt werden können. Die Lösung generiert Warnungen, benachrichtigt im Falle einer Schwellenwertüberschreitung für einen Netzwerklink, gewährleistet eine zeitnahe Erkennung von Leistungsproblemen im Netzwerk und ordnet die Ursache des Problems einem bestimmten Netzwerksegment oder Gerät zu. 

Der NPM umfasst drei breit gefasste Funktionen: 

[Systemmonitor:](log-analytics-network-performance-monitor-performance-monitor.md) Überwachen Sie die Netzwerkkonnektivität für Cloudbereitstellungen und lokale Standorte, für mehrere Rechenzentren und Zweigstellen sowie für unternehmenswichtige Microservices/Anwendungen mit mehreren Ebenen. Mithilfe des Systemmonitors können Sie Netzwerkprobleme erkennen, bevor sie Ihre Benutzer beinträchtigen.  

[Dienstendpunktmonitor:](log-analytics-network-performance-monitor-service-endpoint.md) Überwachen Sie die Benutzerkonnektivität mit Diensten, die für Sie interessant sind, bestimmen Sie die Infrastruktur, durch die der Pfad führt, und lokalisieren Sie Netzwerkengpässe. Mit dieser Lösung wissen Sie noch vor Ihren Benutzern über Ausfälle Bescheid und können genau sehen, wo auf Ihrem Netzwerkpfad Probleme vorliegen. 

Mithilfe dieser Funktion können Sie HTTP-, HTTPS-, TCP- und ICMP-basierte Tests durchführen, um in Echtzeit oder anhand von Aufzeichnungen die Verfügbarkeit und die Reaktionszeit Ihres Diensts sowie den Beitrag des Netzwerks zu Paketverlusten und Wartezeiten zu überwachen. Mit der Netzwerktopologiekarte können Sie Netzwerkverlangsamungen eingrenzen, indem Sie Problempunkte entlang des Netzwerkpfads vom Knoten bis zum Dienst anhand von Latenzdaten zu jedem Hop identifizieren. Mithilfe integrierter Tests überwachen Sie ganz ohne Vorkonfiguration die Konnektivität mit Office 365 und Dynamics CRM. Mit dieser Funktion können Sie die Netzwerkkonnektivität mit einem beliebigen TCP-fähigen Endpunkt überwachen (also beispielsweise mit Websites, SaaS-Anwendungen, PaaS-Anwendungen und SQL-Datenbanken).  

[ExpressRoute-Monitor:](log-analytics-network-performance-monitor-expressroute.md) Überwachen Sie die End-to-End-Konnektivität und -Leistung zwischen Ihren Zweigstellen und Azure über Azure ExpressRoute.  
 

## <a name="set-up-and-configure"></a>Einrichten und Konfigurieren

### <a name="install-and-configure-agents"></a>Installieren und Konfigurieren von Agents 

Verwenden Sie zum Installieren von Agents die grundlegenden Prozesse, die unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md) und [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md) beschrieben werden.

### <a name="where-to-install-the-agents"></a>Bestimmen des Installationsorts für die Agents 

**Systemmonitor:** Installieren Sie OMS-Agents auf mindestens einem Knoten, der mit den einzelnen Subnetzwerken verbunden ist, über die Sie die Netzwerkkonnektivität mit anderen Subnetzwerken überwachen möchten.  

Zur Überwachung einer Netzwerkverbindung müssen an beiden Endpunkten der Verbindung Agents installiert werden.  Sollten Sie sich bei der Topologie Ihres Netzwerks nicht sicher sein, installieren Sie die Agents auf Servern mit kritischen Workloads, zwischen denen Sie die Netzwerkleistung überwachen möchten. Wenn Sie also beispielsweise die Netzwerkverbindung zwischen einem Webserver und einem Server mit SQL überwachen möchten, installieren Sie auf beiden Servern einen Agent. Agents überwachen nicht die eigentlichen Hosts, sondern die Netzwerkkonnektivität (Verbindungen) zwischen Hosts. 

**Dienstendpunktmonitor:** Installieren Sie einen OMS-Agent auf jedem Knoten, von dem aus Sie die Netzwerkkonnektivität mit dem Dienstendpunkt überwachen möchten. Wenn Sie also beispielsweise die Netzwerkkonnektivität mit Office 365 für die Bürostandorte O1, O2 und O3 überwachen möchten, installieren Sie den OMS-Agent jeweils auf mindestens einem Knoten in O1, O2 und O3. 

**ExpressRoute-Monitor:** Installieren Sie mindestens einen OMS-Agent in Ihrem virtuellen Azure-Netzwerk und mindestens einen Agent in Ihrem lokalen Subnetzwerk, das über das private Peering mit ExpressRoute verbunden ist.  

### <a name="configure-oms-agents-for-monitoring"></a>Konfigurieren von OMS-Agents für die Überwachung  

Der NPM verwendet synthetische Transaktionen, um die Netzwerkleistung zwischen Quell- und Ziel-Agents zu überwachen. Bei der Überwachung im Systemmonitor und im Dienstendpunktmonitor kann zwischen dem TCP- und dem ICMP-Protokoll gewählt werden. Beim ExpressRoute-Monitor wird dagegen TCP verwendet. Stellen Sie sicher, dass die Firewall die Kommunikation zwischen den OMS-Überwachungs-Agents über das für die Überwachung gewählte Protokoll zulässt.  

**TCP-Protokoll:** Wenn Sie sich für TCP als Überwachungsprotokoll entschieden haben, öffnen Sie den Firewallport für die Agents, die für den Systemmonitor und den ExpressRoute-Monitor verwendet werden, um sicherzustellen, dass die Agents eine Verbindung miteinander herstellen können. Führen Sie hierzu in einem PowerShell-Fenster mit Administratorrechten das PowerShell-Skript „EnableRules.ps1“ ohne Parameter aus.  

Das Skript erstellt die für die Lösung erforderlichen Registrierungsschlüssel sowie Windows-Firewallregeln, die es den Agents ermöglichen, untereinander TCP-Verbindungen herzustellen. Die vom Skript erstellten Registrierungsschlüssel geben außerdem an, ob die Debugprotokolle und der Pfad zur Protokolldatei protokolliert werden sollen. Ferner definieren sie den für die Kommunikation verwendeten Agent-TCP-Port. Die Werte für diese Schlüssel werden automatisch durch das Skript festgelegt, daher sollten Sie diese Schlüssel nicht manuell ändern. Standardmäßig wird Port 8084 geöffnet. Sie können einen benutzerdefinierten Port verwenden, indem Sie im Skript den Parameter „portNumber“ angeben. Allerdings sollte auf allen Computern, auf denen das Skript ausgeführt wird, derselbe Port verwendet werden. 

>[!NOTE]
> Das Skript konfiguriert nur die lokale Windows-Firewall. Bei Verwendung einer Netzwerkfirewall müssen Sie sicherstellen, dass diese den Datenverkehr für den vom NPM verwendeten TCP-Port zulässt. 

>[!NOTE]
> Für den Dienstendpunktmonitor muss das PowerShell-Skript „EnableRules.ps1“ nicht ausgeführt werden. 

 

**ICMP-Protokoll:** Wenn Sie sich für ICMP als Überwachungsprotokoll entschieden haben, aktivieren Sie die folgenden Firewallregeln, um ICMP zuverlässig verwenden zu können: 

 
```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```
 

### <a name="configure-the-solution"></a>Konfigurieren der Projektmappe 

1. Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem Arbeitsbereich die Netzwerkleistungsmonitor-Lösung hinzu. 
2. Öffnen Sie Ihren Log Analytics-Arbeitsbereich, und klicken Sie auf die Kachel **Übersicht**.  
3. Klicken Sie auf die Kachel **Netzwerkleistungsmonitor** mit der Meldung  *Für die Lösung ist eine weitere Konfiguration erforderlich*.

    ![NPM-Kachel](media/log-analytics-network-performance-monitor/npm-config.png)

3. Auf der Seite **Setup** finden Sie die Option zum Installieren von OMS-Agents sowie zum Konfigurieren der Agents für die Überwachung in der Ansicht **Allgemeine Einstellungen**. Falls Sie bereits OMS-Agents installiert und konfiguriert haben, klicken Sie auf die Ansicht **Setup**, um die gewünschte Funktion zu konfigurieren.  

    **Ansicht „Systemmonitor“:** Wählen Sie aus, welches Protokoll für synthetische Transaktionen in der Systemmonitor-Standardregel verwendet werden soll, und klicken Sie anschließend auf „Speichern und fortfahren“. Diese Protokollauswahl gilt nur die für die vom System generierte Standardregel, und das Protokoll muss bei jeder Erstellung einer Systemmonitorregel explizit ausgewählt werden. Sie können jederzeit zu den Standardregeleinstellungen auf der Registerkarte „Systemmonitor“ wechseln und das Protokoll ändern. (Die Registerkarte „Systemmonitor“ wird nach Abschluss der Tag-0-Konfiguration angezeigt.) Sollten Sie die Systemmonitorfunktion nicht benötigen, können Sie die Standardregel über die Standardregeleinstellungen auf der Registerkarte „Systemmonitor“ deaktivieren. 

    ![NPM-Konfiguration](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
    **Ansicht „Dienstendpunktmonitor“:** Diese Funktion bietet integrierte, vorkonfigurierte Tests zur Überwachung der Netzwerkkonnektivität mit Office 365 und Dynamics 365 über Ihre Agents. Aktivieren Sie die Kontrollkästchen der Office 365- und Dynamics 365-Dienste, die Sie überwachen möchten. Klicken Sie auf die Schaltfläche „Agents hinzufügen“, um die gewünschten Agents für die Überwachung auszuwählen. Falls Sie diese Funktion nicht verwenden oder erst später einrichten möchten, können Sie diesen Schritt überspringen und direkt auf **Speichern** und **Fortfahren** klicken, ohne etwas auszuwählen.  

    ![NPM-Konfiguration](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

    **Ansicht „ExpressRoute-Monitor“:** Klicken Sie auf die Schaltfläche **Jetzt ermitteln**, um alle privaten ExpressRoute-Peerings zu ermitteln, die mit den VNets in dem Azure-Abonnement verbunden sind, das mit diesem Log Analytics-Arbeitsbereich verknüpft ist.  


    >[!NOTE] 
    > Die Lösung ermittelt derzeit nur private ExpressRoute-Peerings. 

    >[!NOTE] 
    > Es werden nur private Peerings ermittelt, die mit den VNets verbunden sind, welche dem mit diesem Log Analytics-Arbeitsbereich verknüpften Abonnement zugeordnet sind. Falls Ihre ExpressRoute-Instanz außerhalb des mit diesem Workspace verknüpften Abonnements mit VNets verbunden ist, müssen Sie einen Log Analytics-Arbeitsbereich in den entsprechenden Abonnements erstellen und diese Peerings mithilfe des NPM überwachen. 

    ![NPM-Konfiguration](media/log-analytics-network-performance-monitor/npm-express-route.png)

    Nach Abschluss der Ermittlung werden die ermittelten privaten Peerings in einer Tabelle aufgeführt.  

    ![NPM-Konfiguration](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
    Die Überwachung für diese Peerings ist zunächst deaktiviert. Klicken Sie auf die einzelnen Peerings, die Sie überwachen möchten, und konfigurieren Sie die Überwachung jeweils in der Detailansicht auf der rechten Seite.  Klicken Sie auf die Schaltfläche „Speichern“, um die Konfiguration zu speichern. Weitere Informationen finden Sie unter [Konfigurieren der ExpressRoute-Überwachung]().  

    Nach Abschluss der Einrichtung dauert es zwischen 30 Minuten und einer Stunde, bis die Daten aufgefüllt wurden. Während die Lösung Daten aus Ihrem Netzwerk aggregiert, wird auf der NPM-Übersichtskachel *Für die Lösung ist eine weitere Konfiguration erforderlich.* angezeigt. Nachdem die Daten gesammelt und indiziert wurden, informiert die Übersichtskachel über die allgemeine Integrität Ihres Netzwerks. Daraufhin können Sie die Überwachung der Knoten, auf denen OMS-Agents installiert sind, sowie die Subnetze, die in Ihrer Umgebung ermittelt wurden, bearbeiten. 

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Bearbeiten von Überwachungseinstellungen für Subnetze und Knoten 

Alle Subnetze mit mindestens einem installierten Agent werden auf der Registerkarte „Subnetzwerke“ der Konfigurationsseite aufgeführt. 


So aktivieren bzw. deaktivieren Sie die Überwachung bestimmter Subnetzwerke 

1. Aktivieren bzw. deaktivieren Sie das Kontrollkästchen neben der  **Subnetzwerk-ID** , und vergewissern Sie sich anschließend, dass  **Zur Überwachung verwenden** je nach Bedarf aktiviert oder deaktiviert ist. Sie können mehrere Subnetze aktivieren oder deaktivieren. Deaktivierte Subnetzwerke werden nicht überwacht, da die Agents dahingehend aktualisiert werden, dass sie keine Pings anderer Agents mehr ausführen. 
2. Wählen Sie die Knoten aus, die Sie in einem bestimmten Subnetzwerk überwachen möchten. Wählen Sie hierzu in der Liste das gewünschte Subnetzwerk aus, und verschieben Sie die erforderlichen Knoten zwischen den Listen mit den nicht überwachten Knoten und den überwachten Knoten. Sie können dem Subnetzwerk eine benutzerdefinierte **Beschreibung** hinzufügen. 
3. Klicken Sie zum Speichern der Konfiguration auf **Speichern**. 

#### <a name="choose-nodes-to-monitor"></a>Auswählen zu überwachender Knoten

Alle Knoten, auf denen ein Agent installiert ist, werden auf der Registerkarte **Knoten** aufgeführt. 

1. Aktivieren bzw. deaktivieren Sie die Knoten, die überwacht werden sollen bzw. deren Überwachung beendet werden soll. 
2. Aktivieren bzw. deaktivieren Sie  **Zur Überwachung verwenden** (je nach Bedarf). 
3. Klicken Sie auf **Speichern**. 


Konfigurieren Sie die gewünschten Funktionen: 
- [Systemmonitor](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Dienstendpunktmonitor](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Details zur Datensammlung
Der Netzwerkleistungsmonitor verwendet TCP SYN-SYNACK-ACK-Handshakepakete, wenn „TCP“ als Protokoll zum Erfassen von Informationen zu Verlusten und zur Latenz ausgewählt ist, und „ICMP ECHO ICMP ECHO REPLY“, wenn „ICMP“ ausgewählt ist. Außerdem wird Traceroute zum Abrufen von Topologieinformationen verwendet.

Die folgende Tabelle zeigt Datensammlungsmethoden und andere Details, wie Daten für den Netzwerkleistungsmonitor gesammelt werden, an.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-Handshakes/ICMP ECHO-Nachrichten werden alle fünf Sekunden gesendet, Daten alle drei Minuten |
 

 
Die Lösung nutzt synthetische Transaktionen, um die Integrität des Netzwerks zu bewerten. OMS-Agents, die an verschiedenen Punkten im Netzwerk installiert sind, tauschen TCP- oder ICMP Echo-Pakete (je nach dem für die Überwachung ausgewählten Protokoll) miteinander aus. Dabei erhalten Agents Informationen zur Roundtripzeit und ggf. zu Paketverlust. Jeder Agent führt zudem in regelmäßigen Abständen eine Routenverfolgung (Traceroute) zu anderen Agents aus, um alle zu testenden Routen im Netzwerk zu ermitteln. Anhand dieser Daten können die Agents die Netzwerklatenz und die Paketverluste herleiten. Die Tests werden alle fünf Sekunden wiederholt, und die Daten werden von den Agents vor dem Hochladen zum Log Analytics-Dienst für einen Zeitraum von drei Minuten aggregiert. 



>[!NOTE]
> Obwohl die Agents häufig miteinander kommunizieren, generieren sie bei den Tests keinen nennenswerten Netzwerkdatenverkehr. Agents bedienen sich lediglich der TCP SYN-SYNACK-ACK-Handshakepakete, um die Datenverluste und die Latenz zu ermitteln. Sie tauschen keine Datenpakete aus. Bei diesem Vorgang kommunizieren die Agents nur bei Bedarf miteinander, und die Kommunikationstopologie der Agents ist optimiert, um den Netzwerkdatenverkehr zu reduzieren.

## <a name="using-the-solution"></a>Verwenden der Lösung 

### <a name="npm-overview-tile"></a>NPM-Übersichtskachel 

Nach dem Aktivieren der Netzwerkleistungsmonitor-Lösung wird auf der Kachel der Lösung auf der Übersichtsseite eine kurze Übersicht über die Integrität des Netzwerks angezeigt. 

 ![NPM-Übersichtskachel](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Dashboard des Netzwerkleistungsmonitors 

Die Seite **Top-Netzwerkintegritätsereignisse** enthält eine Liste mit den aktuellen Integritätsereignissen und Warnungen im System – einschließlich der Angabe, seit wann das Ereignis aktiv ist. Ein Integritätsereignis oder eine Warnung wird generiert, wenn der Wert der ausgewählten Metrik (Verlust, Wartezeit, Antwortzeit oder Bandbreitennutzung) für die Überwachungsregel den Schwellenwert überschreitet. 

Auf der Seite  **Systemmonitor** wird die Integrität der durch die Lösung überwachten Netzwerk- und Subnetzwerkverbindungen zusammengefasst. Die Topologiekachel informiert über die Anzahl von Netzwerkpfaden, die in Ihrem Netzwerk überwacht werden. Durch Klicken auf diese Kachel gelangen Sie direkt zur Topologieansicht. 

Auf der Seite  **Dienstendpunktmonitor** wird die Integrität der verschiedenen erstellten Tests zusammengefasst. Auf der Topologiekachel erfahren Sie, wie viele Endpunkte überwacht werden. Durch Klicken auf diese Kachel gelangen Sie direkt zur Topologieansicht.

Auf der Seite  **ExpressRoute-Monitor** wird die Integrität der verschiedenen ExpressRoute-Peeringverbindungen zusammengefasst, die durch die Lösung überwacht werden. Die Topologiekachel informiert über die Anzahl von Netzwerkpfaden durch die ExpressRoute-Verbindungen, die in Ihrem Netzwerk überwacht werden. Durch Klicken auf diese Kachel gelangen Sie direkt zur Topologieansicht.

Die Seite  **Allgemeine Abfragen** enthält einen Satz von Suchabfragen zum direkten Abrufen von Rohdaten für die Netzwerküberwachung. Sie können diese Abfragen als Ausgangspunkt für das Erstellen eigener Abfragen für benutzerdefinierte Berichte verwenden. 

![NPM-Dashboard](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Drilldown für mehr Tiefe 

Sie können auf dem Lösungsdashboard auf verschiedene Verbindungen klicken, um zu den für Sie relevanten Bereichen einen Drilldown auszuführen. Wenn beispielsweise auf dem Dashboard eine Warnung oder eine fehlerhafte Netzwerkverbindung angezeigt wird, können Sie darauf klicken, um diesen Umstand näher zu untersuchen. Sie werden zu einer Seite weitergeleitet, auf der alle Subnetzwerkverbindungen für die betreffende Netzwerkverbindung aufgeführt werden. Sie sehen den Verlust, die Latenz und den Integritätsstatus aller Subnetzwerkverbindungen und können schnell feststellen, welche Subnetzwerkverbindungen das Problem verursachen. Klicken Sie anschließend auf  **Knotenverbindungen anzeigen** , um alle Knotenverbindungen für die fehlerhafte Subnetzverbindung anzuzeigen. Nun sehen Sie die einzelnen Knoten-zu-Knoten-Verbindungen und können die fehlerhaften Knotenverbindungen ausfindig machen. 

Klicken Sie auf  **Topologie anzeigen** , um die Hop-by-Hop-Topologie der Routen zwischen den Quell- und den Zielknoten anzuzeigen. Die fehlerhaften Routen werden rot dargestellt, und Sie können die Wartezeit nach Hop anzeigen, sodass Sie das Problem schnell einem bestimmten Teil des Netzwerks zuordnen können. 

 

### <a name="network-state-recorder"></a>Network State Recorder 

Jede Ansicht zeigt eine Momentaufnahme der Netzwerkintegrität zu einem bestimmten Zeitpunkt. Standardmäßig wird der aktuelle Status angezeigt. Die Leiste am oberen Rand der Seite zeigt den Zeitpunkt, für den der Status angezeigt wird. Sie können in der Zeit zurückgehen und die Momentaufnahme der Netzwerkintegrität anzeigen, indem Sie auf der Leiste auf „Aktionen“ klicken. Sie können auch die automatische Aktualisierung für eine beliebige Seite aktivieren oder deaktivieren, während Sie den aktuellen Status anzeigen. 

 ![Network State Recorder](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Trenddiagramme 

Auf jeder Drilldownebene können Sie sich den Trend der entsprechenden Metrik (Verlust, Wartezeit, Antwortzeit, Bandbreitennutzung) ansehen. Mit dem Zeitsteuerelement am oberen Rand des Diagramms können Sie das Zeitintervall für den Trend ändern. 

Trenddiagramme zeigen die Leistung einer Leistungsmetrik im historischen Kontext an. Einige Netzwerkprobleme sind vorübergehender Natur und lassen sich durch einen Blick auf den aktuellen Status des Netzwerks nur schwer erkennen. Derartige Probleme können kurzfristig auftreten und wieder verschwinden, bevor sie bemerkt werden, um dann zu einem späteren Zeitpunkt erneut aufzutreten. Solche vorübergehenden Probleme können auch Anwendungsadministratoren Schwierigkeiten bereiten, da sie sich häufig in einem unerklärlichen Anstieg der Reaktionszeit einer Anwendung niederschlagen, obgleich alle Anwendungskomponenten einwandfrei zu funktionieren scheinen. 

Sie können derartige Probleme unverzüglich erkennen, wenn Sie sich ein Trenddiagramm anschauen, in dem das Problem als plötzliche Spitze bei der Netzwerklatenz oder beim Paketverlust zu sehen ist. Daraufhin können Sie das Problem mithilfe des Network State Recorders untersuchen, um die Netzwerkmomentaufnahme und die Topologie für den Zeitpunkt anzuzeigen, zu dem das Problem aufgetreten ist. 

 
![Trenddiagramme](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topologiekarte 

Der NPM zeigt die Hop-by-Hop-Topologie von Routen zwischen dem Quell- und dem Zielendpunkt auf einer interaktiven Topologiekarte an. Die Topologiekarte können Sie anzeigen, indem Sie auf dem Lösungsdashboard auf die Kachel **Topologie** oder auf den Drilldownseiten auf den Link **Topologie anzeigen** klicken.  

Die Topologiekarte zeigt, wie viele Routen zwischen Quelle und Ziel vorhanden sind und welche Pfade die Datenpakete nehmen. Außerdem wird die Wartezeit pro Netzwerkhop angezeigt. Alle Pfade, bei denen die Gesamtwartezeit des Pfads über dem Schwellenwert liegt (gemäß der entsprechenden Überwachungsregel), werden rot dargestellt.  

Wenn Sie in der Topologiekarte auf einen Knoten klicken oder darauf zeigen, sehen Sie die Eigenschaften des Knotens, z.B. der FQDN und die IP-Adresse. Klicken Sie auf einen Hop, um die dazugehörige IP-Adresse anzuzeigen. Den problematischen Netzwerkhop erkennen Sie an der Wartezeit, die er verursacht. Sie können bestimmte Routen mithilfe der Filter im reduzierbaren Aktionsbereich filtern. Sie können auch die Netzwerktopologien vereinfachen, indem Sie mithilfe des Schiebereglers im Aktionsbereich die zwischenzeitlichen Hops ausblenden. Mit dem Mausrad können Sie die Topologiekarte vergrößern oder verkleinern. 

Beachten Sie, dass es sich bei der in der Karte gezeigten Topologie um eine Layer 3-Topologie handelt, die keine Layer 2-Geräte und -Verbindungen enthält. 

 
![Topologiekarte](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Log Analytics-Suche 

Alle im NPM-Dashboard und auf den Drilldownseiten grafisch dargestellten Daten sind auch nativ über die [Log Analytics-Suche](log-analytics-log-search-new.md) verfügbar. Sie können Daten im Repository interaktiv analysieren, Daten aus verschiedenen Quellen korrelieren, benutzerdefinierte Warnungen und Ansichten erstellen sowie die Daten in Excel oder Power BI exportieren oder einen Link für die Weitergabe generieren. Der Bereich „Allgemeine Abfragen“ im Dashboard enthält einige praktische Abfragen, die Sie als Ausgangspunkt für die Erstellung eigener Abfragen und Berichte verwenden können. 

 

## <a name="provide-feedback"></a>Feedback geben 

**UserVoice** – Sie können Ihre Ideen zu Funktionen des Netzwerkleistungsmonitors veröffentlichen, an denen wir arbeiten sollen. Besuchen Sie unsere  [UserVoice-Seite](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

**Treten Sie unserer Gruppe bei** : Wir freuen uns über neue Kunden, die unserer Gruppe beitreten möchten. Als Teil der Gruppe erhalten Sie vorab Zugriff auf neue Funktionen und helfen uns dabei, den Netzwerkleistungsmonitor zu verbessern. Wenn Sie beitreten möchten, füllen Sie bitte diesen  [kurzen Fragebogen](https://aka.ms/npmcohort) aus. 

## <a name="next-steps"></a>Nächste Schritte 
- Informieren Sie sich ausführlicher über [Systemmonitor](log-analytics-network-performance-monitor-performance-monitor.md), [Dienstendpunktmonitor](log-analytics-network-performance-monitor-performance-monitor.md) und [ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md). 

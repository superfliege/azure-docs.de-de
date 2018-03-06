---
title: "Systemmonitorfeature in der Netzwerkleistungsmonitor-Lösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Die Systemmonitorfunktion im Netzwerkleistungsmonitor unterstützt Sie beim Überwachen der Netzwerkkonnektivität für verschiedene Punkte in Ihrem Netzwerk – beispielsweise für Cloudbereitstellungen und lokale Standorte, für mehrere Rechenzentren und Zweigstellen sowie für unternehmenswichtige Microservices/Anwendungen mit mehreren Ebenen."
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
ms.openlocfilehash: a90ab3bc857b704d9d94daf96d17611844abb1a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution---performance-monitoring"></a>Netzwerkleistungsmonitor-Lösung – Leistungsüberwachung

Die Systemmonitorfunktion im [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md) unterstützt Sie beim Überwachen der Netzwerkkonnektivität für verschiedene Punkte in Ihrem Netzwerk – beispielsweise für Cloudbereitstellungen und lokale Standorte, für mehrere Rechenzentren und Zweigstellen sowie für unternehmenswichtige Microservices/Anwendungen mit mehreren Ebenen. Mithilfe des Systemmonitors können Sie Netzwerkprobleme erkennen, bevor sie Ihre Benutzer beinträchtigen. Zentrale Vorteile: 

- Überwachen von Verlusten und Wartezeiten in verschiedenen Subnetzen und Festlegen von Warnungen 
- Überwachen aller Pfade (einschließlich der redundanten) des Netzwerks 
- Behandeln von vorübergehenden Netzwerkproblemen und Point-in-Time-Netzwerkproblemen, die schwer reproduzierbar sind 
- Ermitteln des spezifischen Netzwerksegments, das für die Leistungsbeeinträchtigung verantwortlich ist 
- Überwachen der Integrität des Netzwerks ohne SNMP 


![Netzwerkleistungsmonitor](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Konfiguration
Öffnen Sie die [Netzwerkleistungsmonitor-Lösung](log-analytics-network-performance-monitor.md), und klicken Sie auf die Schaltfläche **Konfigurieren**, um die Konfiguration für den Netzwerkleistungsmonitor zu öffnen.

![Konfigurieren des Netzwerkleistungsmonitors](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Erstellen neuer Netzwerke

Bei einem Netzwerk im NPM handelt es sich um einen logischen Container für Subnetze. Damit können Sie Ihre Netzwerkinfrastruktur gemäß Ihren Überwachungsanforderungen strukturieren. Sie können ein Netzwerk mit einem Anzeigenamen erstellen und entsprechend Ihrer Geschäftslogik Subnetze hinzufügen. So können Sie beispielsweise ein Netzwerk mit dem Namen „London“ erstellen und alle Subnetze aus Ihrem Londoner Rechenzentrum hinzufügen, oder ein Netzwerk mit dem Namen *ContosoFrontEnd* erstellen und diesem Netzwerk alle Subnetze hinzufügen, die das Front-End Ihrer App namens „Contoso“ bedienen. Die Lösung erstellt automatisch ein Standardnetzwerk mit allen Subnetzen, die in Ihrer Umgebung ermittelt wurden. Immer dann, wenn Sie ein Netzwerk erstellen, fügen Sie diesem ein Subnetz hinzu, und dieses Subnetz wird aus dem Standardnetzwerk entfernt. Wenn Sie ein Netzwerk löschen, werden alle seine Subnetze automatisch an das Standardnetzwerk zurückgegeben. Das Netzwerk „Standard“ ist also der Container für alle Subnetze, die nicht in einem benutzerdefinierten Netzwerk enthalten sind. Sie können das Standardnetzwerk weder bearbeiten noch löschen. Es verbleibt stets im System. Sie können jedoch beliebig viele benutzerdefinierte Netzwerke erstellen. In den meisten Fällen sind die Subnetze in Ihrer Organisation in mehreren Netzwerken angeordnet. Es ist ratsam, mindestens ein Netzwerk zu erstellen, um Ihre Subnetze gemäß Ihrer Geschäftslogik zu gruppieren.

So erstellen Sie ein neues Netzwerk:


1. Klicken Sie auf die Registerkarte **Netzwerke**.
1. Klicken Sie auf  **Netzwerk hinzufügen**, und geben Sie den Netzwerknamen und eine Beschreibung ein. 
2. Wählen Sie ein oder mehrere Subnetze aus, und klicken Sie anschließend auf **Hinzufügen**. 
3. Klicken Sie zum Speichern der Konfiguration auf **Speichern**. 


### <a name="create-monitoring-rules"></a>Erstellen von Überwachungsregeln 

Der Systemmonitor generiert Integritätsereignisse, wenn der Schwellenwert der Leistung von Netzwerkverbindungen zwischen zwei Subnetzwerken oder zwischen zwei Netzwerken überschritten wird. Diese Schwellenwerte können automatisch vom System erlernt werden, oder Sie können benutzerdefinierte Schwellenwerte festlegen. Das System erstellt automatisch eine Standardregel, die immer dann ein Integritätsereignis generiert, wenn der Verlust oder die Wartezeit zwischen einem Netzwerk-/Subnetzwerkverbindungspaar den vom System erlernten Schwellenwert überschreitet. Dies erleichtert der Lösung die Überwachung Ihrer Netzwerkinfrastruktur, solange Sie noch nicht alle Überwachungsregeln explizit erstellt haben. Wenn die **Standardregel** aktiviert ist, senden alle Knoten synthetische Transaktionen an alle anderen Knoten, die Sie für die Überwachung aktiviert haben. Die Standardregel ist bei kleinen Netzwerken nützlich – etwa in einem Szenario, in dem auf einer kleinen Anzahl von Servern ein Microservice ausgeführt wird und Sie die Konnektivität aller Server miteinander sicherstellen möchten. 

>[!NOTE]
> Es wird dringend empfohlen, die **Standardregel** zu deaktivieren und benutzerdefinierte Überwachungsregeln zu erstellen – insbesondere bei großen Netzwerken, in denen Sie sehr viele Knoten für die Überwachung verwenden. Dies reduziert den von der Lösung generierten Datenverkehr und hilft Ihnen, die Überwachung Ihres Netzwerks zu organisieren. 

Erstellen Sie Überwachungsregeln gemäß Ihrer Geschäftslogik. Wenn Sie etwa die Leistung der Netzwerkkonnektivität zwischen zwei Bürostandorten und dem Unternehmenshauptsitz überwachen möchten, gruppieren Sie alle Subnetze am Bürostandort 1 im Netzwerk „O1“, alle Subnetze am Bürostandort 2 im Netzwerk „O2“ und alle Subnetze des Unternehmenshauptsitzes im Netzwerk „H“. Erstellen Sie zwei Überwachungsregeln: eine für die Verbindung zwischen „O1“ und „H“ und die andere für die Verbindung zwischen „O2“ und „H“. 

So erstellen Sie benutzerdefinierte Überwachungsregeln:

1. Klicken Sie auf der Registerkarte **Überwachen** auf **Regel hinzufügen**, und geben Sie den Regelnamen und eine Beschreibung ein. 
2. Wählen Sie das zu überwachende Netzwerk- oder Subnetzwerkverbindungspaar aus den Listen aus. 
3. Wählen Sie zunächst aus der Dropdownliste „Netzwerk“ das Netzwerk aus, in dem das erste relevante Subnetz bzw. die ersten relevanten Subnetze enthalten ist/sind. Wählen Sie anschließend das Subnetzwerk bzw. die Subnetzwerke in der entsprechenden Dropdownliste „Subnetzwerke“ aus. Wählen Sie **Alle Subnetzwerke** aus, wenn alle Subnetzwerke in einer Netzwerkverbindung überwacht werden sollen. Wählen Sie analog die übrigen relevanten Subnetzwerke aus. Ferner können Sie auf **Ausnahme hinzufügen** klicken, um die Überwachung bestimmter Subnetzwerkverbindungen aus Ihrer Auswahl zu entfernen. 
4. Bei der Ausführung synthetischer Transaktionen können Sie zwischen den Protokollen ICMP und TCP wählen. 
5. Wenn keine Integritätsereignisse für die von Ihnen ausgewählten Elemente erstellt werden sollen, deaktivieren Sie das Kontrollkästchen **Integritätsüberwachung für die mit dieser Regel abgedeckten Verbindungen aktivieren**. 
6. Wählen Sie Überwachungsbedingungen aus. Sie können benutzerdefinierte Schwellenwerte für das Generieren von Integritätsereignissen festlegen, indem Sie Schwellenwerte eingeben. Sobald der Wert einer Bedingung den für Sie festgelegten Schwellenwert für das ausgewählte Netzwerk-/Subnetzwerkpaar überschreitet, wird ein Integritätsereignis generiert. 
7. Klicken Sie zum Speichern der Konfiguration auf **Speichern**. 

Nachdem Sie eine Überwachungsregel gespeichert haben, können Sie diese Regel in die Warnungsverwaltung integrieren, indem Sie auf **Warnung erstellen** klicken. Beim Erstellen einer Warnungsregel werden die Suchabfrage und andere erforderliche Parameter automatisch ausgefüllt. Mithilfe einer Warnungsregel können Sie neben den vorhandenen Warnungen in NPM auch E-Mail-basierte Warnungen erhalten. Warnungen können auch Problembehandlungsaktionen mit Runbooks auslösen oder mithilfe von Webhooks in vorhandene Dienstverwaltungslösungen integriert werden. Zum Bearbeiten der Warnungseinstellungen klicken Sie auf die Option zum **Verwalten von Warnungen**. 

Nun können weitere Systemmonitorregeln erstellen oder zum Dashboard der Lösung wechseln, um die Funktion zu verwenden. 

### <a name="choose-the-protocol"></a>Auswählen des Protokolls

Die Funktion „Netzwerkleistungsüberwachung“ (Network Performance Monitor, NPM) nutzt synthetische Transaktionen zum Berechnen von Netzwerkleistungmetriken wie Paketverlust und Linklatenz. Um dies besser zu verstehen, sollten Sie einen NPM-Agent mit einem Ende einer Netzwerkverbindung verbinden. Dieser NPM-Agent sendet Testpakete an einen zweiten NPM-Agent, der mit einem anderen Ende des Netzwerks verbunden ist. Der zweite Agent antwortet mit Antwortpaketen. Dieser Prozess wird einige Male wiederholt. Durch Messen der Anzahl von Antworten und Empfangszeit jeder Antwort kann der erste NPM-Agent die Linklatenz und Paketverluste messen. 

Format, Größe und Reihenfolge dieser Pakete richten sich nach dem Protokoll, das Sie bei der Erstellung von Überwachungsregeln auswählen. Je nach Protokoll der Pakete können die zwischengeschalteten Netzwerkgeräte (Router, Switches usw.) diese Pakete möglicherweise unterschiedlich verarbeiten. Folglich wirkt sich Ihre Protokollauswahl auf die Genauigkeit der Ergebnisse aus. Zudem bestimmt Ihre Wahl des Protokolls auch, ob Sie nach Bereitstellen der NPM-Lösung manuelle Schritte ausführen müssen. 

NPM bietet Ihnen für die Ausführung synthetischer Transaktionen die Wahl zwischen den Protokollen ICMP und TCP. Wenn Sie beim Erstellen einer synthetischen Transaktionsregel ICMP wählen, verwenden die NPM-Agents ICMP ECHO-Nachrichten zum Berechnen von Netzwerklatenz und Paketverlust. ICMP ECHO verwendet dieselbe Nachricht, die vom gängigen Hilfsprogramm Ping gesendet wird. Bei Verwenden von TCP als Protokoll senden NPM-Agents ein TCP SYN-Paket über das Netzwerk. Darauf folgt ein TCP-Handshake bis zum Abschluss und das anschließende Entfernen der Verbindung mithilfe von RST-Paketen. 

Ehe Sie ein Protokoll wählen, sollten Sie die folgenden Informationen berücksichtigen: 

**Ermittlung mehrerer Netzwerkrouten:**  TCP ist genauer, wenn mehrere Routen ermitteln werden, und benötigt weniger Agents in jedem Subnetz. Beispielsweise können ein oder zwei Agents, die TCP verwenden, alle redundanten Pfade zwischen Subnetzen ermitteln. Bei Verwenden von ICMP benötigen Sie mehr Agents, um dasselbe Resultat zu erzielen. Wenn Sie bei Verwendung von ICMP über eine Reihe von Routen zwischen zwei Subnetzen verfügen, benötigen Sie mehr als 5N Agents in einem Quell- oder Zielsubnetz. 

**Genauigkeit der Ergebnisse:** Router und Switches weisen ICMP ECHO-Paketen im Vergleich mit TCP-Paketen tendenziell eine niedrigere Priorität zu. Wenn Netzwerkgeräte in bestimmten Situationen stark ausgelastet sind, geben die von TCP abgerufenen Daten den Paketverlust und die Latenz von Anwendungen präziser zurück. Dies erfolgt, da der meiste Datenverkehr über TCP übertragen wird. In solchen Fällen bietet ICMP im Vergleich mit TCP ungenauere Ergebnisse. 

**Firewallkonfiguration:** Das TCP-Protokoll erfordert, dass TCP-Pakete an einen Zielport gesendet werden. Der von NPM-Agents verwendete Standardport ist 8084, den Sie beim Konfigurieren von Agents jedoch ändern können. Sie müssen daher sicherstellen, dass Ihre Netzwerkfirewalls bzw. NSG-Regeln (Netzwerksicherheitsgruppen) Datenverkehr an diesem Port zulassen. Sie müssen auch sicherstellen, dass die lokale Firewall auf den Computern, auf denen Agents installiert sind, für das Zulassen von Datenverkehr an diesem Port konfiguriert ist. Sie können zum Konfigurieren von Firewallregeln auf Ihren Computern Windows PowerShell-Skripts verwenden. Die Netzwerkfirewall müssen Sie allerdings manuell konfigurieren. Im Gegensatz dazu arbeitet ICMP nicht mit einem Port. In den meisten Unternehmensumgebungen ist ICMP-Datenverkehr durch die Firewalls zulässig, damit Sie Netzwerkdiagnosetools wie das Hilfsprogramm Ping einsetzen können. Wenn Sie also einen Computer von einem anderen aus pingen können, lässt sich das ICMP-Protokoll nutzen, ohne dass Firewalls manuell konfiguriert werden müssen. 

>[!NOTE] 
> Einige Firewalls blockieren möglicherweise ICMP, was zur Neuübertragung führen kann, sodass eine große Anzahl von Ereignissen in Ihrem Sicherheitsinformations- und Ereignisverwaltungssystem auftritt. Stellen Sie sicher, dass das gewählte Protokoll nicht durch eine Netzwerkfirewall/NSG blockiert wird, da das Netzwerksegment andernfalls nicht überwacht werden kann. Aus diesem Grund sollten Sie TCP für die Überwachung verwenden. Sie sollten ICMP in Szenarien verwenden, wo Sie TCP nicht verwenden können, z.B.: 
>
> - Wenn Sie clientbasierte Windows-Knoten verwenden, da TCP-RAW-Sockets im Windows-Client nicht zulässig sind
> - Wenn Ihre Netzwerkfirewall/NSG TCP blockiert
> - Informationen zum Wechseln des Protokolls 

Wenn Sie sich während der Bereitstellung für ICMP entscheiden, können Sie jederzeit zu TCP wechseln, indem Sie die Standardüberwachungsregel bearbeiten:

1. Navigieren Sie zu **Netzwerkleistung** > **Monitor** > **Konfigurieren** > **Monitor**, und klicken Sie auf  **Standardregel**. 
2. Scrollen Sie zum Abschnitt **Protokoll**, und wählen Sie das Protokoll, das Sie verwenden möchten. 
3. Klicken Sie auf **Speichern**, um die Einstellung zu übernehmen. 

Auch wenn die Standardregel ein bestimmtes Protokoll angibt, können Sie neue Regeln mit einem anderen Protokoll erstellen. Sie können sogar eine Kombination aus Regeln erstellen, bei der einige der Regeln ICMP und andere TCP verwenden. 

## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

Nachdem Sie nun mit dem Systemmonitor vertraut sind, erfahren Sie in diesem Abschnitt anhand eines Beispiels, wie Sie auf einfache Weise die Ursachen eines Integritätsereignisses untersuchen.  

Auf dem Lösungsdashboard ist ein Integritätsereignis vorhanden: ein fehlerhafter Netzwerklink. Sie beschließen, das Problem zu untersuchen, und klicken dazu auf die Kachel **Überwachte Netzwerkverbindungen**.

Auf der Drilldownseite stellen Sie fest, dass die Netzwerkverbindung **DMZ2-DMZ1** fehlerhaft ist. Sie klicken auf den Link zum Anzeigen der **Subnetzwerkverbindungen** für diese Netzwerkverbindung. 


Auf der Drilldownseite werden alle Subnetzwerkverbindungen der Netzwerkverbindung **DMZ2-DMZ1** angezeigt. Sie sehen, dass die Latenz beider Subnetzwerkverbindungen den Schwellenwert überschritten hat und die Netzwerkverbindung somit fehlerhaft ist. Sie können auch die Latenztrends der beiden Subnetzwerkverbindungen anzeigen. Mit dem Zeitauswahl-Steuerelement im Diagramm können Sie sich auf den gewünschten Zeitraum konzentrieren. Sie können sehen, zu welcher Tageszeit die Latenz ihren Höchstwert erreicht hat. Sie können anschließend die Protokolle für diesen Zeitraum durchgehen, um das Problem zu untersuchen. Klicken Sie auf **Knotenverbindungen anzeigen**, um einen weiteren Drilldown auszuführen. 
 
 ![Subnetzwerkverbindungen](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Ähnlich wie bei der vorherigen Seite werden auf der Drilldownseite der Subnetzwerkverbindung die enthaltenen Knotenverbindungen aufgeführt. Hier können Sie ähnliche Aktionen ausführen wie im vorherigen Schritt. Klicken Sie auf **Topologie anzeigen**, um die Topologie zwischen den beiden Knoten anzuzeigen. 
 
 ![Knotenverbindungen](media/log-analytics-network-performance-monitor/node-links.png) 

Auf der Topologiekarte werden alle Pfade zwischen den beiden ausgewählten Knoten dargestellt. Sie können die Hop-by-Hop-Topologie der Routen zwischen zwei Knoten in der Topologiekarte grafisch darstellen. So wissen Sie genau, wie viele Routen es zwischen den beiden Knoten gibt und welche Pfade die Datenpakete verwenden. Leistungsengpässe im Netzwerk werden rot gekennzeichnet. Sie finden eine fehlerhafte Netzwerkverbindung bzw. ein fehlerhaftes Netzwerkgerät, indem Sie in der Topologiekarte nach rot gekennzeichneten Elementen suchen. 

 ![Topologiedashboard](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

Verlust, Latenz und Anzahl der Hops in jedem Pfad können im Bereich **Aktion** überprüft werden. Verwenden Sie die Bildlaufleiste, um die Details dieser fehlerhaften Pfade anzuzeigen. Verwenden Sie die Filter, um die Pfade mit dem fehlerhaften Hop auszuwählen, sodass die Topologie nur für die ausgewählten Pfade dargestellt wird. Mit dem Mausrad können Sie die Topologiekarte vergrößern oder verkleinern. 

In der folgenden Abbildung ist anhand der rot gekennzeichneten Pfade und Hops deutlich erkennbar, wo die Ursache für die Problembereiche im betreffenden Abschnitt des Netzwerks liegt. Durch Klicken auf einen Knoten in der Topologiekarte werden die Eigenschaften des Knotens einschließlich FQDN und IP-Adresse angezeigt. Durch Klicken auf einen Hop wird die IP-Adresse des Hops angezeigt. 
 
![Topologiedashboard](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Nächste Schritte
* Mit [Protokollsuchen](log-analytics-log-searches.md) können Sie Detaildatensätze mit Netzwerkleistungsdaten anzeigen.

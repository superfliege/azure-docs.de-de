---
title: Netzwerkleistungsmonitor-Lösung in Azure Log Analytics | Microsoft-Dokumentation
description: Verwenden Sie den Dienstkonnektivitätsmonitor von Netzwerkleistungsmonitor, um die Netzwerkkonnektivität an einem beliebigen Endpunkt zu überwachen, der über einen geöffneten TCP-Port verfügt.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: ''
ms.openlocfilehash: fb84b20630eb63cb53ccb1d13a383ed6287b802b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406618"
---
# <a name="service-connectivity-monitor"></a>Dienstkonnektivitätsmonitor

Mit dem Dienstkonnektivitätsmonitor von [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md) können Sie die Netzwerkkonnektivität an einem beliebigen Endpunkt überwachen, der über einen geöffneten TCP-Port verfügt. Zu solchen Endpunkten zählen Websites, SaaS-Anwendungen, PaaS-Anwendungen und SQL-Datenbanken. 

Der Dienstkonnektivitätsmonitor ermöglicht Folgendes: 

- Überwachen der Netzwerkkonnektivität Ihrer Anwendungen und Netzwerkdienste von mehreren Zweigstellen oder Standorten. Zu den Anwendungen und Netzwerkdiensten gehören Office 365, Dynamics CRM, interne Line-of-Business-Anwendungen und SQL-Datenbanken.
- Ausführen integrierter Tests zur Überwachung der Netzwerkkonnektivität mit Office 365- und Dynamics 365-Endpunkten 
- Ermitteln von Antwortzeiten, Netzwerkwartezeiten und Paketverlusten beim Herstellen der Verbindung mit dem Endpunkt
- Ermitteln, ob eine mangelhafte Anwendungsleistung auf das Netzwerk oder auf ein Problem beim Anwendungsanbieter zurückzuführen ist
- Erkennen von Hotspots im Netzwerk, die möglicherweise die Leistung der Anwendung beeinträchtigen, durch Betrachtung der Hop-spezifischen Wartezeiten auf einer Topologiekarte


![Dienstkonnektivitätsmonitor](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Konfiguration 
Öffnen Sie die [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md)-Lösung, und wählen Sie die Schaltfläche **Konfigurieren** aus, um die Konfiguration für den Netzwerkleistungsmonitor zu öffnen.

![Konfigurieren des Netzwerkleistungsmonitors](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-log-analytics-agents-for-monitoring"></a>Konfigurieren von Log Analytics-Agents für die Überwachung
Aktivieren Sie auf den für die Überwachung verwendeten Knoten die folgenden Firewallregeln, damit die Lösung die Topologie zwischen Ihren Knoten und dem Dienstendpunkt ermitteln kann: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Erstellen von Tests für den Dienstkonnektivitätsmonitor 

Beginnen Sie mit der Erstellung Ihrer Tests zur Überwachung der Netzwerkkonnektivität mit den Dienstendpunkten.

1. Klicken Sie auf die Registerkarte **Dienstkonnektivitätsmonitor**.
2. Klicken Sie auf **Test hinzufügen**, und geben Sie einen Namen und eine Beschreibung für den Test ein. 
3. Wählen Sie die Art des Tests aus:<br>

    * Wählen Sie **Web** aus, wenn Sie die Konnektivität mit einem Dienst überwachen möchten, der auf HTTP/S-Anforderungen reagiert, etwa outlook.office365.com oder bing.com.<br>
    * Wählen Sie **Netzwerk** aus, wenn Sie die Konnektivität mit einem Dienst überwachen möchten, der auf TCP-Anforderungen, aber nicht auf HTTP/S-Anforderungen reagiert, etwa einem SQL-Server, FTP-Server oder SSH-Port. 
4. Wenn Sie keine Netzwerkmessung durchführen möchten, z.B. von Netzwerklatenz, Paketverlusten oder Topologieermittlung, deaktivieren Sie das Kontrollkästchen **Netzwerkmessungen durchführen**. Zur optimalen Nutzung der Funktion sollten Sie das Kontrollkästchen aktiviert lassen. 
5. Geben Sie unter **Ziel** die URL/den FQDN/die IP-Adresse ein, für die bzw. den Sie die Netzwerkkonnektivität überwachen möchten.
6. Geben Sie unter **Portnummer** die Portnummer des Zieldiensts ein. 
7. Geben Sie unter **Testhäufigkeit** einen Wert dafür ein, wie häufig der Test ausgeführt werden soll. 
8. Wählen Sie die Knoten aus, von denen aus Sie die Netzwerkkonnektivität mit dem Dienst überwachen möchten. 

    >[!NOTE]
    > Bei serverbasierten Windows-Knoten verwendet die Funktion TCP-basierte Anforderungen für die Netzwerkmessungen. Bei clientbasierten Windows-Knoten verwendet die Funktion ICMP-basierte Anforderungen für die Netzwerkmessungen. In einigen Fällen blockiert die Zielanwendung eingehende ICMP-basierte Anforderungen, wenn die Knoten auf Windows-Clients basieren. Die Lösung kann keine Netzwerkmessungen ausführen. Für solche Fälle empfiehlt sich die Verwendung auf Windows Server basierender Knoten. 

9. Wenn keine Integritätsereignisse für die ausgewählten Elemente erstellt werden sollen, deaktivieren Sie **Enable Health Monitoring in the targets covered by this test** (Integritätsüberwachung für die von diesem Test abgedeckten Ziele aktivieren). 
10. Wählen Sie Überwachungsbedingungen aus. Sie können benutzerdefinierte Schwellenwerte für das Generieren von Integritätsereignissen festlegen, indem Sie Schwellenwerte eingeben. Sobald der Wert einer Bedingung den festgelegten Schwellenwert für das ausgewählte Netzwerk- oder Subnetzpaar überschreitet, wird ein Integritätsereignis generiert. 
11. Wählen Sie zum Speichern der Konfiguration **Speichern** aus. 

    ![Testkonfigurationen für den Dienstkonnektivitätsmonitor](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

Wechseln Sie zur Dashboardansicht des Netzwerkleistungsmonitors. Auf der Seite **Dienstkonnektivitätsmonitor** erhalten Sie eine Zusammenfassung der Integrität der verschiedenen erstellten Tests. 

![Seite „Dienstkonnektivitätsmonitor“](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Wählen Sie die Kachel aus, um Details zu den Tests auf der Seite **Tests** anzuzeigen. Die linke Tabelle gibt Aufschluss über die Integrität und die Werte der Dienstreaktionszeit, der Netzwerkwartezeit und der Paketverluste für alle Tests zu einem bestimmten Zeitpunkt. Mit dem Steuerelement „Network State Recorder“ können Sie die Ansicht der Netzwerkmomentaufnahme für einen anderen Punkt in der Vergangenheit anzeigen. Wählen Sie in der Tabelle den Test aus, den Sie untersuchen möchten. Die Diagramme auf der rechten Seite geben Aufschluss über den historischen Trend von Verlusten, Wartezeiten und Antwortzeiten. Wählen Sie den Link **Testdetails** aus, um die Leistung für die einzelnen Knoten anzuzeigen.

![Dienstkonnektivitätsmonitor-Tests](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

In der Ansicht **Testknoten** können Sie die Netzwerkkonnektivität der einzelnen Knoten beobachten. Wählen Sie den Knoten aus, dessen Leistung beeinträchtigt ist. Dies ist der Knoten, an dem eine nicht zufriedenstellende Anwendungsgeschwindigkeit festgestellt wurde.

Ermitteln Sie, ob die mangelhafte Anwendungsleistung auf das Netzwerk oder auf ein Problem beim Anwendungsanbieter zurückzuführen ist. Untersuchen Sie hierzu die Korrelation zwischen der Anwendungsantwortzeit und der Netzwerkwartezeit. 

* **Anwendungsproblem:** Eine Spitze bei der Antwortzeit bei konsistenter Netzwerkwartezeit deutet darauf hin, dass das Netzwerk einwandfrei funktioniert und möglicherweise ein Problem mit der Anwendung vorliegt. 

    ![Anwendungsproblem beim Dienstkonnektivitätsmonitor](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

* **Netzwerkproblem:** Eine Spitze bei der Antwortzeit in Verbindung mit einer entsprechenden Spitze bei der Netzwerkwartezeit deutet darauf hin, dass die längere Antwortzeit auf eine erhöhte Netzwerkwartezeit zurückzuführen ist. 

    ![Netzwerkproblem beim Dienstkonnektivitätsmonitor](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Nachdem Sie festgestellt haben, dass das Problem auf das Netzwerk zurückzuführen ist, wählen Sie den Ansichtslink **Topologie** aus, um den problematischen Hop auf der Topologiekarte zu ermitteln. In der folgenden Abbildung ist ein Beispiel angegeben. Von der Gesamtwartezeit von 105 ms zwischen Knoten und Anwendungsendpunkt entfallen 96 ms auf den rot markierten Hop. Nachdem Sie den problematischen Hop identifiziert haben, können Sie Korrekturmaßnahmen ergreifen. 

![Dienstkonnektivitätsmonitor-Tests](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnose 

Gehen Sie im Fall einer Anomalie wie folgt vor:

* Falls Antwortzeit, Netzwerkverlust und Wartezeit des Diensts als nicht verfügbar angezeigt werden, können folgende Gründe vorliegen:

    - Die Anwendung ist nicht verfügbar.
    - Der Knoten, der zum Überprüfen der Netzwerkkonnektivität mit dem Dienst verwendet wird, ist nicht verfügbar.
    - In die Testkonfiguration wurde ein falsches Ziel eingegeben.
    - Der Knoten verfügt nicht über Netzwerkkonnektivität.

* Falls eine gültige Antwortzeit des Diensts angezeigt wird, aber keine Angaben zu Netzwerkverlust und Wartezeit verfügbar sind, können folgende Gründe vorliegen:

    - Wenn es sich bei dem Knoten, der zum Überprüfen der Netzwerkkonnektivität mit dem Dienst verwendet wird, um einen Windows-Clientcomputer handelt, blockiert der Zieldienst möglicherweise ICMP-Anforderungen, oder eine Netzwerkfirewall blockiert ICMP-Anforderungen des Knotens.
    - Das Kontrollkästchen **Netzwerkmessungen durchführen** ist in der Testkonfiguration deaktiviert. 

* Falls die Antwortzeit des Diensts als nicht verfügbar angezeigt wird, aber gültige Angaben zu Netzwerkverlust und Wartezeit verfügbar sind, ist der Zieldienst möglicherweise keine Webanwendung. Bearbeiten Sie die Testkonfiguration, und wählen Sie für den Testtyp **Netzwerk** anstelle von **Web** aus. 

* Ermitteln Sie im Fall einer langsamen Anwendung, ob die mangelhafte Anwendungsleistung auf das Netzwerk oder auf ein Problem beim Anwendungsanbieter zurückzuführen ist.


## <a name="next-steps"></a>Nächste Schritte
Mit [Protokollsuchen](log-analytics-log-searches.md) können Sie Detaildatensätze mit Netzwerkleistungsdaten anzeigen.

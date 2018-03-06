---
title: "Netzwerkleistungsmonitor-Lösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Mit der Dienstendpunktmonitor-Funktion des Netzwerkleistungsmonitors können Sie die Netzwerkkonnektivität mit einem beliebigen Endpunkt überwachen, der über einen geöffneten TCP-Port verfügt."
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
ms.openlocfilehash: ba19a4fc24668bff27c961b5b415f840d1132a34
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="service-endpoint-monitor"></a>Dienstendpunktmonitor

Mit der Dienstendpunktmonitor-Funktion im [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md) können Sie die Netzwerkkonnektivität mit einem beliebigen Endpunkt überwachen, der über einen geöffneten TCP-Port verfügt. Zu solchen Endpunkten zählen Websites, SaaS-Anwendungen, PaaS-Anwendungen und SQL-Datenbanken. 

Der **Dienstendpunktmonitor** ermöglicht Folgendes: 

- Überwachen der Netzwerkkonnektivität mit Ihren Anwendungen und Netzwerkdiensten (wie etwa Office 365, Dynamics CRM, interne Branchenanwendungen, SQL-Datenbank und Ähnliches) über mehrere Zweigstellen/Standorte 
- Ausführen integrierter Tests zur Überwachung der Netzwerkkonnektivität mit Office 365- und Dynamics 365-Endpunkten 
- Bestimmen von Antwortzeit/Netzwerkwartezeit/Paketverlusten beim Herstellen der Verbindung mit dem Endpunkt 
- Bestimmen, ob eine mangelhafte Anwendungsleistung auf das Netzwerk oder auf ein Problem beim Anwendungsanbieter zurückzuführen ist 
- Erkennen von Hotspots im Netzwerk, die möglicherweise die Leistung der Anwendung beeinträchtigen, durch Betrachtung der Hop-spezifischen Wartezeiten auf einer Topologiekarte 


![Dienstendpunktmonitor](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Konfiguration 
Öffnen Sie die [Netzwerkleistungsmonitor-Lösung](log-analytics-network-performance-monitor.md), und klicken Sie auf die Schaltfläche **Konfigurieren**, um die Konfiguration für den Netzwerkleistungsmonitor zu öffnen.

![Konfigurieren des Netzwerkleistungsmonitors](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-oms-agents-for-the-monitoring"></a>Konfigurieren Sie OMS-Agents für die Überwachung.  
Aktivieren Sie auf den für die Überwachung verwendeten Knoten die folgenden Firewallregeln, damit die Lösung die Topologie zwischen Ihren Knoten und dem Dienstendpunkt ermitteln kann: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action 
```

### <a name="create-service-endpoint-monitor-tests"></a>Erstellen von Dienstendpunktmonitor-Tests 

Beginnen Sie mit der Erstellung Ihrer Tests zur Überwachung der Netzwerkkonnektivität mit den Dienstendpunkten: 

1. Klicken Sie auf die Registerkarte **Dienstendpunktmonitor**.
2. Klicken Sie auf **Test hinzufügen**, und geben Sie einen Namen und eine Beschreibung für den Test ein. 
3. Wählen Sie die Art des Tests aus:<br>**Webtest:** Wählen Sie diese Option aus, wenn Sie die Konnektivität mit einem Dienst überwachen möchten, der auf HTTP/S-Anforderungen reagiert (wie etwa outlook.office365.com und bing.com).<br>**Netzwerktest:** Wählen Sie diese Option aus, wenn Sie die Konnektivität mit einem Dienst überwachen möchten, der auf TCP-Anforderungen, aber nicht auf HTTP/S-Anforderungen reagiert (beispielsweise ein SQL-Server, ein FTP-Server, ein SSH-Port oder Ähnliches). 
4. Wenn Sie keine Netzwerkmessungen (Netzwerklatenz, Paketverlust, Topologieerkennung) durchführen möchten, deaktivieren Sie das entsprechende Kontrollkästchen. Zur optimalen Nutzung der Funktion empfiehlt es sich jedoch, das Kontrollkästchen aktiviert zu lassen. 
5. Geben Sie die Ziel-URL/den Ziel-FQDN/die Ziel-IP-Adresse ein, für die bzw. für den Sie die Netzwerkkonnektivität überwachen möchten.  
6. Geben Sie die Portnummer des Zieldiensts ein. 
7. Geben Sie die gewünschte Häufigkeit für die Testausführung ein. 
8. Wählen Sie die Knoten aus, von denen aus Sie die Netzwerkkonnektivität mit dem Dienst überwachen möchten. 

    >[!NOTE]
    > Bei serverbasierten Windows-Knoten verwendet die Funktion TCP-basierte Anforderungen für die Netzwerkmessungen. Bei clientbasierten Windows-Knoten verwendet die Funktion ICMP-basierte Anforderungen für die Netzwerkmessungen. Manchmal blockiert die Zielanwendung eingehende ICMP-basierte Anforderungen, was bei clientbasierten Windows-Knoten dazu führt, dass die Lösung keine Netzwerkmessungen durchführen kann. In solchen Fällen empfiehlt sich die Verwendung serverbasierter Windows-Knoten. 

9. Wenn keine Integritätsereignisse für die von Ihnen ausgewählten Elemente erstellt werden sollen, deaktivieren Sie das Kontrollkästchen **Integritätsüberwachung für die von diesem Test abgedeckten Ziele aktivieren**. 
10. Wählen Sie Überwachungsbedingungen aus. Sie können benutzerdefinierte Schwellenwerte für das Generieren von Integritätsereignissen festlegen, indem Sie Schwellenwerte eingeben. Sobald der Wert einer Bedingung den für Sie festgelegten Schwellenwert für das ausgewählte Netzwerk-/Subnetzwerkpaar überschreitet, wird ein Integritätsereignis generiert. 
11. Klicken Sie zum Speichern der Konfiguration auf **Speichern**. 

 ![Konfiguration des Dienstendpunktmonitors](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

Wechseln Sie zur Dashboardansicht des Netzwerkleistungsmonitors. Die Seite **Dienstendpunktmonitor** enthält eine Integritätszusammenfassung für die verschiedenen Tests, die Sie erstellt haben.  

![Seite „Dienstendpunktmonitor“](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Klicken Sie auf die Kachel, um Detailinformationen zu den Tests auf der Seite **Tests** anzuzeigen. Die linke Tabelle gibt Aufschluss über die Integrität und die Dienstreaktionszeit, die Netzwerkwartezeit und die Paketverluste für alle Tests zu einem bestimmten Zeitpunkt. Mit dem Steuerelement „Network State Recorder“ können Sie die Ansicht der Netzwerkmomentaufnahme für einen anderen Punkt in der Vergangenheit anzeigen. Klicken Sie in der Tabelle auf den Test, den Sie untersuchen möchten. Die Diagramme auf der rechten Seite geben Aufschluss über den historischen Trend von Verlusten, Wartezeiten und Antwortzeiten. Klicken Sie auf den Link „Testdetails“, um die Leistung für die einzelnen Knoten anzuzeigen. 

![Dienstendpunktmonitor-Tests](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

In der Ansicht **Testknoten** können Sie die Netzwerkkonnektivität der einzelnen Knoten beobachten. Klicken Sie auf den Knoten, bei dem eine Leistungsbeeinträchtigung vorliegt.  Dies ist der Knoten, von dem aus eine nicht zufriedenstellende Anwendungsgeschwindigkeit festgestellt wurde. 

Bestimmen Sie, ob die mangelhafte Anwendungsleistung auf das Netzwerk oder auf ein Problem beim Anwendungsanbieter zurückzuführen ist. Untersuchen Sie hierzu die Korrelation zwischen der Anwendungsantwortzeit und der Netzwerkwartezeit. 

**Anwendungsproblem:** Falls eine Spitze bei der Antwortzeit vorliegt, die Netzwerkwartezeit aber konsistent bleibt, deutet dies darauf hin, dass das Netzwerk einwandfrei funktioniert und ein Problem mit der Anwendung vorliegt.  

![Dienstendpunktmonitor: Anwendungsproblem](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

**Netzwerkproblem:** Falls eine Spitze bei der Antwortzeit von einer entsprechenden Spitze bei der Netzwerkwartezeit begleitet wird, deutet dies darauf hin, dass die erhöhte Antwortzeit auf eine erhöhte Netzwerkwartezeit zurückzuführen ist.  

![Dienstendpunktmonitor: Netzwerkproblem](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Nachdem Sie zu dem Schluss gekommen sind, dass das Problem auf das Netzwerk zurückzuführen ist, klicken Sie auf den Ansichtslink **Topologie**, um den problematischen Hop auf der Topologiekarte zu ermitteln. In der folgenden Abbildung sehen Sie beispielsweise, dass von der Gesamtwartezeit von 105 ms zwischen Knoten und Anwendungsendpunkt 96 ms auf den rot markierten Hop entfallen. Nachdem Sie den problematischen Hop identifiziert haben, können Sie Korrekturmaßnahmen ergreifen.  

![Dienstendpunktmonitor-Tests](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnose 

Gehen Sie im Falle einer Anomalie wie folgt vor:

Falls Antwortzeit, Netzwerkverlust und Wartezeit des Diensts als nicht verfügbar angezeigt werden, liegt unter Umständen mindestens einer der folgenden Gründe vor:
- Die Anwendung ist nicht verfügbar.
- Der Knoten, der zum Überprüfen der Netzwerkkonnektivität mit dem Dienst verwendet wird, ist nicht verfügbar.
- In die Testkonfiguration wurde ein falsches Ziel eingegeben.
- Der Knoten verfügt über keine Netzwerkkonnektivität.

Falls eine gültige Antwortzeit des Diensts angezeigt wird, aber keine Angaben zu Netzwerkverlust und Wartezeit verfügbar sind, liegt unter Umständen mindestens einer der folgenden Gründe vor:
- Wenn es sich bei dem Knoten, der zum Überprüfen der Netzwerkkonnektivität mit dem Dienst verwendet wird, um einen Windows-Clientcomputer handelt, blockiert der Zieldienst unter Umständen ICMP-Anforderungen, oder eine Netzwerkfirewall blockiert ICMP-Anforderungen des Knotens.
- In der Testkonfiguration wurde das Kontrollkästchen **Netzwerkmessungen durchführen** deaktiviert. 

Falls die Antwortzeit des Diensts als nicht verfügbar angezeigt wird, aber gültige Angaben zu Netzwerkverlust und Wartezeit verfügbar sind, ist der Zieldienst wahrscheinlich keine Webanwendung. Bearbeiten Sie die Testkonfiguration, und wählen Sie für den Testtyp anstelle von „Webtest“ die Option „Netzwerktest“ aus. 

Im Falle einer langsamen Anwendung sollten Sie ermitteln, ob die mangelhafte Anwendungsleistung auf das Netzwerk oder auf ein Problem beim Anwendungsanbieter zurückzuführen ist.


## <a name="next-steps"></a>Nächste Schritte
* Mit [Protokollsuchen](log-analytics-log-searches.md) können Sie Detaildatensätze mit Netzwerkleistungsdaten anzeigen.

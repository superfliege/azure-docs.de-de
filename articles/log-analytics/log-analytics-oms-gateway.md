---
title: Verbinden von Computern über das OMS-Gateway | Microsoft-Dokumentation
description: Verbinden Sie Geräte und durch Operations Manager überwachte Computer mit dem OMS-Gateway, um Daten an den Azure Automation- und Log Analytics-Dienst zu senden, wenn sie nicht über Internetzugriff verfügen.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: magoedte
ms.openlocfilehash: 66e5444f5346a44cfc8a43cf2b43dbaeacffedc9
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="connect-computers-without-internet-access-by-using-the-oms-gateway"></a>Verbinden von Computern ohne Internetzugriff über das OMS-Gateway
In diesem Dokument erfahren Sie, wie Sie die Kommunikation mit Azure Automation and Log Analytics über das OMS-Gateway konfigurieren, wenn direkt verbundene oder durch Operations Manager überwachte Computer nicht über Internetzugriff verfügen. Das OMS-Gateway ist ein HTTP-Forwardproxy, der HTTP-Tunnel mit dem Befehl „HTTP CONNECT“ unterstützt. Es kann Daten sammeln und im Namen der Computer ohne Internetzugriff an Azure Automation und Log Analytics senden.  

Das OMS-Gateway unterstützt Folgendes:

* Azure Automation – Hybrid-Runbook-Worker  
* Direkt mit einem Log Analytics-Arbeitsbereich verbundene Windows-Computer mit Microsoft Monitoring Agent
* Direkt mit einem Log Analytics-Arbeitsbereich verbundene Linux-Computer mit dem OMS-Agent für Linux  
* System Center Operations Manager 2012 SP1 mit UR7, Operations Manager 2012 R2 mit UR3, Operations Manager 2016 sowie die in Log Analytics integrierte Verwaltungsgruppe der Operations Manager-Version 1801  

Falls bestimmte Computer in Ihrem Netzwerk aufgrund von IT-Sicherheitsrichtlinien keine Verbindung mit dem Internet herstellen können (etwa im Fall von Point-of-Sale-Geräten oder von Servern, die IT-Dienste unterstützen) und Sie zu deren Verwaltung und Überwachung eine Verbindung mit Azure Automation oder Log Analytics herstellen möchten, können sie so konfiguriert werden, dass sie direkt mit dem OMS-Gateway kommunizieren.

 Wenn diese Computer mit dem OMS-Agent konfiguriert werden, um eine direkte Verbindung mit einem Log Analytics-Arbeitsbereich herzustellen, kommunizieren alle Computer stattdessen mit dem OMS-Gateway. Das OMS-Gateway überträgt Daten von den Agents direkt an den Dienst. Dabei werden keinerlei Daten analysiert.

Wenn eine Operations Manager-Verwaltungsgruppe in Log Analytics integriert wird, können die Verwaltungsserver so konfiguriert werden, dass sie eine Verbindung mit dem OMS-Gateway herstellen, um Konfigurationsinformationen zu empfangen und gesammelte Daten zu senden. Operations Manager-Agents senden einige Daten wie Operations Manager-Warnungen, die Konfigurationsbewertung, den Speicherplatz der Instanz sowie Kapazitätsdaten an den Verwaltungsserver. Andere umfangreiche Daten wie IIS-Protokolle, Leistungsinformationen und Sicherheitsereignisse werden direkt an das OMS-Gateway gesendet.  

Falls Sie zur Überwachung nicht vertrauenswürdiger Systeme einen oder mehrere Operations Manager-Gatewayserver in einer DMZ oder in einem anderen isolierten Netzwerk bereitgestellt haben, können diese nicht mit einem OMS-Gateway kommunizieren. Operations Manager-Gatewayservers können nur mit einem Verwaltungsserver kommunizieren. 

Wenn eine Operations Manager-Verwaltungsgruppe für die Kommunikation mit dem OMS-Gateway konfiguriert ist, werden die Proxykonfigurationsinformationen automatisch an alle per Agent verwalteten Computer verteilt, die zum Sammeln von Daten für Log Analytics konfiguriert sind. Das gilt auch, wenn die Einstellung leer ist.    

Zur Gewährleistung der Hochverfügbarkeit für direkt verbundene Gruppen oder Operations Management-Gruppen, die über das OMS-Gateway mit Log Analytics kommunizieren, können Sie den Datenverkehr mithilfe des Netzwerklastenausgleichs umleiten und auf mehrere OMS-Gatewayserver verteilen. Wenn ein Gatewayserver ausfällt, wird der Datenverkehr an einen anderen verfügbaren Knoten umgeleitet.  

Es empfiehlt sich, den OMS-Agent auf dem Computer zu installieren, auf dem die OMS-Gatewaysoftware ausgeführt wird, um ihn zu überwachen und die Leistungs- oder Ereignisdaten zu analysieren. Außerdem unterstützt der Agent das OMS-Gateway beim Ermitteln der Dienstendpunkte, mit denen es kommunizieren muss.

Alle Agents benötigen eine Netzwerkverbindung mit ihrem jeweiligen Gateway, um die automatische Übertragung von Daten zwischen Agent und Gateway zu ermöglichen. Das Gateway sollte nicht auf einem Domänencontroller installiert werden.

Die folgende Abbildung zeigt den Datenfluss von direkten Agents an Azure Automation und Log Analytics unter Verwendung des Gatewayservers. In der Proxykonfiguration des Agents muss der Port verwendet werden, den das OMS-Gateway für die Kommunikation mit dem Dienst verwendet.  

![Diagramm zur direkten Kommunikation von Agents mit Diensten](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Die folgende Abbildung zeigt den Datenfluss von einer Operations Manager-Verwaltungsgruppe an Log Analytics.   

![Abbildung der Kommunikation von Operations Manager mit Log Analytics](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Voraussetzungen

Ein für die Ausführung des OMS-Gateways vorgesehener Computer muss über folgende Komponenten verfügen:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET Framework 4.5
* Vierkernprozessor und 8 GB Arbeitsspeicher (mindestens) 

### <a name="language-availability"></a>Verfügbare Sprachen

Das OMS-Gateway ist in folgenden Sprachen verfügbar:

- Chinesisch (vereinfacht)
- Chinesisch (traditionell)
- Tschechisch
- Niederländisch
- Deutsch
- Französisch
- Deutsch
- Ungarisch
- Italienisch
- Japanisch
- Koreanisch
- Polnisch
- Portugiesisch (Brasilien)
- Portugiesisch (Portugal)
- Russisch
- Spanisch (international)

### <a name="supported-encryption-protocols"></a>Unterstützte Verschlüsselungsprotokolle
Das OMS-Gateway unterstützt nur Transport Layer Security (TLS) 1.0, 1.1 und 1.2.  Secure Sockets Layer (SSL) wird nicht unterstützt.

### <a name="supported-number-of-agent-connections"></a>Unterstützte Anzahl von Agent-Verbindungen
Die folgende Tabelle gibt Aufschluss über die unterstützte Anzahl von Agents, die mit einem Gatewayserver kommunizieren. Diese Unterstützung basiert auf Agents, die alle sechs Sekunden etwa 200 KB an Daten hochladen. Das Datenvolumen pro getestetem Agent beträgt etwa 2,7 GB pro Tag.

|Gateway |Ungefähre Anzahl unterstützter Agents|  
|--------|----------------------------------|  
|- CPU: Intel XEON CPU E5-2660 v3 mit 2,6 GHz und zwei Kernen<br> - Arbeitsspeicher: 4 GB<br> - Netzwerkbandbreite: 1 GBit/s| 600|  
|- CPU: Intel XEON CPU E5-2660 v3 mit 2,6 GHz und vier Kernen<br> - Arbeitsspeicher: 8 GB<br> - Netzwerkbandbreite: 1 GBit/s| 1000|  

## <a name="download-the-oms-gateway"></a>Herunterladen des OMS-Gateways

Die aktuelle Version der Setupdatei für das OMS-Gateway kann auf zwei Arten bezogen werden:

1. Laden Sie sie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443) herunter.

2. Laden Sie sie über das Azure-Portal herunter. Gehen Sie nach der Anmeldung beim Azure-Portal wie folgt vor:  

   1. Durchsuchen Sie die Liste mit den Diensten, und wählen Sie **Log Analytics** aus.  
   2. Wählen Sie einen Arbeitsbereich aus.
   3. Klicken Sie auf dem Blatt für Ihren Arbeitsbereich unter **Allgemein** auf **Schnellstart**.
   4. Wählen Sie unter **Datenquelle zum Herstellen einer Verbindung mit dem Arbeitsbereich auswählen** die Option **Computer** aus.
   5. Klicken Sie im Bereich **Direkt-Agent** auf **OMS-Gateway herunterladen**.
   
    ![Herunterladen des OMS-Gateways](./media/log-analytics-oms-gateway/download-gateway.png)

Alternative: 

   1. Klicken Sie in Ihrem Arbeitsbereichblatt unter **Einstellungen** auf **Erweiterte Einstellungen**.
   2. Klicken Sie auf **Verbundene Quellen** > **Windows-Server**. Klicken Sie auf **OMS-Gateway herunterladen**.

## <a name="install-the-oms-gateway"></a>Installieren des OMS-Gateways

Führen Sie die folgenden Schritte aus, um ein Gateway zu installieren. Sollte die Vorgängerversion (ehemals *Log Analytics Forwarder*) installiert sein, wird ein Upgrade auf die aktuelle Version ausgeführt.  

1. Klicken Sie im Zielordner auf **OMS Gateway.msi**.
2. Wählen Sie auf der Seite **Willkommen** die Option **Weiter** aus.

 ![Gateway-Setup-Assistent](./media/log-analytics-oms-gateway/gateway-wizard01.png)
  
3. Klicken Sie auf der Seite **Lizenzvertrag** auf **Ich stimme den Bedingungen des Lizenzvertrags zu**. Klicken Sie anschließend auf **Weiter**.

4. Gehen Sie auf der Seite für die **Port- und Proxyadresse** wie folgt vor:

   a. Geben Sie die für das Gateway zu verwendende TCP-Portnummer ein. Das Setup konfiguriert eine Eingangsregel mit dieser Portnummer für die Windows-Firewall. Der Standardwert ist 8080. Zulässig sind Portnummern im Bereich zwischen 1 und 65535. Wenn die Eingabe außerhalb dieses Bereichs liegt, wird eine Fehlermeldung angezeigt.

   b. Optional: Falls der Server, auf dem das Gateway installiert ist, über einen Proxy kommunizieren muss, geben Sie den Proxy ein, mit dem das Gateway eine Verbindung herstellen soll. Ein Beispiel wäre `http://myorgname.corp.contoso.com:80`, wie auf dem folgenden Screenshot zu sehen. Wenn Sie dieses Feld leer lassen, versucht das Gateway, eine direkte Verbindung mit dem Internet herzustellen.  Falls der Proxyserver eine Authentifizierung erfordert, geben Sie einen Benutzernamen und ein Kennwort ein.
   
    ![Gateway-Assistent – Proxykonfiguration](./media/log-analytics-oms-gateway/gateway-wizard02.png)

4. Klicken Sie auf **Weiter**.

5. Falls Microsoft Update nicht aktiviert ist, wird die Seite „Microsoft Update“ angezeigt, auf der Sie Microsoft Update aktivieren können. Treffen Sie eine Auswahl, und klicken Sie anschließend auf **Weiter**. Fahren Sie andernfalls mit dem nächsten Schritt fort.

6. Übernehmen Sie auf der Seite **Zielordner** entweder den Standardordner **C:\Programme\OMS Gateway**, oder geben Sie den gewünschten Installationsort für das OMS-Gateway ein. Klicken Sie anschließend auf **Weiter**.

7. Wählen Sie auf der Seite **Bereit zur Installation** die Option **Installieren** aus. Unter Umständen fordert die Benutzerkontensteuerung eine Installationsberechtigung an. Sollte eine Berechtigung angefordert werden, klicken Sie auf **Ja**.

8. Klicken Sie nach Abschluss des Setups auf **Fertig stellen**. Sie können überprüfen, ob der Dienst ausgeführt wird. Öffnen Sie dazu das Snap-In „services.msc“, und vergewissern Sie sich, dass **OMS-Gateway** in der Liste mit den Diensten angezeigt wird und den Status **Wird ausgeführt** besitzt.

    ![Dienste – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Konfigurieren des Netzwerklastenausgleichs 
Sie können das Gateway mithilfe des Netzwerklastenausgleichs (Network Load Balancing, NLB) für Hochverfügbarkeit konfigurieren. Verwenden Sie entweder den Netzwerklastenausgleich von Microsoft oder einen hardwarebasierten Lastenausgleich.  Der Lastenausgleich leitet die angeforderten Verbindungen von OMS-Agents oder Operations Manager-Verwaltungsservern zur Verwaltung des Datenverkehrs für alle Knoten um. Sollte ein Gatewayserver ausfallen, wird der Datenverkehr zu anderen Knoten umgeleitet.

Informationen zum Entwerfen und Bereitstellen eines Netzwerklastenausgleichs-Clusters unter Windows Server 2016 finden Sie unter [Netzwerklastenausgleich](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  In den folgenden Schritten wird das Konfigurieren eines Clusters mit Microsoft-Netzwerklastenausgleich beschrieben.  

1.  Melden Sie sich mit einem Administratorkonto bei dem Windows-Server an, der dem NLB-Cluster angehört.  

2.  Öffnen Sie im Server-Manager den Netzwerklastenausgleich-Manager. Wählen Sie **Tools** und anschließend **Netzwerklastenausgleich-Manager** aus.

3. Um einen OMS-Gatewayserver mit dem installierten Microsoft Monitoring Agent zu verbinden, klicken Sie mit der rechten Maustaste auf die IP-Adresse des Clusters, und klicken Sie dann auf **Host dem Cluster hinzufügen**.

 ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen](./media/log-analytics-oms-gateway/nlb02.png)

4. Geben Sie die IP-Adresse des Gatewayservers ein, mit dem Sie eine Verbindung herstellen möchten.

    ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen: Verbinden](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-the-oms-agent-and-the-operations-manager-management-group"></a>Konfigurieren des OMS-Agents und der Operations Manager-Verwaltungsgruppe
In den folgenden Abschnitten dieses Artikels erfahren Sie, wie Sie direkt verbundene OMS-Agents, eine Operations Manager-Verwaltungsgruppe oder Azure Automation Hybrid Runbook Worker mit dem OMS-Gateway konfigurieren, um die Kommunikation mit Azure Automation und Log Analytics zu ermöglichen.  

Informationen zu den Anforderungen für die Installation des OMS-Agents auf Windows-Computern, die direkt mit Log Analytics verbunden sind, sowie entsprechende Installationsschritte finden Sie unter [Sammeln von Daten von Computern in Ihrer Umgebung mit Log Analytics](log-analytics-concept-hybrid.md#prerequisites).

 Informationen für Linux-Computer finden Sie unter [Sammeln von Daten von einem Linux-Computer, der in Ihrer Umgebung gehostet wird](log-analytics-quick-collect-linux-computer.md). Informationen zum Automation Hybrid Runbook Worker finden Sie unter [Bereitstellen von Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-the-standalone-oms-agent"></a>Konfigurieren des eigenständigen OMS-Agents
Informationen zum Konfigurieren eines Agents für die Verwendung eines Proxyservers (bei dem es sich in diesem Fall um das Gateway handelt) finden Sie unter [Sammeln von Daten von Computern in Ihrer Umgebung mit Log Analytics](log-analytics-concept-hybrid.md#prerequisites). Wenn Sie mehrere Gatewayserver hinter einem Netzwerklastenausgleich bereitgestellt haben, handelt es sich bei der OMS-Agent-Proxykonfiguration um die virtuelle IP-Adresse des NLBs:

![Microsoft Monitoring Agenteigenschaften – Proxyeinstellungen](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager-all-agents-use-the-same-proxy-server"></a>Konfigurieren von Operations Manager: Alle Agents verwenden den gleichen Proxyserver.
Sie konfigurieren Operations Manager, um den Gatewayserver hinzuzufügen.  Die Operations Manager-Proxykonfiguration wird automatisch auf alle Agents angewendet, die Operations Manager unterstellt sind. Das gilt auch, wenn die Einstellung leer ist.  

Wenn Sie das OMS-Gateway zur Unterstützung von Operations Manager verwenden möchten, benötigen Sie folgende Komponenten:

* Auf dem Gateway-Server muss Microsoft Monitoring Agent (Agent-Version: **8.0.10900.0** oder höher) installiert und für die Log Analytics-Arbeitsbereiche konfiguriert sein, mit denen Sie kommunizieren möchten.

* Ein Gateway mit Internetkonnektivität oder Verbindung mit einem Proxyserver, der über Internetkonnektivität verfügt.

> [!NOTE]
> Wenn Sie keinen Wert für das Gateway angeben, werden mithilfe von Push leere Werte an alle Agents übertragen.
> 

Wenn sich Ihre Operations Manager-Verwaltungsgruppe zum ersten Mal bei einem Log Analytics-Arbeitsbereich registriert, ist die Option zum Angeben der Proxykonfiguration für die Verwaltungsgruppe in der Betriebskonsole nicht verfügbar. 

Die Verwaltungsgruppe muss erfolgreich beim Dienst registriert werden. Erst dann wird diese Option verfügbar. Aktualisieren Sie die Systemproxykonfiguration mithilfe von Netsh in dem System, in dem Sie die Betriebskonsole ausführen, sowie auf allen Verwaltungsservern in der Verwaltungsgruppe.

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten.

    a. Navigieren Sie zu **Start**. Geben Sie **cmd** ein.
    
    b. Klicken Sie mit der rechten Maustaste auf **Eingabeaufforderung**. Klicken Sie auf **Als Administrator ausführen**.
    
2. Geben Sie den folgenden Befehl ein, und drücken Sie die**EINGABETASTE**:

    `netsh winhttp set proxy <proxy>:<port>`

Nach Abschluss der Log Analytics-Integration können Sie die Änderung durch Ausführen von `netsh winhttp reset proxy` entfernen. Geben Sie als Nächstes mithilfe der Option **Proxyserver konfigurieren** in der Betriebskonsole den OMS-Gatewayserver an. 

1. Öffnen Sie die Operations Manager-Konsole. Klicken Sie unter **Operations Management Suite** auf **Verbindung**. Klicken Sie anschließend auf **Proxyserver konfigurieren**.

    ![Operations Manager: Proxyserver konfigurieren](./media/log-analytics-oms-gateway/scom01.png)

2. Wählen Sie **Proxyserver für Zugriff auf Operations Management Suite verwenden** aus. Geben Sie dann die IP-Adresse des OMS-Gatewayservers oder die virtuelle IP-Adresse des Netzwerklastenausgleichs ein. Beginnen Sie mit dem Präfix `http://`.

    ![Operations Manager: Proxyserveradresse](./media/log-analytics-oms-gateway/scom02.png)

3. Wählen Sie **Fertig stellen** aus. Ihre Operations Manager-Verwaltungsgruppe ist nun so konfiguriert, dass sie über den Gatewayserver mit dem Log Analytics-Dienst kommuniziert.

### <a name="configure-operations-manager-specific-agents-use-proxy-server"></a>Konfigurieren von Operations Manager: Bestimmte Agents verwenden einen Proxyserver.
Bei großen oder komplexen Umgebungen soll der OMS-Gatewayserver unter Umständen nur von bestimmten Servern (oder Gruppen) verwendet werden. Für diese Server können Sie den Operations Manager-Agent nicht direkt aktualisieren, da dieser Wert durch den globalen Wert für die Verwaltungsgruppe überschrieben wird. Setzen Sie stattdessen die Regel außer Kraft, die verwendet wird, um diese Werte mithilfe von Push zu übertragen.  

> [!NOTE] 
> Mit der gleichen Konfigurationsmethode können Sie die Verwendung mehrerer OMS-Gatewayserver in Ihrer Umgebung ermöglichen. So können beispielsweise spezifische OMS-Gatewayserver für einzelne Regionen angegeben werden.
>  

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Erstellen** aus.

2. Klicken Sie im Arbeitsbereich „Erstellen“ auf **Regeln**. Klicken Sie dann auf der Operations Manager-Symbolleiste auf die Schaltfläche **Bereich**. Sollte diese Schaltfläche nicht verfügbar sein, vergewissern Sie sich, dass im Bereich **Überwachung** kein Ordner, sondern ein Objekt ausgewählt ist. Im Dialogfeld **Management Pack-Objekte in Bereiche einteilen** wird eine Liste mit allgemeinen Zielklassen, Gruppen oder Objekten angezeigt. 

3. Geben Sie **im Suchfeld**den Suchbegriff **Integritätsdienst** ein. Wählen Sie ihn in der Liste aus. Klicken Sie auf **OK**.  

4. Suchen Sie nach der Regel **Advisor Proxy Setting Rule** (Proxyeinstellungsregel für Ratgeber). Klicken Sie auf der Symbolleiste der Betriebskonsole auf **Außerkraftsetzungen**. Klicken Sie auf **Regel außer Kraft setzen\Für ein bestimmtes Objekt der Klasse: Integritätsdienst**. 

5. Wählen Sie als Nächstes in der Liste ein bestimmtes Objekt aus. 

6. Optional: Erstellen Sie eine benutzerdefinierte Gruppe mit dem Integritätsdienstobjekt der Server, auf die Sie diese Außerkraftsetzung anwenden möchten. Daraufhin können Sie die Außerkraftsetzung auf diese Gruppe anwenden.

7. Klicken Sie im Dialogfeld **Außerkraftsetzungseigenschaften**, um in der Spalte **Außerkraftsetzung** neben dem Parameter **WebProxyAddress** ein Häkchen zu setzen.  Geben Sie im Feld **Außerkraftsetzungswert** die URL des OMS-Gatewayservers ein. Beginnen Sie dabei mit dem Präfix `http://`.  

    >[!NOTE]
    > Sie müssen die Regel nicht aktivieren. Sie wird bereits automatisch mit einer Außerkraftsetzung aus dem Microsoft System Center Advisor-Management Pack für die Außerkraftsetzung von sicheren Verweisen verwaltet, das auf die Überwachungsservergruppe von Microsoft System Center Advisor ausgerichtet ist.
    >   

8. Wählen Sie in der Liste **Ziel-Management Pack auswählen** ein Management Pack aus, oder klicken Sie auf **Neu**, um ein neues, nicht versiegeltes Management Pack zu erstellen. 

9. Klicken Sie nach Abschluss der Änderungen auf **OK**. 

### <a name="configure-the-oms-gateway-for-automation-hybrid-runbook-workers"></a>Konfigurieren des OMS-Gateways für Automation Hybrid Runbook Worker
Für den Fall, dass Ihre Umgebung Automation Hybrid Runbook Worker enthält, finden Sie im Anschluss manuelle, temporäre Problemumgehungen, mit denen Sie das OMS-Gateway für deren Unterstützung konfigurieren können.

In den folgenden Schritten müssen Sie die Azure-Region kennen, in der sich das Automation-Konto befindet. Führen Sie zur Ermittlung des Standorts die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie den Azure Automation-Dienst.
3. Wählen Sie das entsprechende Azure Automation-Konto aus.
4. Die Region des Kontos finden Sie unter **Standort**.

    ![Azure-Portal – Standort des Automation-Kontos](./media/log-analytics-oms-gateway/location.png)  

Verwenden Sie die folgenden Tabellen, um die URL für jeden Speicherort zu identifizieren:

**Auftragslaufzeit-Datendienst-URLs**

| **Location** | **URL** |
| --- | --- |
| USA Nord Mitte |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Europa, Westen |we-jobruntimedata-prod-su1.azure-automation.net |
| USA, Süden-Mitte |scus-jobruntimedata-prod-su1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indien, Mitte |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australien |ase-jobruntimedata-prod-su1.azure-automation.net |

**Agent-Dienst-URLs**

| **Location** | **URL** |
| --- | --- |
| USA Nord Mitte |ncus-agentservice-prod-1.azure-automation.net |
| Europa, Westen |we-agentservice-prod-1.azure-automation.net |
| USA Süd Mitte |scus-agentservice-prod-1.azure-automation.net |
| USA (Ost) 2 |eus2-agentservice-prod-1.azure-automation.net |
| Kanada, Mitte |cc-agentservice-prod-1.azure-automation.net |
| Nordeuropa |ne-agentservice-prod-1.azure-automation.net |
| Südostasien |sea-agentservice-prod-1.azure-automation.net |
| Indien, Mitte |cid-agentservice-prod-1.azure-automation.net |
| Japan |jpe-agentservice-prod-1.azure-automation.net |
| Australien |ase-agentservice-prod-1.azure-automation.net |

Wenn Ihr Computer automatisch als Hybrid Runbook Worker für das Patchen mithilfe der Update Management-Lösung registriert wird, gehen Sie wie folgt vor:

1. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Geben Sie beispielsweise Folgendes ein: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

2. Starten Sie den OMS-Gatewaydienst mithilfe des folgenden PowerShell-Cmdlets neu: `Restart-Service OMSGatewayService`

Wenn Sie das Onboarding Ihres Computers bei Azure Automation mithilfe des Cmdlets für die Hybrid Runbook Worker-Registrierung durchführen, gehen Sie wie folgt vor:

1. Fügen Sie die Agent-Dienstregistrierungs-URL der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Geben Sie beispielsweise Folgendes ein: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`

2. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Geben Sie beispielsweise Folgendes ein: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

3. Starten Sie den OMS-Gatewaydienst neu: `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-Cmdlets
Cmdlets können bei Aufgaben hilfreich sein, die zum Aktualisieren der Konfigurationseinstellungen des OMS-Gateways ausgeführt werden müssen. Führen Sie vor deren Verwendung zunächst folgende Schritte aus:

1. Installieren des OMS-Gateways (MSI).
2. Öffnen Sie ein PowerShell-Konsolenfenster.
3. Geben Sie zum Importieren des Moduls den folgenden Befehl ein: `Import-Module OMSGateway`.
4. Wenn im vorherigen Schritt kein Fehler aufgetreten ist, wurde das Modul erfolgreich importiert, und die Cmdlets können verwendet werden. Geben Sie `Get-Module OMSGateway`ein.
5. Starten Sie den OMS-Gatewaydienst neu, nachdem Sie mithilfe von Cmdlets Änderungen vorgenommen haben.

Wenn in Schritt 3 ein Fehler auftritt, wurde das Modul nicht importiert. Der Fehler kann auftreten, wenn PowerShell das Modul nicht finden kann. Es befindet sich im Installationspfad des OMS-Gateways: *C:\Programme\Microsoft OMS Gateway\PowerShell*.

| **Cmdlet** | **Parameter** | **Beschreibung** | **Beispiel** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Schlüssel |Ruft die Konfiguration des Diensts ab |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Schlüssel (erforderlich) <br> Wert |Ändert die Konfiguration des Diensts |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Ruft die Adresse des Relays (Upstreamproxy) ab. |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adresse<br> Username<br> Password |Legt die Adresse (und die Anmeldeinformationen) des Relay-Proxys (Upstreamproxy) fest. |1. Legen Sie einen Relay-Proxy und Anmeldeinformationen fest:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Legen Sie einen Relay-Proxy fest, der keine Authentifizierung erfordert: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Löschen Sie die Relay-Proxyeinstellung:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Ruft den derzeit zulässigen Host ab. (Gilt nur für den lokal konfigurierten zulässigen Host. Automatisch heruntergeladene zulässige Hosts werden nicht berücksichtigt.) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (erforderlich) |Fügt den Host der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (erforderlich) |Entfernt den Host aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Fügt den Clientzertifikatantragsteller der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Entfernt den Clientzertifikatantragsteller aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Ruft die derzeit zulässigen Clientzertifikat-Antragsteller ab. (Gilt nur für lokal konfigurierte zulässige Antragsteller. Automatisch heruntergeladene zulässige Antragsteller werden nicht berücksichtigt.) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problembehandlung
Um vom Gateway protokollierte Ereignisse zu erfassen, muss auch der OMS-Agent installiert sein.

![Ereignisanzeige – OMS-Gatewayprotokoll](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gatewayereignis-IDs und Beschreibungen**

Die folgende Tabelle enthält die Ereignis-IDs und Beschreibungen für OMS Gatewayprotokollereignisse:

| **ID** | **Beschreibung** |
| --- | --- |
| 400 |Anwendungsfehler ohne bestimmte ID. |
| 401 |Falsche Konfiguration. Beispiel: listenPort = "text" (anstelle einer ganzen Zahl). |
| 402 |Ausnahme beim Analysieren von TLS-Handshakenachrichten. |
| 403 |Netzwerkfehler. Beispiel: Mit dem Zielserver konnte keine Verbindung hergestellt werden. |
| 100 |Allgemeine Informationen. |
| 101 |Der Dienst wurde gestartet. |
| 102 |Der Dienst wurde beendet. |
| 103 |Ein HTTP CONNECT-Befehl des Clients wurde empfangen. |
| 104 |Kein HTTP CONNECT-Befehl. |
| 105 |Der Zielserver ist nicht in der Liste zulässiger Elemente enthalten, oder der Zielport ist kein sicherer Port (443). <br> <br> Stellen Sie sicher, dass der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, mit dem gleichen Log Analytics-Arbeitsbereich verbunden sind. |
| 105 |FEHLER TcpConnection – Ungültiges Clientzertifikat: CN=Gateway <br><br> Stellen Sie Folgendes sicher: <br>    <br> - Sie verwenden ein Gateway mit der Versionsnummer 1.0.395.0 oder höher. <br> - Der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, sind mit dem gleichen Log Analytics-Arbeitsbereich verbunden. |
| 106 |Das OMS-Gateway unterstützt nur TLS 1.0, TLS 1.1 und 1.2.  SSL wird nicht unterstützt. Für alle nicht unterstützten Versionen des TLS/SSL-Protokolls generiert das OMS-Gateway Ereignis-ID 106.|
| 107 |Die TLS-Sitzung wurde überprüft. |

**Zu sammelnde Leistungsindikatoren**

Die folgende Tabelle zeigt die verfügbaren Leistungsindikatoren für das OMS-Gateway. Sie können die Indikatoren über den Systemmonitor hinzufügen.

| **Name** | **Beschreibung** |
| --- | --- |
| OMS-Gateway/Aktive Clientverbindung |Anzahl der aktiven Clientnetzwerkverbindungen (TCP) |
| OMS-Gateway/Fehleranzahl |Anzahl von Fehlern |
| OMS-Gateway/Verbundener Client |Anzahl verbundener Clients |
| OMS-Gateway/Ablehnungsanzahl |Anzahl von Ablehnungen aufgrund von TLS-Überprüfungsfehlern |

![OMS-Gateway-Leistungsindikatoren](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Unterstützung
Wenn Sie sich am Azure-Portal angemeldet haben, können Sie eine Anfrage zwecks Unterstützung im Zusammenhang mit dem OMS-Gateway oder anderen Azure-Diensten oder Features eines Diensts erstellen.

Sollten Sie Unterstützung benötigen, klicken Sie in der rechten oberen Ecke des Portals auf das Fragezeichen. Klicken Sie anschließend auf **Neue Supportanfrage**. Füllen Sie das Formular für die neue Supportanfrage aus.

![Neue Supportanfrage](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Nächste Schritte
[Fügen Sie Datenquellen hinzu](log-analytics-data-sources.md), um Daten aus Ihren verbundenen Quellen zu erfassen und in Ihrem Log Analytics-Arbeitsbereich zu speichern.

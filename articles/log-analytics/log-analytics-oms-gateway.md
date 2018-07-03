---
title: Verbinden von Computern über das OMS-Gateway | Microsoft Docs
description: Verbinden Sie Ihre Geräte und durch Operations Manager überwachte Computer mit dem OMS-Gateway, um Daten an den Azure Automation- und Log Analytics-Dienst zu senden, wenn diese Dienste nicht über Internetzugriff verfügen.
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
ms.date: 05/16/2018
ms.author: magoedte
ms.openlocfilehash: b3055e6b22e3f391c0bc3f321cd8117d55a95cf5
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34271648"
---
# <a name="connect-computers-without-internet-access-using-the-oms-gateway"></a>Verbinden von Computern ohne Internetzugriff über das OMS-Gateway
Dieses Dokument beschreibt, wie die Kommunikation mit Azure Automation and Log Analytics über das OMS-Gateway konfiguriert wird, wenn direkt verbundene oder durch Operations Manager überwachte Computer keinen Internetzugang haben.  Das OMS-Gateway, ein HTTP-Weiterleitungsproxy, der HTTP-Tunnel mit dem Befehl HTTP CONNECT unterstützt, kann Daten erfassen und im Auftrag der Computer an Azure Automation und Log Analytics senden.  

Das OMS-Gateway unterstützt Folgendes:

* Azure Automation – Hybrid-Runbook-Worker  
* Direkt mit einem Log Analytics-Arbeitsbereich verbundene Windows-Computer mit Microsoft Monitoring Agent
* Direkt mit einem Log Analytics-Arbeitsbereich verbundene Linux-Computer mit dem OMS-Agent für Linux  
* Integration von System Center Operations Manager 2012 SP1 mit UR7, Operations Manager 2012 R2 mit UR3, Operations Manager 2016 und der Operations Manager-Verwaltungsgruppe, Version 1801, in Log Analytics.  

Falls Computer in Ihrem Netzwerk aufgrund von IT-Sicherheitsrichtlinien keine Verbindung mit dem Internet herstellen können (etwa im Fall von POS-Geräten oder von Servern, die IT-Dienste unterstützen) und Sie zu deren Verwaltung und Überwachung eine Verbindung mit Azure Automation oder Log Analytics herstellen möchten, können sie so konfiguriert werden, dass sie direkt mit dem OMS-Gateway kommunizieren, um in ihrem Auftrag Konfigurations- und Weiterleitungsdaten zu empfangen.  Wenn diese Computer mit dem OMS-Agent konfiguriert werden, um eine direkte Verbindung mit einem Log Analytics-Arbeitsbereich herzustellen, kommunizieren alle Computer stattdessen mit dem OMS-Gateway.  Das Gateway überträgt Daten von den Agents direkt an den Dienst, ohne sie bei der Übertragung zu analysieren.

Wenn eine Operations Manager-Verwaltungsgruppe in Log Analytics integriert wird, können die Verwaltungsserver so konfiguriert werden, dass sie eine Verbindung mit dem OMS-Gateway herstellen, um Konfigurationsinformationen zu empfangen und gesammelte Daten abhängig von der aktivierten Lösung zu senden.  Operations Manager-Agents senden einige Daten wie etwa Operations Manager-Warnungen, Konfigurationsbewertung, Speicherplatz der Instanz und Kapazitätsdaten an den Verwaltungsserver. Andere umfangreiche Daten wie etwa IIS-Protokolle, Leistungsinformationen und Sicherheitsereignisse werden direkt an das OMS-Gateway gesendet.  Falls Sie zur Überwachung nicht vertrauenswürdiger Systeme einen oder mehrere Operations Manager-Gatewayserver in einer DMZ oder in einem anderen isolierten Netzwerk bereitgestellt haben, können diese nicht mit einem OMS-Gateway kommunizieren.  Operations Manager-Gatewayservers können nur mit einem Verwaltungsserver kommunizieren.  Wenn eine Operations Manager-Verwaltungsgruppe für die Kommunikation mit dem OMS-Gateway konfiguriert ist, werden die Proxykonfigurationsinformationen automatisch an alle per Agent verwalteten Computer verteilt, die zum Sammeln von Daten für Log Analytics konfiguriert sind. Das gilt auch, wenn die Einstellung leer ist.    

Zur Gewährleistung der Hochverfügbarkeit für direkt verbundene Gruppen oder Operations Management-Gruppen, die über das Gateway mit Log Analytics kommunizieren, können Sie den Datenverkehr mithilfe des Netzwerklastenausgleichs umleiten und auf mehrere Gatewayserver verteilen.  Wenn ein Gatewayserver ausfällt, wird der Datenverkehr an einen anderen verfügbaren Knoten umgeleitet.  

Der OMS-Agent wird auf dem Computer benötigt, auf dem das OMS-Gateway läuft. Dort identifiziert er die Dienstendpunkte, mit denen er kommunizieren muss, und überwacht das OMS-Gateway, um seine Leistungs- oder Ereignisdaten zu analysieren.

Jeder Agent benötigt eine Netzwerkverbindung mit dem entsprechenden Gateway, damit Agents automatisch Daten an das und vom Gateway übertragen können. Das Gateway sollte nicht auf einem Domänencontroller installiert werden.

Die folgende Abbildung zeigt den Datenfluss von direkten Agents an Azure Automation und Log Analytics unter Verwendung des Gatewayservers.  In der Proxykonfiguration der Agents muss der Port verwendet werden, mit dem das OMS-Gateway für die Kommunikation mit dem Dienst konfiguriert ist.  

![Abbildung der direkten Kommunikation von Agents mit Diensten](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Die folgende Abbildung zeigt den Datenfluss von einer Operations Manager-Verwaltungsgruppe an Log Analytics.   

![Abbildung der Kommunikation von Operations Manager mit Log Analytics](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, der für die Ausführung des OMS-Gateways vorgesehen ist, muss über Folgendes verfügen:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET Framework 4.5
* Vierkernprozessor und 8 GB Arbeitsspeicher (mindestens) 
* OMS-Agent für Windows 

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
Die folgende Tabelle zeigt die unterstützte Anzahl der Agents, die mit einem Gatewayserver kommunizieren können.  Diese Unterstützung basiert auf Agents, die alle 6 Sekunden ungefähr 200 KB Daten hochladen. Die Datenmenge pro getestetem Agent beträgt etwa 2,7 GB pro Tag.

|Gateway |Ungefähre Anzahl unterstützter Agents|  
|--------|----------------------------------|  
|- CPU: Intel XEON CPU E5-2660 v3 mit 2,6 GHz, 2 Kerne<br> - Arbeitsspeicher: 4 GB<br> - Netzwerkbandbreite: 1 GBit/s| 600|  
|- CPU: Intel XEON CPU E5-2660 v3 mit 2,6 GHz, 4 Kerne<br> - Arbeitsspeicher: 8 GB<br> - Netzwerkbandbreite: 1 GBit/s| 1000|  

## <a name="download-the-oms-gateway"></a>Herunterladen des OMS-Gateways

Die neueste Version der Setupdatei für das OMS-Gateway kann auf zwei Arten bezogen werden.

1. Herunterladen über das [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Herunterladen aus dem Azure-Portal.  Gehen Sie folgendermaßen vor, nachdem Sie sich am Azure-Portal angemeldet haben:  

   1. Durchsuchen Sie die Liste mit den Diensten, und wählen Sie **Log Analytics** aus.  
   2. Wählen Sie einen Arbeitsbereich aus.
   3. Klicken Sie auf dem Blatt für Ihren Arbeitsbereich unter **Allgemein** auf **Schnellstart**.
   4. Klicken Sie unter **Choose a data source to connect to the workspace** (Datenquelle auswählen, die mit dem Arbeitsbereich verbunden werden soll) auf **Computer**.
   5. Klicken Sie auf dem Blatt **Direct Agent** auf **Download OMS Gateway** (OMS-Gateway herunterladen).<br><br> ![Download OMS Gateway](./media/log-analytics-oms-gateway/download-gateway.png) (OMS-Gateway herunterladen)

oder 

   1. Klicken Sie in Ihrem Arbeitsbereichblatt unter **Einstellungen** auf **Erweiterte Einstellungen**.
   2. Navigieren Sie zu **Verbundene Quellen** > **Windows-Server**, und klicken Sie auf **OMS Gateway herunterladen**.

## <a name="install-the-oms-gateway"></a>Installieren des OMS-Gateways

Führen Sie die folgenden Schritte aus, um ein Gateway zu installieren.  Falls eine ältere Version (ehemals *Log Analytics Forwarder*) installiert ist, wird ein Upgrade auf die aktuelle Version ausgeführt.  

1. Doppelklicken Sie im Zielordner auf **OMS Gateway.msi**.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**.<br><br> ![Gateway-Setup-Assistent](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br> 
3. Wählen Sie auf der Seite **Lizenzvertrag** die Option **Ich stimme den Bedingungen des Lizenzvertrags zu.** aus, um den Lizenzbestimmungen zuzustimmen, und wählen Sie anschließend **Weiter** aus.
4. Gehen Sie auf der Seite für die **Port- und Proxyadresse** wie folgt vor:
   1. Geben Sie die für das Gateway zu verwendende TCP-Portnummer ein. Das Setup konfiguriert eine Eingangsregel mit dieser Portnummer für die Windows-Firewall.  Der Standardwert ist 8080.
      Der gültige Bereich der Portnummer ist 1 bis 65535. Wenn die Eingabe außerhalb dieses Bereichs liegt, wird eine Fehlermeldung angezeigt.
   2. Optional: Falls der Server, auf dem das Gateway installiert ist, über einen Proxy kommunizieren muss, geben Sie die Proxyadresse ein, mit der das Gateway eine Verbindung herstellen muss. Beispiel: `http://myorgname.corp.contoso.com:80`.  Ohne Angabe versucht das Gateway, eine direkte Verbindung mit dem Internet herzustellen.  Falls der Proxyserver eine Authentifizierung erfordert, geben Sie einen Benutzernamen und ein Kennwort ein.<br><br> ![Gateway-Assistent-Proxykonfiguration](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. Klicken Sie auf **Weiter**.
5. Falls Microsoft Update nicht aktiviert ist, wird die Seite „Microsoft Update“ angezeigt, auf der Sie Microsoft Update aktivieren können. Treffen Sie eine Auswahl, und klicken Sie dann auf **Weiter**. Fahren Sie andernfalls mit dem nächsten Schritt fort.
6. Übernehmen Sie auf der Seite **Zielordner** entweder den Standardordner „C:\Programme\OMS Gateway“, oder geben Sie den Speicherort zum Installieren des Gateways ein, und klicken Sie anschließend auf **Weiter**.
7. Klicken Sie auf der Seite **Bereit zur Installation** auf **Installieren**. Unter Umständen fordert die Benutzerkontensteuerung eine Installationsberechtigung an. Ist dies der Fall, klicken Sie auf **Ja**.
8. Nachdem Setup abgeschlossen ist, klicken Sie auf **Fertig stellen**. Sie können überprüfen, ob der Dienst ausgeführt wird. Öffnen Sie dazu das Snap-In „services.msc“, und überprüfen Sie, ob **OMS-Gateway** in der Liste mit den Diensten angezeigt wird und den Status **Wird ausgeführt** besitzt.<br><br> ![Dienste – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Konfigurieren des Netzwerklastenausgleichs 
Für Hochverfügbarkeit können Sie das Gateway mit Netzwerklastenausgleich (Network Load Balancing, NLB) konfigurieren. Hierzu können Sie entweder den Microsoft-Netzwerklastenausgleich (NLB) oder einen hardwarebasierten Lastenausgleich verwenden.  Der Lastenausgleich leitet die angeforderten Verbindungen von OMS-Agents oder Operations Manager-Verwaltungsservern zur Verwaltung des Datenverkehrs für alle Knoten um. Wenn ein Gatewayserver ausfällt, wird der Datenverkehr zu anderen Knoten umgeleitet.

Informationen zum Entwerfen und Bereitstellen eines Netzwerklastenausgleichs-Clusters unter Windows Server 2016 finden Sie unter [Netzwerklastenausgleich](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Die folgenden Schritte beschreiben, wie Sie einen Cluster mit Microsoft-Netzwerklastenausgleich konfigurieren.  

1. Melden Sie sich mit einem Administratorkonto bei dem Windows-Server an, der dem NLB-Cluster angehört.  
2. Öffnen Sie im Server-Manager den Netzwerklastenausgleich-Manager, klicken Sie auf **Tools**, und klicken Sie anschließend auf **Netzwerklastenausgleich-Manager**.
3. Um einen OMS-Gatewayserver mit dem installierten Microsoft Monitoring Agent zu verbinden, klicken Sie mit der rechten Maustaste auf die IP-Adresse des Clusters, und klicken Sie dann auf **Host dem Cluster hinzufügen**.<br><br> ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen](./media/log-analytics-oms-gateway/nlb02.png)<br> 
4. Geben Sie die IP-Adresse des Gatewayservers ein, mit dem Sie eine Verbindung herstellen möchten.<br><br> ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen: Verbinden](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-oms-agent-and-operations-manager-management-group"></a>Konfigurieren von OMS-Agent und Operations Manager-Verwaltungsgruppe
Im folgenden Abschnitt erfahren Sie, wie Sie direkt verbundene OMS-Agents, eine Operations Manager-Verwaltungsgruppe oder Azure Automation Hybrid Runbook Worker mit dem OMS-Gateway konfigurieren, um die Kommunikation mit Azure Automation und Log Analytics zu ermöglichen.  

### <a name="configure-standalone-oms-agent"></a>Konfigurieren eines eigenständigen OMS-Agents
Informationen zu den Anforderungen und Schritten zum Installieren des OMS-Agents auf Windows-Computern mit Log Analytics-Direktverbindung finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md). Entsprechende Informationen für Linux-Computer finden Sie unter [Verbinden von Linux-Computern mit Log Analytics](log-analytics-quick-collect-linux-computer.md). Statt einen Proxyserver bei der Konfiguration des Agents anzugeben, ersetzen Sie diesen Wert durch die IP-Adresse des OMS-Gatewayservers und die Portnummer.  Wenn Sie mehrere Gatewayserver hinter einem Netzwerklastenausgleich bereitgestellt haben, handelt es sich bei der OMS-Agent-Proxykonfiguration um die virtuelle IP-Adresse des NLB.  

Informationen zum Automation Hybrid Runbook Worker finden Sie unter [Bereitstellen von Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Konfigurieren von Operations Manager (alle Agents verwenden den gleichen Proxyserver)
Sie konfigurieren Operations Manager, um den Gatewayserver hinzuzufügen.  Die Operations Manager-Proxykonfiguration wird automatisch auf alle Agents angewendet, die Operations Manager unterstellt sind. Das gilt auch, wenn die Einstellung leer ist.  

Voraussetzungen für die Unterstützung von Operations Manager mithilfe des Gateways:

* Microsoft Monitoring Agent (Agent-Version: **8.0.10900.0** oder höher) muss auf dem Gateway-Server installiert und für die Log Analytics-Arbeitsbereiche konfiguriert sein, mit denen Sie kommunizieren möchten.
* Das Gateway benötigt Internetkonnektivität oder muss mit einem Proxyserver verbunden sein, bei dem dies der Fall ist.

> [!NOTE]
> Wenn Sie keinen Wert für das Gateway angeben, werden mithilfe von Push leere Werte an alle Agents übertragen.
> 

Wenn sich Ihre Operations Manager-Verwaltungsgruppe zum ersten Mal bei einem Log Analytics-Arbeitsbereich registriert, ist die Option zum Angeben der Proxykonfiguration für die Verwaltungsgruppe in der Betriebskonsole nicht verfügbar.  Die Verwaltungsgruppe muss erfolgreich beim Dienst registriert werden. Erst dann wird diese Option verfügbar.  Sie müssen die Systemproxykonfiguration mit Netsh auf dem System aktualisieren, von dem aus Sie die Betriebskonsole ausführen, um die Integration sowie alle Verwaltungsserver in der Verwaltungsgruppe zu konfigurieren.  

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten.
   a. Navigieren Sie zu **Start**, und geben Sie **cmd** ein.
   b. Klicken Sie mit der rechten Maustaste auf **Eingabeaufforderung**, und wählen Sie „Als Administrator ausführen“** aus.
2. Geben Sie den folgenden Befehl ein, und drücken Sie die **EINGABETASTE**:

    `netsh winhttp set proxy <proxy>:<port>`

Nachdem Sie die Integration mit Log Analytics abgeschlossen haben, können Sie die Änderung entfernen, indem Sie `netsh winhttp reset proxy` ausführen und dann die Option **Proxyserver konfigurieren** in der Betriebskonsole verwenden, um den OMS-Gatewayserver anzugeben. 

1. Öffnen Sie die Operations Manager-Konsole, und klicken Sie unter **Operations Management Suite**, auf **Verbindung** und dann auf **Proxyserver konfigurieren**.<br><br> ![Operations Manager – Proxyserver konfigurieren](./media/log-analytics-oms-gateway/scom01.png)<br> 
2. Wählen Sie **Proxyserver für Zugriff auf den Operations Management Suite-Dienst verwenden** aus, und geben Sie die IP-Adresse des OMS-Gatewayservers oder die virtuelle IP-Adresse des NLBs ein. Beginnen Sie mit dem Präfix `http://`.<br><br> ![Operations Manager – Proxyserveradresse](./media/log-analytics-oms-gateway/scom02.png)<br> 
3. Klicken Sie auf **Fertig stellen**. Ihre Operations Manager-Verwaltungsgruppe ist nun so konfiguriert, dass sie über den Gatewayserver mit dem Log Analytics-Dienst kommuniziert.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Konfigurieren von Operations Manager (bestimmte Agents verwenden einen Proxyserver)
Bei großen oder komplexen Umgebungen soll der OMS-Gatewayserver unter Umständen nur von bestimmten Servern (oder Gruppen) verwendet werden.  Für diese Server können Sie den Operations Manager-Agent nicht direkt aktualisieren, da dieser Wert durch den globalen Wert für die Verwaltungsgruppe überschrieben wird.  Stattdessen müssen Sie die Regel außer Kraft setzen, die verwendet wird, um diese Werte mithilfe von Push zu übertragen.  

> [!NOTE] 
> Mit der gleichen Konfigurationsmethode können Sie die Verwendung mehrerer OMS-Gatewayserver in Ihrer Umgebung ermöglichen.  So können beispielsweise spezifische OMS-Gatewayserver für einzelne Regionen angegeben werden.
>  

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Erstellen** aus.  
2. Wählen Sie im Erstellungsarbeitsbereich die Option **Regeln** aus, und klicken Sie auf der Operations Manager-Symbolleiste auf die Schaltfläche **Bereich**. Sollte diese Schaltfläche nicht verfügbar sein, vergewissern Sie sich, dass im Überwachungsbereich kein Ordner, sondern ein Objekt ausgewählt ist. Im Dialogfeld **Management Pack-Objekte in Bereiche einteilen** wird eine Liste mit allgemeinen Zielklassen, Gruppen oder Objekten angezeigt. 
3. Geben Sie in das **Suchfeld**den Suchbegriff **Integritätsdienst** ein, und wählen Sie den Dienst in der Liste aus.  Klicken Sie auf **OK**.  
4. Suchen Sie nach der Regel **Advisor Proxy Setting Rule** (Proxyeinstellungsregel für Ratgeber), und klicken Sie auf der Symbolleiste der Operations-Konsole auf **Außerkraftsetzungen**. Zeigen Sie anschließend auf **Regel außer Kraft setzen\Für ein bestimmtes Objekt der Klasse: Integritätsdienst**, und wählen Sie in der Liste ein bestimmtes Objekt aus.  Optional können Sie auch eine benutzerdefinierte Gruppe mit dem Integritätsdienstobjekt der Server erstellen, auf die Sie diese Außerkraftsetzung anwenden möchten, und dann die Außerkraftsetzung auf diese Gruppe anwenden.
5. Klicken Sie im Dialogfeld **Außerkraftsetzungseigenschaften**, um in der Spalte **Außerkraftsetzung** neben dem Parameter **WebProxyAddress** ein Häkchen zu setzen.  Geben Sie im Feld **Außerkraftsetzungswert** die URL des OMS-Gatewayservers ein. Beginnen Sie dabei mit dem Präfix `http://`.  

    >[!NOTE]
    > Die Regel muss nicht aktiviert werden, da sie bereits automatisch mit einer Außerkraftsetzung aus dem Microsoft System Center Advisor-Management Pack für die Außerkraftsetzung von sicheren Verweisen verwaltet wird, das auf die Überwachungsservergruppe von Microsoft System Center Advisor ausgerichtet ist.
    >   

6. Wählen Sie in der Liste **Ziel-Management Pack auswählen** ein Management Pack aus, oder klicken Sie auf **Neu**, um ein neues, nicht versiegeltes Management Pack zu erstellen. 
7. Klicken Sie nach Abschluss der Änderungen auf **OK**. 

### <a name="configure-for-automation-hybrid-workers"></a>Konfigurieren für Automation Hybrid Worker
Für den Fall, dass Ihre Umgebung Automation Hybrid Runbook Worker enthält, finden Sie im Anschluss manuelle, temporäre Problemumgehungen, mit denen Sie das Gateway für deren Unterstützung konfigurieren können.

In den folgenden Schritten müssen Sie die Azure-Region kennen, in der sich das Automation-Konto befindet. So finden Sie den Speicherort

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie den Azure Automation-Dienst.
3. Wählen Sie das entsprechende Azure Automation-Konto aus.
4. Zeigen Sie seine Region unter **Speicherort** an.<br><br> ![Azure-Portal – Speicherort des Automation-Kontos](./media/log-analytics-oms-gateway/location.png)  

Verwenden Sie die folgenden Tabellen, um die URL für jeden Speicherort zu identifizieren:

**Auftragslaufzeit-Datendienst-URLs**

| **Speicherort** | **URL** |
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

| **Speicherort** | **URL** |
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

1. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Starten Sie den OMS-Gatewaydienst mithilfe des folgenden PowerShell-Cmdlets neu: `Restart-Service OMSGatewayService`

Wenn das Onboarding Ihres Computers bei Azure Automation mithilfe des Cmdlets für die Hybrid Runbook Worker-Registrierung durchgeführt wird, gehen Sie wie folgt vor:

1. Fügen Sie die Agent-Dienstregistrierungs-URL der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Starten Sie den OMS-Gatewaydienst neu.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets
Cmdlets können Ihnen helfen, Aufgaben durchzuführen, die für die Aktualisierung der Konfigurationseinstellungen des OMS-Gateways erforderlich sind. Bevor Sie sie verwenden, müssen folgende Schritte durchgeführt worden sein:

1. Installieren des OMS-Gateways (MSI).
2. Öffnen Sie ein PowerShell-Konsolenfenster.
3. Geben Sie zum Importieren des Moduls folgenden Befehl ein: `Import-Module OMSGateway`
4. Wenn im vorherigen Schritt kein Fehler aufgetreten ist, wurde das Modul erfolgreich importiert, und die Cmdlets können verwendet werden. Geben Sie `Get-Module OMSGateway` ein
5. Nachdem Sie die Cmdlets geändert haben, stellen Sie sicher, dass Sie den Gatewaydienst neu starten.

Wenn in Schritt 3 ein Fehler auftritt, wurde das Modul nicht importiert. Der Fehler kann auftreten, wenn PowerShell das Modul nicht finden kann. Sie finden es im Gateway-Installationspfad: *C:\Programme\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parameter** | **Beschreibung** | **Beispiel** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Schlüssel |Ruft die Konfiguration des Diensts ab |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Schlüssel (erforderlich) <br> Wert |Ändert die Konfiguration des Diensts |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Ruft die Adresse des Relays (Upstreamproxy) ab |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adresse<br> Username<br> Password |Legt die Adresse (und Anmeldeinformationen) des Relays (Upstreamproxy) fest |1. Legen Sie einen Relay-Proxy und Anmeldeinformationen fest:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Legen Sie einen Relay-Proxy fest, der keine Authentifizierung erfordert: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Löschen Sie die Relay-Proxyeinstellung:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Ruft den derzeit zulässigen Host ab. (Gilt nur für den lokal konfigurierten zulässigen Host. Automatisch heruntergeladene zulässige Hosts werden nicht berücksichtigt.) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (erforderlich) |Fügt den Host der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (erforderlich) |Entfernt den Host aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Fügt den Clientzertifikatantragsteller der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Entfernt den Clientzertifikatantragsteller aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Ruft die derzeit zulässigen Clientzertifikatantragsteller ab. (Gilt nur für lokal konfigurierte zulässige Antragsteller. Automatisch heruntergeladene zulässige Antragsteller werden nicht berücksichtigt.) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problembehandlung
Um vom Gateway protokollierte Ereignisse zu erfassen, muss auch der OMS-Agent installiert sein.<br><br> ![Ereignisanzeige – OMS-Gatewayprotokoll](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gatewayereignis-IDs und Beschreibungen**

Die folgende Tabelle enthält die Ereignis-IDs und Beschreibungen für OMS Gatewayprotokollereignisse.

| **ID** | **Beschreibung** |
| --- | --- |
| 400 |Alle Anwendungsfehler, die keine bestimmte ID haben |
| 401 |Falsche Konfiguration. Beispiel: listenPort = "text" anstelle eines Integers |
| 402 |Fehler bei der Analyse von TLS-Handshakenachrichten |
| 403 |Netzwerkfehler. Beispiel: Verbindung mit dem Zielserver kann nicht hergestellt werden |
| 100 |Allgemeine Informationen |
| 101 |Dienst wurde gestartet |
| 102 |Dienst wurde beendet |
| 103 |HTTP CONNECT-Befehl vom Client empfangen |
| 104 |Kein HTTP CONNECT-Befehl |
| 105 |Zielserver ist nicht in der Liste zulässiger Elemente enthalten, oder der Zielport ist kein sicherer Port (443) <br> <br> Stellen Sie sicher, dass der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, mit dem gleichen Log Analytics-Arbeitsbereich verbunden sind. |
| 105 |FEHLER TcpConnection – Ungültiges Clientzertifikat: CN=Gateway <br><br> Stellen Sie Folgendes sicher: <br>    <br> &#149; Sie verwenden ein Gateway mit der Versionsnummer 1.0.395.0 oder höher. <br> &#149; Der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, sind mit dem gleichen Log Analytics-Arbeitsbereich verbunden. |
| 106 |Das OMS-Gateway unterstützt nur TLS 1.0, TLS 1.1 und 1.2.  SSL wird nicht unterstützt. Für alle nicht unterstützten Versionen des TLS/SSL-Protokolls generiert das OMS-Gateway Ereignis-ID 106.|
| 107 |Die TLS-Sitzung wurde überprüft |

**Zu sammelnde Leistungsindikatoren**

Die folgende Tabelle zeigt die verfügbaren Leistungsindikatoren für das OMS-Gateway. Sie können die Indikatoren mit dem Systemmonitor hinzufügen.

| **Name** | **Beschreibung** |
| --- | --- |
| OMS-Gateway/Aktive Clientverbindung |Anzahl der aktiven Clientnetzwerkverbindungen (TCP) |
| OMS-Gateway/Fehleranzahl |Anzahl von Fehlern |
| OMS-Gateway/Verbundener Client |Anzahl verbundener Clients |
| OMS-Gateway/Ablehnungsanzahl |Anzahl von Ablehnungen aufgrund von TLS-Überprüfungsfehlern |

![OMS-Gateway-Leistungsindikatoren](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Unterstützung
Wenn Sie sich am Azure-Portal angemeldet haben, können Sie eine Anfrage zwecks Unterstützung im Zusammenhang mit dem OMS-Gateway oder anderen Azure-Diensten oder Features eines Diensts erstellen.
Um Unterstützung anzufordern, klicken Sie auf das Fragezeichen in der oberen rechten Ecke des Portals, und klicken Sie dann auf **Neue Supportanfrage**. Füllen Sie das neue Formular für die Supportanfrage aus.

![Neue Supportanfrage](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von Datenquellen](log-analytics-data-sources.md), um Daten aus Ihren verbundenen Quellen zu erfassen und in Ihrem Log Analytics-Arbeitsbereich zu speichern.

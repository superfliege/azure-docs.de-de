---
title: "Konfigurieren des Netzwerkleistungsmonitors für Azure ExpressRoute-Verbindungen (Vorschau) | Microsoft-Dokumentation"
description: "Konfigurieren des Netzwerkleistungsmonitors für Azure ExpressRoute-Verbindungen (Vorschau)"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: cherylmc
ms.openlocfilehash: 3ab8029d035c3ba88ddb8a112e27f9054f7c203c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="configure-network-performance-monitor-for-expressroute-preview"></a>Konfigurieren des Netzwerkleistungsmonitors für ExpressRoute (Vorschau)

Der Netzwerkleistungsmonitor (Network Performance Monitor, NPM) ist eine cloudbasierte Lösung für die Netzwerküberwachung, die die Konnektivität zwischen Azure-Cloudbereitstellungen und lokalen Standorten (Zweigstellen usw.) überwacht. Der Netzwerkleistungsmonitor ist Teil von Microsoft Operations Management Suite (OMS). Der Netzwerkleistungsmonitor bietet nun eine Erweiterung für ExpressRoute, durch die Sie die Netzwerkleistung über ExpressRoute-Verbindungen überwachen können, die dafür konfiguriert sind, privates Peering zu verwenden. Wenn Sie den Netzwerkleistungsmonitor für ExpressRoute konfigurieren, können Sie Netzwerkprobleme erkennen, die identifiziert und entfernt werden sollen.

Ihre Möglichkeiten:

* Überwachen der Verluste und der Latenz über verschiedene VNETs sowie das Einrichten von Warnungen

* Überwachen aller Pfade (einschließlich der redundanten) des Netzwerks

* Problembehandlung bei vorübergehenden und Point-in-Time-Netzwerkproblemen, die schwer zu replizieren sind

* Ermitteln eines bestimmten Segments des Netzwerks, das für die beeinträchtigte Leistung verantwortlich ist

* Abrufen von Durchsatz pro virtuellem Netzwerk (wenn Sie Agents in jedem VNET installiert haben)

* Anzeigen des ExpressRoute-Systemstatus von einem früheren Zeitpunkt aus

## <a name="regions"></a>Unterstützte Regionen

Sie können ExpressRoute-Verbindungen in jedem Teil der Welt mit einem Arbeitsbereich, der in einer der folgenden Regionen gehostet wird, überwachen:

* Europa, Westen 
* USA (Ost) 
* Südostasien 

## <a name="workflow"></a>Workflow

Überwachungs-Agents werden auf mehreren Servern (lokal und in Azure) installiert. Die Agents kommunizieren miteinander, senden jedoch keine Daten, sondern TCP-Handshakepakete. Die Kommunikation zwischen den Agents ermöglicht es Azure, die Netzwerktopologie und den Pfad zuzuordnen, die der Datenverkehr nutzen kann.

1. Erstellen Sie einen NPM-Arbeitsbereich in einer der [unterstützten Regionen](#regions).
2. Installieren und konfigurieren Sie Software-Agents: 
    * Installieren Sie Überwachungs-Agents auf den lokalen Servern und den virtuellen Azure-Computern.
    * Konfigurieren Sie die Einstellungen der Server der Überwachungs-Agents, um diesen die Kommunikation zu ermöglichen. (Öffnen Sie die Firewallports usw.)
3. Konfigurieren Sie Regeln für die Netzwerksicherheitsgruppen, um den auf virtuellen Azure-Computern installierten Überwachungs-Agents die Kommunikation mit den lokalen Überwachungs-Agents zu ermöglichen.
4. Fordern Sie die Aufnahme Ihres NPM-Arbeitsbereichs in eine Whitelist an.
5. Richten Sie die Überwachung ein: automatisches Erkennen und Verwalten, welche Netzwerke im Netzwerkleistungsmonitor sichtbar sind.

Wenn Sie bereits einen Netzwerkleistungsmonitor verwenden, um andere Objekte oder Dienste zu überwachen, und bereits einen Arbeitsbereich in einer der unterstützten Regionen besitzen, können Sie die Schritte 1 und 2 überspringen und Ihre Konfiguration mit Schritt 3 beginnen.

## <a name="configure"></a>Schritt 1: Erstellen eines Arbeitsbereichs

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) in der **Marketplace**-Dienstliste nach „Netzwerkleistungsmonitor“. Klicken Sie in die Ausgabe, um die Seite **Netzwerkleistungsmonitor** zu öffnen.

  ![Portal](.\media\how-to-npm\3.png)<br><br>
2. Klicken Sie im unteren Bereich der Hauptseite **Netzwerkleistungsmonitor** auf **Erstellen**, um die Seite **Network Performance Monitor – Create new solution** (Netzwerkleistungsmonitor – Neue Lösung erstellen) zu öffnen. Klicken Sie auf **OMS Workspace – select a workspace** (OMS-Arbeitsbereich – Arbeitsbereich auswählen), um die Seite „Arbeitsbereich“ zu öffnen. Klicken Sie auf **+ Neuen Arbeitsbereich erstellen**, um die Seite „Arbeitsbereich“ zu öffnen.
3. Klicken Sie auf der Seite **OMS-Arbeitsbereich** auf **Neu erstellen**, und konfigurieren Sie folgende Einstellungen:

  * OMS-Arbeitsbereich: Geben Sie einen Namen für Ihren Arbeitsbereich ein.
  * Abonnement: Wenn Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, das Sie dem neuen Arbeitsbereich zuordnen möchten.
  * Ressourcengruppe: Erstellen Sie eine Ressourcengruppe, oder verwenden Sie eine vorhandene.
  * Standort: Sie müssen eine [unterstützte Region](#regions) auswählen.
  * Tarif: Wählen Sie „Free“ aus.
  
  >[!NOTE]
  >Die ExpressRoute-Verbindung kann sich an einer beliebigen Stelle auf der ganzen Welt befinden. Sie muss nicht in der gleichen Region wie der Arbeitsbereich liegen.
  >


  ![Arbeitsbereich](.\media\how-to-npm\4.png)<br><br>
4. Klicken Sie auf **OK**, um die Einstellungsvorlage zu speichern und bereitzustellen. Sobald die Vorlage überprüft wurde, klicken Sie auf **Erstellen**, um den Arbeitsbereich bereitzustellen.
5. Nachdem der Arbeitsbereich bereitgestellt wurde, navigieren Sie zu der Ressource **NetworkMonitoring(Name)**, die Sie erstellt haben. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Für die Lösung ist eine weitere Konfiguration erforderlich**.

  ![Zusätzliche Konfiguration](.\media\how-to-npm\5.png)
6. Wählen Sie auf der Seite **Willkommen beim Netzwerkleistungsmonitor** **TCP für synthetische Transaktionen verwenden** aus, und klicken Sie dann auf **Absenden**. Die TCP-Transaktionen werden nur zum Herstellen und Trennen der Verbindung verwendet. Über diese TCP-Verbindungen werden keine Daten gesendet.

  ![TCP für synthetische Transaktionen](.\media\how-to-npm\6.png)

## <a name="agents"></a>Schritt 2: Installieren und Konfigurieren von Agents

### <a name="download"></a>2.1: Herunterladen der Setup-Datei für den Agent

1. Klicken Sie auf der Seite **Netzwerkleistungsmonitor – TCP-Setup** Ihrer Ressource im Abschnitt **OMS-Agents installieren** auf den Agent, der dem Prozessor Ihres Servers entspricht, und laden Sie die Setup-Datei herunter.

  >[!NOTE]
  >Der Linux-Agent wird derzeit nicht für die ExpressRoute-Überwachung unterstützt.
  >
  >
2. Kopieren Sie dann die **Arbeitsbereich-ID** und den **Primärschlüssel**, und fügen Sie diese in Editor ein.
3. Laden Sie im Abschnitt **Agents konfigurieren** das PowerShell-Skript herunter. Das PowerShell-Skript unterstützt Sie beim Öffnen des relevanten Firewallports für die TCP-Transaktionen.

  ![PowerShell-Skript](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2: Installieren eines Überwachungs-Agents auf jeden Überwachungsserver

1. Führen Sie das **Setup** aus, um den Agent auf jedem Server zu installieren, den Sie zum Überwachen von ExpressRoute verwenden möchten. Der Server, den Sie für die Überwachung verwenden, kann lokal oder ein virtueller Computer sein und benötigt Zugriff auf das Internet. Sie müssen mindestens einen Agent lokal und einen in jedem Netzwerksegment, das in Azure überwacht werden soll, installieren.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**.
3. Lesen Sie die Seite **Lizenzbedingungen** durch, und klicken Sie anschließend auf **Ich stimme zu**.
4. Auf der Seite **Zielordner** können Sie den Standardinstallationsordner entweder ändern oder beibehalten. Klicken Sie anschließend auf **Weiter**.
5. Auf der Seite **Agent-Setupoptionen** können Sie auswählen, ob der Agent mit Azure Log Analytics (OMS) oder Operations Manager verbunden werden soll. Sie können die Auswahl auch leer lassen, wenn Sie den Agent später konfigurieren möchten. Klicken Sie auf **Weiter**, nachdem Sie Ihre Auswahl getroffen haben.

  * Wenn Sie eine Verbindung mit **Azure Log Analytics (OMS)** herstellen möchten, fügen Sie die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel** (Primärschlüssel) ein, die Sie im vorherigen Abschnitt in Editor kopiert haben. Klicken Sie auf **Weiter**.

    ![ID und Schlüssel](.\media\how-to-npm\8.png)
  * Wenn Sie eine Verbindung zu **Operations Manager** herstellen möchten, geben Sie auf der Seite **Verwaltungsgruppenkonfiguration** den **Namen der Verwaltungsgruppe**, den **Verwaltungsserver** und den **Verwaltungsserverport** ein. Klicken Sie auf **Weiter**.

    ![Operations Manager](.\media\how-to-npm\9.png)
  * Wählen Sie auf der Seite **Agentaktionskonto**  entweder das Konto **Lokales System** oder **Domäne oder lokaler Computer** aus. Klicken Sie auf **Weiter**.

    ![Konto](.\media\how-to-npm\10.png)
6. Überprüfen Sie Ihre Auswahl auf der Seite **Bereit zum Installieren**, und klicken Sie dann auf **Installieren**.
7. Klicken Sie auf der Seite **Die Konfiguration wurde erfolgreich abgeschlossen** auf **Fertig stellen**.
8. Nach Abschluss wird der Microsoft Monitoring Agent in der Systemsteuerung angezeigt. Dort können Sie Ihre Konfiguration überprüfen und sicherstellen, dass der Agent mit Operational Insights (OMS) verbunden ist. Wenn die Verbindung mit OMS hergestellt ist, zeigt der Agent folgende Meldung an: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service** (Für den Microsoft Monitoring Agent wurde die Verbindung mit dem Microsoft Operations Management Suite-Dienst erfolgreich hergestellt).

### <a name="proxy"></a>2.3: Konfigurieren von Proxyeinstellungen (optional)

Wenn Sie einen Webproxy verwenden, um auf das Internet zuzugreifen, verwenden Sie folgende Schritte, um die Proxyeinstellungen für den Microsoft Monitoring Agent zu konfigurieren. Führen Sie diese Schritte für jeden Server aus. Wenn Sie viele Server konfigurieren müssen, ist es möglicherweise einfacher, diesen Prozess mithilfe eines Skripts zu automatisieren. Wenn dies der Fall ist, finden Sie weitere Informationen unter [So konfigurieren Sie Proxyeinstellungen für den Microsoft Monitoring Agent mithilfe eines Skripts](../log-analytics/log-analytics-windows-agents.md#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

So konfigurieren Sie Proxyeinstellungen für den Microsoft Monitoring Agent über die Systemsteuerung:

1. Öffnen Sie die **Systemsteuerung**.
2. Öffnen Sie **Microsoft Monitoring Agent**.
3. Klicken Sie auf die Registerkarte **Proxyeinstellungen** .
4. Wählen Sie **Proxyserver verwenden** aus, und geben Sie die URL und gegebenenfalls die Portnummer ein. Wenn der Proxyserver eine Authentifizierung erfordert, geben Sie den Benutzernamen und das Kennwort für den Zugriff auf den Proxyserver ein.

  ![proxy](.\media\how-to-npm\11.png)

### <a name="verifyagent"></a>2.4: Überprüfen der Agentkonnektivität

Sie können problemlos überprüfen, ob die Agents kommunizieren.

1. Öffnen Sie die **Systemsteuerung** auf einem Server mit dem Überwachungs-Agent.
2. Öffnen Sie den **Microsoft Monitoring Agent**.
3. Klicken Sie auf die Registerkarte **Azure Log Analytics (OMS)**.
4. In der Spalte **Status** sollte angezeigt werden, dass der Agent erfolgreich mit dem Operations Management Suite-Dienst verbunden wurde.

  ![status](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5: Öffnen der Firewallports auf den Servern des Überwachungs-Agents

Für die Verwendung des TCP-Protokolls müssen Sie die Firewallports öffnen, um sicherzustellen, dass die Überwachungs-Agents kommunizieren können.

Sie können ein PowerShell-Skript ausführen, das die vom Netzwerkleistungsmonitor benötigten Registrierungsschlüssel sowie die Windows-Firewallregeln erstellt, um es den Überwachungs-Agents zu ermöglichen, TCP-Verbindungen untereinander herzustellen. Die vom Skript erstellten Registrierungsschlüssel geben außerdem an, ob die Debugprotokolle und der Pfad zur Protokolldatei protokolliert werden sollen. Ferner definieren sie den für die Kommunikation verwendeten Agent-TCP-Port. Die Werte für diese Schlüssel werden automatisch durch das Skript festgelegt, daher sollten Sie diese Schlüssel nicht manuell ändern.

Port 8084 wird standardmäßig geöffnet. Sie können einen benutzerdefinierten Port verwenden, indem Sie im Skript den Parameter „portNumber“ angeben. In diesem Fall müssen Sie jedoch den gleichen Port für alle Server angeben, auf denen Sie das Skript ausführen.

>[!NOTE]
>Das PowerShell-Skript „EnableRules“ konfiguriert Windows-Firewallregeln nur auf dem Server, auf dem das Skript ausgeführt wird. Bei Verwendung einer Netzwerkfirewall sollten Sie sicherstellen, dass diese den Datenverkehr zum TCP-Port erlaubt, der vom Netzwerkleistungsmonitor verwendet wird.
>
>

Öffnen Sie auf den Agent-Servern ein PowerShell-Fenster mit Administratorrechten. Führen Sie das PowerShell-Skript [EnableRules](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) (das Sie zuvor heruntergeladen haben) aus. Verwenden Sie keine Parameter.

  ![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>Schritt 3: Konfigurieren von Regeln für Netzwerksicherheitsgruppen

Für das Überwachen von Agent-Servern in Azure müssen Sie Regeln für Netzwerksicherheitsgruppen konfigurieren, um TCP-Datenverkehr auf einem Port zuzulassen, der vom Netzwerkleistungsmonitor für synthetische Transaktionen verwendet wird. Der Standardport ist 8084. Dadurch wird es einem Überwachungs-Agent, der auf einem virtuellen Azure-Computer installiert ist, ermöglicht, mit einem lokalen Überwachungs-Agent zu kommunizieren.

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-create-nsg-arm-portal.md).

## <a name="whitelist"></a>Schritt 4: Anfordern der Aufnahme des Arbeitsbereichs in eine Whitelist

>[!NOTE]
>Stellen Sie sicher, dass Sie die Agents (sowohl den lokalen Server-Agent als auch den Azure-Server-Agent) installiert und das PowerShell-Skript ausgeführt haben, bevor Sie mit diesem Schritt fortfahren.
>
>

Bevor Sie das ExpressRoute-Überwachungsfeature des Netzwerkleistungsmonitors verwenden können, müssen Sie anfordern, dass Ihr Arbeitsbereich in eine Whitelist aufgenommen wird. [Klicken Sie hier, um zu der Seite zu gelangen und das Anforderungsformular auszufüllen.](https://aka.ms/npmcohort) (Hinweis: Sie sollten diesen Link in einem neuen Fenster oder einer neuen Registerkarte öffnen.) Der Whitelistprozess kann einen Geschäftstag oder mehr in Anspruch nehmen. Sobald der Eintrag in der Whitelist vorgenommen wurde, senden wir Ihnen eine E-Mail zu.

## <a name="setupmonitor"></a>Schritt 5: Konfigurieren des Netzwerkleistungsmonitors für die ExpressRoute-Überwachung

>[!WARNING]
>Fahren Sie nicht fort, bevor Ihr Arbeitsbereich in eine Whitelist aufgenommen wurde und Sie eine Bestätigung per E-Mail erhalten haben.
>
>

Nachdem Sie die vorherigen Abschnitte abgeschlossen und überprüft haben, dass Sie in eine Whitelist aufgenommen wurden, können Sie die Überwachung einrichten.

1. Navigieren Sie zur Übersichtskachel „Netzwerkleistungsmonitor“, indem Sie zur Seite **Alle Ressourcen** wechseln und auf den in eine Whitelist aufgenommenen NPM-Arbeitsbereich klicken.

  ![NPM-Arbeitsbereich](.\media\how-to-npm\npm.png)
2. Klicken Sie auf die Übersichtskachel **Netzwerkleistungsmonitor**, um das Dashboard zu öffnen. Das Dashboard enthält die Seite „ExpressRoute“, die anzeigt, dass ExpressRoute sich in einem nicht konfigurierten Zustand befindet. Klicken Sie auf **Featureeinrichtung**, um die Konfigurationsseite des Netzwerkleistungsmonitors zu öffnen.

  ![Featureeinrichtung](.\media\how-to-npm\npm2.png)
3. Navigieren Sie auf der Konfigurationsseite zur Registerkarte „ExpressRoute-Peerings“, die sich im linken Bereich befindet. Klicken Sie auf **Jetzt ermitteln**.

  ![Ermitteln](.\media\how-to-npm\13.png)
4. Wenn die Ermittlung abgeschlossen ist, werden Ihnen Regeln für eindeutige Verbindungsnamen und VNET-Namen angezeigt. Diese Regeln sind zunächst deaktiviert. Aktivieren Sie diese Regeln, und wählen Sie dann die Überwachungs-Agents und Schwellenwerte aus.

  ![Regeln](.\media\how-to-npm\14.png)
5. Nach dem Aktivieren der Regeln und dem Auswählen der Werte und Agents, die Sie überwachen möchten, gibt es eine Wartezeit von etwa 30 bis 60 Minuten, bis die Werte aufgefüllt und die Kacheln **ExpressRoute-Überwachung** verfügbar sind. Sobald Ihnen die Überwachungskacheln angezeigt werden, werden Ihre ExpressRoute-Verbindungen und -Verbindungsressourcen vom Netzwerkleistungsmonitor überwacht.

  ![Überwachungskacheln](.\media\how-to-npm\15.png)

## <a name="explore"></a>Schritt 6: Anzeigen der Überwachungskacheln

### <a name="dashboard"></a>Seite des Netzwerkleistungsmonitors

Die Seite des Netzwerkleistungsmonitors enthält eine Seite für ExpressRoute, die eine Übersicht über die Integrität der ExpressRoute-Verbindungen und -Peerings anzeigt.

  ![Dashboard](.\media\how-to-npm\dashboard.png)

### <a name="circuits"></a>Liste der Verbindungen

Klicken Sie auf die Kachel **ExpressRoute-Verbindungen**, um eine Liste aller überwachten ExpressRoute-Verbindungen anzuzeigen. Sie können eine Verbindung auswählen und deren Integritätsstatus, Trenddiagramme für den Paketverlust, Bandbreitennutzung und Latenz anzuzeigen. Die Diagramme sind interaktiv. Sie können ein benutzerdefiniertes Zeitfenster für das Zeichnen der Diagramme auswählen. Sie können die Maus über einen Bereich des Diagramms ziehen, um dieses zu vergrößern und detaillierte Datenpunkte anzuzeigen.

  ![circuit_list](.\media\how-to-npm\circuits.png)

#### <a name="trend"></a>Trend des Verlusts, der Latenz und des Durchsatzes

Die Diagramme für Bandbreite, Latenz und Verlust sind interaktiv. Sie können jeden Bereich dieser Diagramme mithilfe der Maussteuerung vergrößern. Sie können auch die Daten anderer Intervalle für Bandbreite, Latenz und Verlust anzeigen, indem Sie auf **Datum/Uhrzeit** klicken. Diese Angabe finden Sie oben links unter der Schaltfläche „Aktionen“.

![Trend](.\media\how-to-npm\16.png)

### <a name="peerings"></a>Liste der Peerings

Durch das Klicken auf die Kachel **Private Peerings** auf dem Dashboard wird eine Liste aller Verbindungen zu virtuellen Netzwerken über privates Peering angezeigt. Hier können Sie eine virtuelle Netzwerkverbindung auswählen und deren Integritätsstatus, Trenddiagramme für Paketverlust, Bandbreitennutzung und Latenz anzeigen lassen.

  ![Liste der Verbindungen](.\media\how-to-npm\peerings.png)

### <a name="topology"></a>Topologie der Verbindung

Klicken Sie auf die Kachel **Topologie**, um die Topologie der Verbindung anzuzeigen. Dadurch gelangen Sie zur Ansicht der Topologie für die ausgewählte Verbindung oder das ausgewählte Peering. Das Topologiediagramm enthält die Latenz für jedes Segment im Netzwerk, außerdem wird jeder Layer 3-Hop durch einen Knoten des Diagramms dargestellt. Durch das Klicken auf einen Hop werden weitere Details zum Hop angezeigt.
Sie können den Sichtbarkeitsgrad erhöhen, um lokale Hops einzuschließen, indem Sie den Schieberegler unter **Filter** bewegen. Das Bewegen des Schiebereglers nach links oder rechts vergrößert oder verkleinert die Anzahl der Hops im Topologiediagramm. Die Latenz ist für jedes Segment sichtbar, wodurch eine schnellere Isolation von Segmenten mit hoher Latenz in Ihrem Netzwerk ermöglicht wird.

![Filter](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Detaillierte Topologieansicht einer Verbindung

Diese Ansicht zeigt die VNET-Verbindungen.
![detaillierte Topologie](.\media\how-to-npm\17.png)

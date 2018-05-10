---
title: Azure-Datenverkehrsanalyse | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Datenflussprotokolle von Azure-Netzwerksicherheitsgruppen mit der Datenverkehrsanalyse analysieren.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: jdial
ms.openlocfilehash: ba7589b0fcbb987ef4e7225ea5a4cceda9ad1179
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="traffic-analytics"></a>Datenverkehrsanalyse

Die Datenverkehrsanalyse (Englisch: Traffic Analytics) ist eine cloudbasierte Lösung, die Einblick in Benutzer- und Anwendungsaktivitäten in Cloudnetzwerken bietet. Mit der Datenverkehrsanalyse werden die Flussprotokolle von Network Watcher für Netzwerksicherheitsgruppen (NSGs) analysiert, um Einblicke in den Datenfluss in Ihrer Azure-Cloud zu ermöglichen. Mit der Datenverkehrsanalyse können Sie folgende Aktionen durchführen:

- Visualisieren der Netzwerkaktivität für Ihre Azure-Abonnements und Identifizieren von Hotspots
- Erkennen von Sicherheitsrisiken für das Netzwerk und Schützen Ihres Netzwerks mithilfe von Informationen zu offenen Ports, Anwendungen, die versuchen, Zugriff auf das Internet zu erhalten, und virtuellen Computern (VM), die Verbindungen mit betrügerischen Netzwerken herstellen
- Verstehen von Mustern im Datenverkehr über Azure-Regionen und das Internet zur Optimierung Ihrer Netzwerkbereitstellung im Hinblick auf Leistung und Kapazität
- Ermitteln von Fehlkonfigurationen im Netzwerk, die zu fehlerhaften Verbindungen in Ihrem Netzwerk führen

## <a name="why-traffic-analytics"></a>Gründe für die Nutzung der Datenverkehrsanalyse

Es ist wichtig, dass Sie Ihr eigenes Netzwerk kennen, überwachen und verwalten, um die Sicherheit, Compliance und Leistung zu gewährleisten. Eine genaue Kenntnis Ihrer eigenen Umgebung ist von großer Bedeutung für deren Schutz und Optimierung. Häufig müssen Sie den aktuellen Status des Netzwerks kennen und wissen, wer womit verbunden ist, welche Ports zum Internet geöffnet sind, welches Netzwerkverhalten zu erwarten ist oder ob unregelmäßiges Netzwerkverhalten oder plötzliche Anstiege beim Datenverkehr auftreten.

Cloudnetzwerke unterscheiden sich von lokalen Unternehmensnetzwerken, in denen Sie über Netflow oder ähnliche protokollfähige Router und Switches verfügen, die Ihnen die Möglichkeit bieten, ein- und ausgehenden IP-Netzwerkdatenverkehr zu erfassen. Durch die Analyse der Daten zum Datenverkehr können Sie eine Analyse der Datenflüsse und Mengen des Netzwerkdatenverkehrs erstellen.

Virtuelle Azure-Netzwerke bieten NSG-Datenflussprotokolle mit Informationen zum ein- und ausgehenden IP-Datenverkehr über eine Netzwerksicherheitsgruppe, die einzelnen Netzwerkschnittstellen, VMs oder Subnetzen zugeordnet ist. Durch die Analyse von unformatierten NSG-Flussprotokollen und das Einfügen von Informationen zu Sicherheit, Topologie und Geografie kann die Datenverkehrsanalyse Ihnen Einblicke in den Verkehrsfluss in Ihrer Umgebung verschaffen. Traffic Analytics liefert Informationen wie die folgenden: Hosts und Anwendungsprotokolle mit der meisten Kommunikation, Hostpaare mit der meisten gemeinsamen Kommunikation, erlaubter/blockierter Datenverkehr, ein-/ausgehender Datenverkehr, offene Internetports, die Regeln, die am meisten blockieren, die Datenverkehrsverteilung nach Azure-Rechenzentrum, virtuelles Netzwerk, Subnetze oder nicht autorisierte Netzwerke.

## <a name="key-components"></a>Wichtige Komponenten 

- **Netzwerksicherheitsgruppe (NSG):** enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit virtuellen Azure-Netzwerken verbunden sind, zugelassen oder abgelehnt wird. NSGs können Subnetzen, einzelnen VMs (klassisch) oder einzelnen Netzwerkschnittstellen (NICs), die mit VMs (Resource Manager) verbunden sind, zugeordnet werden. Weitere Informationen finden Sie unter [Übersicht über Netzwerksicherheit](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **NSG-Flussprotokolle (Netzwerksicherheitsgruppe):** Ermöglichen das Anzeigen von Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe. Die NSG-Flussprotokolle sind im JSON-Format geschrieben und zeigen aus- und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP-Adresse, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr. Weitere Informationen zu NSG-Flussprotokollen finden Sie unter [Einführung in die Datenflussprotokollierung für Netzwerksicherheitsgruppen](network-watcher-nsg-flow-logging-overview.md).
- **Log Analytics:** ein Azure-Dienst, der Überwachungsdaten sammelt und in einem zentralen Repository speichert. Bei diesen Daten kann es sich um Ereignisse, Leistungsdaten oder benutzerdefinierte Daten handeln, die über die Azure-API bereitgestellt wurden. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden. Überwachungsanwendungen, z.B. Netzwerkleistungsmonitor und die Datenverkehrsanalyse, werden auf der Grundlage von Log Analytics erstellt. Weitere Informationen finden Sie unter [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Log Analytics-Arbeitsbereich:** Eine Instanz von Log Analytics, in der Daten zu einem Azure-Konto gespeichert werden. Weitere Informationen zu Log Analytics-Arbeitsbereichen finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Network Watcher:** ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Sie können die NSG-Flussprotokolle mit Network Watcher ein- und ausschalten. Weitere Informationen finden Sie unter [Network Watcher](network-watcher-monitoring-overview.md).

## <a name="how-traffic-analytics-works"></a>Funktionsweise der Datenverkehrsanalyse 

Bei der Datenverkehrsanalyse werden die unformatierten NSG-Flussprotokolle untersucht und reduzierte Protokolle durch das Zusammenfassen allgemeiner Datenflüsse zwischen denselben Quell-IP-Adressen, Ziel-IP-Adressen, Zielports und Protokollen erfasst. Beispiel: Host 1 (IP-Adresse: 10.10.10.10) kommuniziert mit Host 2 (IP-Adresse: 10.10.20.10) über einen Zeitraum von einer Stunde 100-mal über einen Port (z.B. 80) und ein Protokoll (z.B. HTTP). Das reduzierte Protokoll enthält anstelle von 100 Einträgen nur einen Eintrag – nämlich dass Host 1 und Host 2 über einen Zeitraum von einer Stunde 100-mal über Port *80* und das Protokoll *HTTP* kommuniziert haben. Die reduzierten Protokolle werden durch Informationen zu Geografie, Sicherheit und Topologie erweitert und anschließend in einem Log Analytics-Arbeitsbereich gespeichert. Das folgende Bild zeigt den Datenfluss:

![Verarbeitung des Datenflusses für NSG-Flussprotokolle](media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions"></a>Unterstützte Regionen

Die Datenverkehrsanalyse ist als Vorschauversion verfügbar. Features in der Vorschaurelease weisen nicht den gleichen Grad an Verfügbarkeit und Zuverlässigkeit wie bereits allgemein verfügbare Features auf.  Als Vorschauversion können Sie die Datenverkehrsanalyse für Netzwerksicherheitsgruppen in einer der folgenden Regionen verwenden: „USA, Westen-Mitte“, „USA, Osten“, „USA, Osten 2“, „USA, Norden-Mitte“, „USA, Süden-Mitte“, „USA, Mitte“, „USA, Westen“, „USA, Westen 2“, „Europa, Westen“, „Europa, Norden“, „Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“, „Australien, Osten“ und „Australien, Südosten“. Der Log Analytics-Arbeitsbereich muss in einer der folgenden Regionen vorhanden sein: „USA, Westen-Mitte“, „USA, Osten“, „Europa, Westen“, „Australien, Südosten“ oder „Vereinigtes Königreich, Süden“.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="enable-network-watcher"></a>Aktivieren von Network Watcher 

Um Datenverkehr analysieren zu können, benötigen Sie eine Instanz von Network Watcher, oder Sie [aktivieren eine Network Watcher-Instanz](network-watcher-create.md) in jeder Region mit Netzwerksicherheitsgruppen, deren Datenverkehr Sie analysieren möchten. Die Datenverkehrsanalyse kann für Netzwerksicherheitsgruppen aktiviert werden, die in einer der [unterstützten Regionen](#supported-regions) gehostet werden.

### <a name="re-register-the-network-resource-provider"></a>Erneutes Registrieren des Netzwerkressourcenanbieters 

Bevor Sie die Datenverkehrsanalyse während der Vorschauphase verwenden können, müssen Sie Ihren Netzwerkressourcenanbieter erneut registrieren. Klicken Sie im folgenden Codefeld auf **Ausprobieren**, um die Azure Cloud Shell zu öffnen. Die Cloud Shell meldet Sie automatisch bei Ihrem Azure-Abonnement an. Nachdem die Cloud Shell geöffnet ist, geben Sie den folgenden Befehl ein, um den Netzwerkressourcenanbieter erneut zu registrieren:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Auswählen einer Netzwerksicherheitsgruppe 

Vor der Aktivierung der NSG-Flussprotokollierung benötigen Sie eine Netzwerksicherheitsgruppe, deren Datenflüsse Sie protokollieren möchten. Falls Sie noch nicht über eine Netzwerksicherheitsgruppe verfügen, erstellen Sie eine anhand der Informationen in [Erstellen von Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

Wählen Sie auf der linken Seite des Azure-Portals **Monitor**, anschließend **Network Watcher** und dann **NSG-Flussprotokolle** aus. Wählen Sie die Netzwerksicherheitsgruppe aus, für die Sie ein NSG-Flussprotokoll aktivieren möchten, wie in der folgenden Abbildung gezeigt:

![Auswählen von Netzwerksicherheitsgruppen für die Aktivierung der NSG-Flussprotokolle](media/traffic-analytics/selection-of-nsgs-that-require- enablement-of-nsg-flow-logging.png)

Wenn Sie versuchen, die Datenverkehrsanalyse für eine NSG zu aktivieren, die in einer nicht [unterstützten Region](#supported-regions) gehostet wird, erhalten Sie die Fehlermeldung „Nicht gefunden“. 

## <a name="enable-flow-log-settings"></a>Aktivieren der Flussprotokolleinstellungen

Vor der Aktivierung der Flussprotokolleinstellungen müssen Sie die folgenden Aufgaben ausführen:

Registrieren Sie den Azure Insights-Anbieter, falls dieser nicht bereits für Ihr Abonnement registriert wurde:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

Wenn Sie noch kein Azure Storage-Konto zum Speichern der NSG-Flussprotokolle besitzen, müssen Sie ein Speicherkonto erstellen. Sie können mit dem folgenden Befehl ein Speicherkonto erstellen. Ersetzen Sie vor dem Ausführen des Befehls `<replace-with-your-unique-storage-account-name>` durch einen Namen, der an allen Azure-Standorten eindeutig und zwischen 3 und 24 Zeichen lang ist und nur Ziffern und Kleinbuchstaben enthält. Sie können bei Bedarf auch den Namen der Ressourcengruppe ändern.

```azurepowershell-interactive
New-AzureRmStorageAccount `
  -Location westcentralus `
  -Name <replace-with-your-unique-storage-account-name> `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Wählen Sie die folgenden Optionen aus, wie in der Abbildung dargestellt:

1. Wählen Sie *Ein* als **Status** aus.
2. Wählen Sie ein vorhandenes Speicherkonto zum Speichern der Flussprotokolle aus. Wenn Sie die Daten dauerhaft speichern möchten, legen Sie den Wert auf *0* fest. Ihnen werden die Azure Storage-Gebühren für das Speicherkonto berechnet.
3. Legen Sie **Vermerkdauer** auf die Anzahl der Tage fest, die die Daten gespeichert werden sollen.
4. Wählen Sie *Ein* als **Traffic Analytics-Status** aus.
5. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich (OMS) aus, oder wählen Sie **Neuen Arbeitsbereich erstellen** aus, und erstellen Sie dann einen neuen Arbeitsbereich. Traffic Analytics verwendet einen Log Analytics-Arbeitsbereich, um die aggregierten und indizierten Daten zu speichern, die dann zum Generieren der Analyse verwendet werden. Wenn Sie einen vorhandenen Arbeitsbereich auswählen, muss sich dieser in einer [unterstützten Region](#traffic-analytics-supported-regions) befinden und auf die neue Abfragesprache aktualisiert worden sein. Wenn Sie einen vorhandenen Arbeitsbereich nicht aktualisieren möchten oder über keinen Arbeitsbereich in einer unterstützten Region verfügen, erstellen Sie einen neuen. Weitere Informationen über Abfragesprachen finden Sie unter [Upgrade von Azure Log Analytics auf die neue Protokollsuche](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

    Der Log Analytics-Arbeitsbereich, der die Lösung für die Datenverkehrsanalyse hostet, und die Netzwerksicherheitsgruppen (NSGs) müssen sich nicht in derselben Region befinden. Sie können die Datenverkehrsanalyse beispielsweise in einem Arbeitsbereich in der Region „Europa, Westen“ mit Netzwerksicherheitsgruppen in den Regionen „USA, Osten“ und „USA, Westen“ verwenden. Es können mehrere NSGs im selben Arbeitsbereich konfiguriert werden.
6. Wählen Sie **Speichern**aus.

    ![Auswählen von Speicherkonto, Log Analytics-Arbeitsbereich und Traffic Analytics-Aktivierung](media/traffic-analytics/selection-of-storage-account-log-analytics-workspace-and-traffic-analytics-enablement.png)

Wiederholen Sie die oben angegebenen Schritte für alle weiteren NSGs, für die Sie die Datenverkehrsanalyse aktivieren möchten. Die Daten aus den Flussprotokollen werden an den Arbeitsbereich gesendet, daher müssen Sie sicherstellen, dass die vor Ort geltenden Gesetze und Vorschriften in Ihrem Land eine Datenspeicherung in der Region, in der sich der Arbeitsbereich befindet, erlauben.

## <a name="view-traffic-analytics"></a>Anzeigen der Datenverkehrsanalyse

Wählen Sie auf der linken Seite im Portal **Alle Dienste** aus, und geben Sie dann *Monitor* in das Feld **Filter** ein. Wenn **Monitor** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus. Wählen Sie zum Erkunden der Datenverkehrsanalyse und der zugehörigen Funktionen **Network Watcher** und dann **Traffic Analytics (Vorschau)** aus.

![Zugreifen auf das Traffic Analytics-Dashboard](media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

Das erstmalige Anzeigen des Dashboards kann bis zu 30 Minuten dauern, da Traffic Analytics zunächst ausreichend Daten für aussagekräftige Einblicke aggregieren muss, bevor Berichte generiert werden können.

## <a name="usage-scenarios"></a>Verwendungsszenarien

Einige der Erkenntnisse, die Sie nach der vollständigen Konfiguration von Traffic Analytics erhalten können, sind:

### <a name="find-traffic-hotspots"></a>Suchen von Datenverkehrs-Hotspots

**Suchen nach**

- Welche Hosts senden oder empfangen den meisten Datenverkehr?
    - Wenn Sie wissen, welche Hosts den meisten Datenverkehr senden oder empfangen, können Sie die Hosts identifizieren, die den meisten Datenverkehr verarbeiten.
    - Sie können dann prüfen, ob die Menge an Datenverkehr für einen Host angemessen ist. Stellt der Netzwerkverkehr ein normales Verhalten dar, oder bedürfen die Werte einer weiteren Untersuchung?
- Wie viel ein-/ausgehenden Datenverkehr gibt es?
    -   Kann bei dem Host davon ausgegangen werden, dass er mehr eingehenden als ausgehenden Datenverkehr (oder umgekehrt) empfängt?
- Statistiken des zugelassenen/blockierten Datenverkehrs
    - Warum wird auf einem Host eine große Menge an Datenverkehr zugelassen oder blockiert?

    Wählen Sie **More Details** (Weitere Details) unter **Hosts with most traffic** (Hosts mit dem meisten Datenverkehr) aus, wie in der folgenden Abbildung gezeigt:

    ![Dashboard mit Details zum Host mit dem meisten Datenverkehr](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- Das folgende Bild zeigt Zeittrends für die fünf Hosts mit der meisten Kommunikation und die zugehörigen Details zum Datenfluss (zugelassene und blockierte Datenflüsse – jeweils ein- und ausgehend) für einen virtuellen Computer:

    ![Trend der fünf Hosts mit der meisten Kommunikation](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Suchen nach**

- Welche Hostpaare kommunizieren am meisten miteinander?
    - Erwartbares Verhalten wie die Front-End-Back-End-Kommunikation oder abweichendes Verhalten wie Datenverkehr vom Back-End zum Internet.
- Statistiken des zugelassenen/blockierten Datenverkehrs
    - Warum wird auf einem Host besonders viel Datenverkehr zugelassen oder blockiert?
- Am häufigsten verwendetes Anwendungsprotokoll zwischen den Hostpaaren mit der meisten Konversation:
    - Sind diese Anwendungen im Netzwerk zugelassen?
    - Sind die Anwendungen ordnungsgemäß konfiguriert? Verwenden sie das richtige Protokoll für die Kommunikation? Wählen Sie **More Details** (Weitere Details) unter **Most frequent conversations** (Häufigste Kommunikationen) aus, wie in der folgenden Abbildung gezeigt:

        ![Dashboard mit den häufigsten Kommunikationen](media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- Das folgende Bild zeigt Zeittrends für die fünf häufigsten Kommunikationen und die zugehörigen Details zum Datenfluss – z.B. zugelassene und blockierte Datenflüsse (jeweils ein- und ausgehend) für zwei Kommunikationspartner:

    ![Details und Trends für die fünf häufigsten Kommunikationen](media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Suchen nach**

- Welches Anwendungsprotokoll wird in Ihrer Umgebung am häufigsten verwendet, und welche Hosts verwenden das Anwendungsprotokoll als Kommunikationspartner am meisten?
    - Sind diese Anwendungen im Netzwerk zugelassen?
    - Sind die Anwendungen ordnungsgemäß konfiguriert? Verwenden sie das richtige Protokoll für die Kommunikation? Als erwartetes Verhalten können allgemeine Ports wie z.B. 80 oder 443 angenommen werden. Für die Standardkommunikation ist – sofern ungewöhnliche Ports angezeigt werden – eventuell eine Konfigurationsänderung erforderlich. Wählen Sie **More details** (Weitere Details) unter **Top application protocols** (Häufigste Anwendungsprotokolle) aus, wie in der folgenden Abbildung gezeigt:

        ![Dashboard mit den häufigsten Anwendungsprotokollen](media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- Die folgenden Bilder zeigen Zeittrends für die fünf am häufigsten verwendeten L7-Protokolle und Details zum Datenfluss (z.B. erlaubte und verweigerte Datenflüsse) für ein L7-Protokoll:

    ![Details und Trends für die fünf am häufigsten verwendeten L7-Protokolle](media/traffic-analytics/top five-layer-seven-protocols-details-and-trend.png)

    ![Details zum Datenfluss zu einem Anwendungsprotokoll in der Protokollsuche](media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Suchen nach**

- Trends zur Kapazitätsauslastung eines VPN-Gateways in Ihrer Umgebung.
    - Jede VPN-SKU erlaubt eine gewisse Bandbreite. Werden die VPN-Gateways zu wenig ausgelastet?
    - Erreichen Ihre Gateways ihre Kapazität? Sollten Sie ein Upgrade auf die nächsthöhere SKU ausführen?
- Welche Hosts kommunizieren am häufigsten über welches VPN-Gateway und welchen Port?
    - Ist dieses Muster normal? Wählen Sie **More Details** (Weitere Details) unter **Top active VPN connections** (Aktivste VPN-Verbindungen) aus, wie in der folgenden Abbildung gezeigt:

        ![Dashboard mit den aktivsten VPN-Verbindungen](media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- Das folgende Bild zeigt Zeittrends zur Kapazitätsauslastung eines Azure-VPN-Gateways und Details zum Datenfluss (z.B. erlaubte Datenflüsse und Ports):

    ![Auslastungstrend eines VPN-Gateways und Details zum Datenfluss](media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Visualisieren der Verteilung des Datenverkehrs nach Geografie

**Suchen nach**

- Verteilung des Datenverkehrs pro Rechenzentrum, z.B. Hauptquellen des Datenverkehrs in einem Rechenzentrum, häufigste betrügerische Netzwerke, die mit dem Rechenzentrum kommunizieren und Anwendungsprotokolle mit der meisten Kommunikation.
    - Wenn Sie in einem Rechenzentrum mehr Last beobachten, können Sie eine effizientere Verteilung des Datenverkehrs planen.
    - Wenn betrügerische Netzwerke im Rechenzentrum kommunizieren, korrigieren Sie die NSG-Regeln, um diese Netzwerke zu blockieren.

    Wählen Sie **Click here to see live geomap** (Hier klicken für Live-Geomap) unter **Traffic distribution across Azure data centers** (Datenverkehrsverteilung in Azure-Rechenzentren) aus, wie in der folgenden Abbildung gezeigt:

  ![Dashboard mit Verteilung des Datenverkehrs](media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- Sie können in der Geomap auf dem oberen Menüband Parameter, z.B. Rechenzentren (bereitgestellt/nicht bereitgestellt/aktiv/inaktiv/Traffic Analytics aktiviert/Traffic Analytics nicht aktiviert), und Länder, die gutartigen/bösartigen Datenverkehr mit der aktiven Bereitstellung haben, auswählen:

    ![Geomap mit aktiver Bereitstellung](media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- Die Geomap zeigt die Verteilung des Datenverkehrs in einem Rechenzentrum aus Ländern und Kontinenten mit blauen (gutartiger Datenverkehr) und roten (bösartiger Datenverkehr) Linien:

    ![Geomap mit Verteilung des Datenverkehrs auf Länder und Kontinente](media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Details zum Datenfluss für die Verteilung des Datenverkehrs in der Protokollsuche](media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Visualisieren der Verteilung des Datenverkehrs nach virtuellen Netzwerken

**Suchen nach**

- Verteilung des Datenverkehrs nach virtuellem Netzwerk, Topologie, Hauptquellen des Datenverkehrs in das virtuelle Netzwerk, häufigste bösartige Netzwerke mit Kommunikation im virtuellen Netzwerk und Anwendungsprotokolle mit der meisten Kommunikation.
    - Verstehen, welches virtuelle Netzwerk mit welchem virtuellen Netzwerk kommuniziert. Wenn die Kommunikation nicht der Erwartung entspricht, kann sie korrigiert werden.
    - Wenn bösartige Netzwerke mit einem virtuellen Netzwerk kommunizieren, können Sie die NSG-Regeln anpassen, um diese Netzwerke zu blockieren.
 
    Wählen Sie **Click here to see VNet traffic topology** (Hier klicken für VNET-Datenverkehrstopologie) unter **Virtual network distribution** (VNET-Verteilung) aus, wie in der folgenden Abbildung gezeigt: 

    ![Dashboard mit VNET-Verteilung](media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- Sie können in der Topologie des virtuellen Netzwerks auf dem oberen Menüband Parameter auswählen, z.B. für ein virtuelles Netzwerk (Verbindungen zwischen virtuellen Netzwerken/aktiv/inaktiv), externe Verbindungen, aktive Datenflüsse und bösartige Datenflüsse im virtuellen Netzwerk.
- In der Topologie des virtuellen Netzwerks wird die Verteilung des Datenverkehrs in einem virtuellen Netzwerk im Hinblick auf die Datenflüsse (erlaubt/blockiert/eingehend/ausgehend/gutartig/bösartig), das Anwendungsprotokoll und die Netzwerksicherheitsgruppen dargestellt. Beispiel:

    ![Topologie eines virtuellen Netzwerks mit der Verteilung des Datenverkehr und Details zum Datenfluss](media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)

    ![Details zum Datenfluss für die Verteilung des Datenverkehrs in einem virtuellen Netzwerk in der Protokollsuche](media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Suchen nach**

- Verteilung des Datenverkehrs nach Subnetz, Topologie, Hauptquellen des Datenverkehrs in das Subnetz, häufigste bösartige Netzwerke mit Kommunikation im Subnetz und Anwendungsprotokolle mit der meisten Kommunikation.
    - Verstehen, welches Subnetz mit welchem Subnetz kommuniziert. Wenn unerwartete Kommunikationen angezeigt werden, können Sie Ihre Konfiguration korrigieren.
    - Wenn bösartige Netzwerke mit einem Subnetz kommunizieren, können Sie dies beheben, indem Sie z.B. die NSG-Regeln zum Blockieren von bösartigen Netzwerken anpassen.
- Sie können in der Subnetztopologie auf dem oberen Menüband Parameter auswählen, z.B. aktive/inaktive Subnetze, externe Verbindungen, aktive Datenflüsse und bösartige Datenflüsse im Subnetz.
- In der Topologie des Subnetzes wird die Verteilung des Datenverkehrs in einem Subnetz im Hinblick auf die Datenflüsse (erlaubt/blockiert/eingehend/ausgehend/gutartig/bösartig), das Anwendungsprotokoll und die Netzwerksicherheitsgruppen dargestellt. Beispiel:

    ![Subnetztopologie mit Verteilung des Datenverkehrs in einem Subnetz eines virtuellen Netzwerks im Hinblick auf die Datenflüsse](media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Anzeigen der Ports und virtuellen Computer, die Datenverkehr aus dem Internet empfangen

**Suchen nach**

- Welche offenen Ports kommunizieren über das Internet?
    - Wenn Sie unerwartete offene Ports finden, können Sie Ihre Konfiguration korrigieren:

        ![Dashboard mit Ports, die Datenverkehr aus dem Internet empfangen und dorthin senden](media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

        ![Details zu Azure-Zielports und -hosts](media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Suchen nach**

Tritt in Ihrer Umgebung bösartiger Datenverkehr auf? Wo stammt dieser her? Wohin ist er gerichtet?

![Details zum Datenfluss bei bösartigem Datenverkehr in der Protokollsuche](media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsg-rule-hits"></a>Visualisieren von Trends bei Treffern von NSG-Regeln

**Suchen nach**

- Welche NSG/Regel hat die meisten Treffer?
- Welche Kommunikationspartner sind pro NSG am häufigsten Quelle und Ziel?

    ![Dashboard mit Statistiken zu NSG-Treffern](media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- Die folgenden Bilder zeigen Zeittrends für Treffer von NSG-Regeln und Details zum Datenfluss zwischen Quelle und Ziel für eine Netzwerksicherheitsgruppe:

    ![Zeittrends für Treffer von NSG-Regeln und häufigste NSG-Regeln](media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Statistische Details zu den häufigsten NSG-Regeln in der Protokollsuche](media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu Traffic Analytics](traffic-analytics-faq.md).

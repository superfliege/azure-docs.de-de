---
title: Abfragen von Protokollen aus Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: Die Azure Monitor for VMs-Lösung leitet Metriken und Protokolldaten an Log Analytics weiter; in diesem Artikel sind die Datensätze beschrieben, zusammen mit einigen Beispielabfragen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: 6558a888ded1c8657bef6aba886a6f7d14cb554a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254535"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms-preview"></a>Abfragen von Protokollen aus Azure Monitor für VMs (Vorschauversion)
Azure Monitor for VMs sammelt Leistungs- und Verbindungsmetriken, Inventurdaten von Computern und Prozessen sowie Informationen zum Integritätsstatus und leitet diese an den Log Analytics-Datenspeicher in Azure Monitor weiter.  Diese Daten stehen zur [Suche](../log-analytics/log-analytics-queries.md) in Log Analytics zur Verfügung. Diese Daten können in verschiedenen Szenarios von Nutzen sein, z.B. bei der Migrationsplanung, Kapazitätsanalyse, Ermittlung und Ad-hoc-Behebung von Leistungsproblemen.

## <a name="map-records"></a>Zuordnung von Datensätzen
Zusätzlich zu den Datensätzen, die beim Starten eines Prozesses oder Computers oder beim Onboarding beim Zuordnungsfeature von Azure Monitor for VMs generiert werden, wird pro Stunde ein Datensatz für jeden eindeutigen Computer und jeden eindeutigen Prozess generiert. Die Eigenschaften der Datensätze sind in den folgenden Tabellen aufgeführt. Die Felder und Werte in den ServiceMapComputer_CL-Ereignissen sind Feldern der Computerressource in der ServiceMap ARM-API (Azure Resource Manager) zugeordnet. Die Felder und Werte in den ServiceMapProcess_CL-Ereignissen sind Feldern der Prozessressource in der ServiceMap ARM-API zugeordnet. Das Feld „ResourceName_s“ entspricht dem Namensfeld in der entsprechenden ARM-Ressource. 

Es gibt intern generierte Eigenschaften, mit denen Sie eindeutige Prozessen und Computer identifizieren können:

- Computer: Verwenden Sie *ResourceId* oder *ResourceName_s* zur eindeutigen Identifizierung eines Computers in einem Log Analytics-Arbeitsbereich.
- Prozess: Verwenden Sie *ResourceId* zur eindeutigen Identifizierung eines Prozesses in einem Log Analytics-Arbeitsbereich. *ResourceName_s* ist innerhalb des Kontexts des Computers, auf dem der Prozess ausgeführt wird (MachineResourceName_s), eindeutig. 

Da für einen angegebenen Prozess und Computer in einem angegebenen Zeitraum möglicherweise mehrere Datensätze vorhanden sind, können Abfragen mehrere Datensätze für denselben Computer oder Prozess zurückgeben. Wenn nur der aktuellste Datensatz zurückgegeben werden soll, fügen Sie der Abfrage „| dedup ResourceId“ hinzu.

### <a name="connections"></a>Verbindungen
Verbindungsmetriken werden in eine neue Tabelle in Log Analytics geschrieben – VMConnection. Diese Tabelle enthält Informationen zu den Verbindungen für einen Computer (eingehend und ausgehend). Verbindungsmetriken werden ebenfalls mit APIs verfügbar gemacht, die die Möglichkeit zum Abrufen einer bestimmten Metrik während eines Zeitfensters bereitstellen.  TCP-Verbindungen, die durch Ausführen von *accept* auf einem lauschenden Socket hergestellt werden, sind eingehend, während solche, die durch Ausführen von *connect* auf einer bestimmten IP- und Portkombination hergestellt werden, ausgehend sind. Die Richtung einer Verbindung wird durch die Direction-Eigenschaft dargestellt, die auf entweder **inbound** oder **outbound** festgelegt werden kann. 

Die in diesen Tabellen dargestellten Daten wurden aus Daten generiert, die vom Dependency-Agent gemeldet wurden. Jeder Datensatz stellt eine Beobachtung über ein Zeitintervall von einer Minute dar. Die TimeGenerated-Eigenschaft gibt den Anfang des Zeitintervalls an. Jeder Datensatz enthält Informationen zum Identifizieren der jeweiligen Entität – d.h. Verbindung oder Port – sowie der dieser Entität zugeordneten Metriken. Derzeit werden nur Netzwerkaktivitäten gemeldet, die über TCP-over-IPv4 anfallen.

Um Kosten und Komplexität im Zaum zu halten, stellen die Verbindungsdatensätze keine einzelnen physischen Netzwerkverbindungen dar. Mehrere physische Netzwerkverbindungen werden in einer logischen Verbindung gruppiert, die dann in der entsprechenden Tabelle wiedergegeben wird.  Das heißt, dass die Datensätze in der Tabelle *VMConnection* eine logische Gruppierung anstelle der beobachteten einzelnen physischen Verbindungen darstellen. Physische Netzwerkverbindungen, die während eines bestimmten einminütigen Intervalls den gleichen Wert für die folgenden Attribute aufweisen, werden in *VMConnection* zu einem einzelnen logischen Datensatz zusammengefasst. 

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
|Richtung |Richtung der Verbindung, der Wert ist *inbound* oder *outbound* |
|Computer |Der vollqualifizierte Domänenname des Computers |
|Prozess |Identität des Prozesses oder Gruppe von Prozessen, die die Verbindung einleitet/akzeptiert |
|SourceIp |IP-Adresse der Quelle |
|DestinationIp |IP-Adresse des Ziels |
|DestinationPort |Portnummer des Ziels |
|Protokoll |Für die Verbindung verwendetes Protokoll.  Der Wert ist *tcp*. |

Um dem Einfluss der Gruppierung Rechnung zu tragen, werden Informationen über die Anzahl der gruppierten physischen Verbindungen in den folgenden Eigenschaften des Datensatzes bereitgestellt:

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
|LinksEstablished |Die Anzahl der physischen Netzwerkverbindungen, die während des Berichtszeitraums eingerichtet wurden |
|LinksTerminated |Die Anzahl der physischen Netzwerkverbindungen, die während des Berichtszeitraums beendet wurden |
|LinksFailed |Die Anzahl der physischen Netzwerkverbindungen, die während des Berichtszeitraums für ungültig erklärt wurden. Diese Informationen sind derzeit nur für ausgehende Verbindungen verfügbar. |
|LinksLive |Die Anzahl der physischen Netzwerkverbindungen, die am Ende des Berichtszeitraums offen waren|

#### <a name="metrics"></a>Metriken

Über Metriken zur Verbindungsanzahl hinaus sind in den folgenden Eigenschaften des Datensatzes auch Informationen über das Volumen der gesendeten und empfangenen Daten für eine bestimmte logische Verbindung oder einen bestimmten Netzwerkport enthalten:

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
|BytesSent |Gesamtzahl der Bytes, die während des Berichtszeitraums gesendet wurden |
|BytesReceived |Gesamtzahl der Bytes, die während des Berichtszeitraums empfangen wurden |
|Antworten |Die Anzahl der Antworten, die während des Berichtszeitraums beobachtet wurden. 
|ResponseTimeMax |Die längste Antwortzeit (in Millisekunden), die während des Berichtszeitraums beobachtet wurde. Wenn kein Wert vorliegt, ist die Eigenschaft leer.|
|ResponseTimeMin |Die kürzeste Antwortzeit (in Millisekunden), die während des Berichtszeitraums beobachtet wurde. Wenn kein Wert vorliegt, ist die Eigenschaft leer.|
|ResponseTimeSum |Die Summe aller Antwortzeiten (in Millisekunden), die während des Berichtszeitraums beobachtet wurden. Wenn kein Wert vorliegt, ist die Eigenschaft leer.|

Die dritte Art Daten, die gemeldet werden, sind Antwortzeiten – wie viel Zeit verbringt ein Aufrufer mit Warten auf die Verarbeitung und Antwort durch den Remoteendpunkt für eine über eine Verbindung gesendete Anforderung. Die gemeldete Antwortzeit ist eine Schätzung der tatsächlichen Antwortzeit des zugrundeliegenden Anwendungsprotokolls. Sie wird mit heuristischen Verfahren auf der Grundlage der Beobachtung des Datenflusses zwischen dem Quell- und dem Zielende einer physischen Netzwerkverbindung berechnet. Konzeptionell handelt es sich um die Differenz zwischen dem Zeitpunkt, zu dem das letzte Byte einer Anforderung den Sender verlässt, und dem Zeitpunkt, zu dem das letzte Byte der Antwort wieder bei ihm eintrifft. Diese beiden Zeitstempel werden verwendet, um Anforderungs- und Antwortereignisse für eine bestehende physische Verbindung darzustellen. Die Differenz zwischen ihnen stellt die Antwortzeit für eine einzelne Anforderung dar. 

In dieser ersten Version dieses Features stellen unsere Algorithmen eine Annäherung dar, die abhängig von dem für eine bestimmte Netzwerkverbindung tatsächlich verwendeten Anwendungsprotokoll mehr oder weniger erfolgreich funktionieren kann. Beispielsweise funktioniert der aktuelle Ansatz gut bei Anforderungs-/Antwort-basierten Protokollen wie HTTP(S), er funktioniert aber nicht bei unidirektionalen oder auf Nachrichtenwarteschlangen basierenden Protokollen.

Einige wichtige Punkte sind zu beachten:

1. Wenn ein Prozess Verbindungen an der gleichen IP-Adresse, aber über mehrere Netzwerkschnittstellen annimmt, wird für jede Schnittstelle ein separater Datensatz gemeldet. 
2. Datensätze mit Platzhalter-IP-Adresse enthalten keine Aktivitäten. Sie werden aufgenommen, um die Tatsache darzustellen, dass ein Port auf dem Computer für eingehenden Datenverkehr offen ist.
3. Um Ausführlichkeit und Datenvolumen zu verringern, werden Datensätze mit Platzhalter-IP weggelassen, wenn es einen übereinstimmenden Datensatz (für den gleichen Prozess, Port und das gleiche Protokoll) mit einer bestimmten IP-Adresse gibt. Wenn ein Datensatz mit Platzhalter-IP weggelassen wird, wird die Datensatzeigenschaft „IsWildcardBind“ mit der bestimmten IP-Adresse auf „True“ festgelegt, um anzuzeigen, dass der Port über jede Schnittstelle des berichtenden Computers geöffnet ist.
4. Für Ports, die nur an eine bestimmte Schnittstelle gebunden sind, ist „IsWildcardBind“ auf „False“ festgelegt.

#### <a name="naming-and-classification"></a>Benennung und Klassifizierung
Der Einfachheit halber ist die IP-Adresse des Remoteendes einer Verbindung in der Eigenschaft „RemoteIp“ enthalten. Für eingehende Verbindungen ist „RemoteIp“ identisch mit „SourceIp“, während sie für ausgehende Verbindungen mit „DestinationIp“ übereinstimmt. Die Eigenschaft „RemoteDnsCanonicalNames“ stellt die kanonischen DNS-Namen dar, die vom Computer für „RemoteIp“ gemeldet werden. Die Eigenschaften „RemoteDnsQuestions“ und „RemoteClassification“ sind zur zukünftigen Verwendung reserviert. 

#### <a name="geolocation"></a>Geolocation
*VMConnection* enthält außerdem in den folgenden Eigenschaften des Datensatzes Geolocationinformationen für das Remoteende jeder Verbindung: 

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
|RemoteCountry |Der Name des Landes, in dem „RemoteIp“ gehostet ist.  Beispielsweise *USA* |
|RemoteLatitude |Der Breitengrad der Geolocation. Beispielsweise *47,68* |
|RemoteLongitude |Der Längengrad der Geolocation. Beispielsweise *-122,12* |

#### <a name="malicious-ip"></a>Schädliche IP-Adressen
Jede RemoteIp-Eigenschaft in der Tabelle *VMConnection* wird anhand einer Sammlung von IPs überprüft, die für schädliche Aktivitäten bekannt sind. Wenn die RemoteIp als bösartig identifiziert wurde, werden die folgenden Eigenschaften des Datensatzes aufgefüllt (sie sind leer, wenn die IP nicht als schädlich angesehen wird):

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
|MaliciousIp |Die RemoteIp-Adresse |
|IndicatorThreadType |„Bedrohungsindikator erkannt“ kann einen der folgenden Werte haben: *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos* , *MaliciousUrl*, *Malware*, *Phishing*, *Proxy*, *PUA*, *Watchlist*.   |
|BESCHREIBUNG |Beschreibung der beobachteten Bedrohung. |
|TLPLevel |TLP-Stufe (Ampelprotokoll) ist einer der definierten Werte *White*, *Green*, *Amber*, *Red*. |
|Zuverlässigkeit |Werte sind *0–100*. |
|Severity |Werte sind *0–5*, wobei *5* am schwerwiegendsten und *0* überhaupt nicht schwerwiegend ist. Der Standardwert ist *3*.  |
|FirstReportedDateTime |Die Uhrzeit, zu der der Anbieter den Indikator zum ersten Mal gemeldet hat. |
|LastReportedDateTime |Die Uhrzeit, zu der der Indikator zum letzten Mal von Interflow beobachtet wurde. |
|IsActive |Gibt an, dass die Indikatoren deaktiviert sind, mit dem Wert *true* oder *false*. |
|ReportReferenceLink |Links zu Berichten, die im Zusammenhang mit einer bestimmten Beobachtung stehen. |
|AdditionalInformation |Bietet zusätzliche Informationen, falls zutreffend, zur beobachteten Bedrohung. |

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL-Datensätze
Datensätze des Typs *ServiceMapComputer_CL* enthalten Bestandsdaten für Server mit dem Dependency-Agent. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
| Typ | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Der eindeutige Bezeichner für den Computer innerhalb des Arbeitsbereichs |
| ResourceName_s | Der eindeutige Bezeichner für den Computer innerhalb des Arbeitsbereichs |
| ComputerName_s | Der vollqualifizierte Domänenname des Computers |
| Ipv4Addresses_s | Eine Liste der IPv4-Adressen des Servers |
| Ipv6Addresses_s | Eine Liste der IPv6-Adressen des Servers |
| DnsNames_s | Ein Array von DNS-Namen |
| OperatingSystemFamily_s | Windows oder Linux |
| OperatingSystemFullName_s | Der vollständige Name des Betriebssystems  |
| Bitness_s | Die Bitanzahl des Computers (32-Bit oder 64-Bit)  |
| PhysicalMemory_d | Der physischer Speicher in MB |
| Cpus_d | Die Anzahl der CPUs |
| CpuSpeed_d | Die CPU-Geschwindigkeit in MHz|
| VirtualizationState_s | *unknown*, *physical*, *virtual*, *hypervisor* |
| VirtualMachineType_s | *hyperv*, *vmware* usw. |
| VirtualMachineNativeMachineId_g | Die VM-ID, die vom entsprechenden Hypervisor zugewiesen wurde |
| VirtualMachineName_s | Der Name der VM |
| BootTime_t | Die Startzeit |

### <a name="servicemapprocesscl-type-records"></a>Datensätze des ServiceMapProcess_CL-Typs
Datensätze des Typs *ServiceMapProcess_CL* enthalten Bestandsdaten für über TCP verbundene Prozesse auf Servern mit dem Dependency-Agent. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | BESCHREIBUNG |
|:--|:--|
| Typ | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Der eindeutige Bezeichner für den Prozess innerhalb des Arbeitsbereichs |
| ResourceName_s | Der eindeutige Bezeichner für den Prozess auf dem Computer, auf dem er ausgeführt wird|
| MachineResourceName_s | Der Ressourcenname des Computers |
| ExecutableName_s | Der Name der ausführbaren Prozessdatei |
| StartTime_t | Die Startzeit des Prozesspools |
| FirstPid_d | Die erste PID im Prozesspool |
| Description_s | Die Beschreibung des Prozesses |
| CompanyName_s | Der Name des Unternehmens |
| InternalName_s | Der interne Name |
| ProductName_s | Der Name des Produkts |
| ProductVersion_s | Die Produktversion |
| FileVersion_s | Die Dateiversion |
| CommandLine_s | Die Befehlszeile |
| ExecutablePath _s | Der Pfad zur ausführbaren Datei |
| WorkingDirectory_s | Das Arbeitsverzeichnis |
| UserName | Das Konto, unter dem der Prozess ausgeführt wird |
| UserDomain | Die Domäne, unter der der Prozess ausgeführt wird |

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

### <a name="list-all-known-machines"></a>Auflisten aller bekannten Computer
`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Auflisten der physischen Arbeitsspeicherkapazität aller verwalteten Computer
`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project PhysicalMemory_d, ComputerName_s`

### <a name="list-computer-name-dns-ip-and-os"></a>Auflisten von Computernamen, DNS, IP und Betriebssystem
`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s`

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Suchen nach allen Prozesse mit "sql" in der Befehlszeile
`ServiceMapProcess_CL | where CommandLine_s contains_cs "sql" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Suchen eines Computers (aktuellster Datensatz) anhand des Ressourcennamens
`search in (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Suchen eines Computers (aktuellster Datensatz) anhand der IP-Adresse
`search in (ServiceMapComputer_CL) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-all-known-processes-on-a-specified-machine"></a>Auflisten aller bekannten Prozesse auf einem angegebenen Computer auf
`ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-all-computers-running-sql"></a>Auflisten aller Computer, auf denen SQL ausgeführt wird
`ServiceMapComputer_CL | where ResourceName_s in ((search in (ServiceMapProcess_CL) "\*sql\*" | distinct MachineResourceName_s)) | distinct ComputerName_s`

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Auflisten aller eindeutigen Produktversionen von „curl“ im eigenen Rechenzentrum
`ServiceMapProcess_CL | where ExecutableName_s == "curl" | distinct ProductVersion_s`

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Erstellen einer Computergruppe mit allen Computern, auf denen CentOS ausgeführt wird
`ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s`

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Zusammenfassen der ausgehenden Verbindungen für eine Gruppe von Computern
```
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="next-steps"></a>Nächste Schritte
* Wenn Sie gerade erst in das Schreiben von Abfragen in Log Analytics einsteigen, arbeiten Sie [Verwenden der Log Analytics-Seite](../log-analytics/query-language/get-started-analytics-portal.md) im Azure-Portal durch, um Log Analytics-Abfragen zu erstellen.
* Erfahren Sie mehr über das [Schreiben von Suchabfragen](../log-analytics/query-language/search-queries.md).

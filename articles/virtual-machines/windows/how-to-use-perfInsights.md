---
title: Verwenden von PerfInsights in Microsoft Azure | Microsoft-Dokumentation
description: Lernen Sie, wie Sie Leistungsprobleme bei virtuellen Windows-Computern mit PerfInsights behandeln.
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: f15875610e2035c6f4c10c36e19c02f3e045b3ea
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-use-perfinsights"></a>Verwenden von PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) ist ein automatisiertes Skript, das nützliche Diagnoseinformationen sammelt. Es führt darüber hinaus E/A-Belastungen aus und bietet einen Analysebericht zur Problembehandlung bei Windows-VM-Leistungsproblemen in Azure. Es kann auf virtuellen Computern als eigenständiges Skript oder direkt über das Portal ausgeführt werden, indem Sie die [Azure-VM-Erweiterung für die Leistungsdiagnose](performance-diagnostics-vm-extension.md) installieren.

Wenn bei virtuellen Computern Leistungsprobleme auftreten, führen Sie dieses Skript aus, bevor Sie sich an den Support wenden.

## <a name="supported-troubleshooting-scenarios"></a>Unterstützte Problembehandlungsszenarien

PerfInsights kann verschiedene Arten von Informationen erfassen und analysieren. Allgemeine Szenarien werden in den folgenden Abschnitten beschrieben.

### <a name="collect-basic-configuration"></a>Erfassen der grundlegenden Konfiguration 

Dieses Szenario erfasst die Datenträgerkonfiguration und andere wichtige Informationen, u.a.:

-   Ereignisprotokolle

-   Netzwerkstatus für alle ein- und ausgehenden Verbindungen

-   Netzwerk- und Firewallkonfigurationseinstellungen

-   Aufgabenliste für alle derzeit auf dem System ausgeführten Anwendungen

-   Von msinfo32 für den virtuellen Computer erstellte Informationsdatei

-   Microsoft SQL Server-Datenbank-Konfigurationseinstellungen (wenn der virtuelle Computer als Server identifiziert wird, auf dem SQL Server ausgeführt wird)

-   Speicherzuverlässigkeitszähler

-   Wichtige Windows-Hotfixes

-   Installierte Filtertreiber

Dies ist eine passive Sammlung von Informationen, die das System nicht beeinträchtigen sollte. 

>[!Note]
>Dieses Szenario ist automatisch in jedem der folgenden Szenarien enthalten.

### <a name="benchmarking"></a>Benchmarktests

Dieses Szenario führt den [DiskSpd](https://github.com/Microsoft/diskspd)-Vergleichstest (IOPS und MBit/s) für alle Laufwerke aus, die an den virtuellen Computer angefügt sind. 

> [!Note]
> Dieses Szenario kann sich auf das System auswirken und sollte nicht auf einem Liveproduktionssystem ausgeführt werden. Falls erforderlich, führen Sie dieses Szenario in einem dedizierten Wartungsfenster aus, um Probleme zu vermeiden. Eine erhöhte Workload, die durch eine Ablaufverfolgung oder einen Vergleichstest verursacht wird, kann die Leistung Ihres virtuellen Computers beeinträchtigen.
>

### <a name="slow-vm-analysis"></a>Analyse bei langsamer VM 

Dieses Szenario führt eine [Leistungsindikator](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx)-Ablaufverfolgung mithilfe der Indikatoren aus, die in der Datei „Generalcounters.txt“ angegeben werden. Wenn der virtuelle Computer als Server identifiziert wird, auf dem SQL Server ausgeführt wird, wird eine Leistungsindikator-Ablaufverfolgung ausgeführt. Dazu werden die in der Datei „Sqlcounters.txt“ gefundenen Indikatoren verwendet, und außerdem sind Leistungsdiagnosedaten enthalten.

### <a name="slow-vm-analysis-and-benchmarking"></a>Analyse bei langsamer VM und Benchmarktests

Dieses Szenario führt eine [Leistungsindikator](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx)-Ablaufverfolgung aus, der ein [DiskSpd](https://github.com/Microsoft/diskspd)-Vergleichstest folgt. 

> [!Note]
> Dieses Szenario kann sich auf das System auswirken und sollte nicht auf einem Liveproduktionssystem ausgeführt werden. Falls erforderlich, führen Sie dieses Szenario in einem dedizierten Wartungsfenster aus, um Probleme zu vermeiden. Eine erhöhte Workload, die durch eine Ablaufverfolgung oder einen Vergleichstest verursacht wird, kann die Leistung Ihres virtuellen Computers beeinträchtigen.
>

### <a name="azure-files-analysis"></a>Azure Files-Analyse 

Dieses Szenario führt eine besondere Leistungsindikatorerfassung zusammen mit einer Ablaufverfolgung im Netzwerk durch. Die Erfassung enthält alle Indikatoren vom Typ „SMB-Client-Freigaben“ (Server Message Block). Es folgen einige der wichtigsten SMB-Client-Freigaben-Leistungsindikatoren, die Teil der Erfassung sind:

| **Typ**     | **SMB-Client-Freigaben-Indikator** |
|--------------|-------------------------------|
| IOPS         | Datenanforderungen/s             |
|              | Leseanforderungen/s             |
|              | Schreibanforderungen/s            |
| Latency      | Durchschn. Sek./Datenanforderung         |
|              | Durchschn. Sek./Lesevorgang                 |
|              | Durchschn. Sek./Schreibvorgang                |
| E/A-Größe      | Durchschn. Bytes/Datenanforderung       |
|              | Durchschn. Bytes/Lesen               |
|              | Durchschn. Bytes/Schreiben              |
| Throughput   | Datenbytes/s                |
|              | Lesebytes/s                |
|              | Schreibbytes/s               |
| Warteschlangenlänge | Durchschn. Lesewarteschlangen-Länge        |
|              | Durchschn. Schreibwarteschlangen-Länge       |
|              | Durchschn. Datenwarteschlangen-Länge        |

### <a name="custom-slow-vm-analysis"></a>Benutzerdefinierte Analyse bei langsamer VM 

Bei der Ausführung einer benutzerdefinierten Analyse bei langsamen virtuellen Computern legen Sie fest, dass Ablaufverfolgungen parallel ausgeführt werden. Sie können alle Ablaufverfolgungen (Leistungsindikator, Xperf, Netzwerk und StorPort) ausführen. Nach Abschluss der Ablaufverfolgung führt das Tool den DiskSpd-Vergleichstest aus, wenn diese Option ausgewählt ist. 

> [!Note]
> Dieses Szenario kann sich auf das System auswirken und sollte nicht auf einem Liveproduktionssystem ausgeführt werden. Falls erforderlich, führen Sie dieses Szenario in einem dedizierten Wartungsfenster aus, um Probleme zu vermeiden. Eine erhöhte Workload, die durch eine Ablaufverfolgung oder einen Vergleichstest verursacht wird, kann die Leistung Ihres virtuellen Computers beeinträchtigen.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Welche Informationen werden vom Skript gesammelt?

Informationen zu virtuellen Windows-Computern, Datenträger- oder Speicherpoolkonfiguration, Leistungsindikatoren, Protokollen und verschiedenen Ablaufverfolgungen werden erfasst. Dies hängt vom verwendeten Leistungsszenario ab. Details finden Sie in der folgenden Tabelle:

|Gesammelte Daten                              |  |  | Leistungsszenarien |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Erfassen der grundlegenden Konfiguration | Benchmarktests | Analyse bei langsamer VM | Analyse bei langsamer VM und Benchmarktests | Azure Files-Analyse | Benutzerdefinierte Analyse bei langsamer VM |
| Informationen aus Ereignisprotokollen      | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Systeminformationen               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Volumezuordnung                       | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Datenträgerzuordnung                         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Ausgeführte Aufgaben                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Speicherzuverlässigkeitszähler     | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Speicherinformationen              | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| FSUTIL-Ausgabe                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Filtertreiberinformationen               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Netstat-Ausgabe                   | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Network Configuration            | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Firewall-Konfiguration           | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| SQL Server-Konfiguration         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Leistungsdiagnose-Ablaufverfolgungen * | Ja                        | Ja                                | Ja                      |                                | Ja                  | Ja                  |
| Leistungsindikator-Ablaufverfolgung **     |                            |                                    |                          |                                |                      | Ja                  |
| SMB-Indikatorablaufverfolgung **             |                            |                                    |                          |                                | Ja                  |                      |
| SQL Server-Indikatorablaufverfolgung **      |                            |                                    |                          |                                |                      | Ja                  |
| XPerf-Ablaufverfolgung                      |                            |                                    |                          |                                |                      | Ja                  |
| StorPort-Ablaufverfolgung                   |                            |                                    |                          |                                |                      | Ja                  |
| Netzwerkablaufverfolgung                    |                            |                                    |                          |                                | Ja                  | Ja                  |
| DiskSpd-Vergleichstest-Ablaufverfolgung ***      |                            | Ja                                |                          | Ja                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Leistungsdiagnose-Ablaufverfolgung (*)

Führt ein regelbasiertes Modul im Hintergrund aus, um Daten zu sammeln und akute Leistungsprobleme zu diagnostizieren. Die folgenden Regeln werden derzeit unterstützt:

- HighCpuUsage-Regel: Erkennt Zeiträume mit hoher CPU-Auslastung und zeigt die stärksten CPU-Ressourcenverbraucher während dieser Zeiträume an.
- HighDiskUsage-Regel: Erkennt Zeiträume mit hoher Datenträgerauslastung auf physischen Datenträgern und zeigt die stärksten Datenträger-Ressourcenverbraucher während dieser Zeiträume an.
- HighResolutionDiskMetric-Regel: Zeigt IOPS, Durchsatz und E/A-Latenzmetriken pro 50 Millisekunden für jeden physischen Datenträger an. Mit der Lösung lassen sich schnell Datenträgerdrosselungs-Perioden identifizieren.
- HighMemoryUsage-Regel: Erkennt Perioden mit hoher Arbeitsspeicherauslastung und zeigt die stärksten Arbeitsspeicher-Ressourcenverbraucher während dieser Periode an.

> [!NOTE] 
> Derzeit werden Windows-Versionen unterstützt, die .NET Framework 3.5 oder höher beinhalten.

### <a name="performance-counter-trace-"></a>Leistungsindikator-Ablaufverfolgung (**)

Die Methode erfasst die folgenden Leistungsindikatoren:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk und \LogicalDisk
- \Cache\Dirty Pages, \Cache\Lazy Write Flushes/sec, \Server\Pool Nonpaged, Failures und \Server\Pool Paged Failures
- Ausgewählte Indikatoren unter \Network Interface, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments,  \Network Adapter, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \Network QoS Policy\Packets, \Per Processor Network Interface Card Activity und \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>Für SQL Server-Instanzen
- \SQL Server:Buffer Manager, \SQLServer:Resource Pool Stats und \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, Statistics
- \SQLServer:Access Methods

#### <a name="for-azure-files"></a>Für Azure Files
\SMB Client Shares

### <a name="diskspd-benchmark-trace-"></a>DiskSpd-Vergleichstest-Ablaufverfolgung (***)
DiskSpd-E/A-Workloadtests (Betriebssystemdatenträger [Schreibzugriff] und Poollaufwerke [Lese-/Schreibzugriff])

## <a name="run-the-perfinsights-script-on-your-vm"></a>Ausführen des PerfInsights-Skripts auf Ihrem virtuellen Computer

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Was muss ich vor dem Ausführen des Skripts wissen? 

#### <a name="script-requirements"></a>Skriptanforderungen

-  Dieses Skript muss auf dem virtuellen Computer ausgeführt werden, bei dem das Leistungsproblem vorliegt. 

-  Die folgenden Betriebssysteme werden unterstützt: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016; Windows 8.1 und Windows 10.

#### <a name="possible-problems-when-you-run-the-script-on-production-vms"></a>Mögliche Probleme, wenn Sie das Skript für Produktions-VMs ausführen

-  Bei Verwendung von Benchmarktestszenarien oder des Szenarios „Benutzerdefinierte Analyse bei langsamer VM“, das für die Verwendung von XPerf oder DiskSpd konfiguriert ist, kann das Skript die Leistung des virtuellen Computers beeinträchtigen. Sie sollten diese Szenarien nicht in einer Produktionsumgebung ausführen.

-  Stellen Sie bei Verwendung von Benchmarktestszenarien oder des Szenarios „Benutzerdefinierte Analyse bei langsamer VM“, das für die Verwendung von DiskSpd konfiguriert ist, sicher, dass die E/A-Workload nicht durch eine andere Hintergrundaktivität beeinträchtigt wird.

-  Standardmäßig verwendet das Skript das temporäre Speicherlaufwerk zum Speichern von Daten. Wenn die Ablaufverfolgung für einen längeren Zeitraum aktiviert bleibt, könnte die Menge der gesammelten Daten relevant sein. Dies kann den verfügbaren Speicher auf dem temporären Datenträger reduzieren und sich damit auf jede Anwendung auswirken, die von diesem Laufwerk abhängig ist.

### <a name="how-do-i-run-perfinsights"></a>Wie führe ich PerfInsights aus? 

Sie können PerfInsights auf einem virtuellen Computer ausführen, indem Sie die [Azure-VM-Erweiterung für die Leistungsdiagnose](performance-diagnostics-vm-extension.md) installieren. Sie können PerfInsights auch als eigenständiges Skript ausführen. 

**Installieren und Ausführen von PerfInsights über das Azure-Portal**

Weitere Informationen zu dieser Option finden Sie unter [Azure-VM-Erweiterung für die Leistungsdiagnose unter Windows](performance-diagnostics-vm-extension.md#install-the-extension).  

**Ausführen des PerfInsights-Skripts im eigenständigen Modus**

Gehen Sie folgendermaßen vor, um das PerfInsights Skript auszuführen:


1. Laden Sie [PerfInsights.zip](http://aka.ms/perfinsightsdownload) herunter.

2. Entsperren Sie die Datei „PerfInsights.zip“. Klicken Sie zu diesem Zweck mit der rechten Maustaste auf die Datei „PerfInsights.zip“, und wählen Sie **Eigenschaften**. Wählen Sie auf der Registerkarte **Allgemein** die Option **Entsperren**, und klicken Sie auf **OK**. So stellen Sie sicher, dass das Skript ohne zusätzliche Sicherheitsabfragen ausgeführt wird.  

    ![Screenshot der PerfInsights-Eigenschaften, auf dem die Option „Entsperren“ hervorgehoben ist](media/how-to-use-perfInsights/unlock-file.png)

3.  Extrahieren Sie die komprimierte Datei „PerfInsights.zip“ auf das temporäre Laufwerk (standardmäßig in der Regel Laufwerk „D“). Die komprimierte Datei sollte die folgenden Dateien und Ordner enthalten:

    ![Screenshot der Dateien im ZIP-Ordner](media/how-to-use-perfInsights/file-folder.png)

4.  Öffnen Sie Windows PowerShell als Administrator, und führen Sie das Skript „PerfInsights.ps1“ aus.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Möglicherweise müssen Sie „y“ eingeben, um zu bestätigen, dass Sie die Ausführungsrichtlinie ändern möchten.

    Im Dialogfeld **Datenschutzhinweis und Zustimmung** haben Sie die Option, Diagnoseinformationen an den Microsoft-Support weiterzuleiten. Um fortzufahren, müssen Sie auch dem Lizenzvertrag zustimmen. Treffen Sie die Auswahl, und klicken Sie dann auf **Skript ausführen**.

    ![Screenshot des Dialogfelds „Datenschutzhinweis und Zustimmung“](media/how-to-use-perfInsights/disclaimer.png)

5.  Senden Sie die Fallnummer, falls verfügbar, wenn Sie das Skript ausführen. Wählen Sie dann **OK**aus.
    
    ![Screenshot des Dialogfelds „Support-ID“](media/how-to-use-perfInsights/enter-support-number.png)

6.  Wählen Sie Ihr temporäres Speicherlaufwerk. Das Skript kann automatisch den Laufwerkbuchstaben des Laufwerks erkennen. Wenn in dieser Phase Probleme auftreten, könnten Sie aufgefordert werden, das Laufwerk auszuwählen (das Standardlaufwerk ist „D“). Generierte Protokolle werden hier im Ordner „log\_collection“ gespeichert. Nachdem Sie den Laufwerkbuchstaben eingegeben oder übernommen haben, klicken Sie auf **OK**.

    ![Screenshot des Dialogfelds „Temporäres Laufwerk“](media/how-to-use-perfInsights/enter-drive.png)

7.  Wählen Sie ein Problembehandlungsszenario in der vorliegenden Liste aus.

       ![Screenshot der Liste mit Problembehandlungsszenarien](media/how-to-use-perfInsights/select-scenarios.png)

Sie können PerfInsights auch ohne Benutzeroberfläche ausführen. Der folgende Befehl führt das Problembehandlungsszenario „Analyse bei langsamer VM“ ohne Benutzeroberflächen aus. Sie werden aufgefordert, dem gleichen Haftungsausschluss und Lizenzvertrag wie in Schritt 4 zuzustimmen.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

Wenn Sie PerfInsights im unbeaufsichtigten Modus ausführen möchten, verwenden Sie den **-AcceptDisclaimerAndShareDiagnostics**-Parameter. Verwenden Sie z.B. den folgenden Befehl:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Wie behebe ich Probleme beim Ausführen des Skripts?

Wenn das Skript nicht normal beendet wird, können Sie einen konsistenten Status wiederherstellen, indem Sie das Skript wie folgt zusammen mit der Option „-Cleanup“ ausführen:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Wenn während der automatischen Erkennung des temporären Laufwerks Probleme auftreten, könnten Sie aufgefordert werden, das Laufwerk auszuwählen (das Standardlaufwerk ist „D“).

![Screenshot des Dialogfelds „Temporäres Laufwerk“](media/how-to-use-perfInsights/enter-drive.png)

Das Skript deinstalliert die Hilfsprogramme und entfernt temporäre Ordner.

### <a name="troubleshoot-other-script-issues"></a>Problembehandlung bei anderen Skriptproblemen 

Wenn bei der Ausführung des Skripts Probleme auftreten, drücken Sie STRG+C, um das Skript zu unterbrechen. Wenn auch nach mehreren Versuchen weiterhin Skriptfehler auftreten, führen Sie das Skript im Debugmodus aus, indem Sie beim Start die Parameteroption „-Debug“ verwenden.

Wenn der Fehler aufgetreten ist, kopieren Sie die vollständige Ausgabe der PowerShell-Konsole, und senden Sie sie an den Microsoft-Support-Agent, der Sie bei der Behebung des Problems unterstützt.

### <a name="how-do-i-run-the-script-in-custom-slow-vm-analysis-mode"></a>Wie führe ich das Skript im Modus „Benutzerdefinierte Analyse bei langsamer VM“ aus?

Durch Auswahl von **Custom slow VM analysis** (Benutzerdefinierte Analyse bei langsamer VM) können Sie mehrere parallele Ablaufverfolgungen aktivieren (verwenden Sie die UMSCHALTTASTE, um mehrere Ablaufverfolgungen auszuwählen):

![Screenshot der Liste von Szenarien](media/how-to-use-perfInsights/select-scenario.png)

Befolgen Sie bei der Auswahl der Szenarien für Leistungsindikator-Ablaufverfolgung, XPerf-Ablaufverfolgung, Ablaufverfolgung im Netzwerk oder StorPort-Ablaufverfolgung nach dem Starten von Ablaufverfolgungen die Anweisungen in den nachfolgenden Dialogfeldern. Versuchen Sie, das Leistungsdefizitproblem nach dem Starten der Ablaufverfolgungen zu reproduzieren.

Sie starten eine Ablaufverfolgung über das folgende Dialogfeld:

![Screenshot des Dialogfelds zum Starten der Leistungsindikator-Ablaufverfolgung](media/how-to-use-perfInsights/start-trace-message.png)

Um die Ablaufverfolgungen zu beenden, müssen Sie den Befehl in einem zweiten Dialogfeld bestätigen.

![Screenshot des Dialogfelds zum Beenden der Leistungsindikator-Ablaufverfolgung](media/how-to-use-perfInsights/stop-trace-message.png)
![Screenshot des Dialogfelds zum Beenden aller Ablaufverfolgungen](media/how-to-use-perfInsights/ok-trace-message.png)

Wenn die Ablaufverfolgungen oder Vorgänge abgeschlossen sind, wird eine neue Datei unter „D:\\log\_collection“ (oder auf dem temporären Laufwerk) angezeigt. Der Name der Datei lautet **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip**. Sie können diese Datei zur Analyse an den Support-Agent senden.

## <a name="review-the-diagnostics-report"></a>Überprüfen des Diagnoseberichts

Innerhalb der Datei **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip** finden Sie einen HTML-Bericht, der die Ergebnisse von PerfInsights detailliert beschreibt. Um den Bericht anzuzeigen, erweitern Sie die Datei **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip**, und öffnen Sie dann die Datei **PerfInsights Report.html**.

Wählen Sie die Registerkarte **Ergebnisse**.

![Screenshot des PerfInsights-Berichts](media/how-to-use-perfInsights/findingtab.png)
![Screenshot des PerfInsights-Berichts](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Bei Ergebnissen, die als „Kritisch“ eingestuft werden, handelt es sich um bekannte Probleme, die zu Leistungsproblemen führen können. Ergebnisse vom Typ „Wichtig“ sind suboptimale Konfigurationen, die nicht unbedingt Leistungsprobleme verursachen. Ergebnisse, die als „Information“ eingestuft werden, sind nur für Informationszwecke bestimmt.

Sehen Sie sich die Empfehlungen und Links zu allen kritischen und wichtigen Ergebnissen an. Dort erhalten Sie Informationen zu ihren möglichen Auswirkungen auf die Leistung sowie zu bewährten Vorgehensweisen für leistungsoptimierte Konfigurationen.

### <a name="storage-tab"></a>Registerkarte „Speicher“

Im Abschnitt **Ergebnisse** werden verschiedene Ergebnisse und Empfehlungen zum Speicher angezeigt.

Die Abschnitte **Datenträgerzuordnung** und **Volumezuordnung** beschreiben, wie logische Volumes und physische Datenträger miteinander verknüpft werden.

In der Perspektive für physische Datenträger (Datenträgerzuordnung) zeigt die Tabelle alle logischen Volumes, die auf dem Datenträger ausgeführt werden. Im folgenden Beispiel führt **PhysicalDrive2** zwei logische Volumes aus, die auf mehreren Partitionen („J“ und „H“) erstellt werden:

![Screenshot der Datenträger-Registerkarte](media/how-to-use-perfInsights/disktab.png)

In der Volume-Perspektive (Volumezuordnung) zeigen die Tabellen alle physischen Datenträger unter den einzelnen logischen Volumes an. Beachten Sie, dass Sie für RAID-Datenträger und dynamische Datenträger ein logisches Volume auf mehreren physischen Datenträgern ausführen können. Im folgenden Beispiel ist *C:\\mount* ein Bereitstellungspunkt, der als *SpannedDisk* auf den physischen Datenträgern 2 und 3 konfiguriert ist:

![Screenshot der Registerkarte „Volume“](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>Registerkarte „SQL“

Wenn der virtuelle Zielcomputer SQL Server-Instanzen hostet, sehen Sie im Bericht eine zusätzliche Registerkarte mit dem Namen **SQL**:

![Screenshot der Registerkarte „SQL“](media/how-to-use-perfInsights/sqltab.png)

Dieser Abschnitt enthält die Registerkarte **Ergebnisse** und zusätzliche Registerkarten für jede SQL Server-Instanz, die auf dem virtuellen Computer gehostet wird.

Die Registerkarte **Ergebnisse** enthält eine Liste mit allen SQL-bezogenen Leistungsproblemen, die gefunden wurden, und die dazugehörigen Empfehlungen.

Im folgenden Beispiel wird **PhysicalDrive0** (auf Laufwerk „C“ ausgeführt) angezeigt. Das liegt daran, dass sich sowohl die Datei **modeldev** als auch die Datei **modellog** auf Laufwerk „C“ befindet und sie unterschiedlichen Typs sind (z.B. Datendatei und Transaktionsprotokoll).

![Screenshot der Protokollinformationen](media/how-to-use-perfInsights/loginfo.png)

Die Registerkarten für bestimmte SQL Server-Instanzen enthalten einen allgemeinen Abschnitt, in dem grundlegende Informationen zur ausgewählten Instanz angezeigt werden. Die Registerkarten enthalten außerdem zusätzliche Abschnitte für erweiterte Informationen, einschließlich Einstellungen, Konfigurationen und Benutzeroptionen.

### <a name="diagnostic-tab"></a>Registerkarte „Diagnose“
Die Registerkarte **Diagnose** enthält Informationen zu den entsprechenden Consumern von CPU, Datenträger und Arbeitsspeicher auf dem Computer mit den höchsten Werten für die Dauer der PerfInsights-Ausführung. Hier finden Sie auch weitere Informationen zu kritischen Patches, die im System ggf. fehlen, Aufgabenlisten und wichtigen Systemereignissen. 

## <a name="references-to-the-external-tools-used"></a>Verweise auf die verwendeten externen Tools

### <a name="diskspd"></a>DiskSpd

DiskSpd ist ein Speicherladegenerator- und Leistungstesttool von Microsoft. Weitere Informationen finden Sie unter [DiskSpd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf ist ein Befehlszeilentool zum Erfassen von Ablaufverfolgungen aus dem Windows Performance Toolkit. Weitere Informationen finden Sie unter [Windows Performance Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Nächste Schritte

Sie können Diagnoseprotokolle und Berichte an den Microsoft-Support zur weiteren Prüfung hochladen. Sie werden vom Support unter Umständen aufgefordert, zur Unterstützung bei der Problembehandlung die Ausgabe zu senden, die von PerfInsights generiert wird.

Der folgende Screenshot zeigt eine Nachricht, die der an Sie gesendeten Nachricht ähnelt:

![Screenshot der Beispielnachricht vom Microsoft-Support](media/how-to-use-perfInsights/supportemail.png)

Befolgen Sie die Anweisungen in der Nachricht, um auf den Arbeitsbereich für die Dateiübertragung zuzugreifen. Zur Erhöhung der Sicherheit müssen Sie das Kennwort bei der ersten Verwendung ändern.

Nach der Anmeldung sehen Sie ein Dialogfeld zum Hochladen der von PerfInsights erfassten Datei **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip**.

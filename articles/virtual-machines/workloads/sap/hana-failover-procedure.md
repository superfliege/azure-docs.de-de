---
title: HANA-Failover zum Standort für die Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Ausführen von Failover zum Standort für die Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca4d5912d75dd7b33737f61737a209284b7a5a47
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "59616151"
---
# <a name="disaster-recovery-failover-procedure"></a>Failoverprozedur für die Notfallwiederherstellung


>[!IMPORTANT]
>Diese Dokumentation ist kein Ersatz für die Verwaltungsdokumentation zu SAP HANA oder für SAP Notes. Es wird erwartet, dass der Leser über solide Kenntnisse in Bezug auf die SAP HANA-Verwaltung und den Betrieb verfügt, vor allem in den Bereichen Sicherung, Wiederherstellung, Hochverfügbarkeit und Notfallwiederherstellung. In dieser Dokumentation werden Screenshots von SAP HANA Studio verwendet. Inhalt, Struktur und grundlegender Aufbau der Bildschirme in den SAP-Verwaltungstools sowie auch die Tools selbst können sich von Release zu Release von SAP HANA ändern.

Beim Failover auf den Standort für die Notfallwiederherstellung müssen zwei Fälle berücksichtigt werden:

- Die SAP HANA-Datenbank muss zum aktuellen Status der Daten zurückgeführt werden. Für diesen Fall steht ein Self-Service-Skript zur Verfügung, mit dem Sie das Failover durchführen können, ohne sich an Microsoft zu wenden. Für das Failback müssen Sie aber mit Microsoft zusammenarbeiten.
- Sie möchten eine Speichermomentaufnahme wiederherstellen, bei der es sich nicht um die letzte replizierte Momentaufnahme handelt. In diesem Fall müssen Sie mit Microsoft zusammenarbeiten. 

>[!NOTE]
>Die folgenden Schritte müssen auf der HANA-Einheit (große Instanzen) ausgeführt werden, die der Notfallwiederherstellungseinheit entspricht. 
 
Führen Sie die folgenden Schritte aus, um die letzten replizierten Speichermomentaufnahmen wiederherzustellen: 

1. Fahren Sie die nicht für die Produktion bestimmte Instanz von HANA auf der ausgeführten Einheit für die Notfallwiederherstellung von HANA (große Instanzen) herunter. Der Grund hierfür ist, dass eine ruhende HANA-Produktionsinstanz vorinstalliert ist.
1. Stellen Sie sicher, dass keine SAP HANA-Prozesse ausgeführt werden. Verwenden Sie für diese Überprüfung den folgenden Befehl: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. In der Ausgabe muss der Prozess **hdbdaemon** den Zustand „Beendet“ aufweisen, und es dürfen keine anderen HANA-Prozesse mit dem Zustand „Gestartet“ vorhanden sein.
1. Führen Sie auf der HANA-Einheit (große Instanzen), die sich auf der Notfallwiederherstellungsseite befindet, das Skript *azure_hana_dr_failover.pl* aus. Das Skript fragt, welche SAP HANA-SID wiederhergestellt werden soll. Geben Sie bei entsprechender Aufforderung eine (bzw. die einzige) SAP HANA-SID ein, die repliziert wurde und in der Datei *HANABackupCustomerDetails.txt* auf der HANA-Einheit (große Instanzen) am Standort der Notfallwiederherstellung verwaltet wird. 

      Wenn Sie für mehrere SAP HANA-Instanzen ein Failover durchführen möchten, müssen Sie das Skript mehrfach ausführen. Geben Sie bei entsprechender Aufforderung die SAP HANA-SID ein, für die Sie das Failover und die Wiederherstellung durchführen möchten. Nach Abschluss des Vorgangs zeigt das Skript eine Liste mit Bereitstellungspunkten der Volumes an, die der HANA-Einheit (große Instanzen) hinzugefügt werden. Diese Liste enthält auch die wiederhergestellten Notfallwiederherstellungsvolumes.

1. Binden Sie die wiederhergestellten Notfallwiederherstellungsvolumes mithilfe von Linux-Betriebssystembefehlen in die HANA-Einheit (große Instanzen) am Standort für die Notfallwiederherstellung ein. 
1. Starten Sie die ruhende SAP HANA-Produktionsinstanz.
1. Wenn Sie zur Verringerung der RPO-Zeit Protokolle der Transaktionsprotokollsicherung kopiert haben, müssen Sie diese Transaktionsprotokollsicherungen mit dem neu eingebundenen Notfallwiederherstellungsverzeichnis „hana/logbackups“ zusammenführen. Überschreiben Sie keine vorhandenen Sicherungen. Kopieren Sie neuere Sicherungen, die nicht mit der neuesten Replikation einer Speichermomentaufnahme repliziert wurden.
1. Sie können auch einzelne Dateien über die Momentaufnahme wiederherstellen, die auf dem Volume „/hana/shared/PRD“ in der Azure-Region für die Notfallwiederherstellung repliziert wurden. 

Sie können das Notfallwiederherstellungs-Failover auch ohne Auswirkungen auf die tatsächliche Replikationsbeziehung testen. Führen Sie zum Durchführen eines Testfailovers die obigen Schritte 1 und 2 aus, und fahren Sie anschließend mit dem unten angegebenen Schritt 3 fort.

>[!IMPORTANT]
>Führen Sie auf der Instanz, die Sie am Standort der Notfallwiederherstellung beim **Testen eines Failovers** mit dem in Schritt 3 vorgestellten Skript erstellt haben, *keine* Produktionstransaktionen durch. Mit dem Befehl wird einen Satz mit Volumes erstellt, die keine Beziehung zum primären Standort haben. Eine Synchronisierung zurück an den primären Standort ist daher *nicht* möglich. 

Schritt 3 für den Failovertest:

Führen Sie auf der HANA-Einheit (große Instanzen), die sich auf der Notfallwiederherstellungsseite befindet, das Skript **azure_hana_test_dr_failover.pl** aus. Dieses Skript beendet *nicht* die Replikationsbeziehung zwischen dem primären Standort und dem Standort der Notfallwiederherstellung. Stattdessen klont dieses Skript die Notfallwiederherstellungs-Speichervolumes. Nach dem Klonvorgang werden die geklonten Volumes auf den Zustand der letzten Momentaufnahme wiederhergestellt und dann auf der Notfallwiederherstellungseinheit eingebunden. Das Skript fragt, welche SAP HANA-SID wiederhergestellt werden soll. Geben Sie eine (bzw. die einzige) SAP HANA-SID ein, die repliziert wurde und in der Datei *HANABackupCustomerDetails.txt* auf der HANA-Einheit (große Instanzen) am Standort der Notfallwiederherstellung verwaltet wird. 

Wenn Sie Tests für mehrere SAP HANA-Instanzen durchführen möchten, müssen Sie das Skript mehrfach ausführen. Geben Sie bei entsprechender Aufforderung die SAP HANA-SID der Instanz ein, die Sie für das Failover testen möchten. Anschließend zeigt das Skript eine Liste mit Bereitstellungspunkten der Volumes an, die der HANA-Einheit (große Instanzen) hinzugefügt werden. Diese Liste enthält auch die geklonten Notfallwiederherstellungvolumes.

Fahren Sie mit Schritt 4 fort.

   >[!NOTE]
   >Wenn Sie ein Failover am Standort der Notfallwiederherstellung ausführen müssen, um einige vor mehreren Stunden gelöschte Daten zu retten, und die Notfallwiederherstellungsvolumes auf eine frühere Momentaufnahme festgelegt werden müssen, wenden Sie dieses Verfahren an. 

1. Fahren Sie die nicht für die Produktion bestimmte Instanz von HANA auf der ausgeführten Einheit für die Notfallwiederherstellung von HANA (große Instanzen) herunter. Der Grund hierfür ist, dass eine ruhende HANA-Produktionsinstanz vorinstalliert ist.
1. Stellen Sie sicher, dass keine SAP HANA-Prozesse ausgeführt werden. Verwenden Sie für diese Überprüfung den folgenden Befehl: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. In der Ausgabe muss der Prozess **hdbdaemon** den Zustand „Beendet“ aufweisen, und es dürfen keine anderen HANA-Prozesse mit dem Zustand „Gestartet“ vorhanden sein.
1. Überprüfen Sie, unter welchen Momentaufnahmenamen bzw. welcher SAP HANA-Sicherungs-ID der Standort für die Notfallwiederherstellung wiederhergestellt werden soll. In der Praxis handelt es sich dabei in der Regel um die neueste Momentaufnahme. Wenn Sie verlorene Daten wiederherstellen müssen, wählen Sie eine ältere Momentaufnahme.
1. Wenden Sie sich mit einer Supportanfrage mit hoher Priorität an den Azure-Support. Bitten Sie um die Wiederherstellung dieser Momentaufnahme (mit dem entsprechenden Namen und Datum) oder der HANA-Sicherungs-ID am Standort der Notfallwiederherstellung. In der Standardeinstellung wird auf der Betriebsseite nur das Volume „/hana/data“ wiederhergestellt. Wenn auch die Volumes „/hana/logbackups“ wiederhergestellt werden sollen, müssen Sie dies gesondert angeben. *Stellen Sie das Volume „/hana/shared“ nicht wieder her.* Stattdessen sollten Sie bestimmte Dateien wie etwa „global.ini“ aus dem Verzeichnis **.snapshot** und den zugehörigen Unterverzeichnissen auswählen, nachdem Sie das Volume „/hana/shared“ für das PRD-Volume erneut bereitgestellt haben. 

   Auf der Betriebsseite werden die folgenden Schritte ausgeführt:

   a. Die Replikation von Momentaufnahmen aus dem Produktionsvolume auf den Notfallwiederherstellungsvolumes wird beendet. Diese Unterbrechung könnte bereits stattgefunden haben, wenn ein Ausfall am Produktionsstandort der Grund dafür ist, dass Sie das Notfallwiederherstellungsverfahren durchführen müssen.
   
   b. Der Name der Speichermomentaufnahme oder die Momentaufnahme mit der Sicherungs-ID, den bzw. die Sie auswählen, wird auf den Notfallwiederherstellungsvolumes wiederhergestellt.
   
   c. Nach der Wiederherstellung können die Notfallwiederherstellungsvolumes in die Einheiten von HANA (große Instanzen) in der Notfallwiederherstellungsregion eingebunden werden.
      
1. Binden Sie die Notfallwiederherstellungsvolumes in die Einheit von HANA (große Instanzen) am Standort für die Notfallwiederherstellung ein. 
1. Starten Sie die ruhende SAP HANA-Produktionsinstanz.
1. Wenn Sie zur Verringerung der RPO-Zeit Protokolle der Transaktionsprotokollsicherung kopiert haben, müssen Sie diese Transaktionsprotokollsicherungen mit dem neu eingebundenen Notfallwiederherstellungsverzeichnis „hana/logbackups“ zusammenführen. Überschreiben Sie keine vorhandenen Sicherungen. Kopieren Sie neuere Sicherungen, die nicht mit der neuesten Replikation einer Speichermomentaufnahme repliziert wurden.
1. Sie können auch einzelne Dateien über die Momentaufnahme wiederherstellen, die auf dem Volume „/hana/shared/PRD“ in der Azure-Region für die Notfallwiederherstellung repliziert wurden.

Die nächsten Schritte dienen zur Wiederherstellung der SAP HANA-Produktionsinstanz auf der Grundlage der wiederhergestellten Speichermomentaufnahme und der verfügbaren Transaktionsprotokollsicherungen:

1. Ändern Sie mithilfe von SAP HANA Studio das Sicherungsverzeichnis in **/hana/logbackups**.
   ![Ändern des Sicherungsspeicherorts für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA überprüft alle Sicherungsdateispeicherorte und schlägt für die Wiederherstellung die neueste Transaktionsprotokollsicherung vor. Die Überprüfung kann einige Minuten dauern. Danach wird ein Bildschirm wie der folgende angezeigt: ![Liste mit Transaktionsprotokollsicherungen für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Passen Sie einige Standardeinstellungen an:

      - Deaktivieren Sie **Deltasicherungen verwenden**.
      - Wählen Sie **Protokollbereich initialisieren**.

   ![Aktivieren der Option „Protokollbereich initialisieren“](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. Wählen Sie **Fertig stellen** aus.

   ![Fertigstellen der Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Daraufhin sollte ein Statusfenster wie das folgende erscheinen. Beachten Sie, dass es sich bei diesem Beispiel um eine Notfallwiederherstellung einer SAP HANA-Konfiguration mit drei Knoten für horizontales Hochskalieren handelt.

![Status der Wiederherstellung](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Falls es auf dem Bildschirm **Fertig stellen** so scheint, als würde die Wiederherstellung nicht mehr reagieren, und wenn der Statusbildschirm nicht angezeigt wird, sollten Sie überprüfen, ob alle SAP HANA-Instanzen auf den Workerknoten ausgeführt werden. Starten Sie die SAP HANA-Instanzen bei Bedarf manuell.


## <a name="failback-from-a-dr-to-a-production-site"></a>Failback von einem Standort für die Notfallwiederherstellung auf den Produktionsstandort
Sie können ein Failback von einem Notfallwiederherstellungsstandort auf einen Produktionsstandort durchführen. Wir sehen uns nun ein Szenario an, bei dem das Failover auf den Standort für die Notfallwiederherstellung auf Probleme in der Azure-Produktionsregion zurückzuführen ist (und nicht darauf, dass Sie verloren gegangene Daten wiederherstellen müssen). Sie haben Ihre SAP-Produktionsworkload für einen gewissen Zeitraum am Standort für die Notfallwiederherstellung ausgeführt. Nachdem die Probleme am Produktionsstandort behoben wurden, sollten Sie ein Failback auf Ihren Produktionsstandort durchführen. Da hierbei keine Daten verloren gehen dürfen, umfasst der Wechsel zum Produktionsstandort mehrere Schritte und eine enge Zusammenarbeit mit dem Betriebsteam für SAP HANA in Azure. Nach der Behebung der Probleme müssen Sie sich mit dem Betriebsteam in Verbindung setzen, um die Rücksynchronisierung mit dem Produktionsstandort anhand eines Triggers zu initiieren.

Dies ist die erforderliche Schrittsequenz:

1. Das Betriebsteam für SAP HANA in Azure ruft den Trigger zum Synchronisieren der Produktionsspeichervolumes aus den Speichervolumes für die Notfallwiederherstellung ab, die jetzt den Produktionszustand darstellen. In diesem Zustand wird die Einheit von HANA (große Instanzen) am Produktionsstandort heruntergefahren.
1. Das Betriebsteam für SAP HANA in Azure überwacht die Replikation und stellt sicher, dass die Daten auf dem neuesten Stand sind, bevor Sie informiert werden.
1. Sie fahren die Anwendungen herunter, die die HANA-Produktionsinstanz am Standort für die Notfallwiederherstellung verwenden. Anschließend führen Sie eine HANA-Transaktionsprotokollsicherung durch. Beenden Sie als Nächstes die HANA-Instanz, die am Standort für die Notfallwiederherstellung in Einheiten von HANA (große Instanzen) ausgeführt wird.
1. Nach dem Herunterfahren der HANA-Instanz, die am Standort für die Notfallwiederherstellung in der Einheit von HANA (große Instanzen) ausgeführt wurde, synchronisiert das Betriebsteam erneut manuell die Datenträgervolumes.
1. Das Betriebsteam für SAP HANA in Azure startet die Einheit von HANA (große Instanzen) am Produktionsstandort wieder und übergibt sie an Sie. Sie vergewissern sich, dass sich die SAP HANA-Instanz beim Start der Einheit von HANA (große Instanzen) im Zustand „Heruntergefahren“ befindet.
1. Führen Sie die gleichen Datenbankwiederherstellungsschritte durch wie zuvor beim Failover auf den Standort für die Notfallwiederherstellung.

## <a name="monitor-disaster-recovery-replication"></a>Überwachen der Replikation für die Notfallwiederherstellung

Sie können den Status der Speicherreplikation mithilfe des Skripts `azure_hana_replication_status.pl` überwachen. Dieses Skript muss über eine Einheit ausgeführt werden, die am Standort für die Notfallwiederherstellung ausgeführt wird, damit es wie erwartet funktioniert. Für die Ausführung des Skripts spielt es keine Rolle, ob die Replikation aktiv ist. Das Skript kann für jede Einheit von HANA (große Instanzen) Ihres Mandanten am Standort für die Notfallwiederherstellung ausgeführt werden. Es kann nicht verwendet werden, um Details zum Startvolume abzurufen.

Rufen Sie das Skript mit diesem Befehl auf:
```
./azure_hana_replication_status.pl
```

Die Ausgabe wird nach Volume in folgende Abschnitte unterteilt:  

- Linkstatus
- Aktuelle Replikationsaktivität
- Neueste replizierte Momentaufnahme 
- Größe der neuesten Momentaufnahme
- Aktueller Zeitabstand zwischen Momentaufnahmen (zwischen der zuletzt abgeschlossenen Momentaufnahmereplikation und dem aktuellen Zeitpunkt)

Der Linkstatus lautet **Aktiv**, wenn die Verbindung zwischen Standorten nicht unterbrochen ist oder gerade ein Failoverereignis stattfindet. Die Replikationsaktivität gibt Aufschluss darüber, ob momentan Daten repliziert werden, ob sich die Aktivität im Leerlauf befindet oder ob gerade andere Aktivitäten für den Link stattfinden. Für die zuletzt replizierte Momentaufnahme sollte nur `snapmirror…` angezeigt werden. Anschließend folgt die Größe der letzten Momentaufnahme. Zuletzt wird der Zeitabstand angezeigt. Der Zeitabstand ist die Zeit zwischen der geplanten Replikation und dem Abschluss der Replikation. Der Wert für den Zeitabstand kann bei der Datenreplikation mehr als eine Stunde betragen, obwohl die Replikation gestartet wurde. Dies gilt insbesondere bei der ersten Replikation. Der Zeitabstand nimmt bis zum Abschluss der laufenden Replikation immer weiter zu.

Hier ist ein Beispiel für die Ausgabe angegeben:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```

**Nächste Schritte**
- Lesen Sie [Überwachung und Problembehandlung auf HANA-Seite](hana-monitor-troubleshoot.md).

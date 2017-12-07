---
title: Behandeln von Problemen bei Azure Migrate | Microsoft-Dokumentation
description: "Bietet eine Übersicht über bekannte Probleme im Azure Migrate-Dienst und Problembehandlungstipps für häufige Fehler."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 40faffa3f-1f44-4a72-94bc-457222ed7ac8
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: 8c7c79a23ee09a7de35252d7819d1f0e5b1d98c5
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-migrate"></a>Problembehandlung für Azure Migrate

## <a name="troubleshoot-common-errors"></a>Problembehandlung für häufige Fehler

[Azure Migrate](migrate-overview.md) bewertet lokale Workloads für die Migration zu Azure. Verwenden Sie diesen Artikel zur Problembehandlung bei der Bereitstellung und Verwendung von Azure Migrate.


**Der Collector kann mit der Projekt-ID und dem Schlüssel, die ich aus dem Portal kopiert habe, keine Verbindung mit dem Projekt herstellen.**

Stellen Sie sicher, dass Sie die richtigen Informationen kopiert und eingefügt haben. Installieren Sie Problembehandlung Microsoft Monitoring Agent (MMA) wie folgt:

1. Laden Sie [MMA](https://go.microsoft.com/fwlink/?LinkId=828603) auf den virtuellen Collector-Computer herunter.
2. Doppelklicken Sie auf die heruntergeladenen Datei, um die Installation zu starten.
3. Klicken Sie im Setup auf der Seite **Willkommen** auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
4. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, oder ändern Sie ihn, und klicken Sie anschließend auf **Weiter**.
5. Wählen Sie unter **Agent-Setupoptionen** die Optionen **Azure Log Analytics (OMS)** > **Weiter**.
6. Klicken Sie auf **Hinzufügen**, um einen neuen Log Analytics-Arbeitsbereich hinzuzufügen. Fügen Sie die Projekt-ID und den Schlüssel ein, die Sie kopiert haben. Klicken Sie auf **Weiter**.
7. Stellen Sie sicher, dass der Agent eine Verbindung mit dem Projekt herstellen kann. Ist dies nicht der Fall, überprüfen Sie die Einstellungen. Wenn der Agent eine Verbindung herstellen kann, der Collector jedoch nicht, wenden Sie sich an den Support.


**Fehler 802: Bei der Datums- und Zeitsynchronisierung tritt ein Fehler auf.**

Möglicherweise weicht die Serveruhr um mehr als fünf Minuten von der aktuellen Uhrzeit ab. Passen Sie die Uhrzeit auf dem virtuellen Collector-Computer folgendermaßen an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
2. Führen Sie „w32tm /tz“ aus, um die Zeitzone zu überprüfen.
3. Führen Sie „w32tm /resync“ aus, um die Zeit zu synchronisieren.

**Mein Projektschlüssel enthält am Ende die Zeichen „==“. Diese werden vom Collector in andere alphanumerische Zeichen codiert. Entspricht dies dem erwarteten Verhalten?**

Ja, jeder Projektschlüssel endet mit „==“. Der Collector verschlüsselt den Projektschlüssel vor der Verarbeitung.

**Als Leistungsdaten für Datenträger und Netzwerkadapter werden NULL-Werte angezeigt.**

Dies kann auftreten, wenn die Einstellungsebene für Statistiken dem vCenter-Server auf weniger als drei festgelegt ist. Ab Ebene 3 speichert vCenter VM den Leistungsverlauf virtueller Computer für Compute, Speicher und Netzwerk. Für Ebenen unter drei speichert vCenter keine Speicher- und Netzwerkdaten, sondern nur CPU- und Arbeitsspeicherdaten. In diesem Szenario werden Leistungsdaten in Azure Migrate als NULL angezeigt, und Azure Migrate stellt Größenempfehlungen für Datenträger und Netzwerke basierend auf den Metadaten bereit, die auf den lokalen Computern erfasst wurden.

Zum Aktivieren der Erfassung von Datenträger- und Netzwerkleistungsdaten ändern Sie die Einstellungsebene für Statistiken zu drei. Warten Sie dann mindestens einen Tag vor dem Ermitteln und Bewerten ihrer Umgebung. 

**Ich habe Agents installiert und die Visualisierung von Abhängigkeiten verwendet, um Gruppen zu erstellen. Jetzt zeigen die Computer nach dem Failover die Aktion „Agent installieren“ anstelle von „Abhängigkeiten anzeigen“ an.**
* Nach einem geplanten oder ungeplanten Failover werden lokale Computer abgeschaltet, und entsprechende Computer in Azure werden hochgefahren. Diese Computer beziehen eine andere MAC-Adresse. Je nachdem, ob der Benutzer die lokale IP-Adresse beibehalten möchte oder nicht, beziehen sie möglicherweise auch eine andere IP-Adresse. Wenn sich sowohl MAC- als auch IP-Adressen unterscheiden, ordnet Azure Migrate die lokalen Computern keinen Dienstzuordnungs-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.
* Nach dem Testfailover bleiben die lokalen Computer erwartungsgemäß eingeschaltet. Entsprechende Computer, die in Azure hochgefahren wurden, erhalten andere MAC-Adressen und u.U. auch andere IP-Adressen. Sofern der Benutzer ausgehenden OMS-Datenverkehr auf diesen Computer nicht sperrt, ordnet Azure Migrate die lokalen Computern keinen Dienstzuordnungs-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.


## <a name="troubleshoot-readiness-issues"></a>Behandeln von Problemen bei der Bereitschaft

**Problem** | **Behebung**
--- | ---
Starttyp nicht unterstützt | Wechseln Sie zum BIOS, bevor Sie eine Migration ausführen.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Verkleinern Sie Datenträger vor der Migration auf weniger als 4 TB. 
Datenträger am angegebenen Speicherort nicht verfügbar | Stellen Sie vor der Migration sicher, dass sich der Datenträger am Zielspeicherort befindet.
Datenträger für die angegebene Redundanz nicht verfügbar | Der Datenträger muss den in den Bewertungseinstellungen definierten Redundanzspeichertyp verwenden (standardmäßig LRS).
Datenträgereignung konnte aufgrund eines internen Fehlers nicht ermittelt werden | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen. 
Kein virtueller Computer gefunden, der die Kern- und Arbeitsspeicheranforderungen erfüllt | Azure konnte keinen geeigneten VM-Typ finden. Reduzieren Sie vor der Migration den Arbeitsspeicher und die Anzahl der Kerne auf dem lokalen Computer. 
Mindestens ein Datenträger ist ungeeignet. | Stellen Sie sicher, dass lokale Datenträger höchstens 4 TB groß sind, bevor Sie eine Migration ausführen.
Mindestens ein Netzwerkadapter ist ungeeignet. | Entfernen Sie nicht verwendete Netzwerkadapter vor der Migration vom Computer.
Eignung des virtuellen Computers konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen. 
Eignung konnte für mindestens einen Datenträger aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Netzwerkadapter aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Kein virtueller Computer für erforderliche Speicherleistung gefunden. | Die erforderliche Speicherleistung (IOPS/Durchsatz) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie vor der Migration die Speicheranforderungen für den Computer.
Kein virtueller Computer für erforderliche Netzwerkleistung gefunden. | Die erforderliche Netzwerkleistung (ein-/ausgehend) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie die Netzwerkanforderungen für den Computer. 
Kein virtueller Computer für den angegebenen Tarif gefunden. | Überprüfen Sie die Tarifeinstellungen. 
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Probleme bei der Unterstützung von Linux-Betriebssystemen | Stellen Sie sicher, dass Sie 64-Bit mit diesen unterstützten [Betriebssystemen](../virtual-machines/linux/endorsed-distros.md) ausführen.
Probleme bei der Unterstützung von Windows-Betriebssystemen | Stellen Sie sicher, dass ein unterstütztes Betriebssystem ausführen. [Weitere Informationen](concepts-assessment-calculation.md#azure-suitability-analysis)
Unbekanntes Betriebssystem. | Überprüfen Sie, ob in vCenter das richtige Betriebssystem angegeben ist, und wiederholen Sie den Ermittlungsprozess.
Erfordert Visual Studio-Abonnement. | Windows-Clientbetriebssysteme werden nur in Visual Studio (MSDN)-Abonnements unterstützt.


## <a name="collect-logs"></a>Erfassen von Protokollen

**Wie erfasse ich Protokolle auf dem virtuellen Collector-Computer?**

Die Protokollierung ist standardmäßig aktiviert. Die Protokolle befinden sich an folgenden Speicherorten:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Führen Sie folgende Schritte aus, um die Ereignisablaufverfolgung für Windows zu erfassen:

1. Öffnen Sie auf dem virtuellen Collector-Computer ein PowerShell-Befehlsfenster.
2. Führen Sie **Get-EventLog -LogName Application | export-csv eventlog.csv** aus.

**Wie erfasse ich im Portal Protokolle des Netzwerkdatenverkehrs?**

1. Öffnen Sie den Browser, navigieren Sie zum [Portal](https://portal.azure.com), und melden Sie sich an.
2. Drücken Sie F12, um die Entwicklertools zu starten. Deaktivieren Sie ggf. die Einstellung **Einträge beim Navigieren löschen**.
3. Klicken Sie auf die Registerkarte **Netzwerk**, und starten Sie die Erfassung von Netzwerkdatenverkehr:
 - Wählen Sie in Chrome die Option **Protokoll speichern**. Die Aufzeichnung sollte automatisch gestartet werden. Ein roter Kreis gibt an, dass der Datenverkehr erfasst wird. Wenn er nicht angezeigt wird, klicken Sie zum Starten auf den schwarzen Kreis.
 - In Edge/IE sollte die Aufzeichnung automatisch gestartet werden. Ist dies nicht der Fall, klicken Sie auf die grüne Wiedergabeschaltfläche.
4. Versuchen Sie, den Fehler zu reproduzieren.
5. Nachdem der Fehler während der Aufzeichnung aufgetreten ist, beenden Sie die Aufzeichnung, und speichern Sie eine Kopie der aufgezeichneten Aktivität:
 - Klicken Sie in Chrome mit der rechten Maustaste, und klicken Sie dann auf **Als HAR mit Inhalt speichern**. Daraufhin werden die Protokolle komprimiert und als HAR-Datei exportiert.
 - In Edge/IE klicken Sie auf das Symbol **Aufgezeichneten Datenverkehr exportieren**. Daraufhin werden die Protokolle komprimiert und exportiert.
6. Navigieren Sie zur Registerkarte **Konsole**, um sie auf Warnungen oder Fehler zu prüfen. So speichern Sie das Konsolenprotokoll:
 - Klicken Sie in Chrome mit der rechten Maustaste auf eine beliebige Stelle im Konsolenprotokoll. Wählen Sie **Speichern als**, um das Protokoll zu exportieren und zu komprimieren.
 - Klicken Sie in Edge/IE mit der rechten Maustaste auf die Fehler, und wählen Sie **Alle kopieren**. 
7. Schließen Sie die Entwicklertools.
 




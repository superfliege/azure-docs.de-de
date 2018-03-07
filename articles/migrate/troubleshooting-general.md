---
title: Behandeln von Problemen bei Azure Migrate | Microsoft-Dokumentation
description: "Bietet eine Übersicht über bekannte Probleme im Azure Migrate-Dienst und Problembehandlungstipps für häufige Fehler."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: troubleshooting
ms.date: 02/21/2018
ms.author: raynew
ms.openlocfilehash: 249de45dbd9bedf1b3c2d2a5957acf31d6c0d243
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="troubleshoot-azure-migrate"></a>Problembehandlung für Azure Migrate

## <a name="troubleshoot-common-errors"></a>Problembehandlung für häufige Fehler

[Azure Migrate](migrate-overview.md) bewertet lokale Workloads für die Migration zu Azure. Verwenden Sie diesen Artikel zur Problembehandlung bei der Bereitstellung und Verwendung von Azure Migrate.


**Collector kann keine Verbindung mit dem Internet herstellen**

Dies kann passieren, wenn sich der Computer, den Sie verwenden, hinter einem Proxy befindet. Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
Wenn Sie URL-basierte Firewallproxys zur Steuerung ausgehender Verbindungen verwenden, stellen Sie sicher, dass die folgenden erforderlichen Dienst-URLs zur Whitelist hinzugefügt werden:

**URL** | **Zweck**  
--- | ---
*.portal.azure.com | Erforderlich, um die Konnektivität mit dem Azure-Dienst zu überprüfen und Probleme mit der Zeitsynchronisation zu überprüfen.
*.oneget.org | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen.

**Der Collector kann mit der Projekt-ID und dem Schlüssel, die ich aus dem Portal kopiert habe, keine Verbindung mit dem Projekt herstellen.**

Stellen Sie sicher, dass Sie die richtigen Informationen kopiert und eingefügt haben. Installieren Sie zur Problembehandlung Microsoft Monitoring Agent (MMA), und vergewissern Sie sich, dass MMA eine Verbindung mit dem Projekt herstellen kann. Gehen Sie dazu wie folgt vor:

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
Nicht unterstützter Starttyp | Azure unterstützt keine virtuellen Computer mit dem Starttyp „EFI“. Vor einer Migration muss der Starttyp in „BIOS“ konvertiert werden. <br/><br/>Die entsprechenden virtuellen Computer können mit [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) migriert werden, da Azure Site Recovery den Starttyp des virtuellen Computers im Rahmen der Migration in „BIOS“ konvertiert.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Azure unterstützt Datenträger mit einer Größe von bis zu 4 TB. Verkleinern Sie Datenträger vor der Migration auf weniger als 4 TB. 
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
Für den angegebenen Tarif wurde kein virtueller Computer gefunden. | Wenn der Tarif auf „Standard“ festgelegt ist, sollten Sie den virtuellen Computer vor der Migration ggf. herabstufen. Ist der Tarif auf „Basic“ festgelegt, empfiehlt es sich unter Umständen, den Tarif der Bewertung in „Standard“ zu ändern. 
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Unbekanntes Betriebssystem | Das Betriebssystem des virtuellen Computers wurde in vCenter Server als „Anderes“ angegeben, und Azure Migrate kann deshalb nicht beurteilen, ob der virtuelle Computer für Azure bereit ist. Vergewissern Sie sich, dass das Betriebssystem auf dem Computer von Azure [unterstützt](https://aka.ms/azureoslist) wird, bevor Sie den Computer migrieren.
Bedingt unterstütztes Windows-Betriebssystem | Der Unterstützungszeitraum für das Betriebssystem ist abgelaufen. Für die [Unterstützung in Azure](https://aka.ms/WSosstatement) wird eine benutzerdefinierte Supportvereinbarung (Custom Support Agreement, CSA) benötigt. Führen Sie vor der Migration ggf. ein Upgrade des Betriebssystems durch.
Nicht unterstütztes Windows-Betriebssystem | Azure unterstützt nur [bestimmte Windows-Betriebssystemversionen](https://aka.ms/WSosstatement). Führen Sie vor der Migration zu Azure ggf. ein Upgrade des Betriebssystems durch. 
Bedingt unterstütztes Linux-Betriebssystem | Azure unterstützt nur [bestimmte Linux-Betriebssystemversionen](../virtual-machines/linux/endorsed-distros.md). Führen Sie vor der Migration zu Azure ggf. ein Upgrade des Betriebssystems durch.
Nicht unterstütztes Linux-Betriebssystem | Der Computer startet zwar unter Umständen in Azure, das Betriebssystem wird von Azure jedoch nicht unterstützt. Führen Sie vor der Migration zu Azure ggf. ein Upgrade auf eine [unterstützte Linux-Version](../virtual-machines/linux/endorsed-distros.md) durch.
Nicht unterstützte Bitanzahl für das Betriebssystem | Virtuelle Computer mit 32-Bit-Betriebssystem starten zwar unter Umständen in Azure, es empfiehlt sich jedoch, vor der Migration zu Azure ein Betriebssystemupgrade auf die 64-Bit-Version durchzuführen.
Erfordert Visual Studio-Abonnement. | Auf dem Computer wird ein Windows-Clientbetriebssystem ausgeführt. Dies wird nur in Visual Studio-Abonnements unterstützt.


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
 




---
title: Behandeln von Problemen mit der Azure SQL-Datensynchronisierung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie häufige Probleme mit der Azure SQL-Datensynchronisierung behandeln.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: 0f836a857d6f9748416fda1526a1957af4fc51e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163595"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>Behandeln von Problemen mit der SQL-Datensynchronisierung

Dieser Artikel beschreibt, wie Sie bekannte Probleme mit der Azure SQL-Datensynchronisierung behandeln. Sofern eine Problemlösung verfügbar ist, ist sie hier angegeben.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md).

## <a name="sync-issues"></a>Probleme bei der Synchronisierung

- [Die Synchronisierung lokaler Datenbanken, die dem Client-Agent zugeordnet sind, ist über die Portalbenutzeroberfläche nicht erfolgreich.](#sync-fails)

- [Meine Synchronisierungsgruppe ist im Verarbeitungszustand hängengeblieben.](#sync-stuck)

- [Meine Tabellen enthalten fehlerhafte Daten.](#sync-baddata)

- [Nach einer erfolgreichen Synchronisierung sind inkonsistente Primärschlüsseldaten vorhanden.](#sync-pkdata)

- [Die Leistung hat sich erheblich verschlechtert.](#sync-perf)

- [Mit wird eine Meldung angezeigt, die besagt, dass ich den Wert NULL nicht in die Spalte „Spalte“ einfügen kann,<column> da die Spalte keine NULL-Werte zulässt. Was bedeutet dies, und wie kann ich das Problem beheben?](#sync-nulls)

- [Wie geht die Datensynchronisierung mit Zirkelbezügen um? Diese liegen vor, wenn dieselben Daten in mehreren Synchronisierungsgruppen synchronisiert werden und sich daher ständig ändern.](#sync-circ)

### <a name="sync-fails"></a>Die Synchronisierung lokaler Datenbanken, die dem Client-Agent zugeordnet sind, ist über die Portalbenutzeroberfläche nicht erfolgreich.

Die Synchronisierung lokaler Datenbanken, die dem Client-Agent zugeordnet sind, ist über die Benutzeroberfläche des SQL-Datensynchronisierungsportals nicht erfolgreich. Auf dem lokalen Computer, auf dem der Agent ausgeführt wird, enthält das Ereignisprotokoll Fehler vom Typ „System.IO.IOException“. Diese Fehler weisen darauf hin, dass auf dem Datenträger nicht genügend Speicherplatz zur Verfügung steht.

- **Ursache**. Auf dem Laufwerk ist nicht genügend Speicherplatz verfügbar.

- **Lösung**. Schaffen Sie mehr Platz auf dem Laufwerk, auf dem sich das Verzeichnis „%TEMP%“ befindet.

### <a name="sync-stuck"></a>Meine Synchronisierungsgruppe ist im Verarbeitungszustand hängengeblieben.

Eine Synchronisierungsgruppe in der SQL-Datensynchronisierung bleibt lange Zeit im Verarbeitungszustand. Sie reagiert nicht auf den **stop**-Befehl, und das Protokoll zeigt keine neuen Einträge.

Folgende Bedingungen können dazu führen, dass eine Synchronisierungsgruppe im Verarbeitungszustand hängenbleibt:

- **Ursache**. Der Client-Agent ist offline.

- **Lösung**. Vergewissern Sie sich, dass der Client-Agent online ist, und versuchen Sie es dann noch mal.

- **Ursache**. Der Client-Agent wurde deinstalliert oder ist aus einem anderen Grund nicht vorhanden.

- **Lösung**. Falls der Client-Agent deinstalliert wurde oder nicht vorhanden ist, gehen Sie wie folgt vor:

    1. Entfernen Sie die Agent-XML-Datei aus dem Installationsordner der SQL-Datensynchronisierung (sofern die Datei vorhanden ist).
    1. Installieren Sie den Agent auf einem lokalen Computer (hierbei kann es sich um denselben oder einen anderen Computer handeln). Übermitteln Sie dann den Agent-Schlüssel, der im Portal für den als offline angezeigten Agent generiert wurde.

- **Ursache**. Der SQL-Datensynchronisierungsdienst wurde beendet.

- **Lösung**. Starten Sie den SQL-Datensynchronisierungsdienst neu.

    1. Suchen Sie im Menü **Start** nach **Dienste**.
    1. Klicken Sie in den Suchergebnissen auf **Dienste**.
    1. Suchen Sie den **SQL-Datensynchronisierungsdienst**.
    1. Falls der Status **Beendet** lautet, klicken Sie mit der rechten Maustaste auf den Dienstnamen, und wählen Sie dann **Starten** aus.

> [!NOTE]
> Wenn die obigen Informationen nicht ausreichen, um Ihre Synchronisierungsgruppe aus dem Verarbeitungszustand zu lösen, kann der Microsoft-Support den Status der Synchronisierungsgruppe zurücksetzen. Um den Status der Synchronisierungsgruppe zurücksetzen zu lassen, erstellen Sie einen Beitrag im [Forum zu Azure SQL-Datenbank](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted). Teilen Sie in diesem Beitrag Ihre Abonnement-ID und die ID der Synchronisierungsgruppe mit, die zurückgesetzt werden soll. Ein Microsoft-Supportmitarbeiter antwortet auf Ihren Beitrag und informiert Sie, wenn der Status zurückgesetzt wurde.

### <a name="sync-baddata"></a> Meine Tabellen enthalten fehlerhafte Daten.

Wenn Tabellen, die den gleichen Namen aufweisen, aber aus verschiedenen Datenbankschemas stammen, in eine Synchronisierung eingeschlossen werden, werden nach der Synchronisierung falsche Daten in den Tabellen angezeigt.

- **Ursache**. Der Bereitstellungsprozess der SQL-Datensynchronisierung verwendet die gleichen Nachverfolgungstabellen für Tabellen mit gleichem Namen, aber unterschiedlichen Schemas. Aus diesem Grund spiegeln sich Änderungen in beiden Tabellen in der gleichen Nachverfolgungstabelle wider. Dies führt zu falschen Daten während der Synchronisierung.

- **Lösung**. Achten Sie darauf, dass sich die Namen der zu synchronisierenden Tabellen unterscheiden, auch wenn sie zu unterschiedlichen Schemas in der Datenbank gehören.

### <a name="sync-pkdata"></a> Nach einer erfolgreichen Synchronisierung sind inkonsistente Primärschlüsseldaten vorhanden.

Eine Synchronisierung wird als erfolgreich gemeldet, und das Protokoll enthält keine fehlerhaften oder übersprungenen Zeilen enthält, aber Sie stellen fest, dass Primärschlüsseldaten in den Datenbanken in der Synchronisierungsgruppe nicht konsistent sind.

- **Ursache**. Dieses Ergebnis ist programmbedingt. Änderungen in einer Primärschlüsselspalte führen zu inkonsistenten Daten in den Zeilen, in denen der Primärschlüssel geändert wurde.

- **Lösung**. Achten Sie zur Vermeidung dieses Problems darauf, dass keine Daten in einer Primärschlüsselspalte geändert werden. Um das Problem zu beheben, löschen Sie die Zeile mit den inkonsistenten Daten von allen Endpunkten in der Synchronisierungsgruppe. Fügen Sie die Zeile dann erneut ein.

### <a name="sync-perf"></a> Die Leistung hat sich erheblich verschlechtert.

Die Leistung verschlechtert sich deutlich – unter Umständen so weit, dass Sie nicht einmal mehr die Benutzeroberfläche der Datensynchronisierung starten können.

- **Ursache**. Dies ist höchstwahrscheinlich auf eine Synchronisierungsschleife zurückzuführen. Eine Synchronisierungsschleife tritt auf, wenn eine Synchronisierung von Synchronisierungsgruppe A eine Synchronisierung von Synchronisierungsgruppe B auslöst, die dann eine Synchronisierung von Synchronisierungsgruppe A auslöst. Die tatsächliche Situation kann komplexer sein und mehr als zwei Synchronisierungsgruppen in der Schleife umfassen. Das Problem ist hier ein zirkuläres Auslösen der Synchronisierung, das dadurch verursacht wird, dass Synchronisierungsgruppen sich überlappen.

- **Lösung**. Die beste Lösung ist Prävention. Vergewissern Sie sich, dass Ihre Synchronisierungsgruppen keine Zirkelbezüge enthalten. Eine Zeile, die von einer Synchronisierungsgruppe synchronisiert wird, darf nicht von einer anderen Synchronisierungsgruppe synchronisiert werden.

### <a name="sync-nulls"></a> Mit wird eine Meldung angezeigt, die besagt, dass ich den Wert NULL nicht in die Spalte „Spalte“ einfügen kann,<column> da die Spalte keine NULL-Werte zulässt. Was bedeutet dies, und wie kann ich das Problem beheben? 
Diese Fehlermeldung besagt, dass eines der beiden folgenden Probleme aufgetreten ist:
-  Eine Tabelle weist keinen Primärschlüssel auf. Um dieses Problem zu beheben, fügen Sie allen Tabellen, die Sie synchronisieren möchten, einen Primärschlüssel hinzu.
-  Es gibt eine WHERE-Klausel in Ihrer CREATE INDEX-Anweisung. Die Datensynchronisierung verarbeitet diese Bedingung nicht. Um dieses Problem zu beheben, entfernen Sie die WHERE-Klausel, oder nehmen Sie die Änderungen an allen Datenbanken manuell vor. 
 
### <a name="sync-circ"></a> Wie geht die Datensynchronisierung mit Zirkelbezügen um? Diese liegen vor, wenn dieselben Daten in mehreren Synchronisierungsgruppen synchronisiert werden und sich daher ständig ändern.
Die Datensynchronisierung beseitigt Zirkelbezüge nicht. Vermeiden Sie sie deshalb unbedingt. 

## <a name="client-agent-issues"></a>Probleme mit dem Client-Agent

- [Der Client-Agent kann nicht installiert, deinstalliert oder repariert werden.](#agent-install)

- [Der Client-Agent funktioniert nach dem Abbrechen der Deinstallation nicht mehr.](#agent-uninstall)

- [Meine Datenbank ist in der Agent-Liste nicht enthalten.](#agent-list)

- [Der Client-Agent startet nicht (Fehler 1069).](#agent-start)

- [Ich kann den Agent-Schlüssel nicht übermitteln.](#agent-key)

- [Der Client-Agent kann nicht aus dem Portal gelöscht werden, wenn die zugeordnete lokale Datenbank nicht erreichbar ist.](#agent-delete)

- [Die lokale Synchronisierungs-Agent-App kann keine Verbindung mit dem lokalen Synchronisierungsdienst herstellen.](#agent-connect)

### <a name="agent-install">Der Client-Agent kann nicht installiert, deinstalliert oder repariert werden.</a>

- **Ursache**. Viele Szenarien können diesen Fehler verursachen. Um die genaue Ursache für diesen Fehler zu ermitteln, sehen Sie sich die Protokolle an.

- **Lösung**. Generieren Sie die Windows Installer-Protokolle, und untersuchen Sie diese, um die genaue Fehlerursache zu finden. Sie können die Protokollierung über eine Eingabeaufforderung aktivieren. Wenn es sich bei der heruntergeladenen AgentServiceSetup.msi-Datei um „LocalAgentHost.msi“ handelt, generieren und untersuchen Sie die Protokolldateien mithilfe der folgenden Befehlszeilen:

    -   Für Installationen: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
    -   Für Deinstallationen: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

    Sie können die Protokollierung auch für alle mit Windows Installer durchgeführten Installationen aktivieren. Im Microsoft Knowledge Base-Artikel [Aktivieren der Windows Installer-Protokollierung](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) erfahren Sie, wie Sie die Protokollierung für Windows Installer mit nur einem Klick aktivieren. Dort finden Sie auch Informationen zum Speicherort der Protokolle.

### <a name="agent-uninstall"></a> Der Client-Agent funktioniert nach dem Abbrechen der Deinstallation nicht mehr.

Der Client-Agent funktioniert nicht mehr, auch nachdem Sie die Deinstallation abgebrochen haben.

- **Ursache**. Dieses Problem tritt auf, weil der Client-Agent der SQL-Datensynchronisierung keine Anmeldeinformationen speichert.

- **Lösung**. Sie können die folgenden beiden Lösungen ausprobieren:

    -   Verwenden Sie „services.msc“, um die Anmeldeinformationen für den Client-Agent erneut einzugeben.
    -   Deinstallieren Sie den Client-Agent, und installieren Sie einen neuen. Den neuesten Client-Agent können Sie aus dem [Download Center](http://go.microsoft.com/fwlink/?linkid=221479) herunterladen und installieren.

### <a name="agent-list"></a> Meine Datenbank ist in der Agent-Liste nicht enthalten.

Wenn Sie versuchen, einer Synchronisierungsgruppe eine bereits vorhandene SQL Server-Datenbank hinzuzufügen, wird die Datenbank nicht in der Agent-Liste angezeigt.

Folgende Szenarien können diesen Fehler verursachen:

- **Ursache**. Client-Agent und Synchronisierungsgruppe befinden sich in unterschiedlichen Rechenzentren.

- **Lösung**. Der Client-Agent und die Synchronisierungsgruppe müssen sich im gleichen Rechenzentrum befinden. Hierfür stehen Ihnen zwei Möglichkeiten zur Verfügung:

    -   Erstellen Sie einen neuen Agent in dem Rechenzentrum, in dem sich die Synchronisierungsgruppe befindet. Registrieren Sie anschließend die Datenbank bei diesem Agent.
    -   Löschen Sie die aktuelle Synchronisierungsgruppe, Erstellen Sie die Synchronisierungsgruppe dann in dem Rechenzentrum neu, in dem sich der Agent befindet.

- **Ursache**. Die Datenbankliste des Client-Agents ist nicht auf dem neuesten Stand.

- **Lösung**. Beenden Sie den Client-Agent-Dienst, und starten Sie ihn neu.

    Der lokale Agent lädt die Liste mit den zugeordneten Datenbanken nur bei der ersten Übermittlung des Agent-Schlüssels herunter. Bei nachfolgenden Übermittlungen wird die Liste nicht heruntergeladen. Daher werden Datenbanken, die während der Verschiebung eines Agents registriert wurden, in der ursprünglichen Agent-Instanz nicht angezeigt.

### <a name="agent-start"></a> Der Client-Agent startet nicht (Fehler 1069).

Sie stellen fest, dass der Agent auf einem Computer, der SQL Server hostet, nicht ausgeführt wird. Wenn Sie versuchen, den Agent manuell zu starten, wird ein Dialogfeld mit folgender Fehlermeldung angezeigt: „Fehler 1069: Der Dienst konnte wegen einer fehlerhaften Anmeldung nicht gestartet werden.“

![Dialogfeld mit dem Datensynchronisierungsfehler 1069](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Ursache**. Eine wahrscheinliche Ursache für diesen Fehler ist, dass sich das Kennwort auf dem lokalen Server geändert hat, seit Sie den Agent und das Agent-Kennwort erstellt haben.

- **Lösung**. Legen Sie das Kennwort des Agents auf Ihr aktuelles Serverkennwort fest:

  1. Suchen Sie den Client-Agent-Dienst für die SQL-Datensynchronisierung.  
    a. Wählen Sie **Starten** aus.  
    b. Geben Sie **services.msc** in das Suchfeld ein.  
    c. Klicken Sie in den Suchergebnissen auf **Dienste**.  
    d. Scrollen Sie im Fenster **Dienste** zum Eintrag für den **SQL-Datensynchronisierungs-Agent**.  
  1. Klicken Sie mit der rechten Maustaste auf **SQL-Datensynchronisierungs-Agent**, und wählen Sie **Beenden** aus.
  1. Klicken Sie mit der rechten Maustaste auf **SQL-Datensynchronisierungs-Agent**, und wählen Sie **Eigenschaften** aus.
  1. Klicken Sie im Fenster **Eigenschaften des SQL-Datensynchronisierungs-Agents** auf die Registerkarte **Anmelden**.
  1. Geben Sie Ihr Kennwort in das Textfeld **Kennwort** ein.
  1. Geben Sie das Kennwort im Feld **Kennwort bestätigen** noch einmal ein.
  1. Klicken Sie auf **Apply** (Anwenden) und dann auf **OK**.
  1. Klicken Sie im Fenster **Dienste** mit der rechten Maustaste auf den Dienst **SQL-Datensynchronisierungs-Agent**, und klicken Sie anschließend auf **Starten**.
  1. Schließen Sie das Fenster **Dienste**.

### <a name="agent-key"></a> Ich kann den Agent-Schlüssel nicht übermitteln.

Sie versuchen, einen erstellten oder neu erstellten Schlüssel für einen Agent über die SqlAzureDataSyncAgent-Anwendung zu übermitteln. Die Übermittlung kann jedoch nicht abgeschlossen werden.

![Dialogfeld mit Synchronisierungsfehler: Agent-Schlüssel kann nicht übermittelt werden.](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

- **Voraussetzungen** Bevor Sie fortfahren, überprüfen Sie die folgenden Voraussetzungen:

  - Der Windows-Dienst für die SQL-Datensynchronisierung wird ausgeführt.

  - Das Dienstkonto für den Windows-Dienst „SQL-Datensynchronisierung“ verfügt über Netzwerkzugriff.

  - Der ausgehende Port 1433 ist in Ihrer lokalen Firewallregel geöffnet.

  - Die lokale IP-Adresse wird zum Server oder zur Datenbankfirewallregel für die Synchronisierung der Metadaten-Datenbank hinzugefügt.

- **Ursache**. Durch den Agent-Schlüssel wird jeder lokale Agent eindeutig identifiziert. Der Schlüssel muss zwei Bedingungen erfüllen:

  -   Der Client-Agent-Schlüssel auf dem Server für die SQL-Datensynchronisierung und dem lokalen Computer muss identisch sein.
  -   Der Client-Agent-Schlüssel kann nur einmal verwendet werden.

- **Lösung**. Sollte der Agent nicht funktionieren, ist mindestens eine dieser Bedingungen nicht erfüllt. So beheben Sie das Problem mit dem Agent:

  1. Generieren Sie einen neuen Schlüssel.
  1. Wenden Sie den neuen Schlüssel auf den Agent an.

  So wenden Sie den neuen Schlüssel auf den Agent an:

  1. Wechseln Sie im Explorer zum Installationsverzeichnis Ihres Agents. Das Standardinstallationsverzeichnis lautet C:\\Programme (x86)\\Microsoft SQL Data Sync.
  1. Doppelklicken Sie auf das Unterverzeichnis „bin“.
  1. Öffnen Sie die SqlAzureDataSyncAgent-Anwendung.
  1. Klicken Sie auf **Agent-Schlüssel übermitteln**.
  1. Fügen Sie den Schlüssel aus der Zwischenablage in das dafür vorgesehene Feld ein.
  1. Klicken Sie auf **OK**.
  1. Schließen Sie das Programm.

### <a name="agent-delete"></a> Der Client-Agent kann nicht aus dem Portal gelöscht werden, wenn die zugeordnete lokale Datenbank nicht erreichbar ist.

Wenn ein lokaler, bei einem Client-Agent für die SQL-Datensynchronisierung registrierter Endpunkt (also eine Datenbank) nicht erreichbar ist, kann der Client-Agent nicht gelöscht werden.

- **Ursache**. Der lokale Agent kann nicht gelöscht werden, da die nicht erreichbare Datenbank noch bei dem Agent registriert ist. Wenn Sie versuchen, den Agent zu löschen, versucht der Löschprozess erfolglos, die Datenbank zu erreichen.

- **Lösung**. Verwenden Sie „force delete“, um die nicht erreichbare Datenbank zu löschen.

> [!NOTE]
> Wenn nach der Verwendung von „force delete“ Tabellen mit Synchronisierungsmetadaten zurückbleiben, verwenden Sie `deprovisioningutil.exe`, um diese zu bereinigen.

### <a name="agent-connect"></a> Die lokale Synchronisierungs-Agent-App kann keine Verbindung mit dem lokalen Synchronisierungsdienst herstellen.

- **Lösung**. Probieren Sie die folgenden Schritte aus:

  1. Beenden Sie die App.  
  1. Öffnen Sie den Bereich „Komponentendienste“.  
    a. Geben Sie **services.msc** in das Suchfeld der Taskleiste ein.  
    b. Doppelklicken Sie in den Suchergebnissen auf **Dienste**.  
  1. Beenden Sie den **SQL-Datensynchronisierungsdienst**.
  1. Starten Sie den **SQL-Datensynchronisierungsdienst** neu.  
  1. Öffnen Sie die App erneut.

## <a name="setup-and-maintenance-issues"></a>Probleme bei Setup und Wartung

- [Ich erhalte eine Meldung mit dem Hinweis, dass auf dem Datenträger kein Speicherplatz mehr zur Verfügung steht.](#setup-space)

- [Ich kann meine Synchronisierungsgruppe nicht löschen.](#setup-delete)

- [Ich kann die Registrierung einer lokalen SQL Server-Datenbank nicht aufheben.](#setup-unreg)

- [Ich bin nicht zum Starten von Systemdiensten berechtigt.](#setup-perms)

- [Eine Datenbank hat den Status „Veraltet“.](#setup-date)

- [Eine Synchronisierungsgruppe hat den Status „Veraltet“.](#setup-date2)

- [Eine Synchronisierungsgruppe kann innerhalb von drei Minuten nach dem Deinstallieren oder Beenden des Agents nicht gelöscht werden.](#setup-delete2)

- [Was geschieht beim Wiederherstellen einer verloren gegangenen oder beschädigten Datenbank?](#setup-restore)

### <a name="setup-space"></a> Ich erhalte eine Meldung mit dem Hinweis, dass auf dem Datenträger kein Speicherplatz mehr zur Verfügung steht.

- **Ursache**. Die Meldung, dass auf dem Datenträger kein Speicherplatz mehr zur Verfügung steht, kann angezeigt werden, wenn übrig gebliebene Dateien gelöscht werden müssen. Dies kann durch eine Virenschutzsoftware verursacht werden oder daran liegen, dass während Löschversuchen Dateien geöffnet waren.

- **Lösung**. Löschen Sie die Synchronisierungsdateien, die sich im Ordner „%temp%“ befinden, manuell (`del \*sync\* /s`). Löschen Sie dann die Unterverzeichnisse im Ordner „%temp%“.

> [!IMPORTANT]
> Löschen Sie keine Dateien, während eine Synchronisierung ausgeführt wird.

### <a name="setup-delete"></a> Ich kann meine Synchronisierungsgruppe nicht löschen.

Der Versuch, eine Synchronisierungsgruppe zu löschen, ist nicht erfolgreich. Jedes der folgenden Szenarien kann dazu führen, dass eine Synchronisierungsgruppe sich nicht löschen lässt:

- **Ursache**. Der Client-Agent ist offline.

- **Lösung**. Vergewissern Sie sich, dass der Client-Agent online ist, und versuchen Sie es dann noch mal.

- **Ursache**. Der Client-Agent wurde deinstalliert oder ist aus einem anderen Grund nicht vorhanden.

- **Lösung**. Falls der Client-Agent deinstalliert wurde oder nicht vorhanden ist, gehen Sie wie folgt vor:  
    a. Entfernen Sie die Agent-XML-Datei aus dem Installationsordner der SQL-Datensynchronisierung (sofern die Datei vorhanden ist).  
    b. Installieren Sie den Agent auf einem lokalen Computer (hierbei kann es sich um denselben oder einen anderen Computer handeln). Übermitteln Sie dann den Agent-Schlüssel, der im Portal für den als offline angezeigten Agent generiert wurde.

- **Ursache**. Eine Datenbank ist offline.

- **Lösung**. Vergewissern Sie sich, dass alle SQL-Datenbanken und SQL Server-Datenbanken online sind.

- **Ursache**. Die Synchronisierungsgruppe führt einen Bereitstellungs- oder Synchronisierungsvorgang aus.

- **Lösung**. Warten Sie, bis der Bereitstellungs- oder Synchronisierungsvorgang beendet ist, und versuchen Sie dann erneut, die Synchronisierungsgruppe zu löschen.

### <a name="setup-unreg"></a> Ich kann die Registrierung einer lokalen SQL Server-Datenbank nicht aufheben.

- **Ursache**. Wahrscheinlich versuchen Sie, die Registrierung einer bereits gelöschten Datenbank aufzuheben.

- **Lösung**. Wenn Sie die Registrierung einer lokalen SQL Server-Datenbank aufheben möchten, wählen Sie die Datenbank aus, und klicken Sie anschließend auf **Löschen erzwingen**.

  Sollte die Datenbank dadurch nicht aus der Synchronisierungsgruppe entfernt werden, gehen Sie folgendermaßen vor:

  1. Beenden Sie den Client-Agent-Hostdienst, und starten Sie ihn neu:  
    a. Klicken Sie auf das Menü **Start**.  
    b. Geben Sie **services.msc** in das Suchfeld ein.  
    c. Doppelklicken Sie im Ergebnisbereich im Abschnitt **Programme** auf **Dienste**.  
    d. Klicken Sie mit der rechten Maustaste auf den **SQL-Datensynchronisierungsdienst**.  
    e. Falls der Dienst ausgeführt wird, beenden Sie ihn.  
    f. Klicken Sie mit der rechten Maustaste auf den Dienst, und wählen Sie **Starten** aus.  
    g. Überprüfen Sie, ob die Datenbank immer noch registriert ist. Falls sie nicht mehr registriert ist, sind keine weiteren Schritte erforderlich. Fahren Sie andernfalls mit dem nächsten Schritt fort.
  1. Öffnen Sie die Client-Agent-App (SqlAzureDataSyncAgent).
  1. Wählen Sie **Anmeldeinformationen bearbeiten** aus, und geben Sie die Anmeldeinformationen für die Datenbank ein.
  1. Fahren Sie mit der Aufhebung der Registrierung fort.

### <a name="setup-perms"></a> Ich bin nicht zum Starten von Systemdiensten berechtigt.

- **Ursache**. Dieser Fehler tritt in zwei Situationen auf:
  -   Der Benutzername/das Kennwort ist falsch.
  -   Das angegebene Benutzerkonto verfügt nicht über ausreichende Berechtigungen für die Anmeldung als Dienst.

- **Lösung**. Erteilen Sie dem Benutzerkonto die Berechtigung für die Anmeldung als Dienst.

  1. Wechseln Sie zu **Start** > **Systemsteuerung** > **Verwaltung** > **Lokale Sicherheitsrichtlinie** > **Lokale Richtlinien** > **Zuweisen von Benutzerrechten**.
  1. Wählen Sie **Anmelden als Dienst** aus.
  1. Klicken Sie im Dialogfeld **Eigenschaften** auf das Benutzerkonto.
  1. Klicken Sie auf **Apply** (Anwenden) und dann auf **OK**.
  1. Schließen Sie alle Fenster.

### <a name="setup-date"></a> Eine Datenbank hat den Status „Veraltet“.

- **Ursache**. Datenbanken, die mindestens 45 Tage vom Dienst getrennt waren (gemessen ab dem Zeitpunkt, zu dem die Datenbank offline geschaltet wurde), werden von der SQL-Datensynchronisierung entfernt. Eine Datenbank, die online geschaltet wird, nachdem sie mindestens 45 Tage offline war, weist den Status **Veraltet** auf.

- **Lösung**. Sie können den Status **Veraltet** vermeiden, indem Sie sicherstellen, dass keine Ihrer Datenbanken 45 Tage oder länger offline ist.

  Wenn eine Datenbank den Status **Veraltet** aufweist, gehen Sie folgendermaßen vor:

  1. Entfernen Sie die Datenbank mit dem Status **Veraltet** aus der Synchronisierungsgruppe.
  1. Fügen Sie die Datenbank wieder der Synchronisierungsgruppe hinzu.

  > [!WARNING]
  > Änderungen, die an der Datenbank vorgenommen wurden, während sie offline war, gehen verloren.

### <a name="setup-date2"></a> Eine Synchronisierungsgruppe hat den Status „Veraltet“.

- **Ursache**. Falls eine Änderung innerhalb der gesamten Aufbewahrungszeit von 45 Tagen nicht angewendet werden kann, kann eine Synchronisierungsgruppe den Status „Veraltet“ erhalten.

- **Lösung**. Um den Status **Veraltet** zu vermeiden, überprüfen Sie regelmäßig die Ergebnisse Ihrer Synchronisierungsaufträge in der Verlaufsanzeige. Untersuchen Sie alle Änderungen, die nicht angewendet werden konnten, und behandeln Sie die entsprechenden Probleme.

  Wenn eine Synchronisierungsgruppe den Status **Veraltet** aufweist, löschen Sie die Gruppe und erstellen sie neu.

### <a name="setup-delete2"></a> Eine Synchronisierungsgruppe kann innerhalb von drei Minuten nach dem Deinstallieren oder Beenden des Agents nicht gelöscht werden.

Sie können eine Synchronisierungsgruppe nach dem Deinstallieren oder Beenden des zugeordneten Client-Agents für die SQL-Datensynchronisierung drei Minuten lang nicht löschen.

- **Lösung**.

  1. Entfernen Sie eine Synchronisierungsgruppe, während die zugeordneten Synchronisierungs-Agents online sind (empfohlen).
  1. Falls der Agent installiert, aber offline ist, schalten Sie ihn auf dem lokalen Computer online. Warten Sie, bis der Agent im Portal der SQL-Datensynchronisierung als **online** angezeigt wird. Entfernen Sie dann die Synchronisierungsgruppe.
  1. Ist der Agent offline, weil er deinstalliert wurde, gehen Sie folgendermaßen vor:  
    a.  Entfernen Sie die Agent-XML-Datei aus dem Installationsordner der SQL-Datensynchronisierung (sofern die Datei vorhanden ist).  
    b.  Installieren Sie den Agent auf einem lokalen Computer (hierbei kann es sich um denselben oder einen anderen Computer handeln). Übermitteln Sie dann den Agent-Schlüssel, der im Portal für den als offline angezeigten Agent generiert wurde.  
    c. Versuchen Sie, die Synchronisierungsgruppe zu löschen.

### <a name="setup-restore"></a> Was geschieht beim Wiederherstellen einer verloren gegangenen oder beschädigten Datenbank?

Wenn Sie eine verloren gegangene oder beschädigte Datenbank aus einer Sicherung wiederherstellen, stellt sich möglicherweise keine Konvergenz der Daten in den Synchronisierungsgruppen ein, zu denen die Datenbank gehört.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md)  
-   [Einrichten von Azure SQL-Datensynchronisierung](sql-database-get-started-sql-data-sync.md)  
-   [Best practices for Azure SQL Data Sync (Preview)](sql-database-best-practices-data-sync.md) (Bewährte Methoden für die Azure SQL-Datensynchronisierung-Vorschauversion)  
-   [Überwachen der Azure SQL-Datensynchronisierung mit Log Analytics](sql-database-sync-monitor-oms.md)  
-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:  
    -   [Verwenden von PowerShell zum Synchronisieren zwischen mehreren Azure SQL-Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)  

Weitere Informationen zu SQL-Datenbank finden Sie hier:

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)

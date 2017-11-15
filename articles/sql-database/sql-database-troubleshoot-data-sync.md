---
title: Behandeln von Problemen mit der Azure SQL-Datensynchronisierung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie allgemeine Probleme mit der Azure SQL-Datensynchronisierung behandeln.
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bbfcac5a54f04f20dbdeeecef7c06b91128b8c6a
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="troubleshoot-issues-with-azure-sql-data-sync-preview"></a>Behandeln von Problemen mit der Azure SQL-Datensynchronisierung-Vorschauversion

In diesem Artikel erfahren Sie, wie Sie aktuelle Probleme behandeln, die dem Team für die SQL-Datensynchronisierung-Vorschauversion bekannt sind. Sofern eine Problemumgehung verfügbar ist, ist sie hier angegeben.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md).
                                                           
## <a name="my-client-agent-doesnt-work"></a>Der Client-Agent funktioniert nicht.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Beim Versuch, den Client-Agent zu verwenden, treten folgende Fehler auf:

„Fehler beim Deserialisieren von Parameter www.microsoft.com/.../05:GetBatchInfoResult. Weitere Details finden Sie unter "InnerException".“

„Meldung der inneren Ausnahme: Der Typ "Microsoft.Synchronization.ChangeBatch" ist ein ungültiger Sammlungstyp, da er keinen Standardkonstruktor besitzt.“


### <a name="cause"></a>Ursache

Dieser Fehler ist auf ein Problem mit der SQL-Datensynchronisierung-Vorschauversion zurückzuführen.

Wahrscheinlichste Ursache dieses Problems:

-   Sie verwenden Windows 8 Developer Preview. Oder:

-   .NET 4.5 ist installiert.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Der Client-Agent muss auf einem Computer installiert werden, auf dem weder Windows 8 Developer Preview ausgeführt wird noch .NET Framework 4.5 installiert ist.

## <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Der Client-Agent funktioniert nach dem Abbrechen der Deinstallation nicht mehr.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Der Client-Agent funktioniert nicht mehr, obwohl Sie die Deinstallation abgebrochen haben.

### <a name="cause"></a>Ursache

Dieses Problem tritt auf, da der Client-Agent der SQL-Datensynchronisierung-Vorschauversion keine Anmeldeinformationen speichert.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Für diesen Fall stehen zwei Lösungsoptionen zur Verfügung:

-   Erstens: Verwenden Sie „services.msc“, um Ihre Anmeldeinformationen für den Client-Agent erneut einzugeben.

-   Zweitens: Deinstallieren Sie den Client-Agent, und installieren Sie einen neuen. Den neuesten Client-Agent können Sie aus dem [Download Center](http://go.microsoft.com/fwlink/?linkid=221479) herunterladen und installieren.

## <a name="my-database-isnt-listed-in-the-agent-dropdown"></a>Meine Datenbank ist nicht in der Dropdownliste des Agents enthalten.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Wenn Sie versuchen, einer Synchronisierungsgruppe eine bereits vorhandene SQL Server-Datenbank hinzuzufügen, wird die Datenbank nicht in der Dropdownliste angezeigt.

### <a name="cause"></a>Ursache

Dieser Fehler kann verschiedene Ursachen haben:

-   Client-Agent und Synchronisierungsgruppe befinden sich in unterschiedlichen Rechenzentren.

-   Die Datenbankliste des Client-Agents ist nicht auf dem neuesten Stand.

### <a name="solution"></a>Lösung

Die Lösung hängt von der Ursache ab.

#### <a name="the-client-agent-and-sync-group-are-in-different-data-centers"></a>Client-Agent und Synchronisierungsgruppe befinden sich in unterschiedlichen Rechenzentren.

Der Client-Agent und die Synchronisierungsgruppe müssen sich im gleichen Rechenzentrum befinden. Diese Konfiguration können Sie durch eine der folgenden Aktionen erreichen:

-   Erstellen Sie einen neuen Agent in dem Rechenzentrum, in dem sich auch die Synchronisierungsgruppe befindet. Registrieren Sie anschließend die Datenbank bei diesem Agent. Weitere Informationen finden Sie unter [How To: Install a SQL Data Sync (Preview) Client Agent](#install-a-sql-data-sync-client-agent) (Installieren eines Client-Agents der SQL-Datensynchronisierung-Vorschauversion).

-   Löschen Sie die aktuelle Synchronisierungsgruppe, und erstellen Sie sie in dem Rechenzentrum, in dem sich auch der Agent befindet, neu.

#### <a name="the-client-agents-list-of-databases-is-not-current"></a>Die Datenbankliste des Client-Agents ist nicht auf dem neuesten Stand.

Beenden Sie den Client-Agent-Dienst, und starten Sie ihn neu.
Der lokale Agent lädt die Liste mit den zugeordneten Datenbanken nur bei der ersten Übermittlung des Agent-Schlüssels herunter. Bei nachfolgenden Übermittlungen wird die Liste nicht heruntergeladen. Daher werden Datenbanken, die während der Verschiebung eines Agents registriert wurden, in der ursprünglichen Agent-Instanz nicht angezeigt.

## <a name="client-agent-doesnt-start-error-1069"></a>Der Client-Agent startet nicht. (Fehler 1069)

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Sie stellen fest, dass der Agent auf einem Computer, der SQL Server hostet, nicht ausgeführt wird. Wenn Sie versuchen, den Agent manuell zu starten, wird ein Fehlerdialogfeld mit folgender Fehlermeldung angezeigt: „Fehler 1069: Der Dienst konnte wegen einer fehlerhaften Anmeldung nicht gestartet werden.“

![Dialogfeld mit dem Datensynchronisierungsfehler 1069](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

### <a name="cause"></a>Ursache

Dieser Fehler kann unter anderem auftreten, wenn sich das Kennwort auf dem lokalen Server geändert hat, seit Sie den Agent erstellt und mit einem Anmeldekennwort versehen haben.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Legen Sie das Kennwort des Agents auf Ihr aktuelles Serverkennwort fest.

1. Suchen Sie den Client-Agent-Dienst (Vorschauversion) der SQL-Datensynchronisierung-Vorschauversion.

    a. Klicken Sie auf **Start**.

    b. Geben Sie „services.msc“ in das Suchfeld ein.

    c. Klicken Sie in den Suchergebnissen auf „Dienste“.

    d. Scrollen Sie im Fenster **Dienste** zum Eintrag **SQL Data Sync (Preview) Agent Preview** (SQL-Datensynchronisierung-Vorschauversion: Agent-Vorschau).

2. Klicken Sie mit der rechten Maustaste auf den Eintrag, und klicken Sie auf **Beenden**.

3. Klicken Sie mit der rechten Maustaste auf den Eintrag, und klicken Sie auf **Eigenschaften**.

4. Klicken Sie im Fenster **SQL Data Sync (Preview) Agent Preview Properties** (Eigenschaften von SQL-Datensynchronisierung-Vorschauversion: Agent-Vorschau) auf die Registerkarte **Anmelden**.

5. Geben Sie Ihr Kennwort in das Textfeld „Kennwort“ ein.

6. Bestätigen Sie Ihr Kennwort im Textfeld „Kennwort bestätigen“.

7. Klicken Sie auf **Übernehmen** und anschließend auf **OK**.

8. Klicken Sie im Fenster **Dienste** mit der rechten Maustaste auf den Dienst **SQL Data Sync (Preview) Agent Preview** (SQL-Datensynchronisierung-Vorschauversion: Agent-Vorschau), und klicken Sie anschließend auf **Starten**.

9. Schließen Sie das Fenster **Dienste**.

## <a name="i-get-a-disk-out-of-space-message"></a>Ich erhalte eine Meldung mit dem Hinweis, dass auf dem Datenträger kein Speicherplatz mehr zur Verfügung steht.

### <a name="cause"></a>Ursache

Die Meldung über unzureichenden Speicherplatz kann angezeigt werden, wenn Dateien nicht ordnungsgemäß gelöscht wurden. Dieser Fall kann auf Antivirensoftware oder auf Dateien zurückzuführen sein, die zum Zeitpunkt des Löschvorgangs geöffnet waren.

### <a name="solution"></a>Lösung

Löschen Sie die Synchronisierungsdateien manuell unter `%temp%` (`del \*sync\* /s`), und entfernen Sie anschließend auch die Unterverzeichnisse.

> [!IMPORTANT]
> Warten Sie mit dem Löschen von Dateien, bis die Synchronisierung abgeschlossen ist.

## <a name="i-cannot-delete-my-sync-group"></a>Ich kann meine Synchronisierungsgruppe nicht löschen.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Der Versuch, eine Synchronisierungsgruppe zu löschen, ist nicht erfolgreich.

### <a name="causes"></a>Ursachen

Wenn sich eine Synchronisierungsgruppe nicht löschen lässt, kann das folgende Ursachen haben:

-   Der Client-Agent ist offline.

-   Der Client-Agent wurde deinstalliert oder ist aus einem anderen Grund nicht vorhanden. 

-   Eine Datenbank ist offline. 

-   Die Synchronisierungsgruppe führt einen Bereitstellungs- oder Synchronisierungsvorgang aus. 

### <a name="solutions"></a>Lösungen

Sollte sich eine Synchronisierungsgruppe nicht löschen lassen, überprüfen Sie Folgendes:

-   Vergewissern Sie sich, dass der Client-Agent online ist, und versuchen Sie es dann noch mal.

-   Falls der Client-Agent deinstalliert wurde oder nicht vorhanden ist, gehen Sie wie folgt vor:

    a. Entfernen Sie die Agent-XML-Datei aus dem Installationsordner der SQL-Datensynchronisierung-Vorschauversion (sofern die Datei vorhanden ist).

    b. Installieren Sie den Agent auf dem gleichen/einem anderen lokalen Computer, und übermitteln Sie über das Portal den Agent-Schlüssel, der für den als offline angezeigten Agent generiert wurde.

-   **Der Dienst der SQL-Datensynchronisierung-Vorschauversion wurde beendet.**

    a. Suchen Sie im Startmenü nach **Dienste**.

    b. Klicken Sie in den Suchergebnissen auf „Dienste“.

    c. Suchen Sie den Dienst **SQL Data Sync (Preview) Preview** (SQL-Datensynchronisierung-Vorschauversion: Vorschauversion).

    d. Falls der Status **Beendet** lautet, klicken Sie mit der rechten Maustaste auf den Dienstnamen, und klicken Sie im Kontextmenü auf **Starten**.

-   Vergewissern Sie sich, dass alle SQL-Datenbanken und SQL Server-Datenbanken online sind.

-   Warten Sie, bis der Bereitstellungs- oder Synchronisierungsvorgang abgeschlossen ist. Versuchen Sie dann erneut, die Synchronisierungsgruppe zu löschen.

## <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-associated-with-the-client-agent"></a>Die Synchronisierung lokaler, dem Client-Agent zugeordneter Datenbanken ist über die Portalbenutzeroberfläche nicht erfolgreich.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Die Synchronisierung lokaler, dem Client-Agent zugeordneter Datenbanken ist über die Portalbenutzeroberfläche der SQL-Datensynchronisierung-Vorschauversion nicht erfolgreich. Auf dem lokalen Computer, auf dem der Agent ausgeführt wird, enthält das Ereignisprotokoll Fehler vom Typ „System.IO.IOException“ mit dem Hinweis, dass auf dem Datenträger nicht genügend Speicherplatz zur Verfügung steht.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Schaffen Sie mehr Platz auf dem Laufwerk, auf dem sich das Verzeichnis „%TEMP%“ befindet.

## <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>Ich kann die Registrierung einer lokalen SQL Server-Datenbank nicht aufheben.

### <a name="cause"></a>Ursache

Wahrscheinlich versuchen Sie, die Registrierung einer bereits gelöschten Datenbank aufzuheben.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Wenn Sie die Registrierung einer lokalen SQL Server-Datenbank aufheben möchten, wählen Sie die Datenbank aus, und klicken Sie anschließend auf **Löschen erzwingen**.

Sollte die Datenbank dadurch nicht aus der Synchronisierungsgruppe entfernt werden, führen Sie die folgenden Schritte aus:

1. Beenden Sie den Client-Agent-Hostdienst, und starten Sie ihn neu.

    a. Klicken Sie auf das Startmenü.

    b. Geben Sie *services.msc* in das Suchfeld ein.

    c. Doppelklicken Sie im Ergebnisbereich im Abschnitt „Programme“ auf **Dienste**.

    d. Klicken Sie mit der rechten Maustaste auf den Dienst **SQL-Datensynchronisierung-Vorschauversion**.

    e. Falls der Dienst ausgeführt wird, beenden Sie ihn.

    f. Klicken Sie mit der rechten Maustaste, und klicken Sie anschließend auf **Starten**.

    g. Vergewissern Sie sich, dass die Datenbank nicht mehr registriert ist. Falls sie nicht mehr registriert ist, sind keine weiteren Schritte erforderlich. Fahren Sie andernfalls mit dem nächsten Schritt fort:

2. Öffnen Sie die Client-Agent-App (SqlAzureDataSyncAgent).

3. Klicken Sie auf **Anmeldeinformationen bearbeiten**, und geben Sie die Anmeldeinformationen für die Datenbank an, damit sie erreichbar ist.

4. Fahren Sie mit der Aufhebung der Registrierung fort.

## <a name="i-cannot-submit-the-agent-key"></a>Ich kann den Agent-Schlüssel nicht übermitteln.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Sie versuchen, einen erstellten oder neu erstellten Schlüssel für einen Agent über die Anwendung „SqlAzureDataSyncAgent“ zu übermitteln, die Übermittlung kann jedoch nicht abgeschlossen werden.

![Dialogfeld mit Synchronisierungsfehler: Agent-Schlüssel kann nicht übermittelt werden.](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Vergewissern Sie sich zunächst, dass folgende Bedingungen erfüllt sind, um sie als Fehlerursache auszuschließen:

-   Der Windows-Dienst für die SQL-Datensynchronisierung-Vorschauversion wird ausgeführt.

-   Das Dienstkonto für den Windows-Dienst „SQL Data Sync (Preview) Preview“ (SQL-Datensynchronisierung-Vorschauversion: Vorschauversion) verfügt über Netzwerkzugriff.

-   Der Client-Agent kann den Locatordienst kontaktieren. Vergewissern Sie sich, dass der folgende Registrierungsschlüssel den Wert „https://locator.sync.azure.com/LocatorServiceApi.svc“ hat.

    -   x86-Computer: `HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

    -   x64-Computer: `HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

### <a name="cause"></a>Ursache

Durch den Agent-Schlüssel wird jeder lokale Agent eindeutig identifiziert. Der Schlüssel muss zwei Bedingungen erfüllen:

-   Der Client-Agent-Schlüssel auf dem Server für die SQL-Datensynchronisierung-Vorschauversion und auf dem lokalen Computer muss identisch sein.

-   Der Client-Agent-Schlüssel kann nur einmal verwendet werden.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Sollte der Agent nicht funktionieren, ist mindestens eine dieser Bedingungen nicht erfüllt. So beheben Sie das Problem mit dem Agent:

1. Generieren Sie einen neuen Schlüssel.

2. Wenden Sie den neuen Schlüssel auf den Agent an.

Führen Sie die folgenden Schritte aus, um den neuen Schlüssel auf den Agent anzuwenden:

1. Navigieren Sie im Explorer zum Installationsverzeichnis Ihres Agents. (Standardinstallationsverzeichnis: `c:\\program files (x86)\\microsoft sql data sync`)

2. Doppelklicken Sie auf das Unterverzeichnis `bin`.

3. Starten Sie die Anwendung `SqlAzureDataSyncAgent`.

4. Klicken Sie auf **Submit Agent Key** (Agent-Schlüssel übermitteln).

5. Fügen Sie den Schlüssel aus der Zwischenablage in das dafür vorgesehene Feld ein.

6. Klicken Sie auf **OK**.

7. Schließen Sie das Programm.

## <a name="i-do-not-have-sufficient-privileges-to-start-system-services"></a>Ich bin nicht zum Starten von Systemdiensten berechtigt.

### <a name="cause"></a>Ursache

Dieser Fehler tritt in zwei Situationen auf:

-   Der Benutzername/das Kennwort ist falsch.

-   Das angegebene Benutzerkonto verfügt nicht über ausreichende Berechtigungen für die Anmeldung als Dienst.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Erteilen Sie dem Benutzerkonto Anmeldeinformationen für die Anmeldung als Dienst.

1. Navigieren Sie zu **Start > Systemsteuerung > System und Sicherheit > Verwaltung > Lokale Sicherheitsrichtlinie > Lokale Richtlinien > Zuweisen von Benutzerrechten**.

2. Klicken Sie auf den Eintrag **Anmelden als Dienst**.

3. Fügen Sie das Benutzerkonto im Dialogfeld **Eigenschaften von Anmelden als Dienst** hinzu.

4. Klicken Sie auf **Übernehmen** und anschließend auf **OK**.

5. Schließen Sie die Fenster.

## <a name="local-sync-agent-app-is-unable-to-connect-to-the-local-sync-service"></a>Die lokale Synchronisierungs-Agent-App kann keine Verbindung mit dem lokalen Synchronisierungsdienst herstellen.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Probieren Sie die folgenden Schritte aus:

1. Beenden Sie die App.

2. Öffnen Sie den Bereich „Komponentendienste“.

    a. Geben Sie auf der Taskleiste die Zeichenfolge „Services.msc“ in das Suchfeld ein.

    b. Doppelklicken Sie in den Suchergebnissen auf „Dienste“.

3. Beenden Sie den Dienst „SQL Data Sync (Preview) Preview“ (SQL-Datensynchronisierung-Vorschauversion: Vorschauversion), und starten Sie ihn anschließend wieder.

4. Starten Sie die App neu.

## <a name="install-uninstall-or-repair-fails"></a>Die Installation, Deinstallation oder Reparatur ist nicht erfolgreich.

### <a name="cause"></a>Ursache

Für diesen Fehler gibt es verschiedene Ursachen. Die genaue Ursache für diesen Fehler muss anhand der Protokolle ermittelt werden.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Zur Ermittlung der genauen Fehlerursache müssen Sie die Windows Installer-Protokolle generieren und untersuchen. Die Protokollierung kann über die Befehlszeile aktiviert werden. Angenommen, die heruntergeladene Datei vom Typ „AgentServiceSetup.msi“ ist „LocalAgentHost.msi“. Verwenden Sie zum Generieren und Überprüfen von Protokolldateien die folgenden Befehlszeilen:

-   Für Installationen: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`

-   Für Deinstallationen: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

Die Protokollierung kann auch für alle mit Windows Installer durchgeführten Installationen aktiviert werden. Im Microsoft Knowledge Base-Artikel [Aktivieren der Windows Installer-Protokollierung](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) erfahren Sie, wie Sie die Protokollierung für Windows Installer mit nur einem Klick aktivieren. Dort finden Sie auch Informationen zum Speicherort der Protokolle.

## <a name="a-database-has-an-out-of-date-status"></a>Eine Datenbank hat den Status „Veraltet“.

### <a name="cause"></a>Ursache

Datenbanken, die mindestens 45 Tage offline waren (gemessen ab dem Zeitpunkt, zu dem die Datenbank offline geschaltet wurde), werden von der SQL-Datensynchronisierung-Vorschauversion aus dem Dienst entfernt. Eine Datenbank, die online geschaltet wird, nachdem sie mindestens 45 Tage offline war, hat den Status „Veraltet“.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Sie können den Status „Veraltet“ vermeiden, indem Sie sicherstellen, dass keine Ihrer Datenbanken 45 Tage oder länger offline ist.

Für eine Datenbank mit dem Status „Veraltet“ müssen folgende Schritte ausgeführt werden:

1. Entfernen Sie die veraltete Datenbank aus der Synchronisierungsgruppe.

2. Fügen Sie die Datenbank wieder der Synchronisierungsgruppe hinzu.

> [!WARNING]
> Änderungen, die an der Datenbank vorgenommen wurden, während sie offline war, gehen verloren.

## <a name="a-sync-group-has-an-out-of-date-status"></a>Eine Synchronisierungsgruppe hat den Status „Veraltet“.

### <a name="cause"></a>Ursache

Falls eine Änderung innerhalb der gesamten Aufbewahrungszeit von 45 Tagen nicht angewendet werden kann, kann eine Synchronisierungsgruppe den Status „Veraltet“ erhalten.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Um den Status „Veraltet“ zu vermeiden, überprüfen Sie regelmäßig die Ergebnisse Ihrer Synchronisierungsaufträge in der Verlaufsanzeige, untersuchen Sie alle Änderungen, die nicht angewendet werden konnten, und behandeln Sie die entsprechenden Probleme.

Synchronisierungsgruppen mit dem Status „Veraltet“ müssen gelöscht und neu erstellt werden.

## <a name="i-see-erroneous-data-in-my-tables"></a>Meine Tabellen enthalten fehlerhafte Daten.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Wenn Tabellen mit gleichem Namen, aber aus unterschiedlichen Schemas in einer Datenbank synchronisiert werden, werden nach der Synchronisierung in diesen Tabellen fehlerhafte Daten angezeigt.

### <a name="cause-and-fix"></a>Ursache und Lösung

Der Bereitstellungsprozess der SQL-Datensynchronisierung-Vorschauversion verwendet für Tabellen mit gleichem Namen, aber unterschiedlichen Schemas die gleichen Nachverfolgungstabellen. Dadurch gelangen Änderungen aus beiden Tabellen in die gleiche Nachverfolgungstabelle, was bei der Synchronisierung zu fehlerhaften Datenänderungen führt.

### <a name="resolution-or-workaround"></a>Lösung oder Problemumgehung

Achten Sie darauf, dass sich die Namen der zu synchronisierenden Tabellen unterscheiden, auch wenn sie zu unterschiedlichen Schemas gehören.

## <a name="i-see-inconsistent-primary-key-data-after-a-successful-synchronization"></a>Nach einer erfolgreichen Synchronisierung sind inkonsistente Primärschlüsseldaten vorhanden.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Sie stellen fest, dass Primärschlüsseldaten zwischen den Datenbanken in der Synchronisierungsgruppe nicht konsistent sind, obwohl die Synchronisierung erfolgreich durchgeführt wurde und das Protokoll keine fehlerhaften oder übersprungenen Zeilen enthält.

### <a name="cause"></a>Ursache

Dieses Verhalten ist beabsichtigt. Änderungen in einer Primärschlüsselspalte führen zu inkonsistenten Daten in den Zeilen, in denen der Primärschlüssel geändert wurde.

### <a name="resolution-or-workaround"></a>Lösung oder Problemumgehung

Achten Sie zur Vermeidung dieses Problems darauf, dass keine Daten in einer Primärschlüsselspalte geändert werden.

Sollte das Problem bereits aufgetreten sein, müssen Sie die betroffene Zeile an allen Endpunkten in der Synchronisierungsgruppe verwerfen und dann die Zeile erneut einfügen.

## <a name="i-see-a-significant-degradation-in-performance"></a>Die Leistung hat sich erheblich verschlechtert.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Die Leistung verschlechtert sich deutlich – unter Umständen so weit, dass Sie nicht einmal mehr die Benutzeroberfläche der Datensynchronisierung starten können.

### <a name="cause"></a>Ursache

Dies ist höchstwahrscheinlich auf eine Synchronisierungsschleife zurückzuführen. Eine Synchronisierungsschleife liegt vor, wenn eine Synchronisierung der Synchronisierungsgruppe A eine Synchronisierung der Synchronisierungsgruppe B auslöst, die wiederum eine Synchronisierung der Synchronisierungsgruppe A auslöst. Das tatsächliche Szenario kann komplexer sein, und die Schleife kann mehr als zwei Synchronisierungsgruppen umfassen. Der wesentliche Faktor ist jedoch die zirkuläre Auslösung von Synchronisierungen durch sich überschneidende Synchronisierungsgruppen.

### <a name="resolution-or-workaround"></a>Lösung oder Problemumgehung

Die beste Lösung ist Prävention. Vergewissern Sie sich, dass Ihre Synchronisierungsgruppen keine Zirkelverweise enthalten. Eine Zeile, die von einer Synchronisierungsgruppe synchronisiert wird, darf nicht von einer anderen Synchronisierungsgruppe synchronisiert werden.

## <a name="client-agent-cannot-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>Der Client-Agent kann nicht aus dem Portal gelöscht werden, wenn die zugeordnete lokale Datenbank nicht erreichbar ist.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Wenn ein lokaler, bei einem Client-Agent der SQL-Datensynchronisierung-Vorschauversion registrierter Endpunkt (also eine Datenbank) nicht erreichbar ist, kann der Client-Agent nicht gelöscht werden.

### <a name="cause"></a>Ursache

Der lokale Agent kann nicht gelöscht werden, da die nicht erreichbare Datenbank noch bei dem Agent registriert ist. Wenn Sie versuchen, den Agent zu löschen, versucht der Löschprozess erfolglos, die Datenbank zu erreichen.

### <a name="resolution-or-workaround"></a>Lösung oder Problemumgehung

Verwenden Sie „force delete“, um die nicht erreichbare Datenbank zu löschen.

> [!NOTE]
> Nach Verwendung von „force delete“ möglicherweise zurückgebliebene Tabellen mit Synchronisierungsmetadaten können mithilfe von „deprovisioningutil.exe“ bereinigt werden.

## <a name="a-sync-group-cannot-be-deleted-within-three-minutes-of-uninstallingstopping-the-agent"></a>Eine Synchronisierungsgruppe kann bis zu drei Minuten nach dem Deinstallieren/Beenden des Agents nicht gelöscht werden.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Eine Synchronisierungsgruppe kann bis zu drei Minuten nach dem Deinstallieren/Beenden des zugeordneten Client-Agents der SQL-Datensynchronisierung-Vorschauversion nicht gelöscht werden.

### <a name="resolution-or-workaround"></a>Lösung oder Problemumgehung

1. Entfernen Sie eine Synchronisierungsgruppe, während die zugeordneten Synchronisierungs-Agents online sind (empfohlen).

2. Falls der Agent installiert, aber offline ist, schalten Sie ihn auf dem lokalen Computer online. Warten Sie, bis der Agent im Portal der SQL-Datensynchronisierung-Vorschauversion online ist, und entfernen Sie dann die Synchronisierungsgruppe.

3. Ist der Agent offline, weil er deinstalliert wurde, führen Sie die folgenden Schritte aus. Versuchen Sie anschließend, die Synchronisierungsgruppe zu löschen.

    a.  Entfernen Sie die Agent-XML-Datei aus dem Installationsordner der SQL-Datensynchronisierung-Vorschauversion (sofern die Datei vorhanden ist).

    b.  Installieren Sie den Agent auf dem gleichen oder auf einem anderen lokalen Computer, und übermitteln Sie über das Portal den Agent-Schlüssel, der für den als offline angezeigten Agent generiert wurde.

## <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Meine Synchronisierungsgruppe ist im Verarbeitungszustand hängengeblieben.

### <a name="description-and-symptoms"></a>Beschreibung und Symptome

Eine Synchronisierungsgruppe in der SQL-Datensynchronisierung-Vorschauversion befindet sich bereits sehr lange im Verarbeitungszustand und reagiert nicht auf den Beendigungsbefehl, und die Protokolle enthalten keine neuen Einträge.

### <a name="causes"></a>Ursachen

Folgende Bedingungen können dazu führen, dass eine Synchronisierungsgruppe im Verarbeitungszustand hängenbleibt:

-   **Der Client-Agent ist offline.** Vergewissern Sie sich, dass der Client-Agent online ist, und versuchen Sie es dann noch mal.

-   **Der Client-Agent wurde deinstalliert oder ist aus einem anderen Grund nicht vorhanden.** Falls der Client-Agent deinstalliert wurde oder nicht vorhanden ist, gehen Sie wie folgt vor:

    1. Entfernen Sie die Agent-XML-Datei aus dem Installationsordner der SQL-Datensynchronisierung-Vorschauversion (sofern die Datei vorhanden ist).

    2. Installieren Sie den Agent auf dem gleichen/einem anderen lokalen Computer. Übermitteln Sie dann über das Portal den Agent-Schlüssel, der für den als offline angezeigten Agent generiert wurde.

-   **Der Dienst der SQL-Datensynchronisierung-Vorschauversion wurde beendet.**

    1. Suchen Sie im Startmenü nach **Dienste**.

    2. Klicken Sie in den Suchergebnissen auf „Dienste“.

    3. Suchen Sie den Dienst **SQL-Datensynchronisierung-Vorschauversion**.

    4. Falls der Status **Beendet** lautet, klicken Sie mit der rechten Maustaste auf den Dienstnamen, und klicken Sie im Kontextmenü auf **Starten**.

### <a name="solution-or-workaround"></a>Lösung oder Problemumgehung

Sollte sich das Problem nicht beheben lassen, kann der Status Ihrer Synchronisierungsgruppe vom Microsoft Support zurückgesetzt werden. Wenn Sie Ihren Status zurücksetzen lassen möchten, erstellen Sie einen Forumsbeitrag im [Azure SQL-Datenbank-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), und geben Sie Ihre Abonnement-ID und die ID der Synchronisierungsgruppe an, die zurückgesetzt werden soll. Ein Microsoft-Supportmitarbeiter antwortet auf Ihren Beitrag und informiert Sie, wenn der Status zurückgesetzt wurde.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md)
-   [Get Started with Azure SQL Data Sync](sql-database-get-started-sql-data-sync.md) (Erste Schritte mit der Azure SQL-Datensynchronisierung-Vorschauversion)
-   [Best practices for Azure SQL Data Sync (Preview)](sql-database-best-practices-data-sync.md) (Bewährte Methoden für die Azure SQL-Datensynchronisierung-Vorschauversion)

-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:
    -   [Verwenden von PowerShell zum Synchronisieren von Daten zwischen mehreren Azure SQL-­Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)

Die Schritte zum Aufheben der Registrierung eines Prozessservers unterscheidet sich je nach den zugehörigen Verbindungsstatus und dem Konfigurationsserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Aufheben der Registrierung eines Process-Servers, der in einem verbundenen Zustand befindet

1. Eine Remoteverbindung mit dem Prozessserver als Administrator.
2. Starten Sie die **Systemsteuerung** , und öffnen Sie **Programme > Programm deinstallieren**
3. Deinstallieren Sie ein Programm mit dem Namen **Microsoft Azure Site Recovery Konfigurationsprozess/Server**
4. Wenn Schritt 3 abgeschlossen ist, können Sie Deinstallieren **Microsoft Azure Site Recovery Konfigurationsprozess/Server-Abhängigkeiten**

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Aufheben der Registrierung eines Process-Servers, der in einem getrennten Zustand befindet

> [!WARNING]
> Mithilfe der unten aufgeführten Schritten sollte verwendet werden, wenn es gibt keine Möglichkeit, die virtuelle Maschine wieder aufnehmen, auf dem Prozessserver installiert wurde.

1. Melden Sie sich an Ihren Konfigurationsserver als Administrator an.
2. Öffnen Sie eine administratoreingabeaufforderung, und navigieren Sie zum Verzeichnis `%ProgramData%\ASR\home\svsystems\bin`.
3. Führen Sie nun den Befehl.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Dadurch wird die Details der Process-Server aus dem System entfernen.

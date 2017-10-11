In diesem Schritt erstellen Sie eine Firewallregel, um den testport für den Endpunkt mit Lastenausgleich (59999 wie oben angegeben) öffnen und eine andere Regel den Port des verfügbarkeitsgruppenlisteners zu öffnen. Da Sie den Endpunkt mit Lastenausgleich auf den virtuellen Computern, die verfügbarkeitsgruppenreplikaten enthalten erstellt, müssen Sie den testport und den Listenerport auf den entsprechenden VMs öffnen.

1. Starten Sie auf virtuellen Computern, die Replikate hosten, **Windows-Firewall mit erweiterter Sicherheit**.

2. Mit der rechten Maustaste **eingehende Regeln**, und klicken Sie dann auf **neue Regel**.

3. Auf der **Regeltyp** Seite **Port**, und klicken Sie dann auf **Weiter**.

4. Auf der **Protokoll und Ports** Seite **TCP**, Typ **59999** in der **bestimmte lokale Ports** Feld, und klicken Sie dann auf **Weiter**.

5. Auf der **Aktion** Seite, behalten **Verbindung zulassen** ausgewählt, und klicken Sie dann auf **Weiter**.

6. Auf der **Profil** Seite die Standardeinstellungen übernehmen, und klicken Sie dann auf **Weiter**.

7. Auf der **Namen** Seite in der **Namen** Text geben einen Regelnamen an, wie z. B. **immer auf Prüfpunkt Überwachungsport**, und klicken Sie dann auf **Fertig stellen**.

8. Wiederholen Sie die vorherigen Schritte für den Port des verfügbarkeitsgruppenlisteners (wie weiter oben in der $EndpointPort-Parameter des Skripts angegeben), und geben Sie einen geeigneten Regelnamen wie z. B. **immer auf Überwachungsport**.


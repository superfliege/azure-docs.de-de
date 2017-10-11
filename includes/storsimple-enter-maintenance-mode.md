<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Um den Wartungsmodus
1. Wählen Sie im Menü seriellen Konsole Option 1, **Anmelden mit Vollzugriff**.
2. Geben Sie das Kennwort ein. Das Standardkennwort lautet **Password1**.
3. Geben Sie an der Eingabeaufforderung
   
     `Enter-HcsMaintenanceMode`
4. Daraufhin wird eine Warnmeldung, die besagt, dass Wartungsmodus alle e/a-Anforderungen unterbrochen werden, und trennt die Verbindung zum klassischen Azure-Portal, und Sie zur Bestätigung aufgefordert werden werden. Typ **Y** Wartungsmodus eingeben.
   
    Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine andere Meldung darauf hingewiesen, dass das Gerät im Wartungsmodus befindet.


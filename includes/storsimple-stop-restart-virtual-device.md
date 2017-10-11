#### <a name="to-stop-and-start-a-virtual-device"></a>Beenden und starten ein virtuelles Gerät
Um ein virtuelles Gerät beenden möchten, klicken Sie auf dessen Namen, und klicken Sie dann auf **Herunterfahren**. Während das virtuelle Gerät heruntergefahren wird, wird sein Status **beenden**. Nachdem das virtuelle Gerät beendet wird, wird sein Status **beendet**.

Verwenden Sie die folgenden Cmdlets zum Beenden und starten ein virtuelles Gerät.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-virtual-device"></a>Um ein virtuelles Gerät neu zu starten.
Wenn ein virtuelles Gerät ausgeführt wird, und Sie ihn neu starten möchten, klicken Sie auf dessen Namen, und klicken Sie dann auf **Neustart**. Während das virtuelle Gerät neu gestartet wird, wird sein Status **neu**. Wenn das virtuelle Gerät für die Verwendung bereit ist, wird sein Status **ausführen**.

Verwenden Sie das folgende Cmdlet, um ein virtuelles Gerät neu zu starten.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`


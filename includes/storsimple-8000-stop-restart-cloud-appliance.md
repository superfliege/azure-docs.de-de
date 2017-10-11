#### <a name="to-stop-and-start-a-cloud-appliance"></a>Beenden und starten eine Cloud-Anwendung

1. Um eine Cloud-Anwendung zu beenden, wechseln Sie zu dem virtuellen Computer für Ihre Cloud-Anwendung.
    ![Cloud StorSimple Appliance-VM](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. Klicken Sie auf der Befehlsleiste auf **beenden**.

    ![Cloud StorSimple Appliance-VM](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**.

    ![Cloud StorSimple Appliance-VM](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. Wenn Sie einen virtuellen Computer beenden, ruft er freigegeben. Während der Cloud-Anwendung beendet wird, wird sein Status **Deallocating**. Nach dem Cloud-Anwendung beendet wird, wird sein Status **beendet (Zuordnung aufgehoben)**.

    ![Cloud StorSimple Appliance-VM](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. Sobald ein virtueller Computer beendet wird, klicken Sie auf **starten** (Schaltfläche verfügbar) auf den virtuellen Computer zu starten. Nach dem Cloud-Anwendung gestartet wurde, wird sein Status **gestartet**.

    ![Cloud StorSimple Appliance-VM](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

Verwenden Sie die folgenden Cmdlets zum Beenden und starten eine Cloud-Anwendung.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-cloud-appliance"></a>Um eine Cloud-Anwendung neu zu starten.

Um eine Cloud-Anwendung neu zu starten, wechseln Sie zu dem virtuellen Computer für Ihre Cloud-Anwendung. Klicken Sie auf der Befehlsleiste auf **Neustart**. Wenn Sie aufgefordert werden, vergewissern Sie sich mit den Neustart. Wenn die Cloud-Anwendung für die Verwendung bereit ist, wird sein Status **ausführen**.

![Cloud StorSimple Appliance-VM](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

Verwenden Sie das folgende Cmdlet, um eine Cloud-Anwendung neu zu starten.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`


Wenn Sie einen Datenträger, der an einen virtuellen Computer angefügt ist nicht mehr benötigen, können Sie problemlos trennen. Trennen eines Datenträgers entfernt den Datenträger vom virtuellen Computer jedoch nicht, löschen Sie den Datenträger aus dem Azure-Speicherkonto.

Wenn Sie die vorhandenen Daten auf dem Datenträger erneut verwenden möchten, können Sie es dieselbe virtuelle Maschine, oder ein anderer Computer anzufügen.  

> [!NOTE]
> Um ein Betriebssystem-Datenträger zu trennen, müssen Sie zuerst den virtuellen Computer löschen.
>

## <a name="find-the-disk"></a>Suchen Sie den Datenträger
Wenn Sie nicht den Namen des Datenträgers kennen oder es überprüfen, bevor Sie sie trennen möchten, gehen Sie folgendermaßen vor.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **VMs**, und wählen Sie dann den entsprechenden virtuellen Computer.

3. Klicken Sie auf **Datenträger** am linken Rand des virtuellen Computers-Dashboard unter **Einstellungen**.

 Das Dashboard des virtuellen Computers Listet den Namen und Typ, der alle angeschlossenen Datenträger. Dieser Bildschirm zeigt z. B. einen virtueller Computer mit einem Betriebssystem (BS)-Datenträger und einen Datenträger:

    ![Datenträger suchen](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a>Trennen Sie den Datenträger
1. Klicken Sie auf der Azure-Portal auf **virtuelle Maschinen**, und klicken Sie dann auf den Namen des virtuellen Computers an, die den Datenträger hat Sie trennen möchten.

2. Klicken Sie auf **Datenträger** am linken Rand des virtuellen Computers-Dashboard unter **Einstellungen**.

3. Klicken Sie auf dem Datenträger, die, den Sie trennen möchten.

  ![Identifizieren Sie den Datenträger zu trennen](./media/howto-detach-disk-windows-linux/disklist.png)

4. Klicken Sie auf der Befehlsleiste auf **trennen**.

  ![Suchen Sie die Detach-Befehl](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. Klicken Sie im Bestätigungsfenster auf **Ja** beim Trennen des Datenträgers.

  ![Vergewissern Sie sich durch das Trennen des Datenträgers](./media/howto-detach-disk-windows-linux/confirmdetach.png)

Der Datenträger im Speicher verbleibt jedoch nicht mehr an einen virtuellen Computer angefügt ist.

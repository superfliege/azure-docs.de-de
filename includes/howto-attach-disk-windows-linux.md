


## <a name="attach-an-empty-disk"></a>Einen leeren Datenträger anfügen
Anfügen eines leeren Datenträgers ist eine einfache Möglichkeit, einen Datenträger hinzugefügt werden, da Azure die VHD-Datei für Sie erstellt und speichert ihn in das Speicherkonto an.

1. Klicken Sie auf **virtuelle Computer (klassisch)**, und wählen Sie dann den entsprechenden virtuellen Computer.

2. Klicken Sie im Menü "Einstellungen" auf **Datenträger**.

   ![Fügen Sie einen neuen leeren Datenträger](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Klicken Sie auf der Befehlsleiste auf **anhängen**.  
    Die **neue Datenträger anfügen** Dialogfeld wird angezeigt.

    ![Fügen Sie einen neuen Datenträger](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Füllen Sie die folgenden Informationen ein:
    - In **Dateiname**, übernehmen Sie den Standardnamen, oder geben Sie einen für die VHD-Datei. Des Datenträgers verwendet einen automatisch generierten Name, auch wenn Sie einen anderen Namen für die VHD-Datei eingeben.
    - Wählen Sie die **Typ** des Datenträgers. Alle virtuellen Maschinen unterstützt die standard-Datenträger. Viele virtuelle Computer unterstützen auch Premium-Datenträger.
    - Wählen Sie die **Grösse (GB)** des Datenträgers.
    - Für **Hostzwischenspeicherung**auswählen, none oder schreibgeschützt.
    - Klicken Sie auf OK, um den Vorgang abzuschließen.

4. Nachdem der Datenträger erstellt und angefügt ist, wird sie im datenträgerabschnitt des virtuellen Computers aufgeführt.

   ![Neue und leere Datenträger erfolgreich angefügt.](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Nachdem Sie einen Datenträger hinzugefügt haben, müssen Sie mit dem virtuellen Computer anmelden, und initialisieren Sie den Datenträger aus, sodass sie verwendet werden kann.

## <a name="how-to-attach-an-existing-disk"></a>Vorgehensweise: vorh. Datenträger anfügen
Anfügen eines vorhandenen Datenträgers benötigen Sie eine VHD-Datei in einem Speicherkonto verfügbar. Verwenden der [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) -Cmdlet zum Hochladen der VHD-Datei mit dem Speicherkonto. Nachdem Sie erstellt und die VHD-Datei hochgeladen haben, können Sie es mit einem virtuellen Computer anfügen.

1. Klicken Sie auf **virtuelle Computer (klassisch)**, und wählen Sie dann den geeigneten virtuellen Computer.

2. Klicken Sie im Menü "Einstellungen" auf **Datenträger**.

3. Klicken Sie auf der Befehlsleiste auf **anfügen vorhandenen**.

    ![Datenträger anfügen](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Klicken Sie auf **Speicherort**. Die verfügbaren Speicherkonten anzeigen. Wählen Sie als Nächstes eine entsprechende Speicherkonto aus der Liste ein.

    ![Datenträger-Speicherkonto angeben](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. Ein **Speicherkonto** enthält mindestens einen Container, die Festplatten (Vhds) enthalten. Wählen Sie den entsprechenden Container aus der Liste aus.

    ![Bereitstellen von virtuellen Computern Windows container](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. Die **Vhds** Bereich listet die Laufwerke, die im Container frei. Klicken Sie auf einen der Datenträger, und klicken Sie dann auf aktivieren.

    ![Bereitstellen Sie Datenträger-Image für virtuelle Maschinen windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. Die **vorhandenen Datenträger anfügen** Bereich zeigt sich erneut mit dem Speicherort, der mit dem Speicherkonto, Container und ausgewählte Festplatte (Vhd) mit dem virtuellen Computer hinzufügen.

  Legen Sie **Hostzwischenspeicherung** auf none oder lesen, klicken Sie dann auf OK.

    ![Datenträger erfolgreich angefügt.](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)

<!--author=SharS last changed: 02/04/2016-->

#### <a name="to-create-a-volume"></a>So erstellen Sie ein Volume
1. Auf dem Gerät **Schnellstart** auf **Hinzufügen eines Volumes**. Dadurch wird das Volume Assistenten zum Hinzufügen eines gestartet.
2. Hinzufügen eines Assistenten Volume unter **Grundeinstellungen**, gehen Sie folgendermaßen vor:
   
   1. Geben Sie einen **Namen** für Ihr Volume.
   2. Geben Sie die **bereitgestellte Kapazität** für Ihr Volume in GB oder TB. Die Volumekapazität muss zwischen 1 GB und 64 TB für ein physisches Gerät sein.
   3. Wählen Sie in der Dropdownliste die **Verwendungstyp** für Ihr Volume. 
   4. Wenn Sie dieses Volume für Archivdaten arbeiten, wählen Sie die **verwenden Sie dieses Volume für Archivdaten, auf die häufig zugegriffen** Kontrollkästchen. Für alle anderen Fällen wählen Sie einfach **Ebenen Volume**. (Mehrstufigen Volumes wurden früher primäre Volumes genannt).
      
        ![Hinzufügen von Volumes](./media/storsimple-create-volume/ScreenshotUpdate1VolumeFlow.png)
      
      1. Klicken Sie auf das Symbol "Pfeil" ![Pfeilsymbol](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) um zur nächsten Seite zu wechseln.
3. In der **zusätzliche Einstellungen** Dialogfeld, fügen Sie einen neuen Access Control Record (ACR) hinzu:
   
   1. Geben Sie einen **Namen** für Ihren ACR.
   2. Unter **iSCSI-Initiatorname**, geben Sie die iSCSI Qualified Name (IQN) Ihres Windows-Hosts. Wenn Sie den IQN haben, fahren Sie mit [Abrufen des IQNS eines Windows Server-Hosts](#get-the-iqn-of-a-windows-server-host).
   3. Es wird empfohlen, eine standardsicherung, indem aktivieren die **standardsicherung für dieses Volume aktivieren** Kontrollkästchen. Die standardsicherung erstellt eine Richtlinie, die um 22:30 Uhr täglich (Gerätezeit) ausgeführt wird und eine Cloud-Momentaufnahme dieses Volumes erstellt.
      
      > [!NOTE]
      > Nachdem die Sicherung hier aktiviert wurde, kann sie nicht zurückgesetzt werden. Sie müssen so bearbeiten Sie das Volume, um diese Einstellung zu ändern.
      > 
      > 
      
        ![Hinzufügen von Volumes](./media/storsimple-create-volume/AddVolume2-include.png)
4. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-create-volume/HCS_CheckIcon-include.png). Ein Volume wird mit den angegebenen Einstellungen erstellt.

![Video zur Verfügung](./media/storsimple-create-volume/Video_icon.png) **Video verfügbar**

Um ein Video sehen Sie sich die Verwendung ein StorSimple-Volumes zu erstellen, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/).


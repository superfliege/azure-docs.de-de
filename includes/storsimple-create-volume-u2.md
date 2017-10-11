<!--author=alkohli last changed: 08/16/2016-->

#### <a name="to-create-a-volume"></a>So erstellen Sie ein Volume
1. Auf dem Gerät **Schnellstart** auf **Hinzufügen eines Volumes** Volume-Assistenten zu starten.
2. Hinzufügen eines Assistenten Volume unter **Grundeinstellungen**:
   
   1. Geben Sie einen **Namen** für Ihr Volume.
   2. Wählen Sie in der Dropdownliste die **Verwendungstyp** für Ihr Volume. Wählen Sie für Workloads, bei denen lokale Garantien, geringe Wartezeiten und höhere Leistung erfordern, einen **lokal angeheftet** Volume. Wählen Sie für alle anderen Daten eine **mehrstufig** Volume. Wenn Sie dieses Volume für Archivdaten arbeiten, überprüfen Sie **verwenden Sie dieses Volume für Archivdaten, auf die häufig zugegriffen**. 
      
       Ein lokales Volume bereitgestellt (Thick Provisioning), und stellt sicher, dass die primären Daten auf dem Volume, und ist nicht in die Cloud spill lokal auf dem Gerät bleibt.  Wenn Sie ein lokales Volume erstellen, sucht das Gerät verfügbare Speicherplatz auf dem lokalen Ebenen auf das Volume mit der angeforderten Größe bereitstellen. Der Vorgang der Erstellung eines lokales Volumes u. Überlaufs von vorhandenen Daten vom Gerät in die Cloud, und die Zeit für die Volumeerstellung länger sein kann. Die gesamte Zeit hängt von der Größe des bereitgestellten Volumes, die verfügbare Netzwerkbandbreite und die Daten auf Ihrem Gerät. 
      
       Einem mehrstufigen Volume ist Schlank zugewiesen und kann schnell erstellt werden. Auswählen von **verwenden Sie dieses Volume für Archivdaten, auf die häufig zugegriffen** für mehrstufiges Volume für Archivdaten Deduplizierung Segmentgröße für Ihr Volume an 512 KB ausgerichtet. Wenn dieses Feld nicht aktiviert ist, wird das entsprechende mehrstufige Volume eine Blockgröße von 64 KB verwendet. Eine höhere Blockgröße für die Deduplizierung ermöglicht dem Gerät um die Übertragung von großen Archivdaten in die Cloud zu beschleunigen.
   3. Geben Sie die **bereitgestellte Kapazität** für Ihr Volume. Notieren Sie die Kapazität, die verfügbar ist basierend auf dem Volume aktiviert. Die angegebene Volumegröße muss den verfügbaren Speicherplatz nicht überschreiten.
      
       Sie können lokale Volumes bereitstellen bis zu 8,5 TB oder mehrstufige Volumes bis zu 200 TB auf dem Gerät 8100. Auf dem größeren 8600 Gerät können Sie bereitstellen, lokale Volumes bis zu 22,5 TB oder mehrstufige Volumes bis zu 500 TB. Wie der lokale Speicherplatz auf dem Gerät erforderlich ist, um den Arbeitssatz von mehrstufigen Volumes zu hosten, beeinträchtigt Erstellung eines lokalen Volumes den zur Bereitstellung mehrstufiger Volumes verfügbaren Speicherplatz. Wenn Sie ein lokales Volume erstellen, wird daher für die Erstellung von mehrstufiger Volumes verfügbaren Speicherplatz reduziert. Wenn einem mehrstufigen Volume erstellt wird, wird der verfügbare Speicherplatz für die Erstellung eines lokalen Volumes auf ähnliche Weise reduziert.
      
       Wenn Sie ein lokales Volume 8,5 TB (maximal zulässige Größe) auf Ihrem Gerät 8100 bereitstellen, haben Sie alle der lokalen Speicherplatz auf dem Gerät erschöpft. Sie werden keine mehrstufiges Volume ab diesem Punkt zu erstellen, oder höher, als es keine lokaler Speicherplatz auf dem Gerät ist, um den Arbeitssatz von mehrstufigen Volumes zu hosten. Vorhandenen mehrstufigen Volumes wirken sich auch auf den verfügbaren Speicherplatz. Beispielsweise haben angeheftet ein Typ, die bereits Volumes mit ungefähr 106 TB mehrstufig verteilt wurde, nur 4 TB an Speicherplatz zur Verfügung stehen für lokal 8100 Volumes.
      
       Die folgende Abbildung zeigt die **Grundeinstellungen** Dialogfeld für ein lokales Volume.
      
        ![Lokales Volume hinzufügen](./media/storsimple-create-volume-u2/add-local-volume-include.png)
      
       Die folgende Abbildung zeigt die **Grundeinstellungen** Dialogfeld für ein mehrstufiges Volume.
      
        ![Lokales Volume hinzufügen](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)
   
   1. Klicken Sie auf das Symbol "Pfeil" ![Pfeilsymbol](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) um zur nächsten Seite zu wechseln.
3. In der **zusätzliche Einstellungen** Dialogfeld, fügen Sie einen neuen Access Control Record (ACR) hinzu:
   
   1. Geben Sie einen **Namen** für Ihren ACR.
   2. Unter **iSCSI-Initiatorname**, geben Sie die iSCSI Qualified Name (IQN) Ihres Windows-Hosts. Wenn Sie den IQN haben, fahren Sie mit [Abrufen des IQNS eines Windows Server-Hosts](#get-the-iqn-of-a-windows-server-host).
   3. Klicken Sie unter **standardsicherung für dieses Volume?**, wählen die **aktivieren** Kontrollkästchen. Die standardsicherung erstellt eine Richtlinie, die um 22:30 Uhr täglich (Gerätezeit) ausgeführt wird und eine Cloud-Momentaufnahme dieses Volumes erstellt.
      
      > [!NOTE]
      > Nachdem die Sicherung hier aktiviert wurde, kann sie nicht zurückgesetzt werden. Sie müssen das Volume, um diese Einstellung ändern, zu bearbeiten.
      > 
      > 
      
      ![Hinzufügen von Volumes](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)
4. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png). Ein Volume wird mit den angegebenen Einstellungen erstellt.


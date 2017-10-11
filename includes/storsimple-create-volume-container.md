<!--author=SharS last changed: 9/17/15-->

#### <a name="to-create-a-volume-container"></a>Zum Erstellen eines volumecontainers
1. Auf dem Gerät **Schnellstart** auf **Hinzufügen eines volumecontainers**. Die **Volumecontainer erstellen** Dialogfeld wird angezeigt.
   
    ![Erstellen eines Volumecontainers](./media/storsimple-create-volume-container/HCS_CreateVolumeContainerM-include.png)
2. In der **volumecontainer erstellen** (Dialogfeld):
   
   1. Geben Sie einen **Namen** für Ihren volumecontainer. Der Name muss 3 bis 32 Zeichen lang sein.
   2. Wählen Sie eine **Speicherkonto** , die diesem volumecontainer zugeordnet werden soll. Sie können das Standardkonto auswählen, das zum Zeitpunkt der Erstellung von Diensten generiert wird. Sie können auch die **Add new** Option aus, um ein Speicherkonto angeben, die nicht mit diesem Dienstabonnement verknüpft ist.
   3. Wählen Sie **Cloudspeicherverschlüsselung aktivieren** zum Aktivieren der Verschlüsselung der Daten vom Gerät in die Cloud gesendet.
   4. Geben Sie und bestätigen ein **Cloudspeicher-Verschlüsselungsschlüssel** , 8 bis 32 Zeichen lang sein. Dieser Schlüssel wird vom Gerät verwendet, um die verschlüsselten Daten zugreifen.
   5. Wählen Sie **unbegrenzt** in der **geben Bandbreite** Dropdown-Liste, wenn Sie die gesamte verfügbare Bandbreite nutzen möchten. Sie können diese Option auch festlegen, um **benutzerdefinierte** Bandbreite zu steuern, und geben Sie einen Wert zwischen 1 und 1.000 MBit/s. 
      Wenn Sie die Informationen zur bandbreitenauslastung haben, können Sie möglicherweise Bandbreite basierend auf einem Zeitplan durch Angabe zuzuordnen **bandbreitenvorlage auswählen**. Eine schrittweise Anleitung finden Sie unter [Hinzufügen einer bandbreitenvorlage](../articles/storsimple/storsimple-manage-bandwidth-templates.md#add-a-bandwidth-template).
   6. Klicken Sie auf das Häkchensymbol ![Kontrollkästchen-Symbol](./media/storsimple-create-volume-container/HCS_CheckIcon-include.png) Um diesen volumecontainer zu speichern und den Assistenten zu beenden. 
   
   Der neu erstellte volumecontainer wird aufgeführt, auf die **volumecontainer** Seite.

![Video zur Verfügung](./media/storsimple-create-volume-container/Video_icon.png) **Video verfügbar**

Um ein Video zu beobachten, die veranschaulicht, wie einen volumecontainer in Ihrer StorSimple-Lösung zu erstellen, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/create-a-volume-container-in-your-storsimple-solution/).


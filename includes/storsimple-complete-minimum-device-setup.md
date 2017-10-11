<!--author=alkohli last changed: 9/17/15-->

#### <a name="to-complete-the-minimum-storsimple-device-setup"></a>Die Mindestkonfiguration des StorSimple-Gerät abgeschlossen
1. In der **Geräte** Seite, wählen Sie das Gerät, klicken Sie auf den Pfeil für den Gerätenamen, um zur Seite "Gerät" zu wechseln. 
   
    ![Seite "Geräte" mit Gerät online](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 
2. Klicken Sie auf Symbol "Schnellstart" ![Quick-Startsymbol](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) auf die schnellstartseite Gerät zugreifen. Klicken Sie auf **Ausführen des gerätesetups** zum Starten der **Gerät konfigurieren** Assistenten.
   
    ![Seite "Schnellstart" Gerät](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)
3. Auf der **Grundeinstellungen** Seite, gehen Sie folgendermaßen vor:
   
   1. Geben Sie einen **Anzeigenamen** für Ihr Gerät. Der Standardname für das Gerät gibt Informationen wie Gerätemodell und Seriennummer. Sie können einen Anzeigenamen mit bis zu 64 Zeichen enthalten, Ihr Gerät verwalten zuweisen.
   2. Legen Sie die **Zeitzone** basierend auf dem geografischen Standort, in dem das Gerät bereitgestellt wird. Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.
   3. Klicken Sie unter **DNS-Einstellungen**, geben Sie eine Adresse für Ihre **sekundären DNS-Server**. Wenn Sie IPv6 verwenden, wird das Feld basierend auf dem IPv6-Präfix, das in Windows PowerShell-Schnittstelle bereitgestellten aufgefüllt. 
      Wenn der sekundäre DNS-Server nicht konfiguriert ist, werden Sie nicht zulässig auf Ihrem Gerätekonfiguration zu speichern.
   4. Aktivieren Sie unter den Schnittstellen des iSCSI-aktiviert mindestens ein Netzwerk für iSCSI. Mindestens eine Netzwerkschnittstelle muss cloudaktiviert sein, und eine Schnittstelle muss iSCSI-aktiviert sein. DATA 0 ist automatisch cloudaktiviert.
      
      ![Grundlegende Einstellungen für StorSimple Mindestanforderungen für Geräte einrichten](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)
4. Klicken Sie auf das Pfeilsymbol. ![StorSimple-Symbol "Pfeil"](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
5. Auf der **Netzwerkschnittstellen** Seite, geben Sie die festen IP-Adressen für Controller 0 und Controller 1. Wenn die DATA 0-Schnittstelle für IPv4 konfiguriert wurde, müssen die festen IP-Adressen im IPv4-Format angegeben werden. Wenn Sie ein Präfix für die IPv6-Konfiguration angegeben, werden die festen IP-Adressen in diesen Feldern automatisch aufgefüllt.

    > [!NOTE] 
    > - Die festen IP-Adressen des Controllers müssen freie IP-Adressen innerhalb des Subnetzes, das die IP-Adresse des Geräts zugänglich sein.
    > - Die festen IP-Adressen für den Controller für die Wartung der Updates auf dem Gerät verwendet werden darf, und daher die feste IP-Adressen routingfähig mit dem Internet herstellen können.

    ![StorSimple minimale geräteeinrichtung – Schnittstellen](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

1. Klicken Sie auf das Häkchensymbol ![StorSimple Häkchensymbol](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Sie gelangen zurück auf das Gerät **Schnellstart** Seite.
   
   > [!NOTE]
   > Sie können alle anderen geräteeinstellungen jederzeit ändern, indem Sie den Zugriff auf die **konfigurieren** Seite.
   > 
   > 

![Video zur Verfügung](./media/storsimple-complete-minimum-device-setup/Video_icon.png) **Video verfügbar**

Um ein Video zu beobachten, die mindestens erforderliche gerätesetup veranschaulicht, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/).


<!--author=alkohli last changed: 9/17/15-->

#### <a name="to-complete-the-minimum-storsimple-device-setup"></a>Die Mindestkonfiguration des StorSimple-Gerät abgeschlossen
1. Wählen Sie das Gerät, und klicken Sie auf **Schnellstart**. Klicken Sie auf **Ausführen des gerätesetups** um den Assistenten zum Konfigurieren von Geräten zu starten.
2. In den Assistenten zum Konfigurieren von Geräten **Grundeinstellungen** Dialogfeld Feld, gehen Sie folgendermaßen vor:
   
   1. Geben Sie einen **Anzeigenamen** für Ihr Gerät. Der Standardname für das Gerät gibt Informationen wie Gerätemodell und Seriennummer. Sie können einen Anzeigenamen mit bis zu 64 Zeichen enthalten, Ihr Gerät verwalten zuweisen.
   2. Legen Sie die **Zeitzone** basierend auf dem geografischen Standort, in dem das Gerät bereitgestellt wird. Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.
   3. Klicken Sie unter **DNS-Einstellungen**, geben Sie eine Adresse für Ihre **sekundären DNS-Server**. Wenn Sie IPv6 verwenden, wird das Feld basierend auf dem IPv6-Präfix, das in Windows PowerShell-Schnittstelle bereitgestellten aufgefüllt. 
      Wenn der sekundäre DNS-Server nicht konfiguriert ist, werden Sie nicht zulässig auf Ihrem Gerätekonfiguration zu speichern.
   4. Aktivieren Sie unter den Schnittstellen des iSCSI-aktiviert mindestens ein Netzwerk für iSCSI. Mindestens eine Netzwerkschnittstelle muss cloudaktiviert sein, und eine Schnittstelle muss iSCSI-aktiviert sein. DATA 0 ist automatisch cloudaktiviert.
      
      ![Grundlegende Einstellungen für StorSimple Mindestanforderungen für Geräte einrichten](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupBasicSettings1-include.png)
3. Klicken Sie auf das Pfeilsymbol. ![StorSimple-Symbol "Pfeil"](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
4. In der **Netzwerkschnittstellen** Dialogfeld geben die festen IP-Adressen für Controller 0 und Controller 1. **Die festen IP-Adressen des Controllers müssen freie IP-Adressen innerhalb des Subnetzes, das die IP-Adresse des Geräts zugänglich sein.** Wenn die DATA 0-Schnittstelle für IPv4 konfiguriert wurde, müssen die festen IP-Adressen im IPv4-Format angegeben werden. Wenn Sie ein Präfix für die IPv6-Konfiguration angegeben, werden die festen IP-Adressen in diesen Feldern automatisch aufgefüllt.

    ![StorSimple minimale geräteeinrichtung – Schnittstellen](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

    Die festen IP-Adressen für den Controller für die Wartung der Updates auf dem Gerät verwendet werden darf, und daher die feste IP-Adressen routingfähig mit dem Internet herstellen können. Sehen Sie sich, dass die festen Controller IP-Adressen routingfähig sind, mithilfe der [Test-HcsmConnection] [ Test] Cmdlet. Das folgende Beispiel zeigt die festen Controller IP-Adressen, die mit dem Internet weitergeleitet werden und die Microsoft Update-Servern zugreifen können. 

     ![Test-HcsmConnection routbare IP-Adressen anzeigen](./media/storsimple-complete-minimum-device-setup-u1/Test-HcsmConnectionOutputRegisteredDevice.png)

1. Klicken Sie auf das Häkchensymbol ![StorSimple Häkchensymbol](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Sie gelangen zurück auf das Gerät **Schnellstart** Seite.
   
   > [!NOTE]
   > Sie können alle anderen geräteeinstellungen jederzeit ändern, indem Sie den Zugriff auf die **konfigurieren** Seite.
   > 
   > 

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx
<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Aktualisieren von SharePoint 2010 auf SharePoint 2013, und klicken Sie dann die StorSomple-Adapter für SharePoint installieren
> [!IMPORTANT]
> Alle Dateien, die zuvor über RBS in einem externen Speicher verschoben wurden werden nicht verfügbar sein, bis die Aktualisierung abgeschlossen ist und die RBS-Funktion erneut aktiviert wird. Um die Auswirkungen für Benutzer zu beschränken, führen Sie alle Upgrade- oder erneuten Installationsvorgänge während eines geplanten Wartungsfensters aus.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>Aktualisieren von SharePoint 2010 auf SharePoint 2013, und installieren Sie den adapter
1. Beachten Sie in der SharePoint 2010-Farm den BLOB-Speicherpfad für die externalisierten BLOBs und die Inhaltsdatenbanken für die RBS aktiviert ist. 
2. Installieren Sie und konfigurieren Sie die neue SharePoint 2013-Farm. 
3. Verschieben von Datenbanken, Anwendungen und Websitesammlungen aus der SharePoint 2010-Farm auf die neue SharePoint 2013-Farm. Anweisungen hierzu finden Sie unter [Überblick über den Upgradeprozess für SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Installieren des StorSimple-Adapters für SharePoint auf der neuen Farm. Wechseln Sie zu [des StorSimple-Adapters für SharePoint installieren](#install-the-storsimple-adapter-for-sharepoint) Verfahren.
5. Mithilfe der Informationen, die Sie in Schritt 1 notiert haben, aktivieren Sie RBS für den gleichen Satz von Inhaltsdatenbanken, und geben Sie die BLOB-Speicherpfad an, die in der SharePoint 2010-Installation verwendet wurde. Wechseln Sie zu [RBS konfigurieren](#configure-rbs) Verfahren. Nachdem Sie diesen Schritt abgeschlossen haben, sollte die zuvor externalisierte Dateien von der neuen Farm zugänglich sein. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade des StorSimple-Adapters für SharePoint
> [!IMPORTANT]
> Planen Sie das Upgrade auf die während eines geplanten Wartungsfensters aus den folgenden Gründen auftreten:
> 
> * Externalisierte Inhalte werden zuvor nicht verfügbar sein, bis der Adapter erneut installiert wird.
> * Alle Inhalte, die auf die Website hochgeladen werden, nachdem Sie die vorherige Version des StorSimple-Adapters für SharePoint deinstallieren, aber vor der Installation der neuen Version wird in der Inhaltsdatenbank gespeichert werden. Sie müssen nach der Installation des neuen Adapters, dass der Inhalt zum StorSimple-Gerät zu verschieben. Sie können die Microsoft` RBS Migrate()` PowerShell-Cmdlets enthalten, die in SharePoint. damit die Inhalte zu migrieren. Weitere Informationen finden Sie unter [Migrieren von Inhalten in oder aus RBS](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>So aktualisieren Sie den StorSimple-Adapter für SharePoint
1. Deinstallieren Sie die vorherige Version von StorSimple-Adapters für SharePoint.
   
   > [!NOTE]
   > Dadurch wird automatisch RBS für die Inhaltsdatenbanken deaktiviert. Vorhandene BLOBs bleibt jedoch auf dem StorSimple-Gerät. Da RBS deaktiviert ist, und die BLOBs nicht zurück in die Inhaltsdatenbanken migriert wurden, werden alle Anforderungen für diese BLOBs fehl. 
   > 
   > 
2. Installieren Sie den neuen StorSimple-Adapter für SharePoint. Der neue Adapter erkennt automatisch die Inhaltsdatenbanken, die zuvor aktiviert oder deaktiviert für RBS wurden und die vorherigen Einstellungen verwenden.


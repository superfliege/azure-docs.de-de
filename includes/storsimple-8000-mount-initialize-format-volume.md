<!--author=SharS last changed: 9/17/15-->

#### <a name="to-mount-initialize-and-format-a-volume"></a>Zum Bereitstellen, initialisieren und Formatieren eines Volumes
1. Starten Sie den Microsoft iSCSI-Initiator.
2. In der **iSCSI-Initiator-Eigenschaften** Fenster auf die **Ermittlung** auf **Portal ermitteln**.
3. In der **Zielportal** (Dialogfeld), geben Sie die IP-Adresse Ihrer Netzwerkschnittstelle mit iSCSI-aktiviert, und klicken Sie dann auf **OK**. 
4. In der **iSCSI-Initiator-Eigenschaften** Fenster auf die **Ziele** Registerkarte, suchen Sie nach der **ermittelte Ziele**. Der Gerätestatus sollte nun wie **inaktiv**.
5. Wählen Sie das Zielgerät, und klicken Sie dann auf **verbinden**. Nachdem das Gerät verbunden ist, sollten in der Status ändern **verbunden**. (Weitere Informationen zur Verwendung des Microsoft iSCSI-Initiators finden Sie unter [installieren und Konfigurieren des Microsoft iSCSI-Initiator][1]).
6. Drücken Sie auf dem Windows-Host, die Windows-Logo-Taste + X, und klicken Sie dann auf **ausführen**. 
7. In der **ausführen** Geben Sie im Dialogfeld **"diskmgmt.msc"**. Klicken Sie auf **OK**, und die **Datenträgerverwaltung** Dialogfeld wird angezeigt. Rechte Bereich werden die Volumes auf Ihrem Host angezeigt.
8. In der **Datenträgerverwaltung** Fenster, die bereitgestellten Volumes werden angezeigt, wie in der folgenden Abbildung dargestellt. Mit der rechten Maustaste des ermittelten Volumes (klicken Sie auf den Datenträgernamen), und klicken Sie dann auf **Online**.
   
     ![Volumeformatieren einleiten](./media/storsimple-8000-mount-initialize-format-volume/step7initializeformatvolume.png) 
9. Mit der rechten Maustaste des Volumes (klicken Sie auf den Datenträgernamen) erneut aus, und klicken Sie dann auf **initialisieren**.
10. Um ein einfaches Volume zu formatieren, führen Sie die folgenden Schritte aus:
    
    1. Wählen Sie das Volume, das rechten Maustaste darauf (klicken Sie im rechten Bereich), und klicken Sie auf **Neues einfaches Volume**.
    2. Geben Sie die Volume-Größe und die Laufwerkbuchstaben, und konfigurieren Sie das Volume als NTFS-Dateisystem, in den Assistenten Neues einfaches Volume.
    3. Geben Sie eine zuordnungseinheitsgröße von 64 KB. Diese zuordnungseinheitsgröße eignet sich gut für die deduplikationsalgorithmen, die in der StorSimple-Lösung verwendet.
    4. Durchführen Sie schnellformatierung.

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx


1. In der **hybridverbindungen** Blatt, klicken Sie auf die hybridverbindung, die Sie gerade erstellt haben, klicken Sie dann auf **Listener Setup**.
   
    ![Klicken Sie auf Listener-Setup](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. Die **Hybrid-Verbindungseigenschaften** Blatt wird geöffnet. Klicken Sie unter **lokalen Hybrid Connection Manager**, wählen Sie **herunterladen und manuell konfigurieren**, speichern Sie das heruntergeladene HybridConnectionManager.msi-Paket und kopieren Sie das Gateway-Verbindungszeichenfolge.
   
    ![Klicken Sie hier, um zu installieren](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Geben Sie eine Eingabeaufforderung mit Administratorrechten aus den folgenden Befehl aus, um das Installationsprogramm zu starten:
   
        start HybridConnectionManager.msi
4. Nachdem das Installationsprogramm ausgeführt wird, klicken Sie auf **nicht jetzt**, navigieren Sie zu dem Ordner %ProgramFiles%\Microsoft\HybridConnectionManager, HCMConfigWizard.exe ausgeführt, und klicken Sie auf **Ja** in der **User Account Control** Dialogfeld.
5. Fügen Sie die Hybrid-Verbindungszeichenfolge, die Sie zuvor kopiert haben, und klicken Sie auf **OK**. 
   
    ![Installieren von](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **schließen**.
   
    ![Klicken Sie auf Schließen](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Auf der **hybridverbindungen** Blatt der **Status** Spalte zeigt jetzt **verbunden**. 
   
    ![Status verbunden](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)


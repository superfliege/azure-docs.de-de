

## <a name="generate-the-certificate-signing-request-file"></a>Generieren Sie die Zertifikatsignieranforderung-Datei
Apple Push Notification Service (APNS) werden Zertifikate verwendet, um Ihre Pushbenachrichtigungen zu authentifizieren. So erstellen die erforderlichen Push-Zertifikat zum Senden und Empfangen von Benachrichtigungen, gehen Sie wie folgt vor. Weitere Informationen zu diesen Konzepten finden Sie unter den offiziellen [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) Dokumentation.

Generieren Sie die Zertifikatsignieranforderung (CSR)-Datei, die von Apple verwendet wird, um eine signierte Push-Zertifikat zu generieren.

1. Auf Ihrem Mac, führen Sie das Tool Schlüsselbundverwaltung. Kann aus geöffnet werden die **Hilfsprogramme** Ordner oder die **andere** Ordner auf das Auffüllzeichen starten.
2. Klicken Sie auf **Schlüsselbundverwaltung**, erweitern Sie **Zertifikat-Assistenten**, klicken Sie dann auf **ein Zertifikat von einer Zertifizierungsstelle anfordern...** .
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Wählen Sie Ihre **Benutzer e-Mail-Adresse** und **allgemeiner Name** , stellen Sie sicher, dass **auf dem Datenträger gespeicherte** ausgewählt ist, und klicken Sie dann auf **Fortfahren**. Lassen Sie die **CA-e-Mail-Adresse** Feld leer, da es nicht erforderlich ist.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Geben Sie einen Namen für die Zertifikatsignieranforderung (CSR) Datei **speichern unter**, wählen Sie den Speicherort in **, in denen**, klicken Sie dann auf **speichern**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Dies spart die CSR-Datei am ausgewählten Speicherort; Der Standardspeicherort ist auf dem Desktop. Beachten Sie den Speicherort für diese Datei ausgewählt.

Als Nächstes Sie registrieren Ihre app mit dem Apple, aktivieren Sie Pushbenachrichtigungen und dieses exportierte CSR zum Erstellen einer Push-Zertifikat hochladen.

## <a name="register-your-app-for-push-notifications"></a>Registrieren Sie Ihrer app für Pushbenachrichtigungen
Um Pushbenachrichtigungen an eine iOS-app senden zu können, müssen Sie Registrieren Ihrer Anwendung bei Apple und auch die Registrierung für Pushbenachrichtigungen.  

1. Wenn Sie Ihre app noch nicht registriert haben, navigieren Sie zu der <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS-Bereitstellungsportal</a> beim Apple Developer Center anmelden durch Ihre Apple-ID, klicken Sie auf **Bezeichner**, klicken Sie dann auf **App-IDs**, und klicken Sie abschließend auf die  **+**  zum Registrieren einer neuen app signieren.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. Aktualisieren Sie die folgenden drei Felder für die neue app, und klicken Sie dann auf **Fortfahren**:
   
   * **Name**: Geben Sie einen beschreibenden Namen für Ihre app in der **Name** -Feld in der **Beschreibung der App-ID** Abschnitt.
   * **Bündeln Bezeichner**: unter der **explizite App-ID** Geben Sie im Abschnitt eine **Paket-ID** in Form `<Organization Identifier>.<Product Name>` wie die [App Distribution Handbuch](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Die *Organisations-ID* und *Product Name* Sie verwenden, muss der Bezeichner und Product Name der Organisation Sie verwenden bei der Erstellung von Ihrem XCode-Projekts übereinstimmen. In der folgenden Screeshot *NotificationHubs* dient als eine Organisation-ID und *GetStarted* dient als Name des Produkts. Stellen Sie sicher, dass dies die Werte übereinstimmt, die Sie in Ihrem XCode-Projekt verwenden können Sie die richtige Veröffentlichungsprofil mit XCode zu verwenden. 
   * **Pushbenachrichtigungen**: Überprüfen Sie die **Pushbenachrichtigungen** -Option in der **Anwendungsdienste** Abschnitt.
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      Dadurch wird Ihre App-ID generiert und Anforderungen, die Informationen zu bestätigen. Klicken Sie auf **registrieren** , bestätigen die neuen App-ID.
     
      Nach dem Klicken auf **registrieren**, sehen Sie die **Registrierung abschließen** Bildschirm, wie unten dargestellt. Klicken Sie auf **Fertig**.
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. Suchen Sie in der Developer Center unter App-IDs die app-ID, die Sie gerade erstellt, und klicken Sie auf die entsprechende Zeile auf.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      Durch Klicken auf die app-ID wird die app-Details angezeigt. Klicken Sie auf die **bearbeiten** unten auf die Schaltfläche.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. Führen Sie einen Bildlauf zum unteren Rand des Bildschirms, und klicken Sie auf die **Zertifikat erstellen...**  Schaltfläche klicken Sie im Abschnitt **Entwicklung mithilfe von Push übertragen SSL-Zertifikat**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      Dadurch wird der Assistent "IOS Zertifikat hinzufügen" angezeigt.
   
   > [!NOTE]
   > Dieses Lernprogramm verwendet ein Entwicklungszertifikat. Dasselbe Verfahren wird verwendet, wenn ein produktionszertifikat zu registrieren. Achten Sie darauf, dass Sie denselben Zertifikattyp beim Senden von Benachrichtigungen verwenden.
   > 
   > 
3. Klicken Sie auf **Choose File**, navigieren Sie zum Speicherort, in dem Sie die CSR-Datei, die Sie in der ersten Aufgabe erstellt gespeichert, und dann auf **generieren**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. Nachdem das Zertifikat vom Portal erstellt wurde, klicken Sie auf die **herunterladen** aus, und klicken Sie auf **Fertig**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      Lädt das Zertifikat, und speichert sie in Ihrem Computer im Ordner "Downloads".
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Wird standardmäßig die heruntergeladene Datei ein Entwicklungszertifikat heißt **aps_development.cer**.
   > 
   > 
5. Doppelklicken Sie auf die heruntergeladene Push-Zertifikat **aps_development.cer**.
   
      Dadurch werden das neue Zertifikat im Schlüsselbund, installiert, wie unten dargestellt:
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > Der Name in Ihr Zertifikat unterschiedlich sein, aber es wird als Präfix verwendet werden **Entwicklung Apple iOS Dienste mithilfe von Push übertragen:**.
   > 
   > 
6. Schlüsselbundverwaltung, mit der Maustaste des neue Push-Zertifikats, das Sie in erstellt die **Zertifikate** Kategorie. Klicken Sie auf **exportieren**, nennen Sie die Datei, wählen Sie die **P12** formatieren, und klicken Sie dann auf **speichern**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    Stellen Sie sich den Dateinamen und Speicherort des Zertifikats exportierten P12. Es wird zum Aktivieren der Authentifizierung bei APNS verwendet werden.
   
   > [!NOTE]
   > In diesem Lernprogramm erstellt eine QuickStart.p12-Datei. Die Dateinamen und Speicherort können abweichen.
   > 
   > 

## <a name="create-a-provisioning-profile-for-the-app"></a>Erstellen Sie ein Bereitstellungsprofil für die app
1. Wieder in der <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS-Bereitstellungsportal</a>wählen **Provisioning Profile**wählen **alle**, und klicken Sie dann auf die  **+**  Schaltfläche, um ein neues Profil erstellen. Dadurch wird die **Provisiong Profil für iOS hinzufügen** Assistenten
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Wählen Sie **iOS-App-Entwicklung** unter **Entwicklung** als Provisiong Profiltyp aus, und klicken Sie auf **Fortfahren**. 
3. Wählen Sie die app-ID, die Sie gerade erstellt haben, aus der **App-ID** Dropdown-Liste aus, und klicken Sie auf **fortfahren**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. In der **wählen Sie Zertifikate** Bildschirm, wählen Sie das übliche Entwicklungszertifikat zum Signieren von Code verwendet, und klicken Sie auf **Fortfahren**. Dies ist nicht der Push-Zertifikat, das Sie gerade erstellt haben.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Wählen Sie als Nächstes die **Geräte** zu Testzwecken verwenden, und klicken Sie auf **fortfahren**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Schließlich wählen Sie einen Namen für das Profil in **Profilname**, klicken Sie auf **generieren**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. Aus der Erstellung der neue provisioning-Profil klicken Sie zum Herunterladen und installieren Sie es auf dem Entwicklungscomputer Xcode auf. Klicken Sie dann auf **Fertig**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

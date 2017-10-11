#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Konfigurieren von iOS-Projekt in Xamarin Studio
1. Öffnen Sie in Xamarin.Studio, **"Info.plist"**, und Aktualisieren der **Paket-ID** mit der Paket-ID, die Sie zuvor erstellt, mit Ihrer neuen app-ID. haben

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Führen Sie einen Bildlauf nach unten bis zum **Hintergrundmodi**. Wählen Sie die **Aktivieren von Hintergrundmodi** Feld und die **Remote Benachrichtigungen** Feld.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Doppelklicken Sie auf das Projekt in der Systemsteuerung Projektmappe öffnen **Projektoptionen**.
4. Klicken Sie unter **erstellen**, wählen Sie **iOS Bundle Signing**, und wählen Sie die entsprechende Identität und Bereitstellung erstellen ein Profil eingerichtet oben für dieses Projekt.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Dadurch wird sichergestellt, dass das Projekt das neue Profil zum Signieren von Code verwendet. Die offizielle Xamarin gerätebereitstellung Dokumentation, finden Sie unter [Xamarin Gerätebereitstellung].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Konfigurieren Sie die iOS-Projekt in Visual Studio
1. Klicken Sie in Visual Studio mit der rechten Maustaste des Projekts, und klicken Sie dann auf **Eigenschaften**.
2. Klicken Sie in den Eigenschaftenseiten auf die **iOS-Anwendung** Registerkarte, und aktualisieren Sie die **Bezeichner** mit der ID, die Sie zuvor erstellt haben.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. In der **iOS Bundle Signing** Registerkarte, wählen Sie die entsprechende Identität und ein Bereitstellungsprofil Sie nur für dieses Projekt richten.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dadurch wird sichergestellt, dass das Projekt das neue Profil zum Signieren von Code verwendet. Die offizielle Xamarin gerätebereitstellung Dokumentation, finden Sie unter [Xamarin Gerätebereitstellung].
4. Doppelklicken Sie auf die Datei "Info.plist", um ihn zu öffnen, klicken Sie dann aktivieren **RemoteNotifications** unter **Hintergrundmodi**.

[Xamarin Gerätebereitstellung]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/

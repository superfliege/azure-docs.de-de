1. Android SDK Manager zu öffnen, indem Sie auf das Symbol auf der Symbolleiste von Android Studio oder durch Klicken auf **Tools** -> **Android** -> **SDK-Manager** auf das Menü. Suchen Sie die Zielversion von Android SDK, das verwendet wird, in Ihrem Projekt, öffnen Sie ihn durch Klicken auf **Paketdetails anzeigen**, und wählen Sie **Google APIs**, sofern er nicht bereits installiert ist.
2. Klicken Sie auf die **SDK-Tools** Registerkarte. Wenn Sie Google Play-Dienst noch nicht installiert haben, klicken Sie auf **Google Play-Dienste** wie unten dargestellt. Klicken Sie dann auf **übernehmen** zu installieren. 
   
    Beachten Sie den SDK-Pfad für die Verwendung in einem späteren Schritt aus. 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Öffnen der **build.gradle** -Datei in das app-Verzeichnis.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Fügen Sie diese Zeile unter *Abhängigkeiten*: 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. Klicken Sie auf die **mit Dateien, die Gradle-Synchronisierung – Projekt** Symbol in der Symbolleiste.
6. Open **AndroidManifest.xml** und fügen Sie dieses Tag in der *Anwendung* Tag.
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />


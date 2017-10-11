
1. Besuchen Sie die [Azure-Portal]. Klicken Sie auf **alle Durchsuchen** > **Mobile Apps** > das Back-End, das Sie gerade erstellt haben. Klicken Sie in der mobilen app-Einstellungen auf **Schnellstart** > **Android)**. Klicken Sie unter **Konfigurieren Ihrer Clientanwendung**, klicken Sie auf **herunterladen**. Dadurch werden einen vollständige Android-Projekt für eine app zur Verbindung mit Ihrem Back-End vorkonfiguriert. 
2. Öffnen Sie das Projekt mit **Android Studio**mit **Import Project (Eclipse ADT, Gradle, usw.)** . Stellen Sie sicher, dass Sie alle JDK-Fehler zu vermeiden dieser Import-Auswahl treffen.
3. Drücken Sie die **ausführen "app"** Schaltfläche, um das Projekt erstellen und starten Sie die app im Android-Simulator.
4. Geben Sie in der app einen aussagekräftigen Text ein, z. B. *Abschließen des Lernprogramms* , und klicken Sie dann auf die Schaltfläche "Hinzufügen". Sendet eine POST-Anforderung an den Azure-Back-End, das Sie zuvor bereitgestellt haben. Die Back-End-fügt die Daten aus der Anforderung ist in der TodoItem-SQL-Tabelle und gibt Informationen über die neu gespeicherte Elemente an der mobilen Anwendung zurück. Die mobile Anwendung zeigt diese Daten in der Liste. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure-Portal]: https://portal.azure.com/

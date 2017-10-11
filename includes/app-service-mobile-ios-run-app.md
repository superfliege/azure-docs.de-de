
1. Auf Ihrem Mac, besuchen Sie die [Azure-Portal]. Klicken Sie auf **alle Durchsuchen** > **Mobile Apps** > das Back-End, das Sie gerade erstellt haben. Klicken Sie in der mobilen app-Einstellungen auf **Schnellstart** > **iOS (Objective-C)**. Falls Swift gewünscht, klicken Sie auf **Schnellstart** > **iOS (Swift)** stattdessen. Klicken Sie unter **herunterladen und Ausführen von iOS-Projekt**, klicken Sie auf **herunterladen**. Dadurch werden ein vollständiges Xcode-Projekt für eine app zur Verbindung mit Ihrem Back-End vorkonfiguriert. Das Projekt mithilfe von Xcode zu öffnen.
2. Drücken Sie die **ausführen** Schaltfläche, um das Projekt erstellen und starten Sie die app im iOS-Simulator.
3. Geben Sie in der app einen aussagekräftigen Text ein, z. B. *Abschließen des Lernprogramms* , und klicken Sie dann auf das Pluszeichen (**+**) Symbol ". Sendet eine POST-Anforderung an den Azure-Back-End, das Sie zuvor bereitgestellt haben. Die Back-End-fügt die Daten aus der Anforderung ist in der TodoItem-SQL-Tabelle und gibt Informationen über die neu gespeicherte Elemente an der mobilen Anwendung zurück. Die mobile Anwendung zeigt diese Daten in der Liste. 

   ![Schnellstart-app unter iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure-Portal]: https://portal.azure.com/

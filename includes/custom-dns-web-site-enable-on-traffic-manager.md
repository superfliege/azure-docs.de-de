Nach der Weitergabe der Datensätze für Ihren Domänennamen, sollten Sie möglicherweise Ihren Browser verwenden, um sicherzustellen, dass Ihren benutzerdefinierten Domänennamen auf Ihre Web-app in Azure App Service verwendet werden kann.

> [!NOTE]
> Es dauert einige Zeit für Ihre CNAME im DNS-System verteilt. Sie können z. B. einen Dienst verwenden <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> , stellen Sie sicher, dass der CNAME-Eintrag verfügbar ist.
> 
> 

Wenn Sie Ihre Web-app noch nicht als Traffic Manager-Endpunkt hinzugefügt haben, müssen Sie dies tun, bevor namensauflösung, als die benutzerdefinierte Domäne-Namen Routen zu Traffic Manager funktioniert. Traffic Manager leitet dann an Ihre Web-app. Verwenden Sie die Informationen in [hinzufügen oder Löschen von Endpunkten](../articles/traffic-manager/traffic-manager-endpoints.md) Ihre Web-app als Endpunkt in Traffic Manager-Profil hinzufügen.

> [!NOTE]
> Wenn Ihre Web-app nicht aufgeführt ist, wenn Sie einen Endpunkt hinzufügen, stellen Sie sicher, dass er für konfiguriert ist **Standard** App Service-Plan-Modus. Verwenden Sie **Standard** Modus für Ihre Web-app, damit Sie mit Traffic Manager funktioniert.
> 
> 

1. Öffnen Sie in Ihrem Browser die [Azure-Portal](https://portal.azure.com).
2. In der **Web-Apps** Registerkarte, klicken Sie auf den Namen Ihrer Web-App wählen **Einstellungen**, und wählen Sie dann **benutzerdefinierte Domänen**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. In der **benutzerdefinierte Domänen** Blatt, klicken Sie auf **hinzufügen Hostname**.
4. Verwenden der **Hostname** Textfelder ein, und geben Sie den Traffic Manager-Domänennamen mit dieser Web-app zuordnen.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Klicken Sie auf **Validate** zum Speichern der Konfiguration der Domäne-Namen.
6. Wenn Sie auf **Validate** Azure wird Kickoff Domänenüberprüfung Workflow. Dies wird domänenbesitzes sowie Hostname Verfügbarkeit und Bericht Erfolgs- oder ausführliche mit normativen Guidence zum Beheben des Fehlers geprüft.    
7. Bei einer erfolgreichen Validierung **hinzufügen Hostname** Schaltfläche aktiv, und es werden der Hostname weisen möglich. Wechseln Sie jetzt mit Ihrem benutzerdefinierten Domänennamen in einem Browser. Ihre app ausgeführt wird, die mit Ihren benutzerdefinierten Domänennamen sollte jetzt angezeigt werden. 
   
   Sobald die Konfiguration abgeschlossen ist, wird der benutzerdefinierte Domänenname aufgeführt der **Domänennamen** Teil Ihrer Web-app.

An diesem Punkt sollte die Traffic Manager-Domänenname im Browser eingeben, und sehen, dass es erfolgreich zu Ihrer Web-app gelangen können.


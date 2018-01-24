## <a name="rest"></a>Bereitstellen mithilfe von REST-APIs 
 
Sie können die [REST-APIs des Bereitstellungsdiensts](https://github.com/projectkudu/kudu/wiki/REST-API) verwenden, um die ZIP-Datei in Ihrer App in Azure bereitzustellen. Senden Sie dazu einfach eine POST-Anforderung an „https://<app_name>.scm.azurewebsites.net/api/zipdeploy“. Die POST-Anforderung muss die ZIP-Datei im Nachrichtentext enthalten. Die Anmeldeinformationen für die Bereitstellung für Ihre App werden in der Anforderung mithilfe von HTTP-Standardauthentifizierung bereitgestellt. Weitere Informationen finden Sie in der [.zip push deployment reference](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file) (Referenz zur ZIP-Push-Bereitstellung). 

Das folgende Beispiel verwendet das cURL-Tool, um eine Anforderung zu senden, die die ZIP-Datei enthält. Sie können cURL auf Mac- oder Linux-Computern aus dem Terminal ausführen oder auf Windows-Computern Bash verwenden. Ersetzen Sie den Platzhalter `<zip_file_path>` durch den Pfad zum Speicherort der ZIP-Datei Ihres Projekts. Ersetzen Sie außerdem `<app_name>` durch den eindeutigen Namen Ihrer App.

Ersetzen Sie den Platzhalter `<deployment_user>` durch den Benutzernamen Ihrer Anmeldeinformationen für die Bereitstellung. Wenn Sie von cURL dazu aufgefordert werden, geben Sie das Kennwort ein. Informationen zum Festlegen von Anmeldeinformationen für die Bereitstellung für Ihre App finden Sie unter [Festlegen und Zurücksetzen von Anmeldeinformationen auf Benutzerebene](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Diese Anforderung löst die Push-Bereitstellung aus der hochgeladenen ZIP-Datei aus. Sie können die aktuelle und vorhergegangene Bereitstellungen mithilfe des Endpunkts „https://<app_name>.scm.azurewebsites.net/api/deployments“ überprüfen, wie im folgenden cURL-Beispiel zu sehen. Ersetzen Sie auch hier `<app_name>` durch den Namen Ihrer App und `<deployment_user>` durch den Benutzernamen Ihrer Anmeldeinformationen für die Bereitstellung.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```
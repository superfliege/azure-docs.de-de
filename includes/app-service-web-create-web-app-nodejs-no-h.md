Erstellen Sie in Cloud Shell im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create) eine Web-App. Vergessen Sie nicht, `<app_name>` durch einen eindeutigen App-Namen zu ersetzen.

Die Runtime im folgenden Befehl ist auf `NODE|6.9` festgelegt. Führen Sie zum Anzeigen aller unterstützten Runtimes den Befehl [az webapp list-runtimes](/cli/azure/webapp#list-runtimes) aus. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Sie haben eine leere Web-App mit aktivierter Git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>

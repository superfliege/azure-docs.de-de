Erstellen Sie in der Cloud Shell im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create) eine [Web-App](../articles/app-service/app-service-web-overview.md). 

Die Web-App bietet Ihnen eine Hostingumgebung für Ihren Code sowie eine URL, unter der Sie sich die bereitgestellte App ansehen können.

Ersetzen Sie im folgenden Befehl *\<app_name>* durch einen eindeutigen Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). Wenn `<app_name>` nicht eindeutig ist, wird die Fehlermeldung „Eine Website mit dem Namen <App-Name> ist bereits vorhanden“ angezeigt. Die Standard-URL der Web-App lautet `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Nach dem Erstellen der Web-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

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

Navigieren Sie zu der Website, um sich Ihre neu erstellte Web-App anzusehen.

```bash
http://<app_name>.azurewebsites.net
```

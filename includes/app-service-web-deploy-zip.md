## <a name="deploy-uploaded-zip-file"></a>Bereitstellen der hochgeladenen ZIP-Datei

Stellen Sie in Cloud Shell mit dem Befehl [`az webapp deployment source config-zip`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) die hochgeladene ZIP-Datei für Ihre Web-App bereit. Ersetzen Sie *\<app_name>* durch den Namen Ihrer Web-App.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myAppFiles.zip
```

Mit diesem Befehl werden die Dateien und Verzeichnisse aus der ZIP-Datei in Ihrem Standardordner der App Service-Anwendung (`\home\site\wwwroot`) bereitgestellt, und die App wird neu gestartet. Falls ein zusätzlicher benutzerdefinierter Buildprozess konfiguriert wurde, wird dieser ebenfalls ausgeführt.

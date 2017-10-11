Verwenden Sie die Azure-CLI, um die remote-URL für Ihre API-App zu erhalten. Ersetzen Sie in den folgenden Befehl  *\<App_name >* mit Ihrer Web-app-Namen.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Konfigurieren Sie die lokale Git-Bereitstellung, um mithilfe von Push an der Remoteinstanz übertragen können.

```bash
git remote add azure <URI from previous step>
```

Push an den Azure remote zum Bereitstellen Ihrer app. Sie sind für das Kennwort aufgefordert, die Sie zuvor erstellt haben, wenn Sie den bereitstellungsbenutzer erstellt haben. Stellen Sie sicher, dass Sie das Kennwort, das Sie zuvor im Quickstart in erstellt haben und nicht durch das Kennwort zum Anmelden bei Azure-Portal verwendeten eingeben.

```bash
git push azure master
```

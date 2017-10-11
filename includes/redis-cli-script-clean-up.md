## <a name="clean-up-deployment"></a>Bereitstellung bereinigen 

Nachdem das Beispielskript ausgeführt wurde, kann der folgende Befehl verwendet werden, entfernen Sie die Ressourcengruppe, Azure-Redis-Cache-Instanz und alle zugehörigen Ressourcen in der Ressourcengruppe.

```azurecli
az group delete --name contosoGroup
```
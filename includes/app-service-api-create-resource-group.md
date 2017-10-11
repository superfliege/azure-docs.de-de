Erstellen einer Ressourcengruppe mit dem [az Gruppe erstellen](/cli/azure/group#create) Befehl.

[!INCLUDE [resource group intro text](resource-group.md)]

Das folgende Beispiel erstellt eine Ressourcengruppe namens *MyResourceGroup* in der *Westeurope* Speicherort.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Um die verfügbaren Standorte anzuzeigen, führen Sie die `az appservice list-locations` Befehl. Im Allgemeinen erstellen Sie Ressourcen in einer Region in Ihrer Nähe.

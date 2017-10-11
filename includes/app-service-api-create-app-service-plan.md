Erstellen Sie einen App Service-Plan mit der [az Appservice-Plan erstellen](/cli/azure/appservice/plan#create) Befehl.

[!INCLUDE [app-service-plan](app-service-plan.md)]

Das folgende Beispiel erstellt einen App Service-Plan mit dem Namen `myAppServicePlan` in der **frei** Tarif:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Wenn die App Service-Plan erstellt wurde, zeigt der Azure-CLI Informationen ähnlich wie im folgenden Beispiel:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
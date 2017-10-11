Wenn Sie eine Parameterdatei zum Übergeben von Parameterwerten während der Bereitstellung verwenden, müssen Sie eine JSON-Datei in einem Format ähnlich wie im folgenden Beispiel erstellen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Die Größe der Parameterdatei darf nicht mehr als 64 KB sein.

Wenn Sie einen sensiblen Wert für einen Parameter (z. B. ein Kennwort) bereitstellen müssen, fügen Sie diesen Wert auf einen schlüsseltresor hinzu. Abrufen der schlüsseltresor während der Bereitstellung an, wie im vorherigen Beispiel gezeigt. Weitere Informationen finden Sie unter [sichere Werte übergeben, während der Bereitstellung](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 


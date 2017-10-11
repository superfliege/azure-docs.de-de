### <a name="app-service-plan"></a>App Service-plan
Erstellt den Service-Plan für das Hosten von Web-app. Geben Sie den Namen des Plans durch die **HostingPlanName** Parameter. Der Speicherort des Plans ist der Ort, für die Ressourcengruppe verwendet. Tarif Ebene und Workerrollen Größe werden angegeben, der **Sku** und **WorkerSize** Parameter

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },


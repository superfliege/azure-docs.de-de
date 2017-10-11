Mit Azure Resource Manager, definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt Parameter aufgerufen, der alle Parameterwerte enthält.
Sie sollten einen Parameter für diese Werte definieren, die unterschiedlich sind, basierend auf das Projekt, das Sie bereitstellen oder basierend auf die Umgebung aus, der Sie bereitstellen. Definieren Sie keine Parameter für Werte, die immer gleich bleiben werden. Jeder Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden. 

Wenn Sie Parameter definieren, verwenden die **AllowedValues** Feld an, die einen Benutzer Werte kann während der Bereitstellung bereitstellen. Verwenden der **"DefaultValue"** Feld, um den Parameter einen Wert zuweisen, wenn während der Bereitstellung wird kein Wert angegeben.

Wir beschreiben Sie jeden Parameter in der Vorlage.

### <a name="sitename"></a>siteName
Der Name der Web-app, die Sie erstellen möchten.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Der Name des App Service-Plan für das Hosten von Web-app verwenden.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
Der Tarif für den hostingplan.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind, und weist den Standardwert (S1), wenn kein Wert angegeben wird.

### <a name="workersize"></a>workerSize
Die Instanzgröße des hostingplans (klein, Mittel oder groß).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Die Vorlage definiert die Werte, die für diesen Parameter (0, 1 oder 2) zulässig sind, und weist einen Standardwert (0), wenn kein Wert angegeben wird. Die Werte entsprechen "klein", mittelgroßen und großen.


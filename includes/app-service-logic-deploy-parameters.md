Mit Azure Resource Manager, definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt Parameter aufgerufen, der alle Parameterwerte enthält.
Sie sollten einen Parameter für diese Werte definieren, die unterschiedlich sind, basierend auf das Projekt, das Sie bereitstellen oder basierend auf die Umgebung aus, der Sie bereitstellen. Definieren Sie keine Parameter für Werte, die immer gleich bleiben werden. Jeder Parameterwert wird in der Vorlage verwendet, definieren Sie die Ressourcen, die bereitgestellt werden. 

Wenn Sie Parameter definieren, verwenden die **AllowedValues** Feld an, die einen Benutzer Werte kann während der Bereitstellung bereitstellen. Verwenden der **"DefaultValue"** Feld, um den Parameter einen Wert zuweisen, wenn während der Bereitstellung wird kein Wert angegeben.

Wir beschreiben Sie jeden Parameter in der Vorlage.

### <a name="logicappname"></a>logicAppName
Der Name der Logik-app zu erstellen.

    "logicAppName": {
        "type": "string"
    }
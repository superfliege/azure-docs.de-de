## <a name="create-a-project-zip-file"></a>Erstellen einer ZIP-Datei für das Projekt

Stellen Sie sicher, dass Sie sich noch im Stammverzeichnis Ihres Beispielprojekts befinden. Erstellen Sie ein ZIP-Archiv mit allen Elementen Ihres Projekts. Mit dem folgenden Befehl wird das Standardtool in Ihrem Terminal verwendet:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

Später laden Sie diese ZIP-Datei in Azure hoch und stellen sie für App Service bereit.
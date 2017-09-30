# <a name="persist-files-in-azure-cloud-shell"></a>Beibehalten von Dateien in Azure Cloud Shell
Cloud Shell nutzt Azure-Dateispeicher, um Dateien sitzungsübergreifend beizubehalten.

## <a name="set-up-a-clouddrive-file-share"></a>Einrichten einer clouddrive-Dateifreigabe
Beim ersten Start fordert Cloud Shell Sie auf, eine neue oder vorhandene Dateifreigabe zu verknüpfen, um Dateien sitzungsübergreifend beizubehalten.

> [!NOTE]
> Für Bash und PowerShell wird dieselbe Dateifreigabe verwendet. In Cloud Shell kann der automatischen Bereitstellung nur eine Dateifreigabe zugeordnet werden.

### <a name="create-new-storage"></a>Erstellen von neuem Speicher

Wenn Sie die grundlegenden Einstellungen verwenden und nur ein Abonnement auswählen, werden von Cloud Shell in der für Sie unterstützten nächstgelegenen Region drei Ressourcen erstellt:
* Ressourcengruppe: `cloud-shell-storage-<region>`
* Speicherkonto: `cs<uniqueGuid>`
* Dateifreigabe: `cs-<user>-<domain>-com-<uniqueGuid>`

![Einstellung „Abonnement“](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

Die Dateifreigabe wird in Ihrem Verzeichnis `$Home` als `clouddrive` bereitgestellt. Dies ist eine einmalige Aktion, und die Dateifreigabe wird in den nachfolgenden Sitzungen automatisch bereitgestellt. 

In Bash enthält die Dateifreigabe auch ein 5-GB-Image, das für Sie erstellt wird und mit dem Daten automatisch in Ihrem Verzeichnis `$Home` beibehalten werden. 

### <a name="use-existing-resources"></a>Verwenden vorhandener Ressourcen

Sie können vorhandene Ressourcen zuordnen, indem Sie die erweiterte Option verwenden. Wählen Sie bei der Anzeige mit der Aufforderung zum Einrichten des Speichers die Option **Erweiterte Einstellungen anzeigen**, um weitere Optionen anzuzeigen. Die Dropdownmenüs werden für Ihre zugewiesene Cloud Shell-Region und die Konten für den lokal redundanten und georedundanten Speicher gefiltert.

In Bash erhalten vorhandene Dateifreigaben ein für Sie erstelltes 5-GB-Image zum Beibehalten Ihres Verzeichnisses `$Home`.

![Einstellung „Ressourcengruppe“](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Beschränken der Ressourcenerstellung mit einer Azure-Ressourcenrichtlinie
Speicherkonten, die Sie in Cloud Shell erstellen, erhalten das Tag `ms-resource-usage:azure-cloud-shell`. Wenn Sie nicht möchten, dass Benutzer Speicherkonten in Cloud Shell erstellen, können Sie eine [Azure-Ressourcenrichtlinie für Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) erstellen, die durch das jeweilige Tag ausgelöst werden.

## <a name="supported-storage-regions"></a>Unterstützte Speicherregionen
Zugeordnete Azure-Speicherkonten müssen sich in derselben Region wie der Cloud Shell-Computer befinden, auf dem Sie diese bereitstellen.

Sie können wie folgt vorgehen, um nach Ihrer zugewiesenen Region zu suchen:
* Anzeigen des Hinweises im Dialogfeld „Advanced storage settings“ (Erweiterte Speichereinstellungen)
* Verwenden des Namens des für Sie erstellten Speicherkontos (z.B. `cloud-shell-storage-westus`)
* Ausführen von `env` und Suchen nach der Variablen `ACC_LOCATION`

Cloud Shell-Computer sind in folgenden Regionen vorhanden:
|Bereich|Region|
|---|---|
|Amerika|USA, Osten; USA, Süden-Mitte; USA, Westen|
|Europa|„Europa, Norden“, „Europa, Westen“|
|Asien-Pazifik|Indien, Mitte; Asien, Südosten|


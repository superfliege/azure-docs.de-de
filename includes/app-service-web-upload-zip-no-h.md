Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressourcengruppen** > **cloud-shell-storage-\<Ihre_Region>** > **\<Speicherkontoname>**.

![Suchen nach dem Cloud Shell-Speicherkonto](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

Wählen Sie auf der Seite **Übersicht** des Speicherkontos die Option **Dateien**.

Wählen Sie die automatisch generierte Dateifreigabe aus, und klicken Sie auf **Hochladen**. Diese Dateifreigabe wird in der Cloud Shell als `clouddrive` bereitgestellt.

![Schaltfläche „Hochladen“](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Klicken Sie auf die Dateiauswahl, wählen Sie Ihre ZIP-Datei aus, und klicken Sie dann auf **Hochladen**. 

Verwenden Sie in der Cloud Shell den Befehl `ls`, um zu überprüfen, ob die hochgeladene ZIP-Datei in der Standardfreigabe `clouddrive` angezeigt wird.

```azurecli-interactive
ls clouddrive
```

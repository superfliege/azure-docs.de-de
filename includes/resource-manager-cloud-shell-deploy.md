## <a name="deploy-template-from-cloud-shell"></a>Bereitstellen der Vorlage über Cloud Shell

Sie können Ihre Vorlage mithilfe von [Cloud Shell](../articles/cloud-shell/overview.md) bereitstellen. Die Vorlage muss allerdings zuerst in die Dateifreigabe für Ihre Cloud Shell-Instanz geladen werden. Für den Fall, dass Sie Cloud Shell noch nicht verwendet haben, finden Sie unter [Übersicht über Azure Cloud Shell (Vorschau)](../articles/cloud-shell/overview.md) Informationen zum Einrichten von Cloud Shell.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

1. Wählen Sie Ihre Cloud Shell-Ressourcengruppe aus. Namensmuster: `cloud-shell-storage-<region>`.

   ![Auswählen der Ressourcengruppe](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Wählen Sie das Speicherkonto für Ihre Cloud Shell-Instanz aus.

   ![Auswählen des Speicherkontos](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Wählen Sie **Dateien** aus.

   ![Auswählen von Dateien](./media/resource-manager-cloud-shell-deploy/select-files.png)

1. Wählen Sie die Dateifreigabe für Cloud Shell aus. Namensmuster: `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Auswählen der Dateifreigabe](./media/resource-manager-cloud-shell-deploy/select-file-share.png)

1. Wählen Sie **Verzeichnis hinzufügen** aus.

   ![Hinzufügen des Verzeichnisses](./media/resource-manager-cloud-shell-deploy/select-add-directory.png)

1. Nennen Sie es **templates**, und wählen Sie **OK** aus.

   ![Benennen des Verzeichnisses](./media/resource-manager-cloud-shell-deploy/name-templates.png)

1. Wählen Sie Ihr neues Verzeichnis aus.

   ![Auswählen des Verzeichnisses](./media/resource-manager-cloud-shell-deploy/select-templates.png)

1. Wählen Sie die Option **Hochladen**.

   ![Auswählen von „Hochladen“](./media/resource-manager-cloud-shell-deploy/select-upload.png)

1. Suchen Sie Ihre Vorlage, und laden Sie sie hoch.

   ![Hochladen der Datei](./media/resource-manager-cloud-shell-deploy/upload-files.png)

1. Öffnen Sie die Eingabeaufforderung.

   ![Öffnen von Cloud Shell](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)

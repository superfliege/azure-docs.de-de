## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-roles"></a>Azure-Rollen
Damit Sie Data Factory-Instanzen erstellen können, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, ein Mitglied der Rolle **Mitwirkender** oder **Besitzer** oder ein **Administrator** des Azure-Abonnements sein. Klicken Sie im Azure-Portal in der oberen rechten Ecke auf Ihren **Benutzernamen** und auf **Berechtigungen**, um Ihre Berechtigungen im Abonnement anzuzeigen. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. Beispielanweisungen zum Hinzufügen eines Benutzers zu einer Rolle finden Sie im Artikel [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](../articles/billing/billing-add-change-azure-subscription-administrator.md).

### <a name="azure-storage-account"></a>Azure Storage-Konto
Sie verwenden in diesem Schnellstart ein Azure Storage-Allzweckkonto (Blob Storage) als Datenspeicher vom Typ **Quelle** und vom Typ **Ziel**. Falls Sie noch nicht über ein Azure Storage-Allzweckkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) weiter. 

#### <a name="get-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Schnellstart verwenden Sie Name und Schlüssel Ihres Azure-Speicherkontos. Das folgende Verfahren enthält die Schritte zum Abrufen des Namens und Schlüssels für Ihr Speicherkonto. 

1. Starten Sie einen Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com). Melden Sie sich mit Ihrem Azure-Benutzernamen und -Kennwort an. 
2. Klicken Sie im Menü auf der linken Seite auf **Weitere Dienste >**, filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie **Speicherkonten**.

    ![Suchen nach einem Speicherkonto](media/data-factory-quickstart-prerequisites/search-storage-account.png)
3. Filtern Sie in der Liste mit den Speicherkonten nach Ihrem Speicherkonto (falls erforderlich), und wählen Sie **Ihr Speicherkonto** aus. 
4. Wählen Sie auf der Seite **Speicherkonto** im Menü die Option **Zugriffsschlüssel**.

    ![Abrufen des Speicherkontonamens und -schlüssels](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. Kopieren Sie die Werte für die Felder **Speicherkontoname** und **key1** in die Zwischenablage. Fügen Sie sie in einen Editor ein, und speichern Sie sie. Sie benötigen sie später in dieser Schnellstartanleitung noch.   

#### <a name="create-input-folder-and-files"></a>Erstellen des Eingabeordners und der Dateien
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen **adftutorial** in Ihrem Azure-Blobspeicher. Anschließend erstellen Sie den Ordner **Eingabe** im Container und laden eine Beispieldatei in den Eingabeordner hoch. 

1. Wechseln Sie auf der Seite **Speicherkonto** zu **Übersicht**, und klicken Sie dann auf **Blobs**. 

    ![Option zum Auswählen von Blobs](media/data-factory-quickstart-prerequisites/select-blobs.png)
2. Klicken Sie auf der Seite **Blob-Dienst** auf der Symbolleiste auf **+ Container**. 

    ![Schaltfläche „Container hinzufügen“](media/data-factory-quickstart-prerequisites/add-container-button.png)    
3. Geben Sie im Dialogfeld **Neuer Container** als Namen **adftutorial** ein, und klicken Sie auf **OK**. 

    ![Eingeben des Containernamens](media/data-factory-quickstart-prerequisites/new-container-dialog.png)
4. Klicken Sie in der Liste der Container auf **adftutorial**. 

    ![Auswählen des Containers](media/data-factory-quickstart-prerequisites/seelct-adftutorial-container.png)
1. Klicken Sie auf der Seite **Container** auf der Symbolleiste auf **Hochladen**.  

    ![Schaltfläche zum Hochladen](media/data-factory-quickstart-prerequisites/upload-toolbar-button.png)
6. Klicken Sie auf der Seite **Blob hochladen** auf **Erweitert**.

    ![Klicken auf den Link „Erweitert“](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
7. Starten Sie **Editor**, und erstellen Sie eine Datei mit dem Namen **emp.txt** und folgendem Inhalt: Speichern Sie sie im Ordner **c:\ADFv2QuickStartPSH**: Erstellen Sie den Ordner **ADFv2QuickStartPSH**, sofern er noch nicht vorhanden ist.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. Wählen Sie im Azure-Portal auf der Seite **Blob hochladen** im Feld **Dateien** die Datei **emp.txt** aus. 
9. Geben Sie für das Feld **In Ordner hochladen** den Wert **Eingabe** ein. 

    ![Einstellungen für das Hochladen von Blobs](media/data-factory-quickstart-prerequisites/upload-blob-settings.png)    
10. Vergewissern Sie sich, dass als Ordner **Eingabe** und als Datei **emp.txt** ausgewählt ist, und klicken Sie auf **Hochladen**.
11. Daraufhin sollten in der Liste die Datei **emp.txt** und der Status des Uploads angezeigt werden. 
12. Schließen Sie die Seite **Blob hochladen**, indem Sie in der Ecke auf **X** klicken. 

    ![Schließen der Seite „Blob hochladen“](media/data-factory-quickstart-prerequisites/close-upload-blob.png)
1. Lassen Sie die Seite **Container** geöffnet. Sie überprüfen darauf am Ende dieser Schnellstartanleitung die Ausgabe.
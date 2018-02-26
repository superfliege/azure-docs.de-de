1. Klicken Sie links oben im Azure-Portal auf **Ressource erstellen** und anschließend auf **Compute** > **Funktionen-App**. 

    ![Erstellen einer Funktions-App im Azure-Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Verwenden Sie die in der Tabelle unter der Abbildung angegebenen Einstellungen für die Funktions-App.

    ![Definieren neuer Funktions-App-Einstellungen](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App-Name** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind `a-z`, `0-9` und `-`.  | 
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Funktions-App erstellt wird. | 
    | **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird | 
    | **Betriebssystem** | Windows | Das serverlose Hosting ist derzeit nur bei der Ausführung unter Windows verfügbar. Weitere Informationen zum Hosting unter Linux finden Sie unter [Erstellen Ihrer ersten Funktion unter Linux in der Azure-Befehlszeilenschnittstelle](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Hostingplan](../articles/azure-functions/functions-scale.md)** |   Verbrauchsplan | Der Hostingplan, der definiert, wie Ihre Ressourcen der Funktionen-App zugewiesen werden Im **Standard-Verbrauchstarif** werden Ressourcen je nach Bedarf der Funktionen dynamisch hinzugefügt. Beim [serverlosen Hosting](https://azure.microsoft.com/overview/serverless-computing/) bezahlen Sie nur die Zeit, in der Ihre Funktionen ausgeführt werden.   |
    | **Location** | Europa, Westen | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |
    | **[Speicherkonto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Global eindeutiger Name |  Der Name des neuen Speicherkontos, das von Ihrer Funktionen-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können außerdem ein vorhandenes Konto verwenden. |

1. Klicken Sie auf **Erstellen** , um die neue Funktionen-App bereitzustellen. Sie können den Status der Bereitstellung überwachen, indem Sie auf das Benachrichtigungssymbol rechts oben im Portal klicken. 

    ![Definieren neuer Funktions-App-Einstellungen](./media/functions-create-function-app-portal/function-app-create-notification.png)

    Durch Klicken auf **Zu Ressource wechseln** gelangen Sie zu der neuen Funktions-App.

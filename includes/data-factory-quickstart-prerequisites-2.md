### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-powershell"></a>Installieren von PowerShell
Installieren Sie die neueste Version von PowerShell, falls sie auf Ihrem Computer noch nicht vorhanden ist. 

1. Navigieren Sie in Ihrem Webbrowser zur [Azure-Downloadseite](https://azure.microsoft.com/downloads/). 
2. Klicken Sie unter **Befehlszeilentools** -> **PowerShell** auf **Windows-Installation**. 
3. Führen Sie die **MSI**-Datei aus, um PowerShell zu installieren. 

Ausführliche Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>Anmelden an PowerShell

1. Starten Sie **PowerShell** auf Ihrem Computer. Lassen Sie PowerShell bis zum Ende dieser Schnellstartanleitung geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.

    ![Starten von PowerShell](media/data-factory-quickstart-prerequisites-2/search-powershell.png)
1. Führen Sie den folgenden Befehl aus, und geben Sie den gleichen Azure-Benutzernamen und das gleiche Kennwort ein, die Sie bei der Anmeldung beim Azure-Portal verwendet haben:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Sollten Ihrem Konto mehrere Azure-Abonnements zugeordnet sein, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen. Ersetzen Sie **SubscriptionId** durch die ID Ihres Azure-Abonnements:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

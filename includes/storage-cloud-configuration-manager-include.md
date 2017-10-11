Die [Microsoft Azure Configuration Manager-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) stellt eine Klasse für die Analyse einer Verbindungszeichenfolge aus einer Konfigurationsdatei bereit. Die [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) Klasse analysiert Konfigurationseinstellungen unabhängig davon, ob die Clientanwendung auf dem Desktop auf einem mobilen Gerät, das im virtuellen Computer in Azure oder in einem Azure-Cloud-Dienst ausgeführt wird.

Um das Paket CloudConfigurationManager zu verweisen, fügen Sie die folgenden `using` Richtlinie:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

Hier ist ein Beispiel, das zeigt, wie Sie eine Verbindungszeichenfolge aus einer Konfigurationsdatei abrufen:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Mithilfe des Azure-Konfigurations-Managers ist optional. Sie können auch eine API wie die .NET Framework [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) Klasse.


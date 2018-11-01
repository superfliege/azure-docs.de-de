---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 3a065e5cd6e951544b3147d5833b4ad300ae5e30
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165695"
---
Die [Microsoft Azure Configuration Manager-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) stellt eine Klasse für die Analyse einer Verbindungszeichenfolge aus einer Konfigurationsdatei bereit. Die [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx)-Klasse analysiert Konfigurationseinstellungen unabhängig davon, ob die Clientanwendung auf dem Desktop, auf einem mobilen Gerät, auf einem virtuellen Azure-Computer oder in einem Azure-Clouddienst ausgeführt wird.

Fügen Sie Ihrer Klasse die folgende `using`-Anweisung hinzu, um auf das CloudConfigurationManager-Paket zu verweisen:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

Das folgende Beispiel zeigt, wie Sie eine Verbindungszeichenfolge aus einer Konfigurationsdatei abrufen:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Die Verwendung von Azure Configuration Manager ist optional. Sie können auch eine API wie die [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx)-Klasse von .NET Framework verwenden.


---
title: 'Azure PowerShell-Skriptbeispiel: Migrieren von Blobs zwischen Speicherkonten mit AzCopy unter Windows | Microsoft-Dokumentation'
description: Dieses Beispiel kopiert mithilfe von AzCopy die Blobinhalte eines Azure Storage-Kontos in ein anderes Konto.
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/01/2018
ms.author: rogarana
ms.openlocfilehash: 970315c5d597d691454f9dea0a76f2c0dc4a40ec
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
ms.locfileid: "29360716"
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Migrieren von Blobs zwischen Speicherkonten mit AzCopy unter Windows

Dieses Beispiel kopiert alle Blobobjekte aus einem vom Benutzer bereitgestellten Quellspeicherkonto in ein vom Benutzer bereitgestelltes Zielspeicherkonto. 

Dies erfolgt mithilfe des `Get-AzureStorageContainer`-Befehls, der alle Container in einem Speicherkonto auflistet. Danach gibt das Beispiel AzCopy-Befehle aus, um jeden Container aus dem Quellspeicherkonto in das Zielspeicherkonto zu kopieren. Im Fall eines Fehlers unternimmt das Beispiel so viele Wiederholungsversuche, wie in „$retryTimes“ festgelegt ist (der Standardwert ist 3 und kann mit dem Parameter `-RetryTimes` geändert werden). Wenn bei jedem Wiederholungsversuch ein Fehler auftritt, kann der Benutzer über den Parameter `-LastSuccessContainerName` den letzten erfolgreich kopierten Container angeben und das Skript erneut ausführen. Das Beispiel fährt dann an diesem Punkt mit dem Kopieren von Containern fort.

Für dieses Beispiel ist das Azure PowerShell-Speichermodul Version **4.0.2** oder höher erforderlich. Sie können Ihre Version mithilfe von `Get-Module -ListAvailable Azure.storage` überprüfen. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Dieses Beispiel erfordert auch die neueste Version von [AzCopy unter Windows](http://aka.ms/downloadazcopy). Das Standardinstallationsverzeichnis lautet `C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`.

Dieses Beispiel verwendet den Namen und Schlüssel eines Quellspeicherkontos, den Namen und Schlüssel eines Zielspeicherkontos und den vollständigen Dateipfad der AzCopy.exe-Datei (falls die Installation nicht im Standardverzeichnis erfolgt ist).

Im Folgenden finden Sie Beispiele für die Eingabe für dieses Beispiel:

Wenn AzCopy im Standardverzeichnis installiert ist:
```PowerShell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
```

Wenn AzCopy nicht im Standardverzeichnis installiert ist:

```Powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
AzCopyPath: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe
```

Wenn ein Fehler auftritt und das Beispiel aus einem bestimmten Container erneut ausgeführt werden muss: 

`.\copyScript.ps1 -LastSuccessContainerName myContainerName`

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/migrate-blobs-between-accounts/migrate-blobs-between-accounts.ps1 "Migrate blobs between storage accounts.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um Daten aus einem Speicherkonto in ein anderes zu kopieren. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzureStorageContainer](/powershell/module/azure.storage/Get-AzureStorageContainer) | Gibt die Container zurück, die diesem Storage-Konto zugeordnet sind. |
| [New-AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Erstellt einen Azure Storage-Kontext. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für Speicher finden Sie in den [PowerShell-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-powershell.md).

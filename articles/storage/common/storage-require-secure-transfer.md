---
title: "Vorschreiben einer sicheren Übertragung in Azure Storage | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Feature „Sichere Übertragung erforderlich“ für Azure Storage. Zudem wird beschrieben, wie das Feature aktiviert wird."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: 1bb87cf3e37e486f9a03da43df652442c19fd218
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="require-secure-transfer-in-azure-storage"></a>Vorschreiben einer sicheren Übertragung in Azure Storage

Die Funktion „Sichere Übertragung erforderlich“ steigert die Sicherheit Ihres Speicherkontos, da Anforderungen an das Konto von sicheren Verbindungen zugelassen werden. Beim Aufrufen von REST-APIs zum Zugreifen auf das Speicherkonto müssen Sie beispielsweise eine Verbindung über HTTPS herstellen. „Sichere Übertragung erforderlich“ lehnt Anforderungen ab, die HTTP verwenden.

Wenn Sie den Azure Files-Dienst verwenden, schlägt jede Verbindung ohne Verschlüsselung fehl, wenn „Sichere Übertragung erforderlich“ aktiviert ist. Hierzu zählen Szenarien, in denen SMB 2.1, SMB 3.0 ohne Verschlüsselung und einige Versionen des Linux SMB-Clients verwendet werden. 

In der Standardeinstellung ist die Option „Sichere Übertragung erforderlich“ deaktiviert.

> [!NOTE]
> Da Azure Storage keine Unterstützung von HTTPS für benutzerdefinierte Domänennamen bietet, wird diese Option nicht angewendet, wenn ein benutzerdefinierter Domänenname verwendet wird. Zudem werden klassische Speicherkonten nicht unterstützt.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>Aktivieren von „Sichere Übertragung erforderlich“ im Azure-Portal

Sie können die Einstellung „Sichere Übertragung erforderlich“ beim Erstellen eines Speicherkontos im [Azure-Portal](https://portal.azure.com) aktivieren. Sie können sie aber auch für vorhandene Speicherkonten aktivieren.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Vorschreiben einer sicheren Übertragung für ein neues Speicherkonto

1. Öffnen Sie den Bereich **Speicherkonto erstellen** im Azure-Portal.
1. Wählen Sie unter **Sichere Übertragung erforderlich** die Option **Aktiviert** aus.

  ![Blatt „Speicherkonto erstellen“](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Vorschreiben einer sicheren Übertragung für ein vorhandenes Speicherkonto

1. Wählen Sie ein vorhandenes Speicherkonto im Azure-Portal aus.
1. Wählen Sie im Menübereich des Speicherkontos unter **EINSTELLUNGEN** die Option **Konfiguration** aus.
1. Wählen Sie unter **Sichere Übertragung erforderlich** die Option **Aktiviert** aus.

  ![Menübereich „Speicherkonto“](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>Programmgesteuertes Aktivieren der Einstellung „Sichere Übertragung erforderlich“

Um die sichere Übertragung programmgesteuert zu erfordern, verwenden Sie die Einstellung _supportsHttpsTrafficOnly_ in den Eigenschaften des Speicherkontos mit REST-API, Tools oder Bibliotheken:

* [REST-API](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts) (Version: 2016-12-01)
* [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0) (Version: 4.1.0)
* [CLI](https://pypi.python.org/pypi/azure-cli-storage/2.0.11) (Version: 2.0.11)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/) (Version: 1.1.0)
* [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview) (Version: 6.3.0)
* [Python SDK](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0) (Version: 1.1.0)
* [Ruby SDK](https://rubygems.org/gems/azure_mgmt_storage) (Version: 0.11.0)

### <a name="enable-secure-transfer-required-setting-with-powershell"></a>Aktivieren der Einstellung „Sichere Übertragung erforderlich“ mit PowerShell

Für dieses Beispiel ist das Azure PowerShell-Modul Version 4.1 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Führen Sie zum Starten `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen.

 Verwenden Sie die folgende Befehlszeile, um die Einstellung zu überprüfen:

```powershell
> Get-AzureRmStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}"
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : False
...

```

Verwenden Sie die folgende Befehlszeile, um die Einstellung zu aktivieren:

```powershell
> Set-AzureRmStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $True
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : True
...

```

### <a name="enable-secure-transfer-required-setting-with-cli"></a>Aktivieren der Einstellung „Sichere Übertragung erforderlich“ per Befehlszeilenschnittstelle

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 Verwenden Sie die folgende Befehlszeile, um die Einstellung zu überprüfen:

```azurecli-interactive
> az storage account show -g {ResourceGroupName} -n {StorageAccountName}
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": false,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

Verwenden Sie die folgende Befehlszeile, um die Einstellung zu aktivieren:

```azurecli-interactive
> az storage account update -g {ResourceGroupName} -n {StorageAccountName} --https-only true
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": true,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

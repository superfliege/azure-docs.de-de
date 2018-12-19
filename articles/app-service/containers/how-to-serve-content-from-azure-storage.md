---
title: Bereitstellen von Inhalt aus Azure Storage in Linux – App Service
description: Informationen zum Konfigurieren und Bereitstellen von Inhalten aus Azure Storage in Azure App Service unter Linux.
author: msangapu
manager: jeconnoc
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 11/01/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 855df2820db6bba2b47d543ab671bee2193d7d9b
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250915"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Bereitstellen von Inhalt aus Azure Storage in App Service unter Linux

Diese Anleitung verdeutlicht die Bereitstellen statischen Inhalts in App Service unter Linux mithilfe von [Azure Storage](/azure/storage/common/storage-introduction). Zu den Vorteilen gehören sicherer Inhalt, die Portabilität des Inhalts, Zugriff auf mehrere Apps und mehrere Übertragungsmethoden. In diesem Leitfaden erfahren Sie, wie Sie Inhalte in Azure Storage durch das [Konfigurieren eines benutzerdefinierten Speichers](https://blogs.msdn.microsoft.com/appserviceteam/2018/09/24/announcing-bring-your-own-storage-to-app-service/) bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Eine vorhandene Web-App (App Service unter Linux oder Web-App für Container).
- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 oder höher).

## <a name="create-azure-storage"></a>Erstellen eines Azure Storage

> [!NOTE]
> Azure Storage ist ein nicht standardmäßiger Speicher und wird separat berechnet, nicht im Lieferumfang der Web-App enthalten.
>

Erstellen Sie ein [Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Hochladen von Dateien zu Azure Storage

Um ein lokales Verzeichnis in das Speicherkonto hochzuladen, verwenden Sie den Befehl [`az storage blob upload-batch`](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch) wie im folgenden Beispiel:

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app"></a>Verknüpfen des Speichers mit Ihrer Web-App

> [!CAUTION]
> Beim Verknüpfen eines vorhandenen Verzeichnis in einer Web-App mit einem Speicherkonto werden die Inhalte des Verzeichnisses gelöscht. Wenn Sie Dateien für eine bestehende Anwendung migrieren, erstellen Sie vor Beginn eine Sicherung Ihrer Anwendung und ihres Inhalts.
>

Um ein Speicherkonto in einem Verzeichnis in Ihre App Service-App bereitzustellen, verwenden Sie den Befehl [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). Als Speichertyp kann „AzureBlob“ oder „AzureFiles“ verwendet werden. Verwenden Sie „AzureBlob“ für diesen Container.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Gehen Sie genauso für alle anderen Verzeichnisse vor, die mit einem Speicherkonto verknüpft werden sollen.

## <a name="verify"></a>Überprüfen

Sobald ein Speichercontainer mit einer Web-App verknüpft ist, können Sie dies durch Ausführen des folgenden Befehls überprüfen:

```azurecli
az webapp config storage-account list --resource-group <group_name> --name <app_name>
```

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-configure)

---
title: Ausführen von Azure CLI- oder PowerShell-Befehlen unter einer Azure AD-Identität für den Zugriff auf Blob- und Warteschlangendaten | Microsoft-Dokumentation
description: Die Azure-Befehlszeilenschnittstelle und PowerShell unterstützen die Anmeldung mit einer Azure AD-Identität zum Ausführen von Befehlen für Azure Storage-Blobs und -Warteschlangen und die enthaltenen Daten. Ein Zugriffstoken wird jeweils für die Sitzung bereitgestellt und zum Autorisieren von Aufrufvorgängen verwendet. Die Berechtigungen hängen von der RBAC-Rolle ab, die der Azure AD-Identität zugewiesen ist.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/26/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: a0972beff48e07b6ce8afdcec10581300f59ed41
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803577"
---
# <a name="use-an-azure-ad-identity-to-access-blob-and-queue-data-with-cli-or-powershell"></a>Verwenden einer Azure AD-Identität für den Zugriff auf Blob- und Warteschlangendaten über die Befehlszeilenschnittstelle oder PowerShell

Azure Storage umfasst Erweiterungen für die Azure-Befehlszeilenschnittstelle und PowerShell, mit denen Sie sich anmelden und Skriptbefehle unter einer Azure Active Directory-Identität (Azure AD) ausführen können. Eine Azure AD-Identität kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) sein. Sie können der Azure AD-Identität über die rollenbasierte Zugriffssteuerung (RBAC) Berechtigungen für den Zugriff auf Blob- und Warteschlangendaten zuweisen. Weitere Informationen zu RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC](storage-auth-aad-rbac.md).

Wenn Sie sich bei der Azure-Befehlszeilenschnittstelle oder PowerShell mit einer Azure AD-Identität anmelden, wird ein Zugriffstoken für den Zugriff auf Azure Storage mit dieser Identität zurückgegeben. Dieses Token wird dann automatisch von der Befehlszeilenschnittstelle oder PowerShell verwendet, um Vorgänge in Azure Storage zu autorisieren. Für unterstützte Vorgänge müssen Sie mit dem Befehl keinen Kontoschlüssel und kein SAS-Token mehr übergeben.

## <a name="supported-operations"></a>Unterstützte Vorgänge

Die Erweiterungen werden für Vorgänge in Containern und Warteschlangen unterstützt. Welche Vorgänge Sie aufrufen können, hängt von den Berechtigungen der Azure AD-Identität ab, mit der Sie sich bei der Azure-Befehlszeilenschnittstelle oder PowerShell anmelden. Die Berechtigungen für Azure Storage-Container oder -Warteschlangen werden über die rollenbasierte Zugriffssteuerung (RBAC) zugewiesen. Wenn der Identität z.B. die Rolle „Datenleser“ zugewiesen wird, können Sie Skriptbefehle ausführen, die Daten aus einem Container oder einer Warteschlange lesen. Wenn der Identität die Rolle „Mitwirkender an Daten“ zugewiesen wird, können Sie Skriptbefehle ausführen, die einen Container oder eine Warteschlange oder die darin enthaltenen Daten lesen, schreiben oder löschen. 

Einzelheiten zu den Berechtigungen, die für die einzelnen Azure Storage-Vorgänge in einem Container oder einer Warteschlange erforderlich sind, finden Sie unter [Berechtigungen für das Aufrufen von REST-Vorgängen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Aufrufen von Befehlszeilenschnittstellenbefehlen mithilfe von Azure AD-Anmeldeinformationen

Die Azure-Befehlszeilenschnittstelle unterstützt den Parameter `--auth-mode` für Datenvorgänge in Blobs und Warteschlangen:

- Legen Sie den Parameter `--auth-mode` auf `login` fest, um sich mit einem Azure AD-Sicherheitsprinzipal anzumelden.
- Legen Sie den Parameter `--auth-mode` auf den älteren Wert `key` fest, um zu versuchen, einen Kontoschlüssel abzurufen, wenn keine Authentifizierungsparameter für das Konto bereitgestellt werden. 

Im folgenden Beispiel sehen Sie, wie in einem neuen Speicherkonto mithilfe Ihrer Azure AD-Anmeldeinformationen über die Azure-Befehlszeilenschnittstelle ein Container erstellt wird. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen: 

1. Vergewissern Sie sich, dass mindestens Version 2.0.46 der Azure-Befehlszeilenschnittstelle installiert ist. Führen Sie `az --version` aus, um Ihre installierte Version zu überprüfen.

1. Führen Sie `az login` aus, und authentifizieren Sie sich im Browserfenster: 

    ```azurecli
    az login
    ```
    
1. Geben Sie das gewünschte Abonnement an. Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) eine Ressourcengruppe. Erstellen Sie mithilfe des Befehls [az storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) ein Speicherkonto in dieser Ressourcengruppe: 

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```
    
1. Weisen Sie sich vor der Erstellung des Containers selbst die Rolle [Mitwirkender an Storage-Blobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) zu. Obwohl Sie der Kontobesitzer sind, benötigen Sie explizite Berechtigungen, um Datenvorgänge in Ihrem Speicherkonto ausführen zu können. Weitere Informationen zum Zuweisen von RBAC-Rollen finden Sie unter [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über das Azure-Portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Die RBAC-Rollenzuweisungen können einige Minuten dauern.
    
1. Rufen Sie mithilfe des `--auth-mode`-Parameters, für den `login` festgelegt wurde, den Befehl [az storage container create](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) auf, um den Container mithilfe Ihrer Azure AD-Anmeldeinformationen zu erstellen:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

Dem Parameter `--auth-mode` ist die Umgebungsvariable `AZURE_STORAGE_AUTH_MODE` zugeordnet. Sie können den passenden Wert in der Umgebungsvariable angeben, um zu umgehen, ihn in jedem Aufruf eines Azure Storage-Datenvorgangs einschließen zu müssen.

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Aufrufen von PowerShell-Befehlen mithilfe von Azure AD-Anmeldeinformationen

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Wenn Sie sich über Azure PowerShell anmelden und mithilfe der Azure AD-Anmeldeinformationen nachfolgende Vorgänge in Azure Storage ausführen möchten, erstellen Sie einen Speicherkontext als Verweis für das Speicherkonto. Der `-UseConnectedAccount`-Parameter sollte eingeschlossen werden.

Im folgenden Beispiel sehen Sie, wie in einem neuen Speicherkonto mithilfe Ihrer Azure AD-Anmeldeinformationen über Azure PowerShell ein Container erstellt wird. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

1. Melden Sie sich mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Authentifizierungsanweisungen auf dem Bildschirm, um Ihre Azure AD-Anmeldeinformationen einzugeben: 

    ```powershell
    Connect-AzAccount
    ```
    
1. Erstellen Sie eine Azure-Ressourcengruppe, indem Sie [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) aufrufen. 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Erstellen Sie ein Speicherkonto, indem Sie [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) aufrufen.

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Rufen Sie den Speicherkontokontext ab, der das neue Speicherkonto bestimmt, indem Sie [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext) aufrufen. Wenn Sie Aktionen für ein Speicherkonto ausführen, können Sie auf den Kontext verweisen, anstatt immer wieder die Anmeldeinformationen zu übergeben. Schließen Sie den `-UseConnectedAccount`-Parameter ein, um mithilfe Ihrer Azure AD-Anmeldeinformationen beliebige anschließende Datenvorgänge aufzurufen:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Weisen Sie sich vor der Erstellung des Containers selbst die Rolle [Mitwirkender an Storage-Blobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) zu. Obwohl Sie der Kontobesitzer sind, benötigen Sie explizite Berechtigungen, um Datenvorgänge in Ihrem Speicherkonto ausführen zu können. Weitere Informationen zum Zuweisen von RBAC-Rollen finden Sie unter [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über das Azure-Portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Die RBAC-Rollenzuweisungen können einige Minuten dauern.

1. Erstellen Sie einen Container, indem Sie [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer) aufrufen. Da dieser Aufruf den Kontext verwendet, den Sie im vorherigen Schritt erstellt haben, wird der Container mithilfe Ihrer Azure AD-Anmeldeinformationen erstellt. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md).
- Informationen zur Verwendung verwalteter Identitäten für Azure-Ressourcen mit Azure Storage finden Sie unter [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Azure-Identitäten für Azure-Ressourcen](storage-auth-aad-msi.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).

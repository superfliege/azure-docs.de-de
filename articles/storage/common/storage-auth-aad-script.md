---
title: Ausführen von Azure CLI- oder PowerShell-Befehlen unter einer Azure AD-Identität für den Zugriff auf Azure Storage (Vorschau) | Microsoft-Dokumentation
description: Die Azure-Befehlszeilenschnittstelle und PowerShell unterstützen die Anmeldung mit einer Azure AD-Identität zum Ausführen von Befehlen für Azure Storage-Container und -Warteschlangen und die enthaltenen Daten. Ein Zugriffstoken wird jeweils für die Sitzung bereitgestellt und zum Autorisieren von Aufrufvorgängen verwendet. Die Berechtigungen hängen von der Rolle ab, die der Azure AD-Identität zugewiesen ist.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/20/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 6354d89ff5a23ccb51b85737b3a842c08534683e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223609"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Verwenden einer Azure AD-Identität für den Zugriff auf Azure Storage mit der Befehlszeilenschnittstelle oder PowerShell (Vorschau)

Azure Storage stellt Vorschauerweiterungen für die Azure-Befehlszeilenschnittstelle und PowerShell bereit, mit denen Sie sich anmelden und Skriptbefehle unter einer Azure AD-Identität (Azure Active Directory) ausführen können. Die Azure AD-Identität kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Dienstidentität](../../active-directory/managed-identities-azure-resources/overview.md) sein. Sie können der Azure AD-Identität über die rollenbasierte Zugriffssteuerung (RBAC) Berechtigungen für den Zugriff auf Speicherressourcen zuweisen. Weitere Informationen zu RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).

Wenn Sie sich bei der Azure-Befehlszeilenschnittstelle oder PowerShell mit einer Azure AD-Identität anmelden, wird ein Zugriffstoken für den Zugriff auf Azure Storage mit dieser Identität zurückgegeben. Dieses Token wird dann automatisch von der Befehlszeilenschnittstelle oder PowerShell verwendet, um Vorgänge in Azure Storage zu autorisieren. Für unterstützte Vorgänge müssen Sie mit dem Befehl keinen Kontoschlüssel und kein SAS-Token mehr übergeben.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="supported-operations"></a>Unterstützte Vorgänge

Die Vorschauerweiterungen werden für Vorgänge in Containern und Warteschlangen unterstützt. Welche Vorgänge Sie aufrufen können, hängt von den Berechtigungen der Azure AD-Identität, mit der Sie sich bei der Azure-Befehlszeilenschnittstelle oder PowerShell anmelden, ab. Die Berechtigungen für Azure Storage-Container oder -Warteschlangen werden über die rollenbasierte Zugriffssteuerung (RBAC) zugewiesen. Wenn der Identität z.B. die Rolle „Datenleser“ zugewiesen wird, können Sie Skriptbefehle ausführen, die Daten aus einem Container oder einer Warteschlange lesen. Wenn der Identität die Rolle „Mitwirkender an Daten“ zugewiesen wird, können Sie Skriptbefehle ausführen, die einen Container oder eine Warteschlange oder die darin enthaltenen Daten lesen, schreiben oder löschen. 

Einzelheiten zu den Berechtigungen, die für die einzelnen Azure Storage-Vorgänge in einem Container oder einer Warteschlange erforderlich sind, finden Sie unter [Berechtigungen für das Aufrufen von REST-Vorgängen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>Aufrufen von CLI-Befehlen mit einer Azure AD-Identität

So installieren Sie die Vorschauerweiterung für die Azure-Befehlszeilenschnittstelle

1. Vergewissern Sie sich, dass mindestens Version 2.0.32 der Azure-Befehlszeilenschnittstelle installiert ist. Führen Sie `az --version` aus, um Ihre installierte Version zu überprüfen.
2. Führen Sie den folgenden Befehl aus, um die Vorschauerweiterung zu installieren: 

    ```azurecli
    az extension add -n storage-preview
    ```

Die Vorschauerweiterung fügt den unterstützten Befehlen den neuen Parameter `--auth-mode` hinzu:

- Legen Sie den Parameter `--auth-mode` auf `login` fest, um sich mit einer Azure AD-Identität anzumelden.
- Legen Sie den Parameter `--auth-mode` auf den älteren Wert `key` fest, um zu versuchen, einen Kontoschlüssel abzurufen, wenn keine Authentifizierungsparameter für das Konto bereitgestellt werden. 

Um z.B. ein Blob in der Azure-Befehlszeilenschnittstelle mit einer Azure AD-Identität herunterzuladen, führen Sie zunächst `az login` aus und rufen dann den Befehl auf, wobei `--auth-mode` auf `login` festgelegt ist:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

Dem Parameter `--auth-mode` ist die Umgebungsvariable `AZURE_STORAGE_AUTH_MODE` zugeordnet.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>Aufrufen von PowerShell-Befehlen mit einer Azure AD-Identität

Azure PowerShell unterstützt die Anmeldung mit einer Azure AD-Identität nur mit einem der folgenden Vorschaumodule: 

- 4.4.0-preview 
- 4.4.1-preview 

So verwenden Sie Azure PowerShell für die Anmeldung mit einer Azure AD-Identität

1. Deinstallieren Sie alle älteren Installationen von Azure PowerShell:

    - Entfernen Sie alle früheren Installationen von Azure PowerShell mit der Einstellung **Apps & Features** (unter **Einstellungen**) aus Windows.
    - Entfernen Sie alle **Azure***-Module aus `%Program Files%\WindowsPowerShell\Modules`.

1. Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie den folgenden Befehl aus, um die neueste Version zu installieren:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```
1. Schließen Sie nach dem Installieren von PowerShellGet das PowerShell-Fenster, und öffnen Sie es dann erneut. 

1. Installieren Sie die neueste Version von Azure PowerShell:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

1. Installieren Sie eines der Azure Storage-Vorschaumodule, das Azure AD unterstützt:

    ```powershell
    Install-Module Azure.Storage –Repository PSGallery -RequiredVersion 4.4.1-preview  –AllowPrerelease –AllowClobber –Force 
    ```
1. Schließen Sie das PowerShell-Fenster, und öffnen Sie es dann erneut.
1. Rufen Sie das Cmdlet [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) auf, um einen Kontext zu erstellen und den Parameter `-UseConnectedAccount` einzufügen. 
1. Für das Aufrufen eines Cmdlets mit einer Azure AD-Identität übergeben Sie den neu erstellten Kontext an das Cmdlet.

Das folgende Beispiel zeigt, wie Sie die Blobs in einem Container aus Azure PowerShell mit einer Azure AD-Identität auflisten. Achten Sie darauf, dass Sie Platzhalterkonto- und Containernamen durch Ihre eigenen Werte ersetzen: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName storagesamples -UseConnectedAccount 
Get-AzureStorageBlob -Container sample-container -Context $ctx 
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).
- Informationen zur Verwendung verwalteter Identitäten für Azure-Ressourcen mit Azure Storage finden Sie unter [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Azure-Identitäten für Azure-Ressourcen (Vorschau)](storage-auth-aad-msi.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Weitere Informationen zur Azure AD-Integration für Azure-Blobs und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).

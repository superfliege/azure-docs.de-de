---
title: Ausführen von Azure CLI- oder PowerShell-Befehlen unter einer Azure AD-Identität für den Zugriff auf Azure Storage (Vorschau) | Microsoft-Dokumentation
description: Die Azure-Befehlszeilenschnittstelle und PowerShell unterstützen die Anmeldung mit einer Azure AD-Identität zum Ausführen von Befehlen für Azure Storage-Container und -Warteschlangen und die enthaltenen Daten. Ein Zugriffstoken wird jeweils für die Sitzung bereitgestellt und zum Autorisieren von Aufrufvorgängen verwendet. Die Berechtigungen hängen von der Rolle ab, die der Azure AD-Identität zugewiesen ist.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/29/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: abd4a3b21ede2ddbdede2ec133938d412d5d4c8d
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248164"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Verwenden einer Azure AD-Identität für den Zugriff auf Azure Storage mit der Befehlszeilenschnittstelle oder PowerShell (Vorschau)

Azure Storage stellt Vorschauerweiterungen für die Azure-Befehlszeilenschnittstelle und PowerShell bereit, mit denen Sie sich anmelden und Skriptbefehle unter einer Azure AD-Identität (Azure Active Directory) ausführen können. Die Azure AD-Identität kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Dienstidentität](../../active-directory/managed-service-identity/overview.md) sein. Sie können der Azure AD-Identität über die rollenbasierte Zugriffssteuerung (RBAC) Berechtigungen für den Zugriff auf Speicherressourcen zuweisen. Weitere Informationen zu RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).

Wenn Sie sich bei der Azure-Befehlszeilenschnittstelle oder PowerShell mit einer Azure AD-Identität anmelden, wird ein Zugriffstoken für den Zugriff auf Azure Storage mit dieser Identität zurückgegeben. Dieses Token wird dann automatisch von der Befehlszeilenschnittstelle oder PowerShell verwendet, um Vorgänge in Azure Storage zu autorisieren. Für unterstützte Vorgänge müssen Sie mit dem Befehl keinen Kontoschlüssel und kein SAS-Token mehr übergeben.

> [!IMPORTANT]
> Diese Vorschau ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind erst dann verfügbar, wenn die Azure AD-Integration für Azure Storage als allgemein verfügbar deklariert wird. Wenn die Azure AD-Integration für Ihr Szenario noch nicht unterstützt wird, verwenden Sie in Ihren Anwendungen weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel oder SAS-Token. Weitere Informationen zur Vorschau finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory (Vorschau)](storage-auth-aad.md).
>
> In der Vorschauphase kann die Verteilung von RBAC-Rollenzuweisungen bis zu fünf Minuten dauern.
>
> Für die Azure AD-Integration mit Azure Storage müssen Sie HTTPS für Azure Storage-Vorgänge verwenden.

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

So verwenden Sie Azure PowerShell für die Anmeldung mit einer Azure AD-Identität

1. Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist. Führen Sie den folgenden Befehl aus, um die neueste Version zu installieren:
 
    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber -RequiredVersion "4.4.1-preview"
    ```

2. Deinstallieren Sie alle älteren Installationen von Azure PowerShell.
3. Installieren Sie AzureRM:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Installieren Sie das Vorschaumodul:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Rufen Sie das Cmdlet [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) auf, um einen Kontext zu erstellen und den Parameter `-UseConnectedAccount` einzufügen. 
6. Für das Aufrufen eines Cmdlets mit einer Azure AD-Identität übergeben Sie den Kontext an das Cmdlet.

Das folgende Beispiel zeigt, wie Sie die Blobs in einem Container aus Azure PowerShell mit einer Azure AD-Identität auflisten: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).
- Informationen zur Verwendung der verwalteten Dienstidentität mit Azure Storage finden Sie unter [Authentifizieren mit Azure AD über eine verwaltete Azure-Dienstidentität (Vorschau)](storage-auth-aad-msi.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Weitere Informationen zur Azure AD-Integration für Azure-Blobs und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).

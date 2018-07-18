---
title: Authentifizieren mit Azure AD über eine verwaltete Dienstidentität (MSI) eines virtuellen Azure-Computers (Vorschau) | Microsoft-Dokumentation
description: Authentifizieren mit Azure AD über eine verwaltete Dienstidentität (MSI) eines virtuellen Azure-Computers (Vorschau).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 080cb3ee536227e5ddce3fac856de79b2b061dcf
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970767"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Authentifizieren mit Azure AD über eine verwaltete Azure-Dienstidentität (Vorschau)

Azure Storage unterstützt die Azure AD-Authentifizierung (Azure Active Directory) über eine [verwaltete Dienstidentität](../../active-directory/managed-service-identity/overview.md). Eine verwaltete Dienstidentität (Managed Service Identity, MSI) stellt eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereit. Sie können aus Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden, aus Funktions-Apps, VM-Skalierungsgruppen usw. eine verwaltete Dienstidentität (MSI) zum Authentifizieren bei Azure Storage verwenden. Mithilfe der verwalteten Dienstidentität (MSI) und der Leistungsfähigkeit der Azure AD-Authentifizierung können Sie das Speichern von Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen vermeiden.  

Um einer verwalteten Dienstidentität (MSI) die Berechtigungen für Speichercontainer oder Warteschlangen zu gewähren, weisen Sie dieser eine RBAC-Rolle zu, welche die MSI-Speicherberechtigungen beinhaltet. Weitere Informationen zu RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC (Vorschau)](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Diese Vorschau ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind erst dann verfügbar, wenn die Azure AD-Integration für Azure Storage als allgemein verfügbar deklariert wird. Wenn die Azure AD-Integration für Ihr Szenario noch nicht unterstützt wird, verwenden Sie in Ihren Anwendungen weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel oder SAS-Token. Weitere Informationen zur Vorschau finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory (Vorschau)](storage-auth-aad.md).
>
> Während der Vorschau können RBAC-Rollenzuweisungen bis zu fünf Minuten dauern.

Dieser Artikel zeigt die Authentifizierung bei Azure Storage mit einer verwalteten Dienstidentität (MSI) von einem virtuellen Azure-Computer aus.  

## <a name="enable-msi-on-the-vm"></a>Aktivieren der verwalteten Dienstidentität (MSI) auf dem virtuellen Computer

Bevor Sie MSI zum Authentifizieren bei Azure Storage vom virtuellen Computer aus verwenden können, müssen Sie die verwaltete Dienstidentität zuerst auf dem virtuellen Computer aktivieren. Lesen Sie einen der folgenden Artikel, um zu erfahren, wie Sie MSI aktivieren:

- [Azure-Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-service-identity/qs-configure-powershell-windows-vm.md)
- [Azure-CLI](../../active-directory/managed-service-identity/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-service-identity/qs-configure-template-windows-vm.md)
- [Azure SDKs](../../active-directory/managed-service-identity/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>Abrufen eines MSI-Zugriffstokens

Zum Authentifizieren mit MSI muss Ihre Anwendung oder das Skript ein MSI-Zugriffstoken beziehen. Weitere Informationen zum Beziehen eines Zugriffstokens finden Sie unter [Verwenden einer verwalteten Dienstidentität (MSI) eines virtuellen Azure-Computers für Tokenbezug](../../active-directory/managed-service-identity/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>.NET Codebeispiel: Erstellen eines Blockblobs

Im Codebeispiel wird davon ausgegangen, dass Sie ein MSI-Zugriffstoken haben. Das Zugriffstoken wird verwendet, um die verwaltete Dienstidentität für das Erstellen eines Blockblobs zu autorisieren.

### <a name="add-references-and-using-statements"></a>Hinzufügen von Verweisen und Verwenden von Anweisungen  

Installieren Sie in Visual Studio die Vorschauversion der Azure Storage-Clientbibliothek. Wählen Sie im Menü **Extras** den Eintrag **NuGet-Paket-Manager** und danach **Paket-Manager-Konsole** aus. Geben Sie an der Konsole folgenden Befehl ein:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Fügen Sie mithilfe von Anweisungen Ihrem Code Folgendes hinzu:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Erstellen von Anmeldeinformationen aus dem MSI-Zugriffstoken

Verwenden Sie zum Erstellen des Blockblobs die **TokenCredentials**-Klasse, die vom Vorschaupaket bereitgestellt wird. Erstellen Sie eine neue Instanz von **TokenCredentials**, und übergeben Sie das zuvor abgerufene MSI-Zugriffstoken:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Für die Azure AD-Integration mit Azure Storage müssen Sie HTTPS für Azure Storage-Vorgänge verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Informationen zum Anmelden bei der Azure-Befehlszeilenschnittstelle (CLI) und bei PowerShell mit einer Azure AD-Identität finden Sie unter [Verwenden einer Azure AD-Identität für den Zugriff auf Azure Storage über die Befehlszeilenschnittstelle oder PowerShell (Vorschau)](storage-auth-aad-script.md).
- Weitere Informationen zur Azure AD-Integration für Azure-Blobs und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).

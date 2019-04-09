---
title: Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Identitäten in Azure Active Directory für Azure-Ressourcen – Azure Storage | Microsoft-Dokumentation
description: Azure Blob Storage und Azure Queue Storage unterstützen die Azure Active Directory-Authentifizierung mit verwalteten Identitäten für Azure-Ressourcen. Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blobs und Warteschlangen über Anwendungen zu authentifizieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen u.a. ausgeführt werden. Mithilfe von verwalteten Identitäten für Azure-Ressourcen und der Leistungsfähigkeit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: dfdb419a5c06dc50717c0a8a3bdaffb302db52d0
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793015"
---
# <a name="authenticate-access-to-blobs-and-queues-with-managed-identities-for-azure-resources"></a>Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Identitäten für Azure-Ressourcen

Azure Blob Storage und Azure Queue Storage unterstützen die Azure Active Directory-Authentifizierung (Azure AD-Authentifizierung) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md). Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blobs und Warteschlangen mithilfe von Azure AD-Anmeldeinformationen über Anwendungen zu authentifizieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen u.a. ausgeführt werden. Mithilfe von verwalteten Identitäten für Azure-Ressourcen und der Leistungsfähigkeit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.  

Um einer verwalteten Identität Berechtigungen für einen Blobcontainer oder eine Warteschlange zu erteilen, weisen Sie der verwalteten Identität eine RBAC-Rolle (rollenbasierte Zugriffssteuerung) zu, die Berechtigungen im entsprechenden Bereich für die Ressource umfasst. Weitere Informationen zu RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md). 

In diesem Artikel wird die Authentifizierung bei Azure Blob Storage oder Azure Queue Storage mit einer verwalteten Identität über einen virtuellen Azure-Computer erläutert.  

## <a name="enable-managed-identities-on-a-vm"></a>Aktivieren von verwalteten Identitäten auf einem virtuellen Computer

Damit Sie verwaltete Identitäten für Azure-Ressourcen zum Authentifizieren des Zugriffs auf Blobs und Warteschlangen über Ihren virtuellen Computer verwenden können, müssen Sie zunächst verwaltete Identitäten für Ressourcen auf dem virtuellen Computer aktivieren. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure-Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDKs](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="assign-an-rbac-role-to-an-azure-ad-managed-identity"></a>Zuweisen einer RBAC-Rolle zu einer verwalteten Azure AD-Identität

Damit Sie eine verwaltete Identität aus Ihrer Azure Storage-Anwendung authentifizieren können, konfigurieren Sie die Einstellungen der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für diese verwaltete Identität. Azure Storage definiert RBAC-Rollen, die Berechtigungen für Container und Warteschlangen beinhalten. Wenn die RBAC-Rolle einer verwalteten Identität zugewiesen wird, erhält die verwaltete Identität Zugriff auf die Ressource. Weitere Informationen finden Sie unter [Verwalten von Zugriffsrechten für Blob- und Warteschlangendaten mit RBAC](storage-auth-aad-rbac.md).

## <a name="get-a-managed-identity-access-token"></a>Abrufen des Sicherheitstokens einer verwalteten Identität

Für die Authentifizierung mit einer verwalteten Identität muss in der Anwendung oder dem Skript das Sicherheitstoken der verwalteten Identität abgerufen werden. Informationen zum Abrufen eines Sicherheitstokens finden Sie unter [Verwenden von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer zum Abrufen eines Zugriffstokens](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

Um Blob- und Warteschlangenvorgänge mit einem OAuth-Token zu autorisieren, müssen Sie HTTPS verwenden.

## <a name="net-code-example-create-a-block-blob"></a>Codebeispiel für .NET: Erstellen eines Blockblobs

Im Codebeispiel wird davon ausgegangen, dass Sie über ein Zugriffstoken der verwalteten Identität verfügen. Das Zugriffstoken wird verwendet, um die verwaltete Identität für das Erstellen eines Blockblobs zu autorisieren.

### <a name="add-references-and-using-statements"></a>Hinzufügen von Verweisen und using-Anweisungen  

Installieren Sie in Visual Studio die Azure Storage-Clientbibliothek. Wählen Sie im Menü **Extras** den Eintrag **NuGet-Paket-Manager** und danach **Paket-Manager-Konsole** aus. Geben Sie an der Konsole folgenden Befehl ein:

```powershell
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage  
```

Fügen Sie mithilfe von Anweisungen Ihrem Code Folgendes hinzu:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-managed-identity-access-token"></a>Erstellen von Anmeldeinformationen aus dem Zugriffstoken einer verwalteten Identität

Verwenden Sie zum Erstellen des Blockblobs die **TokenCredentials**-Klasse. Erstellen Sie eine neue Instanz von **TokenCredentials**, und übergeben Sie das zuvor abgerufene Zugriffstoken der verwalteten Identität:

```dotnet
// Create storage credentials from your managed identity access token.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Für die Azure AD-Integration mit Azure Storage müssen Sie HTTPS für Azure Storage-Vorgänge verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Informationen zum Anmelden bei der Azure-Befehlszeilenschnittstelle (CLI) und bei PowerShell mit einer Azure AD-Identität finden Sie unter [Verwenden einer Azure AD-Identität für den Zugriff auf Azure Storage mit der Befehlszeilenschnittstelle oder PowerShell](storage-auth-aad-script.md).
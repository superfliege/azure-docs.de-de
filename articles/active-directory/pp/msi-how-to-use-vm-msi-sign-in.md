---
title: "Verwenden der verwalteten Dienstidentität eines virtuellen Azure-Computers für die Anmeldung"
description: "Schrittanweisungen und Beispiele zur Verwendung des MSI-Dienstprinzipals eines virtuellen Azure-Computers für die Anmeldung und den Zugriff auf Ressourcen eines Skriptclients."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/05/2018
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c5c1be01947dba8b7f4ef8aa54aa6aedfb191d32
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="sign-in-using-a-vm-user-assigned-managed-service-identity-msi"></a>Anmelden mit einer verwalteten Dienstidentität, die einem VM-Benutzer zugewiesen ist

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]
Dieser Artikel enthält CLI-Skriptbeispiele für die Anmeldung mit einem MSI-Dienstprinzipal, der einem Benutzer zugewiesen ist, sowie eine Anleitung zu wichtigen Themen wie der Fehlerbehandlung.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Wenn Sie die Azure CLI-Beispiele in diesem Artikel verwenden möchten, müssen Sie die neueste Version von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) installieren. 

> [!IMPORTANT]
> - Bei allen Beispielskripts in diesem Artikel wird vorausgesetzt, dass der Befehlszeilenclient auf einem MSI-fähigen virtuellen Computer ausgeführt wird. Verwenden Sie die Funktion „Verbinden“ im Azure-Portal zum Herstellen einer Remoteverbindung mit Ihrem virtuellen Computer. Weitere Informationen zur Aktivierung von MSI auf einer VM finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure-Befehlszeilenschnittstelle](msi-qs-configure-cli-windows-vm.md) oder in einem der verwandten Artikel (PowerShell, Portal, Vorlage oder Azure SDK). 
> - Um Fehler beim Anmelden und beim Zugriff auf Ressourcen zu verhindern, muss die MSI mindestens über den Zugriff „Leser“ im entsprechenden Bereich (virtueller Computer oder höher) verfügen, um Azure Resource Manager-Vorgänge auf dem virtuellen Computer zu ermöglichen. Weitere Informationen finden Sie unter [Zuweisen des MSI-Zugriffs (Managed Service Identity, verwaltete Dienstidentität) auf eine Ressource mit der Azure-Befehlszeilenschnittstelle](msi-howto-assign-access-cli.md).

## <a name="overview"></a>Übersicht

Eine MSI enthält einen [Dienstprinzipal](~/articles/active-directory/develop/active-directory-dev-glossary.md#service-principal-object), der [beim Aktivieren von MSI](msi-overview.md#how-does-it-work) auf dem virtuellen Computer erstellt wird. Der Dienstprinzipal kann Zugriff auf Azure-Ressourcen erhalten und von Skript- oder Befehlszeilenclients als Identität für die Anmeldung und den Zugriff auf Ressourcen verwendet werden. Bislang war für einen Skriptclient für den Zugriff auf geschützte Ressourcen unter seiner eigenen Identität Folgendes erforderlich:  

   - Registrierung und Zustimmung als vertrauliche und Webclientanwendung in Azure AD
   - Anmeldung unter dem zugehörigen Dienstprinzipal mit den Anmeldeinformationen der Anwendung (wahrscheinlich im Skript eingebettet)

Mit MSI ist für den Skriptclient beides nicht mehr erforderlich, da er unter dem MSI-Dienstprinzipal angemeldet werden kann. 

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Mit diesem Skript wird Folgendes veranschaulicht:

1. Anmelden bei Azure AD unter dem MSI-Dienstprinzipal des Benutzers  
2. Aufrufen von Azure Resource Manager und Abrufen der Azure-Region für einen virtuellen Computer Die Verwaltung des Tokenabrufs und der Tokenverwendung erfolgt automatisch über die Befehlszeilenschnittstelle. Ersetzen Sie unbedingt `<VM NAME>` durch Ihren VM-Namen und `<MSI ID>` durch die MSI-Ressourcen-ID des Benutzers. Die MSI-Ressourcen-ID wird während der Erstellung einer einem Benutzer zugewiesenen MSI in der `id`-Eigenschaft zurückgegeben (unter [Konfigurieren einer einem Benutzer zugewiesenen verwalteten Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Computer mit der Azure-Befehlszeilenschnittstelle](msi-qs-configure-cli-windows-vm.md) finden Sie Beispiele für den Befehl `az identity create`).

    ```azurecli
    az login --msi –u <MSI ID>
   
    vmLocation=$(az resource list -n <VM NAME> --query [*].location --out tsv)
    echo The VM region location is $vmLocation
    ```

    Beispielantworten:
   
    ```bash
    user@vmLinux:~$ az login --msi -u /subscriptions/80c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName
    [
      {
        "environmentName": "AzureCloud",
        "id": "90c696ff-5efa-4909-a64d-z1b616f423bl",
        "isDefault": true,
        "name": "MSIResource-/subscriptions/90c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName@50342",
        "state": "Enabled",
        "tenantId": "933a8f0e-ec41-4e69-8ad8-971zc4b533ll",
        "user": {
          "name": "userAssignedIdentity",
          "type": "servicePrincipal"
        }
      }
    ]  
    user@vmLinux:~$ vmLocation=$(az resource list -n vmLinux --query [*].location --out tsv)
    user@vmLinux:~$ echo The VM region location is $vmLocation
    The VM region location is westcentralus
    ```

## <a name="resource-ids-for-azure-services"></a>Ressourcen-IDs für Azure-Dienste

Eine Liste mit Ressourcen, die Azure AD unterstützen und mit MSI getestet wurden, und die jeweiligen zugehörigen Ressourcen-IDs finden Sie unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](msi-overview.md#azure-services-that-support-azure-ad-authentication).

## <a name="error-handling-guidance"></a>Anleitung zur Fehlerbehandlung 

Die folgenden Antworten können darauf hinweisen, dass die MSI nicht ordnungsgemäß konfiguriert wurde:

- CLI: *MSI: Failed to retrieve a token from 'http://localhost:50342/oauth2/token' with an error of 'HTTPConnectionPool(host='localhost', port=50342)* [MSI: Fehler beim Abrufen eines Tokens aus 'http://localhost:50342/oauth2/token' mit dem Fehler 'HTTPConnectionPool(host='localhost', port=50342)] 

Wenn einer dieser Fehler angezeigt wird, wechseln Sie zu dem virtuellen Azure-Computer im [Azure-Portal](https://portal.azure.com) und:

- Stellen Sie auf der Seite **Konfiguration** sicher, dass „Verwaltete Dienstidentität“ auf „Ja“ festgelegt ist.
- Vergewissern Sie sich auf der Seite **Erweiterungen**, dass die MSI-Erweiterung erfolgreich bereitgestellt wurde.

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut der Ressource zuweisen oder den Fehler bei der Bereitstellung beheben. Hilfe zur Konfiguration des virtuellen Computers finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure-Befehlszeilenschnittstelle](msi-qs-configure-cli-windows-vm.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Aktivieren der MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure-Befehlszeilenschnittstelle](msi-qs-configure-cli-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.









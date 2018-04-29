---
title: Verknüpfen eines Azure-Kontos mit der Partner-ID | Microsoft-Dokumentation
description: Verfolgen Sie Bindungen mit Azure-Kunden nach, indem Sie die Partner-ID mit dem Benutzerkonto verknüpfen, mit dem Sie die Ressourcen des Kunden verwalten.
services: billing
author: dhirajgandhi
ms.author: dhgandhi
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: abab0e63f91ad34d2671c37773d47c31eeeb8339
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="link-partner-id-to-your-azure-accounts"></a>Verknüpfen einer Partner-ID mit Ihren Azure-Konten 
Als Partner können Sie die Auswirkungen Ihrer Kundenbindungen durch Verknüpfen Ihrer Partner-ID mit den Konten nachverfolgen, mit denen Sie die Ressourcen Ihrer Kunden verwalten.

Dieses Feature ist als Public Preview verfügbar. 

## <a name="get-access-from-your-customer"></a>Erlangen des Zugriffs seitens Ihres Kunden 
Bevor Sie Ihre Partner-ID verknüpfen, muss Ihr Kunde Ihnen über eine der folgenden Optionen Zugriff auf ihre Azure-Ressourcen erteilen:

- **Gastbenutzer**: Ihr Kunde kann Sie als Gastbenutzer hinzufügen und Ihnen eine beliebige RBAC-Rolle zuweisen. Weitere Informationen finden Sie unter [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Directory-Konto**: Ihr Kunde kann einen neuen Benutzer von Ihrer Organisation in ihrem Verzeichnis erstellen und diesem eine beliebige RBAC-Rolle zuweisen. 

- **Dienstprinzipal**: Ihr Kunde kann eine App oder ein Skript von Ihrer Organisation in ihrem Verzeichnis erstellen und dieser/m eine beliebige RBAC-Rolle zuweisen. Die Identität der App oder des Skripts wird als „Dienstprinzipal“ bezeichnet.

## <a name="link-partner-id"></a>Verknüpfen der Partner-ID
Wenn Sie Zugriff auf die Ressourcen des Kunden haben, verknüpfen Sie Ihre Microsoft Partner Network-ID (MPN-ID) mithilfe von PowerShell oder der CLI mit Ihrer Benutzer-ID oder Ihrem Dienstprinzipal. Sie müssen die Partner-ID für jeden Kundenmandanten verknüpfen. 

### <a name="use-powershell-to-link-new-partner-id"></a>Verknüpfen einer neuen Partner-ID mit PowerShell

1. Installieren Sie das [AzurePartnerRP](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner/0.1.0-preview) PowerShell-Modul.

2. Melden Sie sich mit dem Benutzerkonto oder dem Dienstprinzipal beim Mandanten des Kunden an. Weitere Informationen finden Sie unter [Anmelden mit PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Verknüpfen Sie die neue Partner-ID. Die Partner-ID ist die [MPN-ID (Microsoft Partner Network)](https://partner.microsoft.com/) Ihrer Organisation.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>Abrufen der verknüpften Partner-ID
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Aktualisieren der verknüpften Partner-ID
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Löschen der verknüpften Partner-ID
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>Verknüpfen einer neuen Partner-ID mit der CLI
1.  Installieren Sie die CLI-Erweiterung.

    ```azure-cli
    C:\ az extension add --name managementpartner
    ``` 

2.  Melden Sie sich mit dem Benutzerkonto oder Dienstprinzipal beim Mandanten des Kunden an. Weitere Informationen finden Sie unter [Erste Schritte mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azure-cli
    C:\ az login --tenant <tenant>
    ``` 


3.  Verknüpfen Sie die neue Partner-ID. Die Partner-ID ist die [MPN-ID (Microsoft Partner Network)](https://partner.microsoft.com/) Ihrer Organisation.

     ```azure-cli
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Abrufen der verknüpften Partner-ID
```azure-cli
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Aktualisieren der verknüpften Partner-ID
```azure-cli
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Löschen der verknüpften Partner-ID
```azure-cli
C:\ az managementpartner delete --partner-id 12345
``` 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wer kann die Partner-ID verknüpfen?**

Jeder Benutzer der Partnerorganisation, der eine Ressource des Kunden verwaltet, kann die Partner-ID mit dem Konto verknüpfen.
  

**Kann eine Partner-ID geändert werden, nachdem diese verknüpft wurde?**

Ja, verknüpfte Partner-IDs können geändert, hinzugefügt oder entfernt werden.

**Was geschieht, wenn ein Benutzer ein Konto bei mehreren Kundenmandanten besitzt?**

Die Verknüpfung zwischen der Partner-ID und dem Konto erfolgt für jeden Kundenmandanten.  Sie müssen die Partner-ID bei jedem Kundenmandanten verknüpfen.

**Können andere Partner oder Kunden die Verknüpfung mit der Partner-ID bearbeiten oder entfernen?**

Die Verknüpfung wird auf Kontoebene erstellt. Nur Sie können die Verknüpfung mit der Partner-ID bearbeiten oder entfernen. Weder der Kunde noch andere Partner können die Verknüpfung mit der Partner-ID ändern. 



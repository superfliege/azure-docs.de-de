---
title: Verknüpfen eines Azure-Kontos mit der Partner-ID | Microsoft-Dokumentation
description: Verfolgen Sie Bindungen mit Azure-Kunden nach, indem Sie die Partner-ID mit dem Benutzerkonto verknüpfen, mit dem Sie die Ressourcen des Kunden verwalten.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: cwatson
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: a0dad228f23b055d68009eb737e0347ade49e94b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968047"
---
# <a name="link-partner-id-to-your-azure-accounts"></a>Verknüpfen einer Partner-ID mit Ihren Azure-Konten

Als Partner können Sie die Auswirkungen Ihrer Kundenbindungen durch Verknüpfen Ihrer Partner-ID mit den Konten nachverfolgen, mit denen Sie die Ressourcen Ihrer Kunden verwalten.

Dieses Feature ist als Public Preview verfügbar.

## <a name="get-access-from-your-customer"></a>Erlangen des Zugriffs seitens Ihres Kunden

Bevor Sie Ihre Partner-ID verknüpfen, muss Ihr Kunde Ihnen über eine der folgenden Optionen Zugriff auf ihre Azure-Ressourcen erteilen:

- **Gastbenutzer:** Ihr Kunde kann Sie als Gastbenutzer hinzufügen und Ihnen eine beliebige RBAC-Rolle zuweisen. Weitere Informationen finden Sie unter [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Verzeichniskonto:**  Ihr Kunde kann ein Benutzerkonto für die Sie in seinem eigenen Verzeichnis erstellen und dem Konto eine beliebige RBAC-Rolle zuweisen.

- **Dienstprinzipal:**  Ihr Kunde kann eine App oder ein Skript von Ihrer Organisation in seinem Verzeichnis hinzufügen und dieser oder diesem eine beliebige RBAC-Rolle zuweisen. Die Identität der App oder des Skripts wird als „Dienstprinzipal“ bezeichnet.

## <a name="link-partner-id"></a>Verknüpfen der Partner-ID

Wenn Sie Zugriff auf die Ressourcen des Kunden haben, verknüpfen Sie Ihre Microsoft Partner Network-ID (MPN-ID) über das Azure-Portal, über PowerShell oder über die CLI mit Ihrer Benutzer-ID oder Ihrem Dienstprinzipal. Sie müssen die Partner-ID bei jedem Kundenmandanten verknüpfen.

### <a name="use-azure-portal-to-link-new-partner-id"></a>Verknüpfen einer neuen Partner-ID im Azure-Portal

1. Wechseln Sie im Azure-Portal zu [Mit einer Partner-ID verknüpfen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade).

2. Melden Sie sich beim Azure-Portal an.

3. Geben Sie die Microsoft-Partner-ID ein. Die Partner-ID ist die MPN-ID ([Microsoft Partner Network](https://partner.microsoft.com/)) Ihrer Organisation.

  ![Screenshot, der Verknüpfen einer Partner-ID zeigt](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Wenn Sie die Partner-ID für einen anderen Kunden verknüpfen möchten, verwenden Sie den Verzeichniswechsler. Wählen Sie unter „Verzeichnis wechseln“ das gewünschte Verzeichnis aus.

  ![Screenshot, der Verknüpfen einer Partner-ID zeigt](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-new-partner-id"></a>Verknüpfen einer neuen Partner-ID mit PowerShell

1. Installieren Sie das PowerShell-Modul [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner).

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

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2.  Melden Sie sich mit dem Benutzerkonto oder Dienstprinzipal beim Mandanten des Kunden an. Weitere Informationen finden Sie unter [Anmelden mit der Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3.  Verknüpfen Sie die neue Partner-ID. Die Partner-ID ist die [MPN-ID (Microsoft Partner Network)](https://partner.microsoft.com/) Ihrer Organisation.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Abrufen der verknüpften Partner-ID
```azurecli-interactive
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Aktualisieren der verknüpften Partner-ID
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Löschen der verknüpften Partner-ID
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
``` 

## <a name="next-steps"></a>Nächste Schritte

Schließen Sie sich der Diskussion in der [Microsoft-Partner-Community](https://aka.ms/PALdiscussion) an, um Aktualisierungen zu erhalten oder Feedback zu senden.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wer kann die Partner-ID verknüpfen?**

Jeder Benutzer der Partnerorganisation, der Azure-Ressourcen des Kunden verwaltet, kann die Partner-ID mit dem Konto verknüpfen.

**Kann eine Partner-ID geändert werden, nachdem diese verknüpft wurde?**

Ja, verknüpfte Partner-IDs können geändert, hinzugefügt oder entfernt werden.

**Was geschieht, wenn ein Benutzer ein Konto bei mehreren Kundenmandanten besitzt?**

Die Verknüpfung zwischen der Partner-ID und dem Konto erfolgt für jeden Kundenmandanten.  Sie müssen die Partner-ID bei jedem Kundenmandanten verknüpfen.

**Können andere Partner oder Kunden die Verknüpfung mit der Partner-ID bearbeiten oder entfernen?**

Die Verknüpfung wird auf Benutzerkontoebene zugeordnet. Nur Sie können die Verknüpfung mit der Partner-ID bearbeiten oder entfernen. Weder der Kunde noch andere Partner können die Verknüpfung mit der Partner-ID ändern. 

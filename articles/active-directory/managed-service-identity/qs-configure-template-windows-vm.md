---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer mithilfe einer Vorlage
description: Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mithilfe einer Azure Resource Manager-Vorlage.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 05859187a5734d982b750e287c3ecd375ed1da2f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723744"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Konfigurieren einer verwalteten VM-Dienstidentität mithilfe einer Vorlage

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure Resource Manager-Bereitstellungsvorlage die folgenden Vorgänge für verwaltete Dienstidentitäten (Managed Service Identity, MSI) auf einer Azure-VM ausführen können:

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.

## <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

Wie beim Azure-Portal und der Skripterstellung bieten [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portalbasiert verfügbar, einschließlich:

   - Verwenden einer [benutzerdefinierten Vorlage vom Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage, die auf einer vorhandenen, oft verwendeten Vorlage oder [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
   - Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) oder aus dem [aktuellen Status der Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group) exportiert wird.
   - Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md) und anschließendes Hochladen und Bereitstellen mithilfe von PowerShell oder CLI.
   - Verwenden des [Azure-Ressourcengruppenprojekts](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) von Visual Studio für das Erstellen und Bereitstellen einer Vorlage.  

Unabhängig von der gewählten Option ist die Vorlagensyntax während der ursprünglichen und erneuten Bereitstellung identisch. Die Aktivierung einer vom System oder einer vom Benutzer zugewiesenen Identität auf einer neuen oder vorhandenen VM erfolgt auf dieselbe Weise. Standardmäßig führt der Azure Resource Manager außerdem ein [inkrementelles Update](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) für Bereitstellungen durch.

## <a name="system-assigned-identity"></a>Vom System zugewiesene Identität

In diesem Abschnitt aktivieren und deaktivieren Sie eine vom System zugewiesene Identität mithilfe einer Azure Resource Manager-Vorlage.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Aktivieren einer vom System zugewiesenen Identität beim Erstellen einer Azure-VM oder vorhandenen VM

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, durch die Sie Schreibberechtigungen auf dem virtuellen Computer erhalten (z.B. die Rolle „Mitwirkender für virtuelle Computer“).

2. Nachdem die Vorlage in einen Editor geladen wurde, suchen Sie die gewünschte `Microsoft.Compute/virtualMachines`-Ressource im Abschnitt `resources`. Dieser Screenshot weicht möglicherweise geringfügig von der Darstellung bei Ihnen ab. Dies hängt davon ab, welchen Editor Sie verwenden und ob Sie eine Vorlage für eine neue oder eine vorhandene Bereitstellung bearbeiten.

   >[!NOTE] 
   > Dieses Beispiel setzt voraus, dass Variablen wie z. B. `vmName`, `storageAccountName`, und `nicName` in der Vorlage definiert wurden.
   >

   ![Screenshot der Vorlage – VM suchen](../media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Um eine vom System zugewiesene Identität zu aktivieren, fügen Sie die Eigenschaft `"identity"` auf der gleichen Ebene wie die Eigenschaft `"type": "Microsoft.Compute/virtualMachines"` hinzu. Verwenden Sie die folgende Syntax:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Optional) Fügen Sie die VM-MSI-Erweiterung als `resources`-Element hinzu. Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können.  Verwenden Sie die folgende Syntax:

   >[!NOTE] 
   > Im folgenden Beispiel wird angenommen, dass eine Windows-VM-Erweiterung (`ManagedIdentityExtensionForWindows`) bereitgestellt wird. Sie können die Konfiguration auch für Linux ausführen, indem Sie stattdessen `ManagedIdentityExtensionForLinux` für die Elemente `"name"` und `"type"` verwenden.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

5. Wenn Sie fertig sind, sollte Ihre Vorlage wie folgt aussehen:

   ![Screenshot der Vorlage nach dem Update](../media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Deaktivieren einer vom System zugewiesenen Identität von einer Azure-VM

> [!NOTE]
> Die Deaktivierung einer verwalteten Dienstidentität über einen virtuellen Computer wird derzeit nicht unterstützt. In der Zwischenzeit können Sie zwischen vom System zugewiesenen und vom Benutzer zugewiesenen Identitäten wechseln.

Wenn eine verwaltete Dienstidentität auf einer VM nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, durch die Sie Schreibberechtigungen auf dem virtuellen Computer erhalten (z.B. die Rolle „Mitwirkender für virtuelle Computer“).

2. Ändern Sie den Identitätstyp in `UserAssigned`.

## <a name="user-assigned-identity"></a>Vom Benutzer zugewiesene Identität

In diesem Abschnitt weisen Sie einer Azure-VM mit der Azure Resource Manager-Vorlage eine vom Benutzer zugewiesene Identität zu.

> [!Note]
> Um mithilfe einer Azure Resource Manager-Vorlage eine vom Benutzer zugewiesene Identität zu erstellen, lesen Sie [Erstellen einer vom Benutzer zugewiesenen Identität](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Zuweisen einer vom Benutzer zugewiesenen Identität zu einer Azure-VM

1. Fügen Sie unter dem `resources`-Element den folgenden Eintrag hinzu, um Ihrer VM eine vom Benutzer zugewiesene Identität zuzuweisen.  Achten Sie darauf, dass Sie `<USERASSIGNEDIDENTITY>` durch den Namen der vom Benutzer zugewiesenen Identität ersetzen, die Sie erstellt haben.
    ```json
    {
        "apiVersion": "2017-12-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
            ]
        },
    ```
    
2. (Optional) Fügen Sie als Nächstes unter dem `resources`-Element den folgenden Eintrag hinzu, um Ihrer VM die Erweiterung der verwalteten Identität zuzuweisen. Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können. Verwenden Sie die folgende Syntax:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2015-05-01-preview",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
3.  Wenn Sie fertig sind, sollte Ihre Vorlage wie folgt aussehen:

      ![Screenshot einer benutzerzugewiesenen Identität](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)


## <a name="related-content"></a>Verwandte Inhalte

- Ausführliche Informationen zu MSI finden Sie unter [Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen](overview.md).


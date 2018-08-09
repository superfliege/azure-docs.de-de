---
title: Konfigurieren einer verwalteten Dienstidentität für eine Azure-VM-Skalierungsgruppe über eine Vorlage
description: Schritt-für-Schritt-Anweisungen zum Konfigurieren einer verwalteten Dienstidentität in einer Azure-VMSS mithilfe einer Azure Resource Manager-Vorlage.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: bee75bcefb370382825c6867ea504e14102aa107
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628282"
---
# <a name="configure-managed-service-identity-on-virtual-machine-scale-using-a-template"></a>Konfigurieren einer verwalteten Dienstidentität für eine VM-Skalierungsgruppe über eine Vorlage

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure Resource Manager-Bereitstellungsvorlage die folgenden Vorgänge für verwaltete Dienstidentitäten für eine Azure-VM-Skalierungsgruppe ausführen können:
- Aktivieren und Deaktivieren der systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität in einer Azure VM-Skalierungsgruppe

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM-Skalierungsgruppe zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität auf einer VM-Skalierungsgruppe zu aktivieren bzw. daraus zu entfernen.
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene Identität zu erstellen.
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene Identität einer VM-Skalierungsgruppe zuzuweisen bzw. daraus zu entfernen.

## <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

Wie beim Azure-Portal und der Skripterstellung bieten [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portalbasiert verfügbar, einschließlich:

   - Verwenden einer [benutzerdefinierten Vorlage vom Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage, die auf einer vorhandenen, oft verwendeten Vorlage oder [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
   - Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) oder aus dem [aktuellen Status der Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group) exportiert wird.
   - Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md) und anschließendes Hochladen und Bereitstellen mithilfe von PowerShell oder CLI.
   - Verwenden des [Azure-Ressourcengruppenprojekts](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) von Visual Studio für das Erstellen und Bereitstellen einer Vorlage.  

Unabhängig von der gewählten Option ist die Vorlagensyntax während der ursprünglichen und erneuten Bereitstellung identisch. Die Aktivierung einer verwalteten Dienstidentität auf einer neuen oder vorhandenen VM erfolgt auf dieselbe Weise. Standardmäßig führt Azure Resource Manager außerdem ein [inkrementelles Update](../../azure-resource-manager/deployment-modes.md) für Bereitstellungen durch.

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität

In diesem Abschnitt aktivieren und deaktivieren Sie die systemzugewiesene Identität mithilfe einer Azure Resource Manager-Vorlage.

### <a name="enable-system-assigned-identity-during-creation-the-creation-of-or-an-existing-azure-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität bei der Erstellung einer Azure-VM-Skalierungsgruppe oder auf einer vorhandenen Azure-VM-Skalierungsgruppe

1. Laden Sie die Vorlage in einen Editor, und suchen Sie nach der gewünschten `Microsoft.Compute/virtualMachineScaleSets`-Ressource im Abschnitt `resources`. Dieser Screenshot weicht möglicherweise geringfügig von der Darstellung bei Ihnen ab. Dies hängt davon ab, welchen Editor Sie verwenden und ob Sie eine Vorlage für eine neue oder eine vorhandene Bereitstellung bearbeiten.
   
   ![Screenshot der Vorlage – VM suchen](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Um die systemzugewiesene Identität zu aktivieren, fügen Sie die Eigenschaft `"identity"` auf derselben Ebene wie die Eigenschaft `"type": "Microsoft.Compute/virtualMachineScaleSets"` hinzu. Verwenden Sie die folgende Syntax:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (Optinal) Fügen Sie dann die Erweiterung der verwalteten Dienstidentität für die VM-Skalierungsgruppe als `extensionsProfile`-Element hinzu. Dieser Schritt ist optional, da Sie für den Tokenabruf auch die Azure IMDS-Identität (Instance Metadata Service) verwenden können.  Verwenden Sie die folgende Syntax:

   >[!NOTE] 
   > Im folgenden Beispiel wird angenommen, dass eine Windows-VM-Skalierungsgruppenerweiterung (`ManagedIdentityExtensionForWindows`) bereitgestellt wird. Sie können die Konfiguration auch für Linux ausführen, indem Sie stattdessen `ManagedIdentityExtensionForLinux` für die Elemente `"name"` und `"type"` verwenden.
   >

   ```JSON
   "extensionProfile": {
        "extensions": [
            {
                "name": "MSIWindowsExtension",
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

4. Wenn Sie fertig sind, sollte Ihre Vorlage wie folgt aussehen:

   ![Screenshot der Vorlage nach dem Update](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Deaktivieren einer systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe

Wenn die verwaltete Dienstidentität in einer VM-Skalierungsgruppe nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

2. Laden Sie die Vorlage in einen [Editor](#azure-resource-manager-templates), und suchen Sie nach der gewünschten `Microsoft.Compute/virtualMachineScaleSets`-Ressource im Abschnitt `resources`. Wenn Ihre VM-Skalierungsgruppe nur über eine vom System zugewiesene Identität verfügt, können Sie diese deaktivieren, indem Sie den Identitätstyp in `None` ändern.  Wenn Ihre VM-Skalierungsgruppe sowohl vom System als auch vom Benutzer zugewiesene Identitäten enthält, entfernen Sie `SystemAssigned` aus dem Identitätstyp, und behalten Sie `UserAssigned` zusammen mit dem Array `identityIds` der vom Benutzer zugewiesenen Identitäten bei.  Das folgende Beispiel zeigt Ihnen, wie Sie eine vom System zugewiesene Identität aus einer VM-Skalierungsgruppe ohne vom Benutzer zugewiesene Identitäten entfernen:
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt weisen Sie einer Azure VM-Skalierungsgruppe mit der Azure Resource Manager-Vorlage eine benutzerzugewiesene Identität zu.

> [!Note]
> Um mithilfe einer Azure Resource Manager-Vorlage eine benutzerzugewiesene Identität zu erstellen, lesen Sie [Erstellen einer benutzerzugewiesenen Identität](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Zuweisen einer benutzerzugewiesenen Identität zu einer Azure VM-Skalierungsgruppe

1. Fügen Sie unter dem `resources`-Element den folgenden Eintrag hinzu, um Ihrer VM-Skalierungsgruppe eine benutzerzugewiesene Identität zuzuweisen.  Achten Sie darauf, dass Sie `<USERASSIGNEDIDENTITY>` durch den Namen der benutzerzugewiesenen Identität ersetzen, die Sie erstellt haben.

   > [!Important]
   > Der Wert `<USERASSIGNEDIDENTITYNAME>`, der in folgendem Beispiel dargestellt wird, muss in einer Variable gespeichert werden.  Für die derzeit unterstützte Implementierung der Zuweisung von Identitäten zu virtuellen Computern durch Benutzer mithilfe von Resource Manager-Vorlagen muss die API-Version mit der im folgenden Beispiel übereinstimmen. 

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
            ]
        }

    }
    ```

2. Fügen Sie den folgenden Eintrag unter dem `extensionProfile`-Element hinzu, um Ihrer VM-Skalierungsgruppe die MSI-Erweiterung zuzuweisen. Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können. Verwenden Sie die folgende Syntax:
   
    ```JSON
       "extensionProfile": {
            "extensions": [
                {
                    "name": "MSIWindowsExtension",
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

3.  Wenn Sie fertig sind, sollte Ihre Vorlage wie folgt aussehen:
   
      ![Screenshot einer benutzerzugewiesenen Identität](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

### <a name="remove-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Entfernen einer vom Benutzer zugewiesenen Identität aus einer Azure-VM-Skalierungsgruppe

Wenn die verwaltete Dienstidentität in einer VM-Skalierungsgruppe nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

2. Laden Sie die Vorlage in einen [Editor](#azure-resource-manager-templates), und suchen Sie nach der gewünschten `Microsoft.Compute/virtualMachineScaleSets`-Ressource im Abschnitt `resources`. Wenn Ihre VM-Skalierungsgruppe nur über eine vom Benutzer zugewiesene Identität verfügt, können Sie diese deaktivieren, indem Sie den Identitätstyp in `None` ändern.  Wenn Ihre VM-Skalierungsgruppe sowohl vom System als auch vom Benutzer zugewiesene Identitäten enthält, und Sie die vom System zugewiesene Identität behalten möchten, entfernen Sie `UserAssigned` zusammen mit dem `identityIds`-Array der vom Benutzer zugewiesenen Identitäten aus dem Identitätstyp.
    
   Um eine einzelne vom Benutzer zugewiesene Identität aus einer VM-Skalierungsgruppe zu entfernen, entfernen Sie sie aus dem `identityIds`-Array.
   
   Das folgende Beispiel zeigt Ihnen, wie Sie alle vom Benutzer zugewiesenen Identitäten aus einer VM-Skalierungsgruppe ohne vom System zugewiesene Identitäten entfernen:
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zur verwalteten Dienstidentität finden Sie unter [Was ist die verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen?](overview.md).


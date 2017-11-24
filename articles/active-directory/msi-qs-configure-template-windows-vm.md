---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer mithilfe einer Vorlage
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mithilfe einer Azure Resource Manager-Vorlage."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: 014a9c814e91e7d0677b7ea1d994c81384719548
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Konfigurieren einer verwalteten VM-Dienstidentität mithilfe einer Vorlage

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität (Managed Service Identity, MSI) stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Computer mithilfe einer Azure Resource Manager-Bereitstellungsvorlage aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers oder auf einem vorhandenen virtuellen Computer

Wie beim Azure-Portal und der Skripterstellung bieten Azure Resource Manager-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portalbasiert verfügbar, einschließlich:

   - Verwenden einer [benutzerdefinierten Vorlage vom Azure Marketplace](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage, die auf einer vorhandenen, oft verwendeten Vorlage oder [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
   - Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) oder aus dem [aktuellen Status der Bereitstellung](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group) exportiert wird.
   - Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../azure-resource-manager/resource-manager-create-first-template.md) und anschließendes Hochladen und Bereitstellen mithilfe von PowerShell oder CLI.
   - Verwenden des [Azure-Ressourcengruppenprojekts](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) von Visual Studio für das Erstellen und Bereitstellen einer Vorlage.  

Unabhängig von der gewählten Option ist die Vorlagensyntax während der ursprünglichen und erneuten Bereitstellung identisch. Die MSI-Aktivierung erfolgt auf einer neuen oder vorhandenen VM auf dieselbe Weise. Standardmäßig führt Azure Resource Manager außerdem ein [inkrementelles Update](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) für Bereitstellungen durch:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, durch die Sie Schreibberechtigungen auf dem virtuellen Computer erhalten (z.B. die Rolle „Mitwirkender für virtuelle Computer“).

2. Nachdem die Vorlage in einen Editor geladen wurde, suchen Sie die gewünschte `Microsoft.Compute/virtualMachines`-Ressource im Abschnitt `resources`. Dieser Screenshot weicht möglicherweise geringfügig von der Darstellung bei Ihnen ab. Dies hängt davon ab, welchen Editor Sie verwenden und ob Sie eine Vorlage für eine neue oder eine vorhandene Bereitstellung bearbeiten.

   >[!NOTE] 
   > Dieses Beispiel setzt voraus, dass Variablen wie z. B. `vmName`, `storageAccountName`, und `nicName` in der Vorlage definiert wurden.
   >

   ![Screenshot der Vorlage – VM suchen](./media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Fügen Sie die `"identity"`-Eigenschaft auf derselben Ebene wie die `"type": "Microsoft.Compute/virtualMachines"`-Eigenschaft hinzu. Verwenden Sie die folgende Syntax:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Fügen Sie dann die VM-MSI-Erweiterung als `resources`-Element hinzu. Verwenden Sie die folgende Syntax:

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

   ![Screenshot der Vorlage nach dem Update](./media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält. Stellen Sie außerdem sicher, dass Ihr Konto zu einer Rolle gehört, durch die Sie Schreibberechtigungen auf dem virtuellen Computer erhalten (z.B. die Rolle „Mitwirkender für virtuelle Computer“).

2. Entfernen Sie die zwei Elemente, die im vorherigen Abschnitt hinzugefügt wurden: die Eigenschaft `"identity"` des virtuellen Computers und die `"Microsoft.Compute/virtualMachines/extensions"`-Ressource.

## <a name="related-content"></a>Verwandte Inhalte

- Ausführliche Informationen zu MSI finden Sie unter [Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen](msi-overview.md).


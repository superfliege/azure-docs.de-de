---
title: Erstellen und Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe des Azure Resource Manager
description: Ausführliche Anweisungen zum Erstellen und Löschen von vom Benutzer zugewiesenen verwalteten Identitäten mithilfe des Azure Resource Manager
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
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: adfa7ebfd911bfcbc88e01030777e91c48841784
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338344"
---
# <a name="create-list-and-delete-a-user-assigned-managed-identity-using-azure-resource-manager"></a>Erstellen, Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe des Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erstellen Sie eine vom Benutzer zugewiesene verwaltete Identität mithilfe des Azure Resource Manager.

Das Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe einer Azure Resource Manager-Vorlage ist nicht möglich.  Informationen zum Erstellen und Auflisten einer vom Benutzer zugewiesenen verwalteten Identität finden Sie in den folgenden Artikeln:

- [Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Löschen einer vom Benutzer zugewiesenen verwalteten Identität](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)
## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Vorgänge in diesem Artikel durchzuführen, benötigt Ihr Konto die folgende Rollenzuweisung:
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, zu lesen (aufzulisten), zu aktualisieren und zu löschen

## <a name="template-creation-and-editing"></a>Erstellung und Bearbeitung von Vorlagen

Wie beim Azure-Portal und der Skripterstellung bieten Azure Resource Manager-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portalbasiert verfügbar, einschließlich:

- Verwenden einer [benutzerdefinierten Vorlage vom Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage, die auf einer vorhandenen, oft verwendeten Vorlage oder [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
- Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) oder aus dem [aktuellen Status der Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group) exportiert wird.
- Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md) und anschließendes Hochladen und Bereitstellen mithilfe von PowerShell oder CLI.
- Verwenden des [Azure-Ressourcengruppenprojekts](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) von Visual Studio für das Erstellen und Bereitstellen einer Vorlage. 

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer vom Benutzer zugewiesenen verwalteten Identität 

Um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, verwenden Sie die folgende Vorlage. Ersetzen Sie den Wert `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2015-08-31-PREVIEW",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe einer Vorlage](qs-configure-template-windows-vm.md).


 

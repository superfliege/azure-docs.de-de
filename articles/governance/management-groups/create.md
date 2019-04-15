---
title: Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen – Azure Governance
description: Hier erfahren Sie, wie Sie Azure-Verwaltungsgruppen zum Verwalten mehrerer Ressourcen über das Portal, mithilfe von Azure PowerShell oder mithilfe der Azure CLI erstellen.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: a89df98224634c08c84cb059eb58e64e3c7febf7
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58801265"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Erstellen von Verwaltungsgruppen zum Organisieren und Verwalten von Ressourcen

Bei Verwaltungsgruppen handelt es sich um Container, mit denen Sie Zugriff, Richtlinien und Konformität abonnementübergreifend verwalten können. Erstellen Sie diese Container, um eine effektive und effiziente Hierarchie zu erstellen, die mit [Azure Policy](../policy/overview.md) und mit der [rollenbasierten Zugriffssteuerung (Role-Based Access Controls, RBAC) von Azure](../../role-based-access-control/overview.md) verwendet werden kann. Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren von Ressourcen mit Azure-Verwaltungsgruppen](overview.md).

Es kann bis zu 15 Minuten dauern, bis die Erstellung der ersten Verwaltungsgruppe im Verzeichnis abgeschlossen ist. Bei der ersten Erstellung werden Prozesse zum Einrichten des Verwaltungsgruppendiensts in Azure für Ihr Verzeichnis ausgeführt. Sie erhalten eine Benachrichtigung, wenn der Vorgang abgeschlossen ist.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="create-a-management-group"></a>Erstellen einer Verwaltungsgruppe

Die Verwaltungsgruppe kann über das Portal, mithilfe von PowerShell oder mithilfe der Azure CLI erstellt werden. Derzeit können Sie keine Resource Manager-Vorlagen zum Erstellen von Verwaltungsgruppen verwenden.

### <a name="create-in-portal"></a>Erstellen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Klicken Sie auf der Hauptseite auf **Neue Verwaltungsgruppe**.

   ![Seite für die Arbeit mit Verwaltungsgruppen](./media/main.png)

1. Füllen Sie das Feld für die Verwaltungsgruppen-ID aus.

   - Die **ID der Verwaltungsgruppe** ist der eindeutige Bezeichner des Verzeichnisses, der zum Übermitteln von Befehlen für diese Verwaltungsgruppe verwendet wird. Dieser Bezeichner kann nach der Erstellung nicht bearbeitet werden, da er im gesamten Azure-System zum Identifizieren dieser Gruppe verwendet wird.
   - Im Feld für den Anzeigenamen wird der Name angegeben, der im Azure-Portal angezeigt wird. Beim Erstellen der Verwaltungsgruppe gibt es ein optionales Feld für einen separaten Anzeigenamen, der jederzeit geändert werden kann.  

   ![Bereich „Optionen“ zum Erstellen einer neuen Verwaltungsgruppe](./media/create_context_menu.png)  

1. Wählen Sie **Speichern** aus.

### <a name="create-in-powershell"></a>Erstellen in PowerShell

In PowerShell verwenden Sie das Cmdlet „New-AzManagementGroup“:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** ist ein eindeutiger Bezeichner, der erstellt wird. Diese ID wird von anderen Befehlen zum Verweisen auf diese Gruppe verwendet. Sie kann später nicht geändert werden.

Falls für die Verwaltungsgruppe im Azure-Portal ein anderer Name angezeigt werden soll, fügen Sie den Parameter **DisplayName** zur Zeichenfolge hinzu. Beispiel: Wenn Sie eine Verwaltungsgruppe mit dem Gruppennamen „Contoso“ und dem Anzeigenamen „Contoso Group“ erstellen möchten, verwenden Sie das folgende Cmdlet:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group' -ParentId '/providers/Microsoft.Management/managementGroups/ContosoTenant'
```

Verwenden Sie den Parameter **ParentId**, um diese Verwaltungsgruppe unter einer anderen Verwaltung zu erstellen.

### <a name="create-in-azure-cli"></a>Erstellen in der Azure CLI

Verwenden Sie bei der Azure CLI den Befehl „az account management-group create“.

```azurecli-interactive
az account management-group create --name 'Contoso'
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links:

- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](create.md)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](manage.md)
- [Überprüfen von Verwaltungsgruppen im Azure PowerShell-Ressourcenmodul](/powershell/module/az.resources#resources)
- [Überprüfen von Verwaltungsgruppen in der REST-API](/rest/api/resources/managementgroups)
- [Überprüfen von Verwaltungsgruppen in der Azure CLI](/cli/azure/account/management-group)
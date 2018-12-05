---
title: Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen in Azure
description: Hier erfahren Sie, wie Sie die Verwaltungsgruppenhierarchie verwalten und aktualisieren.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 10dfa9812a0546f3a8c57e28227851b6f72657fc
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582414"
---
# <a name="manage-your-resources-with-management-groups"></a>Verwalten von Ressourcen mit Verwaltungsgruppen

Bei Verwaltungsgruppen handelt es sich um Container, mit denen Sie Zugriff, Richtlinien und Konformität abonnementübergreifend verwalten können. Sie können diese Container ändern, löschen und verwalten, um Hierarchien zu erhalten, die mit [Azure Policy](../policy/overview.md) und mit der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure](../../role-based-access-control/overview.md) verwendet werden können. Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).

Wenn Sie Änderungen an einer Verwaltungsgruppe vornehmen möchten, müssen Sie Mitglied der Rolle „Besitzer“ oder „Mitwirkender“ für die Verwaltungsgruppe sein. Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu RBAC-Rollen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../../role-based-access-control/overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Ändern des Namens einer Verwaltungsgruppe

Der Name einer Verwaltungsgruppe kann über das Portal, mithilfe von PowerShell oder mithilfe der Azure CLI geändert werden.

### <a name="change-the-name-in-the-portal"></a>Ändern des Namens im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die Verwaltungsgruppe aus, die Sie umbenennen möchten.

1. Klicken Sie oben auf der Seite auf die Option **Gruppe umbenennen**.

   ![Gruppe umbenennen](./media/detail_action_small.png)

1. Wenn das Menü geöffnet wird, geben Sie den neuen Namen ein, der angezeigt werden soll.

   ![Gruppe umbenennen](./media/rename_context.png)

1. Wählen Sie **Speichern**aus.

### <a name="change-the-name-in-powershell"></a>Ändern des Namens in PowerShell

Verwenden Sie zum Aktualisieren des Anzeigenamens den Befehl **Update-AzureRmManagementGroup**. Wenn Sie beispielsweise den Namen einer Verwaltungsgruppe von „Contoso IT“ in „Contoso Group“ ändern möchten, führen Sie den folgenden Befehl aus:

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Ändern des Namens über die Azure CLI

Verwenden Sie für die Azure CLI den Aktualisierungsbefehl.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Löschen einer Verwaltungsgruppe

Um eine Verwaltungsgruppe zu löschen, müssen die folgenden Anforderungen erfüllt sein:

1. Unter der Verwaltungsgruppe gibt es keine untergeordneten Verwaltungsgruppen oder Abonnements.

   - Informationen zum Verschieben eines Abonnements aus einer Verwaltungsgruppe finden Sie unter [Verschieben eines Abonnements in eine andere Verwaltungsgruppe](#Move-subscriptions-in-the-hierarchy).

   - Informationen zum Verschieben einer Verwaltungsgruppe in eine andere Verwaltungsgruppe finden Sie unter [Verschieben von Verwaltungsgruppen in der Hierarchie](#Move-management-groups-in-the-hierarchy).

1. Sie haben Schreibzugriff auf die Verwaltungsgruppe (Rolle „Besitzer“ oder „Mitwirkender“ für die Verwaltungsgruppe). Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu RBAC-Rollen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Löschen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die zu löschende Verwaltungsgruppe aus.

1. Wählen Sie **Löschen** aus.

   - Wenn das Symbol abgeblendet ist, zeigen Sie mit der Maus darauf, um den Grund dafür anzuzeigen.

   ![Löschen einer Gruppe](./media/delete.png)

1. Ein Fenster wird geöffnet, in dem Sie das Löschen der Verwaltungsgruppe bestätigen müssen.

   ![Löschen einer Gruppe](./media/delete_confirm.png)

1. Wählen Sie **Ja** aus.

### <a name="delete-in-powershell"></a>Löschen in PowerShell

Verwenden Sie zum Löschen von Verwaltungsgruppen den Befehl **Remove-AzureRmManagementGroup** in PowerShell.

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Löschen über die Azure-Befehlszeilenschnittstelle

Verwenden Sie bei der Azure CLI den Befehl „az account management-group delete“.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Anzeigen von Verwaltungsgruppen

Sie können jede Verwaltungsgruppe anzeigen, für die Sie eine direkte oder geerbte RBAC-Rolle besitzen.  

### <a name="view-in-the-portal"></a>Anzeigen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Die Hierarchieseite der Verwaltungsgruppe wird geladen, auf der Sie alle Verwaltungsgruppen und Abonnements einsehen können, auf die Sie Zugriff haben. Durch Auswahl des Gruppennamens gelangen Sie eine Ebene in der Hierarchie nach unten. Die Navigation funktioniert genauso wie im Datei-Explorer.

   ![Main](./media/main.png)

1. Um die Details der Verwaltungsgruppe zu sehen, wählen Sie den Link **(Details)** neben dem Titel der Verwaltungsgruppe. Wenn dieser Link nicht verfügbar ist, sind Sie zum Anzeigen dieser Verwaltungsgruppe nicht berechtigt.  

### <a name="view-in-powershell"></a>Anzeigen in PowerShell

Mit dem Befehl „Get-AzureRmManagementGroup“ rufen Sie alle Gruppen ab.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```

Verwenden Sie den Parameter „-GroupName“, um die Informationen einer einzelnen Verwaltungsgruppe anzuzeigen.

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="view-in-azure-cli"></a>Anzeigen in der Azure CLI

Verwenden Sie den Befehl „list“, um alle Gruppen abzurufen.  

```azurecli-interactive
az account management-group list
```

Verwenden Sie den Befehl „show“, um die Informationen einer einzelnen Verwaltungsgruppe anzuzeigen.

```azurecli-interactive
az account management-group show --name 'Contoso'
```

## <a name="move-subscriptions-in-the-hierarchy"></a>Verschieben von Abonnements in der Hierarchie

Ein Grund zum Erstellen einer Verwaltungsgruppe ist das Bündeln von Abonnements. Nur Verwaltungsgruppen und Abonnements können als untergeordnete Elemente einer anderen Verwaltungsgruppe festgelegt werden. Ein Abonnement, das in eine Verwaltungsgruppe verschoben wird, erbt den gesamten Benutzerzugriff und alle Richtlinien von der übergeordneten Verwaltungsgruppe.

Für das Verschieben des Abonnements benötigen Sie einige Berechtigungen:

- Die Rolle „Besitzer“ für das untergeordnete Abonnement
- Die Rolle „Besitzer“ oder „Mitwirkender“ für die neue übergeordnete Verwaltungsgruppe
- Die Rolle „Besitzer“ oder „Mitwirkender“ für die alte übergeordnete Verwaltungsgruppe

Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu RBAC-Rollen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Verschieben von Abonnements im Portal

#### <a name="add-an-existing-subscription-to-a-management-group"></a>Hinzufügen eines vorhandenen Abonnements zu einer Verwaltungsgruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die Verwaltungsgruppe aus, die als übergeordnete Gruppe festgelegt werden soll.

1. Klicken Sie am oberen Rand der Seite auf **Abonnement hinzufügen**.

1. Wählen Sie in der Liste das Abonnement mit der richtigen ID aus.

   ![Untergeordnete Elemente](./media/add_context_sub.png)

1. Wählen Sie „Speichern“ aus.

#### <a name="remove-a-subscription-from-a-management-group"></a>Entfernen eines Abonnements aus einer Verwaltungsgruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die geplante Verwaltungsgruppe aus. Hierbei handelt es sich um die aktuelle übergeordnete Gruppe.  

1. Klicken Sie in der Liste am Ende der Zeile des zu verschiebenden Abonnements auf die Ellipse.

   ![Move](./media/move_small.png)

1. Klicken Sie auf **Verschieben**.

1. Klicken Sie im angezeigten Menü auf **Übergeordnete Verwaltungsgruppe**.

   ![Move](./media/move_small_context.png)

1. Wählen Sie **Speichern**aus.

### <a name="move-subscriptions-in-powershell"></a>Verschieben von Abonnements in PowerShell

Verschieben Sie Abonnements in PowerShell mit dem Befehl „New-AzureRmManagementGroupSubscription“.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Verwenden Sie zum Entfernen der Verknüpfung zwischen dem Abonnement und der Verwaltungsgruppe den Befehl „Remove-AzureRmManagementGroupSubscription“.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Verschieben von Abonnements in der Azure CLI

Verwenden Sie zum Verschieben eines Abonnements in der CLI den Befehl „add“.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Verwenden Sie zum Entfernen des Abonnements aus der Verwaltungsgruppe den Befehl „remove“.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>Verschieben von Verwaltungsgruppen in der Hierarchie  

Wenn Sie eine übergeordnete Verwaltungsgruppe verschieben, werden auch alle untergeordneten Ressourcen, die Verwaltungsgruppen, Abonnements, Ressourcengruppen und Ressourcen enthalten, mit der übergeordneten Gruppe verschoben.

### <a name="move-management-groups-in-the-portal"></a>Verschieben von Verwaltungsgruppen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die Verwaltungsgruppe aus, die als übergeordnete Gruppe festgelegt werden soll.

1. Klicken Sie am oberen Rand der Seite auf **Verwaltungsgruppe hinzufügen**.

1. Ein Menü wird geöffnet, in dem Sie auswählen, ob Sie eine neue oder vorhandene Verwaltungsgruppe verwenden möchten.

   - Durch die Auswahl von „Neu“ wird eine neue Verwaltungsgruppe erstellt.
   - Wenn Sie eine vorhandene Verwaltungsgruppe auswählen, wird eine Dropdownliste mit allen Verwaltungsgruppen angezeigt, die Sie in diese Verwaltungsgruppe verschieben können.  

   ![Verschieben](./media/add_context_MG.png)

1. Wählen Sie **Speichern**aus.

### <a name="move-management-groups-in-powershell"></a>Verschieben von Verwaltungsgruppen in PowerShell

Verwenden Sie den Befehl „Update-AzureRmManagementGroup“ in PowerShell, um eine Verwaltungsgruppe in eine andere Gruppe zu verschieben.

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'Contoso' -ParentName 'ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Verschieben von Verwaltungsgruppen in der Azure CLI

Verwenden Sie den Befehl „update“, um eine Verwaltungsgruppe mit der Azure CLI zu verschieben.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent 'Contoso Tenant'
```

## <a name="audit-management-groups-using-activity-logs"></a>Überwachen von Verwaltungsgruppen mithilfe von Aktivitätsprotokollen

Verwenden Sie zum Nachverfolgen von Verwaltungsgruppen über diese API die [Mandantenaktivitätsprotokoll-API](/rest/api/monitor/tenantactivitylogs). Zurzeit ist es nicht möglich, PowerShell, die CLI oder das Azure-Portal zum Nachverfolgen der Verwaltungsgruppenaktivität zu verwenden.

1. Als Mandantenadministrator des Azure AD-Mandanten [erhöhen Sie die Zugriffsrechte](../../role-based-access-control/elevate-access-global-admin.md), und weisen Sie dann dem überwachenden Benutzer eine Leserrolle über den Bereich `/providers/microsoft.insights/eventtypes/management` zu.
1. Rufen Sie als überwachender Benutzer die [Mandantenaktivitätsprotokoll-API](/rest/api/monitor/tenantactivitylogs) auf, um Verwaltungsgruppenaktivitäten anzuzeigen. Es wird empfohlen, für alle Verwaltungsgruppenaktivitäten nach dem Ressourcenanbieter **Microsoft.Management** zu filtern.  Beispiel:

```xml
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Management'"
```

> [!NOTE]
> Um diese API bequem über die Befehlszeile aufzurufen, verwenden Sie [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links:

- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](create.md)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](manage.md)
- [Überprüfen von Verwaltungsgruppen im Azure PowerShell-Ressourcenmodul](https://aka.ms/mgPSdocs)
- [Überprüfen von Verwaltungsgruppen in der REST-API](https://aka.ms/mgAPIdocs)
- [Überprüfen von Verwaltungsgruppen in der Azure CLI](https://aka.ms/mgclidoc)

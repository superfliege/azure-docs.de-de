---
title: Anzeigen von Azure-Aktivitätsprotokollen zur Überwachung von Ressourcen | Microsoft-Dokumentation
description: Verwenden Sie die Aktivitätsprotokolle zur Überprüfung von Benutzeraktionen und Fehlern. Zeigt Azure-Portal, PowerShell, Azure-Befehlszeilenschnittstelle und REST.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: tomfitz
ms.openlocfilehash: 70f6f8a7837b9e87b2720a866f14983356d23691
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487675"
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Anzeigen von Aktivitätsprotokollen, um Aktionen an Ressourcen zu überwachen

Mithilfe von Aktivitätsprotokollen können Sie Folgendes ermitteln:

* Welche Vorgänge in den Ressourcen in Ihrem Abonnement ausgeführt wurden
* Den Benutzer oder das System, von dem der Vorgang initiiert wurde
* Wann der Vorgang abgeschlossen wurde
* Den Status des Vorgangs
* Die Werte anderer Eigenschaften, mit denen Sie den Vorgang eventuell untersuchen können

Das Aktivitätsprotokoll enthält alle Schreibvorgänge (PUT, POST, DELETE), die für Ihre Ressourcen durchgeführt wurden. Es enthält jedoch keine Lesevorgänge (GET). Eine Liste der Ressourcenvorgänge finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../role-based-access-control/resource-provider-operations.md). Sie können bei der Problembehandlung mithilfe der Überwachungsprotokolle einen Fehler ermitteln oder nachverfolgen, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat.

Aktivitätsprotokolle werden 90 Tage lang aufbewahrt. Sie können beliebige Datumsbereiche abfragen, das Startdatum darf jedoch maximal 90 Tage zurückliegen.

Sie können Informationen aus den Aktivitätsprotokollen über das Portal, PowerShell, die Azure-Befehlszeilenschnittstelle, die Insights-REST-API oder die [Insights-.NET-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Insights/)abrufen.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="the-azure-portal"></a>Das Azure-Portal

1. Wählen Sie zum Anzeigen der Aktivitätsprotokolle über das Portal die Option **Überwachen** aus.

    ![Option „Überwachen“ auswählen](./media/resource-group-audit/select-monitor.png)

1. Wählen Sie **Aktivitätsprotokoll** aus.

    ![Aktivitätsprotokoll auswählen](./media/resource-group-audit/select-activity-log.png)

1. Es wird eine Zusammenfassung der aktuellen Vorgänge angezeigt. Auf die Vorgänge wird ein Standardsatz von Filtern angewendet.

    ![Anzeige einer Zusammenfassung der aktuellen Vorgänge](./media/resource-group-audit/audit-summary.png)

1. Um schnell einen vordefinierten Satz von Filtern anzuwenden, klicken Sie auf **Quick Insights** und wählen eine der verfügbaren Optionen aus.

    ![Auswählen der Abfrage](./media/resource-group-audit/quick-insights.png)

1. Um sich auf bestimmte Vorgänge zu konzentrieren, ändern Sie die Filter oder wenden neue Filter an. In der folgenden Abbildung wurde beispielsweise ein neuer Wert für den **Zeitraum** festgelegt, und als **Ressourcentyp** wurden Speicherkonten ausgewählt. 

    ![Filteroptionen festlegen](./media/resource-group-audit/set-filter.png)

1. Wenn Sie die Abfrage später erneut ausführen möchten, wählen Sie **Aktuelle Filter anheften** aus.

    ![Filter anheften](./media/resource-group-audit/pin-filters.png)

1. Geben Sie dem Filter einen Namen.

    ![Filter benennen](./media/resource-group-audit/name-filters.png)

1. Der Filter ist im Dashboard verfügbar.

    ![Filter im Dashboard anzeigen](./media/resource-group-audit/show-dashboard.png)

## <a name="powershell"></a>PowerShell

* Um Protokolleinträge abzurufen, führen Sie den Befehl **Get-AzLog** aus. Wenn Sie die Liste der Einträge filtern möchten, können Sie dem Befehl Parameter hinzufügen. Wenn Sie keinen Start- und Endzeitpunkt angeben, werden die Einträge der letzten sieben Tage zurückgegeben.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup
  ```

    Das folgende Beispiel zeigt, wie Sie mithilfe des Aktivitätsprotokolls nach Vorgängen suchen können, die während eines bestimmten Zeitraums durchgeführt wurden. Die Start- und Enddaten werden in einem Datumsformat angegeben.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup -StartTime 2019-01-09T06:00 -EndTime 2019-01-15T06:00
  ```

    Alternativ können Sie mithilfe von Datumsfunktionen den Datumsbereich angeben, beispielsweise die letzten 14 Tage.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

* Sie können die Aktionen anzeigen, die von einem bestimmten Benutzer durchgeführt wurden, selbst für eine Ressourcengruppe, die nicht mehr vorhanden ist.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

* Sie können nach Vorgängen mit Fehlern filtern.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup -Status Failed
  ```

* Sie können sich auf einen Fehler konzentrieren, indem Sie die Statusmeldung für diesen Eintrag betrachten.

  ```azurepowershell-interactive
  ((Get-AzLog -ResourceGroup ExampleGroup -Status Failed).Properties[0].Content.statusMessage | ConvertFrom-Json).error
  ```

* Sie können bestimmte Werte auswählen, um die zurückgegebenen Daten einzuschränken.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroupName ExampleGroup | Format-table EventTimeStamp, Caller, @{n='Operation'; e={$_.OperationName.value}}, @{n='Status'; e={$_.Status.value}}, @{n='SubStatus'; e={$_.SubStatus.LocalizedValue}}
  ```

* Je nachdem, welche Startzeit Sie angeben, können die oben stehenden Befehle eine sehr lange Liste mit Vorgängen für diese Ressourcengruppe zurückgeben. Sie können die Ergebnisse filtern, indem Sie Suchkriterien eingeben. Beispielsweise können Sie nach dem Typ des Vorgangs filtern.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup | Where-Object {$_.OperationName.value -eq "Microsoft.Resources/deployments/write"}
  ```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

* Um Protokolleinträge abzurufen, führen Sie den Befehl [az monitor activity-log list](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) mit einem Offset zum Angeben des Zeitraums aus.

  ```azurecli-interactive
  az monitor activity-log list --resource-group ExampleGroup --offset 7d
  ```

  Das folgende Beispiel zeigt, wie Sie mithilfe des Aktivitätsprotokolls nach Vorgängen suchen können, die während eines bestimmten Zeitraums durchgeführt wurden. Die Start- und Enddaten werden in einem Datumsformat angegeben.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --start-time 2019-01-01 --end-time 2019-01-15
  ```

* Sie können die Aktionen anzeigen, die von einem bestimmten Benutzer durchgeführt wurden, selbst für eine Ressourcengruppe, die nicht mehr vorhanden ist.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --caller someone@contoso.com --offset 5d
  ```

* Sie können nach Vorgängen mit Fehlern filtern.

  ```azurecli-interactive
  az monitor activity-log list -g demoRG --status Failed --offset 1d
  ```

* Sie können sich auf einen Fehler konzentrieren, indem Sie die Statusmeldung für diesen Eintrag betrachten.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --status Failed --offset 1d --query [].properties.statusMessage
  ```

* Sie können bestimmte Werte auswählen, um die zurückgegebenen Daten einzuschränken.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --offset 1d --query '[].{Operation: operationName.value, Status: status.value, SubStatus: subStatus.localizedValue}'
  ```

* Je nachdem, welche Startzeit Sie angeben, können die oben stehenden Befehle eine sehr lange Liste mit Vorgängen für diese Ressourcengruppe zurückgeben. Sie können die Ergebnisse filtern, indem Sie Suchkriterien eingeben. Beispielsweise können Sie nach dem Typ des Vorgangs filtern.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --offset 1d --query "[?operationName.value=='Microsoft.Storage/storageAccounts/write']"
  ```

## <a name="rest-api"></a>REST-API

Die REST-Vorgänge für die Arbeit mit Aktivitätsprotokollen gehören zur [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Informationen zum Abrufen von Aktivitätsprotokollereignissen finden Sie unter [Auflisten der Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Nächste Schritte

* Azure-Aktivitätsprotokolle können mit Power BI verwendet werden, um bessere Einblicke in die in Ihrem Abonnement ausgeführten Aktionen zu erhalten. Informationen hierzu finden Sie unter [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)(Anzeigen und Analysieren von Azure-Aktivitätsprotokollen in Power BI und mehr).
* Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/role-assignments-portal.md).
* Weitere Informationen zu den Befehlen zum Anzeigen von Bereitstellungsvorgängen finden Sie unter [View deployment operations with Azure Resource Manager](resource-manager-deployment-operations.md) (Anzeigen von Bereitstellungsvorgängen mit Azure Resource Manager).
* Informationen zum Verhindern von Löschvorgängen für eine Ressource für alle Benutzer finden Sie unter [Sperren von Ressourcen mit Azure Resource Manager](resource-group-lock-resources.md).
* Eine Liste der Vorgänge, die für jeden Microsoft Azure Resource Manager-Anbieter verfügbar sind, finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../role-based-access-control/resource-provider-operations.md).

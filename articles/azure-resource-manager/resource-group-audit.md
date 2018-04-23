---
title: Anzeigen von Azure-Aktivitätsprotokollen zur Überwachung von Ressourcen | Microsoft-Dokumentation
description: Verwenden Sie die Aktivitätsprotokolle zur Überprüfung von Benutzeraktionen und Fehlern. Zeigt Azure-Portal, PowerShell, Azure-Befehlszeilenschnittstelle und REST.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2018
ms.author: tomfitz
ms.openlocfilehash: bd11ec7c8a32792853e60b4bb14d4ed52f53f4a3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Anzeigen von Aktivitätsprotokollen, um Aktionen an Ressourcen zu überwachen

Mithilfe von Aktivitätsprotokollen können Sie Folgendes ermitteln:

* Welche Vorgänge in den Ressourcen in Ihrem Abonnement ausgeführt wurden
* Wer den Vorgang initiiert hat (auch wenn die von einem Back-End-Dienst initiierten Vorgänge keinen Benutzer als Aufrufer zurückgeben)
* Wann der Vorgang abgeschlossen wurde
* Den Status des Vorgangs
* Die Werte anderer Eigenschaften, mit denen Sie den Vorgang eventuell untersuchen können

Das Aktivitätsprotokoll enthält alle Schreibvorgänge (PUT, POST, DELETE), die für Ihre Ressourcen durchgeführt wurden. Es enthält jedoch keine Lesevorgänge (GET). Eine Liste der Ressourcenvorgänge finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../role-based-access-control/resource-provider-operations.md). Sie können bei der Problembehandlung mithilfe der Überwachungsprotokolle einen Fehler ermitteln oder nachverfolgen, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat.

Aktivitätsprotokolle werden 90 Tage lang aufbewahrt. Sie können beliebige Datumsbereiche abfragen, das Startdatum darf jedoch maximal 90 Tage zurückliegen.



Sie können Informationen aus den Aktivitätsprotokollen über das Portal, PowerShell, die Azure-Befehlszeilenschnittstelle, die Insights-REST-API oder die [Insights-.NET-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Insights/)abrufen.

## <a name="portal"></a>Portal

1. Wählen Sie zum Anzeigen der Aktivitätsprotokolle über das Portal die Option **Überwachen** aus.
   
    ![Aktivitätsprotokolle auswählen](./media/resource-group-audit/select-monitor.png)

   Sie können auch automatisch nach dem Aktivitätsprotokoll für eine bestimmte Ressource oder Ressourcengruppe filtern, indem Sie die Option **Aktivitätsprotokoll** auswählen. Beachten Sie, dass das Aktivitätsprotokoll automatisch nach der ausgewählten Ressource gefiltert wird.
   
    ![Nach Ressourcen filtern](./media/resource-group-audit/filtered-by-resource.png)
2. Im **Aktivitätsprotokoll** finden Sie eine Zusammenfassung der aktuellen Vorgänge.
   
    ![Aktionen anzeigen](./media/resource-group-audit/audit-summary.png)
3. Um die Anzahl der angezeigten Vorgänge zu einzuschränken, wählen Sie verschiedene Bedingungen aus. Die folgende Abbildung zeigt beispielsweise die Felder **Zeitraum** und **Ereignis initiiert von**, die geändert wurden, um die Aktionen anzuzeigen, die von einem bestimmten Benutzer oder einer bestimmten Anwendung im letzten Monat durchgeführt wurden. Wählen Sie **Übernehmen** aus, um die Ergebnisse Ihrer Abfrage anzuzeigen.
   
    ![Filteroptionen festlegen](./media/resource-group-audit/set-filter.png)

4. Wenn Sie die Abfrage später erneut ausführen möchten, wählen Sie **Speichern** aus, und benennen Sie die Abfrage.
   
    ![Abfrage speichern](./media/resource-group-audit/save-query.png)
5. Zum schnellen Ausführen einer Abfrage können Sie eine der integrierten Abfragen auswählen, z.B. Bereitstellungsfehler.

    ![Auswählen der Abfrage](./media/resource-group-audit/select-quick-query.png)

   Die ausgewählte Abfrage legt automatisch die erforderlichen Filterwerte fest.

    ![Anzeigen von Bereitstellungsfehlern](./media/resource-group-audit/view-failed-deployment.png)   

6. Wählen Sie einen der Vorgänge aus, um eine Zusammenfassung des Ereignisses anzuzeigen.

    ![Anzeigen von Vorgängen](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell

1. Um Protokolleinträge abzurufen, führen Sie den Befehl **Get-AzureRmLog** aus. Wenn Sie die Liste der Einträge filtern möchten, können Sie dem Befehl Parameter hinzufügen. Wenn Sie keine Start- und Endzeit angeben, werden die Einträge der letzten Stunde zurückgegeben. Führen Sie beispielsweise folgenden Befehl aus, um die Vorgänge für eine Ressourcengruppe während der letzten Stunde abzurufen:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    Das folgende Beispiel zeigt, wie Sie mithilfe des Aktivitätsprotokolls nach Vorgängen suchen können, die während eines bestimmten Zeitraums durchgeführt wurden. Die Start- und Enddaten werden in einem Datumsformat angegeben.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Alternativ können Sie mithilfe von Datumsfunktionen den Datumsbereich angeben, beispielsweise die letzten 14 Tage.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Je nachdem, welche Startzeit Sie angeben, können die oben stehenden Befehle eine sehr lange Liste mit Vorgängen für diese Ressourcengruppe zurückgeben. Sie können die Ergebnisse filtern, indem Sie Suchkriterien eingeben. Wenn Sie beispielsweise herausfinden möchten, wie eine Web-App angehalten wurde, können Sie folgenden Befehl ausführen:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    In diesem Beispiel wird damit gezeigt, dass die Aktion durch someone@contoso.com durchgeführt wurde. 

  ```powershell 
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. Sie können die Aktionen anzeigen, die von einem bestimmten Benutzer durchgeführt wurden, selbst für eine Ressourcengruppe, die nicht mehr vorhanden ist.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Sie können nach Vorgängen mit Fehlern filtern.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Sie können sich auf einen Fehler konzentrieren, indem Sie die Statusmeldung für diesen Eintrag betrachten.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Ausgabe des Befehls:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Um Protokolleinträge abzurufen, führen Sie den Befehl [az monitor activity-log list](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) aus.

  ```azurecli
  az monitor activity-log list --resource-group <group name>
  ```


## <a name="rest-api"></a>REST-API

Die REST-Vorgänge für die Arbeit mit Aktivitätsprotokollen gehören zur [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Informationen zum Abrufen von Aktivitätsprotokollereignissen finden Sie unter [Auflisten der Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Nächste Schritte

* Azure-Aktivitätsprotokolle können mit Power BI verwendet werden, um bessere Einblicke in die in Ihrem Abonnement ausgeführten Aktionen zu erhalten. Informationen hierzu finden Sie unter [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)(Anzeigen und Analysieren von Azure-Aktivitätsprotokollen in Power BI und mehr).
* Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/role-assignments-portal.md).
* Weitere Informationen zu den Befehlen zum Anzeigen von Bereitstellungsvorgängen finden Sie unter [View deployment operations with Azure Resource Manager](resource-manager-deployment-operations.md) (Anzeigen von Bereitstellungsvorgängen mit Azure Resource Manager).
* Informationen zum Verhindern von Löschvorgängen für eine Ressource für alle Benutzer finden Sie unter [Sperren von Ressourcen mit Azure Resource Manager](resource-group-lock-resources.md).
* Eine Liste der Vorgänge, die für jeden Microsoft Azure Resource Manager-Anbieter verfügbar sind, finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../role-based-access-control/resource-provider-operations.md).


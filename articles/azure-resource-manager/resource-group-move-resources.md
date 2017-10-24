---
title: Verschieben von Azure-Ressourcen in ein neues Abonnement oder eine neue Ressourcengruppe | Microsoft-Dokumentation
description: Verwenden Sie Azure Resource Manager, um Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: tomfitz
ms.openlocfilehash: 326039c58466e65183a594e222db24e998f151b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement

In diesem Artikel erfahren Sie, wie Sie Ressourcen in ein neues Abonnement oder eine neue Ressourcengruppe im gleichen Abonnement verschieben. Sie können das Portal, PowerShell, die Azure-Befehlszeilenschnittstelle oder die REST-API verwenden, um Ressourcen zu verschieben. Die Verschiebevorgänge in diesem Artikel werden Ihnen ohne Unterstützung durch den Azure-Support zur Verfügung gestellt.

Beim Verschieben von Ressourcen werden die Quell- und Zielgruppe für die Dauer des Vorgangs gesperrt. Schreib- und Löschvorgänge in den Ressourcengruppen werden bis zum Abschluss der Verschiebung blockiert. Diese Sperre bedeutet, dass Sie in Ressourcengruppen keinen Ressourcen hinzufügen, aktualisieren oder löschen können. Es heißt aber nicht, dass die Ressourcen eingefroren sind. Wenn Sie beispielsweise eine SQL Server-Instanz und ihre Datenbank in eine neue Ressourcengruppe verschieben, weist eine Anwendung, die die Datenbank nutzt, keine Ausfallzeiten auf. Sie hat weiterhin Lese- und Schreibzugriff auf die Datenbank.

Sie können nicht den Speicherort der Ressource ändern. Wenn Sie ein Ressource verschieben, wird sie nur in eine neue Ressourcengruppe verschoben. Die neue Ressourcengruppe hat möglicherweise einen anderen Speicherort, das heißt jedoch nicht, dass der Speicherort der Ressource geändert wird.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie Ressourcen in einem vorhandenen Azure-Kontoangebot verschieben. Falls Sie Ihr Azure-Kontoangebot ändern (z.B. ein Upgrade von nutzungsbasierter Bezahlung auf einen Pre-Pay-Tarif) und Ihre vorhandenen Ressourcen weiterverwenden möchten, helfen Ihnen die Informationen unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](../billing/billing-how-to-switch-azure-offer.md) weiter.
>
>

## <a name="checklist-before-moving-resources"></a>Checkliste vor dem Verschieben von Ressourcen

Beim Verschieben einer Ressource sollten Sie einige wichtige Schritte ausführen: Indem Sie diese Bedingungen überprüfen, können Sie Fehler vermeiden.

1. Quell- und Zielabonnement müssen im selben [Azure Active Directory-Mandanten](../active-directory/active-directory-howto-tenant.md) vorhanden sein. Um zu überprüfen, ob beide Abonnements die gleiche Mandanten-ID aufweisen, verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle.

  Verwenden Sie für Azure PowerShell Folgendes:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Wenn die Mandanten-IDs für das Quell- und das Zielabonnement nicht gleich sind, müssen Sie sich an den [Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) wenden, um die Ressourcen in einen neuen Mandanten zu verschieben.

2. Der Dienst muss die Möglichkeit bieten, Ressourcen zu verschieben. In diesem Artikel wird erläutert, welche Dienste das Verschieben von Ressourcen ermöglichen und welche nicht.
3. Das Zielabonnement muss für den Ressourcenanbieter der verschobenen Ressource registriert sein. Andernfalls erhalten Sie eine Fehlermeldung, die besagt, dass das **Abonnement nicht für einen Ressourcentyp registriert ist**. Dieses Problem kann auftreten, wenn eine Ressource zu einem neuen Abonnement verschoben wird, dieses aber noch nie mit diesem Ressourcentyp verwendet wurde.

  Verwenden Sie für PowerShell die folgenden Befehle zum Abrufen des Registrierungsstatus:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  Verwenden Sie für die Azure CLI die folgenden Befehle zum Abrufen des Registrierungsstatus:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

## <a name="when-to-call-support"></a>Kontaktaufnahme mit dem Support

Sie können die meisten Ressourcen mithilfe der in diesem Artikel gezeigten Self-Service-Vorgänge verschieben. Verwenden Sie die Self-Service-Vorgänge für Folgendes:

* Verschieben von Resource Manager-Ressourcen
* Verschieben von klassischen Ressourcen unter Berücksichtigung der [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)

Wenden Sie sich in folgenden Fällen an den [Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview):

* Verschieben von Ressourcen in ein neues Azure-Konto (und einen anderen Azure Active Directory-Mandanten)
* Verschieben von klassischen Ressourcen, wenn Probleme durch Einschränkungen auftreten

## <a name="services-that-enable-move"></a>Dienste, die eine Verschiebung ermöglichen

Die folgenden Dienste ermöglichen das Verschieben in eine neue Ressourcengruppe und ein neues Abonnement:

* API Management
* App Service-Apps (Web-Apps) – siehe [App Service-Einschränkungen](#app-service-limitations)
* Application Insights
* Automation
* Azure Cosmos DB
* Batch
* Bing Maps
* CDN
* Cloud Services – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)
* Cognitive Services
* Content Moderator
* Datenkatalog
* Data Factory
* Data Lake Analytics
* Data Lake-Speicher
* DNS
* Event Hubs
* HDInsight-Cluster – siehe [HDInsight-Einschränkungen](#hdinsight-limitations)
* IoT Hubs
* Schlüsseltresor
* Load Balancer
* Logik-Apps
* Machine Learning
* Media Services
* Mobile Engagement
* Notification Hubs
* Operational Insights
* Operations Management
* Power BI
* Redis-Cache
* Scheduler
* Suche
* Server Management
* SERVICE BUS
* Service Fabric
* Speicher
* Speicher (klassisch) – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)
* Stream Analytics – Stream Analytics-Aufträge können nicht verschoben werden, wenn sie ausgeführt werden.
* SQL-Datenbankserver – Die Datenbank und der Server müssen sich in derselben Ressourcengruppe befinden. Wenn Sie eine SQL Server-Instanz verschieben, werden auch alle ihre Datenbanken verschoben.
* Traffic Manager
* Virtual Machines – Virtuelle Computer mit verwalteten Datenträgern können nicht verschoben werden. Weitere Informationen finden Sie unter [Einschränkungen von virtuellen Computern](#virtual-machines-limitations).
* Virtual Machines (klassisch) – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)
* VM-Skalierungsgruppen – siehe [Einschränkungen von virtuellen Computern](#virtual-machines-limitations)
* Virtual Network – siehe [Einschränkungen von virtuellen Netzwerken](#virtual-networks-limitations)
* VPN Gateway

## <a name="services-that-do-not-enable-move"></a>Dienste, die keine Verschiebung ermöglichen

Die folgenden Dienste ermöglichen das Verschieben einer Ressource derzeit nicht:

* AD Domain Services
* AD Hybrid Health Service
* Application Gateway
* BizTalk Services
* Container Service
* ExpressRoute
* DevTest-Labs: Das Verschieben in eine neue Ressourcengruppe im gleichen Abonnement ist möglich, ein abonnementübergreifendes Verschieben jedoch nicht.
* Dynamics LCS
* Verwaltete Anwendungen
* Managed Disks – siehe [Einschränkungen von virtuellen Computern](#virtual-machines-limitations)
* Recovery Services-Tresor – verschieben Sie außerdem nicht die dem Recovery Services-Tresor zugeordneten Compute-, Netzwerk- und Speicherressourcen. Siehe [Einschränkungen von Recovery Services](#recovery-services-limitations).
* Sicherheit
* StorSimple-Geräte-Manager
* Virtual Networks (klassisch) – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Einschränkungen von virtuellen Computern

Verwaltete Datenträger unterstützen das Verschieben nicht. Diese Einschränkung bedeutet, dass auch verschiedene zugehörige Ressourcen nicht verschoben werden können. Folgende Elemente können nicht verschoben werden:

* Verwaltete Datenträger
* Virtuelle Computer mit verwalteten Datenträgern
* Auf der Grundlage von verwalteten Datenträgern erstellte Images
* Auf der Grundlage von verwalteten Datenträgern erstellte Momentaufnahmen
* Verfügbarkeitsgruppen mit virtuellen Computern mit verwalteten Datenträgern

Von Marketplace-Ressourcen erstellte virtuelle Computer können nicht abonnementübergreifend verschoben werden. Heben Sie die Bereitstellung des virtuellen Computers im aktuellen Abonnement auf, und stellen Sie ihn im neuen Abonnement erneut bereit.

Virtuelle Computer mit in Key Vault gespeichertem Zertifikat können in eine neue Ressourcengruppe im gleichen Abonnement verschoben werden, das abonnementübergreifende Verschieben ist jedoch nicht möglich.

## <a name="virtual-networks-limitations"></a>Einschränkungen von virtuellen Netzwerken

Um ein mittels Peering verknüpftes virtuelles Netzwerk zu verschieben, müssen Sie zunächst das Peering des virtuellen Netzwerks deaktivieren. Nach der Deaktivierung können Sie das virtuelle Netzwerk verschieben. Aktivieren Sie nach der Verschiebung das Peering des virtuellen Netzwerks wieder.

Ein virtuelles Netzwerk kann nicht in ein anderes Abonnement verschoben werden, wenn das virtuelle Netzwerk ein Subnetz mit Navigationslinks für Ressourcen enthält. Beispiel: Wenn eine Redis Cache-Ressource in einem Subnetz bereitgestellt wird, enthält dieses Subnetz einen Navigationslink für die Ressource.

## <a name="app-service-limitations"></a>App Service-Einschränkungen

Bei der Arbeit mit App Service-Apps können Sie nicht nur einen App Service-Plan verschieben. Zum Verschieben von App Service-Apps stehen folgende Optionen bereit:

* Verschieben Sie den App Service-Plan und alle anderen App Service-Ressourcen in dieser Ressourcengruppe in eine neue Ressourcengruppe, die noch nicht über App Service-Ressourcen verfügt. Diese Anforderung bedeutet, dass Sie auch die App Service-Ressourcen verschieben müssen, die nicht dem App Service-Plan zugeordnet sind.
* Verschieben Sie Apps in eine andere Ressourcengruppe, behalten Sie jedoch alle App Services-Pläne in der ursprünglichen Ressourcengruppe bei.

Der App Service-Plan muss sich nicht in derselben Ressourcengruppe wie die App befinden, damit die App ordnungsgemäß funktioniert.

Angenommen, Ihre Ressourcengruppe enthält folgende Ressourcen:

* **web-a**, die **plan-a** zugeordnet ist
* **web-b**, die **plan-b** zugeordnet ist

Die Optionen sind wie folgt:

* Verschieben von **web-a**, **plan-a**, **web-b** und **plan-b**
* Verschieben von **web-a** und **web-b**
* Verschieben von **web-a**
* Verschieben von **web-b**

Alle anderen Kombinationen umfassen das Hinterlassen eines Ressourcentyps, der beim Verschieben eines App Service-Plans (beliebiger Typ von App Service-Ressource) nicht zurückbleiben darf.

Wenn sich Ihre Web-App in einer anderen Ressourcengruppe als ihr App Service-Plan befindet, aber Sie beide in eine neue Ressourcengruppe verschieben möchten, muss das Verschieben in zwei Schritten erfolgen. Beispiel:

* **web-a** befindet sich in **web-group**
* **plan-a** befindet sich in **plan-group**
* Sie möchten, dass sich **web-a** und **plan-a** in **combined-group** befinden

Um dies zu erreichen, führen Sie zwei getrennte Verschiebevorgänge in der folgenden Reihenfolge aus:

1. Verschieben Sie **web-a** nach **plan-group**.
2. Verschieben Sie **web-a** und **plan-a** nach **combined-group**.

Sie können ein App Service Certificate ohne Probleme in eine neue Ressourcengruppe oder ein neues Abonnement verschieben. Wenn Ihre Web-App jedoch ein SSL-Zertifikat enthält, das Sie extern erworben und in die App hochgeladen haben, müssen Sie das Zertifikat löschen, bevor Sie die Web-App verschieben. Sie können beispielsweise die folgenden Schritte ausführen:

1. Löschen des hochgeladenen Zertifikats aus der Web-App
2. Verschieben der Web-App
3. Hochladen des Zertifikats in die Web-App

## <a name="classic-deployment-limitations"></a>Einschränkungen bei der klassischen Bereitstellung

Die Optionen zum Verschieben von Ressourcen, die über das klassische Modell bereitgestellt wurden, unterscheiden sich abhängig davon, ob Sie die Ressourcen innerhalb eines Abonnements oder in ein neues Abonnement verschieben.

### <a name="same-subscription"></a>Gleiches Abonnement

Beim Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere innerhalb des gleichen Abonnements gelten die folgenden Einschränkungen:

* Virtuelle Netzwerke (klassisch) können nicht verschoben werden.
* Virtuelle Computer (klassisch) müssen mit dem Clouddienst verschoben werden.
* Der Clouddienst kann nur verschoben werden, wenn der Verschiebevorgang alle dazugehörigen virtuellen Computer umfasst.
* Es kann immer nur ein Clouddienst gleichzeitig verschoben werden.
* Es kann immer nur ein Speicherkonto (klassisch) gleichzeitig verschoben werden.
* Ein Speicherkonto (klassisch) kann nicht im selben Vorgang mit einem virtuellen Computer oder Clouddienst verschoben werden.

Um klassische Ressourcen in eine neue Ressourcengruppe innerhalb des gleichen Abonnements zu verschieben, verwenden Sie die standardmäßigen Verschiebevorgänge im [Portal](#use-portal), in [Azure PowerShell](#use-powershell), in der [Azure-Befehlszeilenschnittstelle (CLI)](#use-azure-cli) oder in der [REST-API](#use-rest-api). Sie verwenden hierbei die gleichen Vorgänge wie beim Verschieben von Resource Manager-Ressourcen.

### <a name="new-subscription"></a>Neues Abonnement

Beim Verschieben von Ressourcen in ein neues Abonnement gelten die folgenden Einschränkungen:

* Alle klassische Ressourcen im Abonnement müssen im selben Vorgang verschoben werden.
* Das Zielabonnement darf keine anderen klassischen Ressourcen enthalten.
* Der Verschiebevorgang kann nur über eine separate REST-API für klassische Verschiebevorgänge angefordert werden. Die Resource Manager-Standardbefehle für das Verschieben funktionieren beim Verschieben klassischer Ressourcen in ein neues Abonnement nicht.

Verwenden Sie zum Verschieben von klassischen Ressourcen in ein neues Abonnement die REST-Vorgänge, die für klassische Ressourcen spezifisch sind. Führen Sie zum Verwenden von REST die folgenden Schritte aus:

1. Überprüfen Sie, ob das Quellabonnement an einem abonnementübergreifenden Verschiebevorgang teilnehmen kann. Gehen Sie folgendermaßen vor:

  ```HTTP
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     Fügen Sie Folgendes in den Anforderungstext ein:

  ```json
  {
    "role": "source"
  }
  ```

     Die Antwort für den Überprüfungsvorgang erfolgt in folgendem Format:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Überprüfen Sie, ob das Zielabonnement an einem abonnementübergreifenden Verschiebevorgang teilnehmen kann. Gehen Sie folgendermaßen vor:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     Fügen Sie Folgendes in den Anforderungstext ein:

  ```json
  {
    "role": "target"
  }
  ```

     Die Antwort liegt im gleichen Format vor wie bei der Überprüfung des Quellabonnements.
3. Wenn beide Abonnements die Überprüfung bestehen, verschieben Sie alle klassischen Ressourcen mithilfe des folgenden Vorgangs aus einem Abonnement in ein anderes:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    Fügen Sie Folgendes in den Anforderungstext ein:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

Dieser Vorgang kann einige Minuten dauern.

## <a name="recovery-services-limitations"></a>Einschränkungen von Recovery Services

Das Verschieben von Speicher-, Netzwerk- und Computeressourcen, die dazu dienen, eine Notfallwiederherstellung mit Azure Site Recovery einzurichten, ist nicht möglich.

Angenommen, Sie haben die Replikation Ihrer lokalen Computer in ein Speicherkonto (Storage1) eingerichtet, und möchten, dass der geschützte Computer nach einem Failover zu Azure als virtueller Computer (VM1) angezeigt wird, der an ein virtuelles Netzwerk (Network1) angeschlossen ist. Sie können dann die Azure-Ressourcen „Storage1“, „VM1“ und „Network1“ nicht zwischen Ressourcengruppen im selben Abonnement oder zwischen Abonnements verschieben.

## <a name="hdinsight-limitations"></a>HDInsight-Einschränkungen

Sie können HDInsight-Cluster in ein neues Abonnement oder eine neue Ressourcengruppe verschieben. Dagegen können die mit dem HDInsight-Cluster verknüpften Netzwerkressourcen (z.B. virtuelles Netzwerk, NIC oder Load Balancer) nicht zwischen Abonnements verschoben werden. Darüber hinaus kann eine NIC, die an einen virtuellen Computer für den Cluster angefügt ist, nicht in eine neue Ressourcengruppe verschoben werden.

Beim Verschieben eines HDInsight-Clusters in ein neues Abonnement sollten Sie zunächst andere Ressourcen (z.B. das Speicherkonto) verschieben. Verschieben Sie erst anschließend den HDInsight-Cluster.

## <a name="use-portal"></a>Mithilfe des Portals

Um Ressourcen zu verschieben, wählen Sie die Ressourcengruppe mit diesen Ressourcen und dann die Schaltfläche **Verschieben** aus.

![Verschieben von Ressourcen](./media/resource-group-move-resources/select-move.png)

Wählen Sie aus, ob Sie die Ressource in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.

Wählen Sie die zu verschiebenden Ressourcen und die Zielressourcengruppe aus. Bestätigen, dass Sie Skripts für diese Ressourcen aktualisieren müssen, und wählen Sie **OK**aus. Wenn Sie im vorherigen Schritt das Symbol „Abonnement bearbeiten“ ausgewählt haben, müssen Sie auch das Zielabonnement auswählen.

![Ziel auswählen](./media/resource-group-move-resources/select-destination.png)

Unter **Benachrichtigungen**wird angezeigt, dass der Verschiebevorgang ausgeführt wird.

![Verschiebestatus anzeigen](./media/resource-group-move-resources/show-status.png)

Sobald der Vorgang abgeschlossen ist, werden Sie über das Ergebnis informiert.

![Ergebnis des Verschiebens anzeigen](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Verwenden von PowerShell

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) . Im folgenden Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden:

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Um Ressourcen in ein neues Abonnement zu verschieben, schließen Sie einen Wert für den Parameter `DestinationSubscriptionId` ein.

## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl [az resource move](/cli/azure/resource?view=azure-cli-latest#az_resource_move). Geben Sie die Ressourcen-IDs der zu verschiebenden Ressourcen an. Im folgenden Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden: Geben Sie im `--ids`-Parameter eine durch Leerzeichen getrennte Liste der zu verschiebenden Ressourcen-IDs an.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Um Ressourcen in ein neues Abonnement zu verschieben, geben Sie den Parameter`--destination-subscription-id` an.

## <a name="use-rest-api"></a>REST-API

Führen Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement Folgendes aus:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

Geben Sie im Anforderungstext die Zielgruppe und die zu verschiebenden Ressourcen an. Weitere Informationen zur REST-Verschiebung finden Sie unter [Verschieben von Ressourcen](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu PowerShell-Cmdlets zum Verwalten Ihres Abonnements finden Sie unter [Verwenden von Azure PowerShell mit Resource Manager](powershell-azure-resource-manager.md).
* Informationen zu Befehlen der Azure-Befehlszeilenschnittstelle zum Verwalten Ihres Abonnements finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle mit Resource Manager](xplat-cli-azure-resource-manager.md).
* Informationen zu Portalfeatures zum Verwalten Ihres Abonnements finden Sie unter [Verwenden des Azure-Portals zum Verwalten von Ressourcen](resource-group-portal.md).
* Informationen zum Anwenden einer logischen Organisation auf Ihre Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md).

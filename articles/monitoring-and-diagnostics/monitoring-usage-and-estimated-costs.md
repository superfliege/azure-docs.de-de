---
title: Überwachen der Nutzung und geschätzten Kosten in Azure Monitor
description: Übersicht über die Verwendung der Azure Monitor-Seite für die Nutzung und geschätzten Kosten
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.component: ''
ms.openlocfilehash: edfcc244105403ae33251777c560d4cc21dfe5cb
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264281"
---
# <a name="monitoring-usage-and-estimated-costs"></a>Überwachen der Nutzung und der geschätzten Kosten

Die Seite **Nutzung und geschätzte Kosten** im Monitor-Hub des Azure-Portals erläutert die Nutzung wichtiger Überwachungsfeatures wie [Metriken, Warnungen, Benachrichtigungen](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) und [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Für Kunden mit Tarifen, die vor April 2018 verfügbar waren, umfasst dies auch die Log Analytics-Nutzung, die über das Insights- und Analytics-Angebot erworben wurde.

Auf dieser Seite können Benutzer ihre Ressourcennutzung der letzten 31 Tage, aggregiert pro Abonnement, einsehen. Detailansichten zeigen die Nutzungstrends über den Zeitraum von 31 Tagen. Für diese Schätzung müssen viele Daten zusammengefasst werden, sodass Sie beim Laden der Seite etwas Geduld aufbringen müssen.

Dieses Beispiel zeigt die Nutzungsüberwachung und eine Schätzung der anfallenden Kosten:

![Screenshot: „Nutzung und geschätzte Kosten“ im Portal](./media/monitoring-usage-and-estimated-costs/001.png)

Wählen Sie in der Spalte „Monatliche Nutzung“ den Link, um ein Diagramm mit den Nutzungstrends für die letzten 31 Tage anzuzeigen:

![Screenshot des Balkendiagramms: „Pro Knoten enthalten“](./media/monitoring-usage-and-estimated-costs/002.png)

Hier ist eine andere ähnliche Verwendung und Kostenzusammenfassung. Dieses Beispiel zeigt ein Abonnement aus dem neuen nutzungsbasierten Preismodell von April 2018. Beachten Sie, dass keine knotenbasierte Abrechnung vorhanden ist. Die Informationen zur Datenerfassung und -aufbewahrung für Log Analytics und Application Insights werden jetzt über eine neue gemeinsame Anzeige angegeben.

![Screenshot: „Nutzung und geschätzte Kosten“ im Portal – April 2018](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Neues Preismodell

Im April 2018 wurde ein [neues Preismodell für die Überwachung eingeführt](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Es handelt sich hierbei um für die Cloud optimierte und verbrauchsbasierte Preise. Sie zahlen ausschließlich nutzungsabhängig und gehen keine knotenbasierten Verpflichtungen ein. Die Details des neuen Preismodells sind jetzt für [Metriken, Warnungen, Benachrichtigungen](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) und [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) verfügbar. 

Für Kunden, die das Onboarding für Log Analytics oder Application Insights nach dem 2. April 2018 durchführen, ist das neue Preismodell die einzige Option. Für Kunden, die diese Dienste bereits nutzen, ist die Umstellung auf das neue Preismodell optional.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Auswirkungen des neuen Preismodells
Das neue Preismodell hat für jeden Kunden je nach Nutzungsmuster der Überwachung unterschiedliche Auswirkungen. Für Kunden, die Log Analytics oder Application Insights vor dem 2. April 2018 verwendet haben, wird auf der Seite **Nutzung und geschätzte Kosten** in Azure Monitor alle Änderungen der Kosten geschätzt, wenn sie auf das neue Preismodell umsteigen. Sie bietet die Möglichkeit, ein Abonnement in das neue Modell zu übernehmen. Für die meisten Kunden wird das neue Preismodell von Vorteil sein. Bei Kunden mit besonders hohem Datennutzungsverhalten oder in Regionen mit höheren Kosten ist dies möglicherweise nicht der Fall.

Um eine Kostenschätzung für die Abonnements zu sehen, die Sie auf der Seite **Nutzung und geschätzte Kosten** ausgewählt haben, wählen Sie das blaue Banner oben auf der Seite aus. Am besten ist es, wenn Sie dies für nur ein Abonnement auf einmal ausführen, denn das ist die Grundlage, auf der das neue Preismodell übernommen werden kann.

![Screenshot: Überwachen der Nutzung und der geschätzten Kosten im neuen Preismodell](./media/monitoring-usage-and-estimated-costs/004.png)

Die neue Seite zeigt eine ähnliche Version der vorherigen Seite mit einem grünen Banner:

![Screenshot: Überwachen der Nutzung und der geschätzten Kosten im aktuellen Preismodell](./media/monitoring-usage-and-estimated-costs/005.png)

Die Seite zeigt auch andere Anzeigen, die für das neue Preismodell verwendet werden. Diese Liste ist ein Beispiel:

- Insight & Analytics\Überschreitung pro Knoten
- Insight & Analytics\Pro Knoten enthalten
- Application Insights\Basic-Überschreitungsdaten
- Application Insights\Enthaltene Daten

Das neue Preismodell enthält keine knotenbasierten Datenzuordnungen. Daher werden diese Anzeigen für die Datenerfassung zu einer neuen gemeinsamen Anzeige für die Datenerfassung namens **Shared Services\Datenerfassung** zusammengefasst. 

Es gibt eine weitere Änderung der in Log Analytics oder Application Insights erfassten Daten in Regionen mit höheren Kosten. Daten für diese hochpreisigeren Regionen werden mit den neuen regionalen Verbrauchseinheiten angezeigt. Ein Beispiel ist **Datenerfassung (USA, Westen-Mitte)**.

> [!NOTE]
> Die Kontoebenenberechtigungen pro Knoten für das OMS-Abonnement (Operations Management Suite) werden nicht in die geschätzten Kosten pro Abonnement einbezogen. Wenden Sie sich in diesem Fall an Ihren Kontobeauftragten, um ausführlichere Informationen zum neuen Preismodell zu erhalten.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Neues Preismodell und Berechtigungen für Operations Management Suite-Abonnement

Kunden, die Microsoft Operations Management Suite E1 und E2 erworben haben, verfügen über Datenerfassungsberechtigungen pro Knoten für [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) und [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing#the-price-plans). Um diese Berechtigungen für Log Analytics-Arbeitsbereiche oder Application Insights-Ressourcen in einem bestimmten Abonnement zu erhalten, muss folgende Voraussetzung erfüllt sein: 

- Als Preismodell für dieses Abonnement muss das vor April 2018 gültige Modell weiterverwendet werden.
- Log Analytics-Arbeitsbereiche sollten den Tarif „Pro-Knoten (OMS)“ verwenden.
- Application Insights-Ressourcen sollten den „Enterprise“-Preisplan verwenden.

Je nach Anzahl von Knoten einer Suite, die von Ihrer Organisation erworben wurde, könnte die Umstellung von Abonnements auf das neue Preismodell trotzdem vorteilhaft sein. Dieser Schritt bedarf aber sorgfältiger Überlegung. Im Allgemeinen ist es empfehlenswert, einfach wie oben beschrieben das vor April 2018 gültige Modell beizubehalten.

> [!WARNING]
> Wenn Ihre Organisation Microsoft Operations Management Suite E1 und E2 erworben hat, sollten Sie normalerweise für Ihre Abonnements das vor April 2018 gültige Preismodell beibehalten. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Änderungen bei der Umstellung auf das neue Preismodell

Das neue Preismodell reduziert Log Analytics- und Application Insights-Preisoptionen auf nur einen einzigen Tarif (oder Plan). Die Umstellung eines Abonnements auf das neue Preismodell hat folgende Konsequenzen:

- Der Tarif für jede Log Analytics-Instanz wird in einen neuen Pro-GB-Tarif (im Azure Resource Manager „pergb2018“ genannt) umgewandelt.
- Alle Application Insights-Ressourcen des Enterprise-Tarifs werden in den Basic-Tarif geändert.

Die Kostenschätzung zeigt die Auswirkungen dieser Änderungen.

> [!WARNING]
> Beachten Sie den folgenden wichtigen Hinweis, wenn Sie Azure Resource Manager oder PowerShell verwenden, um [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) oder [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) in einem Abonnement bereitzustellen, das Sie auf das neue Preismodell umgestellt haben. Wenn Sie einen anderen Tarif/Plan als „pergb2018“ für Log Analytics oder „Basic“ für Application Insights angeben, wird die Bereitstellung nicht wegen Angabe eines ungültigen Tarifs/Plans erfolglos sein, **aber es wird nur der gültige Tarif/Plan verwendet**. (Dies gilt nicht für den Log Analytics-Tarif „Free“, da dabei eine Meldung über einen ungültige Tarif generiert wird).
>

## <a name="moving-to-the-new-pricing-model"></a>Durchführen der Umstellung auf das neue Preismodell

Wenn Sie sich entschieden haben, für ein Abonnement die Umstellung auf das neue Preismodell durchzuführen, können Sie oben auf der Seite **Nutzung und geschätzte Kosten** die Option **Preismodellauswahl** auswählen:

![Screenshot: Überwachen der Nutzung und der geschätzten Kosten im neuen Preismodell](./media/monitoring-usage-and-estimated-costs/006.png)

Die Seite **Preismodellauswahl** wird geöffnet. Sie zeigt eine Liste aller Abonnements, die Sie auf der vorherigen Seite angezeigt haben:

![Screenshot: Preismodellauswahl](./media/monitoring-usage-and-estimated-costs/007.png)

Aktivieren Sie einfach das jeweilige Kontrollkästchen, und wählen Sie **Speichern**, um ein Abonnement auf das neue Preismodell umzustellen. Auf die gleiche Weise können Sie auch wieder zurück zum alten Preismodell wechseln. Beachten Sie hierbei, dass die Berechtigung „Besitzer“ oder „Mitwirkender“ für das Abonnement erforderlich ist, um das Preismodell zu ändern.

## <a name="automate-moving-to-the-new-pricing-model"></a>Automatisieren der Umstellung auf das neue Preismodell

Für die unten angegebenen Skripts ist das Azure PowerShell-Modul erforderlich. Um zu überprüfen, ob Sie über die neueste Version verfügen, lesen Sie [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.1.0).

Sobald Sie über die neueste Version von Azure PowerShell verfügen, müssen Sie zunächst ``Connect-AzureRmAccount`` ausführen.

``` PowerShell
# To check if your subscription is eligible to adjust pricing models.
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Ein Ergebnis von TRUE unter isGrandFatherableSubscription zeigt an, dass das Preismodell dieses Abonnements auf ein anders Preismodell umgestellt werden kann. Das Fehlen eines Werts unter optedInDate bedeutet, dass dieses Abonnement zurzeit auf das alte Preismodell festgelegt ist.

```
isGrandFatherableSubscription optedInDate
----------------------------- -----------
                         True            
```

Führen Sie zum Migrieren dieses Abonnements zum neuen Preismodell Folgendes aus:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
```

Führen Sie Folgendes erneut aus, um zu überprüfen, ob die Änderung erfolgreich war:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Wenn die Migration erfolgreich war, sollte das Ergebnis wie folgt aussehen:

```
isGrandFatherableSubscription optedInDate                      
----------------------------- -----------                      
                         True 2018-05-31T13:52:43.3592081+00:00
```

optInDate enthält nun einen Zeitstempel, der angibt, wann dieses Abonnement das neue Preismodell abonniert hat.

Wenn Sie das alte Preismodell wiederherstellen müssen, führen Sie Folgendes aus:

```PowerShell
 $ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action rollbacktolegacypricingmodel `
 -Force
```

Wenn Sie dann das vorherige Skript, das ``-Action listmigrationdate`` enthält, erneut ausführen, sollten Sie nun einen leeren optedInDate Wert sehen, der angibt, dass Ihr Abonnement auf das alte Preismodell zurückgesetzt wurde.

Wenn Sie über mehrere Abonnements verfügen, die Sie migrieren möchten und die unter demselben Mandanten gehostet werden, können Sie Ihre eigene Variante mit Hilfe von Teilen der folgenden Skripts erstellen:

```PowerShell
#Query tenant and create an array comprised of all of your tenants subscription ids
$TenantId = <Your-tenant-id>
$Tenant =Get-AzureRMSubscription -TenantId $TenantId
$Subscriptions = $Tenant.Id
```

Um zu überprüfen, ob alle Abonnements in Ihrem Mandanten für das neue Preismodell berechtigt sind, können Sie Folgendes ausführen:

```PowerShell
Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
}
```

Das Skript kann weiter durch Erstellen eines Skripts verfeinert werden, das drei Arrays generiert. Ein Array besteht aus allen Abonnement-IDs, für die ```isGrandFatherableSubscription``` auf TRUE festgelegt ist und optedInDate zurzeit keinen Wert aufweist. Ein zweites Array besteht aus allen Abonnements, für die zurzeit das neue Preismodell verwendet wird. Ein drittes Array wird nur mit Abonnement-IDs in Ihrem Mandanten aufgefüllt, die für das neue Preismodell nicht geeignet sind:

```PowerShell
[System.Collections.ArrayList]$Eligible= @{}
[System.Collections.ArrayList]$NewPricingEnabled = @{}
[System.Collections.ArrayList]$NotEligible = @{}

Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
$Result= Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force

     if ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $False)
     {
     $Eligible.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $True)
     {
     $NewPricingEnabled.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $False)
     {
     $NotEligible.add($id)
     }
}
```

> [!NOTE]
> Abhängig von der Anzahl der Abonnements kann die Ausführung des oben aufgeführten Skripts einige Zeit in Anspruch nehmen. Aufgrund der Verwendung der .add()-Methode zeigt das PowerShell-Fenster das Inkrementieren von Werten an, wenn jedem Array Elemente hinzugefügt werden.

Da Sie Ihre Abonnements nun auf die drei Arrays aufgeteilt haben, sollten Sie die Ergebnisse sorgfältig überprüfen. Möglicherweise möchten Sie eine Sicherungskopie der Inhalte des Arrays erstellen, damit Sie die Änderungen auf einfache Weise rückgängig machen können, wenn dies in der Zukunft erforderlich sein sollte. Wenn Sie sich entschieden haben, dass Sie alle geeigneten Abonnements, die zurzeit das alte Preismodell verwenden, in das neue Preismodell konvertieren möchten, können Sie die diese Aufgabe nun folgendermaßen erledigen:

```PowerShell
Foreach ($id in $Eligible)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
}

```
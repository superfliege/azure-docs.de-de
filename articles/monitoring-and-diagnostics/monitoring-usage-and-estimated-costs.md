---
title: Überwachen der Nutzung und geschätzten Kosten in Azure Monitor | Microsoft-Dokumentation
description: Übersicht über die Verwendung der Azure Monitor-Seite für die Nutzung und geschätzten Kosten
author: dalekoetke
manager: carmonmills
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: Dale.Koetke;mbullwin
ms.openlocfilehash: f25c39b602449be3ab9d1cd7e67d6fcfc78afb17
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
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

Im April 2018 wurde ein neues Preismodell für die Überwachung eingeführt. Es handelt sich um für die Cloud optimierte und verbrauchsbasierte Preise. Sie zahlen ausschließlich nutzungsabhängig und gehen keine knotenbasierten Verpflichtungen ein. Die Details des neuen Preismodells sind jetzt für [Metriken, Warnungen, Benachrichtigungen](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) und [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) verfügbar.

Für Kunden, die mit der Verwendung von Log Analytics oder Application Insights nach dem 2. April 2018 begonnen haben, ist das neue Preismodell die einzige Option. Für Kunden, die diese Dienste bereits nutzen, ist die Umstellung auf das neue Preismodell optional.

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

Kunden, die Microsoft Operations Management Suite E1 und E2 erworben haben, verfügen über Datenerfassungsberechtigungen pro Knoten für [Log Analytics](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite) und [Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-pricing#the-price-plans). Um diese Berechtigungen für Log Analytics-Arbeitsbereiche oder Application Insights-Ressourcen in einem bestimmten Abonnement zu erhalten, muss für dieses Abonnement das Preismodell für den Zeitraum vor April 2018 weiterverwendet werden. In diesem Modell sind der Log Analytics-Tarif „Pro Knoten (OMS)“ und der Application Insights-Tarif „Enterprise“ verfügbar. Je nach Anzahl von Knoten einer Suite, die von Ihrer Organisation erworben wurde, kann die Umstellung von Abonnements auf das neue Preismodell trotzdem vorteilhaft sein. Dieser Schritt bedarf aber sorgfältiger Überlegung.

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Änderungen bei der Umstellung auf das neue Preismodell

Beim Umstellen eines Abonnements auf das neue Preismodell ändert sich der Tarif für jede Log Analytics-Instanz in einen neuen Tarif pro GB, und alle Ressourcen werden umgestellt (in Azure Resource Manager als „pergb2018“ bezeichnet). Im Rahmen dieser Umstellung werden auch alle Application Insights-Ressourcen des Enterprise-Tarifs in den Basic-Tarif geändert. Die Kostenschätzung zeigt die Auswirkungen dieser Änderungen.

## <a name="moving-to-the-new-pricing-model"></a>Durchführen der Umstellung auf das neue Preismodell

Wenn Sie sich entschieden haben, für ein Abonnement die Umstellung auf das neue Preismodell durchzuführen, können Sie oben auf der Seite **Nutzung und geschätzte Kosten** die Option **Preismodellauswahl** auswählen:

![Screenshot: Überwachen der Nutzung und der geschätzten Kosten im neuen Preismodell](./media/monitoring-usage-and-estimated-costs/006.png)

Die Seite **Preismodellauswahl** wird geöffnet. Sie zeigt eine Liste aller Abonnements, die Sie auf der vorherigen Seite angezeigt haben:

![Screenshot: Preismodellauswahl](./media/monitoring-usage-and-estimated-costs/007.png)

Aktivieren Sie einfach das jeweilige Kontrollkästchen, und wählen Sie **Speichern**, um ein Abonnement auf das neue Preismodell umzustellen. Auf die gleiche Weise können Sie auch wieder zurück zum alten Preismodell wechseln. Beachten Sie hierbei, dass die Berechtigung „Besitzer“ oder „Mitwirkender“ für das Abonnement erforderlich ist, um das Preismodell zu ändern.

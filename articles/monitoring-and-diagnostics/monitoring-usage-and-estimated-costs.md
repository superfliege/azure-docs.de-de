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
ms.date: 04/02/2018
ms.author: Dale.Koetke;mbullwin
ms.openlocfilehash: f87705ebbdd14b1bbf7cade481a7dbe7dd3d5131
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Überwachen der Nutzung und der geschätzten Kosten

Die Seite **Nutzung und geschätzte Kosten** im Monitor-Hub des Azure-Portals soll Informationen zur Nutzung wichtiger Überwachungsfeatures liefern, z.B. [Metriken, Warnungen, Benachrichtigungen](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) und [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Für Kunden mit Tarifen, die vor April 2018 verfügbar waren, umfasst dies auch die Log Analytics-Nutzung, die über das Insights- und Analytics-Angebot erworben wurde.

Auf dieser Seite können Benutzer die Nutzung dieser Ressourcen in den letzten 31 Tagen nach Abonnement sortiert anzeigen und den Nutzungstrend für diesen Zeitraum ermitteln. Für diese Schätzung müssen viele Daten zusammengefasst werden, sodass Sie beim Laden der Seite etwas Geduld aufbringen müssen.
Hier ist ein Beispiel für die Anzeige der Nutzungsüberwachung und eine Schätzung der anfallenden Kosten angegeben:

![Screenshot: „Nutzung und geschätzte Kosten“ im Portal](./media/monitoring-usage-and-estimated-costs/001.png)

Wenn Sie in der Spalte „Monatliche Nutzung“ auf den Link klicken, wird ein Diagramm mit den Nutzungstrends für die letzten 31 Tage angezeigt:

![Screenshot: „Pro Knoten enthalten“ – 671,47 GB](./media/monitoring-usage-and-estimated-costs/002.png)

Hier ist eine weitere ähnliche Zusammenfassung der Nutzung und der Kosten dargestellt. Hierbei geht es um ein Abonnement für das neue verbrauchsbasierte Preismodell ab April 2018. Beachten Sie, dass keine knotenbasierte Abrechnung vorhanden ist und dass die Informationen zur Datenerfassung und -aufbewahrung für Log Analytics und Application Insights jetzt über eine neue gemeinsame Anzeige angegeben werden.

![Screenshot: „Nutzung und geschätzte Kosten“ im Portal](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Neues Preismodell

Im April 2018 wurde ein neues Preismodell für die Überwachung eingeführt.  Es handelt sich hierbei um für die Cloud optimierte und verbrauchsbasierte Preise. Sie zahlen ausschließlich nutzungsabhängig und gehen keine knotenbasierten Verpflichtungen ein. Die Details des neuen Preismodells sind jetzt für [Metriken, Warnungen, Benachrichtigungen](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) und [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) verfügbar.

Für Kunden, die das Onboarding für Log Analytics oder Application Insights nach dem 2. April 2018 durchführen, ist das neue Preismodell die einzige Option. Für Kunden, die diese Dienste bereits genutzt haben, ist die Umstellung auf das neue Preismodell optional.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Auswirkungen des neuen Preismodells

Das neue Preismodell hat für jeden Kunden je nach Nutzungsmuster der Überwachung unterschiedliche Auswirkungen. Für Kunden, die Log Analytics oder Application Insights bereits vor dem 2. April 2018 verwendet haben, bietet die Seite **Nutzung und geschätzte Kosten** in Azure Monitor die Möglichkeit, bei einer Umstellung auf das neue Preismodell die Änderung der Kosten zu schätzen. Außerdem kann ein Abonnement auf das neue Modell umgestellt werden. Für die meisten Kunden führt das neue Preismodell zu Vorteilen, aber für Kunden mit besonders hoher Datennutzung oder in Regionen mit höheren Kosten ist dies unter Umständen nicht der Fall.

Klicken Sie oben auf der Seite auf das blaue Banner, um eine Schätzung Ihrer Kosten für die Abonnements anzuzeigen, die Sie auf der Seite **Nutzung und geschätzte Kosten** ausgewählt haben. Es ist am besten, dies für Abonnements einzeln nacheinander durchzuführen, da dies die Ebene ist, auf der das neue Preismodell übernommen werden kann.

![Screenshot: Preismodellauswahl](./media/monitoring-usage-and-estimated-costs/004.png)

Hier ist eine ähnliche Version dieser Seite mit einem grünen Banner angegeben:

![Screenshot: Preismodellauswahl](./media/monitoring-usage-and-estimated-costs/005.png)

Hier sehen Sie andere Anzeigen, die für das neue Preismodell verwendet werden. Beispielsweise wurden folgende Anzeigen für die Datenerfassung zusammengefasst:

1. Insight & Analytics\Überschreitung pro Knoten
2. Insight & Analytics\Pro Knoten enthalten
3. Application Insights\Basic-Überschreitungsdaten
4. Application Insights\Enthaltene Daten

Diese Anzeigen wurden in einer neuen gemeinsamen Anzeige für die Datenerfassung mit dem Namen **Gemeinsame Dienste\Datenerfassung** kombiniert. Der Grund ist, dass das neue Preismodell keine knotenbasierten Datenzuordnungen enthält.

Eine weitere Änderung besteht darin, dass Daten, die in Log Analytics oder Application Insights in Regionen mit höheren Kosten erfasst werden, mit den neuen regionalen Anzeigen dargestellt werden, um dies richtig widerzuspiegeln, z.B. **Datenerfassung (USA, Westen-Mitte)**.

> [!NOTE]
> Wenn Sie über ein OMS-Abonnement (Operations Management Suite) verfügen, erhalten Sie Log Analytics- und Application Insights-Datenerfassungszuordnungen für jeden erworbenen Knoten. Da dies auf Kontoebene angewendet wird (nicht auf Abonnementebene), kann für diese Schätzung die Auswirkung dieser Zuordnungen nicht angezeigt werden. Wenden Sie sich in diesem Fall an Ihren Kontobeauftragten, um ausführlichere Informationen zum neuen Preismodell zu erhalten.

## <a name="changes-when-moving-to-the-new-pricing-model"></a>Änderungen bei der Umstellung auf das neue Preismodell

Beim Umstellen eines Abonnements auf das neue Preismodell ändert sich der Tarif für jede Log Analytics-Instanz in einen neuen Tarif pro GB, und alle Ressourcen werden umgestellt (in Azure Resource Manager als „pergb2018“ bezeichnet). Im Rahmen dieser Umstellung werden auch alle Application Insights-Ressourcen des Enterprise-Tarifs in den Basic-Tarif geändert. Die Auswirkungen dieses Vorgangs sind oben im Abschnitt zur Kostenschätzung beschrieben. 

## <a name="moving-to-the-new-pricing-model"></a>Durchführen der Umstellung auf das neue Preismodell

Wenn Sie sich entschieden haben, für ein Abonnement die Umstellung auf das neue Preismodell durchzuführen, können Sie oben auf der Seite **Nutzung und geschätzte Kosten** auf die Option **Preismodellauswahl** klicken:

![Screenshot: Überwachen der Nutzung und der geschätzten Kosten im neuen Preismodell](./media/monitoring-usage-and-estimated-costs/006.png)

Die Seite **Preismodellauswahl** wird geöffnet und enthält die einzelnen Abonnements, die auf der vorherigen Seite angezeigt wurden:

![Screenshot: Preismodellauswahl](./media/monitoring-usage-and-estimated-costs/007.png)

Aktivieren Sie einfach das jeweilige Kontrollkästchen, und klicken Sie auf **Speichern**, um ein Abonnement auf das neue Preismodell umzustellen.  Auf die gleiche Weise können Sie auch wieder zurück zum alten Preismodell wechseln. Beachten Sie hierbei, dass die Berechtigung „Besitzer“ oder „Mitwirkender“ für das Abonnement erforderlich ist, um das Preismodell zu ändern.
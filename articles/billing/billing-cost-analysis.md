---
title: Ermitteln von Azure-Kosten mithilfe der Kostenanalyse | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie sich mit der Kostenanalyse einen Überblick über Azure-Kosten für Ihre Organisation verschaffen und wie Sie diese Kosten analysieren.
services: billing
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/16/2018
ms.topic: conceptual
ms.service: billing
manager: dougeby
ms.custom: ''
ms.openlocfilehash: eeaf02853f8ffe9ca67dbf31afc687afb7dee242
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180571"
---
# <a name="explore-and-analyze-costs-with-cost-analysis"></a>Ermitteln und Analysieren von Kosten mit der Kostenanalyse

Voraussetzung für die Kontrolle und Optimierung von Azure-Kosten sind Kenntnisse über die Kostenstellen in Ihrer Organisation. Außerdem sollte bekannt sein, wie hoch die Kosten für Ihre Dienste sind und für welche Umgebungen und Systeme diese Aufwendungen genutzt werden. Der Überblick über sämtliche Kosten ist entscheidend, um Ausgabestrukturen Ihrer Organisation genau nachvollziehen zu können. Mithilfe dieser Strukturen können Kontrollmechanismen wie Budgets eingesetzt werden.

In diesem Artikel ermitteln Sie mithilfe der Kostenanalyse Azure-Kosten, die für Ihre Organisation anfallen, und analysieren diese. Durch die Anzeige aggregierter Organisationskosten können Sie Kostenstellen und Ausgabentrends nachvollziehen. Mithilfe der akkumulierten Kosten lassen sich unter Berücksichtigung von Budgets Kostenschätzungen für monatliche, vierteljährliche oder jährliche Zeiträume erstellen. Budgets unterstützen Sie dabei, Ausgabengrenzen einzuhalten oder tägliche oder monatliche Kosten einzusehen, wodurch Sie Unregelmäßigkeiten bei Ausgaben gesondert identifizieren können. Des Weiteren können Sie die Daten des aktuellen Berichts herunterladen, um diese noch genauer zu analysieren oder sie in externen Systemen zu verwenden.

## <a name="requirements"></a>Anforderungen

Die Kostenanalyse steht allen EA-Kunden (Enterprise Agreement) zur Verfügung. Zum Aufrufen von Kostendaten benötigen Sie für mindestens einen der folgenden Bereiche Lesezugriff:

- Azure EA-Abrechnungskonto (Registrierung)
- Azure EA-Abonnement
- Ressourcengruppe innerhalb eines Azure EA-Abonnements

## <a name="review-costs-in-cost-analysis"></a>Überprüfen von Kosten mithilfe der Kostenanalyse

Gehen Sie wie folgt vor, um mit der Kostenanalyse eine Kostenüberprüfung durchzuführen: Öffnen Sie das Azure-Portal, und navigieren Sie zu **Kostenverwaltung + Abrechnung** &gt; **Abrechnungskonten**. Wählen Sie dann zuerst unter „Cost Management“ Ihr EA-Abrechnungskonto und anschließend **Kostenanalyse** aus.

Die Kostenanalyseansicht umfasst anfänglich die folgenden Bereiche:

**Gesamt:** Gesamtkosten im aktuellen Monat.

**Budget:** geplantes Ausgabenlimit für den ausgewählten Bereich (falls verfügbar).

**Akkumulierte Kosten:** akkumulierte Ausgaben auf Tagesbasis ab Monatsbeginn. Wenn Sie für Ihr Abrechnungskonto oder Abonnement ein [Budget erstellt haben](billing-cost-management-budget-scenario.md#create-the-azure-budget), können Sie sich Ausgabentrends im Verhältnis zum Budget anzeigen lassen. Zeigen Sie auf ein Datum, um sich die akkumulierten Kosten bis zu einem bestimmten Tag anzusehen.

**Pivot-/Ringdiagramme:** dynamische Pivotdiagramme. Mit diesen werden Gesamtkosten nach gemeinsamen Standardeigenschaften aufgeschlüsselt. In den Diagrammen sind die akkumulierten Kosten für den aktuellen Monat absteigend sortiert. Sie können die Pivotdiagramme jederzeit anpassen, indem Sie einen anderen Pivotbereich auswählen. Kosten werden standardmäßig nach Dienst (Kategorie für Messung), Standort (Region) und untergeordnetem Bereich (beispielsweise Registrierungskonten unter Abrechnungskonten, Ressourcengruppen unter Abonnements und Ressourcen unter Ressourcengruppen) aufgeschlüsselt.

![Anfängliche Kostenanalyseansicht](./media/billing-cost-analysis/cost-analysis-01.png)



## <a name="customizing-cost-views"></a>Anpassen von Kostenansichten

Mit der Standardansicht können Sie beispielsweise folgende häufig gestellte Fragen beantworten:

- Wie hoch sind meine Ausgaben?
- Wird mein Budget überschritten?

Es gibt jedoch viele Fälle, in denen eine ausführlichere Analyse erforderlich ist. Wählen Sie zur Anpassung der Ansicht oben auf der Seite den Datumsbereich aus.

In der Kostenanalyse werden standardmäßig die Daten für den aktuellen Monat angezeigt. Mit dem Auswahlelement können Sie zwischen dem letzten Monat, dem aktuellen Monat, dem aktuellen Kalenderquartal, dem aktuellen Kalenderjahr und dem benutzerdefinierten Datumsbereich wechseln. Am schnellsten analysieren Sie die letzten Azure-Rechnungen und stimmen Kosten ab, indem Sie den letzten Monat auswählen. Wenn Sie das aktuelle Quartal oder Jahr auswählen, können Sie Kosten im Verhältnis zu langfristigen Budgets überwachen. Sie haben außerdem die Möglichkeit, kürzere oder längere Datumsbereiche festzulegen, die beispielsweise ein Tag, sieben Tage oder sogar maximal ein Jahr vor dem aktuellen Monat liegen.

![Auswahlelement für den Datumsbereich](./media/billing-cost-analysis/date-selector.png)

Standardmäßig werden in der Kostenanalyse auch **akkumulierte** Kosten angezeigt. Diese umfassen die Kosten pro Tag und die summierten Kosten der Vortage. Dadurch wird der Anstieg der akkumulierten Kosten pro Tag visualisiert. In dieser Ansicht wird besonders gut dargestellt, in welchem Verhältnis Ausgabentrends und Budget innerhalb eines festgelegten Zeitbereichs stehen.

Des Weiteren ist auch eine **Tagesansicht** verfügbar. In dieser werden die Kosten für die einzelnen Tage aufgeführt. Der Wachstumstrend wird dabei nicht visualisiert. Die Tagesansicht eignet sich zur Anzeige von Unregelmäßigkeiten wie einem überdurchschnittlich starken Kostenanstieg oder -abfall zwischen einzelnen Tagen. Wenn Sie ein Budget auswählen, wird in der Tagesansicht auch das geschätzte Tagesbudget angezeigt. Wenn die täglichen Kosten dauerhaft über dem Tagesbudget liegen, ist davon auszugehen, dass Ihr Monatsbudget überschritten wird. Das geschätzte Tagesbudget ist lediglich ein Hilfsmittel, mit dem Sie Ihr Budget auf einer granulareren Ebene visualisieren können. Wenn bei täglichen Kosten Schwankungen auftreten, ist der Vergleich von geschätztem Tagesbudget und Monatsbudget ungenauer.

![Tagesansicht](./media/billing-cost-analysis/daily-view.png)

Durch die Auswahl der Option **Gruppieren nach** können Sie eine Gruppenkategorie auswählen. Dadurch werden für das Gesamtkostendiagramm im oberen Bereich die Daten angepasst. Mithilfe einer Gruppierung können Sie Ihre Ausgaben leicht nach Ressourcentyp kategorisieren. Auf der folgenden Abbildung werden Azure-Kosten visualisiert, die im letzten Monat angefallen sind.

![Tagesansicht für akkumulierte Kosten mit Gruppierungen](./media/billing-cost-analysis/grouped-daily-accum-view.png)

Pivotdiagramme befinden sich in der Ansicht „Gesamt“ im oberen Bereich. Mit diesen können Sie sich Ansichten für unterschiedliche Gruppierungen und Filterkategorien anzeigen lassen. Wenn Sie eine Gruppenkategorie auswählen, werden alle Daten für die Ansicht „Gesamt“ im unteren Bereich angezeigt. Auf der folgenden Abbildung ist ein Beispiel für Ressourcengruppen zu sehen.

![Alle Daten für aktuelle Ansicht](./media/billing-cost-analysis/full-data-set.png)

### <a name="download-cost-analysis-data"></a>Herunterladen von Kostenanalysedaten

Wenn Sie Kostenanalysedaten **herunterladen**, wird eine CSV-Datei für alle Daten erstellt, die derzeit im Azure-Portal angezeigt werden. Wenn Sie Filter oder Gruppierungen verwenden, sind diese in der Datei enthalten. Einige der für die Ansicht „Gesamt“ im oberen Bereich zugrunde liegenden Daten, die nicht aktiv im Portal angezeigt werden, sind auch in der CSV-Datei enthalten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Dokumentation zur Abrechnungs- und Kostenverwaltung in Azure](billing-cost-management-budget-scenario.md).

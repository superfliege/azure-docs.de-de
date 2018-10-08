---
title: 'Schnellstart: Ermitteln von Azure-Kosten mithilfe der Kostenanalyse | Microsoft-Dokumentation'
description: In diesem Schnellstart wird beschrieben, wie Sie sich mit der Kostenanalyse einen Überblick über Azure-Kosten für Ihre Organisation verschaffen und wie Sie diese Kosten analysieren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018s
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 9092629c7bef46cdb7c464fca5e22d4aea0da9fc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041539"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Schnellstart: Ermitteln und Analysieren von Kosten mit der Kostenanalyse

Voraussetzung für die Kontrolle und Optimierung von Azure-Kosten sind Kenntnisse über die Kostenstellen in Ihrer Organisation. Außerdem sollte bekannt sein, wie hoch die Kosten für Ihre Dienste sind und für welche Umgebungen und Systeme diese Aufwendungen genutzt werden. Der Überblick über sämtliche Kosten ist entscheidend, um Ausgabestrukturen Ihrer Organisation genau nachvollziehen zu können. Ausgabestrukturen können verwendet werden, um Kontrollmechanismen wie Budgets zu erzwingen.

In diesem Schnellstart ermitteln Sie mithilfe der Kostenanalyse Azure-Kosten, die für Ihre Organisation anfallen, und analysieren diese. Durch die Anzeige aggregierter Kosten nach Organisationen können Sie ermitteln, wo Kosten im Zeitverlauf anfallen, und Ausgabentrends nachvollziehen. Mithilfe der akkumulierten Kosten können Sie unter Berücksichtigung von Budgets Kostenschätzungen für monatliche, vierteljährliche oder jährliche Zeiträume erstellen. Ein Budget hilft, finanziellen Zwänge einzuhalten. Mithilfe eines Budgets können Sie zudem die täglichen oder monatlichen Kosten anzeigen, um Ausgabenunregelmäßigkeiten zu vermeiden. Des Weiteren können Sie die Daten des aktuellen Berichts herunterladen, um diese noch genauer zu analysieren oder sie in externen Systemen zu verwenden.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

- Überprüfen von Kosten mithilfe der Kostenanalyse
- Anpassen von Kostenansichten
- Herunterladen von Kostenanalysedaten


## <a name="prerequisites"></a>Voraussetzungen

Die Kostenanalyse steht allen [EA-Kunden (Enterprise Agreement)](https://azure.microsoft.com/pricing/enterprise-agreement/) zur Verfügung. Zum Aufrufen von Kostendaten benötigen Sie für einen oder mehrere der folgenden Bereiche mindestens Lesezugriff:

- Abrechnungskonto
- Department
- Registrierungskonto
- Verwaltungsgruppe
- Abonnement
- Ressourcengruppe


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

- Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="review-costs-in-cost-analysis"></a>Überprüfen von Kosten mithilfe der Kostenanalyse

Um Ihre Kosten mithilfe der Kostenanalyse zu überprüfen, navigieren Sie im Azure-Portal zu **Kostenverwaltung + Abrechnung** &gt; **Kostenverwaltung** &gt; **Bereich ändern**, wählen Sie einen Bereich aus, und klicken Sie dann auf **Auswählen**.

Der von Ihnen ausgewählte Bereich wird in der gesamten Kostenverwaltung verwendet, um Daten zu konsolidieren und den Zugriff auf Kosteninformationen zu steuern. Wenn Sie Bereiche verwenden, wählen Sie diese nicht mehrfach aus. Wählen Sie stattdessen einen größeren Bereich aus, in dem andere zusammengefasst werden, und filtern Sie diesen dann nach Ihren Wünschen. Dies sollten Sie verstehen, da einige Personen keinen Zugriff auf einen übergeordneten Bereich haben sollten, in den die untergeordneten Bereiche zusammengefasst werden.

Klicken Sie auf **Kostenanalyse öffnen**.

Die Kostenanalyseansicht umfasst anfänglich die folgenden Bereiche:

**Gesamt:** Gesamtkosten im aktuellen Monat.

**Budget:** geplantes Ausgabenlimit für den ausgewählten Bereich (falls verfügbar).

**Akkumulierte Kosten:** akkumulierte Ausgaben auf Tagesbasis ab Monatsbeginn. Nachdem Sie für Ihr Abrechnungskonto oder Abonnement [ein Budget erstellt haben](tutorial-acm-create-budgets.md), können Sie sich Ausgabentrends für Ihr Budget anzeigen lassen. Zeigen Sie auf ein Datum, um die akkumulierten Kosten bis zu einem bestimmten Tag anzuzeigen.

**Pivot-/Ringdiagramme** – Stellen Sie dynamische Pivotdiagramme bereit, die die Gesamtkosten nach einem gemeinsamen Satz von Standardeigenschaften aufschlüsseln. Darin sind die akkumulierten Kosten für den aktuellen Monat absteigend sortiert. Sie können die Pivotdiagramme jederzeit anpassen, indem Sie einen anderen Pivotbereich auswählen. Die Kosten werden standardmäßig nach Dienst (Kategorie der Verbrauchseinheit), Standort (Region) und untergeordnetem Bereich kategorisiert. Beispielsweise Registrierungskonten unter Abrechnungskonten, Ressourcengruppen unter Abonnements und Ressourcen unter Ressourcengruppen.

![Anfängliche Kostenanalyseansicht](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Anpassen von Kostenansichten

Mit der Standardansicht können Sie beispielsweise folgende häufig gestellte Fragen beantworten:

- Wie hoch sind meine Ausgaben?
- Wird mein Budget überschritten?

Es gibt jedoch viele Fälle, in denen eine ausführlichere Analyse erforderlich ist. Wählen Sie zur Anpassung der Ansicht oben auf der Seite den Datumsbereich aus.

In der Kostenanalyse werden standardmäßig die Daten für den aktuellen Monat angezeigt. Mit dem Auswahlelement können Sie zwischen dem letzten Monat, dem aktuellen Monat, dem aktuellen Kalenderquartal, dem aktuellen Kalenderjahr und dem benutzerdefinierten Datumsbereich wechseln. Am schnellsten analysieren Sie die letzten Azure-Rechnungen und stimmen Kosten ab, indem Sie den letzten Monat auswählen. Wenn Sie das aktuelle Quartal oder Jahr auswählen, können Sie Kosten im Verhältnis zu langfristigen Budgets überwachen. Sie können auch einen anderen Datumsbereich auswählen. Sie können z. B. einen einzigen Tag, sieben Tage oder sogar maximal ein Jahr vor dem aktuellen Monat auswählen.

![Auswahlelement für den Datumsbereich](./media/quick-acm-cost-analysis/date-selector.png)

Standardmäßig werden in der Kostenanalyse **akkumulierte** Kosten angezeigt. Diese umfassen die Kosten pro Tag und die summierten Kosten der Vortage. Dadurch wird der Anstieg der akkumulierten Kosten pro Tag visualisiert. In dieser Ansicht wird besonders gut dargestellt, in welchem Verhältnis Ausgabentrends und Budget innerhalb eines festgelegten Zeitbereichs stehen.

Es gibt auch die **Tagesansicht**, die die Kosten für jeden Tag anzeigt. Die Tagesansicht zeigt keinen Wachstumstrend an. Die Ansicht ist so konzipiert, dass Unregelmäßigkeiten als Kostenspitzen oder -einbrüche von Tag zu Tag dargestellt werden. Wenn Sie ein Budget ausgewählt haben, wird in der Tagesansicht auch das geschätzte Tagesbudget angezeigt. Wenn die täglichen Kosten dauerhaft über dem Tagesbudget liegen, ist davon auszugehen, dass Ihr Monatsbudget überschritten wird. Das geschätzte Tagesbudget ist lediglich ein Hilfsmittel, mit dem Sie Ihr Budget auf einer granulareren Ebene visualisieren können. Wenn bei täglichen Kosten Schwankungen auftreten, ist der Vergleich von geschätztem Tagesbudget und Monatsbudget ungenauer.

![Tagesansicht](./media/quick-acm-cost-analysis/daily-view.png)

Durch die Auswahl der Option **Gruppieren nach** können Sie eine Gruppenkategorie auswählen. Dadurch werden für das Gesamtkostendiagramm im oberen Bereich die Daten angepasst. Mithilfe einer Gruppierung können Sie Ihre Ausgaben leicht nach Ressourcentyp kategorisieren. Auf der folgenden Abbildung werden Azure-Kosten visualisiert, die im letzten Monat angefallen sind.

![Tagesansicht für akkumulierte Kosten mit Gruppierungen](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Pivotdiagramme unter der Ansicht „Gesamt“ im oberen Bereich zeigen Ansichten für verschiedene Gruppierungs- und Filterkategorien. Wenn Sie eine Gruppenkategorie auswählen, werden alle Daten für die Ansicht „Gesamt“ im unteren Bereich angezeigt. Auf der folgenden Abbildung ist ein Beispiel für Ressourcengruppen zu sehen.

![Alle Daten für aktuelle Ansicht](./media/quick-acm-cost-analysis/full-data-set.png)

Das vorhergehende Bild zeigt die Namen der Ressourcengruppen. Anzeigetags für Ressourcen sind in keiner der Kostenanalysesichten, Filter oder Gruppierungen verfügbar.

## <a name="download-cost-analysis-data"></a>Herunterladen von Kostenanalysedaten

Sie können Kostenanalysedaten **herunterladen**, um eine CSV-Datei für alle Daten zu erstellen, die derzeit im Azure-Portal angezeigt werden. Alle Filter oder Gruppierungen, die Sie anwenden, sind in der Datei enthalten. Die der Ansicht „Gesamt“ im oberen Bereich zugrunde liegenden Daten, die nicht aktiv angezeigt werden, sind in der CSV-Datei enthalten.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Tutorial erfahren Sie, wie Sie Budgets erstellen und verwalten.

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Budgets](tutorial-acm-create-budgets.md)

---
title: 'Schnellstart: Ermitteln von Azure-Kosten mithilfe der Kostenanalyse | Microsoft-Dokumentation'
description: In diesem Schnellstart wird beschrieben, wie Sie sich mit der Kostenanalyse einen Überblick über Azure-Kosten für Ihre Organisation verschaffen und wie Sie diese Kosten analysieren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/07/2019
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: cb07ce71162a766add5ca251c97a11d353ee8084
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077656"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Schnellstart: Ermitteln und Analysieren von Kosten mit der Kostenanalyse

Voraussetzung für die Kontrolle und Optimierung von Azure-Kosten sind Kenntnisse über die Kostenstellen in Ihrer Organisation. Außerdem sollte bekannt sein, wie hoch die Kosten für Ihre Dienste sind und für welche Umgebungen und Systeme diese Aufwendungen genutzt werden. Der Überblick über sämtliche Kosten ist entscheidend, um Ausgabestrukturen Ihrer Organisation genau nachvollziehen zu können. Ausgabestrukturen können verwendet werden, um Kontrollmechanismen wie Budgets zu erzwingen.

In diesem Schnellstart ermitteln Sie mithilfe der Kostenanalyse Azure-Kosten, die für Ihre Organisation anfallen, und analysieren diese. Durch die Anzeige aggregierter Kosten nach Organisationen können Sie ermitteln, wo Kosten im Zeitverlauf anfallen, und Ausgabentrends nachvollziehen. Mithilfe der akkumulierten Kosten können Sie unter Berücksichtigung von Budgets Kostenschätzungen für monatliche, vierteljährliche oder jährliche Zeiträume erstellen. Ein Budget trägt zur Einhaltung finanzieller Vorgaben bei. Mithilfe eines Budgets können Sie zudem die täglichen oder monatlichen Kosten anzeigen, um Ausgabenunregelmäßigkeiten zu vermeiden. Des Weiteren können Sie die Daten des aktuellen Berichts herunterladen, um diese noch genauer zu analysieren oder sie in externen Systemen zu verwenden.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

- Überprüfen von Kosten mithilfe der Kostenanalyse
- Anpassen von Kostenansichten
- Herunterladen von Kostenanalysedaten


## <a name="prerequisites"></a>Voraussetzungen

Die Kostenanalyse steht allen [EA-Kunden (Enterprise Agreement)](https://azure.microsoft.com/pricing/enterprise-agreement/) zur Verfügung. Zum Aufrufen von Kostendaten benötigen Sie für einen oder mehrere der folgenden Bereiche mindestens Lesezugriff. Weitere Informationen zum Zuweisen des Zugriffs auf Cost Management-Daten finden Sie unter [Assign access to Cost Management data](assign-access-acm-data.md) (Zuweisen des Zugriffs auf Cost Management-Daten).

- Abrechnungskonto
- Department
- Registrierungskonto
- Verwaltungsgruppe
- Abonnement
- Ressourcengruppe

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

- Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="review-costs-in-cost-analysis"></a>Überprüfen von Kosten mithilfe der Kostenanalyse

Um Ihre Kosten mithilfe der Kostenanalyse zu überprüfen, navigieren Sie im Azure-Portal zu **Kostenverwaltung + Abrechnung** &gt; **Kostenverwaltung** &gt; **Bereich ändern**, wählen Sie einen Bereich aus, und klicken Sie dann auf **Auswählen**.

Der von Ihnen ausgewählte Bereich wird in der gesamten Kostenverwaltung verwendet, um Daten zu konsolidieren und den Zugriff auf Kosteninformationen zu steuern. Wenn Sie Bereiche verwenden, wählen Sie diese nicht mehrfach aus. Wählen Sie stattdessen einen größeren Bereich aus, in dem andere zusammengefasst werden, und filtern Sie diesen dann nach Ihren Wünschen. Hiermit sollten Sie sich unbedingt vertraut machen, da einige Personen keinen Zugriff auf einen übergeordneten Bereich haben sollten, unter dem untergeordnete Bereiche zusammengefasst sind.

Klicken Sie auf **Kostenanalyse öffnen**.

Die Kostenanalyseansicht umfasst anfänglich die folgenden Bereiche:

**Gesamt:** Gesamtkosten im aktuellen Monat.

**Budget:** geplantes Ausgabenlimit für den ausgewählten Bereich (falls verfügbar).

**Akkumulierte Kosten:** akkumulierte Ausgaben auf Tagesbasis ab Monatsbeginn. Nachdem Sie für Ihr Abrechnungskonto oder Abonnement [ein Budget erstellt haben](tutorial-acm-create-budgets.md), können Sie sich Ausgabentrends für Ihr Budget anzeigen lassen. Zeigen Sie auf ein Datum, um die akkumulierten Kosten bis zu einem bestimmten Tag anzuzeigen.

**Pivot-/Ringdiagramme** – Stellen Sie dynamische Pivotdiagramme bereit, die die Gesamtkosten nach einem gemeinsamen Satz von Standardeigenschaften aufschlüsseln. Darin sind die akkumulierten Kosten für den aktuellen Monat absteigend sortiert. Sie können die Pivotdiagramme jederzeit anpassen, indem Sie einen anderen Pivotbereich auswählen. Die Kosten werden standardmäßig nach Dienst (Kategorie der Verbrauchseinheit), Standort (Region) und untergeordnetem Bereich kategorisiert. Beispielsweise Registrierungskonten unter Abrechnungskonten, Ressourcengruppen unter Abonnements und Ressourcen unter Ressourcengruppen.

![Anfängliche Ansicht der Kostenanalyse im Azure-Portal](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Anpassen von Kostenansichten

Mit der Standardansicht können Sie beispielsweise folgende häufig gestellte Fragen beantworten:

- Wie hoch sind meine Ausgaben?
- Wird mein Budget überschritten?

Es gibt jedoch viele Fälle, in denen eine ausführlichere Analyse erforderlich ist. Wählen Sie zur Anpassung der Ansicht oben auf der Seite den Datumsbereich aus.

In der Kostenanalyse werden standardmäßig die Daten für den aktuellen Monat angezeigt. Mit dem Auswahlelement können Sie zwischen dem letzten Monat, dem aktuellen Monat, dem aktuellen Kalenderquartal, dem aktuellen Kalenderjahr und dem benutzerdefinierten Datumsbereich wechseln. Am schnellsten analysieren Sie die letzten Azure-Rechnungen und stimmen Kosten ab, indem Sie den letzten Monat auswählen. Wenn Sie das aktuelle Quartal oder Jahr auswählen, können Sie Kosten im Verhältnis zu langfristigen Budgets überwachen. Sie können auch einen anderen Datumsbereich auswählen. Sie können z. B. einen einzigen Tag, sieben Tage oder sogar maximal ein Jahr vor dem aktuellen Monat auswählen.

![Das Auswahlelement für den Datumsbereich zeigt eine Beispielauswahl für diesen Monat](./media/quick-acm-cost-analysis/date-selector.png)

Standardmäßig werden in der Kostenanalyse **akkumulierte** Kosten angezeigt. Diese umfassen die Kosten pro Tag und die summierten Kosten der Vortage. Dadurch wird der Anstieg der akkumulierten Kosten pro Tag visualisiert. In dieser Ansicht wird besonders gut dargestellt, in welchem Verhältnis Ausgabentrends und Budget innerhalb eines festgelegten Zeitbereichs stehen.

Es gibt auch die **Tagesansicht**, die die Kosten für jeden Tag anzeigt. Die Tagesansicht zeigt keinen Wachstumstrend an. Die Ansicht ist so konzipiert, dass Unregelmäßigkeiten als Kostenspitzen oder -einbrüche von Tag zu Tag dargestellt werden. Wenn Sie ein Budget ausgewählt haben, wird in der Tagesansicht auch das geschätzte Tagesbudget angezeigt. Wenn die täglichen Kosten dauerhaft über dem Tagesbudget liegen, ist davon auszugehen, dass Ihr Monatsbudget überschritten wird. Das geschätzte Tagesbudget ist lediglich ein Hilfsmittel, mit dem Sie Ihr Budget auf einer granulareren Ebene visualisieren können. Wenn bei täglichen Kosten Schwankungen auftreten, ist der Vergleich von geschätztem Tagesbudget und Monatsbudget ungenauer.

Im Allgemeinen können Sie erwarten, dass Daten oder Benachrichtigungen zu verbrauchten Ressourcen innerhalb von acht Stunden angezeigt werden.

![Tägliche Anzeige mit einem Beispiel täglicher Kosten für den aktuellen Monat](./media/quick-acm-cost-analysis/daily-view.png)

Durch die Auswahl der Option **Gruppieren nach** können Sie eine Gruppenkategorie auswählen. Dadurch werden für das Gesamtkostendiagramm im oberen Bereich die Daten angepasst. Mit Gruppierungen können Sie auf einen Blick erkennen, wie Ihre Ausgaben anhand allgemeiner Ressourcen- und Nutzungseigenschaften wie Ressourcengruppe oder Ressourcentags kategorisiert wurden. Wählen Sie zum Gruppieren nach Tags den Tagschlüssel aus, nach dem Sie gruppieren möchten. Sie sehen, dass für jeden Wert dieses Tags die Kosten aufgeschlüsselt sind. Es ist auch ein zusätzliches Segment für Ressourcen vorhanden, auf die dieses Tag nicht angewendet wird.

Die meisten [Azure-Ressourcen unterstützen das Tagging](../azure-resource-manager/tag-support.md), aber einige Tags sind in Cost Management und für die Abrechnung nicht verfügbar. Darüber hinaus werden Ressourcengruppentags nicht unterstützt. Cost Management unterstützt nur Ressourcentags ab dem Datum, an dem die Tags direkt auf die Ressource angewendet werden.

Auf der folgenden Abbildung werden Azure-Kosten visualisiert, die im letzten Monat angefallen sind.

![Gruppierte tägliche akkumulierte Ansicht mit Beispiel für Azure-Dienstkosten für den letzten Monat](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Pivotdiagramme unterhalb des Hauptdiagramms enthalten verschiedene Gruppierungen und bieten damit einen allgemeinen Überblick über die Gesamtkosten für den ausgewählten Zeitraum und die ausgewählten Filter. Wählen Sie eine Eigenschaft oder ein Tag aus, um aggregierte Kosten anhand beliebiger Dimensionen anzuzeigen. Alle Daten für die Ansicht „Gesamt“ werden im unteren Bildschirmbereich angezeigt. Erweitern Sie dazu den Bereich **Daten**, oder wählen Sie oben auf dem Bildschirm **Exportieren > CSV herunterladen** aus. Hier sehen Sie ein Beispiel des Datenbereichs für Ressourcengruppen.

![Vollständige Daten für die aktuelle Ansicht mit Ressourcengruppennamen](./media/quick-acm-cost-analysis/full-data-set.png)

Das vorhergehende Bild zeigt die Namen der Ressourcengruppen. Sie können zum Anzeigen der Gesamtkosten pro Tag nach Tag gruppieren. Es gibt jedoch in keiner der Kostenanalyseansichten ein Option zum Anzeigen aller Tags pro Ressource oder Ressourcengruppe.

Beim Gruppieren von Kosten nach einem bestimmten Attribut wird die Top-Ten-Liste der Kostenverursacher angezeigt (von den höchsten zu den niedrigsten Kosten). Wenn mehr als zehn Gruppen vorhanden sind, werden die obersten neun Kostenverursacher angezeigt. Außerdem wird die Gruppe **Andere** angezeigt, unter der alle verbleibenden Gruppen zusammengefasst sind. Beim Gruppieren nach Tags wird unter Umständen eine Gruppe vom Typ **Ohne Markierungen** für Kosten angezeigt, auf die der Tagschlüssel nicht angewendet wurde. **Keine Markierungen** wird immer zuletzt angezeigt, selbst wenn mehr Kosten ohne Markierungen als Kosten mit Markierungen vorhanden sind. Sind mindestens zehn Tagwerte vorhanden, werden Kosten ohne Markierungen unter **Andere** angezeigt.

Für *klassische* virtuelle Computer (Azure Service Management, ASM), Netzwerke und Speicherressourcen werden keine ausführlichen Abrechnungsdaten bereitgestellt. Sie werden beim Gruppieren der Kosten als **Classic services** (Klassische Dienste) gruppiert.


## <a name="download-cost-analysis-data"></a>Herunterladen von Kostenanalysedaten

Sie können Kostenanalysedaten **herunterladen**, um eine CSV-Datei für alle Daten zu erstellen, die derzeit im Azure-Portal angezeigt werden. Alle Filter oder Gruppierungen, die Sie anwenden, sind in der Datei enthalten. Die der Ansicht „Gesamt“ im oberen Bereich zugrunde liegenden Daten, die nicht aktiv angezeigt werden, sind in der CSV-Datei enthalten.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Tutorial erfahren Sie, wie Sie Budgets erstellen und verwalten.

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Budgets](tutorial-acm-create-budgets.md)

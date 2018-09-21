---
title: Verwalten von Budgets in Azure Cost Management | Microsoft-Dokumentation
description: Dieser Artikel unterstützt Sie beim Erstellen und Verwalten von Budgets in Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/25/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 106e8f082d148ed9a8c58313177be81ee074a2c3
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578462"
---
# <a name="manage-budgets"></a>Verwalten von Budgets

Die Einrichtung von Budgets und budgetbasierten Warnmeldungen hilft Ihnen, Ihre Cloudgovernance und Verantwortlichkeit zu verbessern. Mit diesem Artikel können Sie Budgets in Cost Management schnell erstellen und verwalten.

Wenn Sie ein Unternehmenskonto oder MSP-Konto haben, können Sie Ihre hierarchische Kostenentitätsstruktur verwenden, um monatliche Budgetkontingente verschiedenen Geschäftseinheiten, Abteilungen oder anderen Kostenentitäten zuzuordnen. Wenn Sie ein Premium-Konto haben, können Sie die Budgetverwaltung nutzen, die dann auf Ihre gesamten Cloudausgaben angewendet wird. Alle Budgets werden manuell zugewiesen.

Auf der Grundlage der zugewiesenen Budgets können Sie Schwellenwertwarnungen basierend auf dem prozentualen Anteil des verbrauchten Budgets festlegen und den Schweregrad der einzelnen Schwellenwerte festlegen.

Das zugewiesene Budget wird im Budgetbericht angezeigt. Benutzer können sehen, wann ihre Ausgaben über, unter oder auf dem Niveau ihres Verbrauchs im Laufe der Zeit liegen. Wenn Sie oben im Budgetbericht **Felder ein-/ausblenden** auswählen, können Sie die Kosten, das Budget, die akkumulierten Kosten und das Gesamtbudget anzeigen.

## <a name="create-budgets"></a>Erstellen von Budgets

Wenn Sie ein Budget erstellen, legen Sie es für Ihr Geschäftsjahr fest, sodass es für eine bestimmte Entität gilt.

So erstellen Sie ein Budget und weisen es einer Entität zu:

1. Navigieren Sie zu **Kosten** &gt; **Cost Management** &gt; **Budget**.
2. Wählen Sie auf der Seite „Budgetverwaltung“ unter **Entität** die Entität, in der Sie das Budget erstellen möchten.
3. Wählen Sie unter „Budgetjahr“ das Jahr, für das Sie das Budget erstellen möchten.
4. Legen Sie für jeden Monat einen Budgetwert fest. Wenn Sie fertig sind, klicken Sie auf **Speichern**.
In diesem Beispiel wurde das Monatsbudget für Juni 2018 auf 135.000 USD festgelegt. Das Gesamtbudget für das Jahr beläuft sich auf 1.615.000,00 USD.
![Budget erstellen](./media/manage-budgets/set-budget.png)


So importieren Sie eine Datei für das Jahresbudget:

1. Wählen Sie unter **Aktionen** die Option **Export**, um eine leere CSV-Vorlage als Basis für Ihr Budget herunterzuladen.
2. Tragen Sie in die CSV-Datei Ihre Budgeteinträge ein, und speichern Sie sie lokal.
3. Wählen Sie unter **Aktionen** die Option **Import**.
4. Wählen Sie Ihre gespeicherte Datei aus, und klicken Sie auf **OK**.

Um Ihr abgeschlossenes Budget als CSV-Datei zu exportieren, wählen Sie unter **Aktionen** die Option **Export**, um die Datei herunterzuladen.

## <a name="view-budget-in-reports"></a>Anzeigen des Budgets in Berichten

Nach Abschluss wird Ihr Budget im Großteil der Kostenberichte unter **Kosten** &gt; **Kostenanalyse** und im Bericht mit der Gegenüberstellung der Kosten und des Budgets im Lauf der Zeit aufgeführt. Unter **Aktionen** können Sie auch Berichte basierend auf Budgetschwellenwerten planen.

Hier ist ein Beispiel für einen Kostenanalysebericht. Er zeigt das Gesamtbudget und die Kosten nach Workload und Verwendungstyp seit Jahresbeginn.

![Beispiel für Kostenanalysebericht mit Budget](./media/manage-budgets/cost-analysis-budget-example.png)

Im folgenden Beispiel wird der 22. Juni als aktuelles Datum angenommen. Die Kosten für Juni 2018 belaufen sich auf 71.611,28 USD im Vergleich zum monatlichen Budget von 135.000 USD. Die Kosten sind deutlich niedriger als das monatliche Budget, da es noch acht Tage bis zum Monatsende gibt.

Eine weitere Möglichkeit, den Bericht anzuzeigen, ist ein Vergleich der akkumulierten Kosten mit Ihrem Budget. Um die akkumulierten Kosten anzuzeigen, wählen Sie **Felder ein-/ausblenden**, anschließend **Akkumulierte Kosten** und **Gesamtbudget**. Hier ist ein Beispiel für die akkumulierten Kosten seit Anfang des Jahres.

![Akkumuliertes Budget](./media/manage-budgets/accumulated-budget.png)

Irgendwann in der Zukunft könnten Ihre akkumulierten Kosten Ihr Budget übersteigen. Sie können das leichter erkennen, wenn Sie die Diagrammansicht auf den Typ _Zeile_ ändern.

![Darstellung des Budgets als Liniendiagramm](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Erstellen von Budgetwarnungen für einen Filter

Im vorherigen Beispiel sehen Sie, dass sich die akkumulierten Kosten dem Budget angenähert haben. Sie können automatische Budgetwarnungen erstellen, sodass Sie benachrichtigt werden, wenn die Ausgaben Ihr Budget erreichen oder überschreiten. Grundsätzlich ist eine Warnung ein geplanter Bericht mit einem Schwellenwert. Budgetwarnschwellenwerte beinhalten:

- Restkosten im Vergleich zum Budget – zum Angeben des Schwellenwerts für den Währungswert
- Kosten-Prozentanteil im Vergleich zum Budget – zum Angeben eines Schwellenwerts für den Prozentwert

Schauen wir uns ein Beispiel an.

Klicken Sie im Bericht mit dem Vergleich der Kosten und des Budgets im Laufe der Zeit auf **Aktionen** , und wählen Sie **Bericht planen** aus. Wählen Sie auf der Registerkarte „Schwellenwert“ eine Schwellenwertmetrik aus. Zum Beispiel **Kosten-Prozentanteil im Vergleich zum Budget**. Wählen Sie einen Warnungstypen, und geben Sie einen Prozentwert für das Budget ein. Wenn Sie nur einmalig benachrichtigt werden möchten, geben Sie unter **Anzahl der aufeinanderfolgenden Warnungen** _1_ ein. Klicken Sie auf **Speichern**.

![Budgetwarnung](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Nächste Schritte

- Falls Sie das erste Tutorial zu Cost Management noch nicht abgeschlossen haben, lesen Sie es unter [Überprüfen der Nutzung und der Kosten](https://docs.microsoft.com/azure/cost-management/tutorial-review-usage).
- Erfahren Sie mehr über die [in Cost Management verfügbaren Berichte](use-reports.md).

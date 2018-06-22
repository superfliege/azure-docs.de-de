---
title: Verwenden von Cost Management-Berichten in Azure Cost Management | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie verschiedene Cost Management-Berichte im Cloudyn-Portal verwenden.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 1a2067228629b04f1ef2cc407a28f379acc75f60
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297514"
---
# <a name="use-cost-management-reports"></a>Verwenden von Cost Management-Berichten

In diesem Artikel wird beschrieben, wie Sie verschiedene Cost Management-Berichte im Cloudyn-Portal verwenden. Die meisten Cloudyn-Berichte sind intuitiv und weisen ein einheitliches Erscheinungsbild auf. Eine Übersicht über Cloudyn-Berichte finden Sie unter [Grundlagen zu Kostenberichten](understanding-cost-reports.md). In diesem Artikel werden zudem verschiedene in den meisten Berichten verwendete Optionen und Felder beschrieben.

## <a name="cost-analysis-reports"></a>Berichte zur Kostenanalyse

In Berichten zur Kostenanalyse werden Abrechnungsdaten von Ihren Cloudanbietern angezeigt. Mithilfe der Berichte können Sie verschiedene in der Abrechnungsdatei aufgeführte Datensegmente gruppieren und detailliert anzeigen. Die Berichte ermöglichen eine differenzierte Navigation durch die Kosten in den Abrechnungsrohdaten der Cloudanbieter.

Bei der Anzeige als Diagramm haben Analyseberichte eine Y-Achse *Kosten* und eine X-Achse für eine *Kategorie oder ein Tag*, die bzw. das Sie auswählen. Obwohl das Diagramm über einen Bereich verfügt, beispielsweise die letzten 30 Tage, wird der Bereich als Filter verwendet. Analyseberichte sind ideal zum Vergleichen von Kategorien. Beispielsweise können die Kosten jedes Abonnements in den letzten 30 Tagen verglichen werden oder die Kosten für Speicher- oder Computedienste über einen bestimmten Zeitraum.

In Berichten zur Kostenanalyse werden Kosten nicht nach Tags gruppiert. Die tagbasierte Berichterstellung ist nur in den Berichten zur Kostenzuteilung verfügbar, nachdem Sie mit „Cost Allocation 360“ ein Kostenmodell erstellt haben.

### <a name="actual-cost-analysis"></a>Analyse der Ist-Kosten

Im Bericht „Actual Cost Analysis“ (Analyse der Ist-Kosten) werden die hauptsächlichen Kostenbeiträge angezeigt, einschließlich der laufenden Kosten und einmaligen Gebühren.

 Sie können den Bericht „Actual Cost Analysis“ (Analyse der Ist-Kosten) für Folgendes verwenden:

- Analysieren und Überwachen der innerhalb eines angegebenen Zeitraums aufgewendeten Ist-Kosten
- Planen einer Schwellenwertwarnung
- Analysieren von Showback- und Chargeback-Kosten

#### <a name="to-use-the-actual-cost-analysis-report"></a>So verwenden Sie den Bericht „Actual Cost Analysis“ (Analyse der Ist-Kosten)

Führen Sie mindestens die folgenden Schritte aus. Sie können auch andere Optionen und Felder verwenden.

1. Wählen Sie einen Datumsbereich aus.
2. Wählen Sie einen Filter aus.

Sie können mit der rechten Maustaste auf die Berichtsergebnisse klicken und weitere detaillierte Informationen anzeigen.

![Beispiel für Bericht „Actual Cost Analysis“ (Analyse der Ist-Kosten)](./media/use-reports/actual-cost-analysis.png)

### <a name="actual-cost-over-time"></a>Actual Cost Over Time (Ist-Kosten im Zeitverlauf)

Der Bericht „Actual Cost Over Time“ (Ist-Kosten im Zeitverlauf) ist ein Standardbericht zur Kostenanalyse, in dem Kosten über eine definierte Zeitauflösung verteilt werden. Im Bericht werden Ausgaben im Zeitverlauf angezeigt, sodass Sie Trends beobachten und Unregelmäßigkeiten bei den Kosten erkennen können. In diesem Bericht werden die hauptsächlichen Kostenbeiträge, einschließlich der laufenden Kosten und einmaligen Gebühren für reservierte Instanzen, angezeigt, die innerhalb eines ausgewählten Zeitraums aufgewendet werden.

Bei der Anzeige als Diagramm verfügen Berichte, die die Kosten im Zeitverlauf darstellen, über eine Y-Achse *Kosten* und eine X-Achse *Zeitraum*. Diese Berichte zeigen die Kostenentwicklung im Zeitverlauf.

Sie können den Bericht „Actual Cost Over Time“ (Ist-Kosten im Zeitverlauf) für Folgendes verwenden:

- Anzeigen von Kostentrends über einen bestimmten Zeitraum
- Suchen von Unregelmäßigkeiten bei den Kosten
- Anzeigen aller kostenbezogenen Belange in Bezug auf Amazon Web Services

#### <a name="to-use-the-actual-cost-over-time-report"></a>So verwenden Sie den Bericht „Actual Cost Over Time“ (Ist-Kosten im Zeitverlauf)

Führen Sie mindestens die folgenden Schritte aus. Sie können auch andere Optionen und Felder verwenden.

- Wählen Sie einen Datumsbereich aus.

Sie können beispielsweise Gruppen auswählen, um die entsprechenden Kosten im Zeitverlauf anzuzeigen. Dann können Sie Filter hinzufügen, um die Ergebnisse einzugrenzen.

![Beispiel für Bericht „Actual Cost Over Time“ (Ist-Kosten im Zeitverlauf)](./media/use-reports/actual-cost-over-time.png)



### <a name="amortized-cost-reports"></a>Berichte zu amortisierten Kosten

In dieser Gruppe von Berichten zu amortisierten Kosten werden linearisierte nicht nutzungsbasierte Dienstgebühren oder einmalig zu zahlende Kosten angezeigt und diese Kosten gleichmäßig während ihrer Lebensdauer aufgeteilt.

Beispiele für einmalige Gebühren:

- Jährliche Supportgebühren
- Jährliche Gebühren für Sicherheitskomponenten
- Gebühren für den Kauf von reservierten Instanzen
- Einige Azure Marketplace-Elemente

In der Abrechnungsdatei werden einmalige Gebühren gekennzeichnet, wenn das Start- und das Enddatum oder die Zeitstempel der Dienstnutzung gleiche Werte aufweisen. Diese werden dann in Cloudyn als einmalige Gebühren erkannt, die amortisiert werden können. Andere nutzungsbasierte Dienste mit Kosten für bedarfsgesteuerte Nutzung können nicht amortisiert werden.

Sehen Sie sich zur Veranschaulichung von amortisierten Kosten das folgende Beispiel für den Bericht „Actual Cost Over Time“ (Ist-Kosten im Zeitverlauf) an. In diesem Beispiel wird am 23. August ein Kostenanstieg verzeichnet. Dies kann im Vergleich zu den sonst üblichen täglichen Kosten auf eine Unregelmäßigkeit hindeuten. Durch eine Ursachenanalyse und die Navigation durch die Daten ergibt sich, dass diese Kosten für die jährliche APN-Reservierung des AWS-Diensts anfallen. Dabei handelt es sich um eine einmalige Gebühr, die an diesem Tag gezahlt und abgerechnet wurde. Im nächsten Abschnitt können Sie sehen, wie diese Kosten amortisiert werden.

![Beispiel für Bericht „Actual Cost Over Time“ (Ist-Kosten im Zeitverlauf) mit einmaligen Kosten](./media/use-reports/actual-amort-example.png)

#### <a name="to-use-the-amortized-cost-over-time-report"></a>So verwenden Sie den Bericht „Amortized Cost Over Time“ (Amortisierte Kosten im Zeitverlauf)

Führen Sie mindestens die folgenden Schritte aus. Sie können auch andere Optionen und Felder verwenden.

1. Wählen Sie einen Datumsbereich aus.
2. Wählen Sie einen Dienst und einen Anbieter aus.

In der Weiterführung des vorherigen Beispiels können Sie in der folgenden Abbildung sehen, dass die einmaligen Kosten nun amortisiert sind:

![Beispiel für Bericht „Amortized Cost Over Time“ (Amortisierte Kosten im Zeitverlauf)](./media/use-reports/amort-cost-over-time.png)

In dieser Abbildung sind die amortisierten Kosten für die APN-Reservierung im Zeitverlauf dargestellt. In diesem Bericht werden die Amortisierung der einmaligen Gebühren und die APN-Kosten als Kauf der jährlichen Reservierung angezeigt. Die APN-Kosten werden gleichmäßig anteilig pro Tag auf 1/365 der Anschaffungskosten für die Reservierung aufgeteilt.

## <a name="cost-allocation-analysis-reports"></a>Berichte zur Analyse der Kostenzuteilung

Berichte zur Analyse der Kostenzuteilung sind verfügbar, nachdem Sie mit „Cost Allocation 360“ ein Kostenmodell erstellt haben. In Cloudyn werden Kosten- und Abrechnungsdaten verarbeitet und die Daten mit den Nutzungs- und Tagdaten Ihrer Cloudkonten abgeglichen. Für den Abgleich der Daten benötigt Cloudyn Zugriff auf Ihre Nutzungsdaten. Konten ohne Anmeldeinformationen werden als nicht kategorisierte Ressourcen gekennzeichnet.

### <a name="cost-analysis-report"></a>Bericht „Kostenanalyse“

Der Bericht „Cost Analysis“ (Kostenanalyse) bietet Einblick in die Cloudnutzung und die für die Cloud anfallenden Kosten innerhalb eines ausgewählten Zeitraums. Die im Cost Allocation Manager festgelegten Richtlinien werden im Bericht „Cost Analysis“ (Kostenanalyse) verwendet.

Wie wird dieser Bericht in Cloudyn berechnet?

In Cloudyn wird durch Anwenden der Kontenaffinität sichergestellt, dass bei der Kostenzuteilung die Integrität der einzelnen verknüpften Konten beibehalten wird. Durch die Affinität wird sichergestellt, dass einem Konto, das einen bestimmten Dienst nicht verwendet, keine Kosten für diesen Dienst zugeordnet werden. Die in diesem Konto anfallenden Kosten verbleiben im Konto und werden nicht über die Zuordnungsrichtlinien berechnet. Angenommen, Sie verfügen über fünf verknüpfte Konten. Wenn nur in drei dieser Konten Speicherdienste verwendet werden, werden die Kosten für Speicherdienste nur Tags in den drei Konten zugeteilt.

 Sie können den Bericht „Cost Analysis“ (Kostenanalyse) für Folgendes verwenden:

- Anzeigen einer aggregierten Ansicht Ihrer gesamten Bereitstellung für einen bestimmten Zeitraum
- Anzeigen der Kosten nach Tagkategorien basierend auf den im Kostenmodell erstellten Richtlinien

#### <a name="to-use-the-cost-analysis-report"></a>So verwenden Sie den Bericht „Cost Analysis“ (Kostenanalyse)

1. Wählen Sie einen Datumsbereich aus.
2. Fügen Sie nach Bedarf Tags hinzu.
3. Fügen Sie Gruppen hinzu.
4. Wählen Sie ein Kostenmodell aus, das Sie zuvor erstellt haben.

In der folgenden Abbildung ist ein Beispiel für den Bericht „Cost Analysis“ (Kostenanalyse) im Sunburst-Format dargestellt. In den Ringen werden jeweils Gruppen angezeigt. Im äußeren Ring werden die Dienste und im inneren Ring die Einheiten angezeigt.

![Beispiel für Bericht „Cost Analysis“ (Kostenanalyse)](./media/use-reports/cost-analysis01.png)



Es folgt ein Beispiel für die gleichen Informationen in einer Tabellenansicht.

![Beispiel für Bericht „Cost Analysis“ (Kostenanalyse)](./media/use-reports/cost-analysis02.png)



### <a name="cost-over-time-report"></a>Bericht für die Kosten im Lauf der Zeit

Im Bericht „Cost Over Time“ (Kosten im Zeitverlauf) werden Ausgaben im Zeitverlauf angezeigt, sodass Sie Trends beobachten und Unregelmäßigkeiten in Ihrer Bereitstellung erkennen können. Im Wesentlichen werden die über einen definierten Zeitraum verteilten Kosten angezeigt. Der Bericht enthält die hauptsächlichen Kostenbeiträge, einschließlich der laufenden Kosten und einmaligen Gebühren für reservierte Instanzen, die innerhalb eines ausgewählten Zeitraums aufgewendet werden. In diesem Bericht können die in Cost Manager 360 festgelegten Richtlinien verwendet werden.

Sie können den Bericht „Cost Over Time“ (Kosten im Zeitverlauf) für Folgendes verwenden:

- Anzeigen von Änderungen im Zeitverlauf und der Einwirkungen, die sich von einem Tag (oder Datumsbereich) auf den anderen ändern
- Analysieren der Kosten im Zeitverlauf für eine bestimmte Instanz
- Analysieren der Ursachen für den Kostenanstieg für eine bestimmte Instanz

#### <a name="to-use-the-cost-over-time-report"></a>So verwenden Sie den Bericht „Cost Over Time“ (Kosten im Zeitverlauf)

1. Wählen Sie einen Datumsbereich aus.
2. Fügen Sie nach Bedarf Tags hinzu.
3. Fügen Sie Gruppen hinzu.
4. Wählen Sie ein Kostenmodell aus, das Sie zuvor erstellt haben.
5. Wählen Sie Ist-Kosten oder amortisierte Kosten aus.
6. Legen Sie fest, ob Zuordnungsregeln angewendet werden, um die Ansicht der Abrechnungsrohdaten anzuzeigen oder um Kosten in der Ansicht neu zu berechnen.

Es folgt ein Beispiel für den Bericht.

![Beispiel für den Bericht „Cost Over Time“ (Kosten im Zeitverlauf)](./media/use-reports/cost-over-time.png)



## <a name="next-steps"></a>Nächste Schritte

- Falls Sie das erste Tutorial zu Kostenverwaltung noch nicht abgeschlossen haben, lesen Sie es unter [Überprüfen der Nutzung und der Kosten](tutorial-review-usage.md).

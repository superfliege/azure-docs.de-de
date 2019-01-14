---
title: Diagnose und Problembehandlung in Azure Time Series Insights (Vorschauversion) | Microsoft-Dokumentation
description: Machen Sie sich mit der Diagnose und Problembehandlung mit Azure Time Series Insights Preview vertraut.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: e44a02e2126e7b25f69999de34fe5cf42499a90c
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556831"
---
# <a name="diagnose-and-troubleshoot"></a>Diagnostizieren und Behandeln von Problemen

In diesem Artikel werden einige häufig auftretende Probleme zusammengefasst, die auftreten können, wenn Sie mit Ihrer Azure Time Series Insights Preview-Umgebung arbeiten. Darüber hinaus werden mögliche Ursachen und Lösungen für jedes der Probleme beschrieben.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Problem: Ich finde meine Umgebung im Times Series Insights Preview-Explorer nicht

Dieses Problem kann auftreten, wenn Sie nicht über die Berechtigung für den Zugriff auf die Time Series Insights-Umgebung verfügen. Benutzer benötigen eine Zugriffsrolle auf Leser-Ebene, um ihre Time Series Insights-Umgebung anzuzeigen. Um die aktuellen Zugriffsebenen zu überprüfen und zusätzlichen Zugriff zu gewähren, besuchen Sie den Abschnitt „Datenzugriffsrichtlinien“ in der Time Series Insights-Ressource im [Azure-Portal](https://portal.azure.com/).

  ![Environment][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Problem: Es werden keine Daten im Time Series Insights Preview-Explorer angezeigt

Es gibt verschiedene mögliche Gründe, aus denen Ihre Daten im [Azure Time Series Insights Preview-Explorer](https://insights.timeseries.azure.com/preview) nicht angezeigt werden.

- Ihre Ereignisquelle empfängt möglicherweise keine Daten.

    Stellen Sie sicher, dass Ihre Ereignisquelle, bei der es sich um einen Event Hub oder einen IoT-Hub handelt, Daten von Ihren Tags oder Instanzen empfängt. Um dies zu überprüfen, wechseln Sie zur Übersichtsseite Ihrer Ressourcen im Azure-Portal.

    ![Dashboard-Erkenntnisse][2]

- Ihre Ereignisquelldaten sind nicht im JSON-Format.

    Time Series Insights unterstützt nur JSON-Daten. JSON-Beispiele finden Sie unter [Unterstützte JSON-Formen](./how-to-shape-query-json.md).

- Dem Schlüssel Ihrer Ereignisquelle fehlt eine erforderliche Berechtigung.

    * Für IoT Hub müssen Sie einen Schlüssel mit der Berechtigung **Dienstverbindung** bereitstellen.

    ![Konfiguration][3]

    * Wie in der vorherigen Abbildung dargestellt, würden beide Richtlinien **iothubowner** und **service** funktionieren, da sie die Berechtigung **Dienstverbindung** besitzen.
    * Für einen Event Hub müssen Sie einen Schlüssel mit der Berechtigung **Lauschen** bereitstellen.
  
    ![Berechtigungen][4]

    * Wie in der vorherigen Abbildung dargestellt, würden beide Richtlinien **read** und **write** funktionieren, da sie die Berechtigung **Lauschen** besitzen.

- Ihre bereitgestellte Consumergruppe ist für Time Series Insights nicht exklusiv.

    Während der Registrierung eines IoT-Hubs oder eines Event Hubs geben Sie die Consumergruppe an, die zum Lesen der Daten verwendet wird. Teilen Sie diese Consumergruppe nicht. Wenn die Consumergruppe geteilt wird, trennt der zugrunde liegende Event Hub automatisch einen der Leser nach dem Zufallsprinzip. Stellen Sie eine eindeutige Consumergruppe bereit, aus der Time Series Insights lesen soll.

- Ihre Time Series-ID-Eigenschaft, die zum Zeitpunkt der Bereitstellung angegeben wurde, ist falsch, fehlt oder ist null.

    Dieses Problem kann auftreten, wenn die Time Series-ID-Eigenschaft zum Zeitpunkt der Bereitstellung der Umgebung fehlerhaft konfiguriert ist. Weitere Informationen finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](./time-series-insights-update-how-to-id.md). Zu diesem Zeitpunkt können Sie eine vorhandene Time Series Insights-Umgebung nicht so aktualisieren, dass sie eine andere Time Series-ID verwendet.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Problem: Einige Daten werden angezeigt, andere hingegen fehlen.

Möglicherweise werden Sie Daten ohne die Time Series-ID.

- Dieses Problem kann auftreten, wenn Sie Ereignisse ohne das Feld „Time Series-ID“ in der Nutzlast senden. Weitere Informationen finden Sie unter [Unterstützte JSON-Formen](./how-to-shape-query-json.md).

- Dieses Problem kann auftreten, weil Ihre Umgebung gedrosselt wird.

    > [!NOTE]
    > Zurzeit unterstützt Time Series Insights eine maximale Datenerfassungsrate von 6 Mbit/s.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problem: Die Einstellung für den Namen der Timestamp-Eigenschaft meiner Ereignisquelle funktioniert nicht.

Stellen Sie sicher, dass Name und Wert den folgenden Regeln entsprechen:

* Der Name der Timestamp-Eigenschaft beachtet Groß-/Kleinschreibung.
* Der Wert für die Timestamp-Eigenschaft, der als JSON-Zeichenfolge von Ihrer Ereignisquelle eingeht, hat das Format `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Ein Beispiel für eine solche Zeichenfolge ist `“2008-04-12T12:53Z”`.

Sie können am einfachsten sicherstellen, dass Ihr Name der Timestamp-Eigenschaft erfasst wird und richtig funktioniert, indem Sie den Time Series Insights-Explorer verwenden. Wählen Sie im Time Series Insights-Explorer mithilfe des Diagramms einen Zeitraum aus, nachdem Sie den Namen der Timestamp-Eigenschaft angegeben haben. Klicken Sie mit der rechten Maustaste auf die Auswahl, und wählen Sie die Option **Ereignisse untersuchen** aus. Die erste Spaltenüberschrift ist der Name Ihrer Timestamp-Eigenschaft. Er sollte neben dem Wort `Timestamp` die Zeichenfolge `($ts)` aufweisen, statt:

* `(abc)`, was anzeigt, dass Time Series Insights die Datenwerte als Zeichenfolgen liest.
* Kalendersymbol, das anzeigt, dass Time Series Insights die Datenwerte als Datum/Uhrzeit-Wert liest.
* `#`, was anzeigt, dass Time Series Insights die Datenwerte als ganze Zahl liest.

Wenn die Timestamp-Eigenschaft nicht explizit angegeben ist, wird der Zeitpunkt der Einreihung eine Ereignisses in die Warteschlange für einen IoT-Hub oder Event Hub als Standardzeitstempel verwendet.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Problem: Ich kann mein Zeitreihenmodell weder bearbeiten noch anzeigen

- Möglicherweise greifen Sie auf eine Time Series Insights S1- oder S2-Umgebung zu.

   Zeitreihenmodelle werden nur in PAYG-Umgebungen unterstützt. Weitere Informationen zum Zugriff auf Ihre S1/S2-Umgebung über den Time Series Insights Preview-Explorer finden Sie unter [Visualisieren von Daten im Explorer](./time-series-insights-update-explorer.md).

   ![Access][5]

- Möglicherweise besitzen Sie keine Berechtigungen zum Anzeigen und Bearbeiten des Modells.

   Benutzer benötigen Zugriff auf Mitwirkender-Ebene, um ihre Zeitreihenmodelle zu bearbeiten und anzuzeigen. Um die aktuellen Zugriffsebenen zu überprüfen und zusätzlichen Zugriff zu gewähren, besuchen Sie den Abschnitt „Datenzugriffsrichtlinien“ in Ihrer Time Series Insights-Ressource im Azure-Portal.

## <a name="problem-all-my-instances-in-the-time-series-insights-preview-explorer-dont-have-a-parent"></a>Problem: Keine meiner Instanzen im Times Series Insights Preview-Explorer hat ein übergeordnetes Element

Dieses Problem kann auftreten, wenn in Ihrer Umgebung keine Zeitreihenmodell-Hierarchie definiert ist. Weitere Informationen finden Sie unter [Arbeiten mit Zeitreihenmodellen](./time-series-insights-update-how-to-tsm.md).

  ![Zeitreihenmodelle][6]

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Arbeiten mit Zeitreihenmodellen](./time-series-insights-update-how-to-tsm.md).
- Lesen Sie [Unterstützte JSON-Formen](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png
---
title: Erstellen einer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie eine neue Time Series Insights-Umgebung über das Azure-Portal erstellen."
services: time-series-insights
ms.service: time-series-insights
author: op-ravi
ms.author: omravi
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 6dba703851161a1eebce0101be8076682f09c76f
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Erstellen einer neuen Time Series Insights-Umgebung über das Azure-Portal
In diesem Artikel wird beschrieben, wie Sie eine neue Time Series Insights-Umgebung über das Azure-Portal erstellen.

Time Series Insights ermöglicht Ihnen den Einstieg in die Visualisierung und Abfrage des Dateneingangs in Azure IoT Hubs und Event Hubs in wenigen Minuten, sodass Sie große Mengen an Zeitreihendaten innerhalb von Sekunden abfragen können.  Dieser Dienst wurde für die Skalierung des Internets der Dinge (Internet of Things, IoT) entwickelt und kann Daten im Terabytebereich verarbeiten.

## <a name="steps-to-create-the-environment"></a>Schritte zum Erstellen der Umgebung
Führen Sie zum Erstellen einer Umgebung die folgenden Schritte aus:

1.  Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2.  Wählen Sie die Schaltfläche **+ Neu** aus.

3.  Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus.

   ![Erstellen der Time Series Insights-Umgebung](media/time-series-insights-get-started/1-new-tsi.png)

4.  Wählen Sie auf der Seite **Time Series Insights** die Option **Erstellen** aus.

5. Füllen Sie die erforderlichen Parameter aus. In der folgenden Tabelle werden die einzelnen Parameter erläutert:
   
   ![Erstellen der Time Series Insights-Ressourcengruppe](media/time-series-insights-get-started/2-create-tsi.png)
   
   Einstellung|Empfohlener Wert|Beschreibung
   ---|---|---
   Umgebungsname | Ein eindeutiger Name | Unter diesem Namen wird die Umgebung im [Time Series-Explorer](https://insights.timeseries.azure.com) dargestellt.
   Abonnement | Ihr Abonnement | Wenn Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, das die Ereignisquelle vorzugsweise enthält. Time Series Insights kann Azure IoT Hub- und Event Hub-Ressourcen im gleichen Abonnement automatisch erkennen.
   Ressourcengruppe | Erstellen einer neuen oder Verwenden einer vorhandenen Ressourcengruppe | Eine Ressourcengruppe ist eine Sammlung von Azure-Ressourcen, die zusammen verwendet werden. Sie können eine vorhandene Ressourcengruppe auswählen, z.B. die Gruppe, die Ihren Event Hub oder IoT Hub enthält. Oder Sie können eine neue Ressourcengruppe erstellen, wenn die Ressource nicht mit den anderen Ressourcen verbunden ist.
   Standort | Am nächsten zu Ihrer Ereignisquelle gelegen | Wählen Sie vorzugsweise denselben Rechenzentrumsstandort aus, der die Daten Ihrer Ereignisquelle enthält, um so beim Verschieben von Daten außerhalb der Region zusätzliche regionsübergreifende und zonenübergreifende Breitbandkosten und eine erhöhte Latenz zu vermeiden.
   Tarif  | S1 | Wählen Sie den erforderlichen Durchsatz aus. Wählen Sie für die niedrigsten Kosten und die Anfangskapazität „S1“ aus.
   Capacity | 1 | Die Kapazität ist der Multiplikator, der auf die Eingangsrate, Speicherkapazität und Kosten der ausgewählten SKU angewendet wird.  Die Kapazität einer Umgebung kann nach der Erstellung geändert werden. Wählen Sie für die niedrigsten Kosten die Kapazität „1“ aus. 
  
6. Aktivieren Sie **An Dashboard anheften** für den zukünftigen optimierten und problemlosen Zugriff auf Ihre Time Series-Umgebung.

   ![Erstellen der Time Series Insights-Anheftung an das Dashboard](media/time-series-insights-get-started/3-pin-create.png)

7. Wählen Sie **Erstellen** aus, um den Bereitstellungsprozess zu starten. Dies kann einige Minuten dauern.

8. Wählen Sie das Symbol **Benachrichtigungen** (Glockensymbol) aus, um den Bereitstellungsprozess zu überwachen.

   ![Überwachen der Benachrichtigungen](media/time-series-insights-get-started/4-notifications.png)

Wenn die Bereitstellung erfolgreich verlaufen ist, können Sie **Zu Ressource wechseln** auswählen, um andere Eigenschaften zu konfigurieren, die Sicherheit mit Datenzugriffsrichtlinien festzulegen, Ereignisquellen hinzuzufügen und weitere Aktionen auszuführen.

## <a name="next-steps"></a>Nächste Schritte
* [Definieren von Datenzugriffsrichtlinien](time-series-insights-data-access.md) zum Schützen Ihrer Umgebung
* [Hinzufügen einer Event Hub-Ereignisquelle ](time-series-insights-how-to-add-an-event-source-eventhub.md) zu Ihrer Azure Time Series Insights-Umgebung 
* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Explorer](https://insights.timeseries.azure.com)

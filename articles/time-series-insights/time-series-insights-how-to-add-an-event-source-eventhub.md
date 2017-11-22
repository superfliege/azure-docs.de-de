---
title: "Gewusst wie: Hinzufügen einer Event Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie eine Ereignisquelle hinzugefügt wird, die mit einem Event Hub in Ihrer Time Series Insights-Umgebung verbunden ist."
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: f3a9a1c7e57383925877f674a2e02f931e5c1e3c
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Gewusst wie: Hinzufügen einer Event Hub-Ereignisquelle zu einer Time Series Insights-Umgebung

In diesem Artikel wird beschrieben, wie im Azure-Portal eine Ereignisquelle hinzugefügt wird, die Daten aus einem Event Hub in Ihre Time Series Insights-Umgebung liest.

## <a name="prerequisites"></a>Voraussetzungen
- Erstellen Sie eine Time Series Insights-Umgebung. Weitere Informationen finden Sie unter [Erstellen einer Azure Time Series Insights-Umgebung](time-series-insights-get-started.md). 
- Erstellen Sie einen Event Hub. Weitere Informationen zu Event Hubs finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](../event-hubs/event-hubs-create.md).
- An den Event Hub müssen aktive Nachrichtenereignisse gesendet werden. Weitere Informationen finden Sie unter [Senden von Ereignissen an Azure Event Hubs mithilfe von .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Erstellen Sie eine dedizierte Consumergruppe in Event Hub, die die Time Series Insight-Umgebung verwenden kann. Jede Time Series Insights-Ereignisquelle benötigt eine eigene dedizierte Consumergruppe, die nicht mit anderen Consumern gemeinsam genutzt wird. Wenn mehrere Leser Ereignisse aus der gleichen Consumergruppe nutzen, werden allen Lesern wahrscheinlich Fehler angezeigt. Beachten Sie, dass darüber hinaus ein Grenzwert von 20 Consumergruppen pro Event Hub gilt. Weitere Informationen finden Sie im [Programmierleitfaden für Event Hubs](../event-hubs/event-hubs-programming-guide.md).

## <a name="add-a-new-event-source"></a>Hinzufügen einer neuen Ereignisquelle
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Suchen Sie die vorhandene Time Series Insights-Umgebung. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**. Wählen Sie Ihre Time Series Insights-Umgebung aus.

3. Klicken Sie unter der Überschrift **Umgebungstopologie** auf **Ereignisquellen**.

   ![Ereignisquellen hinzufügen](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

4. Klicken Sie auf **+ Hinzufügen**.

5. Geben Sie unter **Name der Ereignisquelle** einen eindeutigen Namen für diese Time Series Insights-Umgebung ein, z.B. **event-stream**.

   ![Geben Sie die ersten drei Parameter im Formular ein.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

6. Wählen Sie als **Quelle** die Option **Event Hub** aus.

7. Wählen Sie unter **Importoption** die gewünschte Option aus. 
   - Wählen Sie **Verwenden Sie einen Event Hub aus verfügbaren Abonnements** aus, wenn Sie in einem Ihrer Abonnements bereits über einen Event Hub verfügen. Dies ist der einfachste Ansatz.
   - Wählen Sie **Event Hub-Einstellungen manuell angeben** aus, wenn der Event Hub außerhalb Ihrer Abonnements besteht oder Sie erweiterte Optionen auswählen möchten. 

8. Wenn Sie die Option **Verwenden Sie einen Event Hub aus verfügbaren Abonnements** ausgewählt haben, finden Sie in der folgenden Tabelle die erforderlichen Eigenschaften:

   ![Details zu Abonnements und Event Hub](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Eigenschaft | Beschreibung |
   | --- | --- |
   | Abonnement-ID | Wählen Sie das Abonnement, in dem dieser Event Hub erstellt wurde.
   | Service Bus-Namespace | Wählen Sie den Service Bus-Namespace, der den Event Hub enthält.
   | Event Hub-Name | Wählen Sie den Namen des Event Hubs.
   | Event Hub-Richtlinienname | Wählen Sie die SAS-Richtlinie, die auf der Registerkarte "Event Hub-Konfiguration" erstellt werden kann. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung **Lesen** verfügen.
   | Event Hub-Richtlinienschlüssel | Der Schlüsselwert ist möglicherweise bereits angegeben.
   | Event Hub-Consumergruppe | Die Consumergruppe, die Ereignisse aus dem Event Hub liest. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden. |
   | Ereignisserialisierungsformat | Zurzeit ist JSON die einzige verfügbare Serialisierung. Die Ereignismeldungen müssen in diesem Format vorliegen, damit Daten gelesen werden können. |
   | Name der Timestamp-Eigenschaft | Um diesen Wert zu bestimmen, müssen Sie das Nachrichtenformat der Nachrichtendaten kennen, die an Event Hub gesendet werden. Dieser Wert entspricht **name** der spezifischen Ereigniseigenschaft in den Nachrichtendaten, die Sie als Ereigniszeitstempel verwenden möchten. Bei dem Wert wird die Groß-/Kleinschreibung beachtet. Wenn dieser Wert nicht angegeben wird, wird der Zeitpunkt der **Einreihung des Ereignisses** in die Warteschlange in der Ereignisquelle als Ereigniszeitstempel verwendet. |


9. Wenn Sie die Option **Event Hub-Einstellungen manuell angeben** ausgewählt haben, finden Sie in der folgenden Tabelle die erforderlichen Eigenschaften:

   | Eigenschaft | Beschreibung |
   | --- | --- |
   | Abonnement-ID | Das Abonnement, in dem dieser Event Hub erstellt wurde.
   | Ressourcengruppe | Die Ressourcengruppe, in der dieser Event Hub erstellt wurde.
   | Service Bus-Namespace | Ein Service Bus-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung eines neuen Event Hubs auch einen Service Bus-Namespace erstellt.
   | Event Hub-Name | Der Name des Event Hubs. Bei der Erstellung des Event Hubs haben Sie ihm auch einen bestimmten Namen zugewiesen.
   | Event Hub-Richtlinienname | Die Richtlinie für den gemeinsamen Zugriff, die auf der Registerkarte "Event Hub-Konfiguration" erstellt werden kann. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung **Lesen** verfügen.
   | Event Hub-Richtlinienschlüssel | Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. Geben Sie hier den primären oder sekundären Schlüssel ein.
   | Event Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem Event Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.
   | Ereignisserialisierungsformat | Zurzeit ist JSON die einzige verfügbare Serialisierung. Die Ereignismeldungen müssen in diesem Format vorliegen, damit Daten gelesen werden können. |
   | Name der Timestamp-Eigenschaft | Um diesen Wert zu bestimmen, müssen Sie das Nachrichtenformat der Nachrichtendaten kennen, die an Event Hub gesendet werden. Dieser Wert entspricht **name** der spezifischen Ereigniseigenschaft in den Nachrichtendaten, die Sie als Ereigniszeitstempel verwenden möchten. Bei dem Wert wird die Groß-/Kleinschreibung beachtet. Wenn dieser Wert nicht angegeben wird, wird der Zeitpunkt der **Einreihung des Ereignisses** in die Warteschlange in der Ereignisquelle als Ereigniszeitstempel verwendet. |


10. Wählen Sie **Erstellen** aus, um die neue Ereignisquelle hinzuzufügen.
   
   ![Klicken Sie auf „Erstellen“.](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Nach der Erstellung der Ereignisquelle beginnt Time Series Insights automatisch damit, Daten in Ihre Umgebung zu streamen.


### <a name="add-a-consumer-group-to-your-event-hub"></a>Hinzufügen einer Consumergruppe zum Event Hub
Consumergruppen werden von Anwendungen verwendet, um Daten aus Azure Event Hubs abzurufen. Geben Sie eine dedizierte Consumergruppe an, die nur von dieser Time Series Insights-Umgebung verwendet wird, um zuverlässig Daten aus Ihren Event Hub zu lesen.

Gehen Sie folgendermaßen vor, um Ihrem Event Hub eine Consumergruppe hinzuzufügen:
1. Suchen Sie im Azure-Portal den Event Hub, und öffnen Sie ihn.

2. Wählen Sie unter der Überschrift **Entitäten** den Eintrag **Consumergruppen** aus.

   ![Event Hub – Consumergruppe hinzufügen](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

3. Wählen Sie **+ Consumergruppe** aus, um eine neue Consumergruppe hinzuzufügen. 

4. Geben Sie auf der Seite **Consumergruppen** einen neuen eindeutigen **Namen** an.  Verwenden Sie diesen Namen, wenn Sie eine neue Ereignisquelle in der Time Series Insights-Umgebung erstellen.

5. Wählen Sie **Erstellen** aus, um die neue Consumergruppe zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
- [Definieren Sie Datenzugriffsrichtlinien](time-series-insights-data-access.md), um die Daten zu schützen.
- [Senden Sie Ereignisse](time-series-insights-send-events.md) an die Ereignisquelle.
- Greifen Sie über den [Time Series Insights-Explorer](https://insights.timeseries.azure.com) auf Ihre Umgebung zu.

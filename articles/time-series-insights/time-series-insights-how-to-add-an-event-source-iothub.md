---
title: "Gewusst wie: Hinzufügen einer IoT Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie eine Ereignisquelle hinzugefügt wird, die mit einem IoT Hub in Ihrer Time Series Insights-Umgebung verbunden ist."
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
ms.openlocfilehash: ed31a0e725d1e0863e9c4695d4eccb324f60678a
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Hinzufügen einer IoT Hub-Ereignisquelle zu einer Time Series Insights-Umgebung
In diesem Artikel wird beschrieben, wie im Azure-Portal eine Ereignisquelle hinzugefügt wird, die Daten aus einem IoT Hub in Ihre Time Series Insights-Umgebung einliest.

## <a name="prerequisites"></a>Voraussetzungen
- Erstellen Sie eine Time Series Insights-Umgebung. Weitere Informationen finden Sie unter [Erstellen einer Azure Time Series Insights-Umgebung](time-series-insights-get-started.md). 
- Erstellen Sie einen IoT Hub. Weitere Informationen zu IoT Hubs finden Sie unter [Erstellen eines IoT Hubs über das Azure-Portal](../iot-hub/iot-hub-create-through-portal.md).
- An den IoT Hub müssen aktive Nachrichtenereignisse gesendet werden.
- Erstellen Sie eine dedizierte Consumergruppe in IoT Hub, die die Time Series Insight-Umgebung verwenden kann. Jede Time Series Insights-Ereignisquelle benötigt eine eigene dedizierte Consumergruppe, die nicht mit anderen Consumern gemeinsam genutzt wird. Wenn mehrere Leser Ereignisse aus der gleichen Consumergruppe nutzen, werden allen Lesern wahrscheinlich Fehler angezeigt. Weitere Details finden Sie im [IoT Hub-Entwicklerhandbuch](../iot-hub/iot-hub-devguide.md).

## <a name="add-a-new-event-source"></a>Hinzufügen einer neuen Ereignisquelle
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Suchen Sie die vorhandene Time Series Insights-Umgebung. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**. Wählen Sie Ihre Time Series Insights-Umgebung aus.

3. Klicken Sie unter der Überschrift **Umgebungstopologie** auf **Ereignisquellen**.
   ![Ereignisquellen + Hinzufügen](media/time-series-insights-how-to-add-an-event-source-iothub/1-event-sources.png)

4. Klicken Sie auf **+ Hinzufügen**.

5. Geben Sie unter **Name der Ereignisquelle** einen eindeutigen Namen für diese Time Series Insights-Umgebung ein, z.B. **event-stream**.

   ![Geben Sie die ersten drei Parameter im Formular ein.](media/time-series-insights-how-to-add-an-event-source-iothub/2-import-option.png)

6. Wählen Sie als **Quelle** die Option **IoT Hub** aus.

7. Wählen Sie unter **Importoption** die gewünschte Option aus. 
   - Wählen Sie **IoT Hub aus verfügbaren Abonnements verwenden** aus, wenn Sie in einem Ihrer Abonnements bereits über einen IoT Hub verfügen. Dies ist der einfachste Ansatz.
   - Wählen Sie **IoT Hub-Einstellungen manuell angeben** aus, wenn der IoT Hub sich außerhalb Ihrer Abonnements befindet oder Sie erweiterte Optionen auswählen möchten. 

8. Wenn Sie die Option **IoT Hub aus verfügbaren Abonnements verwenden** ausgewählt haben, finden Sie in der folgenden Tabelle die erforderlichen Eigenschaften:

   ![Details zu Abonnements und Event Hub](media/time-series-insights-how-to-add-an-event-source-iothub/3-new-event-source.png)

   | Eigenschaft | Beschreibung |
   | --- | --- |
   | Abonnement-ID | Wählen Sie das Abonnement, in dem dieser IoT Hub erstellt wurde.
   | IoT Hub-Name | Wählen Sie den Namen des IoT Hubs.
   | IoT Hub-Richtlinienname | Wählen Sie die SAS-Richtlinie aus, die sich auf der Registerkarte „IoT Hub-Einstellungen“ befindet. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung zum Herstellen einer **Dienstverbindung** verfügen.
   | IoT Hub-Richtlinienschlüssel | Der Schlüssel ist bereits angegeben.
   | IoT Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem IoT Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.
   | Ereignisserialisierungsformat | Zurzeit ist JSON die einzige verfügbare Serialisierung. Die Ereignismeldungen müssen in diesem Format vorliegen, damit Daten gelesen werden können. |
   | Name der Timestamp-Eigenschaft | Um diesen Wert ermitteln, müssen Sie das Nachrichtenformat der Nachrichtendaten kennen, die an IoT Hub gesendet werden. Dieser Wert entspricht **name** der spezifischen Ereigniseigenschaft in den Nachrichtendaten, die Sie als Ereigniszeitstempel verwenden möchten. Bei dem Wert wird die Groß-/Kleinschreibung beachtet. Wenn dieser Wert nicht angegeben ist, wird der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange** in der Ereignisquelle als Ereigniszeitstempel verwendet. |

9. Wenn Sie die Option **IoT Hub-Einstellungen manuell angeben** ausgewählt haben, finden Sie in der folgenden Tabelle die erforderlichen Eigenschaften:

   | Eigenschaft | Beschreibung |
   | --- | --- |
   | Abonnement-ID | Das Abonnement, in dem dieser IoT Hub erstellt wurde.
   | Ressourcengruppe | Der Name der Ressourcengruppe, in der dieser IoT Hub erstellt wurde.
   | IoT Hub-Name | Der Name Ihres IoT Hubs. Bei der Erstellung Ihres IoT Hubs haben Sie ihm auch einen bestimmten Namen zugewiesen.
   | IoT Hub-Richtlinienname | Die SAS-Richtlinie, die sich auf der Registerkarte „IoT Hub-Einstellungen“ befindet. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Die SAS-Richtlinie für die Ereignisquelle *muss* über die Berechtigung zum Herstellen einer **Dienstverbindung** verfügen.
   | IoT Hub-Richtlinienschlüssel | Der Schlüssel für den gemeinsamen Zugriff, der für die Authentifizierung des Zugriffs auf den Service Bus-Namespace verwendet wird. Geben Sie hier den primären oder sekundären Schlüssel ein.
   | IoT Hub-Consumergruppe | Die Consumergruppe zum Lesen von Ereignissen aus dem IoT Hub. Es wird ausdrücklich empfohlen, eine dedizierte Consumergruppe für die Ereignisquelle zu verwenden.
   | Ereignisserialisierungsformat | Zurzeit ist JSON die einzige verfügbare Serialisierung. Die Ereignismeldungen müssen in diesem Format vorliegen, damit Daten gelesen werden können. |
   | Name der Timestamp-Eigenschaft | Um diesen Wert ermitteln, müssen Sie das Nachrichtenformat der Nachrichtendaten kennen, die an IoT Hub gesendet werden. Dieser Wert entspricht **name** der spezifischen Ereigniseigenschaft in den Nachrichtendaten, die Sie als Ereigniszeitstempel verwenden möchten. Bei dem Wert wird die Groß-/Kleinschreibung beachtet. Wenn dieser Wert nicht angegeben ist, wird der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange** in der Ereignisquelle als Ereigniszeitstempel verwendet. |

10. Wählen Sie **Erstellen** aus, um die neue Ereignisquelle hinzuzufügen.

   ![Klicken Sie auf „Erstellen“.](media/time-series-insights-how-to-add-an-event-source-iothub/4-create-button.png)

   Nach der Erstellung der Ereignisquelle beginnt Time Series Insights automatisch damit, Daten in Ihre Umgebung zu streamen.

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Hinzufügen einer Consumergruppe zu Ihrem IoT Hub
Consumergruppen werden von Anwendungen verwendet, um Daten aus Azure IoT Hubs abzurufen. Geben Sie eine dedizierte Consumergruppe an, die nur von dieser Time Series Insights-Umgebung verwendet wird, um zuverlässig Daten aus Ihrem IoT Hub zu lesen.

Gehen Sie folgendermaßen vor, um Ihrem IoT Hub eine neue Consumergruppe hinzuzufügen:
1. Suchen Sie im Azure-Portal den IoT Hub, und öffnen Sie ihn.

2. Wählen Sie unter der Überschrift **Messaging** die Option **Endpunkte** aus. 

   ![Hinzufügen einer Consumergruppe](media/time-series-insights-how-to-add-an-event-source-iothub/5-add-consumer-group.png)

3. Wählen Sie den Endpunkt **Ereignisse** aus. Die Seite **Eigenschaften** wird geöffnet.

4. Geben Sie unter der Überschrift **Consumergruppen** einen neuen eindeutigen Namen für die Consumergruppe an. Verwenden Sie diesen Namen, wenn Sie eine neue Ereignisquelle in der Time Series Insights-Umgebung erstellen.

5. Wählen Sie **Speichern** aus, um die neue Consumergruppe zu speichern.

## <a name="next-steps"></a>Nächste Schritte
- [Definieren von Datenzugriffsrichtlinien](time-series-insights-data-access.md) zum Schützen der Daten
- [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
- Zugreifen auf Ihre Umgebung über den [Time Series Insights-Explorer](https://insights.timeseries.azure.com)

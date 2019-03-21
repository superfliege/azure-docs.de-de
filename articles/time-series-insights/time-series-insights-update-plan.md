---
title: Planen Ihrer Azure Time Series Insights Preview-Umgebung | Microsoft-Dokumentation
description: Planen Sie Ihre Azure Time Series Insights Preview-Umgebung.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 251e95744f57d9b5e42df9bdc3743f4880ff5381
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58076995"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planen Ihrer Azure Time Series Insights Preview-Umgebung

Dieser Artikel beschreibt bewährte Methoden zum Planen und schnellen Starten mit der Verwendung der Azure Time Series Insights Preview.

## <a name="best-practices-for-planning-and-preparation"></a>Bewährte Methoden für Planung und Vorbereitung

Kenntnis und Verständnis der folgenden Aspekte erleichtern die ersten Schritte mit Time Series Insights:

* Was Sie erhalten, wenn Sie eine Time Series Insights Preview-Umgebung bereitstellen.
* Welche Ihre Time Series-IDs und Timestamp-Eigenschaften sind.
* Was das neue Zeitreihenmodell ist, und wie Sie Ihr eigenes erstellen.
* Wie Sie Ereignisse effizient in JSON senden. 
* Time Series Insights-Optionen für Business Disaster Recovery (BCDR).

Time Series Insights verwendet ein Geschäftsmodell mit nutzungsbasierter Bezahlung. Weitere Informationen zu Gebühren und Kapazitäten finden Sie unter [Time Series Insights – Preise](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>Die Time Series Insights Preview-Umgebung

Wenn Sie eine Time Series Insights Preview-Umgebung bereitstellen, erstellen Sie zwei Azure-Ressourcen:

* Time Series Insights Preview-Umgebung
* Azure-Speicherkonto vom Typ „Allgemein v1“

Um zu beginnen, benötigen Sie drei zusätzliche Elemente:
 
- Ein [Zeitreihenmodell](./time-series-insights-update-tsm.md) 
- Eine [mit Time Series Insights verbundene Ereignisquelle](./time-series-insights-how-to-add-an-event-source-iothub.md) 
- [In die Ereignisquelle fließende Ereignisse](./time-series-insights-send-events.md), die sowohl dem Modell zugeordnet als auch im gültigen JSON-Format sind 

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>Konfigurieren Ihrer Time Series-IDs und Timestamp-Eigenschaften

Um eine neue Time Series Insights-Umgebung zu erstellen, wählen Sie eine Time Series-ID aus. Diese Vorgehensweise fungiert als logische Partition für Ihre Daten. Wie bereits erwähnt, stellen Sie sicher, dass Sie Ihre Time Series-ID bereit haben.

> [!IMPORTANT]
> Time Series-IDs sind *unveränderlich* und *können später nicht mehr geändert werden*. Überprüfen Sie jede vor der endgültigen Auswahl und der ersten Verwendung.

Sie können bis zu drei (3) Schlüssel auswählen, um Ihre Ressourcen eindeutig zu unterscheiden. Weitere Informationen finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](./time-series-insights-update-how-to-id.md) und [Speicherung und Dateneingang](./time-series-insights-update-storage-ingress.md).

Die Timestamp-Eigenschaft ist ebenfalls wichtig. Sie können diese Eigenschaft festlegen, wenn Sie Ereignisquellen hinzufügen. Jede Ereignisquelle besitzt eine optionale Timestamp-Eigenschaft, die zum Nachverfolgen von Ereignisquellen im Laufe der Zeit verwendet wird. Zeitstempelwerte beachten die Groß- und Kleinschreibung und müssen gemäß der individuellen Spezifikation jeder Ereignisquelle formatiert sein.

> [!TIP]
> Überprüfen Sie die Formatierungs- und Analyseanforderungen für Ihre Ereignisquellen.

Wenn der Zeitstempel eines Ereignisses leer bleibt, wird der Zeitpunkt der Einreihung des Ereignisses in die Warteschlange einer Ereignisquelle verwendet. Wenn Sie historische Daten oder Ereignisse im Batch senden, ist es hilfreicher, die Timestamp-Eigenschaft anzupassen, statt den Zeitpunkt der Einreihung des Ereignisses in die Warteschlange zu verwenden. Weitere Informationen finden Sie unter [Hinzufügen von Ereignisquellen in IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md). 

## <a name="understand-the-time-series-model"></a>Grundlagen des Zeitreihenmodells

Sie können jetzt das Zeitreihenmodell Ihrer Time Series Insights-Umgebung konfigurieren. Das neue Modell erleichtert das Auffinden und Analysieren von IoT-Daten. Es ermöglicht die Zusammenstellung, Wartung und Anreicherung von Zeitreihendaten und hilft bei der Vorbereitung für Consumer bereiter Datasets. Das Modell verwendet **Time Series-IDs**, die einer Instanz zugeordnet werden, die die eindeutige Ressource Variablen, bekannt als Typen, zuordnet sowie Hierarchien. Erfahren Sie mehr über das neue [Zeitreihenmodell](./time-series-insights-update-tsm.md).

Das Modell ist dynamisch, damit es jederzeit erstellt werden kann. Für den schnellen Einstieg erstellen und laden Sie es hoch, bevor Sie Daten per Push in Time Series Insights übertragen. Informationen zum Erstellen Ihres Modells finden Sie unter [Verwenden des Zeitreihenmodells](./time-series-insights-update-how-to-tsm.md).

Bei vielen Kunden wird das Zeitreihenmodell einem vorhandenen Ressourcenmodell oder einem bereits eingerichteten ERP-System zugeordnet. Wenn Sie nicht über ein vorhandenes Modell verfügen, ist wird eine vordefinierte Erfahrung [bereitgestellt](https://github.com/Microsoft/tsiclient), um schnell den Betrieb aufnehmen zu können. Um einen Eindruck zu erhalten, wie Ihnen ein Modell helfen könnte, sehen Sie sich die [Beispieldemoumgebung](https://insights.timeseries.azure.com/preview/demo) an. 

## <a name="shape-your-events"></a>Gestalten Ihrer Ereignisse

Sie können die Art überprüfen, in der Sie Ereignisse an Time Series Insights senden. Idealerweise werden Ihrer Ereignisse gut und effizient denormalisiert.

Eine gute Faustregel ist:

* Speichern von Metadaten in Ihrem Zeitreihenmodell
* Das Zeitreihenmodell, Instanzfelder und Ereignisse enthalten nur notwendige Informationen wie:
  * Time Series-ID
  * Zeitstempel

Weitere Informationen finden Sie unter [Gestalten von Ereignissen](./time-series-insights-send-events.md#json).

## <a name="business-disaster-recovery"></a>Business Disaster Recovery

Time Series Insights ist ein Hochverfügbarkeitsdienst, der Redundanzen auf Azure-Regionsebene verwendet. Eine Konfiguration ist nicht erforderlich, um diese inhärenten Funktionen zu verwenden. Die Microsoft Azure-Plattform bietet auch Features zum Erstellen von Lösungen mit Notfallwiederherstellungsfunktionen oder regionsübergreifender Verfügbarkeit. Um Geräten oder Benutzern globale und regionsübergreifende Hochverfügbarkeit bereitzustellen, nutzen Sie die Vorteile dieser Notfallwiederherstellungsfeatures von Azure. 

Informationen zu integrierten Funktionen in Azure für Business Continuity & Disaster Recovery (BCDR) finden Sie unter [Azure-Geschäftskontinuität – Technische Anleitung](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance). Architekturanleitungen zu Strategien für Azure-Anwendungen zur Erzielung von Hochverfügbarkeit und Notfallwiederherstellung finden Sie in dem Dokument zu [Notfallwiederherstellung und Hochverfügbarkeit für Azure-Anwendungen](https://docs.microsoft.com/azure/architecture/resiliency/index).

> [!NOTE]
> 
>  Time Series Insights verfügt nicht über integrierte BCDR.
> Standardmäßig verfügen Azure Storage, Azure IoT Hub und Event Hubs über integrierte Wiederherstellungsfunktionen.

Weitere Informationen finden Sie hier:

* [Azure-Speicherredundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Hochverfügbarkeit und Notfallwiederherstellung von IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)
* [Event Hub-Richtlinien](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)

Wenn Sie BCDR benötigen, können Sie immer noch eine Wiederherstellungsstrategie implementieren. Erstellen Sie eine zweite Time Series Insights-Umgebung in einer Azure-Sicherungsregion. Senden Sie von Ihrer primären Ereignisquelle Ereignisse an diese sekundäre Umgebung. Verwenden Sie eine zweite dedizierte Consumergruppe und die BCDR-Richtlinien dieser Ereignisquelle.

Führen Sie diese Schritte aus, um eine sekundäre Time Series Insights-Umgebung zu erstellen und zu verwenden.

1. Erstellen Sie eine Umgebung in einer zweiten Region. Weitere Informationen finden Sie unter [Time Series Insights-Umgebungen](./time-series-insights-get-started.md).
1. Erstellen Sie eine zweite dedizierte Consumergruppe für Ihre Ereignisquelle. Verbinden Sie diese Ereignisquelle mit der neuen Umgebung. Stellen Sie sicher, dass Sie die zweite, dedizierte Consumergruppe festlegen. Weitere Informationen finden Sie in der [IoT Hub-Dokumentation](./time-series-insights-how-to-add-an-event-source-iothub.md) oder der [Event Hub-Dokumentation](./time-series-insights-data-access.md).
1. Wenn Ihre primäre Region während eines Notfalls betroffen ist, leiten Sie den Betrieb in die Time Series Insights-Sicherungsumgebung um.

> [!IMPORTANT]
> * Beachten Sie, dass es im Falle eines Failovers zu einer Verzögerung kommen kann.
> * Ein Failover kann auch eine momentane Spitze bei der Nachrichtenverarbeitung verursachen, wenn der Betrieb und Vorgänge umgeleitet werden.
> * Weitere Informationen finden Sie unter [Mildern von Wartezeiten in Time Series Insights](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie hier:

- [Speicherung und Dateneingang in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md)
- [Datenmodellierung](./time-series-insights-update-tsm.md)
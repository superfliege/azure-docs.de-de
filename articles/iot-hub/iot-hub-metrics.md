---
title: Überwachen von Azure IoT Hub mithilfe von Metriken | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Metriken die allgemeine Integrität Ihrer IoT Hub-Instanzen bewerten und überwachen.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: b1c9854d794c88c28c82a4b6e40c81a29552223a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984071"
---
# <a name="understand-iot-hub-metrics"></a>Grundlegendes zu IoT Hub-Metriken
IoT Hub-Metriken liefern bessere Daten zum Zustand der Azure IoT-Ressourcen in Ihrem Azure-Abonnement. Auf der Grundlage von IoT Hub-Metriken können Sie die allgemeine Integrität des IoT Hub-Diensts und der mit ihm verbundenen Geräte beurteilen. Benutzerorientierte Statistiken spielen eine wichtige Rolle. Sie bieten die Basis, um die Vorgänge in Ihrem IoT Hub zu erkennen und Probleme zu beheben, ohne den Azure-Support zu kontaktieren.

Metriken sind standardmäßig aktiviert. IoT Hub-Metriken können über das Azure-Portal angezeigt werden.

## <a name="how-to-view-iot-hub-metrics"></a>Anzeigen von IoT Hub-Metriken
1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT-Hubs finden Sie in der Anleitung mit den [ersten Schritten][lnk-get-started].
2. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Metriken**.
   
    ![][1]
3. Auf dem Blatt „Metriken“ können Sie die Metriken für Ihren IoT-Hub anzeigen und benutzerdefinierte Metrikansichten erstellen. Wenn Sie auf **Diagnoseeinstellungen** klicken, können Sie Ihre Metrikdaten an einen Event Hubs-Endpunkt oder an ein Azure Storage-Konto senden.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub-Metriken und ihre Verwendung
IoT Hub bietet mehrere Metriken, um Ihnen einen Überblick über die Integrität Ihres Hubs und die Gesamtzahl der verbundenen Geräte zu verschaffen. Sie können Informationen aus mehreren Metriken kombinieren, um sich ein umfassenderes Bild des Zustands des IoT Hubs zu machen. In der folgenden Tabelle werden die Metriken beschrieben, die für IoT Hubs nachverfolgt werden. Außerdem erfahren Sie, in welchem Bezug die einzelnen Metriken zum allgemeinen Status des IoT Hubs stehen.

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Count|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine Dimensionen|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Count|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine Dimensionen|
|c2d.commands.egress.complete.success|Commands completed (Abgeschlossene Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden|Keine Dimensionen|
|c2d.commands.egress.abandon.success|Commands abandoned (Abgebrochene Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden|Keine Dimensionen|
|c2d.commands.egress.reject.success|Commands rejected (Abgelehnte Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden|Keine Dimensionen|
|devices.totalDevices|Geräte gesamt (veraltet)|Count|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine Dimensionen|
|devices.connectedDevices.allProtocol|Verbundene Geräte (veraltet) |Count|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine Dimensionen|
|d2c.telemetry.egress.success|Routing: Übermittelte Telemetrienachrichten|Count|Gesamt|Die Anzahl der erfolgreichen Nachrichtenübermittlungen an alle Endpunkte über das IoT Hub-Routing Wenn eine Nachricht an mehrere Endpunkte weitergeleitet wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins Wenn eine Nachricht mehrmals an denselben Endpunkt übermittelt wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins|Keine Dimensionen|
|d2c.telemetry.egress.dropped|Routing: Verworfene Telemetrienachrichten |Count|Gesamt|Die Anzahl der Nachrichten, die vom IoT Hub-Routing aufgrund von inaktiven Endpunkten gelöscht wurden. Dieser Wert zählt nicht die Nachrichten, die an die Fallbackroute übermittelt werden, da gelöschte Nachrichten dort nicht übermittelt werden.|Keine Dimensionen|
|d2c.telemetry.egress.orphaned|Routing: Verwaiste Telemetrienachrichten |Count|Gesamt|Die Häufigkeit, mit der Nachrichten durch das IoT Hub-Routing verwaist wurden, da sie mit keinen Routingregeln (einschließlich der Fallbackregel) übereinstimmten. |Keine Dimensionen|
|d2c.telemetry.egress.invalid|Routing: Nicht kompatible Telemetrienachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten aufgrund einer Inkompatibilität mit dem Endpunkt nicht übermitteln konnte. Dieser Wert umfasst keine Wiederholungen.|Keine Dimensionen|
|d2c.telemetry.egress.fallback|Routing: An den Fallback übermittelte Nachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten an den mit der Fallbackroute verbundenen Endpunkt übermittelt hat.|Keine Dimensionen|
|d2c.endpoints.egress.eventHubs|Routing: An Event Hub übermittelte Nachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Event Hub-Endpunkte übermittelt hat.|Keine Dimensionen|
|d2c.endpoints.latency.eventHubs|Routing: Nachrichtenwartezeit für Event Hub|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt.|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusQueues|Routing: An Service Bus-Warteschlange übermittelte Nachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Warteschlangenendpunkte übermittelt hat.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusQueues|Routing: Nachrichtenwartezeit für Service Bus-Warteschlange|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt.|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusTopics|Routing: An Service Bus-Thema übermittelte Nachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Themaendpunkte übermittelt hat.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusTopics|Routing: Nachrichtenwartezeit für Service Bus-Thema|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Themaendpunkt.|Keine Dimensionen|
|d2c.endpoints.egress.builtIn.events|Routing: An Nachrichten/Ereignisse übermittelte Nachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an den integrierter Endpunkt (Nachrichten/Ereignisse) übermittelt hat.|Keine Dimensionen|
|d2c.endpoints.latency.builtIn.events|Routing: Nachrichtenwartezeit für Nachrichten/Ereignisse|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Telemetrienachricht beim integrierten Endpunkt (Nachrichten/Ereignisse).|Keine Dimensionen|
|d2c.endpoints.egress.storage|Routing: An den Speicher übermittelte Nachrichten|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Speicherendpunkte übermittelt hat.|Keine Dimensionen|
|d2c.endpoints.latency.storage|Routing: Nachrichtenwartezeit für Speicher|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Speicherendpunkt.|Keine Dimensionen|
|d2c.endpoints.egress.storage.bytes|Routing: An den Speicher übermittelte Daten|Byte|Gesamt|Die Datenmenge (Bytes), die das IoT Hub-Routing an die Speicherendpunkte übermittelt.|Keine Dimensionen|
|d2c.endpoints.egress.storage.blobs|Routing: An den Speicher übermittelte Blobs|Count|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Blobs an Speicherendpunkte übermittelt hat.|Keine Dimensionen|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine Dimensionen|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Count|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine Dimensionen|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Count|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine Dimensionen|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|Keine Dimensionen|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|Keine Dimensionen|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine Dimensionen|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|Keine Dimensionen|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Count|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.completed|Abgeschlossene Aufträge|Count|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine Dimensionen|
|jobs.failed|Fehlerhafte Aufträge|Count|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine Dimensionen|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Count|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine Dimensionen|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Count|Durchschnitt|Gesamtzahl der heute verwendeten Nachrichten. Dies ist ein kumulativer Wert, der jeden Tag um 00:00 Uhr UTC auf 0 zurückgesetzt wird.|Keine Dimensionen|
|deviceDataUsage|Gerätedatennutzung gesamt (veraltet)|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine Dimensionen|
|deviceDataUsageV2|Gerätedatennutzung gesamt (Vorschau)|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine Dimensionen|
|totalDeviceCount|Geräte gesamt (Vorschau)|Count|Durchschnitt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine Dimensionen|
|connectedDeviceCount|Verbundene Geräte (Vorschau)|Count|Durchschnitt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine Dimensionen|
|Konfigurationen|Konfigurationsmetriken|Count|Gesamt|Metriken für Konfigurationsvorgänge|Keine Dimensionen|

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun einen Überblick über IoT Hub-Metriken verschafft haben, folgen Sie dem folgenden Link, um mehr über die Verwaltung von IoT Hub zu erfahren:

* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Deploy Azure IoT Edge on a simulated device in Linux - preview][lnk-iotedge] (Bereitstellen von Azure IoT Edge auf einem simulierten Gerät in Linux – Vorschauversion)

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md

---
title: Skalieren von Azure IoT Hub | Microsoft Docs
description: Es wird beschrieben, wie Sie Ihre IoT Hub-Instanz skalieren, um den erwarteten Nachrichtendurchsatz und die gewünschten Features zu unterstützen. Enthält eine Zusammenfassung der unterstützten Durchsätze für die einzelnen Ebenen und Optionen für das Sharding.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: d77953100bb7007553cf1c5ab00b9cdafea281e9
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030436"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Wählen des richtigen IoT Hub-Tarifs für Ihre Lösung

Jede IoT-Lösung ist unterschiedlich, und jeder Azure IoT Hub verfügt in Bezug auf den Preis und die Skalierung über mehrere Optionen. Dieser Artikel soll Ihnen als Hilfe beim Evaluieren Ihrer IoT Hub-Anforderungen dienen. Preisinformationen zu IoT Hub-Tarifen finden Sie unter [IoT Hub – Preise](https://azure.microsoft.com/pricing/details/iot-hub). 

Beantworten Sie für sich die folgenden Fragen, um zu entscheiden, welcher IoT Hub-Tarif für Ihre Lösung geeignet ist:

**Welche Features sollen verwendet werden?**
Azure IoT Hub verfügt über zwei Tarife (Basic und Standard), die sich anhand der unterstützten Features unterscheiden. Wenn Ihre IoT-Lösung auf dem Sammeln der Daten von Geräten und dem zentralen Analysieren basiert, eignet sich für Sie wahrscheinlich der Tarif „Basic“ am besten. Falls Sie anspruchsvollere Konfigurationen verwenden möchten, um IoT-Geräte per Remotezugriff zu steuern oder einen Teil der Workloads auf die Geräte selbst zu verteilen, sollten Sie die Nutzung des Tarifs „Standard“ erwägen. Eine ausführliche Aufschlüsselung der Features des jeweiligen Tarifs finden Sie unter [Basic- und Standard-Tarif](#basic-and-standard-tiers).

**Wie viele Daten sollen täglich verschoben werden?**
Jeder IoT Hub-Tarif ist in drei Größen verfügbar. Die Größe richtet sich danach, welcher Datendurchsatz an einem Tag verarbeitet werden kann. Diese Größen werden mit den Zahlen 1, 2 und 3 bezeichnet. Jede Einheit einer IoT Hub-Instanz der Ebene 1 kann pro Tag 400.000 Nachrichten verarbeiten, während mit einer Einheit der Ebene 3 pro Tag 300 Millionen Nachrichten verarbeitet werden können. Ausführliche Informationen zu den Datenrichtlinien finden Sie unter [Nachrichtendurchsatz](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Basic- und Standard-Tarif

Im Standard-Tarif von IoT Hub können alle Features genutzt werden. Er ist für alle IoT-Lösungen erforderlich, für die die Funktionen für die bidirektionale Kommunikation verwendet werden sollen. Im Basic-Tarif ist ein Teil der Features aktiviert. Dieser Tarif ist für IoT-Lösungen bestimmt, für die nur die unidirektionale Kommunikation von Geräten in die Cloud erforderlich ist. Beide Tarife verfügen über die gleichen Sicherheits- und Authentifizierungsfeatures.

Nachdem Sie Ihre IoT Hub-Instanz erstellt haben, können Sie ein Upgrade vom Basic-Tarif in den Standard-Tarif durchführen, ohne Ihre vorhandenen Vorgänge zu unterbrechen. Weitere Informationen finden Sie unter [How to upgrade your IoT hub](iot-hub-upgrade.md) (Durchführen eines Upgrades für Ihren IoT Hub).

| Funktion | Basic-Tarif | Standard-Tarif |
| ---------- | ---------- | ------------- |
| [Gerät-zu-Cloud-Telemetrie](iot-hub-devguide-messaging.md) | Ja | Ja |
| [Gerätebasierte Identität](iot-hub-devguide-identity-registry.md) | Ja | Ja |
| [Nachrichtenrouting](iot-hub-devguide-messages-read-custom.md) und [Event Grid-Integration](iot-hub-event-grid.md) | Ja | Ja |
| [HTTP-, AMQP- und MQTT-Protokolle](iot-hub-devguide-protocols.md) | Ja | Ja |
| [Device Provisioning-Dienst](../iot-dps/about-iot-dps.md) | Ja | Ja |
| [Überwachung und Diagnose](iot-hub-monitor-resource-health.md) | Ja | Ja |
| [Cloud-zu-Gerät-Messaging](iot-hub-devguide-c2d-guidance.md) |   | Ja |
| [Gerätezwillinge](iot-hub-devguide-device-twins.md), [Modulzwillinge](iot-hub-devguide-module-twins.md) und [Geräteverwaltung](iot-hub-device-management-overview.md) |   | Ja |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | Ja |

Es gibt auch einen kostenlosen Tarif (Free-Tarif) für IoT Hub, der für Tests und Evaluierungen bestimmt ist. Er verfügt über alle Funktionen des Standard-Tarifs, aber das Messaging ist eingeschränkt. Ein Upgrade aus dem Free-Tarif in den Basic- oder Standard-Tarif ist nicht möglich. 

### <a name="iot-hub-rest-apis"></a>REST-APIs für IoT Hub

Der Unterschied zwischen den unterstützten Funktionen zwischen dem Basic- und dem Standard-Tarif für IoT Hub bedeutet, dass einige API-Aufrufe für Hubs des Basic-Tarifs nicht funktionieren. In der folgenden Tabelle ist angegeben, welche APIs verfügbar sind: 

| API | Basic-Tarif | Standard-Tarif |
| --- | ---------- | ------------- |
| [Gerät löschen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/deletedevice) | Ja | Ja |
| [Gerät abrufen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getdevice) | Ja | Ja |
| Modul löschen | Ja | Ja |
| Modul abrufen | Ja | Ja |
| [Registrierungsstatistiken abrufen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getdeviceregistrystatistics) | Ja | Ja |
| [Dienststatistiken abrufen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getservicestatistics) | Ja | Ja |
| [Gerät erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/iothub/service/service/createorupdatedevice) | Ja | Ja |
| „Put“ für Modul durchführen | Ja | Ja |
| [IoT Hub abfragen](https://docs.microsoft.com/rest/api/iothub/service/service/queryiothub) | Ja | Ja |
| Module abfragen | Ja | Ja |
| [SAS-URI für Dateiupload erstellen](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/createfileuploadsasuri) | Ja | Ja |
| [Gerätegebundene Benachrichtigung empfangen](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/receivedeviceboundnotification) | Ja | Ja |
| [Geräteereignis senden](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/senddeviceevent) | Ja | Ja |
| Modulereignis senden | Ja | Ja |
| [Dateiuploadstatus aktualisieren](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/updatefileuploadstatus) | Ja | Ja |
| [Massenvorgang für Geräte](https://docs.microsoft.com/en-us/rest/api/iot-dps/deviceenrollment/bulkoperation) | Ja, mit Ausnahme von IoT Edge-Funktionen | Ja | 
| [Befehlswarteschlange bereinigen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/purgecommandqueue) |   | Ja |
| [Gerätezwilling abrufen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/gettwin) |   | Ja |
| Modulzwilling abrufen |   | Ja |
| [Gerätemethode aufrufen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/invokedevicemethod) |   | Ja |
| [Gerätezwilling aktualisieren](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/updatetwin) |   | Ja | 
| Modulzwilling aktualisieren |   | Ja | 
| [Gerätegebundene Benachrichtigung verwerfen](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/abandondeviceboundnotification) |   | Ja |
| [Gerätegebundene Benachrichtigung abschließen](https://docs.microsoft.com/en-us/rest/api/iothub/device/device/completedeviceboundnotification) |   | Ja |
| [Auftrag abbrechen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/canceljob) |   | Ja |
| [Auftrag erstellen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/createjob) |   | Ja |
| [Auftrag abrufen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/getjob) |   | Ja |
| [Aufträge abfragen](https://docs.microsoft.com/en-us/rest/api/iothub/service/service/queryjobs) |   | Ja |

## <a name="message-throughput"></a>Nachrichtendurchsatz

Die beste Methode zum Skalieren einer IoT Hub-Lösung ist die Auswertung des Datenverkehrs pro Einheit. Achten Sie besonders auf den erforderlichen Spitzendurchsatz für die folgenden Kategorien von Vorgängen:

* D2C-Nachrichten
* C2D-Nachrichten
* Identitätsregistrierungsvorgänge

Datenverkehr wird pro Einheit gemessen, nicht pro Hub. Einer IoT Hub-Instanz der Ebene 1 oder 2 können bis zu 200 Einheiten zugeordnet sein. Einer IoT Hub-Instanz der Ebene 3 können bis zu zehn Einheiten zugeordnet sein. Nachdem Sie Ihren IoT Hub erstellt haben, können Sie die Anzahl von Einheiten ändern oder zwischen den Größen 1, 2 und 3 eines bestimmten Tarifs wechseln, ohne Ihre vorhandenen Vorgänge zu unterbrechen. Weitere Informationen finden Sie unter [How to upgrade your IoT hub](iot-hub-upgrade.md) (Durchführen eines Upgrades für Ihren IoT Hub).

Als Beispiel für die Datenverkehrsfunktionen der einzelnen Tarife sind hier die geltenden Durchsatzrichtlinien für Gerät-zu-Cloud-Nachrichten angegeben:

| Tarif | Anhaltender Durchsatz | Anhaltende Senderate |
| --- | --- | --- |
| B1, S1 |Bis zu 1111 KB/Minute pro Einheit<br/>(1,5 GB/Tag/Einheit) |Durchschnittlich 278 Nachrichten/Minute pro Einheit<br/>(400.000 Nachrichten/Tag pro Einheit) |
| B2, S2 |Bis zu 16 MB/Minute pro Einheit<br/>(22,8 GB/Tag/Einheit) |Durchschnittlich 4.167 Nachrichten/Minute pro Einheit<br/>(6 Millionen Nachrichten/Tag pro Einheit) |
| B3, S3 |Bis zu 814 MB/Minute pro Einheit<br/>(1.144,4 GB/Tag/Einheit) |Durchschnittlich 208.333 Nachrichten/Minute pro Einheit<br/>(300 Millionen Nachrichten/Tag pro Einheit) |

Sehen Sie sich zusätzlich zu diesen Durchsatzinformationen auch [IoT Hub-Kontingente und -Drosselungen][IoT Hub quotas and throttles] an, und entwerfen Sie Ihre Lösung entsprechend.

### <a name="identity-registry-operation-throughput"></a>Durchsatz von Identitätsregistrierungsvorgängen
IoT Hub-Identitätsregistrierungsvorgänge sollten keine Laufzeitvorgänge sein, da sie sich größtenteils auf die Gerätebereitstellung beziehen.

Genaue Zahlen zur Burstleistung finden Sie unter [IoT Hub-Kontingente und -Drosselungen][IoT Hub quotas and throttles].

## <a name="sharding"></a>Sharding (Horizontales Partitionieren)
Eine einzelne IoT Hub-Einheit kann zwar auf Millionen von Geräten skaliert werden, aber es kann sein, dass Ihre Lösung bestimmte Leistungsmerkmale benötigt, die von einer einzelnen IoT Hub-Einheit nicht gewährleistet werden können. In diesem Fall können Sie Ihre Geräte auf mehreren IoT Hubs partitionieren. Mehrere IoT Hubs glätten Datenverkehrsbursts und erzielen den erforderlichen Durchsatz oder die erforderlichen Vorgangsraten.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Funktionen und Leistungsdetails von IoT Hubs finden Sie unter [IoT Hub – Preise][link-pricing] oder [IoT Hub-Kontingente und -Drosselung][IoT Hub quotas and throttles].
* Führen Sie die Schritte unter [Upgrade your IoT hub](iot-hub-upgrade.md) (Durchführen eines Upgrades für Ihren IoT Hub) aus, um Ihren IoT Hub-Tarif zu ändern.

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md

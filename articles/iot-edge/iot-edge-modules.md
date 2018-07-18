---
title: Grundlegendes zu Azure IoT Edge-Modulen | Microsoft-Dokumentation
description: Informationen zu Azure IoT Edge-Modulen und deren Konfiguration
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 02/15/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9c196ec92fc7997617fa464d676dc93ca9fe84f0
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029089"
---
# <a name="understand-azure-iot-edge-modules"></a>Grundlegendes zu Azure IoT Edge-Modulen

Azure IoT Edge ermöglicht Ihnen die Bereitstellung und Verwaltung von Geschäftslogik im Edge in Form von *Modulen*. Azure IoT Edge-Module sind die kleinste von IoT Edge verwaltete Recheneinheit und kann Azure-Dienste (wie Azure Stream Analytics) oder Ihren eigenen lösungsspezifischen Code enthalten. Um zu verstehen, wie Module entwickelt, bereitgestellt und verwaltet werden, stellen Sie sich vier konzeptionelle Teile vor, aus denen ein Modul besteht:

* Ein **Modulimage** ist ein Paket mit der Software, die ein Modul definiert.
* Eine **Modulinstanz** ist die spezifische Recheneinheit, auf der das Modulimage auf einem IoT Edge-Gerät ausgeführt wird. Die Modulinstanz wird von der IoT Edge-Runtime gestartet.
* Eine **Modulidentität** ist eine Information (einschließlich Sicherheitsanmeldeinformationen), die in IoT Hub gespeichert und den einzelnen Modulinstanzen zugeordnet wird.
* Ein **Modulzwilling** ist ein in IoT Hub gespeichertes JSON-Dokument, das Statusinformationen für eine Modulinstanz enthält, einschließlich Metadaten, Konfigurationen und Bedingungen. 

## <a name="module-images-and-instances"></a>Modulimages und -instanzen

IoT Edge-Modulimages enthalten Anwendungen, die die Verwaltungs-, Sicherheits- und Kommunikationsfeatures der IoT Edge-Runtime nutzen. Sie können Ihre eigenen Modulimages entwickeln oder eines aus einem unterstützten Azure-Dienst exportieren, beispielsweise aus Azure Stream Analytics.
Die Images sind in der Cloud enthalten und können aktualisiert, geändert und in verschiedenen Lösungen bereitgestellt werden. Beispielsweise liegt ein Modul, das eine Fließbandausgabe anhand von Machine Learning prognostiziert, in einem anderen Image vor als ein Modul, das mithilfe von maschinellem Sehen eine Drohne steuert. 

Jedes Mal, wenn ein Modulimage auf einem Gerät bereitgestellt und von der IoT Edge-Runtime gestartet wird, wird eine neue Instanz des jeweiligen Moduls erstellt. Zwei Geräte in unterschiedlichen Teilen der Welt könnten dasselbe Modulimage verwenden; allerdings würde jedes eine eigene Modulinstanz besitzen, wenn das Modul auf dem Gerät gestartet wird. 

![Modulimages in der Cloud – Modulinstanzen auf Geräten][1]

Bei der Implementierung liegen Modulimages als Containerimages in einem Repository vor, und Modulinstanzen sind Container auf Geräten. Während die Anzahl der Anwendungsfälle für Azure IoT Edge wächst, werden neue Typen von Modulimages und -instanzen erstellt. Beispielsweise können auf Geräten mit eingeschränkten Ressourcen keine Container ausgeführt werden. Daher sind möglicherweise Modulimages erforderlich, die als Dynamic Link Librarys und Instanzen in Form von ausführbaren Dateien vorliegen. 

## <a name="module-identities"></a>Modulidentitäten

Wenn von der IoT Edge-Runtime eine neue Modulinstanz erstellt wird, wird die Instanz einer entsprechenden Modulidentität zugeordnet. Die Modulidentität wird in IoT Hub gespeichert und als Ziel für die Adressierung und Sicherheit für die gesamte Kommunikation (lokal und in der Cloud) der jeweiligen Modulinstanz verwendet.
Die einer Modulinstanz zugeordnete Identität hängt von der Identität des Geräts, auf dem die Instanz ausgeführt wird, und von dem Namen ab, den Sie in Ihrer Lösung für das Modul angeben. Wenn Sie mit `insight` beispielsweise ein Modul benennen, das Azure Stream Analytics verwendet, und dieses auf einem Gerät namens `Hannover01` bereitstellen, erstellt die IoT Edge-Runtime eine entsprechende Modulidentität namens `/devices/Hannover01/modules/insight`.

In Szenarien, in denen Sie ein Modulimage mehrmals auf demselben Gerät bereitstellen müssen, können Sie dasselbe Image natürlich mehrmals mit verschiedenen Namen bereitstellen.

![Modulidentitäten sind eindeutig][2]

## <a name="module-twins"></a>Modulzwillinge

Jede Modulinstanz besitzt außerdem einen entsprechenden Modulzwilling, den Sie zum Konfigurieren der Modulinstanz verwenden können. Die Instanz und der Zwilling werden einander über die Modulidentität zugeordnet. 

Ein Modulzwilling ist ein JSON-Dokument, in dem Modulinformationen und Konfigurationseigenschaften gespeichert sind. Dieses Konzept gleicht dem Konzept des [Gerätezwillings][lnk-device-twin] in IoT Hub. Die Struktur eines Modulzwillings ist identisch mit der eines Gerätezwillings. Die für die Interaktion mit beiden Typen von Zwillingen verwendeten APIs sind ebenfalls identisch. Der einzige Unterschied zwischen den beiden ist die zum Instanziieren des Client-SDK verwendete Identität. 

```csharp
// Create a DeviceClient object. This DeviceClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
DeviceClient client = new DeviceClient.CreateFromConnectionString(moduleConnectionString, settings); 
await client.OpenAsync(); 
 
// Get the model twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="offline-capabilities"></a>Offlinefunktionen

Azure IoT Edge unterstützt Offlinevorgänge auf Ihren IoT Edge-Geräten. Diese Funktionen sind jetzt eingeschränkt, und es werden zusätzliche Szenarien entwickelt. 

IoT Edge-Module können sich längere Zeit im Offlinemodus befinden, solange die folgenden Anforderungen erfüllt sind: 

* **Die Gültigkeitsdauer (TTL) der Nachricht ist nicht abgelaufen**. Der Standardwert für die Gültigkeitsdauer von Nachrichten beträgt zwei Stunden, aber er kann in den IoT Edge Hub-Einstellungen unter „Konfiguration speichern und weiterleiten“ erhöht bzw. verringert werden. 
* **Module müssen sich gegenüber dem IoT Edge Hub nicht erneut authentifizieren, wenn sie sich im Offlinezustand befinden**. Module können sich nur gegenüber Edge Hubs authentifizieren, die über eine aktive Verbindung mit einem IoT Hub verfügen. Module müssen sich erneut authentifizieren, falls sie aus irgendeinem Grund neu gestartet werden. Module können weiterhin Nachrichten an den Edge Hub senden, nachdem ihr SAS-Token abgelaufen ist. Nachdem die Konnektivität wiederhergestellt wurde, fordert der Edge Hub ein neues Token vom Modul an und überprüft es mit dem IoT Hub. Wenn die Überprüfung erfolgreich ist, leitet der Edge Hub die gespeicherten Modulnachrichten weiter. Dies gilt auch für die Nachrichten, die gesendet wurden, während das Token des Moduls abgelaufen war. 
* **Das Modul, mit dem die Nachrichten im Offlinezustand gesendet wurden, ist noch betriebsbereit, wenn die Konnektivität wiederhergestellt wird**. Beim Wiederherstellen der Verbindung mit dem IoT Hub muss der Edge Hub ein neues Modultoken überprüfen (falls das vorherige abgelaufen ist), bevor er die Modulnachrichten weiterleiten kann. Wenn das Modul nicht verfügbar ist, um ein neues Token bereitzustellen, kann der Edge Hub für die gespeicherten Nachrichten des Moduls keine Aktionen durchführen. 
* **Der Edge Hub verfügt auf dem Datenträger über Speicherplatz für die Nachrichten**. Nachrichten werden standardmäßig im Dateisystem des Edge Hub-Containers gespeichert. Es ist eine Konfigurationsoption zum Angeben eines bereitgestellten Volumes vorhanden, auf dem die Nachrichten stattdessen gespeichert werden können. In beiden Fällen muss Platz zum Speichern der Nachrichten für die verzögerte Bereitstellung für einen IoT Hub vorhanden sein.  

## <a name="next-steps"></a>Nächste Schritte
 - [Grundlegendes zur Azure IoT Edge-Laufzeit und ihrer Architektur][lnk-runtime]

<!-- Images -->
[1]: ./media/iot-edge-modules/image_instance.png
[2]: ./media/iot-edge-modules/identity.png

<!-- Links -->
[lnk-device-identity]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-runtime]: iot-edge-runtime.md
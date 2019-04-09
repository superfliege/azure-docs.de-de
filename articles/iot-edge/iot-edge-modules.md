---
title: Ausführen von Logik auf Geräten mithilfe von Modulen – Azure IoT Edge | Microsoft-Dokumentation
description: Azure IoT Edge-Module sind Containereinheiten mit Logik, die remote bereitgestellt und verwaltet werden können, sodass Sie Geschäftslogik auf IoT Edge-Geräten ausführen können.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: d1e2e35dafd90c16e9d0dbf38afb1e981653d1fe
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311100"
---
# <a name="understand-azure-iot-edge-modules"></a>Grundlegendes zu Azure IoT Edge-Modulen

Azure IoT Edge ermöglicht Ihnen die Bereitstellung und Verwaltung von Geschäftslogik im Edge in Form von *Modulen*. Azure IoT Edge-Module sind die kleinste von IoT Edge verwaltete Recheneinheit und kann Azure-Dienste (wie Azure Stream Analytics) oder Ihren eigenen lösungsspezifischen Code enthalten. Um zu verstehen, wie Module entwickelt, bereitgestellt und verwaltet werden, können Sie sich die vier konzeptionellen Elemente eines Moduls vorstellen:

* Ein **Modulimage** ist ein Paket mit der Software, die ein Modul definiert.
* Eine **Modulinstanz** ist die spezifische Recheneinheit, auf der das Modulimage auf einem IoT Edge-Gerät ausgeführt wird. Die Modulinstanz wird von der IoT Edge-Runtime gestartet.
* Eine **Modulidentität** ist eine Information (einschließlich Sicherheitsanmeldeinformationen), die in IoT Hub gespeichert und den einzelnen Modulinstanzen zugeordnet wird.
* Ein **Modulzwilling** ist ein in IoT Hub gespeichertes JSON-Dokument, das Statusinformationen für eine Modulinstanz enthält, einschließlich Metadaten, Konfigurationen und Bedingungen. 

## <a name="module-images-and-instances"></a>Modulimages und -instanzen

IoT Edge-Modulimages enthalten Anwendungen, die die Verwaltungs-, Sicherheits- und Kommunikationsfeatures der IoT Edge-Runtime nutzen. Sie können Ihre eigenen Modulimages entwickeln oder eines aus einem unterstützten Azure-Dienst exportieren, beispielsweise aus Azure Stream Analytics.
Die Images sind in der Cloud enthalten und können aktualisiert, geändert und in verschiedenen Lösungen bereitgestellt werden. Beispielsweise liegt ein Modul, das eine Fließbandausgabe anhand von Machine Learning prognostiziert, in einem anderen Image vor als ein Modul, das mithilfe von maschinellem Sehen eine Drohne steuert. 

Jedes Mal, wenn ein Modulimage auf einem Gerät bereitgestellt und von der IoT Edge-Runtime gestartet wird, wird eine neue Instanz des jeweiligen Moduls erstellt. Zwei Geräte in verschiedenen Teilen der Welt könnten dasselbe Modulimage verwenden. Allerdings würde jedes Gerät über eine eigene Modulinstanz verfügen, wenn das Modul auf dem Gerät gestartet wird. 

![Diagramm: Modulimages in der Cloud, Modulinstanzen auf Geräten](./media/iot-edge-modules/image_instance.png)

Bei der Implementierung liegen Modulimages als Containerimages in einem Repository vor, und Modulinstanzen sind Container auf Geräten. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Modulidentitäten

Wenn von der IoT Edge-Runtime eine neue Modulinstanz erstellt wird, wird die Instanz einer entsprechenden Modulidentität zugeordnet. Die Modulidentität wird in IoT Hub gespeichert und als Ziel für die Adressierung und Sicherheit für die gesamte Kommunikation (lokal und in der Cloud) der jeweiligen Modulinstanz verwendet.

Die einer Modulinstanz zugeordnete Identität hängt von der Identität des Geräts, auf dem die Instanz ausgeführt wird, und von dem Namen ab, den Sie in Ihrer Lösung für das Modul angeben. Wenn Sie mit `insight` beispielsweise ein Modul benennen, das Azure Stream Analytics verwendet, und dieses auf einem Gerät namens `Hannover01` bereitstellen, erstellt die IoT Edge-Runtime eine entsprechende Modulidentität namens `/devices/Hannover01/modules/insight`.

In Szenarien, in denen Sie ein Modulimage mehrmals auf demselben Gerät bereitstellen müssen, können Sie dasselbe Image natürlich mehrmals mit verschiedenen Namen bereitstellen.

![Diagramm: Modulidentitäten sind in Geräten und geräteübergreifend eindeutig](./media/iot-edge-modules/identity.png)

## <a name="module-twins"></a>Modulzwillinge

Jede Modulinstanz besitzt außerdem einen entsprechenden Modulzwilling, den Sie zum Konfigurieren der Modulinstanz verwenden können. Die Instanz und der Zwilling werden einander über die Modulidentität zugeordnet. 

Ein Modulzwilling ist ein JSON-Dokument, in dem Modulinformationen und Konfigurationseigenschaften gespeichert sind. Dieses Konzept gleicht dem Konzept des [Gerätezwillings](../iot-hub/iot-hub-devguide-device-twins.md) in IoT Hub. Die Struktur eines Modulzwillings ist identisch mit der eines Gerätezwillings. Die für die Interaktion mit beiden Typen von Zwillingen verwendeten APIs sind ebenfalls identisch. Der einzige Unterschied zwischen den beiden ist die zum Instanziieren des Client-SDK verwendete Identität. 

```csharp
// Create a ModuleClient object. This ModuleClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(settings); 
await client.OpenAsync(); 
 
// Get the module twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="offline-capabilities"></a>Offlinefunktionen

Azure IoT Edge unterstützt Offlinevorgänge auf Ihren IoT Edge-Geräten. Diese Funktionen sind jetzt eingeschränkt. Zusätzliche Offlinefunktionen sind in der öffentlichen Vorschauversion verfügbar. Weitere Informationen finden Sie unter [Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte](offline-capabilities.md).

IoT Edge-Module können sich längere Zeit im Offlinemodus befinden, solange die folgenden Anforderungen erfüllt sind: 

* **Die Gültigkeitsdauer (TTL) der Nachricht ist nicht abgelaufen**. Der Standardwert für die Gültigkeitsdauer von Nachrichten beträgt zwei Stunden, aber er kann in den IoT Edge Hub-Einstellungen unter „Konfiguration speichern und weiterleiten“ erhöht bzw. verringert werden. 
* **Module müssen sich gegenüber dem IoT Edge Hub nicht erneut authentifizieren, wenn sie sich im Offlinezustand befinden**. Module können sich nur gegenüber IoT Edge Hubs authentifizieren, die über eine aktive Verbindung mit einem IoT-Hub verfügen. Module müssen sich erneut authentifizieren, falls sie aus irgendeinem Grund neu gestartet werden. Module können weiterhin Nachrichten an den IoT Edge-Hub senden, nachdem ihr SAS-Token abgelaufen ist. Nachdem die Konnektivität wiederhergestellt wurde, fordert der IoT Edge-Hub ein neues Token vom Modul an und überprüft es mit dem IoT-Hub. Wenn die Überprüfung erfolgreich ist, leitet der IoT Edge-Hub die gespeicherten Modulnachrichten weiter. Dies gilt auch für die Nachrichten, die gesendet wurden, während das Token des Moduls abgelaufen war. 
* **Das Modul, mit dem die Nachrichten im Offlinezustand gesendet wurden, ist noch betriebsbereit, wenn die Konnektivität wiederhergestellt wird**. Beim Wiederherstellen der Verbindung mit IoT Hub muss der IoT Edge-Hub ein neues Modultoken überprüfen (falls das vorherige abgelaufen ist), bevor er die Modulnachrichten weiterleiten kann. Wenn das Modul nicht verfügbar ist, um ein neues Token bereitzustellen, kann der IoT Edge-Hub für die gespeicherten Nachrichten des Moduls keine Aktionen durchführen. 
* **Der IoT Edge-Hub verfügt auf dem Datenträger über Speicherplatz für die Nachrichten**. Nachrichten werden standardmäßig im Dateisystem des IoT Edge-Hub-Containers gespeichert. Es ist eine Konfigurationsoption zum Angeben eines bereitgestellten Volumes vorhanden, auf dem die Nachrichten stattdessen gespeichert werden können. In beiden Fällen muss Platz zum Speichern der Nachrichten für die verzögerte Bereitstellung für einen IoT Hub vorhanden sein.  


## <a name="next-steps"></a>Nächste Schritte
 - [Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen](module-development.md)
 - [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md)


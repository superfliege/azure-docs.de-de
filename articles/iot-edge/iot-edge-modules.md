---
title: Grundlegendes zu Azure IoT Edge-Modulen | Microsoft-Dokumentation
description: Informationen zu Azure IoT Edge-Modulen und deren Konfiguration
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3d76f5931e3636f19c2030c4090116a0791db819
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567331"
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

![Modulimages in der Cloud – Modulinstanzen auf Geräten](./media/iot-edge-modules/image_instance.png)

Bei der Implementierung liegen Modulimages als Containerimages in einem Repository vor, und Modulinstanzen sind Container auf Geräten. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Modulidentitäten

Wenn von der IoT Edge-Runtime eine neue Modulinstanz erstellt wird, wird die Instanz einer entsprechenden Modulidentität zugeordnet. Die Modulidentität wird in IoT Hub gespeichert und als Ziel für die Adressierung und Sicherheit für die gesamte Kommunikation (lokal und in der Cloud) der jeweiligen Modulinstanz verwendet.
Die einer Modulinstanz zugeordnete Identität hängt von der Identität des Geräts, auf dem die Instanz ausgeführt wird, und von dem Namen ab, den Sie in Ihrer Lösung für das Modul angeben. Wenn Sie mit `insight` beispielsweise ein Modul benennen, das Azure Stream Analytics verwendet, und dieses auf einem Gerät namens `Hannover01` bereitstellen, erstellt die IoT Edge-Runtime eine entsprechende Modulidentität namens `/devices/Hannover01/modules/insight`.

In Szenarien, in denen Sie ein Modulimage mehrmals auf demselben Gerät bereitstellen müssen, können Sie dasselbe Image natürlich mehrmals mit verschiedenen Namen bereitstellen.

![Modulidentitäten sind eindeutig](./media/iot-edge-modules/identity.png)

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

Azure IoT Edge unterstützt Offlinevorgänge auf Ihren IoT Edge-Geräten. Diese Funktionen sind jetzt eingeschränkt. 

IoT Edge-Module können sich längere Zeit im Offlinemodus befinden, solange die folgenden Anforderungen erfüllt sind: 

* **Die Gültigkeitsdauer (TTL) der Nachricht ist nicht abgelaufen**. Der Standardwert für die Gültigkeitsdauer von Nachrichten beträgt zwei Stunden, aber er kann in den IoT Edge Hub-Einstellungen unter „Konfiguration speichern und weiterleiten“ erhöht bzw. verringert werden. 
* **Module müssen sich gegenüber dem IoT Edge Hub nicht erneut authentifizieren, wenn sie sich im Offlinezustand befinden**. Module können sich nur gegenüber Edge Hubs authentifizieren, die über eine aktive Verbindung mit einem IoT Hub verfügen. Module müssen sich erneut authentifizieren, falls sie aus irgendeinem Grund neu gestartet werden. Module können weiterhin Nachrichten an den Edge Hub senden, nachdem ihr SAS-Token abgelaufen ist. Nachdem die Konnektivität wiederhergestellt wurde, fordert der Edge Hub ein neues Token vom Modul an und überprüft es mit dem IoT Hub. Wenn die Überprüfung erfolgreich ist, leitet der Edge Hub die gespeicherten Modulnachrichten weiter. Dies gilt auch für die Nachrichten, die gesendet wurden, während das Token des Moduls abgelaufen war. 
* **Das Modul, mit dem die Nachrichten im Offlinezustand gesendet wurden, ist noch betriebsbereit, wenn die Konnektivität wiederhergestellt wird**. Beim Wiederherstellen der Verbindung mit dem IoT Hub muss der Edge Hub ein neues Modultoken überprüfen (falls das vorherige abgelaufen ist), bevor er die Modulnachrichten weiterleiten kann. Wenn das Modul nicht verfügbar ist, um ein neues Token bereitzustellen, kann der Edge Hub für die gespeicherten Nachrichten des Moduls keine Aktionen durchführen. 
* **Der Edge Hub verfügt auf dem Datenträger über Speicherplatz für die Nachrichten**. Nachrichten werden standardmäßig im Dateisystem des Edge Hub-Containers gespeichert. Es ist eine Konfigurationsoption zum Angeben eines bereitgestellten Volumes vorhanden, auf dem die Nachrichten stattdessen gespeichert werden können. In beiden Fällen muss Platz zum Speichern der Nachrichten für die verzögerte Bereitstellung für einen IoT Hub vorhanden sein.  

Zusätzliche Offlinefunktionen sind in der öffentlichen Vorschauversion verfügbar. Weitere Informationen finden Sie unter [Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte](offline-capabilities.md).

## <a name="next-steps"></a>Nächste Schritte
 - [Grundlegendes zu den Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen](module-development.md)
 - [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md)

<!-- Images -->
[2]: ./media/iot-edge-modules/identity.png

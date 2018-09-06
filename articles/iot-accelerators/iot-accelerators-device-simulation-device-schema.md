---
title: Geräteschema in der Gerätesimulationslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das JSON-Schema, das ein simuliertes Gerät in der Gerätesimulationslösung definiert.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: b325873819caff139727ec15d6aecd2d4be89c9e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338154"
---
# <a name="understand-the-device-model-schema"></a>Grundlegendes zum Gerätemodellschema

Sie können den Solution Accelerator für Gerätesimulation verwenden, um Testauslastungen für Lösungen zu generieren, die IoT Hub nutzen. Beim Bereitstellen der Gerätesimulationslösung wird automatisch eine Sammlung simulierter Geräte bereitgestellt. Sie können die vorhandenen simulierten Geräte anpassen oder eigene erstellen.

Dieser Artikel beschreibt das Gerätemodellschema, das die Funktionen und das Verhalten eines simulierten Geräts bestimmt. Das Gerätemodell ist in einer JSON-Datei gespeichert.

Die folgenden Artikel beziehen sich auf den aktuellen Artikel:

* [Implementieren des Gerätemodellverhaltens](iot-accelerators-device-simulation-device-behavior.md) beschreibt die JavaScript-Dateien, mit denen Sie das Verhalten eines simulierten Geräts implementieren.
* [Erstellen eines neuen simuliertes Geräts](iot-accelerators-device-simulation-create-simulated-device.md) zeigt, wie Sie einen neuen simulierten Gerätetyp in Ihrer Lösung bereitstellen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

>[!div class="checklist"]
> * Verwenden einer JSON-Datei, um ein simuliertes Gerätemodell zu definieren
> * Festlegen der Eigenschaften des simulierten Geräts
> * Festlegen der Telemetrie, die das simulierte Gerät sendet
> * Festlegen der Cloud-zu-Gerät-Methoden (C2D), auf die das Gerät reagiert

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie Ihr eigenes benutzerdefiniertes simuliertes Gerätemodell erstellen. In diesem Artikel haben Sie erfahren, wie Sie Folgendes durchführen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Verwenden einer JSON-Datei, um ein simuliertes Gerätemodell zu definieren
> * Festlegen der Eigenschaften des simulierten Geräts
> * Festlegen der Telemetrie, die das simulierte Gerät sendet
> * Festlegen der Cloud-zu-Gerät-Methoden (C2D), auf die das Gerät reagiert

Nachdem Sie nun mit dem JSON-Schema vertraut sind, ist es ratsam, mehr über das [Implementieren des Verhaltens Ihres simulierten Geräts](iot-accelerators-device-simulation-device-behavior.md) zu erfahren.

Weitere Entwicklerinformationen zur Gerätesimulationslösung finden Sie im [Referenzhandbuch für Entwickler](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).

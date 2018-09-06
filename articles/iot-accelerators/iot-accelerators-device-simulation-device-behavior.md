---
title: Verhalten simulierter Geräte in einer Gerätesimulationslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie mit JavaScript das Verhalten eines simulierten Geräts in der Gerätesimulationslösung definieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 43edbc653ddbd55aab5e722071de1f2cf4bcd1c4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344515"
---
# <a name="implement-the-device-model-behavior"></a>Implementieren des Gerätemodellverhaltens

Der Artikel [Grundlegendes zum Gerätemodellschema](iot-accelerators-device-simulation-device-schema.md) beschreibt das Schema, das ein simuliertes Gerätemodell definiert. Dieser Artikel bezieht sich auf zwei Arten der JavaScript-Datei, die das Verhalten eines simulierten Geräts implementiert:

- **State** (Zustand): JavaScript-Dateien, die in festen Intervallen ausgeführt werden, um den internen Status des Geräts zu aktualisieren
- **Method** (Methode): JavaScript-Dateien, die ausgeführt werden, wenn die Lösung eine Methode auf dem Gerät aufruft

In diesem Artikel werden folgende Vorgehensweisen behandelt:

>[!div class="checklist"]
> * Steuern des Status eines simulierten Geräts
> * Definieren, wie ein simuliertes Gerät auf einen Methodenaufruf von der IoT Hub-Instanz reagiert, mit der eine Verbindung besteht
> * Debuggen Ihrer Skripts

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie das Verhalten Ihres eigenen benutzerdefinierten simulierten Gerätemodells definieren. In diesem Artikel haben Sie erfahren, wie Sie Folgendes durchführen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Steuern des Status eines simulierten Geräts
> * Definieren, wie ein simuliertes Gerät auf einen Methodenaufruf von der IoT Hub-Instanz reagiert, mit der eine Verbindung besteht
> * Debuggen Ihrer Skripts

Jetzt wissen Sie, wie Sie das Verhalten eines simulierten Geräts festlegen, und sollten als Nächstes das [Erstellen eines neuen simulierten Geräts](iot-accelerators-device-simulation-create-simulated-device.md) kennenlernen.

Weitere Entwicklerinformationen zur Gerätesimulationslösung finden Sie im [Referenzhandbuch für Entwickler](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).

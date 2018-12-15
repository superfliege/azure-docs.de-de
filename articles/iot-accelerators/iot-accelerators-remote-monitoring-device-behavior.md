---
title: Verhalten simulierter Geräte in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie JavaScript verwenden, um das Verhalten eines simulierten Geräts in der Remoteüberwachungslösung zu definieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 70f9ccbbe737bad4d6f88365e804d4421c418d28
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53164006"
---
# <a name="implement-the-device-model-behavior"></a>Implementieren des Gerätemodellverhaltens

Der Artikel [Grundlegendes zum Gerätemodellschema](iot-accelerators-remote-monitoring-device-schema.md) beschreibt das Schema, das ein simuliertes Gerätemodell definiert. Dieser Artikel bezieht sich auf zwei Arten der JavaScript-Datei, die das Verhalten eines simulierten Geräts implementiert:

- **State**-JavaScript-Dateien, die in festen Intervallen ausgeführt werden, um den internen Status des Geräts zu aktualisieren.
- **Method**-JavaScript-Dateien, die ausgeführt werden, wenn die Lösung eine Methode auf dem Gerät aufruft.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

>[!div class="checklist"]
> * Steuern des Status eines simulierten Geräts
> * Definieren, wie ein simuliertes Gerät auf einen Methodenaufruf von der Remoteüberwachungslösung reagiert
> * Debuggen Ihrer Skripts

[!INCLUDE [iot-accelerators-device-behavior](../../includes/iot-accelerators-device-behavior.md)]

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie das Verhalten Ihres eigenen benutzerdefinierten simulierten Gerätemodells definieren. In diesem Artikel haben Sie erfahren, wie Sie Folgendes durchführen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Steuern des Status eines simulierten Geräts
> * Definieren, wie ein simuliertes Gerät auf einen Methodenaufruf von der Remoteüberwachungslösung reagiert
> * Debuggen Ihrer Skripts

Jetzt wissen Sie, wie Sie das Verhalten eines simulierten Geräts festlegen, und sollten als Nächstes das [Erstellen eines neuen simulierten Geräts](iot-accelerators-remote-monitoring-create-simulated-device.md) kennenlernen.

Weitere Entwicklerinformationen zur Remoteüberwachungslösung finden Sie hier:

* [Referenzhandbuch für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Handbuch zur Problembehandlung für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->

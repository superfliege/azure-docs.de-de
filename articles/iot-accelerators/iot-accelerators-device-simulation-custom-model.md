---
title: Konfigurieren eines benutzerdefinierten Gerätemodells – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie ein benutzerdefiniertes Gerätemodell im Solution Accelerator für Gerätesimulation konfigurieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: 61a7c5314df541b4b44a286efe982f4bf93256e3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967477"
---
# <a name="device-models"></a>Gerätemodelle

Beim Konfigurieren einer Simulation können Sie ein vorhandenes Gerätemodell mit einem vordefinierten Satz von Sensoren verwenden oder ein benutzerdefiniertes Gerätemodell mit simulierten Sensoren Ihrer Wahl erstellen. Mit benutzerdefinierten Sensoren können Sie Ihre echten Geräte genauer modellieren.

## <a name="pre-configured-device-models"></a>Vorkonfigurierte Gerätemodelle

Der Solution Accelerator für Gerätesimulation bietet drei vorkonfigurierte Gerätemodelle: Kältemaschinen, Aufzüge und Lkw.

Vorkonfigurierte Gerätemodelle weisen mehrere Sensoren mit erweiterten, in einer JavaScript-Datei definierten Verhaltensweisen auf. Sie können diese Verhaltensweisen nicht auf der Web-Benutzeroberfläche konfigurieren.

Die folgende Tabelle zeigt die Konfigurationen für jedes vorkonfigurierte Gerätemodell:

| Gerätemodell  | Sensor           | Unit  |
| ------------- | ---------------- | ----- |
| Kältemaschine       | Luftfeuchtigkeit         | %     |
|               | pressure         | psig  |
|               | Temperatur      | F     |
| Aufzug      | Etage            |       |
|               | Vibration        | MM    |
|               | Temperatur      | F     |
| LKW         | Breitengrad         |       |
|               | Längengrad        |       |
|               | Geschwindigkeit            | mph   |
|               | Temperatur Ladung | F     |

## <a name="custom-device-models"></a>Benutzerdefinierte Gerätemodelle

Mit benutzerdefinierten Gerätemodellen können Sie Sensoren modellieren, die Ihre eigenen Geräte genauer modellieren. Ein benutzerdefiniertes Gerät kann bis zu 10 benutzerdefinierte Sensoren enthalten.

Wenn Sie den Modelltyp für das benutzerdefinierte Gerät auswählen, fügen Sie neue Sensoren hinzu, indem Sie auf **+ Sensor hinzufügen** klicken.

[![Konfigurieren von Sensoren](./media/iot-accelerators-device-simulation-custom-model/configuresensors-inline.png)](./media/iot-accelerators-device-simulation-custom-model/configuresensors-expanded.png#lightbox)

Benutzerdefinierte Sensoren verfügen über folgende Eigenschaften:

| Feld     | BESCHREIBUNG |
| --------- | ----------- |
| Name des Sensors | Ein Anzeigename für den Sensor wie z.B. **Temperatur** oder **Geschwindigkeit**.  |
| Verhalten  | Die Einstellung „Verhalten“ ermöglicht es, dass Telemetriedaten zwischen den verschiedenen Meldungen variieren, um reale Daten zu simulieren. **Erhöhen** startet beim Mindestwert und erhöht den Wert in jeder gesendeten Meldung um eins. Wenn der Höchstwert erreicht ist, beginnt dieses Verhalten beim Mindestwert wieder von vorne. **Verringern** verhält sich genauso wie **Erhöhen**, zählt jedoch abwärts. Das Verhalten **Zufällig** generiert einen zufälligen Wert zwischen dem Mindest- und den Höchstwerten. |
| Mindestwert | Die kleinste Zahl im akzeptablen Bereich. |
| Höchstwert | Die größte Zahl im akzeptablen Bereich. |
| Unit      | Die Maßeinheit für den Sensor, z.B. „%“ oder „mm“. |

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie gelernt, wie Sie ein benutzerdefiniertes Gerätemodell für eine Simulation konfigurieren. Als Nächstes sollten Sie einige der anderen [IoT-Solution Accelerators](about-iot-accelerators.md) untersuchen.
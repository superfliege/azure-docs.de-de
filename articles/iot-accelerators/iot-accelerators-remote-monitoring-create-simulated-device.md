---
title: Gerätesimulation mit IoT-Remoteüberwachung – Azure | Microsoft-Dokumentation
description: In dieser Anleitung wird gezeigt, wie Sie den Gerätesimulator mit dem Solution Accelerator für Remoteüberwachung verwenden.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: 9d15174bc777b20eb7f3988dc33be15c46b1cc43
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098135"
---
# <a name="create-and-test-a-new-simulated-device"></a>Erstellen und Testen eines neuen simulierten Geräts

Mithilfe des Solution Accelerators für Remoteüberwachung können Sie Ihre eigenen simulierten Geräte definieren. In diesem Artikel wird gezeigt, wie Sie ein neues simuliertes "Lighbulb"-Gerät (Leuchtmittel) definieren und anschließend lokal testen. Der Solution Accelerator enthält simulierte Geräte, z. B. Chillers (Kältemaschinen) und LKWs. Sie können jedoch Ihre eigenen simulierten Geräte definieren, um Ihre IoT-Lösungen zu testen, bevor Sie Echtgeräte bereitstellen.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie simulierte Geräte verwenden, die im Gerätesimulationsdienst gehostet werden. Wenn Sie ein echtes Gerät erstellen möchten, finden Sie weitere Informationen unter [Herstellen einer Verbindung zwischen Ihrem Gerät und dem Solution Accelerator für Remoteüberwachung](iot-accelerators-connecting-devices.md).

Diese Anleitung zeigt Ihnen, wie Sie den Microservice "Gerätesimulation" anpassen. Dieser Microservice ist ein Bestandteil des Solution Accelerators für Remoteüberwachung. Zur Veranschaulichung der Gerätesimulationsfunktionen werden in dieser Anleitung zwei Szenarien in der Contoso IoT-Anwendung verwendet:

[!INCLUDE [iot-solution-accelerators-create-device](../../includes/iot-solution-accelerators-create-device.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung wurde gezeigt, wie Sie benutzerdefinierte simulierte Gerätetypen erstellen und testen, indem Sie den Microservice "Gerätesimulation" lokal ausführen.

Im nächsten empfohlenen Schritt erfahren Sie, wie Sie Ihre benutzerdefinierten simulierten Gerätetypen für den [Solution Accelerator für Remoteüberwachung](iot-accelerators-remote-monitoring-deploy-simulated-device.md) bereitstellen.

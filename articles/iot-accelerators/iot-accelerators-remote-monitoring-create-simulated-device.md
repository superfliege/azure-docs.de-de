---
title: Gerätesimulation mit IoT-Remoteüberwachung – Azure | Microsoft-Dokumentation
description: In dieser Anleitung wird gezeigt, wie Sie den Gerätesimulator mit dem Solution Accelerator für Remoteüberwachung verwenden.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/28/2018
ms.topic: conceptual
ms.openlocfilehash: 06909c06087219cdfa6edccbfed70ea1694e5a03
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633726"
---
# <a name="create-and-test-a-new-simulated-device"></a>Erstellen und Testen eines neuen simulierten Geräts

Mithilfe des Solution Accelerators für Remoteüberwachung können Sie Ihre eigenen simulierten Geräte definieren. In diesem Artikel wird gezeigt, wie Sie ein neues simuliertes "Lighbulb"-Gerät (Leuchtmittel) definieren und anschließend lokal testen. Der Solution Accelerator enthält simulierte Geräte, z. B. Chillers (Kältemaschinen) und LKWs. Sie können jedoch Ihre eigenen simulierten Geräte definieren, um Ihre IoT-Lösungen zu testen, bevor Sie Echtgeräte bereitstellen.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie simulierte Geräte verwenden, die im Gerätesimulationsdienst gehostet werden. Wenn Sie ein physisches Gerät erstellen möchten, lesen Sie [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Node.js)](iot-accelerators-connecting-devices.md).

Diese Anleitung zeigt Ihnen, wie Sie den Microservice "Gerätesimulation" anpassen. Dieser Microservice ist ein Bestandteil des Solution Accelerators für Remoteüberwachung. Zur Veranschaulichung der Gerätesimulationsfunktionen werden in dieser Anleitung zwei Szenarien in der Contoso IoT-Anwendung verwendet:

[!INCLUDE [iot-solution-accelerators-create-device](../../includes/iot-solution-accelerators-create-device.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung wurde gezeigt, wie Sie benutzerdefinierte simulierte Gerätetypen erstellen und testen, indem Sie den Microservice "Gerätesimulation" lokal ausführen.

Im nächsten empfohlenen Schritt erfahren Sie, wie Sie Ihre benutzerdefinierten simulierten Gerätetypen für den [Solution Accelerator für Remoteüberwachung](iot-accelerators-remote-monitoring-deploy-simulated-device.md) bereitstellen.

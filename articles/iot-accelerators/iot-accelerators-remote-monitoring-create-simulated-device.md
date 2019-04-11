---
title: Gerätesimulation mit IoT-Remoteüberwachung – Azure | Microsoft-Dokumentation
description: In dieser Anleitung wird gezeigt, wie Sie den Gerätesimulator mit dem Solution Accelerator für die Remoteüberwachung verwenden.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 81efd9dc5d33ed23574b1cb66f26ccb444d5a3ff
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58180533"
---
# <a name="create-and-test-a-new-simulated-device"></a>Erstellen und Testen eines neuen simulierten Geräts

Mithilfe des Solution Accelerators für die Remoteüberwachung können Sie Ihre eigenen simulierten Geräte definieren. In diesem Artikel wird gezeigt, wie Sie ein neues simuliertes "Lighbulb"-Gerät (Leuchtmittel) definieren und anschließend lokal testen. Der Solution Accelerator enthält simulierte Geräte, z. B. Chillers (Kältemaschinen) und LKWs. Sie können jedoch Ihre eigenen simulierten Geräte definieren, um Ihre IoT-Lösungen zu testen, bevor Sie Echtgeräte bereitstellen.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie simulierte Geräte verwenden, die im Gerätesimulationsdienst gehostet werden. Wenn Sie ein echtes Gerät erstellen möchten, finden Sie weitere Informationen unter [Herstellen einer Verbindung zwischen Ihrem Gerät und dem Solution Accelerator für die Remoteüberwachung](iot-accelerators-connecting-devices.md).

Diese Anleitung zeigt Ihnen, wie Sie den Microservice "Gerätesimulation" anpassen. Dieser Microservice ist ein Bestandteil des Solution Accelerators für die Remoteüberwachung. Zur Veranschaulichung der Gerätesimulationsfunktionen werden in dieser Anleitung zwei Szenarien in der Contoso IoT-Anwendung verwendet:

[!INCLUDE [iot-solution-accelerators-create-device](../../includes/iot-solution-accelerators-create-device.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung wurde gezeigt, wie Sie benutzerdefinierte simulierte Gerätetypen erstellen und testen, indem Sie den Microservice "Gerätesimulation" lokal ausführen.

Im nächsten empfohlenen Schritt erfahren Sie, wie Sie Ihre benutzerdefinierten simulierten Gerätetypen für den [Solution Accelerator für die Remoteüberwachung](iot-accelerators-remote-monitoring-deploy-simulated-device.md) bereitstellen.

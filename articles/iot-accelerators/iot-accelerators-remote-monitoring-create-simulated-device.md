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
ms.openlocfilehash: 7a7a32cf1d67e9a4bbe49996b258164eb25c3763
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446764"
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

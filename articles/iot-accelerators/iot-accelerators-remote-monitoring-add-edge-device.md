---
title: 'Hinzufügen eines Edge-Geräts in der Remoteüberwachungslösung: Azure | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie einem Solution Accelerator für die Remoteüberwachung ein IoT Edge-Gerät hinzufügen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/09/2018
ms.topic: conceptual
ms.openlocfilehash: 67bfde828287d9892ad404f3d950dbe373503a56
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51827242"
---
# <a name="add-an-iot-edge-device-to-your-remote-monitoring-solution-accelerator"></a>Hinzufügen eines IoT Edge-Geräts zu Ihrem Solution Accelerator für die Remoteüberwachung

Führen Sie zum Hinzufügen eines [IoT Edge](../iot-edge/about-iot-edge.md)-Geräts zu Ihrem Solution Accelerator die folgenden zwei Schritte aus:

1. Fügen Sie das Edge-Gerät über die Seite **Geräte** der Webbenutzeroberfläche des Solution Accelerators für Remoteüberwachung hinzu.
1. Installieren Sie die IoT Edge-Runtime auf Ihrem Edge-Gerät.

## <a name="add-the-iot-edge-device"></a>Hinzufügen des IoT Edge-Geräts

Um dem Solution Accelerator für die Remoteüberwachung ein IoT Edge-Gerät hinzuzufügen, navigieren Sie in der Webbenutzeroberfläche zur Seite **Geräte**, und klicken Sie auf **+ Neues Gerät**.

Wählen Sie im Bereich **Neues Gerät** die Option **IoT Edge-Gerät** aus: Für alle anderen Einstellungen können Sie die Standardwerte beibehalten. Klicken Sie anschließend auf **Übernehmen**:

![Hinzufügen eines IoT Edge-Geräts](media/iot-accelerators-remote-monitoring-add-edge-device/addedgedevice.png)

### <a name="alternative-ways-to-add-an-iot-edge-device"></a>Alternative Möglichkeiten zum Hinzufügen eines IoT Edge-Geräts

Sie können ein IoT Edge-Gerät auch direkt bei der IoT Hub-Instanz in Ihrem Solution Accelerator registrieren. Sie benötigen den Namen des IoT-Hubs in Ihrem Solution Accelerator, damit Sie eine der folgenden Schrittanleitungen ausführen können:

- [Registrieren eines neuen Azure IoT Edge-Geräts über das Azure-Portal](../iot-edge/how-to-register-device-portal.md)
- [Registrieren eines neuen Azure IoT Edge-Geräts mithilfe der Azure-Befehlszeilenschnittstelle](../iot-edge/how-to-register-device-cli.md)
- [Registrieren eines neuen Azure IoT Edge-Geräts über Visual Studio Code](../iot-edge/how-to-register-device-vscode.md)

Wenn Sie ein Gerät direkt beim IoT-Hub im Solution Accelerator für die Remoteüberwachung registrieren, wird es auf der Webbenutzeroberfläche auf der Seite **Geräte** aufgelistet.

## <a name="install-the-iot-edge-runtime"></a>Installieren der IoT Edge-Runtime

Damit Sie Module auf Ihrem Edge-Gerät bereitstellen können, müssen Sie die IoT Edge-Runtime auf dem physischen Gerät installieren. In den folgenden Schrittanleitungen wird gezeigt, wie Sie die Runtime auf allgemeinen Geräteplattformen installieren:

- [Installieren der Azure IoT Edge-Runtime unter Linux (x64)](../iot-edge/how-to-install-iot-edge-linux.md)
- [Installieren der Azure IoT Edge-Runtime unter Linux (ARM32v7/armhf)](../iot-edge/how-to-install-iot-edge-linux-arm.md)
- [Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung mit Windows-Containern](../iot-edge/how-to-install-iot-edge-windows-with-windows.md)
- [Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung mit Linux-Containern](../iot-edge/how-to-install-iot-edge-windows-with-linux.md)
- [Installieren der IoT Edge-Runtime unter Windows IoT Core – Vorschau](../iot-edge/how-to-install-iot-core.md)

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihr IoT Edge-Gerät vorbereitet. Im nächsten Schritt stellen Sie Module dafür bereit. Informationen hierzu finden Sie unter [Importieren eines IoT Edge-Pakets in den Solution Accelerator für die Remoteüberwachung](iot-accelerators-remote-monitoring-import-edge-package.md).

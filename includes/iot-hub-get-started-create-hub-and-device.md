---
title: Includedatei
description: Includedatei
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: f8cd78e63099f864c5fc54b6268f6e558d738626
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724927"
---
## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nachdem Sie nun einen IoT Hub erstellt haben, können Sie nach den wichtigen Informationen suchen, die Sie zum Herstellen einer Verbindung für Geräte und Anwendungen mit Ihrem IoT Hub nutzen. 

Öffnen Sie in Ihrem IoT Hub-Navigationsmenü die Option **Freigegebene Zugriffsrichtlinien**.
Wählen Sie die Richtlinie **iothubowner** aus, und kopieren Sie die **Verbindungszeichenfolge – Primärschlüssel** Ihres IoT Hub. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > Die Verbindungszeichenfolge „iothubowner“ wird für dieses Tutorial nicht benötigt. Sie benötigen sie allerdings unter Umständen für einige Tutorials oder andere IoT-Szenarien, nachdem Sie diese Einrichtung abgeschlossen haben.

   ![Abrufen der IoT Hub-Verbindungszeichenfolge](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Registrieren eines Geräts beim IoT Hub für Ihr Gerät

1. Öffnen Sie in Ihrem IoT Hub-Navigationsmenü die Option **IoT-Geräte**, und klicken Sie auf **Hinzufügen**, um ein Gerät in Ihrem IoT Hub zu registrieren.

   ![Hinzufügen eines Geräts im Bereich „IoT-Geräte“ Ihres IoT Hubs](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Geben Sie eine **Geräte-ID** für das neue Gerät ein. Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klicken Sie auf **Speichern**.
5. Öffnen Sie das Gerät nach der Erstellung in der Liste im Bereich **IoT-Geräte**.
6. Kopieren Sie **Verbindungszeichenfolge – Primärschlüssel** zur späteren Verwendung.

   ![Abrufen der Geräte-Verbindungszeichenfolge](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)

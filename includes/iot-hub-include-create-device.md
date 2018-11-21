---
title: Includedatei
description: Includedatei
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 4a4ec51430242cc3e3d6a0b801f2b4be7858ab50
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516090"
---
<!-- put the ## header in the file that includes this file -->

In diesem Abschnitt erstellen Sie eine Geräteidentität in der Identitätsregistrierung im IoT Hub. Ein Gerät kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch](../articles/iot-hub/iot-hub-devguide-identity-registry.md) im Abschnitt zur Identitätsregistrierung. 

1. Öffnen Sie in Ihrem IoT Hub-Navigationsmenü die Option **IoT-Geräte**, und klicken Sie auf **Hinzufügen**, um ein neues Gerät in Ihrem IoT Hub zu registrieren.

    ![Erstellen der Geräteidentität im Portal](./media/iot-hub-include-create-device/create-identity-portal.png)

1. Geben Sie einen Namen für Ihr neues Gerät an, z.B. **myDeviceId**, und klicken Sie auf **Speichern**. Mit dieser Aktion wird eine neue Geräteidentität für Ihren IoT Hub erstellt.

   ![Hinzufügen eines neuen Geräts](./media/iot-hub-include-create-device/create-a-device.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]


1. Öffnen Sie das Gerät nach der Erstellung in der Liste im Bereich **IoT-Geräte**. Kopieren Sie **Verbindungszeichenfolge – Primärschlüssel** zur späteren Verwendung.

    ![Geräte-Verbindungszeichenfolge](./media/iot-hub-include-create-device/device-details.png)

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT-Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

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
ms.openlocfilehash: ffd5da239f8e271a8c9b2aaf3f6d5fd9f885c79c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400270"
---
## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität

In diesem Abschnitt verwenden Sie das [Azure-Portal](https://portal.azure.com), um in der Identitätsregistrierung im IoT Hub eine Geräteidentität zu erstellen. Ein Gerät kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch](../articles/iot-hub/iot-hub-devguide-identity-registry.md) im Abschnitt zur Identitätsregistrierung. Verwenden Sie das Panel **IoT-Geräte** im Portal, um eine eindeutige Geräte-ID und einen Schlüssel für Ihr Gerät zu generieren, damit sich dieses damit beim IoT Hub identifizieren kann. Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

2. Wählen Sie **Alle Ressourcen** aus, und suchen Sie nach Ihrer IoT Hub-Ressource.

3. Klicken Sie bei geöffneter IoT Hub-Ressource auf das Tool **IoT-Geräte** und dann oben auf **Hinzufügen**. 

    ![Erstellen der Geräteidentität im Portal](./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png)

4. Geben Sie einen Namen für Ihr neues Gerät an, z.B. **myDeviceId**, und klicken Sie auf **Speichern**. Mit dieser Aktion wird eine neue Geräteidentität für Ihren IoT Hub erstellt.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Hinzufügen eines neuen Geräts](./media/iot-hub-get-started-create-device-identity-portal/create-a-device.png)

5. Klicken Sie in der Geräteliste auf das neu erstellte Gerät, und kopieren Sie **Verbindungszeichenfolge – Primärschlüssel** zur späteren Verwendung.

    ![Geräte-Verbindungszeichenfolge](./media/iot-hub-get-started-create-device-identity-portal/device-details.png)

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT-Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

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
ms.openlocfilehash: 1df3e188b71b8fa2d5223bad8bc5914513e26286
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371200"
---
## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität

In diesem Abschnitt verwenden Sie das [Azure-Portal][lnk-azure-portal], um in der Identitätsregistrierung im IoT Hub eine Geräteidentität zu erstellen. Ein Gerät kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity] im Abschnitt zur Identitätsregistrierung. Verwenden Sie das Panel **IoT-Geräte** im Portal, um eine eindeutige Geräte-ID und einen Schlüssel für Ihr Gerät zu generieren, damit sich dieses damit beim IoT Hub identifizieren kann. Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

1. Melden Sie sich beim [Azure-Portal][lnk-azure-portal] an.

1. Wählen Sie **Alle Ressourcen** aus, und suchen Sie nach Ihrer IoT Hub-Ressource.

1. Klicken Sie bei geöffneter IoT Hub-Ressource auf das Tool **IoT-Geräte** und dann oben auf **Hinzufügen**. 

    ![Erstellen der Geräteidentität im Portal][img-add-device]

1. Geben Sie einen Namen für Ihr neues Gerät an, z.B. **myDeviceId**, und klicken Sie auf **Speichern**. Mit dieser Aktion wird eine neue Geräteidentität für Ihren IoT Hub erstellt.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Hinzufügen eines neuen Geräts][img-create-device]

1. Klicken Sie in der Geräteliste auf das neu erstellte Gerät, und kopieren Sie **Verbindungszeichenfolge – Primärschlüssel** zur späteren Verwendung.

    ![Geräte-Verbindungszeichenfolge][img-connection-string]

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT-Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-add-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png
[img-create-device]:./media/iot-hub-get-started-create-device-identity-portal/add-device.png

<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md


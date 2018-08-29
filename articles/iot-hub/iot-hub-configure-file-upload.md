---
title: Konfigurieren des Dateiuploads im Azure-Portal | Microsoft Docs
description: Konfigurieren des IoT Hubs im Azure-Portal zum Aktivieren von Dateiuploads von verbundenen Geräten. Enthält Informationen zum Konfigurieren des Azure-Zielspeicherkontos.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: a9f9eeaed2716c5d492099568fd6f90080471af2
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "42142403"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Konfigurieren des Dateiuploads in IoT Hub im Azure-Portal

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Dateiupload

Damit Sie die [Dateiuploadfunktion in IoT Hub](iot-hub-devguide-file-upload.md) verwenden können, müssen Sie Ihrem Hub zunächst ein Azure Storage-Konto zuordnen. Wählen Sie **Dateiupload** zum Anzeigen einer Liste der Dateiuploadeigenschaften für den IoT Hub, der geändert wird.

![Anzeigen der Dateiuploadeinstellungen für den IoT Hub im Portal](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Speichercontainer**: Verwenden Sie das Azure-Portal, um einen Blobcontainer in einem Azure Storage-Konto Ihres aktuellen Azure-Abonnements auszuwählen, der Ihrer IoT Hub-Instanz zugeordnet werden soll. Erstellen Sie bei Bedarf auf dem Blatt **Speicherkonten** ein Azure Storage-Konto und auf dem Blatt **Container** einen Blobcontainer. IoT Hub generiert automatisch SAS-URIs mit Schreibberechtigungen für diesen Blobcontainer, die Geräte beim Hochladen von Dateien verwenden können.

   ![Anzeigen von Speichercontainern für den Dateiupload im Portal](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Benachrichtigungen für hochgeladene Dateien empfangen**: Aktivieren oder deaktivieren Sie Dateiuploadbenachrichtigungen über den Umschalter.

* **SAS-TTL**: Diese Einstellung dient zum Festlegen der Gültigkeitsdauer der SAS-URIs, die von IoT Hub an das Gerät zurückgegeben werden. Standardmäßig auf 1 Stunde festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

* **Standard-TTL für Dateibenachrichtigungseinstellungen**: Die Gültigkeitsdauer einer Dateiuploadbenachrichtigung (Zeit bis zum Ablauf). Standardmäßig auf 1 Tag festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

* **Anzahl maximaler Zustellungen für Dateibenachrichtigungen**: Gibt an, wie oft IoT Hub versucht, eine Dateiuploadbenachrichtigung zu senden. Standardmäßig auf 10 festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

   ![Konfigurieren des Dateiuploads für IoT Hub im Portal](./media/iot-hub-configure-file-upload/file-upload-selected-container.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Dateiuploadfunktionen von IoT Hub finden Sie unter [Hochladen von Dateien von einem Gerät](iot-hub-devguide-file-upload.md) im IoT Hub-Entwicklerhandbuch.

Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [Massenverwaltung von IoT-Geräten](iot-hub-bulk-identity-mgmt.md)
* [IoT Hub-Metriken](iot-hub-metrics.md)
* [Vorgangsüberwachung](iot-hub-operations-monitoring.md)

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklungsleitfaden für IoT Hub](iot-hub-devguide.md)
* [Bereitstellen von KI auf Edge-Geräten mit Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
* [Schützen Ihrer IoT-Lösung von Grund auf](../iot-fundamentals/iot-security-ground-up.md)

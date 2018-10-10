---
title: Registrieren eines neuen Azure IoT Edge-Geräts (Befehlszeilenschnittstelle) | Microsoft-Dokumentation
description: Verwenden der IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle zum Registrieren eines neuen IoT Edge-Geräts
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ee5e68d45c7d966619238312dabedc1628a4bf61
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998031"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli"></a>Registrieren eines neuen Azure IoT Edge-Geräts mithilfe der Azure-Befehlszeilenschnittstelle

Bevor Sie Ihre IoT-Geräte mit Azure IoT Edge verwenden können, müssen Sie sie bei Ihrem IoT Hub registrieren. Nachdem Sie ein Gerät registriert haben, erhalten Sie eine Verbindungszeichenfolge, die zum Einrichten Ihres Geräts für Edge-Workloads verwendet werden kann. 

Die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen wie IoT Edge. Mit ihm können Sie Azure IoT Hub-Ressourcen, Instanzen des Device Provisioning Service und verknüpfte Hubs direkt ohne weitere Tools verwalten. Mit der neuen IoT-Erweiterung wird die Azure-Befehlszeilenschnittstelle um Features wie die Geräteverwaltung und um umfassende IoT Edge-Funktionen erweitert.

Dieser Artikel zeigt die Registrierung eines neuen IoT Edge-Geräts mithilfe der Azure-Befehlszeilenschnittstelle.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) in Ihrem Azure-Abonnement. 
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli) in Ihrer Umgebung. Ihre Azure CLI-Version muss mindestens 2.0.24 lauten. Verwenden Sie `az –-version`, um dies zu überprüfen. Diese Version unterstützt az-Erweiterungsbefehle, und das Framework für Knack-Befehle wird eingeführt. 
* Die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Erstellen eines Geräts

Verwenden Sie den folgenden Befehl, um eine neue Geräteidentität in Ihrem IoT Hub zu erstellen: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Dieser Befehl weist drei Parameter auf:
* **device-id:** Geben Sie einen beschreibenden Namen an, der für Ihren IoT Hub eindeutig ist.
* **hub-name:** Geben Sie den Namen Ihres IoT Hub an.
* **edge-enabled:** Dieser Parameter deklariert, dass das Gerät für die Verwendung mit IoT Edge ausgelegt ist.

   ![IoT Edge-Gerät erstellen](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Anzeigen aller Geräte

Verwenden Sie den folgenden Befehl, um alle Geräte in Ihrem IoT Hub anzuzeigen:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Alle als IoT Edge-Gerät registrierten Geräte weisen die **capabilities.iotEdge**-Eigenschaft mit dem Wert **true** auf. 

## <a name="retrieve-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft. Verwenden Sie den folgenden Befehl, um die Verbindungszeichenfolge für ein einzelnes Gerät zurückzugeben:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

Beim Parameter für die Geräte-ID muss ist Groß-/Kleinschreibung beachtet werden. Kopieren Sie nicht die Anführungszeichen, in die die Verbindungszeichenfolge eingeschlossen ist. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [mithilfe der Azure-Befehlszeilenschnittstelle Module auf einem Gerät bereitstellen](how-to-deploy-modules-cli.md).

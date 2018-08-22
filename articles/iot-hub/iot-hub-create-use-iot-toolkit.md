---
title: Erstellen einer Azure IoT Hub-Instanz mit dem Azure IoT-Toolkit für VS Code | Microsoft-Dokumentation
description: Hier wird erläutert, wie Sie mithilfe des Azure IoT-Toolkits für VS Code eine IoT Hub-Instanz erstellen.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: af31f9375d6a41e13a9122e9730ba9532d3d52c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003068"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Erstellen einer IoT Hub-Instanz mithilfe des Azure IoT-Toolkits für Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Sie können mithilfe des [Azure IoT-Toolkits für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) Azure IoT Hub-Instanzen erstellen. In diesem Artikel wird beschrieben, wie Sie mit dem Azure IoT-Toolkit eine IoT Hub-Instanz erstellen.

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein aktives Azure-Konto.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT-Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Öffnen Sie in Visual Studio Code die **Explorer**-Ansicht.

2. Erweitern Sie im unteren Bereich des Explorers den Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). 

   ![Erweitern von „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte)](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Klicken Sie auf **...** in der Überschrift des Abschnitts **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header. 

4. Klicken Sie auf **IoT Hub erstellen**.

5. In der unteren rechten Ecke wird ein Popupfenster angezeigt, über das Sie sich zum ersten Mal bei Azure anmelden können.

6. Wählen Sie ein Azure-Abonnement aus. 

7. Wählen Sie eine Ressourcengruppe aus.

8. Wählen Sie einen Standort aus.

9. Wählen Sie einen Tarif aus.

10. Geben Sie einen global eindeutigen Namen für Ihre IoT Hub-Instanz ein.

11. Warten Sie einige Minuten, bis die IoT Hub-Instanz erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine IoT Hub-Instanz mit dem Azure IoT-Toolkit für Visual Studio Code bereitgestellt haben, möchten Sie vielleicht mehr wissen:

* [Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub mithilfe der Azure IoT Toolkiterweiterung für Visual Studio Code](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)
* [Verwenden der Azure IoT-Toolkiterweiterung für Visual Studio Code für die Geräteverwaltung mit Azure IoT Hub](iot-hub-device-management-iot-toolkit.md)
* [Wiki-Seite](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki) für das Azure IoT-Toolkit

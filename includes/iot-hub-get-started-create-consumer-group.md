---
author: robinsh
manager: philmea
ms.author: robin.shahan
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 242f601ced4838a1b4e559774c25d05de04ddb77
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2019
ms.locfileid: "57016343"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Hinzufügen einer Consumergruppe zu Ihrem IoT Hub

Consumergruppen werden von Anwendungen verwendet, um Daten aus Azure IoT Hub abzurufen. In diesem Tutorial erstellen Sie eine Consumergruppe, die von einem zukünftigen Azure-Dienst zum Lesen von Daten in Ihrem IoT Hub verwendet wird.

Gehen Sie folgendermaßen vor, um Ihrem IoT Hub eine Consumergruppe hinzuzufügen:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) Ihren IoT-Hub.

2. Klicken Sie im linken Bereich auf **Integrierte Endpunkte**. Wählen Sie anschließend im oberen Bereich **Ereignisse** aus, und geben Sie im rechten Bereich unter **Consumergruppen** einen Namen ein. Klicken Sie nach dem Ändern des Werts von **Standard-TTL** auf **Speichern**, und setzen Sie „Standard-TTL“ auf den ursprünglichen Wert zurück.

   ![Erstellen einer Consumergruppe in Ihrem IoT Hub](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)

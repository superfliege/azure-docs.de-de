---
title: Includedatei
description: Includedatei
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/7/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 1a750a97cdc940c0f0a3d7e33d6be0d33f811425
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53108021"
---
Eine der wichtigen Funktionen von Azure IoT Edge ist die Möglichkeit, Module aus der Cloud auf IoT Edge-Geräten bereitzustellen. Ein IoT-Edge-Modul ist ein ausführbares Paket, das als Container implementiert wird. In diesem Abschnitt wird ein vorgefertigtes Modul aus dem [Abschnitt „IoT Edge-Module“ des Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) bereitgestellt. Dieses Modul generiert Telemetriedaten für Ihr simuliertes Gerät.

1. Geben Sie im Azure-Portal `Simulated Temperature Sensor` in das Suchfeld ein, und öffnen Sie das Marketplace-Ergebnis.

   ![Simulierter Temperatursensor in der Suche im Azure-Portal](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. Wählen Sie im Feld **Abonnement** das Abonnement mit der von Ihnen verwendeten IoT Hub-Instanz aus, sofern es nicht bereits ausgewählt ist.

3. Wählen Sie im Feld **IoT Hub** den Namen der von Ihnen verwendeten IoT Hub-Instanz aus, sofern er nicht bereits ausgewählt ist.

4. Klicken Sie auf **Gerät suchen**, wählen Sie Ihr IoT Edge-Gerät (`myEdgeDevice`) aus, und wählen Sie anschließend **Erstellen** aus.

5. Klicken Sie im Schritt **Module hinzufügen** des Assistenten auf das Modul **SimulatedTemperatureSensor**, um die Konfigurationseinstellungen zu überprüfen. Klicken Sie dann auf **Speichern**, und wählen Sie **Weiter** aus.

6. Vergewissern Sie sich im Schritt **Routen angeben** des Assistenten, dass die Routen ordnungsgemäß mit der Standardroute eingerichtet sind, über die alle Nachrichten von allen Modulen an IoT Hub (`$upstream`) gesendet werden. Ist das nicht der Fall, können Sie den folgenden Code eingeben und dann **Weiter** auswählen.

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

7. Klicken Sie im Schritt **Bereitstellung überprüfen** des Assistenten auf **Senden**.

8. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie **Aktualisieren** aus. Zusätzlich zum edgeAgent-Modul, das beim ersten Start des Diensts erstellt wurde, sollte ein weiteres Runtimemodul mit dem Namen **edgeHub** angezeigt werden, und das Modul **SimulatedTemperatureSensor** sollte aufgeführt sein.

   Es kann einige Minuten dauern, bis die neuen Module angezeigt werden. Das IoT Edge-Gerät muss die neuen Bereitstellungsinformationen aus der Cloud abrufen, die Container starten und dann seinen neuen Status an IoT Hub zurückmelden. 

   ![Anzeigen von „SimulatedTemperatureSensor“ in der Liste mit den bereitgestellten Modulen](./media/iot-edge-deploy-module/deployed-modules-marketplace-temp.png)

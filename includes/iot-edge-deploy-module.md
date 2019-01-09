---
title: Includedatei
description: Includedatei
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/31/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: dd4873017105db190f9a468ec1f1f77f4e8c9c0e
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977126"
---
Eine der wichtigen Funktionen von Azure IoT Edge ist die Möglichkeit, Module aus der Cloud auf IoT Edge-Geräten bereitzustellen. Ein IoT-Edge-Modul ist ein ausführbares Paket, das als Container implementiert wird. In diesem Abschnitt stellen wir ein fertig erstelltes Modul aus dem [IoT Edge-Module-Bereich von Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) bereit. Dieses Modul generiert simulierte Telemetriedaten für Ihr IoT Edge-Gerät.

1. Geben Sie im [Azure-Portal](https://portal.azure.com) **Simulated Temperature Sensor** (Simulierter Temperatursensor) in das Suchfeld ein, und öffnen Sie das Marketplace-Ergebnis.

   ![Simulierter Temperatursensor in der Suche im Azure-Portal](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. Wählen Sie ein IoT Edge-Gerät aus, das dieses Modul empfangen soll. Geben Sie unter **Zielgeräte für IoT Edge-Modul** die folgenden Informationen ein:

   1. **Abonnement**: Wählen Sie das Abonnement aus, das den von Ihnen verwendeten IoT-Hub enthält.

   2. **IoT Hub**: Wählen Sie den Namen des verwendeten IoT-Hubs aus.

   3. **Name des IoT Edge-Geräts**: Wenn Sie den Namen des vorgeschlagenen Geräts bereits zuvor in diesem Schnellstart verwendet haben, geben Sie **meinEdgeGerät** ein. Wählen Sie andernfalls **Gerät suchen** aus, um aus einer Liste der Geräte auf Ihrem IoT-Hub auszuwählen. 
   
   4. Klicken Sie auf **Erstellen**.

3. Jetzt, da Sie ein IoT Edge-Modul im Azure Marketplace und ein das Modul empfangendes IoT Edge-Gerät ausgewählt haben, gelangen Sie zu einem aus drei Schritten bestehenden Assistenten, der Sie bei der genauen Definition der Bereitstellung des Moduls unterstützt. Beachten Sie im Schritt **Module hinzufügen** des Assistenten, dass das Modul **SimulatedTemperatureSensor** (Simulierter Temperatursensor) automatisch aufgefüllt wird. In den Tutorials verwenden Sie diese Seite, um Ihrer Bereitstellung weitere Module hinzuzufügen. Im Rahmen dieses Schnellstarts stellen Sie einfach dieses eine Modul bereit. Wählen Sie **Weiter** aus, um mit dem nächsten Schritt des Assistenten fortzufahren.

4. Im Schritt **Specify Routes** (Routen festlegen) des Assistenten definierten Sie, wie Nachrichten zwischen Modulen und dem IoT Hub übergeben werden. Im Rahmen des Schnellstarts sollen alle Nachrichten von allen Modulen an den IoT Hub (`$upstream`) gelangen. Wenn er nicht automatisch aufgefüllt wurde, fügen Sie in diesem Schritt den folgenden Code hinzu, und wählen Sie dann **Weiter** aus:

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

5. Im Schritt **Bereitstellung überprüfen** des Assistenten können Sie eine Vorschau der JSON-Datei anzeigen, in der alle Module definiert sind, die auf Ihrem IoT Edge-Gerät bereitgestellt werden. Beachten Sie, dass hier das Modul **SimulatedTemperatureSensor** (Simulierter Temperatursensor) ebenso enthalten ist wie zwei zusätzliche Systemmodule mit den Namen **edgeAgent** und **edgeHub**. Wählen Sie **Übermitteln** aus, wenn Sie die Überprüfung abgeschlossen haben.

   Wenn Sie eine neue Bereitstellung an ein IoT Edge-Gerät übermitteln, wird nichts per Push auf Ihr Gerät übertragen. Stattdessen fragt das Gerät den IoT Hub regelmäßig nach neuen Anweisungen ab. Wenn es die neuen Bereitstellungsinformationen findet, verwendet das Gerät diese Informationen, um die Modulimages aus der Cloud herunterzuladen, und beginnt dann mit der lokalen Ausführung der Module. Dieser Vorgang kann einige Minuten in Anspruch nehmen. 

6. Nachdem Sie die Bereitstellungsdetails des Moduls übermittelt haben, werden Sie vom Assistenten zur Seite **IoT Edge** Ihres IoT Hubs zurück geführt. Wählen Sie Ihr Gerät in der Liste der IoT Edge-Geräte aus, um seine Details anzuzeigen. 

7. Scrollen Sie auf der Detailseite des Geräts nach unten zum Abschnitt **Module**. Dort sollten drei Module aufgelistet sein: $edgeAgent, $edgeHub und SimulatedTemperatureSensor. Wenn eins oder mehrere dieser Moduls in der Bereitstellung als angegeben aufgeführt, vom Gerät aber nicht gemeldet werden, bedeutet dies, dass Ihr IoT Edge-Gerät noch mit ihrem Start beschäftigt ist. Warten Sie einige Minuten, und wählen Sie oben auf der Seite **Aktualisieren** aus. 

   ![Anzeigen von „SimulatedTemperatureSensor“ in der Liste mit den bereitgestellten Modulen](./media/iot-edge-deploy-module/deployed-modules-marketplace.png)

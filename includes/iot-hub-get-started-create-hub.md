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
ms.openlocfilehash: 08afdcf91499fdb6f2da6e9ccc82313286f5c964
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43111956"
---
## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs
Erstellen Sie einen IoT-Hub, mit dem Ihre simulierte Geräte-App verbunden werden kann. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Ressource erstellen** > **Internet der Dinge (IoT)** > **IoT Hub** aus.
   
    ![Navigationsleiste im Azure-Portal](./media/iot-hub-get-started-create-hub/create-iot-hub1.png)

3. Geben Sie im Bereich **IoT Hub** die folgenden Informationen für Ihren IoT Hub ein:

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie zum Erstellen dieses IoT-Hubs verwenden möchten.

   * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe zum Hosten des IoT Hubs, oder verwenden Sie eine vorhandene. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../articles/azure-resource-manager/resource-group-portal.md).

   * **Region**: Wählen Sie den nächstgelegenen Standort aus.

   * **Name**: Erstellen Sie einen Namen für Ihren IoT-Hub. Wenn der eingegebene Name verfügbar ist, wird ein grünes Häkchen angezeigt.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Fenster mit IoT Hub-Grundeinstellungen](./media/iot-hub-get-started-create-hub/create-iot-hub2.png)

4. Klicken Sie auf **Next: Size and scale** (Nächster Schritt: Größe festlegen und skalieren), um die Erstellung Ihres IoT-Hubs fortzusetzen. 

5. Wählen Sie eine Option für **Tarif und Skalierung** aus. Legen Sie für diesen Artikel den Tarif **F1 – Free** fest, wenn er für Ihr Abonnement noch verfügbar ist. Weitere Informationen hierzu finden Sie unter [Tarif und Skalierung](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Fenster für IoT Hub-Größe und -Skalierung](./media/iot-hub-get-started-create-hub/create-iot-hub3.png)

6. Klicken Sie auf **Überprüfen + erstellen**.

7. Überprüfen Sie die Informationen zum IoT-Hub, und klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann mehrere Minuten dauern. Sie können den Fortschritt im Bereich **Benachrichtigungen** überwachen.

8. Wenn Ihre neue IoT Hub-Instanz bereit ist, klicken Sie im Azure-Portal auf die entsprechende Kachel, um das Eigenschaftenfenster zu öffnen. Nachdem Sie nun einen IoT Hub erstellt haben, können Sie nach den wichtigen Informationen suchen, die Sie zum Herstellen einer Verbindung für Geräte und Anwendungen mit Ihrem IoT Hub nutzen. Klicken Sie auf **SAS-Richtlinien**.
   
9. Wählen Sie unter **Freigegebene Zugriffsrichtlinien** die Richtlinie **iothubowner** aus. Kopieren Sie unter **Verbindungszeichenfolge – Primärschlüssel** den IoT Hub-Primärschlüssel zur späteren Verwendung. Weitere Informationen finden Sie unter [Zugriffssteuerung](../articles/iot-hub/iot-hub-devguide-security.md) im „Entwicklerhandbuch für Azure IoT Hub“.
   
    ![Freigegebene Zugriffsrichtlinien](./media/iot-hub-get-started-create-hub/create-iot-hub5.png)
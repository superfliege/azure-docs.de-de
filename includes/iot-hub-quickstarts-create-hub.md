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
ms.openlocfilehash: 63acf0297a694ff442d56e67d52fd9b4e49f812d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008585"
---
Im ersten Schritt wird über das Azure-Portal eine IoT Hub-Instanz in Ihrem Abonnement erstellt. Mit der IoT Hub-Instanz können umfangreiche Telemetriedaten von zahlreichen Geräten in der Cloud erfasst werden. Diese Telemetriedaten können dann von in der Cloud ausgeführten Back-End-Diensten gelesen und verarbeitet werden.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

1. Wählen Sie **Ressource erstellen** > **Internet der Dinge (IoT)** > **IoT Hub** aus.

    ![Auswahl für die IoT Hub-Installation][1]

1. Geben Sie im Bereich **IoT Hub** die folgenden Informationen für Ihren IoT Hub ein:

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie zum Erstellen dieses IoT-Hubs verwenden möchten.
   * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, die den IoT Hub enthält, oder verwenden Sie eine vorhandene. Wenn Sie alle verwandten Ressourcen in einer Gruppe (z.B **TestResources**) zusammenfassen, können Sie sie alle zusammen verwalten. Wenn Sie beispielsweise die Ressourcengruppe löschen, werden alle Ressourcen in dieser Gruppe gelöscht. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen][lnk-resource-groups].
   * **Region:** Wählen Sie den Standort aus, der Ihren Geräten am nächsten ist.
   * **Name**: Erstellen Sie einen eindeutigen Namen für Ihren IoT Hub. Wenn der eingegebene Name verfügbar ist, wird ein grünes Häkchen angezeigt.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Fenster mit IoT Hub-Grundeinstellungen][2]

2. Klicken Sie auf **Next: Size and scale** (Nächster Schritt: Größe festlegen und skalieren), um die Erstellung Ihres IoT-Hubs fortzusetzen. 

3. Wählen Sie eine Option für **Tarif und Skalierung** aus. Legen Sie für diesen Artikel den Tarif **F1 – Free** fest, wenn er für Ihr Abonnement noch verfügbar ist. Weitere Informationen hierzu finden Sie unter [Tarif und Skalierung][lnk-pricing].

   ![Fenster für IoT Hub-Größe und -Skalierung][3]

4. Klicken Sie auf **Überprüfen + erstellen**.

1. Überprüfen Sie die Informationen zum IoT-Hub, und klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann mehrere Minuten dauern. Sie können den Fortschritt im Bereich **Benachrichtigungen** überwachen.


<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 13eddced155eab6dedfbce77330e7a178ecfb3cb
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165007"
---
> [!div class="op_single_selector"]
> * [Device: Node.js Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Gerät: C#-Dienst: C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Gerät: Java Dienst: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Gerät: Python-Dienst: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Back-End-Apps können Azure IoT Hub-Grundtypen (etwa [Gerätezwillingsmethode][lnk-devtwin] und [direkte Methode][lnk-c2dmethod]) verwenden, um Geräteverwaltungsaktionen auf Geräten remote zu starten und zu überwachen. In diesem Tutorial wird veranschaulicht, wie eine Back-End-App und eine Gerät-App zusammen verwendet werden können, um einen Remoteneustart des Geräts mit IoT Hub zu initiieren und zu überwachen.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Verwenden Sie eine direkte Methode, um Geräteverwaltungsaktionen (wie Neustarts, Zurücksetzen auf Werkseinstellungen und Firmwareaktualisierung) von einer Back-End-App aus in der Cloud zu initiieren. Das Gerät ist für Folgendes verantwortlich:

* Verarbeiten der von IoT Hub gesendeten Methodenanforderung
* Initiieren der entsprechenden gerätespezifischen Aktion auf dem Gerät.
* Senden von Statusupdates über die *gemeldeten Eigenschaften* an IoT Hub

Sie können eine Back-End-App in der Cloud verwenden, um Gerätezwillingsabfragen auszuführen und Berichte zum Status der Geräteverwaltungsaktionen zu erstellen.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md

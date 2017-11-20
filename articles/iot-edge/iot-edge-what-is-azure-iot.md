---
title: "Azure-Lösungen für das Internet der Dinge (IoT Edge) | Microsoft-Dokumentation"
description: "Übersicht über eine Beispielarchitektur einer IoT-Lösung sowie über die Beziehung zum Azure IoT Hub-Dienst, zu Azure IoT-Geräte-SDKs, Azure IoT-Dienst-SDKs und anderen Azure-Diensten"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: 587b733106d511ec63d71f67a06e520324a3e594
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Nächste Schritte

Azure IoT Edge ist ein Azure-Dienst, mit dem Analysen und die Datenverarbeitung im Edge-Bereich aktiviert werden. Mit IoT Edge können Sie Ihre Geräte mit containerbasiertem Code optimieren, bei dem es sich entweder um Logik handelt, die direkt von den bereits genutzten Azure-Diensten stammt, oder um Ihren eigenen lösungsspezifischen Code. Für Ihre Geräte wird so Folgendes ermöglicht:

* Einsatz als Gatewaygeräte, Aggregation und Verarbeitung von Daten von mehreren Blattknotengeräten
* Durchführung der Anomalieerkennung und Reaktion auf Änderungen in der Umgebung, ohne auf Anweisungen aus der Cloud warten zu müssen
* Reduzierung der Bandbreiten- und Speicherkosten durch Vorverarbeitung der Daten und Senden der Ergebnisse 

IoT Edge enthält auch eine Cloudschnittstelle, die die Remoteverwaltung von Geräten ermöglicht. Ohne dass Sie physisch auf Ihre Geräte zugreifen müssen, können Sie Code bereitstellen, den Status überwachen und Aktualisierungen durchführen. Sie können einzelne Geräte per Remotezugriff verwalten oder Bereitstellungen erstellen, die sich auf umfangreiche Sätze mit von Ihnen definierten Geräten auswirken. Weitere Informationen finden Sie unter [Understand IoT Edge deployments for single devices or at scale][lnk-deployment] (Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig).

Informationen zu den Komponenten, die IoT Edge ermöglichen, finden Sie unter [How Azure IoT Edge works][lnk-overview] (Funktionsweise von Azure IoT Edge).

Wenn Sie Azure IoT Hub verwendet haben, kann es hilfreich sein, mit [Übersicht über den Azure IoT Hub-Dienst][lnk-iot-hub] zu beginnen.

[lnk-deployment]: module-deployment-monitoring.md
[lnk-overview]: how-iot-edge-works.md
[lnk-iot-hub]: ../iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md

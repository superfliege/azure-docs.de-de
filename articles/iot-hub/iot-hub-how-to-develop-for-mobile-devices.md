---
title: Entwicklung für mobile Geräte mithilfe von Azure IoT Hub SDKs | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Erfahren Sie mehr über die Entwicklung für mobile Geräte mithilfe von Azure IoT Hub SDKs.'
author: yzhong94
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 9c4b4a61698556f14a6362984b04e5cc409b5763
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634946"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Entwicklung für mobile Geräte mithilfe von Azure IoT Hub SDKs | Microsoft-Dokumentation

„Dinge“ im Internet der Dinge (Internet of Things, IoT) können sich auf eine breite Palette von Geräten mit unterschiedlichen Fähigkeiten beziehen: Sensoren, Mikrocontroller, intelligente Geräte, industrielle Gateways und sogar mobile Geräte.  Ein mobiles Gerät kann ein IoT-Gerät sein, das Gerät-zu-Cloud-Telemetriedaten sendet und von der Cloud verwaltet wird.  Es kann auch das Gerät sein, auf dem eine Back-End-Dienstanwendung ausgeführt wird, die andere IoT-Geräte verwaltet.  In beiden Fällen können [Azure IoT Hub SDKs][lnk-sdk-overview] verwendet werden, um Anwendungen zu entwickeln, die auf mobilen Geräten funktionieren.  

## <a name="develop-for-native-ios-platform"></a>Entwicklung für die native iOS-Plattform

Azure IoT Hub SDKs unterstützen über das IoT Azure Hub C SDK die native iOS-Plattform.  Sie können es sich als ein iOS SDK vorstellen, das Sie in Ihr XCode-Projekt des Typs Swift oder Objective C einbinden können.  Es gibt zwei Möglichkeiten, das C SDK unter iOS zu verwenden:
- Die CocoaPod-Bibliotheken direkt im XCode-Projekt verwenden.  
- Den Quellcode für das C SDK herunterladen und gemäß der [Buildanweisung][lnk-c-devbox] für macOS Builds für die iOS-Plattform erstellen  

Das Azure IoT Hub C SDK ist zur maximalen Portabilität auf verschiedene Plattformen in C99 geschrieben.  Der Portierungsprozess umfasst das Schreiben einer schlanken Adaptionsschicht für die plattformspezifischen Komponenten, die hier für [iOS][lnk-ios-pal] zu finden ist.  Die Funktionen des C SDK können auf der iOS-Plattform genutzt werden, einschließlich der unterstützten Azure IoT Hub-Primitiven und SDK-spezifischen Funktionen, wie z.B. Wiederholungsrichtlinien für die Netzwerkstabilität.  Die Schnittstelle für das iOS SDK ähnelt auch der Schnittstelle für das Azure IoT Hub C SDK.  

In diesen Dokumentationen erfahren Sie, wie Sie eine Geräte- oder Dienstanwendung auf einem iOS-Gerät entwickeln:
- [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz][lnk-device-ios-quickstart]  
- [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub][lnk-service-ios-quickstart]  

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Entwickeln mit CocoaPod-Bibliotheken für Azure IoT Hub

Die Azure IoT Hub SDKs bieten für die iOS-Entwicklung eine Gruppe von Objective-C CocoaPod-Bibliotheken.  Die aktuelle Liste der CocoaPod-Bibliotheken finden Sie unter [CocoaPods für Microsoft Azure IoT][lnk-cocoapod-list].  Sobald die entsprechenden Bibliotheken in Ihr XCode-Projekt eingebunden sind, gibt es zwei Möglichkeiten, IoT Hub-bezogenen Code zu schreiben:
- Objective-C-Funktion: Wenn Ihr Projekt in Objective-C geschrieben ist, können Sie APIs direkt aus dem Azure IoT Hub C SDK aufrufen.  Wenn Ihr Projekt in Swift geschrieben ist, können Sie ``@objc func`` aufrufen, bevor Sie Ihre Funktion erstellen, und mit dem Schreiben sämtlicher Logiken in Azure IoT Hub unter Verwendung von C- oder Objective-C-Code fortfahren.  Eine Reihe von Beispielen, die beide Verfahren veranschaulichen, finden Sie im [Beispielrepository][lnk-ios-samples-repo].  
- Einbinden von C-Beispielen: Wenn Sie eine C-Geräteanwendung geschrieben haben, können Sie in Ihrem XCode-Projekt direkt auf diese verweisen:
    - Fügen Sie in XCode die Datei „sample.c“ Ihrem XCode-Projekt hinzu.  
    - Fügen Sie Ihrer Abhängigkeit die Headerdatei hinzu.  Ein Beispiel einer Headerdatei ist im [Beispielrepository][lnk-ios-samples-repo] enthalten.  Weitere Informationen finden Sie auf der Apple-Dokumentationsseite für [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Nächste Schritte
* [REST-API-Referenz zu IoT Hub][lnk-rest-ref]
* [Azure IoT C SDK-Quellcode][lnk-c-sdk]

[lnk-sdk-overview]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks
[lnk-c-devbox]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-device-ios-quickstart]:https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-ios
[lnk-service-ios-quickstart]: https://docs.microsoft.com/azure/iot-hub/iot-hub-ios-swift-c2d
[lnk-cocoapod-list]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md
[lnk-ios-samples-repo]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-ios-pal]: https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx
[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/

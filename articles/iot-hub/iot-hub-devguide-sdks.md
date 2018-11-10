---
title: Informationen zu Azure IoT SDKs | Microsoft Docs
description: 'Entwicklerhandbuch: Informationen und Links zu verschiedenen Geräte- und Dienst-SDKs für Azure IoT, mit denen Sie Geräte- und Back-End-Apps erstellen können.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dobett
ms.openlocfilehash: 9daca1876a291cb1f726e7c0eb4840c57f5c54c8
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741316"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Verstehen und Verwenden von Azure IoT Hub SDKs

Es gibt drei Kategorien von Software Development Kits (SDKs) für die Arbeit mit IoT Hub:

* **Geräte-SDKs** ermöglichen das Erstellen von Apps, die auf Ihren IoT-Geräten ausgeführt werden, mit dem Geräteclient oder dem Modulclient. Mit diesen Apps werden Telemetriedaten an die IoT Hub-Instanz gesendet und optional Nachrichten oder Updates für Aufträge, Methoden oder Gerätezwillingen von der IoT Hub-Instanz empfangen.  Sie können mit dem Modulclient auch [Module](../iot-edge/iot-edge-modules.md) für [Azure IoT Edge-Runtime](../iot-edge/about-iot-edge.md) erstellen.

* Mit **Dienst-SDKs** können Sie Ihren IoT-Hub verwalten und für Ihre IoT-Geräte oder -Module optional Nachrichten senden, Aufträge planen, direkte Methoden aufrufen oder gewünschte Eigenschaftsupdates senden.

* Mit **Gerätebereitstellungs-SDKs** können Sie Geräte in IoT Hub mit dem [Device Provisioning Service](../iot-dps/about-iot-dps.md) bereitstellen.

Erfahren Sie mehr über die [Vorteile beim Entwickeln mit Azure IoT SDKs](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IoT-Geräte-SDKs

Die Microsoft Azure IoT-Geräte-SDKs enthalten Code, der das Erstellen von Geräten und Anwendungen ermöglicht, die eine Verbindung mit Azure IoT Hub-Diensten herstellen und von ihnen verwaltet werden.

Azure IoT Hub Device SDK für .NET: 

* Installation über [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-csharp)
* [API-Referenz](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Modulreferenz](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

Azure IoT Hub Device SDK für C wurde in ANSI C (C99) geschrieben und ist auf Portabilität und hohe Plattformkompatibilität ausgelegt.

* Installation über [apt-get, MBED, Arduino-IDE oder Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-c)
* [API-Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Modulreferenz](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

Azure IoT Hub Device SDK für Java: 

* Hinzufügen zum [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk)-Projekt
* [Quellcode](https://github.com/Azure/azure-iot-sdk-java)
* [API-Referenz](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Modulreferenz](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

Azure IoT Hub Device SDK für Node.js: 

* Installation über [npm](https://www.npmjs.com/package/azure-iot-device)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-node)
* [API-Referenz](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Modulreferenz](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

Azure IoT Hub Device SDK für Python: 

* Installation über [pip](https://pypi.python.org/pypi/azure-iothub-device-client/)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-python)
* API-Referenz: siehe [C-API-Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

Azure IoT Hub Device SDK für iOS: 

* Installation über [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient)
* [Beispiele](https://github.com/Azure-Samples/azure-iot-samples-ios)
* API-Referenz: siehe [C-API-Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

> [!NOTE]
> In den „Readme“-Dateien in den GitHub-Repositorys finden Sie Informationen zum Verwenden sprach- und plattformspezifischer Paket-Manager zum Installieren von Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Betriebssystemplattformen und Hardwarekompatibilität

Unterstützte Plattformen für die SDKs sind in [Azure IoT-SDKs – Plattformunterstützung](iot-hub-device-sdk-platform-support.md) aufgeführt.

Weitere Informationen zur Kompatibilität von SDKs mit bestimmten Hardwaregeräten finden Sie im [Azure Certified for IoT-Gerätekatalog](https://catalog.azureiotsuite.com/) oder im jeweiligen Repository.

## <a name="azure-iot-service-sdks"></a>Azure IoT-Dienst-SDKs

Die Azure IoT-Dienst-SDKs enthalten Code zum Erstellen von Anwendungen, die direkt mit IoT Hub interagieren, um Geräte und Sicherheit zu verwalten.

Azure IoT Hub Service SDK für .NET:

* Download über [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-csharp)
* [API-Referenz](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Azure IoT Hub Service SDK für Java: 

* Hinzufügen zum [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
)-Projekt
* [Quellcode](https://github.com/Azure/azure-iot-sdk-java)
* [API-Referenz](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

Azure IoT Hub Service SDK für Node.js: 

* Download über [npm](https://www.npmjs.com/package/azure-iothub)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-node)
* [API-Referenz](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Azure IoT Hub Service SDK für Python: 

* Download über [pip](https://pypi.python.org/pypi/azure-iothub-service-client/)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-python)

Azure IoT Hub Service SDK für C: 

* Download über [apt-get, MBED, Arduino-IDE oder Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Quellcode](https://github.com/Azure/azure-iot-sdk-c)

Azure IoT Hub Service SDK für iOS: 

* Installation über [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient)
* [Beispiele](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> In den „Readme“-Dateien in den GitHub-Repositorys finden Sie Informationen zum Verwenden sprach- und plattformspezifischer Paket-Manager zum Installieren von Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer.

## <a name="device-provisioning-sdks"></a>Gerätebereitstellungs-SDKs

Mit **Microsoft Azure-Bereitstellungs-SDKs** können Sie Geräte in IoT Hub mit dem [Device Provisioning Service](../iot-dps/about-iot-dps.md) bereitstellen.

Geräte- und Dienst-SDKs für die Azure Bereitstellung für C#:

* [Geräteclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [Dienstclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

Geräte- und Dienst-SDKs für die Azure Bereitstellung für Java:

* [Geräteclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-device-client)
* [Dienstclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

Geräte- und Dienst-SDKs für die Azure Bereitstellung für Node.js:

* [Geräteclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [Dienstclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

Geräte- und Dienst-SDKs für die Azure Bereitstellung für Python:

* [Geräteclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [Dienstclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

Geräte- und Dienst-SDKs für die Azure Bereitstellung für C:

* [Geräteclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [Dienstclient-SDK für die Bereitstellung](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_service_client)

## <a name="next-steps"></a>Nächste Schritte

Azure IoT SDKs stellen ebenso Tools für die Entwicklung bereit:
* [iothub-diagnostics:](https://github.com/Azure/iothub-diagnostics) ein plattformübergeifendes Befehlszeilentool zum Diagnostizieren von Problemen, die im Zusammenhang mit der Verbindung mit IoT Hub stehen.
* [device-explorer:](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) eine Windows-Desktopanwendung für die Verbindung mit Ihrem IoT Hub.

Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Endpunkte](iot-hub-devguide-endpoints.md)
* [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting](iot-hub-devguide-query-language.md)
* [Kontingente und Drosselung](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub-MQTT-Unterstützung](iot-hub-mqtt-support.md)
* [REST-API-Referenz zu IoT Hub](/rest/api/iothub/)
* [Azure IoT SDK – Plattformunterstützung](iot-hub-device-sdk-platform-support.md)

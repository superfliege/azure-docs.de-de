---
title: Azure IoT-Geräte-SDKs – Plattformunterstützung | Microsoft-Dokumentation
description: 'Konzepte: Liste mit den von den Azure IoT-Geräte-SDKs unterstützten Plattformen'
author: yzhong94
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: yizhon
ms.openlocfilehash: cf3c80424c4626b62317bda537f9491cafc8198c
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043391"
---
# <a name="azure-iot-sdks-platform-support"></a>Azure IoT-SDKs – Plattformunterstützung

Bei den [Azure IoT-SDKs](iot-hub-devguide-sdks.md) handelt es sich um Bibliotheken für die Interaktion mit IoT Hub und dem Device Provisioning Service mit umfassender Unterstützung für verschiedene Sprachen und Plattformen.  Die SDKs können auf den meisten Plattformen ausgeführt werden, und Entwickler können das C-SDK für bestimmte Plattformen portieren, indem sie die Informationen unter [Porting Guidance](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md) (Leitfaden zur Portierung) befolgen. 

Microsoft unterstützt zahlreiche Betriebssysteme, Plattformen und Frameworks, und eine Erweiterung zur Verwendung des Azure IoT-C-SDK ist ebenfalls möglich.  Einige werden offiziell vom Team unterstützt und in Ebenen gruppiert, die für die für Benutzer zu erwartende Supportebene stehen.  „Vollständig unterstützte Plattformen“ bedeutet, dass Microsoft Folgendes durchführt bzw. bereitstellt:
    - Fortlaufende Build- und End-to-End-Tests für den Master und die Versionen mit LTS-Unterstützung
    - Installationshandbuch oder -pakete, falls zutreffend
    - Vollständige Unterstützung auf GitHub

Darüber hinaus haben einige Partner unser C-SDK auf weitere Plattformen portiert und verwalten die Plattformabstraktionsschicht (Platform Abstraction Layer, PAL).  Im [Azure Certified for IoT-Gerätekatalog](https://catalog.azureiotsolutions.com/) finden Sie auch eine Liste mit den Betriebssystemplattformen, für die die verschiedenen SDKs getestet wurden.  Außerdem werden für die SDKs auf diesen Plattformen auch regelmäßig Builds mit eingeschränkten Tests und Supportleistungen durchgeführt:
- MBED2
- Arduino
- Windows CE 2013 (veraltet ab Oktober 2018)
- .NET Standard 1.3 mit .NET Core 2.1 und .NET Framework 4.7
- Xamarin iOS, Android, UWP
- Android mit Java

## <a name="supported-platforms"></a>Unterstützte Plattformen
### <a name="c-sdk"></a>C-SDK
| Betriebssystem                  | Arch | Compiler             | TLS-Bibliothek       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | gcc-5.4.0            | openssl – 1.0.2g |
| Ubuntu 18.04 LTS    | X64  | gcc-7.3              | WolfSSL – 1.13    |
| Ubuntu 18.04 LTS    | X64  | Clang 6.0.X          | Openssl – 1.1.0g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | Natives OSX        |
| Windows Server 2016 | x64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Stretch    | x64  | gcc-7.3              | Openssl – 1.1.0f  |

### <a name="python-sdk"></a>Python SDK
| Betriebssystem                  | Arch | Compiler   | TLS-Bibliothek |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2,7 | openssl     |
| Windows Server 2016 | x64  | Python 2,7 | openssl     |
| Windows Server 2016 | x86  | Python 3.5 | openssl     |
| Windows Server 2016 | x64  | Python 3.5 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 2,7 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 3.4 | openssl     |
| MacOS High Sierra   | x64  | Python 2,7 | openssl     |

### <a name="net-sdk"></a>.NET SDK
| Betriebssystem                  | Arch | Framework            | Standard          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET Core 2.1        | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Core 2.1        | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.7   | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.5.1 | N/V               |

### <a name="nodejs-sdk"></a>Node.js SDK
| Betriebssystem                                           | Arch | Node-Version |
|----------------------------------------------|------|--------------|
| Ubuntu 16.04 LTS (mit Node 6 Docker-Image) | X64  | Node 6       |
| Windows Server 2016                          | X64  | Node 6       |

### <a name="java-sdk"></a>Java-SDK
| Betriebssystem                  | Arch | Java-Version |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |

## <a name="partner-supported-platforms"></a>Unterstützte Partnerplattformen
| Partner             | Geräte                            | Link                     | Support |
|---------------------|------------------------------------|--------------------------|---------|
| Qualcomm            | Qualcomm MDM9206 LTE IoT Modem     | [Qualcomm LTE für IoT-SDK](https://developer.qualcomm.com/software/lte-iot-sdk) | [Forum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4-Serie STM32F4-Serie      | [X-CUBE-CLOUD](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html)             | [Unterstützung](https://www.st.com/content/st_com/en/support/support-home.html) |
|                     | STM32F7-Serie                     | [X-CUBE-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html)             |         |
|                     | STM32L4 Discovery Kit für IoT-Knoten | [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html)          |         |
|                     |                                    | [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            |         |
| Espressif           | ESP32                              | [Esp-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  |

## <a name="next-steps"></a>Nächste Schritte
- [Geräte- und Dienst-SDKs](iot-hub-devguide-sdks.md)
- [Porting Guidance](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md) (Leitfaden für Portierung)
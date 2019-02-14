---
title: Schützen Ihrer Azure IoT-Bereitstellung (Internet der Dinge) | Microsoft-Dokumentation
description: Dieser Artikel enthält ausführliche Informationen zum Schützen Ihrer Azure IoT-Bereitstellung.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: dobett
ms.openlocfilehash: de4896ac1022cbd2bc19af102c96cfbfa89c0041
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56099019"
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-solution-accelerator-cipher-suites"></a>IoT Solution Accelerator-Verschlüsselungssammlungen

Die IoT-Solution Accelerators unterstützen die folgenden Verschlüsselungssammlungen in dieser Reihenfolge.

| Verschlüsselungssammlung | Länge |
| --- | --- |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (entspricht RSA-Verschlüsselung mit 7680 Bit) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (entspricht RSA-Verschlüsselung mit 3072 Bit) FS |128 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (entspricht RSA-Verschlüsselung mit 7680 Bit) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (entspricht RSA-Verschlüsselung mit 3072 Bit) FS |128 |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>Weitere Informationen

Im IoT Hub-Entwicklerhandbuch finden Sie unter [Verwalten des Zugriffs auf IoT Hub](../iot-hub/iot-hub-devguide-security.md) weitere Informationen zur Sicherheit von IoT Hub. 

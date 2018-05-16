---
title: Schützen Ihrer IoT-Bereitstellung | Microsoft Docs
description: Dieser Artikel enthält Informationen zum Schützen Ihrer IoT-Bereitstellung.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 95c23341-16b0-4954-b3f2-d2e82ab7b367
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: c14d2082854fe88df9d1139c619b061699e443aa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
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
Sie können auch einige andere Features und Funktionen der IoT-Solution Accelerators ausprobieren:

* [Übersicht über den Solution Accelerator für Predictive Maintenance][lnk-predictive-overview]
* [Häufig gestellte Fragen zu IoT-Solution Accelerators][lnk-faq]

Sie können unter [Verwalten des Zugriffs auf IoT Hub][lnk-devguide-security] im IoT Hub-Entwicklerhandbuch mehr über die Sicherheit von IoT Hub erfahren.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md

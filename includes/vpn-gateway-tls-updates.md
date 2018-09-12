---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f2d7aba05fc01c5a4dcdb123f25242c4e4a72578
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "43426423"
---
>[!NOTE]
>Ab dem 1. Juli 2018 wird die Unterstützung für TLS 1.0 und 1.1 vom Azure-VPN-Gateway entfernt. Das VPN-Gateway unterstützt dann nur noch TLS 1.2. Informationen zum Beibehalten der Unterstützung finden Sie unter [Updates zum Ermöglichen der Unterstützung von TLS 1.2](#tls1).

Die folgenden älteren Algorithmen werden ebenfalls am 1. Juli 2018 für TLS als veraltet markiert:

* RC4 (Rivest Cipher 4)
* DES (Data Encryption Algorithm)
* 3DES (Triple Data Encryption Algorithm)
* MD5 (Message Digest 5)

### <a name="tls1"></a>Wie aktiviere ich Unterstützung für TLS 1.2 unter Windows 7 und Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]

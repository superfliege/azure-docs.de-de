---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9a17f34333503436d3da340670abdde154e45ef6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336025"
---
Ab dem 1. Juli 2018 wird die Unterstützung für TLS 1.0 und 1.1 vom Azure-VPN-Gateway entfernt. Das VPN-Gateway unterstützt dann nur noch TLS 1.2. Um TLS-Unterstützung und Konnektivität für Windows 7- und Windows 8-Point-to-Site-Clients, die TLS verwenden, aufrechtzuerhalten, wird empfohlen, die folgenden Updates zu installieren:

•   [Update für die Microsoft EAP-Implementierung, das die Verwendung von TLS ermöglicht](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

•   [Update für die Aktivierung von TLS 1.1 und TLS 1.2 als sichere Standardprotokolle in WinHTTP](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Die folgenden älteren Algorithmen werden ebenfalls am 1. Juli 2018 für TLS als veraltet markiert:

* RC4 (Rivest Cipher 4)
* DES (Data Encryption Algorithm)
* 3DES (Triple Data Encryption Algorithm)
* MD5 (Message Digest 5)

---
title: Installieren eines P2S-Clientzertifikats | Azure
description: "Dieser Artikel unterstützt Sie beim Installieren eines Clientzertifikats für P2S-Zertifikatauthentifizierung (Point-to-Site)."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/24/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: fc0cc37794ef291c9d27b094211b38cec90da55c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Installieren eines Clientzertifikats für Point-to-Site-Verbindungen mit Azure-Zertifikatauthentifizierung

Alle Clients, die mithilfe der P2S-Azure-Zertifikatauthentifizierung eine Verbindung mit virtuellen Netzwerken herstellen, benötigen ein Clientzertifikat. Dieser Artikel unterstützt Sie beim Installieren eines Clientzertifikats, das beim Herstellen einer Verbindung mit einem VNET über P2S für die Authentifizierung verwendet wird.

## <a name="generate"></a>Generieren und Exportieren eines Clientzertifikats

Sie können ein Clientzertifikat mithilfe eines Stammzertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle generiert wurde, oder mithilfe eines selbstsignierten Zertifikats erstellen. Die entsprechenden Schritte finden Sie in den Anweisungen für [PowerShell](vpn-gateway-certificates-point-to-site.md) oder [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Exportieren Sie Clientzertifikate nach ihrer Erstellung als PFX-Dateien. Achten Sie darauf, beim Exportieren die gesamte Zertifikatkette aufzunehmen.

## <a name="installwin"></a>Installieren eines Zertifikats auf Windows-Clients

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Installieren eines Zertifikats auf Mac-Clients

Mac-VPN-Clients werden nur für das Resource Manager-Bereitstellungsmodell unterstützt. Für das klassische Bereitstellungsmodell werden sie nicht unterstützt.

> [!NOTE]
>  IKEv2 befindet sich derzeit in der Vorschauversion.
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit den Schritten für die P2S-Konfiguration fort.

* [Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure-Portal (klassisch)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

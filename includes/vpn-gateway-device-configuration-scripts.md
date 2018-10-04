---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 00889717e0c22477b9933725bccc7de05c82bc4f
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454562"
---
| **Hersteller** | **Gerätefamilie** | **Firmwareversion** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (Vorschau)|
|Cisco | ASA | ASA (*) RouteBased (IKEv2 – ohne BGP) für ASA unter 9.8 |
|Cisco | ASA | ASA RouteBased (IKEv2 – ohne BGP) für ASA 9.8 und höher |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> (*) Erforderlich: NarrowAzureTrafficSelectors (Option „UsePolicyBasedTrafficSelectors“ aktivieren) und CustomAzurePolicies (IKE/IPsec)
>

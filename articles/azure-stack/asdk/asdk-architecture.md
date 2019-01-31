---
title: Azure Stack Development Kit – Architektur | Microsoft-Dokumentation
description: In diesem Artikel wird die Architektur des Azure Stack Development Kits (ASDK) beschrieben.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 2cee4d75a111f56012782228d8070bb728720aca
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238335"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Microsoft Azure Stack Development Kit – Architektur
Das Azure Stack Development Kit (ASDK) ist eine Bereitstellung von Azure Stack mit einem Knoten. Alle Komponenten werden auf virtuellen Computern installiert, die auf einem einzigen Hostcomputer ausgeführt werden. 

## <a name="logical-architecture-diagram"></a>Logisches Architekturdiagramm
Das folgende Diagramm veranschaulicht die logische Architektur des ASDK und der zugehörigen Komponenten.

![ASDK-Architektur](media/asdk-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Rollen virtueller Computer
Das ASDK bietet Dienste unter Verwendung der folgenden auf dem Hostcomputer des Development Kits gehosteten VMs:

| NAME | BESCHREIBUNG |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack-Speicherdienste|
| **AzS-ADFS01** | Active Directory-Verbunddienste (ADFS)  |
| **AzS-BGPNAT01** | Edgerouter, bietet NAT- und VPN-Funktionen für Azure Stack |
| **AzS-CA01** | Zertifizierungsstellendienste für Azure Stack-Rollendienste|
| **AzS-DC01** | Active Directory, DNS und DHCP-Dienste für Microsoft Azure Stack|
| **AzS-ERCS01** | Notfallwiederherstellungskonsolen-VM |
| **AzS-GWY01** | Edge-Gateway-Dienste, z.B. VPN-Site-to-Site-Verbindungen für Mandantennetzwerke|
| **AzS-NC01** | Netzwerkcontroller, der Azure Stack-Netzwerkdienste verwaltet  |
| **AzS-SLB01** | Lastenausgleichs-Multiplexerdienste in Azure Stack für beide Mandanten und Azure Stack-Infrastrukturdienste  |
| **AzS-SQL01** | Interner Datenspeicher für Azure Stack-Infrastrukturrollen  |
| **AzS-WAS01** | Azure Stack-Verwaltungsportal und Azure Resource Manager-Dienste|
| **AzS-WASP01**| Azure Stack-Benutzerportal (Mandantenportal) und Azure Resource Manager-Dienste|
| **AzS-XRP01** | Infrastrukturverwaltungscontroller für Microsoft Azure Stack, einschließlich Compute-, Netzwerk- und Speicherressourcenanbietern|


## <a name="next-steps"></a>Nächste Schritte
[Informationen zu grundlegenden ASDK-Verwaltungsaufgaben](asdk-admin-basics.md)

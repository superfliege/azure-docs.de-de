---
title: "Unterstützte Gastbetriebssysteme für Azure Stack | Microsoft-Dokumentation"
description: "Diese Gastbetriebssysteme können mit Azure Stack verwendet werden."
services: azure-stack
documentationcenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: JeffGoldner
ms.openlocfilehash: 0a31da6cbc2c245b959825a4e715d0dc7511ba99
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Für Azure Stack unterstützte Gastbetriebssysteme

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

## <a name="windows"></a>Windows
Azure Stack unterstützt die folgenden Windows-Gastbetriebssysteme. Im Marketplace sind Images zum Download in Azure Stack verfügbar. Windows-Clientimages stehen im Marketplace nicht zur Verfügung.

Während der Bereitstellung stellt Azure Stack sicher, dass eine geeignete Version des Gast-Agents in das Image eingefügt wird.

| Betriebssystem | BESCHREIBUNG | Herausgeber | OS Type (Betriebssystemtyp) | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 Bit | Microsoft | Windows | Datacenter |
| Windows Server 2012 | 64 Bit | Microsoft | Windows | Datacenter |
| Windows Server 2012 R2 | 64 Bit | Microsoft | Windows | Datacenter |
| Windows Server 2016 | 64 Bit | Microsoft | Windows | Datacenter, Datacenter Core, Datacenter mit Containern |
| Windows 7 | 64 Bit, Pro und Enterprise | Microsoft | Windows | Nein  |
| Windows 8.1 | 64 Bit, Pro und Enterprise | Microsoft | Windows | Nein  |
| Windows 10 | 64 Bit, Pro und Enterprise | Microsoft | Windows | Nein  |

## <a name="linux"></a>Linux

Die hier aufgeführten Linux-Distributionen enthalten den erforderlichen Windows Azure Linux Agent (WALA). 

> [!NOTE]
> Images, die mit älteren WALA-Versionen als 2.2.3 erstellt wurden, werden *nicht* unterstützt und lassen sich in der Regel nicht bereitstellen.

| Distribution | BESCHREIBUNG | Herausgeber | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Container Linux |  64 Bit | CoreOS | Stable |
| 6.9 (CentOS-basiert) | 64 Bit | Rogue Wave | Ja |
| 7.3 (CentOS-basiert) | 64 Bit | Rogue Wave | Ja |
| 7.4 (CentOS-basiert) | 64 Bit | Rogue Wave | Ja |
| Debian 8 „Jessie“ | 64 Bit | credativ |  Ja |
| Debian 9 „Stretch“ | 64 Bit | credativ | Ja |
| Oracle Linux | 64 Bit | Oracle | Nein  |
| Red Hat Enterprise Linux 7.x | 64 Bit | Red Hat | Nein  |
| SLES 11SP4 | 64 Bit | SUSE | Ja |
| SLES 12SP3 | 64 Bit | SUSE | Ja |
| Ubuntu 14.04-LTS | 64 Bit | Canonical | Ja |
| Ubuntu 16.04-LTS | 64 Bit | Canonical | Ja |

Andere Linux-Distributionen können in Zukunft unterstützt werden.





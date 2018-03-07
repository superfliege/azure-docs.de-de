---
title: "Unterstützte Gastbetriebssysteme für Azure Stack | Microsoft-Dokumentation"
description: "Diese Gastbetriebssysteme können mit Azure Stack verwendet werden."
services: azure-stack
documentationcenter: 
author: Brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: 3eceb740b8115d2eaca517017f6158744d6e8e58
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Für Azure Stack unterstützte Gastbetriebssysteme

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

## <a name="windows"></a>Windows
Azure Stack unterstützt die in der folgenden Tabelle aufgeführten Windows-Gastbetriebssysteme: Images im Marketplace stehen zum Download in Azure Stack zur Verfügung. Windows-Clientimages stehen im Marketplace nicht zur Verfügung.

Während der Bereitstellung fügt Azure Stack eine geeignete Version des Gast-Agents in das Image ein.

| Betriebssystem | BESCHREIBUNG | Herausgeber | OS Type (Betriebssystemtyp) | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 Bit | Microsoft | Windows | Datacenter |
| Windows Server 2012 | 64 Bit | Microsoft | Windows | Datacenter |
| Windows Server 2012 R2 | 64 Bit | Microsoft | Windows | Datacenter |
| Windows Server 2016 | 64 Bit | Microsoft | Windows | Datacenter, Datacenter Core, Datacenter mit Containern |
| Windows 10 *(siehe Hinweis 1)* | 64 Bit, Pro und Enterprise | Microsoft | Windows | Nein  |

***Hinweis 1:*** *Um Windows 10-Clientbetriebssysteme für Azure Stack bereitzustellen, benötigen Sie eine [benutzerspezifische Windows-Lizenzierung](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx), oder Sie müssen einen Qualified Multitenant Hoster ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)) erwerben.*


## <a name="linux"></a>Linux

Die hier aufgeführten Linux-Distributionen enthalten den erforderlichen Windows Azure Linux Agent (WALA).

> [!NOTE]   
> Images, die mit älteren WALA-Versionen als 2.2.3 erstellt wurden, werden *nicht* unterstützt und lassen sich in der Regel nicht bereitstellen. Bei einigen WALA-Agent-Versionen ist bekannt, dass sie nicht auf Azure Stack-VMs, einschließlich der Versionen 2.2.12 und 2.2.13, funktionieren.
>
> [cloud-init](https://cloud-init.io/) wird nur in Ubuntu-Distributionen in Azure Stack unterstützt.

| Distribution | BESCHREIBUNG | Herausgeber | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Container Linux |  64 Bit | CoreOS | Stable |
| 6.9 (CentOS-basiert) | 64 Bit | Rogue Wave | Ja |
| 7.4 (CentOS-basiert) | 64 Bit | Rogue Wave | Ja |
| Debian 8 „Jessie“ | 64 Bit | credativ |  Ja |
| Debian 9 „Stretch“ | 64 Bit | credativ | Ja |
| Red Hat Enterprise Linux 7.x (ausstehend) | 64 Bit | Red Hat | Nein  |
| SLES 11SP4 | 64 Bit | SUSE | Ja |
| SLES 12SP3 | 64 Bit | SUSE | Ja |
| Ubuntu 14.04-LTS | 64 Bit | Canonical | Ja |
| Ubuntu 16.04-LTS | 64 Bit | Canonical | Ja |

Andere Linux-Distributionen können in Zukunft unterstützt werden.

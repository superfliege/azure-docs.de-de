---
title: Includedatei
description: Includedatei
services: backup
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 63922eb623576379058c9a8a367d6e52249115f2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109092"
---
Für Azure Backup gelten die folgenden Beschränkungen.

| **Begrenzung** | **Standard** |
| --- | --- |
| Server/Computer, die in einem Tresor registriert werden können | Windows Server/Windows Client/System Center DPM: 50 <br/><br/> IaaS-VMs: 1000  |
| Die Größe einer Datenquelle im Tresorspeicher |Maximal 54400 GB. Die Beschränkung gilt nicht für IaaS-VM-Sicherungen. |
| Sicherungstresore in einem Azure-Abonnement |500 Tresore pro Region |
| Planen täglicher Sicherungen |Windows Server/Client: dreimal täglich<br/> System Center DPM zweimal täglich <br/> IaaS-VMs: einmal täglich  |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind | 32 |
| Einzelner Datenträger, der für die Sicherung an einen virtuellen Azure-Computer angeschlossen ist| 4095 GB|




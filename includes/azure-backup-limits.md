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
ms.openlocfilehash: 9ad1208283527f35e04dede2706fd0e424096dc5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54300669"
---
Für Azure Backup gelten die folgenden Beschränkungen.

| **Begrenzung** | **Standard** |
| --- | --- |
| Server/Computer, die in einem Tresor registriert werden können | Windows Server/Windows Client/System Center DPM: 50 <br/><br/> IaaS-VMs: 1000  |
| Die Größe einer Datenquelle im Tresorspeicher |Maximal 54400 GB. Die Beschränkung gilt nicht für IaaS-VM-Sicherungen. |
| Sicherungstresore in einem Azure-Abonnement |500 Tresore pro Region |
| Planen täglicher Sicherungen |Windows Server/Client: dreimal täglich<br/> System Center DPM zweimal täglich <br/> IaaS-VMs: einmal täglich  |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind | 16 |
| Einzelner Datenträger, der für die Sicherung an einen virtuellen Azure-Computer angeschlossen ist| 4095 GB|

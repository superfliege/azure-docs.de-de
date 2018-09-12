---
title: Includedatei
description: Includedatei
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 9/10/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 64101ea5a3bbaac4a6b2e349a04d06ea84a87081
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381084"
---
Für Azure Backup gelten die folgenden Beschränkungen.

| Bezeichnung | Standardlimit |
| --- | --- |
| Anzahl der Server/Computer, die für jeden Tresor registriert werden können |50 für Windows Server/Windows Client/SCDPM  <br/> 1.000 für IaaS-VMs |
| Größe einer Datenquelle für im Azure-Tresorspeicher gespeicherten Daten |Max. 54.400 GB<sup>1</sup> |
| Anzahl der Sicherungstresore, die in einem Azure-Abonnement erstellt werden können |500 Recovery Services-Tresore pro Region |
| Häufigkeit der geplanten Sicherungen pro Tag |3 Sicherungen pro Tag für Windows Server und Windows Client  <br/> 2 pro Tag für SCDPM <br/> Eine Sicherung pro Tag für virtuelle IaaS-Computer |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind |16 |
| Größe einzelner Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angefügt sind| 4095 GB|

* <sup>1</sup>Die Beschränkung von  54.400 GB gilt nicht für IaaS-VM-Sicherungen.
 


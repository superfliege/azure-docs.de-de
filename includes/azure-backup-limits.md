---
title: Includedatei
description: Includedatei
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 1f41567f7d5dcc9b6536b47ef639ba53abff8ec4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
Für Azure Backup gelten die folgenden Beschränkungen.

| Bezeichnung | Standardlimit |
| --- | --- |
| Anzahl der Server/Computer, die für jeden Tresor registriert werden können |50 für Windows Server/Windows Client/SCDPM  <br/> 1.000 für IaaS-VMs |
| Größe einer Datenquelle für im Azure-Tresorspeicher gespeicherten Daten |Max. 54.400 GB<sup>1</sup> |
| Anzahl der Sicherungstresore, die in einem Azure-Abonnement erstellt werden können |500 Recovery Services-Tresore pro Region |
| Häufigkeit der geplanten Sicherungen pro Tag |3 Sicherungen pro Tag für Windows Server und Windows Client  <br/> 2 pro Tag für SCDPM <br/> Eine Sicherung pro Tag für virtuelle IaaS-Computer |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind |16 |
| Größe einzelner Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angefügt sind| 4095 GB <sup>2</sup>|

* <sup>1</sup>Die Beschränkung von  54.400 GB gilt nicht für IaaS-VM-Sicherungen.
 


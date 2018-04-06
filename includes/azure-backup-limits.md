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
ms.openlocfilehash: b345283f87c446ff3b583b0c5dd8a4be222303ae
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
Für Azure Backup gelten die folgenden Beschränkungen.

| Bezeichnung | Standardlimit |
| --- | --- |
| Anzahl der Server/Computer, die für jeden Tresor registriert werden können |50 für Windows Server/Windows Client/SCDPM  <br/> 200 für virtuelle IaaS-Computer |
| Größe einer Datenquelle für im Azure-Tresorspeicher gespeicherten Daten |Max. 54.400 GB<sup>1</sup> |
| Anzahl der Sicherungstresore, die in einem Azure-Abonnement erstellt werden können |25 Recovery Services-Tresore pro Region |
| Häufigkeit der geplanten Sicherungen pro Tag |3 Sicherungen pro Tag für Windows Server und Windows Client  <br/> 2 pro Tag für SCDPM <br/> Eine Sicherung pro Tag für virtuelle IaaS-Computer |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind |16 |
| Größe einzelner Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angefügt sind| 4095 GB <sup>2</sup>|

* <sup>1</sup>Die Beschränkung von  54.400 GB gilt nicht für IaaS-VM-Sicherungen.
 


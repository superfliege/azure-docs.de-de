---
title: Durchführen eines Upgrades für den Azure Backup-Agent
description: In diesem Artikel wird erläutert, warum Sie ein Upgrade für den Azure Backup-Agent durchführen sollten und wo das Upgrade heruntergeladen werden kann.
services: backup
cloud: ''
suite: ''
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 3/16/2018
ms.author: markgal
ms.openlocfilehash: 4142f88c3ccb376acbdd2af07c5cfdf8fd275780
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
ms.locfileid: "29973298"
---
## <a name="upgrade-the-mars-agent"></a>Durchführen eines Upgrades für den MARS-Agent
Versionen des MARS-Agents (Microsoft Azure Recovery Service) unter 2.0.9083.0 weisen eine Abhängigkeit vom Azure Access Control Service (ACS) auf. Im Jahr 2018 wird [der Azure Access Control Service (ACS) in Azure als veraltet gekennzeichnet](../articles/active-directory/develop/active-directory-acs-migration.md). Seit dem 19. März 2018 treten in allen Versionen des MARS-Agents unter 2.0.9083.0 Sicherungsfehler auf. Um Sicherungsfehler zu vermeiden oder zu beheben, [führen Sie ein Upgrade für den MARS-Agent auf die neueste Version durch](https://go.microsoft.com/fwlink/?linkid=229525). Um die Server zu ermitteln, bei denen ein Upgrade für den MARS-Agent erforderlich ist, führen Sie die Schritte im [Sicherungsblog zur Durchführung eines Upgrades für den Azure Backup-Agent](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/) durch. Der MARS-Agent wird verwendet, um die folgenden Datentypen in Azure sichern:
- Dateien 
- Systemstatusdaten
- Sicherung von System Center DPM in Azure
- Azure Backup Server (MABS)

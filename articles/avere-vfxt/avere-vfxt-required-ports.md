---
title: Whitelist für Ports für Avere vFXT für Azure
description: Liste der von Avere vFXT für Azure verwendeten Ports
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 394415ffc7593d058d8d0bf1c0040b88ec25073b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669927"
---
# <a name="required-ports"></a>Erforderliche Ports

Die in diesem Abschnitt aufgeführten Ports werden für die eingehende und ausgehende Kommunikation von vFXT verwendet.

Setzen Sie den vFXT-Cluster oder die Clustercontrollerinstanz niemals direkt dem öffentlich zugänglichen Internet aus.

## <a name="api"></a>API

| Eingehend: | | |
| --- | ---- | --- |
| TCP | 22  | SSH  |
| TCP | 443 | HTTPS|



| Ausgehend: |     |       |
|----------|-----|-------|
| TCP      | 443 | HTTPS |

 
## <a name="nfs"></a>NFS

| Eingehend und ausgehend  | | |
| --- | --- | ---|
| TCP/UDP | 111  | RPCBIND  |
| TCP/UDP | 2049 | NFS      |
| TCP/UDP | 4045 | NLOCKMGR |
| TCP/UDP | 4046 | MOUNTD   |
| TCP/UDP | 4047 | STATUS   |


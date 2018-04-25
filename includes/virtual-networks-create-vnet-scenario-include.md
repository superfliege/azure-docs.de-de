---
title: Includedatei
description: Includedatei
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 29b2ac1a5dc128028dbd40e683c1b45e6208d124
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Szenario

Um zu veranschaulichen, wie Sie ein VNET und Subnetze erstellen, wird in diesem Dokument das folgende Szenario verwendet:

![VNet-Szenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

In diesem Szenario erstellen Sie ein VNET namens **TestVNet** mit dem reservierten CIDR-Block  **192.168.0.0./16**. Das VNET enthält die folgenden Subnetze: 

* **FrontEnd**, mit **192.168.1.0/24** als CIDR-Block.
* **BackEnd**, mit **192.168.2.0/24** als CIDR-Block.


---
title: Includedatei
description: Includedatei
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: cff737b8c79c44494cb0151d6a6281550401b26e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
ms.locfileid: "31805054"
---
## <a name="scenario"></a>Szenario

Um zu veranschaulichen, wie Sie ein VNET und Subnetze erstellen, wird in diesem Dokument das folgende Szenario verwendet:

![VNet-Szenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

In diesem Szenario erstellen Sie ein VNET namens **TestVNet** mit dem reservierten CIDR-Block  **192.168.0.0./16**. Das VNET enthält die folgenden Subnetze: 

* **FrontEnd**, mit **192.168.1.0/24** als CIDR-Block.
* **BackEnd**, mit **192.168.2.0/24** als CIDR-Block.


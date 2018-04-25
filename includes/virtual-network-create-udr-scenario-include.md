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
ms.openlocfilehash: aa513d63a2af0fe994b8ab1ed7335a30998ff8ce
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Szenario
Um zu veranschaulichen, wie Sie UDRs erstellen, wird in diesem Dokument das folgende Szenario verwendet:

![BILDBESCHREIBUNG](./media/virtual-network-create-udr-scenario-include/figure1.png)

In diesem Szenario erstellen Sie wie folgt eine benutzerdefinierte Route für das *Front-End-Subnetz* und eine weitere für das *Back-End-Subnetz*: 

* **UDR-FrontEnd**. Die Front-End-UDR wird auf das Subnetz *FrontEnd* angewandt und enthält eine Route:    
  * **RouteToBackend**. Diese Route sendet den gesamten an das Back-End-Subnetz gerichteten Datenverkehr an den virtuellen Computer **FW1**.
* **UDR-BackEnd**. Die Back-End-UDR wird auf das Subnetz *BackEnd* angewandt und enthält eine Route:    
  * **RouteToFrontend**. Diese Route sendet den gesamten an das Front-End-Subnetz gerichteten Datenverkehr an den virtuellen Computer **FW1**.

Durch die Kombination dieser Routen wird sichergestellt, dass der gesamte Datenverkehr von einem Subnetz zu einem anderen an den virtuellen Computer **FW1** weitergeleitet wird, der als virtuelles Gerät fungiert. Sie müssen auch die IP-Weiterleitung für den virtuellen Computer **FW1** aktivieren, damit er den an andere virtuelle Computer gerichteten Datenverkehr empfangen kann.


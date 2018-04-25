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
ms.openlocfilehash: 09c6871fc5243296da2f2defd594afb80c62ac95
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Szenario
Um zu veranschaulichen, wie Sie NSGs erstellen, wird in diesem Dokument das folgende Szenario verwendet:

![VNet-Szenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In diesem Szenario erstellen Sie wie folgt eine NSG für jedes Subnetz im virtuellen Netzwerk **TestVNet**: 

* **NSG-FrontEnd**. Die Front-End-NSG wird auf das Subnetz *FrontEnd* angewandt und enthält zwei Regeln:    
  * **rdp-rule**. Ermöglicht RDP-Datenverkehr in das Subnetz *FrontEnd*.
  * **web-rule**. Ermöglicht HTTP-Datenverkehr in das Subnetz *FrontEnd*.
* **NSG-BackEnd**. Die Back-End-NSG wird auf das Subnetz *BackEnd* angewandt und enthält zwei Regeln:    
  * **sql-rule**. SQL-Datenverkehr ist nur aus dem Subnetz *FrontEnd* zulässig.
  * **web-rule**. Verweigert den gesamten Internetdatenverkehr aus dem Subnetz *BackEnd*.

Durch die Kombination dieser Regeln wird ein DMZ-ähnliches Szenario erstellt, in dem das Back-End-Subnetz nur eingehenden Datenverkehr für SQL aus dem Front-End-Subnetz empfangen kann und keinen Zugriff auf das Internet hat, während das Front-End-Subnetz mit dem Internet kommunizieren und nur eingehende HTTP-Anforderungen empfangen kann.


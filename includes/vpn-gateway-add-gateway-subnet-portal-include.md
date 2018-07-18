---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 158400645423d2e9fe92768786b570e917907d98
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30921419"
---
1. Navigieren Sie im Portal zum virtuellen Netzwerk, für das Sie ein Gateway des virtuellen Netzwerks erstellen möchten.
2. Klicken Sie auf der VNET-Seite im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite „Subnetze“ zu erweitern.
3. Klicken Sie auf der Seite **Subnetze** oben auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen.

  ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet.png "Hinzufügen des Gatewaysubnetzes")
4. Als **Name** für Ihr Subnetz wird automatisch der Wert „GatewaySubnet“ eingefügt. Der Wert „GatewaySubnet“ ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** an Ihre Konfigurationsanforderungen an.

  ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Hinzufügen des Gatewaysubnetzes")
5. Klicken Sie unten auf der Seite auf **OK**, um das Subnetz zu erstellen.
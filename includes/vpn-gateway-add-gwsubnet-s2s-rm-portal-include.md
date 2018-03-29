---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3e02242822283ec8597a7be217228cb95e748761
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
1. Navigieren Sie im Portal zum virtuellen Netzwerk, für das Sie ein Gateway des virtuellen Netzwerks erstellen möchten.
2. Klicken Sie auf der VNET-Seite im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite „Subnetze“ zu erweitern.
3. Klicken Sie auf der Seite **Subnetze** oben auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen.

  ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gwsubnet-s2s-rm-portal-include/addgwsub.png "Hinzufügen des Gatewaysubnetzes")
4. Als **Name** für Ihr Subnetz wird automatisch der Wert „GatewaySubnet“ eingefügt. Der Wert „GatewaySubnet“ ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** an Ihre Konfigurationsanforderungen an.

  ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gwsubnet-s2s-rm-portal-include/addsub2.png "Hinzufügen des Gatewaysubnetzes")
5. Klicken Sie unten auf der Seite auf **OK**, um das Subnetz zu erstellen.
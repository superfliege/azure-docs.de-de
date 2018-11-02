---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: bc1e2803a420b95e1abec0886733c5e42a8cfdb4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026622"
---
1. Navigieren Sie im [Portal](http://portal.azure.com) zum virtuellen Resource Manager-Netzwerk, für das Sie ein virtuelles Netzwerkgateway erstellen möchten.
2. Klicken Sie auf der VNET-Seite im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite **Subnetze** zu erweitern.
3. Klicken Sie auf der Seite **Subnetze** auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen. 

  ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Hinzufügen des Gatewaysubnetzes")
4. Als **Name** für Ihr Subnetz wird automatisch der Wert „GatewaySubnet“ eingefügt. Dieser Wert ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** an Ihre Konfigurationsanforderungen an. Konfigurieren Sie keine Routingtabelle und keine Dienstendpunkte.

  ![Hinzufügen des Subnetzes](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Hinzufügen des Subnetzes")
5.  Klicken Sie unten auf der Seite auf **OK**, um das Subnetz zu erstellen.
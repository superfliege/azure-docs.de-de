---
title: "Fügen Sie mehrere VPN-Gateway-Standort-zu-Standort-Verbindungen in einem VNet: Azure-Portal: Ressourcen-Manager | Microsoft Docs"
description: "VPN-Gateway, das eine vorhandene Verbindung wurde S2S-Verbindungen mit mehreren Standorte hinzugefügt"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: 7ec57789ee76f4ec54e4f7b68ea75c19522f3d7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Fügen Sie eine Standort-zu-Standort-Verbindung, um ein VNet mit einer vorhandenen VPN-Gateway-Verbindung

> [!div class="op_single_selector"]
> * [Azure-Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassisch)](vpn-gateway-multi-site.md)
>
> 

Dieser Artikel führt Sie durch die Verwendung von Azure-Portal um Verbindungen von Standort-zu-Standort (S2S) VPN-Gateway hinzufügen, die eine vorhandene Verbindung wurde. Diese Art von Verbindung wird häufig als "Multi" Standortkonfiguration bezeichnet. Sie können eine S2S-Verbindung mit einem VNet hinzufügen, die bereits über ein S2S-Verbindung, die Punkt-zu-Standort-Verbindung oder die VNet-zu-VNet-Verbindung verfügt. Es gibt einige Einschränkungen auf, wenn Verbindungen hinzufügen. Überprüfen Sie die [Vorbereitungen](#before) Abschnitt in diesem Artikel zu überprüfen, bevor Sie Ihre Konfiguration zu beginnen. 

Dieser Artikel gilt für VNets, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt, die ein RouteBased VPN-Gateway haben. Diese Schritte gelten nicht für die Koexistenz verbindungskonfigurationen ExpressRoute/Standort-zu-Standort. Finden Sie unter [ExpressRoute/S2S Koexistenz Verbindungen](../expressroute/expressroute-howto-coexist-resource-manager.md) Informationen zu Koexistenz Verbindungen.

### <a name="deployment-models-and-methods"></a>Bereitstellungsmodelle und Methoden
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Wir aktualisieren diese Tabelle als neue Artikel, und weitere Tools zur Verfügung stehen, für diese Konfiguration. Wenn ein Artikel verfügbar ist, verknüpft es direkt mit, aus dieser Tabelle.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Bevor Sie beginnen
Überprüfen Sie die folgenden Elemente:

* Erstellen Sie keine Koexistenz ExpressRoute/S2S-Verbindung.
* Sie verfügen über ein virtuelles Netzwerk, das mit dem Ressourcen-Manager-Bereitstellungsmodell mit einer vorhandenen Verbindung erstellt wurde.
* Das Gateway des virtuellen Netzwerks für das VNet ist als routebased festgelegt. Wenn Sie ein PolicyBased VPN-Gateway haben, müssen Sie löschen Gateway des virtuellen Netzwerks und erstellen einen neuen VPN-Gateway als routebased festgelegt.
* Keine der Adressbereiche überschneiden für keines der VNets, die diesem VNet eine Verbindung herstellt.
* Sie haben kompatibles VPN-Gerät und eine Person, die sie konfigurieren kann. Finden Sie unter [zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Wenn Sie nicht mit Ihrem VPN-Gerät vertraut sind, oder die IP-Adresse, die Bereiche befinden sich im nicht vertraut sind mit die lokalen Netzwerkkonfiguration, müssen Sie mit jemandem absprechen, die die Details für Sie bereitstellen kann.
* Sie haben eine externe öffentliche IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse kann nicht hinter einer NAT befinden

## <a name="part1"></a>Teil 1: Konfigurieren Sie eine Verbindung
1. In einem Browser, navigieren Sie zu der [Azure-Portal](http://portal.azure.com) und, falls erforderlich, melden Sie sich mit Ihrem Azure-Konto.
2. Klicken Sie auf **alle Ressourcen** und suchen Sie Ihre **Gateway des virtuellen Netzwerks** aus der Liste der Ressourcen, und klicken Sie darauf.
3. Auf der **Gateway des virtuellen Netzwerks** Blatt, klicken Sie auf **Verbindungen**.
   
    ![Blatt "Verbindungen"](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>
4. Auf der **Verbindungen** Blatt, klicken Sie auf **+ Add**.
   
    ![Verbindungsschaltfläche "hinzufügen"](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>
5. Auf der **Verbindung hinzufügen** Blatt, füllen Sie die folgenden Felder:
   
   * **Name:** den Namen, die Sie auf der Website zu gewähren möchten Sie die Verbindung zu erstellen.
   * **Verbindungstyp:** wählen **Standort-zu-Standort (IPsec)**.
     
     ![Blatt "Verbindung" hinzufügen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Teil 2 – ein lokales Netzwerkgateway hinzufügen
1. Klicken Sie auf **Gateway des lokalen Netzwerks** ***wählen Sie ein lokales Netzwerkgateway***. Dies öffnet die **lokales Netzwerkgateway auswählen** Blatt.
   
    ![Lokales Netzwerkgateway auswählen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klicken Sie auf **neu erstellen** So öffnen die **erstellen lokalen Netzwerkgateway** Blatt.
   
    ![Lokales Netzwerk-Gateway Blatt erstellen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>
3. Auf der **erstellen lokalen Netzwerkgateway** Blatt, füllen Sie die folgenden Felder:
   
   * **Name:** den Namen, die Sie auf die lokalen Netzwerk-Gateway-Ressource erteilen möchten.
   * **IP-Adresse:** die öffentliche IP-Adresse des VPN-Geräts auf die Website, die Sie herstellen möchten.
   * **Adressraum:** den Adressraum, der an die neue Website im lokalen Netzwerk weitergeleitet werden soll.
4. Klicken Sie auf **OK** auf die **erstellen lokalen Netzwerkgateway** Blatt ", um die Änderungen zu speichern.

## <a name="part3"></a>Teil 3 – Fügen Sie den gemeinsam verwendeten Schlüssel hinzu, und erstellen Sie die Verbindung
1. Auf der **Verbindung hinzufügen** Blatt, fügen Sie den gemeinsam verwendeten Schlüssel, die Sie verwenden, um die Verbindung erstellen möchten. Sie können entweder den gemeinsam verwendeten Schlüssel aus Ihrem VPN-Gerät abrufen oder hier einen und konfigurieren Sie dann das VPN-Gerät, um den gleichen gemeinsamen Schlüssel zu verwenden. Wichtig ist, dass die Schlüssel identisch sind.
   
    ![Gemeinsam verwendeter Schlüssel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Klicken Sie unten auf dem Blatt auf **OK** zum Erstellen der Verbindung.

## <a name="part4"></a>Teil 4: Überprüfen Sie die VPN-Verbindung


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Verbindung abgeschlossen wurde, können Sie virtuelle Computer mit Ihren virtuellen Netzwerken hinzufügen. Finden Sie unter den virtuellen Computern [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) für Weitere Informationen.

---
title: "Umstellen eines öffentlichen Peerings über Azure ExpressRoute auf Microsoft-Peering | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie Ihr öffentliches Peering über ExpressRoute auf Microsoft-Peering umstellen."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2017
ms.author: cherylmc
ms.openlocfilehash: 311e1de3200cd684bbec1329ebd5217b4fb3a2e2
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Umstellen von öffentlichem Peering auf Microsoft-Peering

ExpressRoute unterstützt nun Azure-PaaS-Dienste wie Azure Storage und Azure SQL-Datenbank mit Microsoft-Peering und Routenfiltern. Für den Zugriff auf PaaS- und SaaS-Dienste von Microsoft ist nun nur noch eine einzelne Routingdomäne erforderlich. Mit Routenfiltern können Sie selektiv die PaaS-Dienstpräfixe für die zu verwendenden Azure-Regionen ankündigen.

Dieser Artikel erklärt, wie Sie ohne Ausfallzeiten von einer öffentlichen Peeringkonfiguration auf Microsoft-Peering umsteigen. Weitere Informationen zu Routingdomänen und Peerings finden Sie unter [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).

> [!IMPORTANT]
> Für die Verwendung von Microsoft-Peering müssen Sie über das ExpressRoute Premium-Add-On verfügen. Weitere Informationen zum Premium-Add-On finden Sie in den [häufig gestellten Fragen zu ExpressRoute](expressroute-faqs.md#expressroute-premium).

## <a name="before-you-begin"></a>Voraussetzungen

* Um eine Verbindung mit Microsoft-Peering herstellen zu können, müssen Sie die NAT einrichten und verwalten. Die NAT wird unter Umständen von Ihrem Konnektivitätsanbieter eingerichtet und verwaltet. Wenn Sie im Rahmen von Microsoft-Peering auf die Azure-PaaS- und Azure-SaaS-Dienste zugreifen möchten, muss der NAT-IP-Adresspool die richtige Größe haben. Weitere Informationen zur NAT für ExpressRoute finden Sie unter [NAT-Anforderungen für Microsoft-Peering](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Wenn Sie bei Verwendung des öffentlichen Azure-Peerings derzeit eine Netzwerk-Zugriffssteuerungsliste (Access Control List, ACL) für die Azure-PaaS-Dienstendpunkte verwenden, muss der mit Microsoft-Peering konfigurierte NAT-IP-Pool in der für den Dienstendpunkt konfigurierten Zugriffssteuerungsliste enthalten sein.

Die Schritte in diesem Artikel müssen in der angegebenen Reihenfolge ausgeführt werden, damit es bei der Umstellung auf Microsoft-Peering zu keinen Ausfällen kommt.

## <a name="1-create-microsoft-peering"></a>1. Erstellen von Microsoft-Peering

Falls Microsoft-Peering noch nicht erstellt wurde, gehen Sie gemäß einem der folgenden Artikel vor, um es zu erstellen. Wenn Ihr Konnektivitätsanbieter verwaltete Layer 3-Dienste anbietet, können Sie ihn mit der Aktivierung des Microsoft-Peerings für Ihre Verbindung beauftragen.

  * [Erstellen von Microsoft-Peering über das Azure-Portal](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Erstellen von Microsoft-Peering mithilfe von Azure-PowerShell](expressroute-howto-routing-arm.md#msft)
  * [Erstellen von Microsoft-Peering mithilfe der Azure-Befehlszeilenschnittstelle](howto-routing-cli.md#msft)

## <a name="2-validate-microsoft-peering-is-enabled"></a>2. Überprüfen des Aktivierungsstatus von Microsoft-Peering

Vergewissern Sie sich, dass Microsoft-Peering aktiviert ist und die angekündigten öffentlichen Präfixe konfiguriert sind.

  * [Azure-Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure-Befehlszeilenschnittstelle](howto-routing-cli.md#getmsft)

## <a name="3-configure-and-attach-a-route-filter-to-the-circuit"></a>3. Konfigurieren eines Routenfilters und Anfügen des Routenfilters an die Verbindung

Standardmäßig kündigen neue Microsoft-Peerings keine Präfixe an, solange kein Routenfilter an die Verbindung angefügt wurde. Beim Erstellen einer Routenfilterregel können Sie die Liste der Dienstcommunitys für Azure-Regionen angeben, die Sie für Azure-PaaS-Dienste nutzen möchten, wie im folgenden Screenshot zu sehen:

![Zusammenführen des öffentlichen Peerings](.\media\how-to-move-peering\public.png)

Gehen Sie zum Konfigurieren von Routenfiltern gemäß einem der folgenden Artikel vor:

  * [Konfigurieren von Routenfiltern für Microsoft-Peering: Azure-Portal](how-to-routefilter-portal.md)
  * [Konfigurieren von Routenfiltern für Microsoft-Peering: PowerShell](how-to-routefilter-powershell.md)
  * [Konfigurieren von Routenfiltern für Microsoft-Peering: Azure CLI](how-to-routefilter-cli.md)

## <a name="4-delete-the-public-peering"></a>4. Löschen des öffentlichen Peerings

Nachdem Sie sich vergewissert haben, dass das Microsoft-Peering konfiguriert ist und die Präfixe, die Sie nutzen möchten, ordnungsgemäß für Microsoft-Peering angekündigt werden, können Sie das öffentliche Peering löschen. Gehen Sie zum Löschen des öffentlichen Peerings gemäß einem der folgenden Artikel vor:

  * [Löschen des öffentlichen Azure-Peerings über das Azure-Portal](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Löschen des öffentlichen Azure-Peerings mithilfe von Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)
  * [Löschen des öffentlichen Azure-Peerings mithilfe der Befehlszeilenschnittstelle](howto-routing-cli.md#deletepublic)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
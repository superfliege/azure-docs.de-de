---
title: Verwenden eines lokalen Datengateways für Azure Virtual Network-Datenquellen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Server für die Verwendung eines Gateways für Datenquellen in einem VNET konfigurieren.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7cb1fb7bcd3a50532d5ff994afac56e226fb2018
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596905"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Verwenden eines Gateways für Datenquellen in einer Azure Virtual Network-Instanz (VNET)

In diesem Artikel wird die Servereigenschaft **AlwaysUseGateway** beschrieben, die verwendet werden soll, wenn sich Datenquellen in einer [Azure Virtual Network-Instanz (VNET)](../virtual-network/virtual-networks-overview.md) befinden.

## <a name="server-access-to-vnet-data-sources"></a>Serverzugriff auf VNET-Datenquellen

Beim Zugriff auf Ihre Datenquellen über ein VNET muss Ihr Azure Analysis Services-Server mit diesen Datenquellen in Ihrer eigenen Umgebung eine Verbindung herstellen, als ob sie lokal wären. Sie können die Servereigenschaft **AlwaysUseGateway** so konfigurieren, dass der Server angewiesen wird, auf alle Datenquellen über ein [lokales Gateway](analysis-services-gateway.md) zuzugreifen. 

> [!NOTE]
> Diese Eigenschaft ist nur wirksam, wenn ein [lokales Datengateway](analysis-services-gateway.md) installiert und konfiguriert ist. Das Gateway kann sich im VNET befinden.

## <a name="configure-alwaysusegateway-property"></a>Konfigurieren der AlwaysUseGateway-Eigenschaft

1. Klicken Sie unter „SSMS“ > „Server“ > **Eigenschaften** > **Allgemein** auf **Show Advanced (All) Properties** (Erweiterte (alle Eigenschaften) anzeigen).
2. Legen Sie **ASPaaS\AlwaysUseGateway** auf **true** fest.

    ![AlwaysUseGateway-Eigenschaft](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Weitere Informationen
[Herstellen einer Verbindung mit lokalen Datenquellen mit dem lokalen Azure-Datengateway](analysis-services-gateway.md)   
[Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md)   
[Was ist Azure Virtual Network?](../virtual-network/virtual-networks-overview.md)   


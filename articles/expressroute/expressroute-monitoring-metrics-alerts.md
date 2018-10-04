---
title: Azure ExpressRoute-Überwachung – Metriken und Warnungen | Microsoft-Dokumentation
description: Diese Seite enthält Informationen zur ExpressRoute-Überwachung.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: fc8abee93983ce4ea06d0b433eb35ed22e0f61b4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47218070"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute-Überwachung, Metriken und Warnungen

 Dieser Artikel macht Sie mit der ExpressRoute-Überwachung, Metriken und Warnungen vertraut. Azure Monitor ist die Zentrale für alle Metriken, Warnungen und Diagnoseprotokolle im gesamten Azure-System.

## <a name="circuit-metrics"></a>Metriken zu Leitungen

Navigieren Sie zu **Metriken**, und klicken Sie auf die Seite „ExpressRoute“ für die Leitung, die Sie überwachen möchten. Auf der Kachel **Überwachung** können Sie die **Metriken** anzeigen.

![Metriken zu Leitungen](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="metrics-per-peering"></a>Peeringspezifische Metriken

Sie können Metriken für privates, öffentliches und Microsoft-Peering in Bits pro Sekunde anzeigen.

![Peeringspezifische Metriken](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>ExpressRoute-Gatewayverbindungen in Bits pro Sekunde

![Gatewayverbindungen](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>Warnungen zu ExpressRoute-Gatewayverbindungen

1. Um Warnungen zu konfigurieren, navigieren Sie zu **Azure Monitor** und klicken dann auf **Warnungen**.

  ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Klicken Sie auf **+Ziel auswählen**, und wählen Sie die ExpressRoute-Gatewayverbindungsressource aus.

  ![Ziel]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Definieren Sie die Warnungsdetails.

  ![Aktionsgruppe](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)


4. Definieren Sie die Aktionsgruppe, und fügen Sie sie hinzu.

  ![Hinzufügen einer Aktionsgruppe](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Warnungen basierend auf dem jeweiligen Peering

 ![was](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Konfigurieren von Warnungen für Aktivitätsprotokolle zu Leitungen

In **Warnungskriterien** können Sie **Aktivitätsprotokoll** als Signaltyp und dann das Signal auswählen.

  ![andere](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
  * [Erstellen und Ändern einer Verbindung](expressroute-howto-circuit-arm.md)
  * [Erstellen und Ändern einer Peeringkonfiguration](expressroute-howto-routing-arm.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)

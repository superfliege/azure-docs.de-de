---
title: "Skalieren der Power BI Embedded-Kapazität | Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, wie Sie eine Power BI Embedded-Kapazität in Microsoft Azure skalieren."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: e1ab6a2f52fa56f1e04c6c327796587daf43596e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="scale-your-power-bi-embedded-capacity"></a>Skalieren der Power BI Embedded-Kapazität

In diesem Artikel wird erläutert, wie Sie eine Power BI Embedded-Kapazität in Microsoft Azure skalieren. Eine Skalierung ermöglicht Ihnen, die Größe Ihrer Kapazität zu erhöhen oder zu verringern.

Dies setzt voraus, dass Sie eine Power BI Embedded-Kapazität eingerichtet haben. Falls nicht, finden Sie unter [Einrichten von Power BI Embedded-Kapazität im Azure-Portal](create-capacity.md) Informationen zum Einstieg.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="scale-a-capacity"></a>Skalieren von Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Weitere Dienste** > **Power BI Embedded** aus, um Ihre Kapazitäten anzuzeigen.

    ![Weitere Dienste im Azure-Portal](media/scale-capacity/azure-portal-more-services.png)

3. Wählen Sie die Kapazität aus, die Sie skalieren möchten.

    ![Liste mit Power BI Embedded-Kapazitäten im Azure-Portal](media/scale-capacity/azure-portal-capacity-list.png)

4. Wählen Sie innerhalb Ihrer Kapazität unter **Staffelung** die Option **Tarif** aus.

    ![Option „Tarif“ unter „Staffelung“](media/scale-capacity/azure-portal-scale-pricing-tier.png)

    Ihr aktueller Tarif ist blau hervorgehoben.

    ![Aktueller Tarif blau hervorgehoben](media/scale-capacity/azure-portal-current-tier.png)

5. Wählen Sie für eine Hoch- oder Herunterskalierung den neuen Tarif aus, zu dem gewechselt werden soll. Wenn Sie einen neuen Tarif auswählen, wird eine blaue Umrandung um die Auswahl gezeichnet. Klicken Sie auf **Auswählen**, um zum neuen Tarif zu wechseln.

    ![Auswählen des neuen Tarifs](media/scale-capacity/azure-portal-select-new-tier.png)

    Die Skalierung Ihrer Kapazität kann einige Minuten dauern.

6. Überprüfen Sie den neuen Tarif auf der Registerkarte „Übersicht“. Der aktuelle Tarif ist aufgeführt.

    ![Bestätigen des aktuellen Tarifs](media/scale-capacity/azure-portal-confirm-tier.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anhalten oder Starten Ihrer Kapazität finden Sie unter [Anhalten und Starten Ihrer Power BI Embedded-Kapazität im Azure-Portal](pause-start.md).

Informationen zum Einbetten von Power BI-Inhalten in Ihre Anwendung finden Sie unter [So können Sie Ihre Dashboards, Berichte und Kacheln von Power BI einbetten](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Weitere Fragen? [Fragen Sie die Power BI-Community](http://community.powerbi.com/)
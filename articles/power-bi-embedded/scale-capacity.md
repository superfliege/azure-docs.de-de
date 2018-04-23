---
title: Skalieren der Power BI Embedded-Kapazität | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie eine Power BI Embedded-Kapazität in Microsoft Azure skalieren.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/19/2018
ms.author: maghan
ms.openlocfilehash: a089d34b3f49188f3037ad57aab4d7bcc8c660f2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="scale-your-power-bi-embedded-capacity"></a>Skalieren der Power BI Embedded-Kapazität

In diesem Artikel wird erläutert, wie Sie eine Power BI Embedded-Kapazität in Microsoft Azure skalieren. Eine Skalierung ermöglicht Ihnen, die Größe Ihrer Kapazität zu erhöhen oder zu verringern.

Dies setzt voraus, dass Sie eine Power BI Embedded-Kapazität eingerichtet haben. Falls nicht, finden Sie unter [Einrichten von Power BI Embedded-Kapazität im Azure-Portal](create-capacity.md) Informationen zum Einstieg.

> [!NOTE]
> Ein Skalierungsvorgang kann etwa eine Minute dauern. In diesem Zeitraum ist die Kapazität nicht verfügbar. Eingebetteter Inhalt kann unter Umständen nicht geladen werden.

## <a name="scale-a-capacity"></a>Skalieren von Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Alle Dienste** > **Power BI Embedded** aus, um Ihre Kapazitäten anzuzeigen.

    ![„Alle Dienste“ im Azure-Portal](media/scale-capacity/azure-portal-more-services.png)

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

Weitere Fragen? [Fragen Sie die Power BI-Community](http://community.powerbi.com/).

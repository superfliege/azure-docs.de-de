---
title: "Anhalten und Starten Ihrer Power BI Embedded-Kapazität im Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, wie Sie eine Power BI Embedded-Kapazität in Microsoft Azure anhalten und starten."
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
ms.openlocfilehash: ab5d184fde2cbcb517b325624c8405954b4d0972
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="pause-and-start-your-power-bi-embedded-capacity-in-the-azure-portal"></a>Anhalten und Starten Ihrer Power BI Embedded-Kapazität im Azure-Portal

In diesem Artikel wird erläutert, wie Sie eine Power BI Embedded-Kapazität in Microsoft Azure anhalten und starten. Dies setzt voraus, dass Sie eine Power BI Embedded-Kapazität eingerichtet haben. Falls nicht, finden Sie unter [Einrichten von Power BI Embedded-Kapazität im Azure-Portal](create-capacity.md) Informationen zum Einstieg.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="pause-your-capacity"></a>Anhalten Ihrer Kapazität

Durch das Anhalten der Kapazität wird die Rechnungsstellung verhindert. Das Anhalten Ihrer Kapazität ist dann sinnvoll, wenn Sie die Kapazität über einen längeren Zeitraum nicht nutzen müssen. Führen Sie die folgenden Schritte aus, um Ihre Kapazität anzuhalten.

> [!NOTE]
> Das Anhalten einer Kapazität kann verhindern, dass Inhalte innerhalb von Power BI verfügbar sind. Stellen Sie sicher, dass Sie die Zuordnung von Arbeitsbereichen zu Ihrer Kapazität aufheben, um Störungen zu vermeiden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Weitere Dienste** > **Power BI Embedded** aus, um Ihre Kapazitäten anzuzeigen.

    ![Weitere Dienste im Azure-Portal](media/pause-start/azure-portal-more-services.png)

3. Wählen Sie die Kapazität aus, die Sie anhalten möchten.

    ![Liste mit Power BI Embedded-Kapazitäten im Azure-Portal](media/pause-start/azure-portal-capacity-list.png)

4. Wählen Sie in den Kapazitätsdetails **Anhalten** aus.

    ![Anhalten Ihrer Kapazität](media/pause-start/azure-portal-pause-capacity.png)

5. Wählen Sie **Ja** aus, was bestätigt, dass die Kapazität angehalten werden soll.

    ![Anhalten bestätigen](media/pause-start/azure-portal-confirm-pause.png)

## <a name="start-your-capacity"></a>Starten Ihrer Kapazität

Setzen Sie die Nutzung durch Starten Ihrer Kapazität fort. Mit dem Starten Ihrer Kapazität wird auch die Abrechnung fortgesetzt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **Weitere Dienste** > **Power BI Embedded** aus, um Ihre Kapazitäten anzuzeigen.

    ![Weitere Dienste im Azure-Portal](media/pause-start/azure-portal-more-services.png)

3. Wählen Sie die Kapazität aus, die Sie starten möchten.

    ![Liste mit Power BI Embedded-Kapazitäten im Azure-Portal](media/pause-start/azure-portal-capacity-list.png)

4. Wählen Sie in den Kapazitätsdetails **Starten** aus.

    ![Starten Ihrer Kapazität](media/pause-start/azure-portal-start-capacity.png)

5. Wählen Sie **Ja** aus, was bestätigt, dass die Kapazität gestartet werden soll.

    ![Starten bestätigen](media/pause-start/azure-portal-confirm-start.png)

Falls dieser Kapazität Inhalte zugeordnet sind, stehen diese nach dem Start zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihre Kapazität nach oben oder unten skaliert werden soll, siehe [Skalieren Ihrer Power BI Embedded-Kapazität](scale-capacity.md).

Informationen zum Einbetten von Power BI-Inhalten in Ihre Anwendung finden Sie unter [So können Sie Ihre Dashboards, Berichte und Kacheln von Power BI einbetten](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Weitere Fragen? [Fragen Sie die Power BI-Community](http://community.powerbi.com/)
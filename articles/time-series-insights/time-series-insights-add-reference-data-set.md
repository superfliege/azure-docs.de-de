---
title: "Hinzufügen eines Verweis-DataSets zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie Ihrer Azure Time Series Insights-Umgebung ein Verweis-DataSet hinzufügen. Verweisdaten lassen sich praktisch mit Ihren Quelldaten verknüpfen, um die Werte zu erweitern."
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 7cdcefbd0daec3b7bab59680afa1470624583c74
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Erstellen eines Verweis-DataSets für Ihre Time Series Insights-Umgebung über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie Ihrer Azure Time Series Insights-Umgebung ein Verweis-DataSet hinzufügen. Verweisdaten lassen sich praktisch mit Ihren Quelldaten verknüpfen, um die Werte zu erweitern.

Ein Verweisdataset ist eine Sammlung von Elementen, die mit den Ereignissen aus Ihrer Ereignisquelle ergänzt werden. Das Time Series Insights-Erfassungsmodul verknüpft ein Ereignis aus Ihrer Ereignisquelle mit einem Element in Ihrem Verweisdataset. Dieses ergänzte Ereignis ist dann für Abfragen verfügbar. Diese Verknüpfung basiert auf den in Ihrem Verweisdataset definierten Schlüsseln.

## <a name="add-a-reference-data-set"></a>Hinzufügen eines Verweis-DataSets

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Suchen Sie die vorhandene Time Series Insights-Umgebung. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**. Wählen Sie Ihre Time Series Insights-Umgebung aus.

3. Wählen Sie unter der Überschrift **Umgebungstopologie** die Option **Verweis-DataSets** aus.

    ![Erstellen des Time Series Insights-Verweisdatasets](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Wählen Sie **+ Hinzufügen** aus, um ein neues Verweis-DataSet hinzuzufügen.

5. Geben Sie einen eindeutigen **Namen des Verweisdatasets** an.

    ![Erstellen des Time Series Insights-Verweisdatasets – Details](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

6. Geben Sie den **Schlüsselnamen** in dem leeren Feld an, und wählen Sie den zugehörigen **Typ** aus. Dieser Name und Typ werden verwendet, um die richtige Eigenschaft aus den Ereignissen in der Ereignisquelle zum Verknüpfen mit den Referenzdaten auszuwählen. 

   Wenn Sie beispielsweise den Schlüsselnamen **DeviceId** und den Typ **Zeichenfolge** angeben, sucht das Time Series Insights-Erfassungsmodul in jedem zu durchsuchenden und zu verknüpfenden eingehenden Ereignis nach einer Eigenschaft mit dem Namen **DeviceId** vom Typ **Zeichenfolge**. Sie können mehrere Schlüssel zur Verknüpfung mit dem Ereignis angeben. Beim Vergleichen der Schlüsselnamen wird die Groß-/Kleinschreibung beachtet.

7. Klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten Sie Verweisdaten](time-series-insights-manage-reference-data-csharp.md) programmgesteuert.
* Die vollständige API-Referenz finden Sie im Dokument zur [Verweisdaten-API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).

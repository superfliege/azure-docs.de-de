---
title: Vorhersage der Ausgaben mit Azure-Kostenverwaltung | Microsoft-Dokumentation
description: Prognostizieren Sie Ausgaben mithilfe von historischer Nutzung und Ausgabendaten.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 09/29/2017
ms.topic: article
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: decae695544e8b9e0ff48cca32a38f533a86a563
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="forecast-future-spending"></a>Vorhersage zukünftiger Ausgaben

Mit der Azure-Kostenverwaltung von Cloudyn können Sie zukünftige Ausgaben mithilfe von historischer Nutzung und Ausgabendaten prognostizieren. Sie verwenden Cloudyn-Berichte, um alle Kostenprognosedaten anzuzeigen. Die Beispiele dieses Tutorials führen Sie in einer exemplarischen Vorgehensweise durch das Überprüfen der Kostenprognosen mithilfe der Berichte. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorhersage zukünftiger Ausgaben

## <a name="forecast-future-spending"></a>Vorhersage zukünftiger Ausgaben

Cloudyn enthält Kostenprognoseberichte, anhand derer Sie Ausgaben basierend auf Ihrer Nutzung im Laufe der Zeit vorhersagen können. Der Hauptzweck besteht darin, Sie dabei zu unterstützen, sicherzustellen, dass Ihre Kostentrends die Erwartungen Ihrer Organisation nicht überschreiten. Die Berichte, die Sie verwenden, beziehen sich auf die aktuellen monatlichen vorhergesagten und jährlichen vorhergesagten Kosten. Beide zeigen, welche Ausgaben zu erwarten sind, wenn Ihre Nutzung mit der Nutzung innerhalb der letzten 30 Tage relativ konsistent bleibt.

Der Bericht über die aktuellen monatlichen vorhergesagten Kosten zeigt die Kosten für Ihre Dienste. Er bezieht sich auf Kosten ab Anfang des Monats und des vorhergehenden Monats, um die voraussichtlichen Kosten anzuzeigen. Klicken Sie im Berichtsmenü am oberen Rand des Portals auf **Cost** > **Projection and Budget** > **Current Month Projected Cost**. Die folgende Abbildung zeigt ein Beispiel.

![Vorhergesagte Kosten im aktuellen Monat](./media/tutorial-forecast-spending/project-month01.png)

Im Beispiel können Sie sehen, für welche Dienste die höchsten Kosten entstanden sind. Azure-Kosten waren niedriger als AWS Kosten. Wenn Sie die Kostenprognose für Azure-VMs im Detail sehen möchten, wählen Sie in der Liste **Filter** **Azure/VM**.

![Vorhergesagte Kosten für Azure-VM im aktuellen Monat](./media/tutorial-forecast-spending/project-month02.png)

Führen Sie die gleichen grundlegenden Schritte aus, um die Prognose der monatlichen Kosten für andere Dienste anzuzeigen, die Sie interessieren.

Der Bericht über die vorhergesagten jährlichen Kosten zeigt die extrapolierten Kosten für Ihre Dienste für die nächsten 12 Monate an.

Klicken Sie im Berichtsmenü am oberen Rand des Portals auf **Cost** > **Projection and Budget** > **Annual Projected Cost**. Die folgende Abbildung zeigt ein Beispiel.

![Bericht über die vorhergesagten jährlichen Kosten](./media/tutorial-forecast-spending/project-annual01.png)

Im Beispiel können Sie sehen, für welche Dienste die höchsten Kosten entstanden sind. Wie im monatlichen Beispiel waren die Azure-Kosten niedriger als die AWS Kosten. Wenn Sie die Kostenprognose für Azure-VMs im Detail sehen möchten, wählen Sie in der Liste **Filter** **Azure/VM**.

![Vorhergesagte jährliche VM-Kosten](./media/tutorial-forecast-spending/project-annual02.png)

In der Abbildung oben belaufen sich die vorhergesagten jährlichen Kosten der Azure-VMs auf 28.374 US-Dollar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Vorhersage zukünftiger Ausgaben


Lesen Sie in der Cloudyn-Dokumentation nach, um mehr über die ersten Schritte mit Cloudyn und die Verwendung der Features von Cloudyn zu erfahren.

> [!div class="nextstepaction"]
> [Cloudyn-Dokumentation](https://support.cloudyn.com/hc/)

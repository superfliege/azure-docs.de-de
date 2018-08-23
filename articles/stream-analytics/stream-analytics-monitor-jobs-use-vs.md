---
title: Überwachen und Verwalten von Azure Stream Analytics-Aufträgen mithilfe von Visual Studio
description: Dieser Artikel beschreibt, wie Sie Azure Stream Analytics-Aufträge mithilfe von Visual Studio überwachen und verwalten.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 9cc65fc5119bedc816f825b7aae6120f4bca3b12
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246839"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-visual-studio"></a>Überwachen und Verwalten von Stream Analytics-Aufträgen mithilfe von Visual Studio

Dieser Artikel erläutert, wie Sie Ihre Stream Analytics-Aufträge in Visual Studio überwachen können. Die Überwachung mit Azure Stream Analytics-Tools für Visual Studio ähnelt der Überwachung im Azure-Portal, nur dass Sie die IDE nicht verlassen müssen. Sie können einen Auftrag überwachen, wenn Sie aus Ihrer **Script.asaql**-Datei **An Azure übermitteln** auswählen. Vorhandene Aufträge lassen sich unabhängig davon, wie sie erstellt wurden, überwachen. 

## <a name="job-summary"></a>Auftragszusammenfassung

Die **Auftragszusammenfassung** und **Auftragsmetriken** bieten einen schnellen Überblick über den Auftrag. Sie können auf einen Blick die Status- und Auftragsinformationen ermitteln.

<img src="./media/stream-analytics-monitor-jobs-use-vs/stream-analytics-job-summary-metrics.png" alt="Stream Analytics job summary and job metrics" width="300px"/> 


## <a name="job-metrics"></a>Auftragsmetriken

Sie können die **Auftragszusammenfassung** ausblenden und auf die Registerkarte **Auftragsmetriken** klicken, um ein Diagramm mit wichtigen Metriken anzuzeigen. Aktivieren und deaktivieren Sie Metriktypen, um sie dem Diagramm hinzuzufügen oder daraus zu entfernen.

![Stream Analytics-Metrik](./media/stream-analytics-monitor-jobs-use-vs/stream-analytics-vs-metrics.png)


## <a name="error-monitoring"></a>Fehlerüberwachung

Sie können auch Fehler überwachen, indem Sie auf die Registerkarte **Fehler** klicken.

![Stream Analytics-Fehler](./media/stream-analytics-monitor-jobs-use-vs/stream-analytics-vs-errors.png)


## <a name="get-support"></a>Support
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erstellen eines Azure Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md)
* [Installieren der Azure Stream Analytics-Tools für Visual Studio](stream-analytics-tools-for-visual-studio-install.md) 



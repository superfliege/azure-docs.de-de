---
title: Starten eines Azure Stream Analytics-Auftrags
description: Dieser Artikel beschreibt das Starten eines Stream Analytics-Auftrags.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: fb1d724907c09e2eb77930f5a235336ca8cd3a25
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57886846"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Starten eines Azure Stream Analytics-Auftrags

Sie können Ihren Azure Stream Analytics-Auftrag über das Azure-Portal, Visual Studio oder PowerShell starten. Wenn Sie einen Auftrag starten, wählen Sie eine Uhrzeit aus, zu der der Auftrag mit dem Erstellen der Ausgabe beginnen soll. Azure-Portal, Visual Studio und PowerShell weisen jeweils unterschiedliche Methoden zum Festlegen der Startzeit auf. Diese Methoden werden nachstehend beschrieben.

## <a name="azure-portal"></a>Azure-Portal

Navigieren Sie im Azure-Portal zu Ihrem Auftrag, und wählen Sie auf der Übersichtsseite **Starten** aus. Wählen Sie eine **Startzeit der Auftragsausgabe** und anschließend **Starten** aus.

Es gibt drei Optionen für **Startzeit der Auftragsausgabe**: *Jetzt*, *Benutzerdefiniert* und *Zeitpunkt der letzten Beendigung*. Durch Auswählen von *Jetzt* wird der Auftrag zum aktuellen Zeitpunkt gestartet. Durch Auswählen von *Benutzerdefiniert* können Sie eine benutzerdefinierte Uhrzeit in der Vergangenheit oder in der Zukunft festlegen, zu der der Auftrag beginnen soll. Um einen angehaltenen Auftrag ohne Datenverlust fortzusetzen, wählen Sie *Zeitpunkt der letzten Beendigung* aus.

## <a name="visual-studio"></a>Visual Studio

Wählen Sie in der Auftragsansicht die Schaltfläche mit dem grünen Pfeil aus, um den Auftrag zu starten. Legen Sie **Startmodus der Auftragsausgabe** fest, und wählen Sie **Start** aus. Der Auftragsstatus wird in **Wird ausgeführt** geändert.

Es gibt drei Optionen für **Startmodus der Auftragsausgabe**: *JobStartTime*, *CustomTime* und *LastOutputEventTime*. Wenn diese Eigenschaft nicht angegeben wird, lautet der Standardwert *JobStartTime*.

*JobStartTime* legt den Startzeitpunkt des Ausgabeereignisstreams als den Zeitpunkt des Auftragsstarts fest.

*CustomTime* beginnt die Ausgabe zu einem benutzerdefinierten Zeitpunkt, der im Parameter *OutputStartTime* angegeben ist.

*LastOutputEventTime* legt den Startzeitpunkt des Ausgabeereignisstreams als den Zeitpunkt der letzten Ereignisausgabe fest.

## <a name="powershell"></a>PowerShell

Verwenden Sie das folgende Cmdlet, um Ihren Auftrag mithilfe von PowerShell zu starten:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Es gibt drei Optionen für **OutputStartMode**: *JobStartTime*, *CustomTime* und *LastOutputEventTime*. Wenn diese Eigenschaft nicht angegeben wird, lautet der Standardwert *JobStartTime*.

*JobStartTime* legt den Startzeitpunkt des Ausgabeereignisstreams als den Zeitpunkt des Auftragsstarts fest.

*CustomTime* beginnt die Ausgabe zu einem benutzerdefinierten Zeitpunkt, der im Parameter *OutputStartTime* angegeben ist.

*LastOutputEventTime* legt den Startzeitpunkt des Ausgabeereignisstreams als den Zeitpunkt der letzten Ereignisausgabe fest.

Weitere Informationen zum Cmdlet `Start-AzStreamAnalyitcsJob` finden Sie in der [Referenz zu Start-AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob).

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mit Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure Stream Analytics-Tools für Visual Studio](stream-analytics-quick-create-vs.md)

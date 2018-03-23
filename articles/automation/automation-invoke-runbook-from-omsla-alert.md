---
title: Aufrufen eines Azure Automation-Runbooks über eine Log Analytics-Warnung
description: In diesem Artikel erfahren Sie, wie Sie ein Automation-Runbook über eine Log Analytics-Warnung in Operations Management Suite aufrufen.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.openlocfilehash: d7e3bc262fa20a2628ad25502a3d1819996a18f7
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="call-an-azure-automation-runbook-from-a-log-analytics-alert"></a>Aufrufen eines Azure Automation-Runbooks über eine Log Analytics-Warnung

Sie können eine Warnung in Azure Log Analytics konfigurieren, um einen Warnungsdatensatz zu erstellen, wenn Ergebnisse Ihren Kriterien entsprechen. Die Warnung kann dann automatisch ein Azure Automation-Runbook ausführen, um zu versuchen, das Problem automatisch zu beheben. 

Eine Warnung kann beispielsweise auf eine längere Prozessorauslastungsspitze hinweisen. Oder sie kann ausgelöst werden, wenn für einen Anwendungsprozess, der für die Funktionalität einer Geschäftsanwendung erforderlich ist, ein Fehler auftritt. Daraufhin kann ein Runbook ein entsprechendes Ereignis in das Windows-Ereignisprotokoll schreiben.  

Runbooks können in der Warnungskonfiguration auf zwei Arten aufgerufen werden:

* Verwenden eines Webhooks
   * Falls Ihr Operations Management Suite-Arbeitsbereich nicht mit einem Automation-Konto verknüpft ist, ist nur diese Option verfügbar.
   * Falls Sie bereits ein Automation-Konto mit einem Operations Management Suite-Arbeitsbereich verknüpft haben, steht diese Option auch weiterhin zur Verfügung.  

* Direktes Auswählen eines Runbooks
   * Diese Option ist nur verfügbar, wenn der Operations Management Suite-Arbeitsbereich mit einem Automation-Konto verknüpft ist.

## <a name="calling-a-runbook-by-using-a-webhook"></a>Aufrufen eines Runbooks unter Verwendung eines Webhooks

Mit einem Webhook können Sie ein bestimmtes Runbook in Azure Automation über eine einfache HTTP-Anforderung starten. Bevor Sie die [Log Analytics-Warnung](../log-analytics/log-analytics-alerts.md#alert-rules) so konfigurieren, dass sie das Runbook unter Verwendung eines Webhooks aufruft, müssen Sie für das Runbook, das mit dieser Methode aufgerufen werden soll, [einen Webhook erstellen](automation-webhooks.md#creating-a-webhook). Notieren Sie sich die Webhook-URL, damit Sie sie später beim Konfigurieren der Warnungsregel zur Hand haben.   

## <a name="calling-a-runbook-directly"></a>Direktes Aufrufen eines Runbooks

Sie können das Angebot „Automation & Control“ in Ihrem Operations Management Suite-Arbeitsbereich installieren und konfigurieren. Beim Konfigurieren der Runbook-Aktionen für die Warnung können Sie in der Dropdownliste **Runbook auswählen** alle Runbooks anzeigen und das spezifische Runbook auswählen, das als Reaktion auf die Warnung ausgeführt werden soll. Das ausgewählte Runbook kann in einem Azure-Arbeitsbereich oder für einen Hybrid Runbook Worker ausgeführt werden. 

Nachdem Sie die Warnung unter Verwendung der Runbookoption erstellt haben, wird für das Runbook ein Webhook erstellt. Den Webhook können Sie anzeigen, indem Sie unter dem Automation-Konto den Webhook-Bereich des ausgewählten Runbooks öffnen. 

Wenn Sie die Warnung löschen, bleibt der Webhook erhalten. Das ist jedoch kein Problem. Der Webhook wird lediglich zu einem verwaisten Element, das bei Gelegenheit manuell gelöscht werden sollte, um das Automation-Konto in Ordnung zu halten.  

## <a name="characteristics-of-a-runbook"></a>Merkmale eines Runbooks

Beide Methoden zum Aufrufen des Runbooks über die Log Analytics-Warnung besitzen Merkmale, mit denen Sie vor dem Konfigurieren Ihrer Warnungsregeln vertraut sein müssen. 

Die Warnungsdaten werden im JSON-Format in einer einzelnen Eigenschaft namens **SearchResult** angegeben. Dieses Format gilt für Runbook- und Webhookaktionen mit einer Standardnutzlast. Für Webhookaktionen mit benutzerdefinierten Nutzlasten (einschließlich **IncludeSearchResults:True** in **RequestBody**) lautet die Eigenschaft **SearchResults**.

Sie benötigen einen Runbook-Eingabeparameter namens **WebhookData** vom Typ **Objekt**. Dieser kann obligatorisch oder optional sein. Mithilfe dieses Eingabeparameters übergibt die Warnung die Suchergebnisse an das Runbook.

```powershell
param  
    (  
    [Parameter (Mandatory=$true)]  
    [object] $WebhookData  
    )
```
Außerdem benötigen Sie Code, um **WebhookData** in ein PowerShell-Objekt zu konvertieren.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
```

**$SearchResult** ist ein Array von Objekten. Jedes Objekt enthält die Felder mit Werten eines einzelnen Suchergebnisses.


## <a name="example-walkthrough"></a>Exemplarische Vorgehensweise

Das folgende Beispiel veranschaulicht die Funktionsweise dieses Prozesses anhand eines grafischen Runbooks. Dabei wird ein Windows-Dienst gestartet.

![Grafisches Runbook zum Starten eines Windows-Diensts](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)

Das Runbook besitzt einen Eingabeparameter vom Typ **Objekt** namens **WebhookData**. Es enthält die von der Warnung mit **SearchResult** übergebenen Webhookdaten.

![Runbookeingabeparameter](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)

In diesem Beispiel wurden zwei benutzerdefinierte Felder in Log Analytics erstellt: **SvcDisplayName_CF** und **SvcState_CF**. Diese Felder extrahieren den Anzeigenamen und den Zustand des Diensts („Ausgeführt“ oder „Beendet“) aus dem Ereignis, das in das Systemereignisprotokoll geschrieben wird. Anschließend wurde eine Warnungsregel mit der folgenden Suchabfrage erstellt, um zu erkennen, wenn der Druckspoolerdienst im Windows-System beendet wird:

`Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` 

Sie können einen beliebigen Dienst verwenden, der für Sie von Interesse ist. In diesem Beispiel verweisen wir auf einen der Dienste, die bereits im Windows-Betriebssystem enthalten sind. Die Warnungsaktion ist so konfiguriert, dass das in diesem Beispiel verwendete Runbook für den Hybrid Runbook Worker ausgeführt wird, der auf dem Zielsystem aktiviert ist.   

Die Runbook-Codeaktivität **Get Service Name from LA** (Dienstname aus LA abrufen) konvertiert die JSON-Zeichenfolge in einen Objekttyp und filtert nach dem Element **SvcDisplayName_CF**. Sie extrahiert den Anzeigenamen des Windows-Diensts und übergibt den Wert an die nächste Aktivität. Diese überprüft, ob der Dienst beendet wurde, bevor sie versucht, ihn neu zu starten. **SvcDisplayName_CF** ist ein [benutzerdefiniertes Feld](../log-analytics/log-analytics-custom-fields.md), das in Log Analytics für dieses Beispiel erstellt wurde.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

Wenn der Dienst beendet wird, erkennt die Warnungsregel in Log Analytics eine Übereinstimmung, löst das Runbook aus und sendet den Warnungskontext an das Runbook. Das Runbook überprüft, ob der Dienst beendet wurde. Ist dies der Fall, versucht das Runbook, den Dienst neu zu starten. Anschließend überprüft es, ob der Dienst ordnungsgemäß gestartet wurde, und zeigt die Ergebnisse an.     

Alternativ können Sie die Warnungsregel mit einer Webhookaktion konfigurieren, wenn Sie Ihr Automation-Konto nicht mit Ihrem Operations Management Suite-Arbeitsbereich verknüpft haben. Die Webhookaktion löst das Runbook aus. Außerdem konfiguriert sie das Runbook so, dass es die JSON-Zeichenfolge konvertiert und nach **SearchResult** filtert (wie weiter oben beschrieben).    

>[!NOTE]
> Wenn für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](../log-analytics/log-analytics-log-search-upgrade.md) durchgeführt wurde, hat sich die Webhooknutzlast geändert. Details des Formats werden unter [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Warnungen in Log Analytics und zu deren Erstellung finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md).

* Informationen zum Auslösen von Runbooks mithilfe eines Webhooks finden Sie unter [Starten eines Azure Automation-Runbooks mit einem Webhook](automation-webhooks.md).

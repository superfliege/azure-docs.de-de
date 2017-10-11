---
title: Azure Automation-Runbook-Aktion im Protokollanalyse Benutzerinitiierte | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie ein Automation-Runbook von einem Protokollanalyse Suche Ergebnis bei Bedarf ausgeführt wird."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: ff938697add98f3d21b4971175432335ee2e39ba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Führen Sie die Aktion mit ein Automation-Runbook von Protokollanalyse Protokoll Suchergebnis

In ein Protokoll-Suchergebnis in Azure Log Analytics, können Sie jetzt auswählen **Maßnahmen** ein Automation-Runbook ausgeführt.  Das Runbook kann verwendet werden, um das Problem zu beheben oder eine andere Aktion Sammeln von Informationen zur Problembehandlung, zum Senden einer e-Mail oder einen Service Request erstellen. 

## <a name="components-and-features-used"></a>Komponenten und features
* [Azure Automation-Konto](../automation/automation-offering-get-started.md)
* [Protokollanalyse-Arbeitsbereich](../log-analytics/log-analytics-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>So initiieren Sie Runbook protokollsuche

Um die Aktion für ein Ereignis aus, und initiieren ein Runbook aus Ihrer protokollsuchergebnissen, erstellen Sie zunächst eine protokollsuche und aus den Ergebnissen können Sie ein Runbook bei Bedarf aufrufen.  Dies kann erreicht werden, aus der Protokoll-Suchfunktion in Azure oder [OMS-Portal](../log-analytics/log-analytics-log-searches.md).  In diesem Beispiel führen Sie eine Suche Protokolldatei vom Azure-Portal mit einer einfachen Demonstration von dieser Funktion.

1. Klicken Sie im Azure-Portal im Hubmenü auf **Weitere Dienste** , und wählen Sie **Protokollanalyse**.  
2. Auf dem Blatt "Protokollanalysen" Wählen Sie den Protokollanalyse-Arbeitsbereich, und wählen Sie auf dem Blatt "Arbeitsbereich" **Protokollsuche**.  
3. Führen Sie auf dem Blatt "Protokollsuche" eine protokollsuche ein.  
4. In den protokollsuchergebnissen klicken Sie auf die Ellipse auf der linken Seite eines der Felder und über das Popup in select **entsprechende Maßnahmen ergreifen**.<br><br> ![Wählen Sie die Aktion ausführen aus Suchergebnis](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. Wählen Sie aus dem Blade Take-Aktion **Ausführung eines Runbooks**, und klicken Sie auf die **Ausführung eines Runbooks** Blatt können Sie zur Ausführung ein Runbooks auswählen.  Sie können jedes Runbook in Automation-Konto auswählen, die mit der Protokollanalyse-Arbeitsbereich verknüpft ist.  . Beachten Sie dabei Folgendes:

    * Runbooks werden nach Kategorien organisiert.
    * Die Werte der Runbook-Eingabeparameter können direkt aus den Feldern der das Suchergebnis ausgewählt werden.  Eine Dropdownliste wird angezeigt, Anzeigen der verfügbaren Felder aus dem Ergebnis aus.  
    * Sie können auswählen, um das Runbook ausgeführt werden, auf eine [Hybriden runbookworker](../automation/automation-hybrid-runbook-worker.md) , dass Sie auf dem Computer, die das Problem aufweist installiert haben, wenn Sie eine entsprechende Hybrid Runbook Worker-Gruppe verfügen, die nur auf den Computer als Mitglied enthält.  Wenn der Name der Hybrid Worker-Gruppe den Namen des Computers, die in das Protokoll Suchergebnis ist übereinstimmt, wird die Gruppe automatisch ausgewählt.    

6. Nachdem Sie auf **ausführen**, das Blatt für die Runbook-Auftrag wird geöffnet, um Sie so überprüfen den Status des Auftrags zu ermöglichen.   

Wenn Sie ein Runbook auswählen, die konfiguriert werden [von eine Warnung für die Protokollanalyse aufgerufen](../automation/automation-invoke-runbook-from-omsla-alert.md), Eingabeparameter aufgerufen werden kann **WebhookData** also **Objekt** Typ.  Wenn der input-Parameter obligatorisch ist, müssen Sie die Suchergebnisse an das Runbook übergeben, damit sie die JSON-formatierte Zeichenfolge in einen Objekttyp, sodass Sie für bestimmte Elemente zu filtern, die Sie im Runbook-Aktivitäten verweisen, konvertieren kann.  Sie dazu **Suchergebnis (Objekt)** aus der Dropdown-Liste.<br><br> ![Wählen Sie die Webhook-Datenobjekt für den Runbook-parameter](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Nächste Schritte

* Überprüfen Sie die [Protokollanalyse protokollieren Suche Verweis](log-analytics-search-reference.md) Anzeigen aller den Suchfeldern und die Facets in Protokollanalyse verfügbar.
* Überprüfen Sie zum Erlernen der automatisch ein Automation-Runbook Aufrufen [Aufrufen von Azure Automation-Runbook aus einem OMS-Protokollanalyse-Warnung](../automation/automation-invoke-runbook-from-omsla-alert.md).  
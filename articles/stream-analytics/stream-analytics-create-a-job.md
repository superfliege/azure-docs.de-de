---
title: "Erstellen eines Auftrags zur Verarbeitung von Datenanalysen für Stream Analytics | Microsoft Docs"
description: "Erstellen eines Auftrags zur Verarbeitung von Datenanalysen für Stream Analytics | Lernpfadsegment."
keywords: Verarbeitung von Datenanalysen
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 98784783beccc19df916920fc41364a23e6bae11
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Erstellen eines Auftrags zur Verarbeitung von Datenanalysen für Stream Analytics
Die Ressource der obersten Ebene in Azure Stream Analytics ist ein Stream Analytics-Auftrag.  Er besteht aus einer oder mehreren Eingabedatenquellen, einer Abfrage zum Ausdrücken der Datentransformation und mindestens einem Ausgabeziel, in das Ergebnisse geschrieben werden. Dadurch kann der Benutzer die Verarbeitung von Datenanalysen für Streamingdatenszenarien durchführen.

Erstellen Sie zuerst einen neuen Stream Analytics-Auftrag, um mit der Verwendung von Stream Analytics zu beginnen.  Beachten Sie, dass diese Aktion keine Auswirkungen auf die Abrechnung hat, bis der Auftrag gestartet wird.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Ressource erstellen** > **Daten + Analysen** > **Stream Analytics-Auftrag** aus.
3. Klicken Sie auf **Erstellen**.
   
3. Geben Sie die gewünschte Konfiguration für den Stream Analytics-Auftrag an.
   
   * Geben Sie im Feld **Auftragsname** einen Namen zur Identifizierung des Stream Analytics-Auftrags ein. Wenn der **Auftragsname** überprüft wurde, wird im Auftragsnamenfeld ein grünes Häkchen angezeigt. Der **Auftragsname** darf nur alphanumerische Zeichen und den Bindestrich „-“ enthalten und muss zwischen 3 und 63 Zeichen lang sein.
   * Unter **Standort** können Sie einen geografischen Standort für die Ausführung des Auftrags angeben.
   * Geben Sie eine neue oder vorhandene **Ressourcengruppe** an, um verwandte Ressourcen für Ihre Anwendung zu speichern.
4. Klicken Sie auf **Erstellen**.
Die Erstellung des Stream Analytics-Auftrags kann einige Minuten dauern. Sie können den Fortschritt im Benachrichtigungshub überwachen, um den Status zu überprüfen.
    
   ![Azure-Portal – Auftrag zur Verarbeitung von Datenanalysen – Auftrag erstellen](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Für den neuen Auftrag wird der Status **Erstellt** angezeigt. Beachten Sie, dass die Schaltfläche **Start** deaktiviert ist. Vor dem Starten des Auftrags müssen Eingabe, Abfrage und Ausgabe des Auftrags konfiguriert werden.

   
   ![Azure-Portal – Auftrag zur Verarbeitung von Datenanalysen – Status](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


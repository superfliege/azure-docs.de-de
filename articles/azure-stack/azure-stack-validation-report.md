---
title: Überprüfungsbericht für Azure Stack | Microsoft-Dokumentation
description: Verwenden Sie den Azure Stack Readiness Checker-Bericht für die Durchsicht von Überprüfungsergebnissen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a0ca0ae3ed615f6bc2774364f7a443023b911b5d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937554"
---
# <a name="azure-stack-validation-report"></a>Azure Stack-Überprüfungsbericht
Das Azure Stack Readiness Checker-Tool führt Überprüfungen durch, die die Bereitstellung und Wartung einer Azure Stack-Umgebung unterstützen. Das Tool schreibt die Überprüfungsergebnisse in eine JSON-Berichtsdatei. Der Bericht zeigt detaillierte und zusammengefasste Daten über den Stand der Voraussetzungen für die Bereitstellung von Azure Stack und über Secrets Rotation für bestehende Azure Stack-Bereitstellungen.  

 ## <a name="where-to-find-the-report"></a>Hier finden Sie den Bericht
Wenn das Tool ausgeführt wird, protokolliert es die Ergebnisse in **AzsReadinessCheckerReport.json**. Das Tool erstellt auch ein Protokoll namens **AzsReadinessChecker.log**. Der Speicherort dieser Dateien wird mit den Überprüfungsergebnissen in PowerShell angezeigt.

![Ausführen der Überprüfung](./media/azure-stack-validation-report/validation.png)

Beide Dateien behalten die Ergebnisse der nachfolgenden Durchsicht der Überprüfung bei, wenn sie auf demselben Computer ausgeführt werden.  Beispielsweise kann das Tool ausgeführt werden, um Zertifikate zu überprüfen, erneut ausgeführt werden, um die Azure-Identität zu überprüfen, und dann ein drittes Mal, um die Registrierung zu überprüfen. Die Ergebnisse aller drei Überprüfungen sind im sich daraus ergebenden JSON-Bericht verfügbar.  

Standardmäßig werden beide Dateien in *C:\Users\<Benutzername>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json* geschrieben.  
- Verwenden Sie den Parameter **-OutputPath** ***&lt;Pfad&gt;*** am Ende der Befehlszeile, um eine andere Berichtsposition anzugeben.   
- Verwenden Sie den Parameter **-CleanReport** am Ende der Befehlszeile, um Informationen von früheren Ausführungen des Tools aus *AzsReadinessCheckerReport.json* zu löschen.

## <a name="view-the-report"></a>Anzeigen des Berichts
Um den Bericht in PowerShell anzuzeigen, geben Sie den Pfad zum Bericht als Wert für **-ReportPath** an. Dieser Befehl zeigt den Inhalt des Berichts an und identifiziert auch Überprüfungen, für die noch keine Ergebnisse vorhanden sind.

Um den Bericht beispielsweise von einer PowerShell-Eingabeaufforderung aus anzuzeigen, die für den Speicherort geöffnet ist, an dem sich der Bericht befindet, führen Sie Folgendes aus: 
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

Die Ausgabe ähnelt der folgenden Abbildung:

![Anzeigen des Berichts](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Anzeigen der Berichtzusammenfassung
Um eine Zusammenfassung des Berichts anzuzeigen, können Sie **-Summary** am Ende der PowerShell-Befehlszeile hinzufügen. Beispiel:  
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

Die Zusammenfassung zeigt Überprüfungen, für die keine Ergebnisse vorhanden sind, und gibt an, ob die abgeschlossenen Überprüfungen bestanden oder nicht bestanden wurden. Die Ausgabe ähnelt der folgenden Abbildung:

![Zusammenfassung des Berichts](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Anzeigen eines gefilterten Berichts
Um einen Bericht anzuzeigen, der für einen einzelnen Überprüfungstyp gefiltert wurde, verwenden Sie den Parameter **-ReportSections** und geben Sie einen der folgenden Werte an, die dem Überprüfungstyp entsprechen, den Sie anzeigen möchten:
- Zertifikat
- AzureRegistration
- AzureIdentity
- Aufträge   
- Alle  

Um beispielsweise die Berichtzusammenfassung nur für Zertifikate anzuzeigen, verwenden Sie die folgende PowerShell-Befehlszeile: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Weitere Informationen
[Referenz zum Cmdlet „Start-AzsReadinessChecker“](azure-stack-azsreadiness-cmdlet.md)

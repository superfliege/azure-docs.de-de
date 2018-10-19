---
title: 'Microsoft Genomics: Handbuch zur Problembehandlung'
titleSuffix: Azure
description: In diesem Artikel erfahren Sie mehr über Strategien für die Problembehandlung.
keywords: troubleshooting, error, debugging
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 07/18/2018
ms.openlocfilehash: bd946f84023345c68a01a48a4dc310b7afb68397
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735385"
---
# <a name="troubleshooting-guide-for-microsoft-genomics"></a>Handbuch zur Problembehandlung für Microsoft Genomics
In dieser Übersicht werden Strategien beschrieben, um häufige Probleme bei der Verwendung des Diensts Microsoft Genomics zu behandeln. Allgemeine häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>Wie überprüfe ich meinen Auftragsstatus?
Sie können den Status Ihres Workflows überprüfen, indem Sie `msgen status` wie dargestellt über die Befehlszeile aufrufen. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Es gibt drei erforderliche Argumente:
* URL: Der Basis-URI für die API
* KEY: Der Zugriffsschlüssel für Ihr Genomics-Konto 
* ID: Die Workflow-ID

Wechseln Sie für die Suche nach URL und KEY zum Azure-Portal, und öffnen Sie die Seite Ihres Genomics-Kontos. Klicken Sie unter der Überschrift **Verwaltung** auf **Zugriffsschlüssel**. Dort finden Sie die API-URL und Ihre Zugriffsschlüssel.

Alternativ können Sie den Pfad zur Konfigurationsdatei einschließen, statt URL und KEY direkt einzugeben. Hinweis: Wenn Sie diese Argumente in der Befehlszeile sowie die Konfigurationsdatei einschließen, haben die Befehlszeilenargumente Vorrang. 

Nach dem Aufruf von `msgen status` wird eine Benutzermeldung angezeigt, die angibt, dass der Workflow erfolgreich war, oder ggf. den Grund für eine fehlerhafte Ausführung des Auftrags. 


## <a name="get-more-information-about-my-workflow-status"></a>Abrufen weiterer Informationen zum Workflowstatus

Um weitere Informationen darüber zu erhalten, warum ein Auftrag nicht erfolgreich ausgeführt wurde, können Sie die während des Workflows erzeugten Protokolldateien heranziehen. In Ihrem Ausgabecontainer sollte ein Ordner namens `[youroutputfilename].logs.zip` angezeigt werden.  Nachdem Sie diesen Ordner entzippt haben, sehen Sie die folgenden Elemente:

* outputFileList.txt: Eine Liste der während des Workflows erstellten Ausgabedateien
* standarderror.txt: Diese Datei ist leer.
* standardoutput.txt: Enthält die Protokollierung der obersten Ebene des Workflows. 
* GATK-Protokolldateien: Alle anderen Dateien im Ordner `logs`

Die Datei `standardoutput.txt` ist ein guter Ausgangspunkt, um herauszufinden, warum Ihr Workflow nicht erfolgreich ausgeführt wurde, da diese weitere systemnahe Informationen über den Workflow enthält. 

## <a name="common-issues-and-how-to-resolve-them"></a>Häufige Probleme und deren Behebung
In diesem Abschnitt werden kurz häufig auftretende Probleme und deren Behebung erläutert.

### <a name="fastq-files-are-unmatched"></a>FASTQ-Dateien stimmen nicht überein.
FASTQ-Dateien sollten sich nur durch das nachfolgende /1 oder/2 im Beispielbezeichner unterscheiden. Wenn Sie versehentlich nicht übereinstimmende FASTQ-Dateien übermittelt haben, werden beim Aufrufen von `msgen status` möglicherweise die folgenden Fehlermeldungen angezeigt.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

Um dies zu beheben, überprüfen Sie, ob die an den Workflow übermittelten FASTQ-Dateien tatsächlich eine zusammengehörige Gruppe darstellen. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>Fehler beim Hochladen der BAM-Datei: Ausgabeblob ist bereits vorhanden, und die Option zum Überschreiben wurde auf „False“ festgelegt.
Wenn die Fehlermeldung `Error uploading .bam file. Output blob already exists and the overwrite option was set to False` angezeigt wird, enthält der Ausgabeordner bereits eine Ausgabedatei mit demselben Namen.  Löschen Sie die vorhandene Ausgabedatei, oder aktivieren Sie in der Konfigurationsdatei die Option zum Überschreiben. Senden Sie den Workflow dann erneut.

### <a name="when-to-contact-microsoft-genomics-support"></a>Es muss Kontakt mit dem Microsoft Genomics-Support aufgenommen werden.
Wenn die folgenden Fehlermeldungen angezeigt werden, ist ein interner Fehler aufgetreten. 

* `Error locating input files on worker machine`
* `Process management failure`

Senden Sie den Workflow erneut. Wenn bei der Auftragsausführung weiterhin Fehler auftreten oder wenn Sie weitere Fragen haben, wenden Sie sich über das Azure-Portal an den Microsoft Genomics-Support. Weitere Informationen zum Übermitteln einer Supportanfrage finden Sie [hier](file-support-ticket-genomics.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie häufige Probleme mit dem Microsoft Genomics-Dienst behandeln und lösen. Weitere Informationen und allgemeinere häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen](frequently-asked-questions-genomics.md). 

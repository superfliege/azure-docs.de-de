---
title: Verwenden von kognitiven U-SQL-Funktionen in Azure Data Lake Analytics | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Intelligenz kognitiver Funktionen in U-SQL einsetzen.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: 019c1d53-4e61-4cad-9b2c-7a60307cbe19
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: ec48a07af0aba78f2e508bad232f34102f0c2073
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="tutorial-get-started-with-the-cognitive-capabilities-of-u-sql"></a>Tutorial: Erste Schritte mit den kognitiven Funktionen von U-SQL

## <a name="overview"></a>Übersicht
Kognitive Funktionen für U-SQL ermöglichen Entwicklern den Einsatz von Intelligenz in ihren Big Data-Programmen. 

Die folgenden kognitiven Funktionen stehen zur Verfügung:
* Bildverarbeitung: Gesichtserkennung
* Bildverarbeitung: Gefühlserkennung
* Bildverarbeitung: Objekterkennung (Markieren)
* Bildverarbeitung: OCR (Optical Character Recognition; optische Zeichenerkennung)
* Text: Schlüsselwortextraktion
* Text: Stimmungsanalyse

## <a name="how-to-use-cognitive-in-your-u-sql-script"></a>So verwenden Sie kognitive Funktionen in Ihrem U-SQL-Skript

Der Gesamtprozess ist einfach:

* Verwenden der REFERENCE ASSEMBLY-Anweisung zum Aktivieren der kognitiven Funktionen für das U-SQL-Skript
* Verwenden Sie die PROCESS-Anweisung für ein Eingaberowset mit einem kognitiven benutzerdefinierten Objekt (UDO), um ein Ausgaberowset zu erstellen.

### <a name="detecting-objects-in-images"></a>Erkennen von Objekten in Bildern

Im folgenden Beispiel wird veranschaulicht, wie Sie kognitive Funktionen verwenden, um Objekte in Bildern zu erkennen.

```
REFERENCE ASSEMBLY ImageCommon;
REFERENCE ASSEMBLY FaceSdk;
REFERENCE ASSEMBLY ImageEmotion;
REFERENCE ASSEMBLY ImageTagging;
REFERENCE ASSEMBLY ImageOcr;

// Get the image data

@imgs =
    EXTRACT 
        FileName string, 
        ImgData byte[]
    FROM @"/usqlext/samples/cognition/{FileName}.jpg"
    USING new Cognition.Vision.ImageExtractor();

//  Extract the number of objects on each image and tag them 

@tags =
    PROCESS @imgs 
    PRODUCE FileName,
            NumObjects int,
            Tags SQL.MAP<string, float?>
    READONLY FileName
    USING new Cognition.Vision.ImageTagger();

@tags_serialized =
    SELECT FileName,
           NumObjects,
           String.Join(";", Tags.Select(x => String.Format("{0}:{1}", x.Key, x.Value))) AS TagsString
    FROM @tags;

OUTPUT @tags_serialized
    TO "/tags.csv"
    USING Outputters.Csv();
```
Schauen Sie sich z.B. die **U-SQL-/Kognitiven Beispiele** im Abschnitt **Nächste Schritte** an.

## <a name="next-steps"></a>Nächste Schritte
* [Beispiele für U-SQL/Kognitive Beispiele](https://github.com/Azure-Samples?utf8=✓&q=usql%20cognitive)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden von U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)

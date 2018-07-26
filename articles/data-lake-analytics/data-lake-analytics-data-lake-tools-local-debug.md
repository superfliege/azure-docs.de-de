---
title: Lokales Debuggen von Azure Data Lake Analytics-Code | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie U-SQL-Aufträge mithilfe von Azure Data Lake Tools für Visual Studio auf der lokalen Arbeitsstation debuggen.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: 181512c12c1e72e6aa8205aabd5ea22816d4c5df
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37890812"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Lokales Debuggen von Azure Data Lake Analytics-Code

Sie können Azure Data Lake Tools für Visual Studio zum Ausführen und Debuggen von Azure Data Lake Analytics-Code auf der Arbeitsstation verwenden, so wie dies auch im Azure Data Lake-Dienst möglich ist.

Erfahren Sie mehr zum [Ausführen von U-SQL-Skripts auf dem lokalen Computer](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Lokales Debuggen von Skripts und C#-Assemblys

Sie können C#-Assemblys debuggen, ohne sie zu senden und beim Azure Data Lake Analytics-Dienst zu registrieren. Sie können sowohl in der Code-Behind-Datei als auch in einem referenzierten C#-Projekt Haltepunkte festlegen.

### <a name="to-debug-local-code-in-code-behind-file"></a>So debuggen Sie lokalen Code in der CodeBehind-Datei:

1. Legen Sie Haltepunkte in der CodeBehind-Datei fest.
2. Drücken Sie F5, um das Skript lokal zu debuggen.

> [!NOTE]
   > Das folgende Verfahren funktioniert nur in Visual Studio 2015. In älteren Versionen von Visual Studio müssen Sie eventuell die PDB-Dateien manuell hinzufügen.  
   >
   >

### <a name="to-debug-local-code-in-a-referenced-c-project"></a>So debuggen Sie lokalen Code in einem referenzierten C#-Projekt:

1. Erstellen Sie ein C#-Assemblyprojekt, und erstellen Sie es, um die Ausgabe-DLL zu generieren.
2. Registrieren Sie die DLL mithilfe einer U-SQL-Anweisung:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Legen Sie Haltepunkte im C#-Code fest.
4. Drücken Sie F5, um das Skript mit Verweis auf die C#-DLL lokal zu debuggen.


## <a name="next-steps"></a>Nächste Schritte

- Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Informationen zum Anzeigen von Auftragsdetails finden Sie unter [Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge](data-lake-analytics-data-lake-tools-view-jobs.md).
- Weitere Informationen zur Verwendung der Scheitelpunktausführungsansicht finden Sie unter [Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

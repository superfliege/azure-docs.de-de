---
title: Inkrementelles Kopieren von Daten mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: In diesen Tutorials wird veranschaulicht, wie Sie Daten inkrementell aus einem Quelldatenspeicher in einen Zieldatenspeicher kopieren. Im ersten Tutorial werden Daten aus einer Tabelle kopiert.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: yexu
ms.openlocfilehash: cb75e943416c227730589ab5e7feeb7b8ba5e245
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957924"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Inkrementelles Laden von Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher

In einer Datenintegrationslösung ist das inkrementelle Laden (oder Deltaladen) von Daten nach einem anfänglichen vollständigen Ladevorgang ein häufig verwendetes Szenario. In den Tutorials dieses Abschnitts werden verschiedene Möglichkeiten zum inkrementellen Laden von Daten mit Azure Data Factory gezeigt.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Laden von Deltadaten aus der Datenbank mit einem Grenzwert
In diesem Fall definieren Sie einen Grenzwert (Englisch: Watermark) in Ihrer Quelldatenbank. Der Grenzwert ist hier eine Spalte, die den Zeitstempel der letzten Aktualisierung oder einen Inkrementierungsschlüssel enthält. Mit einer Lösung für das Deltaladen werden die geänderten Daten geladen, die zwischen einem alten und einem neuen Grenzwert liegen. Der Workflow für diesen Ansatz ist im folgenden Diagramm dargestellt: 

![Workflow für die Verwendung von Grenzwerten](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Die folgenden Tutorials enthalten Schritt-für-Schritt-Anleitungen: 

- [Inkrementelles Kopieren von Daten aus einer Tabelle in Azure SQL-Datenbank in Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Inkrementelles Laden von mehreren SQL Server-Tabellen in Azure SQL-Datenbank](tutorial-incremental-copy-multiple-tables-powershell.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>Laden von Deltadaten aus SQL-Datenbank unter Verwendung der Technologie für die Änderungsnachverfolgung
Die Technologie für die Änderungsnachverfolgung ist eine einfache Lösung in SQL Server und Azure SQL-Datenbank, die über einen effizienten Mechanismus für die Änderungsnachverfolgung für Anwendungen enthält. Hiermit kann eine Anwendung auf einfache Weise Daten identifizieren, die eingefügt, aktualisiert oder gelöscht wurden. 

Der Workflow für diesen Ansatz ist im folgenden Diagramm dargestellt:

![Workflow für die Verwendung der Änderungsnachverfolgung](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Das folgende Tutorial enthält eine Schritt-für-Schritt-Anleitung: <br/>
[Inkrementelles Laden von Daten aus Azure SQL-Datenbank in Azure Blob Storage mit Informationen der Änderungsnachverfolgung](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Ausschließliches Laden neuer und geänderter Dateien unter Verwendung von „LastModifiedDate“
Sie können zuerst die Metadaten (LastModifiedDate) Ihrer Dateien abrufen und anschließend nur die neuen und geänderten Dateien in den Zielspeicher kopieren.

Das folgende Tutorial enthält eine Schritt-für-Schritt-Anleitung: <br/>
[Incrementally copy new and changed files based on LastModifiedDate by using the Copy Data tool](tutorial-incremental-copy-lastmodified-copy-data-tool.md) (Inkrementelles Kopieren neuer und geänderter Dateien auf der Grundlage von „LastModifiedDate“ mithilfe des Tools zum Kopieren von Daten)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Ausschließliches Laden neuer Dateien unter Verwendung zeitpartitionierter Ordner- oder Dateinamen
Sie können den Kopiervorgang auf neue Dateien beschränken, wenn Datei- oder Ordnernamen Zeitangaben zur zeitlichen Partitionierung enthalten (Beispiel: /jjjj/mm/tt/Datei.csv). 

Das folgende Tutorial enthält eine Schritt-für-Schritt-Anleitung: <br/>
[Incrementally copy new files based on time partitioned file name by using the Copy Data tool](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md) (Inkrementelles Kopieren neuer Dateien auf der Grundlage zeitpartitionierter Dateinamen mithilfe des Tools zum Kopieren von Daten)

## <a name="next-steps"></a>Nächste Schritte
Fahren Sie mit dem folgenden Tutorial fort: 

> [!div class="nextstepaction"]
>[Inkrementelles Kopieren von Daten aus einer Tabelle in Azure SQL-Datenbank in Azure Blob Storage](tutorial-incremental-copy-powershell.md)

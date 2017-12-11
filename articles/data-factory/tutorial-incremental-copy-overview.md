---
title: Inkrementelles Kopieren von Daten mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: 'In diesen Tutorials wird veranschaulicht, wie Sie Daten inkrementell aus einem Quelldatenspeicher in einen Zieldatenspeicher kopieren. Im ersten Tutorial werden Daten aus einer Tabelle kopiert. '
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/05/2017
ms.author: shlo
ms.openlocfilehash: 2ae6cb42685dfb227bd75f83e73dfdf646ab909f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2017
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Inkrementelles Laden von Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher

In einer Datenintegrationslösung ist das inkrementelle Laden (oder Deltaladen) von Daten nach einem anfänglichen vollständigen Ladevorgang ein häufig verwendetes Szenario. In den Tutorials dieses Abschnitts werden verschiedene Möglichkeiten zum inkrementellen Laden von Daten mit Azure Data Factory Version 2 gezeigt.

## <a name="delta-data-loading-using-a-watermark"></a>Deltaladen von Daten mit „Grenzwert“
In diesem Fall definieren Sie einen Grenzwert (Englisch: Watermark) in Ihrer Quelldatenbank. Der Grenzwert ist hier eine Spalte, die den Zeitstempel der letzten Aktualisierung oder einen Inkrementierungsschlüssel enthält. Mit einer Lösung für das Deltaladen werden die geänderten Daten geladen, die zwischen einem alten und einem neuen Grenzwert liegen. Der Workflow für diesen Ansatz ist im folgenden Diagramm dargestellt: 

![Workflow für die Verwendung von Grenzwerten](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Die folgenden Tutorials enthalten Schritt-für-Schritt-Anleitungen: 

- [Inkrementelles Kopieren von Daten aus einer Tabelle in Azure SQL-Datenbank in Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Inkrementelles Kopieren von Daten aus mehreren Tabellen einer lokalen SQL Server-Instanz in eine Azure SQL-Datenbank](tutorial-incremental-copy-multiple-tables-powershell.md)


## <a name="delta-data-loading-using-the-change-tracking-technology"></a>Deltaladen von Daten per Technologie für die Änderungsnachverfolgung
Die Technologie für die Änderungsnachverfolgung ist eine einfache Lösung in SQL Server und Azure SQL-Datenbank, die über einen effizienten Mechanismus für die Änderungsnachverfolgung für Anwendungen enthält. Hiermit kann eine Anwendung auf einfache Weise Daten identifizieren, die eingefügt, aktualisiert oder gelöscht wurden. 

Der Workflow für diesen Ansatz ist im folgenden Diagramm dargestellt:

![Workflow für die Verwendung der Änderungsnachverfolgung](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Das folgende Tutorial enthält eine Schritt-für-Schritt-Anleitung: <br/>
[Inkrementelles Kopieren von Daten aus Azure SQL-Datenbank in Azure Blob Storage per Änderungsnachverfolgung](tutorial-incremental-copy-change-tracking-feature-powershell.md)


## <a name="next-steps"></a>Nächste Schritte
Fahren Sie mit dem folgenden Tutorial fort: 

> [!div class="nextstepaction"]
>[Inkrementelles Kopieren von Daten aus einer Tabelle in Azure SQL-Datenbank in Azure Blob Storage](tutorial-incremental-copy-powershell.md)

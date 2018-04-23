---
title: Tutorial mit Adventure Works von Azure Analysis Services | Microsoft-Dokumentation
description: Einführung des Tutorials mit Adventure Works für Azure Analysis Services
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3c95e755c66cfd1bc0a4e4e55bf1d05f08214925
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services – Tutorial mit Adventure Works

Dieses Tutorial enthält Lektionen zum Erstellen und Bereitstellen eines tabellarischen Modells mit dem Kompatibilitätsgrad 1400 mit Visual Studio und [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

Wenn Sie mit Analysis Services und der tabellarischen Modellierung noch nicht vertraut sind, ist dieses Tutorial die schnellste Methode, um zu erfahren, wie Sie mit Visual Studio ein einfaches tabellarisches Modell erstellen und bereitstellen. Sobald alle Voraussetzungen erfüllt sind, nimmt das Tutorial etwa zwei bis drei Stunden in Anspruch.  
  
## <a name="what-you-learn"></a>Lerninhalt   
  
-   Wie Sie ein neues Projekt für ein tabellarisches Modell mit dem **Kompatibilitätsgrad 1400** in Visual Studio mit SSDT erstellen können.
  
-   Wie Sie Daten aus einer relationalen Datenbank in eine Projektarbeitsbereich-Datenbank für ein tabellarisches Modell importieren können.  
  
-   Wie Sie Beziehungen zwischen den einzelnen Tabellen im Modell verwalten können.  
  
-   Wie Sie berechnete Spalten, Maßnahmen und Key Performance Indicators erstellen können, die Benutzern dabei helfen, kritische Geschäftsmetrik zu analysieren.  
  
-   Wie Sie Perspektiven und Hierarchien erstellen und verwalten können, die Benutzern dabei helfen, Modelldaten einfacher zu durchsuchen, indem sie geschäfts- und anwendungsspezifische Blickpunkte bereitstellen.  
  
-   Wie Sie Partitionen erstellen können, die Tabellendaten in kleiner logische Teile aufteilen, die unabhängig von anderen Partitionen verarbeitet werden können.  
  
-   Wie Sie Modellobjekte und -daten sichern können, indem Sie Rollen mit Benutzern erstellen.  
  
-   Wie Sie ein tabellarisches Modell über SSDT für einen Server mit **Azure Analysis Services** oder für einen Server mit **SQL Server 2017 Analysis Services** bereitstellen können.  
  
## <a name="prerequisites"></a>Voraussetzungen  
Für dieses Tutorial benötigen Sie Folgendes:  
  
-   Einen Azure Analysis Services-Server. Registrieren Sie sich für eine kostenlose [Textversion von Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) und [erstellen Sie einen Server](../analysis-services-create-server.md). 

-   Eine [Azure SQL Data Warehouse](../../sql-data-warehouse/create-data-warehouse-portal.md)-Instanz mit der **AdventureWorksDW-Beispieldatenbank** oder eine SQL Server Data Warehouse-Instanz mit der [AdventureWorks-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks).

    **Wichtig**: Wenn Sie die Beispieldatenbank auf einer lokalen SQL Server Data Warehouse-Instanz installieren und Ihr Modell für einen Server mit Azure Analysis Services bereitstellen, benötigen Sie ein [lokales Datengateway](../analysis-services-gateway.md).

-   Die aktuelle Version von [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) für Visual Studio.

-   Die aktuelle Version von [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Eine Client-Anwendung wie z.B. [Power BI Desktop](https://powerbi.microsoft.com/desktop/) oder Excel. 

## <a name="scenario"></a>Szenario  
Dieses Tutorial basiert auf Adventure Works Cycles, einem fiktiven Unternehmen. Adventure Works ist ein großes, multinationales Unternehmen, das Fahrräder, Teile und Zubehör herstellt und diese auf den nordamerikanischen, europäischen und asiatischen Handelsmärkten vertreibt. Das Unternehmen beschäftigt 500 Mitarbeiter. Zusätzlich beschäftigt Adventure Works mehrere regionale Verkaufsteams, die über das gesamte Marktgebiet verteilt sind. Sie werden damit beauftragt, ein tabellarisches Modell für Benutzer aus dem Vertrieb und Marketing zu erstellen, um Internetverkaufsdaten in der Datenbank „AdventureWorksDW“ zu analysieren.  
  
Sie müssen verschiedene Lektionen bearbeiten, um das Tutorial abzuschließen. Jede Lektion setzt sich aus verschiedenen Aufgaben zusammen. Sie müssen die Aufgaben nacheinander durchführen, um die Lektion abzuschließen. Während sich in einer Lektion möglicherweise mehrere Aufgaben befinden, die zu einem ähnlichen Ergebnis führen, unterscheiden sich die Aufgaben bei der Art und Weise der Ausführung. So wird verdeutlicht, dass es oft mehr als eine Möglichkeit gibt, eine bestimmte Aufgabe durchzuführen. Außerdem sollen Sie Fähigkeiten einsetzen, die Sie in vorherigen Lektionen und Aufgaben gelernt haben.  
  
Zweck der Lektionen soll es sein, Sie beim Schreiben eines einfachen tabellarischen Modells mithilfe von vielen in SSDT beinhalteten Funktion zu begleiten. Da jede Lektion auf der vorherigen aufbaut, sollten Sie diese in der richtigen Reihenfolge abschließen.
  
Dieses Lernprogramm bietet keine Lektionen zur Verwaltung eines Servers im Azure-Portal, zum Verwalten eines Servers oder einer Datenbank mit SSMS oder zum Durchsuchen von Modelldaten mithilfe einer Clientanwendung. 


## <a name="lessons"></a>Lektionen  
Dieses Lernprogramm enthält die folgenden Lektionen:  
  
|Lektion|Geschätzter Zeitaufwand|  
|----------|------------------------------|  
|[1. Erstellen eines neuen tabellarischen Modellprojekts](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 Minuten|  
|[2. Abrufen von Daten](../tutorials/aas-lesson-2-get-data.md)|10 Minuten|  
|[3. Markieren als Datumstabelle](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 Minuten|  
|[4. Erstellen von Beziehungen](../tutorials/aas-lesson-4-create-relationships.md)|10 Minuten|  
|[5. Erstellen berechneter Spalten](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 Minuten|
|[6. Erstellen von Measures](../tutorials/aas-lesson-6-create-measures.md)|30 Minuten|  
|[7. Erstellen von Key Performance Indicators (KPI)](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 Minuten|  
|[8. Erstellen von Perspektiven](../tutorials/aas-lesson-8-create-perspectives.md)|5 Minuten|  
|[9. Erstellen von Hierarchien](../tutorials/aas-lesson-9-create-hierarchies.md)|20 Minuten|  
|[10. Erstellen von Partitionen](../tutorials/aas-lesson-10-create-partitions.md)|15 Minuten|  
|[11. Erstellen von Rollen](../tutorials/aas-lesson-11-create-roles.md)|15 Minuten|  
|[12. Analysen in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 Minuten| 
|[13. Bereitstellen](../tutorials/aas-lesson-13-deploy.md)|5 Minuten|  
  
## <a name="supplemental-lessons"></a>Ergänzende Lektionen  
Diese Lektionen sind nicht erforderlich, um das Tutorial abzuschließen. Allerdings tragen sie dazu bei, dass Sie erweiterte Schreibfunktionen für tabellarische Modelle besser verstehen.  
  
|Lektion|Geschätzter Zeitaufwand|  
|----------|------------------------------|  
|[Detailzeilen](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 Minuten|
|[Dynamische Sicherheit](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 Minuten|
|[Unregelmäßige Hierarchien](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 Minuten| 

  
## <a name="next-steps"></a>Nächste Schritte  
Die ersten Schritte finden Sie unter [Lektion 1: Erstellen eines neuen tabellarischen Modellprojekts](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  


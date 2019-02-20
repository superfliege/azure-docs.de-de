---
title: Ausführen von Massenkopiervorgängen mit Steuertabelle für eine Datenbank mit Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Data Factory und einer Lösungsvorlage sämtliche Daten aus einer Datenbank auf einmal kopieren und dabei eine externe Steuertabelle verwenden, um eine Partitionsliste mit Quelltabellen zu speichern.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.openlocfilehash: b267da18f2537e462ecda0ac265eac07a069c293
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55966706"
---
# <a name="bulk-copy-from-database-with-control-table"></a>Ausführen von Massenkopiervorgängen mit Steuertabelle für eine Datenbank

Wenn Sie Daten aus Ihrem Data Warehouse (Oracle-Server, Netezza-Server, Teradata-Server, SQL Server oder Ähnliches) in Azure kopieren möchten, müssen Sie große Datenmengen aus mehreren Tabellen in Datenquellen laden. In den meisten Fällen müssen die Daten in den einzelnen Tabellen weiter partitioniert werden, um Zeilen mit mehreren Threads parallel aus einer einzelnen Tabelle laden zu können. Hierzu kann die vorliegende Vorlage verwendet werden. 

Wenn Sie dagegen Daten aus einigen wenigen Tabellen mit geringer Datengröße kopieren möchten, empfiehlt es sich, das Tool zum Kopieren von Daten zu verwenden, um nur eine einzelne Copy-Aktivität (oder eine ForEach-Aktivität und eine Copy-Aktivität) in Ihrer Pipeline zu haben. Für diesen einfachen Anwendungsfall ist diese Vorlage zu umfassend.

## <a name="about-this-solution-template"></a>Informationen zu dieser Lösungsvorlage

Diese Vorlage ruft die Liste mit den Partitionen der Quelldatenbank aus einer externen Steuertabelle ab, die in den Zielspeicher kopiert werden muss. Anschließend durchläuft sie die einzelnen Partitionen in der Quelldatenbank und führt den Datenkopiervorgang aus.

Die Vorlage enthält drei Aktivitäten:
-   Eine **Lookup**-Aktivität zum Abrufen der Liste mit den Partitionen der Quelldatenbank aus einer externen Steuertabelle.
-   Eine **ForEach**-Aktivität zum Abrufen der Partitionsliste aus der Lookup-Aktivität sowie zum anschließenden Durchlaufen der einzelnen Elemente für die Copy-Aktivität.
-   Eine **Copy**-Aktivität zum Kopieren der einzelnen Partitionen aus dem Quelldatenbankspeicher in den Zielspeicher.

Die Vorlage definiert fünf Parameter:
-   Der Parameter *Control_Table_Name* ist der Tabellenname für Ihre externe Steuertabelle. Die Steuertabelle dient zum Speichern der Partitionsliste für die Quelldatenbank.
-   Der Parameter *Control_Table_Schema_PartitionID* ist der Name der Spalte in Ihrer externen Steuertabelle, in der die einzelnen Partitions-IDs gespeichert werden. Die Partitions-ID muss für jede Partition in der Quelldatenbank eindeutig sein.
-   Der Parameter *Control_Table_Schema_SourceTableName* ist der Name der Spalte in Ihrer externen Steuertabelle, in der die einzelnen Tabellennamen aus der Quelldatenbank gespeichert werden.
-   Der Parameter *Control_Table_Schema_FilterQuery* ist der Name der Spalte in Ihrer externen Steuertabelle, in der die Filterabfrage zum Abrufen der Daten aus den einzelnen Partitionen der Quelldatenbank gespeichert wird. Wenn die Daten also beispielsweise nach Jahr partitioniert wurden, kann in den einzelnen Zeilen eine Abfrage wie die folgende gespeichert werden: ‘select * from datasource where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''
-   Der Parameter *Data_Destination_Folder_Path* ist der Ordnerpfad, an den die Daten in Ihrem Zielspeicher kopiert werden sollen.  Dieser Parameter ist nur sichtbar, wenn es sich bei dem gewählten Ziel um einen dateibasierten Speicher handelt.  Wenn Sie SQL Data Warehouse als Zielspeicher auswählen, ist hier keine Parametereingabe erforderlich. Die Tabellennamen und das Schema in SQL Data Warehouse müssen jedoch den Tabellennamen und dem Schema in der Quelldatenbank entsprechen.

## <a name="how-to-use-this-solution-template"></a>So verwenden Sie diese Lösungsvorlage

1. Erstellen Sie in einer SQL Server- oder SQL Azure-Instanz eine Steuertabelle, um die Partitionsliste der Quelldatenbank für einen Massenkopiervorgang zu speichern.  Im folgenden Beispiel enthält die Quelldatenbank fünf Partitionen: drei für die Tabelle *datasource_table* und zwei für die Tabelle *project_table*. Die Spalte *LastModifyTime* wird verwendet, um die Daten in der Tabelle *datasource_table* aus der Quelldatenbank zu partitionieren. Die Abfrage zum Lesen der ersten Partition lautet wie folgt: 'select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''.  Die Abfrage zum Lesen von Daten aus anderen Partitionen sieht ganz ähnlich aus. 

     ```sql
            Create table ControlTableForTemplate
            (
            PartitionID int,
            SourceTableName  varchar(255),
            FilterQuery varchar(255)
            );

            INSERT INTO ControlTableForTemplate
            (PartitionID, SourceTableName, FilterQuery)
            VALUES
            (1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
            (2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
            (3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
            (4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
            (5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');
    ```

2. Navigieren Sie zur Vorlage **Bulk Copy from Database** (Massenkopieren aus einer Datenbank), und erstellen Sie eine **neue Verbindung** mit Ihrer externen Steuertabelle.  Dies dient dazu, eine Verbindung mit der Datenbank herzustellen, in der Sie im ersten Schritt die Steuertabelle erstellt haben.

    ![Erstellen einer neuen Verbindung mit der Steuertabelle](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Erstellen Sie eine **neue Verbindung** mit der Quelldatenbank, aus der die Daten kopiert werden sollen.

     ![Erstellen einer neuen Verbindung mit der Quelldatenbank](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Erstellen Sie eine **neue Verbindung** mit Ihrem Zieldatenspeicher, in den die Daten kopiert werden sollen.

    ![Erstellen einer neuen Verbindung mit dem Zielspeicher](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Klicken Sie auf **Diese Vorlage verwenden**.

    ![„Diese Vorlage verwenden“](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. Die Pipeline wird im Bereich angezeigt, wie im folgenden Beispiel zu sehen:

    ![Überprüfen der Pipeline](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Klicken Sie auf **Debuggen**, geben Sie die Parameter ein, und klicken Sie dann auf **Fertig stellen**.

    ![Klicken auf „Debuggen“](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Das Ergebnis wird im Bereich angezeigt, wie im folgenden Beispiel zu sehen:

    ![Überprüfen des Ergebnisses](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (Optional) Wenn Sie SQL Data Warehouse als Ziel auswählen, müssen Sie als Stagingbereich auch die Verbindung einer Azure Blob Storage-Instanz eingeben, da dies von SQL Data Warehouse PolyBase vorausgesetzt wird.  Dabei muss der Container in Blob Storage bereits vorhanden sein.  
    
    ![PolyBase-Einstellung](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Nächste Schritte

- [Einführung in den Azure Data Factory-Dienst](introduction.md)
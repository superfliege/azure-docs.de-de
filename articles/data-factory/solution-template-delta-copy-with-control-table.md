---
title: Ausführen von Deltakopiervorgängen mit Steuertabelle für eine Datenbank mit Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Lösungsvorlage verwenden, um mit Azure Data Factory nur neue oder aktualisierte Zeilen inkrementell aus einer Datenbank zu kopieren.
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
ms.date: 12/24/2018
ms.openlocfilehash: 23e1255013cd5e52166fe0e59a8931dd9ecd81a0
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55966640"
---
# <a name="delta-copy-from-database-with-control-table"></a>Ausführen von Deltakopiervorgängen mit Steuertabelle für eine Datenbank

Verwenden Sie diese Vorgehensweise, wenn Sie nur Änderungen (also neue oder aktualisierte Zeilen) inkrementell aus einer Tabelle in einer Datenbank in Azure laden und dabei eine externe Steuertabelle verwenden möchten, in der der hohe Grenzwert gespeichert wird.  Hierzu kann die vorliegende Vorlage verwendet werden. 

Diese Vorlage setzt voraus, dass das Schema der Quelldatenbank eine Zeitstempelspalte oder einen Inkrementierungsschlüssel enthält, um die neuen oder aktualisierten Zeilen identifizieren zu können.

Falls Ihre Quelldatenbank zwar eine Zeitstempelspalte zur Identifizierung der neuen oder aktualisierten Zeilen enthält, Sie aber keine externe Steuertabelle für Deltakopien erstellen möchten, können Sie mithilfe des Tools zum Kopieren von Daten eine Pipeline abrufen, die eine triggerbasierte Zeit als Variable verwendet, um nur die neuen Zeilen aus der Quelldatenbank zu lesen.

## <a name="about-this-solution-template"></a>Informationen zu dieser Lösungsvorlage

Diese Vorlage ruft immer zuerst den alten Grenzwert ab und vergleicht ihn mit dem aktuellen Grenzwert. Auf der Grundlage des Vergleichs der beiden Grenzwerte werden dann nur die Änderungen aus der Quelldatenbank kopiert.  Nach Abschluss des Vorgangs wird der neue hohe Grenzwert in einer externen Steuertabelle gespeichert und beim nächsten Laden von Deltadaten herangezogen.

Die Vorlage enthält vier Aktivitäten:
-   Eine **Lookup**-Aktivität zum Abrufen des alten hohen Grenzwerts aus einer externen Steuertabelle.
-   Eine **Lookup**-Aktivität zum Abrufen des aktuellen hohen Grenzwerts aus der Quelldatenbank.
-   Eine **Copy**-Aktivität, um nur die Änderungen aus der Quelldatenbank in den Zielspeicher zu kopieren. Die Abfrage, die verwendet wird, um in der Copy-Aktivität die Änderungen aus der Quelldatenbank zu identifizieren, sieht in etwa wie folgt aus: 'SELECT * FROM Data_Source_Table WHERE TIMESTAMP_Column > “last high-watermark” and TIMESTAMP_Column <= “current high-watermark”'.
-   Eine **SqlServerStoredProcedure**-Aktivität, um den aktuellen hohen Grenzwert in eine externe Steuertabelle zu schreiben, damit er beim nächsten Deltakopiervorgang zur Verfügung steht.

Die Vorlage definiert fünf Parameter:
-   Der Parameter *Data_Source_Table_Name* ist der Tabellenname aus der Quelldatenbank, aus der Sie Daten laden möchten.
-   Der Parameter *Data_Source_WaterMarkColumn* ist der Spaltenname in der Quelltabelle, der zum Identifizieren der neuen oder aktualisierten Zeilen verwendet werden kann. Bei dieser Spalte handelt es sich in der Regel um eine Spalte mit Datum/Uhrzeit oder einer ganzen Zahl.
-   Der Parameter *Data_Destination_Folder_Path* oder *Data_Destination_Table_Name* ist der Ort, an den die Daten in Ihrem Zielspeicher kopiert werden sollen.
-   Der Parameter *Control_Table_Table_Name* ist der Name der externen Steuertabelle, in der der hohe Grenzwert gespeichert werden soll.
-   Der Parameter *Control_Table_Column_Name* ist der Spaltenname in der externen Steuertabelle, in der der hohe Grenzwert gespeichert werden soll.

## <a name="how-to-use-this-solution-template"></a>So verwenden Sie diese Lösungsvorlage

1. Untersuchen Sie die Quelltabelle, die Sie laden möchten, und definieren Sie die Spalte für den hohen Grenzwert, auf deren Grundlage die neuen oder aktualisierten Zeilen identifiziert werden können. Bei dieser Spalte handelt es sich in der Regel um eine Spalte mit Datum/Uhrzeit oder einer ganzen Zahl, deren Wert sich erhöht, wenn neue Zeilen hinzugefügt werden.  In der folgenden Beispielquelltabelle namens „data_source_table“ können wir die Spalte *LastModifytime* als Spalte für den hohen Grenzwert verwenden.

    ```sql
            PersonID    Name    LastModifytime
            1   aaaa    2017-09-01 00:56:00.000
            2   bbbb    2017-09-02 05:23:00.000
            3   cccc    2017-09-03 02:36:00.000
            4   dddd    2017-09-04 03:21:00.000
            5   eeee    2017-09-05 08:06:00.000
            6   fffffff 2017-09-06 02:23:00.000
            7   gggg    2017-09-07 09:01:00.000
            8   hhhh    2017-09-08 09:01:00.000
            9   iiiiiiiii   2017-09-09 09:01:00.000
    ```
    
2. Erstellen Sie in einer SQL Server- oder SQL Azure-Instanz eine Steuertabelle, um den hohen Grenzwert für das Laden von Deltadaten zu speichern. Im folgenden Beispiel lautet der Name der Steuertabelle *watermarktable*. Darin befindet sich die Spalte *WatermarkValue* vom Typ *datetime*, in der der hohe Grenzwert gespeichert wird.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Erstellen Sie eine gespeicherte Prozedur in der gleichen SQL Server- oder SQL Azure-Instanz, mit der Sie auch die Steuertabelle erstellt haben. Die gespeicherte Prozedur dient dazu, den neuen hohen Grenzwert in der externen Steuertabelle zu speichern, damit er beim nächsten Laden von Deltadaten zur Verfügung steht.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Navigieren Sie zur Vorlage **Delta copy from Database** (Deltakopieren aus einer Datenbank), und erstellen Sie eine **neue Verbindung** mit Ihrer Quelldatenbank, aus der Sie die Daten kopieren möchten.

    ![Erstellen einer neuen Verbindung mit der Quelltabelle](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Erstellen Sie eine **neue Verbindung** mit Ihrem Zieldatenspeicher, in den die Daten kopiert werden sollen.

    ![Erstellen einer neuen Verbindung mit der Zieltabelle](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Erstellen Sie eine **neue Verbindung** mit Ihrer externen Steuertabelle und Ihrer gespeicherten Prozedur.  Dies dient dazu, eine Verbindung mit der Datenbank herzustellen, in der Sie im zweiten und dritten Schritt die Steuertabelle und die gespeicherte Prozedur erstellt haben.

    ![Erstellen einer neuen Verbindung mit dem Datenspeicher der Steuertabelle](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Klicken Sie auf **Diese Vorlage verwenden**.

     ![„Diese Vorlage verwenden“](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. Die Pipeline wird im Bereich angezeigt, wie im folgenden Beispiel zu sehen:

     ![Überprüfen der Pipeline](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Klicken Sie auf die Aktivität „Gespeicherte Prozedur“, wählen Sie **Name der gespeicherten Prozedur** aus, klicken Sie auf **Import parameter** (Importparameter), und klicken Sie anschließend auf **Dynamischen Inhalt hinzufügen**.  

     ![Festlegen der Aktivität „Gespeicherte Prozedur“](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. Schreiben Sie den Inhalt **@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}**, und klicken Sie auf **Fertig stellen**.  

     ![Schreiben des Inhalts für Parameter für die gespeicherte Prozedur](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Klicken Sie auf **Debuggen**, geben Sie die Parameter ein, und klicken Sie dann auf **Fertig stellen**.

    ![Klicken auf „Debuggen“](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Das Ergebnis wird im Bereich angezeigt, wie im folgenden Beispiel zu sehen:

    ![Überprüfen des Ergebnisses](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Sie können neue Zeilen in Ihrer Quelltabelle erstellen.  SQL-Beispiel für die Erstellung neuer Zeilen:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
13. Führen Sie die Pipeline erneut aus, indem Sie auf **Debuggen** klicken, die Parameter eingeben und auf **Fertig stellen** klicken.

    ![Klicken auf „Debuggen“](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

14. Wie Sie sehen, wurden nur neue Zeilen in das Ziel kopiert.

15. (Optional) Wenn Sie SQL Data Warehouse als Ziel auswählen, müssen Sie als Stagingbereich auch die Verbindung einer Azure Blob Storage-Instanz eingeben, da dies von SQL Data Warehouse PolyBase vorausgesetzt wird.  Dabei muss der Container in Blob Storage bereits vorhanden sein.  
    
    ![Konfigurieren von PolyBase](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Nächste Schritte

- [Einführung in den Azure Data Factory-Dienst](introduction.md)
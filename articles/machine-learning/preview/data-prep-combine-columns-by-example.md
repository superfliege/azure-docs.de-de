---
title: "Transformation „Spalten nach Beispiel kombinieren“ mit Azure Machine Learning Workbench"
description: "Dies ist das Referenzdokument zur Transformation „Spalten nach Beispiel kombinieren“."
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 3ca1710c969b9bc5a1f56dc53f52c706e1ed07cd
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="combine-columns-by-example-transformation"></a>Transformation „Spalten nach Beispiel kombinieren“
Diese Transformation ermöglicht Benutzern das Hinzufügen einer neuen Spalte, indem Werte aus mehreren Spalten kombiniert werden. Der Benutzer kann zum Ausführen dieser Transformation ein Trennzeichen oder Beispiele für kombinierte Werte angeben. Wenn der Benutzer Beispiele für Kombinationen angibt, wird die Transformation von demselben **By-Example**-Modul verarbeitet, das bei der Transformation **Spalten nach Beispiel ableiten** verwendet wurde.

## <a name="how-to-perform-this-transformation"></a>Vorgehensweise zum Durchführen dieser Transformation

Führen Sie die folgenden Schritte aus, um diese Transformation durchzuführen:
1. Wählen Sie mindestens zwei Spalten aus, die zu einer Spalte kombiniert werden sollen. 
2. Wählen Sie im Menü **Transformationen** die Option **Spalten nach Beispiel kombinieren**. Klicken Sie alternativ mit der rechten Maustaste auf die Überschrift der ausgewählten Spalten, und wählen Sie im Kontextmenü die Option **Spalten nach Beispiel kombinieren**. Der Editor für Transformationen wird geöffnet, und neben der äußersten rechten ausgewählten Spalte wird eine neue Spalte hinzugefügt. Die neue Spalte enthält die kombinierten Werte, die durch ein Standardtrennzeichen getrennt werden. Ausgewählte Spalten können mit den Kontrollkästchen in den Spaltenüberschriften identifiziert werden. Das Hinzufügen und Entfernen von Spalten für die Auswahl ist über die Kontrollkästchen möglich.
3. Sie können den kombinierten Wert in der neu erstellten Spalte aktualisieren. Der aktualisierte Wert wird als Beispiel verwendet, um die Transformation zu veranschaulichen.
4. Klicken Sie auf **OK**, um die Transformation zu akzeptieren.

### <a name="transform-editor-advanced-mode"></a>Editor für Transformationen: Erweiterter Modus

Im erweiterten Modus können Sie zum Kombinieren von Spalten eine umfangreichere Benutzeroberfläche verwenden. 

Durch die Auswahl von **Trennzeichen** unter **Spalten kombinieren nach** können Benutzer Zeichenfolgen im Textfeld **Trennzeichen** angeben. Schließen Sie das Textfeld **Trennzeichen**, um eine Vorschau der Ergebnisse im Datenraster anzuzeigen. Klicken Sie auf **OK**, um einen Commit für die Transformation auszuführen.

Durch Auswahl von **Beispiele** unter **Spalten kombinieren nach** können Benutzer Beispiele für kombinierte Werte angeben. Um ein Beispiel für eine Zeile anzuzeigen, doppelklicken Sie auf die Zeilen im Raster. Geben Sie die erwartete Ausgabe in das Textfeld für die höher gestufte Zeile ein. Schließen Sie das Textfeld **Trennzeichen**, um eine Vorschau der Ergebnisse im Datenraster anzuzeigen. Klicken Sie auf **OK**, um einen Commit für die Transformation auszuführen. 

Benutzer können zwischen dem **Standardmodus**  und dem **erweiterten Modus** wechseln, indem Sie im Editor für Transformationen auf die entsprechenden Links klicken.

### <a name="editing-existing-transformation"></a>Bearbeiten von vorhandenen Transformationen

Benutzer können eine vorhandene Transformation vom Typ **Spalten nach Beispiel kombinieren** bearbeiten, indem sie während des Transformationsschritts die Option **Bearbeiten** wählen. Durch Klicken auf **Bearbeiten** wird der Editor für Transformationen im **Standardmodus** geöffnet. Benutzer können durch Klicken auf den Link in der Überschrift in den **Erweiterten Modus** wechseln. Alle Beispiele, die bei der Erstellung der Transformation bereitgestellt wurden, werden dort angezeigt.

## <a name="example-using-separators"></a>Beispiel für die Verwendung von Trennzeichen

Ein Komma gefolgt von einem Leerzeichen dient als Trennzeichen in diesem Beispiel, um die Spalten *Straße*, *Stadt*, *Bundesstaat* und *PLZ* zu kombinieren.

|Straße|City|Zustand|ZIP|Column|
|:----|:----|:----|:----|:----|
|16011 N.E. 36th Way|REDMOND|WA|98052|16011 N.E. 36th Way, REDMOND, WA, 98052|
|16021 N.E. 36th Way|REDMOND|WA|98052|16021 N.E. 36th Way, REDMOND, WA, 98052|
|16031 N.E. 36th Way|REDMOND|WA|98052|16031 N.E. 36th Way, REDMOND, WA, 98052|
|16041 N.E. 36th Way|REDMOND|WA|98052|16041 N.E. 36th Way, REDMOND, WA, 98052|
|16051 N.E. 36th Way|REDMOND|WA|98052|16051 N.E. 36th Way, REDMOND, WA, 98052|
|16061 N.E. 36th Way|REDMOND|WA|98052|16061 N.E. 36th Way, REDMOND, WA, 98052|
|3460 157th Avenue NE|REDMOND|WA|98052|3460 157th Avenue NE, REDMOND, WA, 98052|
|3350 157th Ave N.E.|REDMOND|WA|98052|3350 157th Ave N.E., REDMOND, WA, 98052|
|3240 157th Avenue N.E.|REDMOND|WA|98052|3240 157th Avenue N.E., REDMOND, WA, 98052|

## <a name="example-using-by-example"></a>Beispiel für die Kombination nach Beispiel

Der **fett** hervorgehobene Wert wurde als Beispiel angegeben.

|Date|Month (Monat)|Jahr|Hour|Minute|Sekunde|Kombinierte Spalte|
|:----|:----|:----|:----|:----|:----|:----|
|13|Okt|2016|15|01|23|**13-Okt-2016 15:01:23 PDT**|
|16|Okt|2016|16|22|33|16-Okt-2016 15:01:33 PDT|
|17|Okt|2016|12|43|12|17-Okt-2016 15:01:12 PDT|
|12|Nov|2016|14|22|44|12-Nov-2016 15:01:44 PDT|
|23|Nov|2016|01|52|45|23-Nov-2016 15:01:45 PDT|
|16|Jan|2017|22|34|56|16-Jan-2016 15:01:56 PDT|
|23|Mrz|2017|01|55|25|23-Mrz-2016 15:01:25 PDT|
|16|Apr|2017|11|34|36|16-Apr-2016 15:01:36 PDT|



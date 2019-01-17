---
title: 'Azure Analysis Services-Tutorial, Lektion 3: Markieren als Datumstabelle | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie eine Tabelle im Azure Analysis Services-Tutorialprojekt als Datumstabelle markiert werden kann.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c383fe30b8a6be3a5915f3cc1c0f5e5712ab328
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188996"
---
# <a name="mark-as-date-table"></a>Markieren als Datumstabelle

In „Lektion 2: Abrufen von Daten“ haben Sie die Dimensionstabelle DimDate importiert. Die in Ihrem Modell mit DimDate benannte Tabelle kann auch als eine *Datumstabelle* bekannt sein, da sie Tages- und Uhrzeitdaten enthält.  
  
Wenn Sie DAX-Zeitintelligenzfunktionen verwenden, wie Sie es etwas später beim Erstellen von Measures tun werden, müssen Sie Eigenschaften angeben, u.a. eine *Datumstabelle* und ein eindeutiger *Datumsspaltenbezeichner* in dieser Tabelle.
  
In dieser Lektion markieren Sie die DimDate-Tabelle als *Datumstabelle* und die Datumsspalte (in der Datumstabelle) als die *Datumsspalte* (eindeutiger Bezeichner).  

Doch bevor Sie die Datumstabelle und die Datumsspalte markieren, sollten Sie etwas aufräumen, um Ihr Modell verständlicher zu machen. Beachten Sie in der Tabelle „DimDate“ die Spalte mit dem Namen **FullDateAlternateKey**. Diese Spalte enthält eine Zeile für jeden Tag in jedem Kalenderjahr in der Tabelle. Diese Spalte werden Sie in Measureformeln und Berichten häufig benötigen. Allerdings ist FullDateAlternateKey kein guter Bezeichner für diese Spalte. Um sie in Formeln einfacher finden und verwenden zu können, benennen Sie die Spalte in **Datum** um. Immer, wenn dies möglich ist, sollten Tabellen und Spalten so umbenannt werden, dass sie in SSDT und Client-Berichtsanwendungen wie Power BI und Excel einfacher gefunden werden können. 
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **Drei Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige Lektion abgeschlossen haben: [Lektion 2: Abrufen von Daten](../tutorials/aas-lesson-2-get-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Umbenennen der Spalte FullDateAlternateKey

1.  Klicken Sie im Modell-Designer auf die **DimDate**-Tabelle.

2.  Doppelklicken Sie auf die Kopfzeile der Spalte **FullDateAlternateKey**, und benennen Sie sie in **Datum** um.

  
### <a name="to-set-mark-as-date-table"></a>Markieren als Datumstabelle  
  
1.  Wählen Sie die Spalte **Datum**, und klicken Sie dann im Fenster **Eigenschaften** unter **Datentyp**. Überprüfen Sie, ob **Datum** ausgewählt ist.  
  
2.  Klicken Sie auf das Menü **Tabelle**, klicken Sie dann auf **Datum**, und klicken Sie schließlich auf **Als Datumstabelle markieren**.  
  
3.  Wählen Sie im Dialogfeld **Als Datumstabelle markieren** im Listenfeld **Datum** die **Datumsspalte** als eindeutigen Bezeichner. In der Regel ist diese Spalte standardmäßig ausgewählt. Klicken Sie auf **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 4: Erstellen von Beziehungen](../tutorials/aas-lesson-4-create-relationships.md)
  

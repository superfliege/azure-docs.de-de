---
title: 'Azure Analysis Services-Tutorial – Lektion 8: Erstellen von Perspektiven | Microsoft-Dokumentation'
description: Dieser Artikel beschreibt, wie Perspektiven im Azure Analysis Services-Tutorialprojekt erstellt werden.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e92e86f59058f1f226c67f873530cd9c06b5d5fc
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49428333"
---
# <a name="create-perspectives"></a>Erstellen von Perspektiven

In dieser Lektion erstellen Sie eine Perspektive für Internetverkäufe. Durch eine Perspektive ist ein anzeigbarer Teil eines Modells definiert, in dem konzentrierte, geschäfts- oder anwendungsspezifische Sichtweisen geboten werden. Wenn sich ein Benutzer mit einem Modell verbindet und dabei eine Perspektive verwendet, sieht er nur die in dieser Perspektive definierten Objekte als Felder (Tabellen, Spalten, Measures, Hierarchien und KPIs). Weitere Informationen finden Sie unter [Perspektiven](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
Die Perspektive für Internetverkäufer, die Sie in dieser Lektion erstellen, umfasst nicht das Tabellenobjekt „DimCustomer“. Wenn Sie eine Perspektive erstellen, bei der bestimmte Objekte von der Ansicht ausgeschlossen sind, bleiben diese Objekte im Modell vorhanden. Sie sind allerdings nicht in der Feldliste eines Berichterstellungsclients sichtbar. Berechnete Spalten und Measures, die in einer Perspektive entweder enthalten oder nicht enthalten sind, können immer noch Berechnungen aus ausgeschlossenen Objektdaten vornehmen.  
  
In dieser Lektion soll beschrieben werden, wie Sie Perspektiven erstellen und sich mit den tabellarischen Modellerstellungstools vertraut machen können. Wenn Sie dieses Modell später um zusätzliche Tabellen erweitern, können Sie zusätzliche Perspektiven erstellen, um unterschiedliche Sichtweisen für das Modell zu definieren, z. B. Bestand und Verkauf.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **5 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 7: Erstellen von Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md) abgeschlossen haben.  
  
## <a name="create-perspectives"></a>Erstellen von Perspektiven  
  
#### <a name="to-create-an-internet-sales-perspective"></a>Erstellen einer Perspektive für Internetverkäufe  
  
1.  Klicken Sie auf das Menü **Modell** > **Perspektiven** > **Erstellen und Verwalten**.  
  
2.  Klicken Sie im Dialogfeld **Perspektiven** auf **Neue Perspektive**.  
  
3.  Doppelklicken Sie auf den Header der Spalte **Neue Perspektive**, und benennen Sie dann **Internetverkäufe** um.  
  
4.  Wählen Sie alle Tabellen *außer* **DimCustomer** aus.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    In einer späteren Lektion werden Sie diese Perspektive mit „In Excel analysieren“ testen. Die PivotTable-Feldliste in Excel enthält alle Tabellen außer „DimCustomer“.  

## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 9: Erstellen von Hierarchien](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  

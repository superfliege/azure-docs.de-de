---
title: Optimieren der Abfragezeit auf einem Azure Database for PostgreSQL-Server mithilfe der Strategie für TOAST-Tabellenspeicher
description: In diesem Artikel wird beschrieben, wie Sie die Abfragezeit mithilfe der Strategie für TOAST-Tabellenspeicher auf einem Azure Database for PostgreSQL-Server optimieren.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: dee8aaaef4b1998a7234a88d07ad5efbc79d050b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628639"
---
# <a name="optimizing-query-time-with-toast-table-storage-strategy"></a>Optimieren der Abfragezeit mithilfe der Strategie für TOAST-Tabellenspeicher 
In diesem Artikel wird beschrieben, wie Sie die Abfragezeit mithilfe der Strategie für TOAST-Tabellenspeicher optimieren.

## <a name="toast-table-storage-strategies"></a>Strategien für TOAST-Tabellenspeicher
Es gibt vier verschiedene Strategien, um TOAST-fähige Spalten auf einem Datenträger zu speichern, die verschiedene Kombinationen von Komprimierung und Out-of-Line-Speicherung darstellen. Die Strategie kann auf der Ebene des Datentyps und auf der Spaltenebene festgelegt werden.
- **Nur-Text** verhindert die Komprimierung oder Out-of-Line-Speicherung. Zudem wird bei dieser Option die Verwendung von Einzelbyte-Headern für Varlena-Typen deaktiviert. **Nur-Text** ist die einzige mögliche Strategie für Spalten mit nicht TOAST-fähigen Datentypen.
- **Erweitert** ermöglicht sowohl die Komprimierung als auch die Out-of-Line-Speicherung. **Erweitert** ist die Standardeinstellung für die meisten TOAST-fähigen Datentypen. Zuerst wird versucht, eine Komprimierung vorzunehmen. Ist die Zeile danach noch immer zu groß, wird anschließend eine Out-of-Line-Speicherung durchgeführt.
- **Extern** ermöglicht die Out-of-Line-Speicherung, aber keine Komprimierung. Die Verwendung von **Extern** beschleunigt Vorgänge für Teilzeichenfolgen für breite Spalten mit Daten vom Typ „text“ und „bytea“. Der Speicherplatzbedarf nimmt jedoch zu, da diese Vorgänge so optimiert sind, dass sie nur die erforderlichen Teile des Out-of-Line-Werts fetchen, wenn dieser nicht komprimiert ist.
- **Haupt** ermöglicht die Komprimierung, aber keine Out-of-Line-Speicherung. Die Out-of-Line-Speicherung wird weiterhin für solche Spalten ausgeführt, jedoch nur, wenn die Zeile nicht auf andere Weise so verkleinert werden kann, dass sie auf eine Seite passt.

## <a name="using-toast-table-storage-strategies"></a>Verwenden von Strategien für TOAST-Tabellenspeicher
Wenn Ihre Abfragen auf TOAST-fähig Datentypen zugreifen, sollten Sie anstelle der Standardoption **Erweiterte** ggf. die Option **Haupt** verwenden, um die Abfragezeiten zu reduzieren. Die Strategie **Haupt** schließt die Out-of-Line-Speicherung nicht aus. Wenn Ihre Abfragen nicht auf TOAST-fähig Datentypen zugreifen, kann es andererseits von Vorteil sein, die Option **Erweitert** beizubehalten. Dies verbessert die Leistung, da ein größerer Teil der Zeilen der Haupttabelle in den freigegebenen Puffercache passt.

Wenn Sie eine Workload mit einem Schema mit breiten Tabellen und hoher Zeichenanzahl haben, sollten Sie die Verwendung von PostgreSQL-TOAST-Tabellen erwägen. Bei einer Beispielkundentabelle mit mehr als 350 Spalten, die zum Teil 255 Zeichen enthielten, konnte die Abfragezeit im Vergleichstest von 4.203 Sekunden auf 467 Sekunden reduziert werden (eine Verbesserung von 89 Prozent), nachdem die TOAST-Strategie auf **Haupt** umgestellt wurde.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie, ob Ihre Workload die oben aufgeführten Eigenschaften aufweist. 

Lesen Sie die folgende PostgreSQL-Dokumentation: [Chapter 68, Database physical storage](https://www.postgresql.org/docs/current/storage-toast.html) (Kapitel 68, physischer Datenbankspeicher) 
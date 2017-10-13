---
title: "Unterstützte Inspektoren für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste mit Inspektoren, die für die Datenvorbereitung von Azure ML verfügbar sind."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: bc14c051fb0f518b1cff2236a61d24cb052700f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="supported-inspectors-for-this-preview"></a>Unterstützte Inspektoren für diese Vorschau
Dieses Dokument beschreibt den in dieser Vorschau verfügbaren Satz von Inspektoren.

## <a name="the-halo-effect"></a>Der Halo-Effekt 
Einige Inspektoren unterstützen den Halo-Effekt. Dieser Effekt verwendet zwei unterschiedliche Farben, um die Änderung über eine Transformation sofort visuell anzuzeigen. Grau stellt den vorherigen Wert vor der aktuellen Transformation dar, blau den aktuellen Wert. Dieser Effekt kann in den Optionen aktiviert/deaktiviert werden.

## <a name="graphical-filtering"></a>Grafische Filterung 
Einige der Inspektoren unterstützen das Filtern von Daten, indem der Inspektor als Editor verwendet wird. Dies beinhaltet die Auswahl von grafischen Elementen und dann die Verwendung der Symbolleiste oben rechts im Inspektorfenster, um die ausgewählten Werte mithilfe des Filters ein- oder auszublenden. 

## <a name="column-statistics"></a>Spaltenstatistiken
Für numerische Spalten bietet dieser Inspektor eine Vielzahl von verschiedenen Statistiken über die Spalte. Die Statistiken umfassen:
- Minimum
- Unteres Quartil
- Median
- Oberes Quartil
- Maximum
- Durchschnitt
- Standardabweichung


### <a name="options"></a>Optionen 
- Keine

## <a name="histogram"></a>Histogramm 
Berechnet ein Histogramm einer einzelnen numerischen Spalte und zeigt es an. Standardanzahl von Buckets wird mit Scotts Regel berechnet; die Regel kann über die Optionen überschrieben werden.
Dieser Inspektor unterstützt den Halo-Effekt.


### <a name="options"></a>Optionen
- Minimale Anzahl von Buckets (gilt auch, wenn Standardgruppierung aktiviert ist)
- Standardanzahl von Buckets (Scotts Regel) 
- Halo anzeigen
- Kerneldichteplot-Overlay (Gaußscher Kernel) 


### <a name="actions"></a>Actions
Dieser Inspektor unterstützt das Filtern über Buckets, Einfach- oder Mehrfachauswahl von Buckets und das Anwenden von Filtern, wie oben beschrieben.

## <a name="value-counts"></a>Werteanzahlen
Dieser Inspektor bietet eine Häufigkeitstabelle der Werte für die derzeit ausgewählte Spalte. Die Standardanzeige zeigt die obersten 6; das Limit kann in eine beliebige Zahl geändert werden, oder dass von unten nach oben gezählt wird. Dieser Inspektor unterstützt den Halo-Effekt.

### <a name="options"></a>Optionen 
- Anzahl der obersten Werte
- Absteigend
- NULL-/Fehlerwerte einbeziehen
- Halo anzeigen


### <a name="actions"></a>Actions 
Dieser Inspektor unterstützt das Filtern über Balken, Einfach- oder Mehrfachauswahl von Balken und das Anwenden von Filtern, wie oben beschrieben.

## <a name="box-plot"></a>Boxplot 
Ein Box-Whisker-Plot einer numerischen Spalte

### <a name="options"></a>Optionen 
- Gruppieren nach Spalte

## <a name="scatter-plot"></a>Punktdiagramm
Ein Punktdiagramm für zwei numerische Spalten; die Anzahl der Stichproben wird zur Verbesserung der Leistung reduziert (Downsampling), die Stichprobengröße kann in den Optionen überschrieben werden.

### <a name="options"></a>Optionen  
- Spalte für X-Achse
- Spalte für Y-Achse
- Stichprobengröße
- Gruppieren nach Spalte


## <a name="time-series"></a>Zeitreihe
Ein Liniendiagramm mit Zeiterfassung auf der X-Achse

### <a name="options"></a>Optionen
- Datumsspalte
- Numerische Spalte
- Stichprobengröße


### <a name="actions"></a>Actions
Dieser Inspektor unterstützt das Filtern mit der „Klicken-und-Ziehen“-Auswahlmethode, um einen Bereich im Diagramm auszuwählen. Wenden Sie nach Abschluss der Auswahl die Filter wie oben beschrieben an.


## <a name="map"></a>Map 
Eine mit Punkten gezeichnete Karte, wobei vorausgesetzt wird, dass Breiten- und Längengrad angegeben wurden. Der Breitengrad muss zuerst ausgewählt werden.

### <a name="options"></a>Optionen
- Breitengradspalte
- Längengradspalte
- Clustering ein
- Gruppieren nach Spalte


### <a name="actions"></a>Actions
Dieser Inspektor unterstützt das Filtern über Punktauswahl auf der Karte. Drücken Sie STRG, und bilden Sie dann durch Klicken und Ziehen mit der Maus ein Quadrat um die Punkte. Wenden Sie dann wie zuvor beschrieben Filter an.
Durch Auswahl von **E** auf der linken Seite der Karte kann die Größe der Karte schnell angepasst werden, um alle möglichen Punkte und keine weiteren anzuzeigen.

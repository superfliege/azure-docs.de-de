---
title: "Unterstützte Inspektoren für die Azure Machine Learning-Datenvorbereitung | Microsoft-Dokumentation"
description: "Dieses Dokument enthält eine vollständige Liste mit Inspektoren, die für die Azure Machine Learning-Datenvorbereitung verfügbar sind."
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
ms.openlocfilehash: 35d7c04f245e93d8cc795dca7c01c2bab5a14eb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Unterstützte Inspektoren für die Azure Machine Learning-Datenvorbereitung
Dieses Dokument beschreibt den in dieser Vorschau verfügbaren Satz von Inspektoren.

## <a name="the-halo-effect"></a>Der Halo-Effekt 
Einige Inspektoren unterstützen den Halo-Effekt. Dieser Effekt verwendet zwei unterschiedliche Farben, um die Änderung über eine Transformation sofort visuell anzuzeigen. Grau stellt den Wert vor der aktuellen Transformation dar, und Blau den aktuellen Wert. Dieser Effekt kann in den Optionen aktiviert und deaktiviert werden.

## <a name="graphical-filtering"></a>Grafische Filterung 
Einige der Inspektoren unterstützen das Filtern von Daten, indem der Inspektor als Editor verwendet wird. Das Verwenden des Inspektors als Editor beinhaltet die Auswahl von grafischen Elementen und dann die Verwendung der Symbolleiste oben rechts im Inspektorfenster, um die ausgewählten Werte mithilfe des Filters ein- oder auszublenden. 

## <a name="column-statistics"></a>Spaltenstatistiken
Für numerische Spalten bietet dieser Inspektor eine Vielzahl von verschiedenen Statistiken über die Spalte. Die Statistiken umfassen die folgenden Messungen: 
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
Berechnet ein Histogramm einer einzelnen numerischen Spalte und zeigt es an. Die Standardanzahl von Buckets wird anhand Scotts Regel berechnet. Die Regel kann jedoch über die Optionen überschrieben werden.

Dieser Inspektor unterstützt den Halo-Effekt.


### <a name="options"></a>Optionen
- Minimale Anzahl von Buckets (gilt auch, wenn Standardgruppierung aktiviert ist)
- Standardanzahl von Buckets (Scotts Regel) 
- Halo anzeigen
- Kerneldichteplot-Overlay (Gaußscher Kernel) 


### <a name="actions"></a>Actions
Dieser Inspektor unterstützt das Filtern über Buckets, was Einfach- oder Mehrfachauswahlbuckets umfassen kann. Wenden Sie Filter wie zuvor beschrieben an.

## <a name="value-counts"></a>Werteanzahl
Dieser Inspektor bietet eine Häufigkeitstabelle der Werte für die derzeit ausgewählte Spalte. Die Standardanzeige umfasst die obersten sechs Werte. Sie können den Grenzwert jedoch beliebig ändern. Sie können auch die Anzeige der untersten anstatt der obersten Werte festlegen. Dieser Inspektor unterstützt den Halo-Effekt.

### <a name="options"></a>Optionen 
- Anzahl der obersten Werte
- Absteigend
- NULL-/Fehlerwerte einbeziehen
- Halo anzeigen


### <a name="actions"></a>Actions 
Dieser Inspektor unterstützt das Filtern über Balken, was Einfach- oder Mehrfachauswahlbalken umfassen kann. Wenden Sie Filter wie zuvor beschrieben an.

## <a name="box-plot"></a>Boxplot 
Ein Box-Whisker-Plot einer numerischen Spalte.

### <a name="options"></a>Optionen 
- Gruppieren nach Spalte

## <a name="scatter-plot"></a>Punktdiagramm
Ein Punktdiagramm für zwei numerische Spalten. Aus Leistungsgründen werden die Daten im Downsampling erfasst. Die Stichprobengröße kann in den Optionen überschrieben werden.

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
Eine Karte mit gezeichneten Punkten, wobei vorausgesetzt wird, dass Breiten- und Längengrad angegeben wurden. Der Breitengrad muss zuerst ausgewählt werden.

### <a name="options"></a>Optionen
- Breitengradspalte
- Längengradspalte
- Clustering ein
- Gruppieren nach Spalte


### <a name="actions"></a>Actions
Dieser Inspektor unterstützt das Filtern über Punktauswahl auf der Karte. Drücken Sie **STRG**, und bilden Sie dann durch Klicken und Ziehen mit der Maus ein Quadrat um die Punkte. Wenden Sie dann wie zuvor beschrieben Filter an.

Durch Auswahl von **E** auf der linken Seite der Karte können Sie die Größe der Karte schnell anpassen, um alle möglichen Punkte anzuzeigen.

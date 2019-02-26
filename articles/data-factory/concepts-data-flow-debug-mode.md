---
title: Azure Data Factory-Mapping-Datenfluss – Debugmodus
description: Starten einer interaktiven Debugsitzung beim Erstellen von Datenflüssen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 567f64b8b720588807caeb5e49bae15f14c5b0a7
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329795"
---
# <a name="mapping-data-flow-debug-mode"></a>Mapping Data Flow – Debugmodus

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Der Azure Data Factory-Mapping-Datenfluss verfügt über einen Debugmodus, der mit der Debug-Schaltfläche oben auf der Designoberfläche aktiviert werden kann. Wenn Sie beim Erstellen von Datenflüssen den Debugmodus aktivieren, können Sie interaktiv die Transformation der Datenform beobachten, während Sie Ihre Datenflüsse erstellen und debuggen.

<img src="media/data-flow/debugbutton.png" width="400">

## <a name="overview"></a>Übersicht
Wenn der Debugmodus eingeschaltet ist, erstellen Sie Ihren Datenfluss interaktiv mit einem laufenden interaktiven Azure Databricks-Cluster. Die Sitzung wird beendet, sobald Sie das Debuggen in Azure Data Factory deaktivieren. Beachten Sie, dass stündlich Gebühren durch Azure Databricks anfallen, solange die Debugsitzung aktiviert ist.

In den meisten Fällen ist es eine gute Vorgehensweise, Ihre Datenflüsse im Debugmodus zu erstellen, sodass Sie Ihre Geschäftslogik validieren und Ihre Datentransformationen anzeigen können, bevor Sie Ihre Arbeit in Azure Data Factory veröffentlichen.

## <a name="debug-mode-on"></a>Debugmodus aktiviert
Wenn Sie den Debugmodus aktivieren, werden Sie in einem Formular im Seitenfenster aufgefordert, auf Ihren interaktiven Azure Databricks-Cluster zu verweisen und Optionen für das Quellsampling auszuwählen. Sie müssen einen interaktiven Cluster von Azure Databricks verwenden und entweder eine Samplinggröße aus jeder Ihrer Quelltransformationen auswählen oder eine Textdatei für Ihre Testdaten auswählen.

<img src="media/data-flow/upload.png" width="400">

> [!NOTE]
>Wenn der Datenfluss im Debugmodus ausgeführt wird, werden Ihre Daten nicht in die Senkentransformation geschrieben. Eine Debugsitzung soll als Testumgebung für Ihre Transformationen dienen. Senken sind während des Debuggens nicht erforderlich und werden in Ihrem Datenfluss ignoriert. Wenn Sie das Schreiben der Daten in Ihre Senke testen möchten, führen Sie den Datenfluss aus einer Azure Data Factory-Pipeline aus, und verwenden Sie die Debugausführung aus einer Pipeline.

## <a name="debug-settings"></a>Debugeinstellungen
Debugeinstellungen können alle Quellen aus Ihrem Datenfluss sein. Sie werden im Seitenfenster angezeigt und können bearbeitet werden, indem Sie die Option „Quelleinstellungen“ in der Entwurfssymbolleiste auswählen. Sie können hier die Grenzwerten und/oder die Dateiquelle auswählen, die für jede Ihrer Quelltransformationen verwendet werden soll. Sie können auch auswählen, welchen Databricks-Cluster Sie für das Debuggen verwenden möchten.

## <a name="cluster-status"></a>Clusterstatus
Oben in der Entwurfsoberfläche befindet sich eine Clusterstatusanzeige. Diese wird grün, wenn der Cluster bereit zum Debuggen ist. Wenn Ihr Cluster bereits betriebsbereiten ist, wird die Anzeige nahezu sofort grün. Wenn Ihr Cluster noch nicht ausgeführt wird, wenn Sie in den Debugmodus wechseln, müssen Sie 5-7 Minuten warten, bis der Cluster gestartet ist. Die Anzeige ist gelb, bis der Cluster bereit ist. Sobald Ihr Cluster für das Datenfluss-Debugging bereit ist, leuchtet die Anzeige grün.

Wenn Sie mit dem Debuggen fertig sind, deaktivieren Sie den Debugmodus, damit Ihr Azure Databricks-Cluster beendet werden kann.

<img src="media/data-flow/datapreview.png" width="400">

## <a name="data-preview"></a>Datenvorschau
Wenn das Debuggen aktiviert ist, wird im unteren Bereich die Registerkarte „Datenvorschau“ angezeigt. Wenn der Debugmodus deaktiviert ist, werden im Datenfluss auf der Registerkarte „Überprüfen“ nur die für Ihre Transformationen eingehenden und ausgehenden Metadaten angezeigt. Die Datenvorschau fragt nur die Anzahl der Zeilen ab, die Sie in Ihren Quelleinstellungen als Grenzwert festgelegt haben. Sie müssen auf „Daten abrufen“ klicken, um die Datenvorschau zu aktualisieren.

<img src="media/data-flow/stats.png" width="400">

## <a name="data-profiles"></a>Datenprofile
Wenn Sie einzelne Spalten in der Datenvorschau auswählen, wird ganz rechts in Ihrem Datentabelle ein Diagramm mit detaillierten Statistiken zu jedem Feld angezeigt. Azure Data Factory wird basierend auf dem Datensampling bestimmen, welche Art von Diagramm für die Anzeige verwendet wird. Felder mit hoher Kardinalität werden standardmäßig mit NULL / NOT NULL-Diagrammen angezeigt, kategorische und numerische Daten mit niedriger Kardinalität werden in Balkendiagrammen mit Datenwerthäufigkeit dargestellt. Außerdem wird die maximale Länge der Zeichenkettenfelder, Min/Max-Werte in numerischen Feldern, Standardabweichung, Perzentile, Anzahl und Mittelwert angezeigt. 

<img src="media/data-flow/chart.png" width="400">

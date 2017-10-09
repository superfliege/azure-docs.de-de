---
title: "Erste Schritte mit der Datenvorbereitung für Azure Machine Learning | Microsoft-Dokumentation"
description: "Dies ist der Leitfaden zu den ersten Schritten für den Datenvorbereitungsabschnitt von AML Workbench."
author: cforbe
ms.author: cforbe
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: faf6e557914a33454229b5830419b9ef6f200a8c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="getting-started-with-data-preparation"></a>Erste Schritte bei der Datenvorbereitung

Willkommen beim Leitfaden zu den ersten Schritten für die Datenvorbereitung. 

Die Datenvorbereitung stellt eine Gruppe von Tools zum effizienten Untersuchen, Verstehen und Beheben von Problemen mit Daten bereit. Hiermit können Sie Daten in vielen Formen nutzen und in bereinigte Daten transformieren, die für die weitere Nutzung besser geeignet sind.

Die Datenvorbereitung wird als Teil der Azure Machine Learning Workbench-Benutzeroberfläche installiert.  Verwenden Sie die Datenvorbereitung lokal, oder stellen Sie sie in einem Zielcluster und in der Cloud als Laufzeit- oder Ausführungsumgebung bereit.

Für die Entwurfszeit-Laufzeit wird Python aus Gründen der Erweiterbarkeit verwendet, und sie ist von verschiedenen Python-Bibliotheken abhängig, z.B. Pandas. Wie bei anderen Komponenten von Azure ML Workbench auch, müssen Sie Python nicht selbst installieren, sondern die Installation wird für Sie durchgeführt. Falls Sie aber zusätzliche Bibliotheken installieren müssen, ist es erforderlich, diese Bibliotheken im Python-Verzeichnis von Azure ML Workbench und nicht in Ihrem üblichen Python-Verzeichnis zu installieren. Weitere Informationen zur Installation von Paketen finden Sie [hier](data-prep-python-extensibility-overview.md).

Es ist ein Projekt erforderlich, bevor Sie die Datenvorbereitung nutzen können, und nachdem dieses Projekt erstellt wurde, können Sie Daten vorbereiten. 

Navigieren Sie zum Abschnitt „Daten“ des Projekts, indem Sie das Datensymbol ![Datenquellensymbol](media/data-prep-getting-started/data-source-icon.png) links auf dem Bildschirm auswählen.  Klicken Sie erneut auf „+“, um den Vorgang **Datenquelle hinzufügen** durchzuführen. Der Datenquellen-Assistent sollte gestartet werden, und nach Abschluss des Assistenten sollte dem Projekt eine Datei vom Typ **Datenquelle** (.dsource) hinzugefügt werden. Standardmäßig werden die Daten in der Rasteransicht angezeigt. Oberhalb des Rasters können Sie auch die Ansicht „Metriken“ auswählen. In der Ansicht „Metriken“ wird eine Zusammenfassung der statistischen Daten angezeigt.  Klicken Sie nach dem Überprüfen der statistischen Daten am oberen Bildschirmrand auf **Vorbereiten**. [Weitere Informationen zum Datenquellen-Assistenten](data-source-wizard.md) 

## <a name="building-blocks-of-data-preparation"></a>Bausteine der Datenvorbereitung ##
### <a name="the-package"></a>Das Paket ###

Ein Paket ist der primäre Container für Ihre Arbeit. Bei einem Paket handelt es sich um das Artefakt, das auf dem Datenträger gespeichert und von dort geladen wird. Bei der Arbeit auf dem Client wird das Paket im Hintergrund fortlaufend automatisch gespeichert. 

Die Ausgabe bzw. die Ergebnisse eines Pakets können in Python oder über ein Jupyter-Notebook untersucht werden.

Ein Paket kann übergreifend für mehrere Laufzeiten ausgeführt werden, z.B. lokales Python, Spark (auch in Docker) und HDInsight.

Eine Paket enthält einen oder mehrere Datenflüsse, bei denen es sich um die Schritte und Transformationen handelt, die auf die Daten angewendet werden.

Ein Paket kann ein anderes Paket als Datenquelle nutzen (als Referenzdatenfluss bezeichnet).

### <a name="the-dataflow"></a>Der Datenfluss ###
Ein Datenfluss verfügt über eine Quelle und ggf. auch über Transformationen, die über eine Reihe von Schritten und optionalen Zielen angeordnet werden. Durch das Klicken auf einen Schritt werden alle Quellen und Transformationen bis zum ausgewählten Schritt und einschließlich dieses Schritts erneut ausgeführt.  Die transformierten Daten dieses Schritts werden innerhalb des Rasters angezeigt. Schritte können in einem Datenfluss über die Schrittliste hinzugefügt, verschoben und gelöscht werden.

Die Schrittliste auf der rechten Seite des Clients kann geöffnet und geschlossen werden, um für mehr Platz auf dem Bildschirm zu sorgen.

Auf der Benutzeroberfläche können gleichzeitig mehrere Datenflüsse vorhanden sein, wobei jeder Datenfluss hierbei als Registerkarte dargestellt wird.

### <a name="the-source"></a>Die Quelle
Daten stammen aus einer Quelle, und die Quelle bestimmt auch das Format der Daten. Ein Paket für die Datenvorbereitung bezieht seine Daten immer aus einem anderen Datenfluss (Datenquelle). Dieser Referenzdatenfluss enthält die Informationen. Jede Quelle verfügt über eine andere Benutzeroberfläche für die Konfiguration. Die Quelle erstellt eine „rechteckige“ Ansicht bzw. Tabellenansicht der Daten. Falls die Quelldaten auf der rechten Seite „ausgefranst“ sind, wird die Struktur normalisiert, um eine rechteckige Ansicht zu erhalten. [Anhang 2 enthält die aktuelle Liste mit den unterstützten Quellen](data-prep-appendix2-supported-data-sources.md).

### <a name="the-transform"></a>Die Transformation ###
Transformationen nutzen Daten in einem bestimmten Format, führen einige Vorgänge für die Daten durch (z.B. die Änderung des Datentyps) und erstellen dann Daten im neuen Format. Jede Transformation weist eine eigene Benutzeroberfläche und ein eigenes Verhalten auf. Das Verketten von mehreren Transformationen anhand von Schritten im Datenfluss ist der Kern der Funktionalität für die Datenvorbereitung. [Anhang 3 enthält die aktuelle Liste mit den unterstützten Transformationen](data-prep-appendix3-supported-transforms.md).

### <a name="the-inspector"></a>Der Inspektor ###

Inspektoren sind Visualisierungen der Daten und dienen zur Verbesserung des Verständnisses von Daten.  Wenn Sie die Probleme in Bezug auf die Daten und die Datenqualität verstehen, können Sie leichter entscheiden, welche Aktionen (Transformationen) durchgeführt werden sollten. Einige Inspektoren unterstützen Aktionen zur Generierung von Transformationen. Mit dem Inspektor für die Anzahl von Werten können Sie beispielsweise einen Wert auswählen und dann einen Filter anwenden, um den Wert ein- oder auszuschließen. Inspektoren können auch Kontext für Transformationen bereitstellen. Durch die Auswahl einer oder mehrerer Spalten ändern sich beispielsweise die möglichen Transformationen, die angewendet werden können.

Eine Spalte kann jederzeit über mehrere Inspektoren verfügen (z.B. Spaltenstatistiken und ein Histogramm). Es können auch Instanzen eines Inspektors in mehreren Spalten vorhanden sein. Beispielsweise können alle numerischen Spalten gleichzeitig über Histogramme verfügen.

Inspektoren werden am unteren Bildschirmrand im Profilerstellungsbereich angezeigt.  Maximieren Sie Inspektoren, damit sie im Hauptbereich für Inhalte größer angezeigt werden. Stellen Sie sich das Datenraster als Standardinspektor vor. Alle Inspektoren können in den Hauptbereich für Inhalte erweitert werden. Inspektoren im Hauptbereich für Inhalte werden im Profilerstellungsbereich minimiert. Passen Sie den Inspektor an, indem Sie darin auf das Stiftsymbol klicken. Sie können Inspektoren in diesem Bereich per Drag & Drop anordnen.

Einige Inspektoren unterstützen den Modus „Halo“. In diesem Modus wird der Wert oder der Zustand vor Anwendung der letzten Transformation angezeigt. Der alte Wert wird in Grau mit dem aktuellen Wert im Vordergrund angezeigt, und es wird die Auswirkung einer Transformation angegeben. [Anhang 4 enthält die aktuelle Liste mit den unterstützten Inspektoren](data-prep-appendix4-supported-inspectors.md).

### <a name="the-destination"></a>Das Ziel
 Ein Ziel ist der Ort, an den Sie die Daten schreiben bzw. exportieren, nachdem Sie sie in einem Datenfluss vorbereitet haben. Ein bestimmter Datenfluss kann mehrere Ziele haben. Jedes Ziel verfügt über eine andere Benutzeroberfläche für die Konfiguration. Das Ziel nutzt Daten in einem „rechteckigen“ bzw. tabellarischen Format und schreibt sie im jeweiligen Format an einen bestimmten Speicherort. [Anhang 5 enthält die aktuelle Liste mit den unterstützten Zielen](data-prep-appendix5-supported-destinations.md).

### <a name="using-data-preparation"></a>Verwenden der Datenvorbereitung ###
Für die Datenvorbereitung wird eine einfache Methodik bzw. ein einfacher Ansatz mit fünf Schritten genutzt.

#### <a name="step-1-ingestion"></a>Schritt 1: Erfassung ####
Importieren Sie Daten für die Datenvorbereitung, indem Sie in der Projektansicht die Option **Datenquelle hinzufügen** verwenden.  Die gesamte anfängliche Erfassung von Daten wird über den Datenquellen-Assistenten abgewickelt.

#### <a name="step-2-understandprofile-the-data"></a>Schritt 2: Verstehen und Durchführen der Profilerstellung für die Daten ####

Sehen Sie sich zuerst oben in jeder Spalte den Balken zur Datenqualität an. Grün steht für die Zeilen, die Werte enthalten. Grau steht für Zeilen mit einem fehlenden Wert, NULL-Wert usw. Rot steht für die Fehlerwerte. Bewegen Sie den Mauszeiger auf den Balken, um eine QuickInfo mit der genauen Anzahl von Zeilen in jedem der drei Buckets zu erhalten. Für den Balken zur Datenqualität wird eine logarithmische Skalierung verwendet. Sehen Sie sich daher immer die tatsächlichen Zahlen an, um einen ungefähren Eindruck zum Umfang der fehlenden Daten zu erhalten.

![Spalten](media/data-prep-getting-started/columns.png)

Verwenden Sie als Nächstes eine Kombination aus anderen Inspektoren und das Raster, um die Datenmerkmale noch besser zu verstehen.  Beginnen Sie mit dem Formulieren von Hypothesen zur Datenverarbeitung, die für die weitere Analyse erforderlich sind. Die meisten Inspektoren werden für eine einzelne Spalte oder eine geringe Anzahl von Spalten verwendet.  

Die Wahrscheinlichkeit ist relativ hoch, dass mehrere Inspektoren für mehrere Spalten benötigt werden, um die Daten zu verstehen. Sie können im Profilerstellungsbereich durch viele verschiedene Inspektoren scrollen. Hier können Sie Inspektoren auch an den Anfang der Liste verschieben, damit sie im direkt sichtbaren Bereich angezeigt werden.

![Inspektoren](media/data-prep-getting-started/inspectors.PNG)

Für kontinuierliche und kategorische Variablen bzw. Spalten werden unterschiedliche Inspektoren bereitgestellt. Im Inspektor-Menü werden Optionen je nach dem Typ Ihrer Variablen bzw. Spalten aktiviert und deaktiviert.

Bei Verwendung von umfangreichen Datasets mit vielen Spalten empfiehlt sich ein pragmatischer Ansatz für den Umgang mit Teilmengen. Dieser Ansatz umfasst die Konzentration auf eine geringe Anzahl von Spalten (z.B. 5 - 10), die Vorbereitung dieser Spalten und die anschließende Bearbeitung der restlichen Spalten. Der Rasterinspektor unterstützt die vertikale Partitionierung von Spalten. Wenn Sie mehr als 300 Spalten haben, sind diese auf mehrere Seiten verteilt, die Sie durchgehen müssen.
 

#### <a name="step-3-transform-the-data"></a>Schritt 3: Transformieren der Daten ####
Mit Transformationen werden die Daten geändert und Ausführungen der Daten ermöglicht, um die aktuelle Arbeitshypothese zu unterstützen. Transformationen werden in der Schrittliste auf der rechten Seite als Schritte angezeigt. Es ist möglich, quasi eine „Zeitreise“ durch die Schrittliste durchzuführen, indem Sie darin auf einen beliebigen Punkt klicken.

Mit einem grünen Symbol links von einem Schritt wird jeweils angegeben, dass er ausgeführt wurde, und die Daten spiegeln die Ausführung der Transformation wider. Mit einem vertikalen Strich links vom Schritt wird der aktuelle Status der Daten in den Inspektoren angegeben.

![Schritte](media/data-prep-getting-started/steps.PNG)

Versuchen Sie, häufiger kleinere Änderungen an den Daten vorzunehmen und nach jeder Änderung eine Überprüfung durchzuführen (Schritt 4), während die Hypothese weiterentwickelt wird.

#### <a name="step-4-verify-the-impact-of-the-transformation"></a>Schritt 4: Überprüfen der Auswirkungen einer Transformation 
Entscheiden Sie, ob die Hypothese korrekt war. Wenn ja, können Sie die nächste Hypothese entwickeln und die Schritte 2 und 3 dafür erneut ausführen. Wenn nicht, sollten Sie die letzte Transformation rückgängig machen, eine neue Hypothese entwickeln und die Schritte 2 und 3 wiederholen.

Die gängige Vorgehensweise, um zu ermitteln, ob sich für die Transformation die richtigen Auswirkungen ergeben haben, ist die Verwendung der Inspektoren. Nutzen Sie die vorhandenen Inspektoren. Verwenden Sie Inspektoren mit aktiviertem Halo-Effekt, oder starten Sie mehrere Inspektoren, um die Daten für bestimmte Zeitpunkte anzuzeigen.

![Halo-Inspektor](media/data-prep-getting-started/halo1.PNG) ![Halo-Inspektor](media/data-prep-getting-started/halo2.PNG)

Navigieren Sie zur Schrittliste auf der rechten Seite der Benutzeroberfläche, um eine Transformation rückgängig zu machen. (Unter Umständen müssen Sie den Bereich mit der Schrittliste wieder anzeigen. Klicken Sie zum Öffnen auf den nach links zeigenden Chevron-Doppelpfeil). Wählen Sie im Bereich die ausgeführte Transformation aus, die Sie rückgängig machen möchten. Wählen Sie auf der rechten Seite des Benutzeroberflächenblocks die Dropdownliste aus. Wählen Sie entweder **Bearbeiten**, um Änderungen vorzunehmen, oder **Löschen**, um die Transformation aus der Schrittliste und dem Datenfluss zu entfernen.

#### <a name="step-5-output"></a>Schritt 5: Ausgabe 
Nachdem Sie die Datenvorbereitung abgeschlossen haben, können Sie den Datenfluss in eine Ausgabe schreiben. Ein Datenfluss kann über viele Ausgaben verfügen. Im Menü „Transformationen“ können Sie auswählen, als welche Ausgabe das Dataset geschrieben werden soll. Sie können auch das Ziel der Ausgabe auswählen. 

## <a name="list-of-appendices"></a>Liste der Anhänge 
[Appendix 2 - Supported Data Sources](data-prep-appendix2-supported-data-sources.md) (Anhang 2 – Unterstützte Datenquellen)  
[Appendix 3 - Supported Transforms](data-prep-appendix3-supported-transforms.md) (Anhang 3 – Unterstützte Transformationen)  
[Appendix 4 - Supported Inspectors](data-prep-appendix4-supported-inspectors.md) (Anhang 4 – Unterstützte Inspektoren)  
[Appendix 5 - Supported Destinations](data-prep-appendix5-supported-destinations.md) (Anhang 5 – Unterstützte Ziele)  
[Appendix 6 - Sample Filter Expressions in Python](data-prep-appendix6-sample-filter-expressions-python.md) (Anhang 6 – Beispiele für Filterausdrücke in Python)  
[Appendix 7 - Sample Transform Dataflow Expressions in Python](data-prep-appendix7-sample-transform-data-flow-python.md) (Anhang 7 – Beispiele für Ausdrücke für die Datenflusstransformation in Python)  
[Appendix 8 - Sample Data Sources in Python](data-prep-appendix8-sample-source-connections-python.md) (Anhang 8 – Beispiele für Datenquellen in Python)  
[Appendix 9 - Sample Destination Connections in Python](data-prep-appendix9-sample-destination-connections-python.md) (Anhang 9 – Beispiele für Zielverbindungen in Python)  
[Appendix 10 - Sample Column Transforms in Python](data-prep-appendix10-sample-custom-column-transforms-python.md) (Anhang 10 – Beispiele für Spaltentransformationen in Python)  

## <a name="see-also"></a>Weitere Informationen

[Tutorial zur erweiterten Datenvorbereitung](tutorial-bikeshare-dataprep.md)

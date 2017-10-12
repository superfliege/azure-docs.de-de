---
title: "Azure-Datenquellen-Assistent für Azure Machine Learning | Microsoft-Dokumentation"
description: "In diesem Artikel wird der Datenquellen-Assistenten von Azure Machine Learning (AML) Workbench erläutert."
author: cforbe
ms.author: cforbe
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: bed026e8f75618403efa3eed475371d1d2746bc5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="data-source-wizard"></a>Datenquellen-Assistent #

Der Datenquellen-Assistent bietet eine schnelle und einfache Möglichkeit, um ein Dataset ohne Code in Azure ML Workbench einzubinden. In diesem Tool können Sie auch eine Beispielstrategie für das Dataset auswählen. 

## <a name="step-1-trigger-the-data-source-wizard"></a>Schritt 1: Auslösen des Datenquellen-Assistenten ## 

Um Daten in ein Projekt zu importieren, verwenden Sie den Datenquellen-Assistenten. Wählen Sie in der Datenansicht neben dem Suchfeld die Schaltfläche **+** und dann Datenquelle hinzufügen aus. 

![Hinzufügen einer Datenquelle](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>Schritt 2: Auswählen eines Speicherorts für die Daten ##
Geben Sie zunächst an, um welche Daten es sich handelt. Es kann sich um eine Flatfile-Datei, ein Verzeichnis, eine PARQUET-Datei, eine Excel-Datei oder eine Datenbank handeln. Weitere Informationen finden Sie unter [Unterstützte Datenquellen](data-prep-appendix2-supported-data-sources.md).

![Schritt 1](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>Schritt 3: Auswählen einer Datendatei ##
Geben Sie den Dateipfad einer Datei bzw. eines Verzeichnisses an. Wählen Sie in der Dropdownliste den Speicherort der Daten aus – nämlich einen lokalen Pfad, Azure Blob Storage oder Azure Data Lake. 

Geben Sie den Pfad an, indem Sie ihn eingeben oder auf die Schaltfläche **Durchsuchen...** klicken, um zum jeweiligen Speicherort zu navigieren. Sie können zu einem Verzeichnis oder einer bzw. mehreren Dateien navigieren.

Klicken Sie auf **Fertig stellen**, um die Standardwerte für die restlichen Schritte zu übernehmen, oder auf „Weiter“, um mit dem nächsten Schritt fortzufahren.


![Schritt 4](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>Schritt 4: Auswählen der Dateiparameter ##

Der Datenquellen-Assistent kann automatisch den Dateityp, Trennzeichen und die Codierung erkennen. Zudem wird ein Beispiel dafür angezeigt, wie die Daten aussehen. Sie können diese Parameter auch manuell ändern. 

![Schritt 5](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>Schritt 5: Festlegen von Datentypen für Spalten ##

Der Datenquellen-Assistent erkennt automatisch die Datentypen der Spalten des Datasets. Wenn die Erkennung fehlerhaft sein sollte oder Sie einen anderen Datentyp erzwingen möchten, können Sie den Datentyp manuell ändern. Die Spalte **BEISPIELAUSGABEDATEN** zeigt Beispiele dafür an, wie die Daten aussehen.

![Schritt 6](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>Schritt 6: Auswählen einer Strategie für die Stichprobenentnahme von Daten ##

Sie können für das Dataset eine oder mehrere Strategien für die Stichprobenentnahme angeben und eine als aktive Strategie auswählen. Standardmäßig werden die ersten 10.000 Zeilen geladen. Sie können dieses Beispiel bearbeiten, indem Sie auf der Symbolleiste auf die Schaltfläche **Bearbeiten** klicken, oder eine neue Strategie hinzufügen, indem Sie auf „+Neu“ klicken. Derzeit werden folgende Strategien unterstützt:

-     Anzahl der obersten Zeilen
-     Zufällige Anzahl von Zeilen
-     Zufälliger Prozentsatz von Zeilen
-     Vollständige Datei

Sie können bei der Vorbereitung Ihrer Daten beliebig viele Strategien für die Stichprobenentnahme angeben, jedoch nur eine als aktiv festlegen. Sie können jede Strategie als aktiv festlegen, indem Sie die Strategie auswählen und auf der Symbolleiste auf „Als aktiv festlegen“ klicken.

Einige Beispielstrategien werden je nach Datenherkunft möglicherweise nicht unterstützt. Weitere Informationen zur Stichprobenentnahme finden Sie in [diesem Dokument](data-prep-user-guide.md) im Abschnitt zur Stichprobenentnahme. 

![Schritt 6](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>Schritt 7: Verarbeitung der Pfadspalte ##

Wenn der Dateipfad wichtige Daten enthält, können Sie festlegen, dass dieser als erste Spalte im Dataset eingefügt werden soll. Dies ist hilfreich, wenn Sie mehrere Dateien einbinden. Alternativ können Sie festlegen, dass diese nicht eingefügt werden sollen.

![Schritt 7](media/data-source-wizard/step6.png)

Wenn Sie auf „Fertig stellen“ klicken, wird eine neue Datenquelle zum Projekt hinzugefügt. Sie finden diese in der Datenansicht in der Gruppe „Datenquellen“ oder als DSOURCE-Datei in der **Dateiansicht**.

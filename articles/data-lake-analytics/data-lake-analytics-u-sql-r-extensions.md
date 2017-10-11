---
title: Erweitern der U-SQL-Skripts mit R in Azure Data Lake Analytics | Microsoft Docs
description: "Weitere Informationen Sie zum Ausführen von R-Code in U-SQL-Skripts"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: d479af515566f497d9611e75426f6acb8f8276d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Tutorial: Erste Schritte mit Erweitern der U-SQL mit R

Das folgende Beispiel veranschaulicht die grundlegenden Schritte zum Bereitstellen von R-Code:
* Verwenden der `REFERENCE ASSEMBLY` -Anweisung können Sie R-Erweiterungen für das U-SQL-Skript aktivieren.
* Verwenden der` REDUCE` Vorgang, um die Eingabedaten nach einem Schlüssel zu partitionieren.
* Die R-Erweiterungen für U-SQL-enthalten einen integrierten reducerdatei an (`Extension.R.Reducer`) auf jedem Vertex reducerdatei zugewiesen R-Code ausgeführt wird. 
* Verwendung von dedizierten namens Datenrahmen aufgerufen `inputFromUSQL` und `outputToUSQL `zum Übergeben von Daten zwischen U-SQL und r-Eingabe und Ausgabe Datenrahmen Bezeichnernamen behoben werden (d. h. können nicht Benutzer ändern Sie diesen vordefinierten Namen der Eingabe und Ausgabe Datensatz-IDs).

## <a name="embedding-r-code-in-the-u-sql-script"></a>Einbetten von R-Code in das U-SQL-Skript

Können Sie die R-code das U-SQL-Skript mit dem Befehlsparameter des Inline der `Extension.R.Reducer`. Beispielsweise können Sie das R-Skript als eine String-Variable zu deklarieren und als Parameter übergeben, um dem REDUCER an.


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>Halten Sie die R-Code in einer separaten Datei und verweisen das U-SQL-Skript zu

Das folgende Beispiel veranschaulicht die Verwendung ein komplexeres. In diesem Fall wird der R-Code als eine Ressource bereitgestellt, die das U-SQL-Skript ist.

Speichern Sie diese R-Code als separate Datei.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Verwenden Sie eine U-SQL-Skript, um mit der Anweisung Ressource bereitstellen, R-Skript bereitzustellen.

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a>Integration von R mit U-SQL

### <a name="datatypes"></a>Datentypen
* U-SQL-Zeichenfolgen und numerische Spalten werden als konvertiert-liegt zwischen R-Datenrahmen und U-SQL [unterstützte Typen: `double`, `string`, `bool`, `integer`, `byte`].
* Die `Factor` wird in der U-SQL nicht unterstützt.
* `byte[]`serialisiert werden muss, als base64-codierte `string`.
* U-SQL-Zeichenfolgen konvertiert werden können, zu Faktoren, die in R-Code nach dem Erstellen Eingabe R-Datenrahmen U-SQL oder durch Festlegen des Parameters reducerdatei an `stringsAsFactors: true`.

### <a name="schemas"></a>Schemas
* U-SQL-Datasets sind keine doppelten Spaltennamen.
* U-SQL-Datasets Spaltennamen müssen Zeichenfolgen sein.
* Spaltennamen müssen in U-SQL und R-Skripts identisch sein.
* ReadOnly-Spalte darf nicht Teil der Ausgabe Datenrahmen sein. Da Readonly Spalten automatisch wieder in der U-SQL-Tabelle eingefügt werden, wenn es ein Bestandteil des Schemas für die Ausgabe des UDO ist.

### <a name="functional-limitations"></a>Funktionale Beschränkungen
* Die R-Modul kann nicht zweimal in demselben Prozess instanziiert werden. 
* U-SQL unterstützt derzeit nicht UDOs kombinierungsfunktion (combiner) für die Vorhersage Verwenden von partitionierten Modellen mit reducerdatei an UDOs generiert. Benutzer können partitionierte Modelle als Ressource zu deklarieren und in ihre R-Skript zu verwenden (siehe Beispielcode `ExtR_PredictUsingLMRawStringReducer.usql`)

### <a name="r-versions"></a>R-Versionen
Es wird nur von R 3.2.2 unterstützt.

### <a name="standard-r-modules"></a>Standard-R-Module

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a>Eingabe und Ausgabe größenbeschränkungen
Jedem Scheitelpunkt verfügt über eine begrenzte Menge an Arbeitsspeicher zugewiesen. Da die Eingabe- und Ausgabespalten Data Frames im Arbeitsspeicher in der R-Code vorhanden sein muss, darf die Gesamtgröße für die Eingabe und Ausgabe 500 MB nicht überschreiten.

### <a name="sample-code"></a>Codebeispiel
Weitere Beispielcode ist in Ihrem Konto Data Lake-Speicher verfügbar, nach der Installation der U-SQL Advanced Analytics Extensions. Der Pfad für weitere Beispielcode ist: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>Bereitstellen von benutzerdefinierten R-Module mit U-SQL

Erstellen Sie zunächst ein benutzerdefiniertes R-Modul und komprimieren sie und klicken Sie dann hochladen Sie die ZIP-Datei des R-benutzerdefinierten Moduls am ADL-Speicher. Im Beispiel werden wir magittr_1.5.zip auf den Stamm der ADLS-Standardkonto für das Konto ADLA hochladen, das verwendet werden. Nachdem Sie das Modul in ADL Store hochladen, deklarieren Sie es als Ressource bereitstellen verwenden, um es in das U-SQL-Skript und rufen verfügbar zu machen `install.packages` , es zu installieren.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über die Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln Sie U-SQL-Skripts, die mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Mithilfe der U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)

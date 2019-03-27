---
title: 'Schnellstart: Verwenden von Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion) | Microsoft-Dokumentation'
description: In diesem Thema wird veranschaulicht, wie Sie Machine Learning Services in Azure SQL-Datenbank verwenden und R-Skripts ausführen, um bedarfsabhängig Analysen durchführen und die Berechnung und Verarbeitung an dem Ort bereitstellen zu können, an dem sich die Daten befinden. So ist es nicht mehr erforderlich, Daten per Pullvorgang über das Netzwerk zu übertragen.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.reviewer: ''
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: e15cf93514f921223fea37aa480730bba46dd195
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57864948"
---
# <a name="quickstart-use-machine-learning-services-with-r-in-azure-sql-database-preview"></a>Schnellstart: Verwenden von Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion)

In diesem Artikel wird beschrieben, wie Sie die öffentliche Vorschauversion von [Machine Learning Services (mit R) in Azure SQL-Datenbank](sql-database-machine-learning-services-overview.md) verwenden können. Die Grundlagen des Verschiebens von Daten zwischen einer SQL-Datenbank und R werden ausführlich erläutert. Außerdem wird beschrieben, wie Sie wohlgeformten R-Code in der gespeicherten Prozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) umschließen, um Machine Learning-Modelle in einer SQL-Datenbank zu erstellen, zu trainieren und zu verwenden.

Nutzen Sie die Möglichkeiten der Sprache R für erweiterte Analysen und maschinelles Lernen in der Datenbank. Diese Fähigkeit ermöglicht Berechnungen und die Verarbeitung an dem Ort, an dem sich die Daten befinden, sodass diese nicht mehr über das Netzwerk gepullt werden müssen. Nutzen Sie die Leistungsfähigkeit der R-Pakete für Unternehmen außerdem, um erweiterte Analysen in einem großen Maßstab durchzuführen.

Machine Learning Services umfasst eine Basisdistribution von R zusammen mit R-Paketen für Unternehmen von Microsoft. Die R-Funktionen und -Algorithmen von Microsoft sind auf Skalierbarkeit und Anwendbarkeit ausgelegt. Sie ermöglichen Predictive Analytics, statistische Modellierung, Datenvisualisierungen und leistungsstarke Algorithmen für maschinelles Lernen.

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/), bevor Sie beginnen.

> [!IMPORTANT]
> Machine Learning Services (mit R) in Azure SQL-Datenbank befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> [Registrieren für die Vorschauversion](sql-database-machine-learning-services-overview.md#signup)

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des Beispielcodes in diesen Übungen müssen Sie zuerst eine SQL-Datenbank mit Machine Learning Services (mit R) aktivieren. Während der öffentlichen Vorschauphase führt Microsoft für Sie das Onboarding durch und aktiviert das maschinelle Lernen für Ihre vorhandene oder neue Datenbank. Führen Sie die Schritte in [Registrieren für die Vorschauversion](sql-database-machine-learning-services-overview.md#signup) aus.

Sie können eine Verbindung mit der SQL-Datenbank herstellen und die R-Skripts mit einem beliebigen Tool für die Datenbankverwaltung oder für Abfragen ausführen, solange damit eine Verbindung mit einer SQL-Datenbank hergestellt werden kann, und eine T-SQL-Abfrage oder gespeicherte Prozedur ausführen. In dieser Schnellstartanleitung wird [SQL Server Management Studio](sql-database-connect-query-ssms.md) verwendet.

Für die Übung [Hinzufügen eines Pakets](#add-package) müssen Sie auch [R](https://www.r-project.org/) und [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) auf Ihrem lokalen Computer installieren.

Diese Schnellstartanleitung setzt auch voraus, dass Sie eine Firewallregel auf Serverebene konfigurieren. Ein Schnellstartanleitung für die Vorgehensweise finden Sie unter [Erstellen von Firewallregeln auf Serverebene Regel](sql-database-server-level-firewall-rule.md).

## <a name="verify-r-exists"></a>Überprüfen des Vorhandenseins von R

Sie können sich vergewissern, dass Machine Learning Services (mit R) für Ihre SQL-Datenbank aktiviert ist. Führen Sie die folgenden Schritte aus.

1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit Ihrer SQL-Datenbank her. Weitere Informationen zum Herstellen der Verbindung finden Sie unter [Schnellstart: Verwenden von SQL Server Management Studio zum Herstellen der Verbindung mit einer Instanz von Azure SQL-Datenbank und deren Abfrage](sql-database-connect-query-ssms.md).

1. Führen Sie den unten angegebenen Code aus. 

    ```sql
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(31 + 11)';
    GO
    ```
    Wenn alles funktioniert, sollte eine Ergebnismeldung wie hier angegeben angezeigt werden.

    ```text
    STDOUT message(s) from external script: 
    42
    ```

1. Falls Sie Fehler erhalten, kann dies daran liegen, dass die öffentliche Vorschauversion von Machine Learning Services (mit R) für Ihre SQL-Datenbank nicht aktiviert ist. Oben finden Sie die Informationen dazu, wie Sie sich für die öffentliche Vorschauversion registrieren.

## <a name="grant-permissions"></a>Erteilen von Berechtigungen

Wenn Sie Administrator sind, können Sie externen Code automatisch ausführen. Allen anderen Benutzern müssen die entsprechenden Berechtigungen gewährt werden.

Ersetzen Sie `<username>` durch eine gültige Datenbankbenutzeranmeldung, bevor Sie den Befehl ausführen.

```sql
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

## <a name="basic-r-interaction"></a>Grundlegende R-Interaktion

Es gibt zwei Möglichkeiten, wie Sie R-Code in SQL-Datenbank ausführen können:

+ Fügen Sie ein R-Skript als Argument der gespeicherten Systemprozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) hinzu.
+ Stellen Sie über einen [R-Remoteclient](https://docs.microsoft.com/sql/advanced-analytics/r/set-up-a-data-science-client) eine Verbindung mit Ihrer SQL-Datenbank her, und führen Sie Code aus, indem Sie die SQL-Datenbank als Computekontext verwenden.

In der folgenden Übung geht es um das erste Interaktionsmodell: Übergeben von R-Code an eine gespeicherte Prozedur.

1. Führen Sie ein einfaches Skript aus, um zu ermitteln, wie ein R-Skript in Ihrer SQL-Datenbank ausgeführt wird.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R',
    @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c, d)'
    ```

2. Wenn Sie alles richtig eingerichtet haben, wird das richtige Ergebnis berechnet, und die R-Funktion `print` gibt das Ergebnis im Bereich mit den **Meldungen** zurück.

    **Ergebnisse**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```

    Das Abrufen von **stdout**-Meldungen ist nützlich, wenn Sie Ihren Code testen. Häufiger müssen Sie die Ergebnisse aber im Tabellenformat zurückgeben, damit Sie die Daten in einer Anwendung nutzen oder in eine Tabelle schreiben können. Weitere Informationen finden Sie im Abschnitt zu den Ein- und Ausgaben.

Beachten Sie, dass das `@script`-Argument nur gültigen R-Code enthalten darf.

## <a name="inputs-and-outputs"></a>Eingaben und Ausgaben

Standardmäßig akzeptiert [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) ein einzelnes Eingabedataset, das Sie normalerweise in Form einer gültigen SQL-Abfrage bereitstellen. Andere Arten von Eingaben können als SQL-Variablen übergeben werden.

Die gespeicherte Prozedur gibt einen einzelnen R-Datenrahmen als Ausgabe zurück, aber Sie können auch Skalare und Modelle als Variablen ausgeben. Beispielsweise können Sie ein trainiertes Modell als binäre Variable ausgeben und sie an eine INSERT-T-SQL-Anweisung übergeben, um dieses Modell in eine Tabelle zu schreiben. Sie können auch Plots (im binären Format) oder Skalare (einzelne Werte, z.B. das Datum und die Uhrzeit, die für das Trainieren des Modells verstrichene Zeit usw.) generieren.

Wir sehen uns hier jetzt nur die Standardvariablen für die Ein- und Ausgabe von „sp_execute_external_script“ an: `InputDataSet` und `OutputDataSet`.

1. Erstellen Sie eine kleine Tabelle mit Testdaten, indem Sie die folgende T-SQL-Anweisung ausführen:

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    INSERT INTO RTestData VALUES (1);
    INSERT INTO RTestData VALUES (10);
    INSERT INTO RTestData VALUES (100);
    GO
    ```

    Verwenden Sie nach dem Erstellen der Tabelle die folgende Anweisung, um die Tabelle abzufragen:
  
    ```sql
    SELECT * FROM RTestData
    ```

    **Ergebnisse**

    ![Inhalt der RTestData-Tabelle](./media/sql-database-connect-query-r/select-rtestdata.png)

2. Sie können die Daten der Tabelle als Eingabe für Ihr R-Skript abrufen. Führen Sie die unten angegebene Anweisung aus. Die Daten werden aus der Tabelle abgerufen, es wird ein Roundtrip durch die R-Runtime durchgeführt, und dann werden die Werte mit dem Spaltennamen *NewColName* zurückgegeben.

    Die von der Abfrage zurückgegebenen Daten werden an die R-Runtime übergeben, von der die Daten als Datenrahmen an SQL-Datenbank zurückgegeben werden. Mit der WITH RESULT SETS-Klausel wird das Schema der zurückgegebenen Datentabelle für SQL-Datenbank definiert.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    **Ergebnisse**

    ![Ausgabe des R-Skripts, mit dem Daten aus einer Tabelle zurückgegeben werden](./media/sql-database-connect-query-r/r-output-rtestdata.png)

3. Wir ändern jetzt den Namen der Eingabe- bzw. Ausgabevariablen. Im obigen Skript werden die Standardnamen der Ein- und Ausgabevariablen verwendet: _InputDataSet_ und _OutputDataSet_. Zum Definieren der Eingabedaten, die _InputDatSet_ zugeordnet sind, verwenden Sie die Variable *\@input_data_1*.

    In diesem Skript wurden die Namen der Aus- und Eingabevariablen für die gespeicherte Prozedur in *SQL_out* und *SQL_in* geändert:

    ```sql
    EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N' SQL_out <- SQL_in;'
      , @input_data_1 = N' SELECT 12 as Col;'
      , @input_data_1_name  = N'SQL_in'
      , @output_data_1_name =  N'SQL_out'
      WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    Beachten Sie, dass für R die Groß-/Kleinschreibung beachtet wird. Die Schreibweise der Ein- und Ausgabevariablen in `@input_data_1_name` und `@output_data_1_name` muss also mit der Schreibweise im R-Code in `@script` übereinstimmen. 

    Darüber hinaus ist die Reihenfolge der Parameter wichtig. Sie müssen zunächst die erforderlichen Parameter *\@input_data_1* und *\@output_data_1* angeben, um die optionalen Parameter *\@ input_data_1_name* und *\@output_data_1_name* verwenden zu können.

    Nur ein Eingabedataset kann als Parameter übergeben werden, und Sie können nur ein Dataset zurückgeben. Sie können aber andere Datasets in Ihrem R-Code aufrufen und zusätzlich zum Dataset Ausgaben anderer Typen zurückgeben. Außerdem können Sie auch allen Parametern das Schlüsselwort OUTPUT hinzufügen, damit es zusammen mit den Ergebnissen zurückgegeben wird. 

    Mit der Anweisung `WITH RESULT SETS` wird das Schema für die Daten definiert, die in SQL-Datenbank verwendet werden. Sie müssen für jede Spalte, die Sie über R zurückgeben, SQL-kompatible Datentypen angeben. Sie können die Schemadefinition verwenden, um auch neue Spaltennamen anzugeben. Es ist nicht unbedingt erforderlich, dass Sie die Spaltennamen aus dem R-Datenrahmen nutzen.

4. Sie können Werte auch generieren, indem Sie das R-Skript verwenden und die Eingabe-Abfragezeichenfolge in _@input_data_1_ leer lassen.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N' mytextvariable <- c("hello", " ", "world");
            OutputDataSet <- as.data.frame(mytextvariable);'
        , @input_data_1 = N''
    WITH RESULT SETS (([Col1] CHAR(20) NOT NULL));
    ```

    **Ergebnisse**

    ![Abfrageergebnisse mit @script als Eingabe](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>Überprüfen der R-Version

Gehen Sie wie folgt vor, wenn Sie ermitteln möchten, welche Version von R in Ihrer SQL-Datenbank installiert ist:

1. Führen Sie das unten angegebene Skript in Ihrer SQL-Datenbank aus.

    ```SQL
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(version)';
    GO
    ```

2. Mit der R-Funktion `print` wird die Version im Bereich mit den **Meldungen** zurückgegeben. In der Beispielausgabe unten sehen Sie, dass für SQL-Datenbank in diesem Fall die R-Version 3.4.4 installiert ist.

    **Ergebnisse**

    ```text
    STDOUT message(s) from external script:
                   _
    platform       x86_64-w64-mingw32
    arch           x86_64
    os             mingw32
    system         x86_64, mingw32
    status
    major          3
    minor          4.4
    year           2018
    month          03
    day            15
    svn rev        74408
    language       R
    version.string R version 3.4.4 (2018-03-15)
    nickname       Someone to Lean On
    ```

## <a name="list-r-packages"></a>Liste mit R-Paketen

Microsoft stellt verschiedene R-Pakete bereit, die mit Machine Learning Services in Ihrer SQL-Datenbank vorinstalliert werden. Führen Sie die unten angegebenen Schritte aus, um eine Liste mit den Informationen anzuzeigen, welche R-Pakete installiert sind, einschließlich Version, Abhängigkeiten, Lizenz und Bibliothekspfad. Informationen zum Hinzufügen weiterer Pakete finden Sie im Abschnitt [Hinzufügen eines Pakets](#add-package).

1. Führen Sie das unten angegebene Skript in Ihrer SQL-Datenbank aus.

    ```SQL
    EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
    OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
    WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
        , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
    ```

2. Die Ausgabe stammt aus `installed.packages()` in R und wird als Resultset zurückgegeben.

    **Ergebnisse**

    ![Installierte Pakete in R](./media/sql-database-connect-query-r/r-installed-packages.png)

## <a name="create-a-predictive-model"></a>Erstellen eines Vorhersagemodells

Sie können ein Modell mit R trainieren und das Modell in Ihrer SQL-Datenbank in einer Tabelle speichern. In dieser Übung trainieren Sie ein einfaches Regressionsmodell, mit dem der Anhalteweg eines PKW anhand der Geschwindigkeit vorhergesagt wird. Sie nutzen das in R vorhandene Dataset `cars`, weil es klein und leicht verständlich ist.

1. Erstellen Sie zuerst eine Tabelle, um die Trainingsdaten zu speichern.

    ```sql
    CREATE TABLE dbo.CarSpeed (speed INT NOT NULL, distance INT NOT NULL)
    GO
    INSERT INTO dbo.CarSpeed (speed, distance)
    EXEC sp_execute_external_script
    @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

    In der R-Runtime sind viele kleine und große Datasets enthalten. Geben Sie zum Abrufen einer Liste mit Datasets, die mit R installiert werden, an einer R-Eingabeaufforderung `library(help="datasets")` ein.

2. Erstellen Sie ein Regressionsmodell. Die Daten zur PKW-Geschwindigkeit enthalten zwei Spalten, die beide numerischer Art sind: `dist` und `speed`. Es sind mehrere Beobachtungen von Geschwindigkeiten vorhanden. Aus diesen Daten erstellen Sie ein lineares Regressionsmodell, mit dem die Beziehung zwischen der PKW-Geschwindigkeit und dem benötigten Anhalteweg des PKW beschrieben wird.

    Die Anforderungen eines linearen Modells sind einfach:

   - Definieren Sie eine Formel, mit der die Beziehung zwischen der abhängigen Variablen `speed` und der unabhängigen Variablen `distance` beschrieben wird.

   - Stellen Sie die Eingabedaten für die Verwendung beim Trainieren des Modells bereit.

     > [!TIP]
     > Falls Sie eine Auffrischung zu linearen Modellen benötigen, empfehlen wir Ihnen das folgende Tutorial, in dem das Anpassen eines Modells mit rxLinMod beschrieben wird: [Fitting Linear Models](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model) (Anpassen von linearen Modellen).

     Zum Erstellen des Modells definieren Sie die Formel in Ihrem R-Code und übergeben die Daten als Eingabeparameter.

     ```sql
     DROP PROCEDURE IF EXISTS generate_linear_model;
     GO
     CREATE PROCEDURE generate_linear_model
     AS
     BEGIN
       EXEC sp_execute_external_script
       @language = N'R'
       , @script = N'lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
           trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));'
       , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
       , @input_data_1_name = N'CarsData'
       , @output_data_1_name = N'trained_model'
       WITH RESULT SETS ((model VARBINARY(max)));
     END;
     GO
     ```

     Das erste Argument für rxLinMod ist der Parameter *formula*, mit dem der Anhalteweg in Abhängigkeit der Geschwindigkeit definiert wird. Die Eingabedaten werden in der Variablen `CarsData` gespeichert, die mit der SQL-Abfrage aufgefüllt wird. Wenn Sie Ihren Eingabedaten nicht einen spezifischen Namen zuweisen, lautet der Standardvariablenname _InputDataSet_.

2. Erstellen Sie als Nächstes eine Tabelle, in der Sie das Modell speichern, damit Sie es erneut trainieren oder für die Vorhersage verwenden können. Die Ausgabe eines R-Pakets, mit dem ein Modell erstellt wird, ist normalerweise ein **binäres Objekt**. Aus diesem Grund muss in der Tabelle eine Spalte vom Typ **VARBINARY(max)** enthalten sein.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
    );
    ```
3. Führen Sie zum Speichern des Modells die folgende Transact-SQL-Anweisung aus, um die gespeicherte Prozedur aufzurufen und das Modell zu generieren und in einer Tabelle zu speichern.

    ```sql
    INSERT INTO dbo.stopping_distance_models (model)
    EXEC generate_linear_model;
    ```

    Beachten Sie, dass Sie den folgenden Fehler erhalten, wenn Sie diesen Code ein zweites Mal ausführen:

    ```text
    Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object dbo.stopping_distance_models
    ```

    Eine Option zur Vermeidung dieses Fehlers ist das Aktualisieren des Namens für jedes neue Modell. Sie können den Namen beispielsweise in eine aussagekräftigere Zeichenfolge ändern und den Modelltyp, den Tag der Erstellung usw. einfügen.

    ```sql
    UPDATE dbo.stopping_distance_models
    SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
    WHERE model_name = 'default model'
    ```

4. Normalerweise ist die R-Ausgabe der gespeicherten Prozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) auf einen einzelnen Datenrahmen beschränkt.

    Sie können aber auch andere Ausgaben, z.B. Skalare, zusätzlich zum Datenrahmen zurückgeben.

    Angenommen, Sie möchten ein Modell trainieren, aber dann sofort eine Tabelle mit Koeffizienten des Modells anzeigen. Sie sollten die Tabelle mit den Koeffizienten als Hauptresultset erstellen und das trainierte Modell in einer SQL-Variablen ausgeben. Sie können das Modell sofort wiederverwenden, indem Sie die Variable aufrufen, oder Sie können das Modell wie hier gezeigt in einer Tabelle speichern.

    ```sql
    DECLARE @model VARBINARY(max), @modelname VARCHAR(30)
    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
            speedmodel <- rxLinMod(distance ~ speed, CarsData)
            modelbin <- serialize(speedmodel, NULL)
            OutputDataSet <- data.frame(coefficients(speedmodel));'
        , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
        , @input_data_1_name = N'CarsData'
        , @params = N'@modelbin varbinary(max) OUTPUT'
        , @modelbin = @model OUTPUT
        WITH RESULT SETS (([Coefficient] FLOAT NOT NULL));

    -- Save the generated model
    INSERT INTO dbo.stopping_distance_models(model_name, model)
    VALUES ('latest model', @model)
    ```

    **Ergebnisse**

    ![Trainiertes Modell mit zusätzlicher Ausgabe](./media/sql-database-connect-query-r/r-train-model-with-additional-output.png)

## <a name="predict"></a>Vorhersage

Verwenden Sie das Modell, das Sie im vorherigen Abschnitt erstellt haben, um Vorhersagen für neue Daten zu bewerten. Rufen Sie zum Durchführen der _Bewertung_ mit neuen Daten eines der trainierten Modelle aus der Tabelle und dann einen neuen Satz mit Daten auf, auf dem die Vorhersagen basieren sollen. „Bewertung“ ist ein Begriff, der manchmal im Data Science-Bereich verwendet wird, um das Generieren von Vorhersagen, Wahrscheinlichkeiten oder anderen Werten basierend auf neuen Daten zu bezeichnen, die in ein trainiertes Modell einfließen.

1. Erstellen Sie zunächst eine Tabelle mit neuen Daten zur Geschwindigkeit. Haben Sie bemerkt, dass die ursprünglichen Trainingsdaten nur bis 25 Meilen pro Stunde reichen? Der Grund ist, dass diese Originaldaten auf einem Experiment aus dem Jahr 1920 basieren! Sie fragen sich vielleicht, wie lange es dauert, bis ein PKW aus den Zwanziger Jahren zum Stehen kommt, wenn er mit einer Geschwindigkeit von 60 oder sogar 100 Meilen pro Stunde fahren würde. Um diese Frage zu beantworten, müssen Sie einige neue Geschwindigkeitswerte angeben.

    ```sql
    CREATE TABLE dbo.NewCarSpeed(speed INT NOT NULL,
        distance INT NULL
    )
    GO
    INSERT dbo.NewCarSpeed(speed)
    VALUES (40), (50), (60), (70), (80), (90), (100)
    ```

    In diesem Beispiel rufen Sie nicht die generische R-Funktion `predict` auf, sondern die Funktion [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict), weil Ihr Modell auf dem **rxLinMod**-Algorithmus basiert, der mit dem **RevoScaleR**-Paket bereitgestellt wird.

    ```sql
    DECLARE @speedmodel varbinary(max) = 
        (SELECT model FROM dbo.stopping_distance_models WHERE model_name = 'latest model');

    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
                current_model <- unserialize(as.raw(speedmodel));
                new <- data.frame(NewCarData);
                predicted.distance <- rxPredict(current_model, new);
                str(predicted.distance);
                OutputDataSet <- cbind(new, ceiling(predicted.distance));
                '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS ((new_speed INT, predicted_distance INT));
    ```

    Mit dem obigen Skript werden die folgenden Schritte ausgeführt:

   + Verwenden Sie eine SELECT-Anweisung, um ein einzelnes Modell aus der Tabelle abzurufen, und übergeben Sie es als Eingabeparameter.

   + Rufen Sie nach dem Abruf des Modells aus der Tabelle die Funktion `unserialize` für das Modell auf.

       > [!TIP] 
       > Sehen Sie sich auch die neuen [Serialisierungsfunktionen](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) von RevoScaleR an, für die die Bewertung in Echtzeit unterstützt wird.
   + Wenden Sie die Funktion `rxPredict` mit den passenden Argumenten auf das Modell an, und geben Sie die neuen Eingabedaten an.

   + Im Beispiel wird die Funktion `str` während der Testphase hinzugefügt, um das Schema der Daten zu überprüfen, die von R zurückgegeben werden. Sie können die Anweisung dann später entfernen.

   + Die im R-Skript genutzten Spaltennamen werden nicht unbedingt an die Ausgabe der gespeicherten Prozedur übergeben. Hier haben wir die WITH RESULTS-Klausel verwendet, um einige neue Spaltennamen zu definieren.

     **Ergebnisse**

     ![Resultset für die Vorhersage des Anhaltewegs](./media/sql-database-connect-query-r/r-predict-stopping-distance-resultset.png)

     Es ist auch möglich, [PREDICT in Transact-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) zu verwenden, um einen vorhergesagten Wert oder eine Bewertung basierend auf einem gespeicherten Modell zu generieren.

<a name="add-package"></a>

## <a name="add-a-package"></a>Hinzufügen eines Pakets

Wenn Sie ein Paket verwenden müssen, das nicht bereits in Ihrer SQL-Datenbank installiert ist, können Sie es per [sqlmlutils](https://github.com/Microsoft/sqlmlutils) installieren. Führen Sie die unten angegebenen Schritte aus, um das Paket zu installieren.

1. Laden Sie die aktuelle **sqlmlutils**-ZIP-Datei von [github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) auf Ihren lokalen Computer herunter. Sie müssen die Datei nicht entzippen.

1. Wenn Sie R nicht installiert haben, können Sie R unter [www.r-project.org](https://www.r-project.org/) herunterladen und auf Ihrem lokalen Computer installieren. R ist für Windows, macOS und Linux verfügbar. In diesem Beispiel verwenden wir Windows.

1. Installieren Sie zuerst das Paket **RODBCext**, das eine Voraussetzung für **sqlmlutils** ist. Mit **RODBCext** wird auch die Abhängigkeit für das **RODBC**-Paket installiert. Öffnen Sie eine **Eingabeaufforderung**, und führen Sie den folgenden Befehl aus:

    ```
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    ```

    Wenn Sie den Fehler „'R' wird nicht als interner oder externer Befehl, ausführbares Programm oder Batchdatei erkannt“ erhalten, bedeutet dies normalerweise, dass der Pfad zu „R.exe“ unter Windows nicht in Ihrer Umgebungsvariablen **PATH** enthalten ist. Sie können vor dem Ausführen des Befehls das Verzeichnis entweder der Umgebungsvariablen hinzufügen oder über die Eingabeaufforderung zum Verzeichnis navigieren (z.B. `cd C:\Program Files\R\R-3.5.1\bin`).

1. Verwenden Sie den Befehl **R CMD INSTALL**, um **sqlmlutils** zu installieren. Geben Sie den Pfad zum Verzeichnis, in das Sie die ZIP-Datei heruntergeladen haben, und den Namen der ZIP-Datei an. Beispiel: 

    ```
    R CMD INSTALL C:\Users\youruser\Downloads\sqlmlutils_0.5.0.zip
    ```

    Die Ausgabe sollte in etwa wie folgt aussehen:

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/youruser/Documents/R/win-library/3.5'
    package 'sqlmlutils' successfully unpacked and MD5 sums checked
    ```

1. In diesem Beispiel verwenden Sie RStudio Desktop als IDE. Sie können aber auch eine andere IDE nutzen, falls Sie dies vorziehen. Laden Sie RStudio Desktop von [www.rstudio.com/products/rstudio/download/](https://www.rstudio.com/products/rstudio/download/) herunter, und installieren Sie die Anwendung, falls Sie noch nicht über eine RStudio-Installation verfügen.

1. Öffnen Sie **RStudio**, und erstellen Sie eine neue **R-Skript**-Datei. Verwenden Sie den folgenden R-Code, um ein Paket mit sqlmlutils zu installieren. Im Beispiel unten installieren Sie das [glue](https://cran.r-project.org/web/packages/glue/)-Paket, mit dem eine Zeichenfolge formatiert und interpoliert werden kann.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!NOTE]
    > Der **Bereich** kann entweder **ÖFFENTLICH** oder **PRIVAT** sein. Der öffentliche Bereich ist für den Datenbankadministrator nützlich, um Pakete zu installieren, die von allen Benutzern verwendet werden können. Beim privaten Bereich ist das Paket nur für den Benutzer verfügbar, der es installiert. Wenn Sie den Bereich nicht angeben, wird standardmäßig **PRIVAT** verwendet.

1. Überprüfen Sie als Nächstes, ob das **glue**-Paket installiert wurde.

    ```R
    r<-sql_installed.packages(connectionString = connection, fields=c("Package", "LibPath", "Attributes", "Scope"))
    View(r)
    ```

    **Ergebnisse**

    ![Inhalt der RTestData-Tabelle](./media/sql-database-connect-query-r/r-verify-package-install.png)


1. Nachdem das Paket installiert wurde, können Sie es in Ihrem R-Skript mit **sp_execute_external_script** verwenden. Öffnen Sie **SQL Server Management Studio**, und stellen Sie eine Verbindung mit Ihrer SQL-Datenbank her. Führen Sie das folgende Skript aus:

    ```sql
    EXEC sp_execute_external_script @language = N'R'
    , @script = N'
    library(glue)

    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
        ''my age next year is {age + 1}, '',
        ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    print(text)
    ';
    ```

    Auf der Registerkarte mit den Meldungen wird das folgende Ergebnis angezeigt.

    **Ergebnisse**

    ```text
    STDOUT message(s) from external script:
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

1. Führen Sie auf Ihrem lokalen Computer in **RStudio** das folgende R-Skript aus, wenn Sie das Paket entfernen möchten.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC") 
    ```

> [!NOTE]
> Eine weitere Möglichkeit zum Installieren von R-Paketen für Ihre SQL-Datenbank ist das Hochladen des R-Pakets aus dem Bytedatenstrom mit [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Machine Learning Services finden Sie in den folgenden Artikeln. Einiger dieser Artikel sind zwar für SQL Server bestimmt, aber die meisten Informationen gelten auch für Machine Learning Services (mit R) in Azure SQL-Datenbank.

- [Machine Learning Services (mit R) in Azure SQL-Datenbank](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Tutorial: Learn in-database analytics using R in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers) (Datenbankinterne Analysen mit R in SQL Server)
- [End-to-end data science walkthrough for R and SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) (Lückenlose exemplarische Vorgehensweise für Data Science für R und SQL Server)
- [Tutorial: Use RevoScaleR R functions with SQL Server data](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages) (Verwenden von RevoScaleR-R-Funktionen mit SQL Server-Daten)

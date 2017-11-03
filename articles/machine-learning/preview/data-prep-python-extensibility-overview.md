---
title: Verwenden der Python-Erweiterbarkeit mit der Azure Machine Learning-Datenvorbereitung | Microsoft Docs
description: "Dieses Dokument bietet eine Übersicht und einige detaillierte Beispiele, wie Sie Python-Code verwenden, um die Funktionen der Datenvorbereitung zu erweitern."
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
ms.date: 09/07/2017
ms.openlocfilehash: 53771c407fedc53f27a38ec3fe9b381d6b8c0dad
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="data-preparations-python-extensions"></a>Python-Erweiterungen für die Datenvorbereitung
Die Azure Machine Learning-Datenvorbereitung enthält Erweiterbarkeit auf mehreren Ebenen, um Funktionslücken zwischen integrierten Funktionen zu füllen. In diesem Dokument wird die Erweiterbarkeit mithilfe des Python-Skripts erläutert. 

## <a name="custom-code-steps"></a>Benutzerdefinierte Codeschritte 
Die Datenvorbereitung verfügt über folgende benutzerdefinierte Schritte, mit denen Sie Benutzercode schreiben können:

* Dateileser*
* Writer*
* Spalte hinzufügen
* Erweiterter Filter
* Datenfluss transformieren
* Partition transformieren

*Diese Schritte werden aktuell nicht in einer Spark-Ausführung unterstützt.

## <a name="code-block-types"></a>Codeblocktypen 
Wir unterstützen für jeden dieser Schritte zwei Codeblocktypen. Zum einen unterstützen wir einen bloßen Python-Ausdruck, der unverändert ausgeführt wird. Und zum anderen unterstützen wir ein Python-Modul, in dem wir eine bestimmte Funktion mit einer bekannten Signatur in dem von Ihnen angegebenen Code aufrufen.

Sie können z.B. eine neue Spalte hinzufügen, die das Protokoll einer anderen Spalte auf die folgenden zwei Arten berechnet:

Ausdruck 

```python    
    math.log(row["Score"])
```

Modul 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


Bei der Transformation „Spalte hinzufügen“ im Modulmodus wird davon ausgegangen, dass eine Funktion mit dem Namen `newvalue` gefunden wird, die eine Zeilenvariable akzeptiert und den Wert für die Spalte zurückgibt. Dieses Modul kann eine beliebige Anzahl von Python-Code mit anderen Funktionen, Importen usw. enthalten.

Die Details zu den einzelnen Erweiterungspunkten werden in den folgenden Abschnitten erläutert. 

## <a name="imports"></a>Importe 
Wenn Sie den Ausdrucksblocktyp verwenden, können Sie Ihrem Code noch immer **import**-Anweisungen hinzufügen. Diese müssen alle in der oberen Zeilen Ihres Codes gruppiert werden.

Richtig 

```python
import math 
import numpy 
math.log(row["Score"])
```
 

Falsch  

```python
import math  
math.log(row["Score"])  
import numpy
```
 
 
Wenn Sie den Modulblocktyp verwenden, können Sie bei der Verwendung der **import**-Anweisung den normalen Python-Regeln folgen. 

## <a name="default-imports"></a>Standardimporte
Die folgenden Importe sind immer enthalten und in Ihrem Code verwendbar. Sie müssen diese nicht erneut importieren. 

```python
import math  
import numbers  
import datetime  
import re  
import pandas as pd  
import numpy as np  
import scipy as sp
```
  

## <a name="install-new-packages"></a>Installieren neuer Pakete
Wenn Sie ein Paket verwenden möchten, das nicht standardmäßig installiert ist, müssen Sie es zunächst in den Umgebungen installieren, die die Datenvorbereitung verwenden. Diese Installation muss auf Ihrem lokalen Computer und in allen Computezielen erfolgen, in denen die Ausführung erfolgen soll.

Sie müssen die Datei „conda_dependencies.yml“ im Ordner „aml_config“ im Stamm Ihres Projekts ändern, um Ihre Pakete in einem Computeziel zu installieren.

### <a name="windows"></a>Windows 
Sie finden den Speicherort unter Windows, indem Sie nach der App-spezifischen Installation von Python und dem zugehörigen Skriptverzeichnis suchen. Dies ist der Standardspeicherort:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts.` 

Führen Sie anschließend einen der folgenden Befehlen aus: 

`conda install <libraryname>` 

oder 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
Sie finden den Speicherort auf einem Mac, indem Sie nach der App-spezifischen Installation von Python und dem zugehörigen Skriptverzeichnis suchen. Dies ist der Standardspeicherort: 

`/Users/<user>/Library/Caches/AmlWorkbench>/Python/bin` 

Führen Sie anschließend einen der folgenden Befehlen aus: 

`./conda install <libraryname>`

oder 

`./pip install <libraryname>`

## <a name="column-data"></a>Spaltendaten 
Auf Spaltendaten kann aus einer Zeile zugegriffen werden, indem die Punktnotation oder die Schlüssel-Wert-Notation verwendet wird. Auf Spaltennamen, die Leerzeichen oder Sonderzeichen enthalten, kann nicht mithilfe der Punktnotation zugegriffen werden. Die Variable `row` sollte immer in beiden Modi der Python-Erweiterungen (Modul und Ausdruck) definiert sein. 

Beispiele 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="file-reader"></a>Dateileser 
### <a name="purpose"></a>Zweck 
Mit dem Dateileser-Erweiterungspunkt können Sie den Prozess des Lesens einer Datei in einen Datenfluss vollständig steuern. Das System ruft Ihren Code auf und übergibt die Liste der Dateien, die verarbeitet werden sollen. Ihr Code muss einen Pandas-Datenrahmen erstellen und zurückgeben. 

>[!NOTE]
>Dieser Erweiterungspunkt funktioniert in Spark nicht. 


### <a name="how-to-use"></a>Verwendung 
Sie greifen über den Assistenten zum **Öffnen der Datenquelle** auf diesen Erweiterungspunkt zu. Wählen Sie **Datei**  auf der ersten Seite und anschließend den Dateispeicherort aus. Wählen Sie auf der Seite **Dateiparameter auswählen** in der Dropdownliste **Dateityp** die Option **Benutzerdefinierte Datei (Skript)** aus. 

Ihr Code erhält einen Pandas-Datenrahmen mit dem Namen „df“, der Informationen zu den Dateien enthält, die Sie lesen müssen. Wenn Sie ein Verzeichnis mit mehreren Dateien öffnen, enthält der Datenrahmen mehrere Zeilen.  

Dieser Datenrahmen verfügt über die folgenden Spalten:

- „Path“: Die zu lesende Datei.
- „PathHint“: Der Speicherort der Datei. Werte: „Local“, „AzureBlobStorage“ und „AzureDataLakeStorage“.
- „AuthenticationType“: Der Typ der Authentifizierung, der für den Zugriff auf die Datei verwendet wird. Werte: „None“, „SasToken“ und „OAuthToken“.
- „AuthenticationValue“: Enthält „None“ oder das zu verwendende Token.

### <a name="syntax"></a>Syntax 
Ausdruck 

```python
    paths = df['Path'].tolist()  
    df = pd.read_csv(paths[0])
```


Modul  
```python
PathHint = Local  
def read(df):  
    paths = df['Path'].tolist()  
    filedf = pd.read_csv(paths[0])  
    return filedf  
```
 

## <a name="writer"></a>Writer 
### <a name="purpose"></a>Zweck 
Mit dem Writer-Erweiterungspunkt können Sie den Prozess des Schreibens von Daten aus einem Datenfluss vollständig steuern. Das System ruft Ihren Code ab und übergibt einen Datenrahmen. Ihr Code kann den Datenrahmen zum beliebigen Schreiben von Daten verwenden. 

>[!NOTE]
>Der Writer-Erweiterungspunkt funktioniert in Spark nicht.


### <a name="how-to-use"></a>Verwendung 
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Datenfluss schreiben (Skript)“ hinzufügen. Er ist im Menü **Transformationen** der obersten Ebene verfügbar.

### <a name="syntax"></a>Syntax 
Ausdruck

```python
    df.to_csv('c:\\temp\\output.csv')
```

Modul

```python
def write(df):  
    df.to_csv('c:\\temp\\output.csv')  
    return df
```
 
 
Dieser benutzerdefinierte Schreibblock kann in der Mitte einer Liste von Schritten vorhanden sein. Wenn Sie ein Modul verwenden, muss die write-Funktion den Datenrahmen zurückgeben, der die Eingabe für den folgenden Schritt darstellt. 

## <a name="add-column"></a>Spalte hinzufügen 
### <a name="purpose"></a>Zweck
Mit dem Erweiterungspunkt „Spalte hinzufügen“ können Sie Python-Code schreiben, der eine neue Spalte berechnet. Der von Ihnen geschriebene Code kann auf die ganze Zeile zugreifen. Er muss einen neuen Spaltenwert für jede Zeile zurückgeben. 

### <a name="how-to-use"></a>Verwendung
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Spalte hinzufügen (Skript)“ hinzufügen. Er ist im Menü **Transformationen** der obersten Ebene sowie im Kontextmenü **Spalte** verfügbar. 

### <a name="syntax"></a>Syntax
Ausdruck

```python
    math.log(row["Score"])
```

Modul 

```python
def newvalue(row):  
     return math.log(row["Score"])
```
 

## <a name="advanced-filter"></a>Erweiterter Filter
### <a name="purpose"></a>Zweck 
Mit dem Erweiterungspunkt „Erweiterter Filter“ können Sie einen benutzerdefinierten Filter schreiben. Sie besitzen Zugriff auf die gesamte Zeile, und Ihr Code muss TRUE (Einschließen der Zeile) oder FALSE (Ausschließen der Zeile) zurückgeben. 

### <a name="how-to-use"></a>Verwendung
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Erweiterter Filter (Skript)“ hinzufügen. Er ist im Menü **Transformationen** der obersten Ebene verfügbar. 

### <a name="syntax"></a>Syntax

Ausdruck

```python
    row["Score"] > 95
```

Modul  

```python
def includerow(row):  
    return row["Score"] > 95
```
 

## <a name="transform-dataflow"></a>Datenfluss transformieren
### <a name="purpose"></a>Zweck 
Mit dem Erweiterungspunkt „Datenfluss transformieren“ können Sie den Datenfluss vollständig transformieren. Sie besitzen Zugriff auf einen Pandas-Datenrahmen, der alle Spalten und Zeilen enthält, die Sie verarbeiten. Ihr Code muss einen Pandas-Datenrahmen mit den neuen Daten zurückgeben. 

>[!NOTE]
>In Python müssen alle Daten in einem Pandas-Datenrahmen in den Arbeitsspeicher geladen werden, wenn diese Erweiterung verwendet wird. 
>
>In Spark werden alle Daten in einem einzelnen Workerknoten gesammelt. Wenn die Daten sehr groß sind, kann es vorkommen, dass ein Worker nicht über genügend Arbeitsspeicher verfügt. Verwenden Sie dies mit Bedacht.

### <a name="how-to-use"></a>Verwendung 
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Datenfluss transformieren (Skript)“ hinzufügen. Er ist im Menü **Transformationen** der obersten Ebene verfügbar. 
### <a name="syntax"></a>Syntax 

Ausdruck

```python
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Modul 

```python
def transform(df):  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()  
    return df
```
  

## <a name="transform-partition"></a>Partition transformieren  
### <a name="purpose"></a>Zweck 
Mit dem Erweiterungspunkt „Partition transformieren“ können Sie eine Partition des Datenflusses transformieren. Sie besitzen Zugriff auf einen Pandas-Datenrahmen, der alle Spalten und Zeilen für die betreffende Partition enthält. Ihr Code muss einen Pandas-Datenrahmen mit den neuen Daten zurückgeben. 

>[!NOTE]
>Abhängig von der Größe Ihrer Daten erhalten Sie in Python möglicherweise eine einzelne Partition oder mehrere Partitionen. In Spark arbeiten Sie mit einem Datenrahmen, der die Daten für eine Partition auf einem vorhandenen Workerknoten enthält. Sie können in beiden Fällen nicht davon ausgehen, dass Sie Zugriff auf das gesamte Dataset besitzen. 


### <a name="how-to-use"></a>Verwendung
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Partition transformieren (Skript)“ hinzufügen. Er ist im Menü **Transformationen** der obersten Ebene verfügbar. 

### <a name="syntax"></a>Syntax 

Ausdruck 

```python
    df['partition-id'] = index  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Modul 

```python
def transform(df, index):
    df['partition-id'] = index
    df['index-column'] = range(1, len(df) + 1)
    df = df.reset_index()
    return df
```


## <a name="datapreperror"></a>DataPrepError  
### <a name="error-values"></a>Fehlerwerte  
Die Datenvorbereitung verwendet ein Konzept, das als „Fehlerwerte“ bezeichnet wird. 

In benutzerdefiniertem Python-Code können Fehlerwerte auftreten. Sie sind Instanzen einer Python-Klasse mit dem Namen `DataPrepError`. Diese Klasse dient als Wrapper für eine Python-Ausnahme und weist mehrere Eigenschaften auf. Die Eigenschaften enthalten Informationen zum Fehler, der bei der Verarbeitung des ursprünglichen Werts aufgetreten ist, sowie zum ursprünglichen Wert. 


### <a name="datapreperror-class-definition"></a>DataPrepError-Klassendefinition
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
Die Erstellung von „DataPrepError“ im Python-Framework der Datenvorbereitung sieht in der Regel folgendermaßen aus: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Verwendung 
Mithilfe der zuvor erwähnten Erstellungsmethode kann Python in einem Erweiterungspunkt ausgeführt werden, um „DataPrepErrors“ als Rückgabewerte zu generieren. Es ist sehr viel wahrscheinlicher, dass „DataPrepErrors“ auftreten, wenn Daten an einem Erweiterungspunkt verarbeitet werden. An diesem Punkt muss der benutzerdefinierte Python-Code „DataPrepError“ als einen gültigen Datentyp behandeln.

#### <a name="syntax"></a>Syntax 
Ausdruck 
```python 
    if (isinstance(row["Score"], DataPrepError)): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
```python 
    if (hasattr(row["Score"], "originalValue")): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
Modul 
```python 
def newvalue(row): 
    if (isinstance(row["Score"], DataPrepError)): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
``` 
```python 
def newvalue(row): 
    if (hasattr(row["Score"], "originalValue")): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
```  

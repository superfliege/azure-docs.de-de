---
title: Verwenden der Python-Erweiterbarkeit mit Azure Machine Learning Data Preparation | Microsoft-Dokumentation
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
ms.openlocfilehash: 4e1935a7830b8174796ac12792fbbc0ed110d081
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="data-prep-python-extensions"></a>Python-Erweiterungen für die Datenvorbereitung
Die Datenvorbereitung enthält Erweiterbarkeit auf mehreren Ebenen, um Funktionslücken zwischen integrierten Funktionen zu füllen. In diesem Dokument wird die Erweiterbarkeit mithilfe des Python-Skripts erläutert. 

## <a name="custom-code-steps"></a>Benutzerdefinierte Codeschritte 
Die Datenvorbereitung verfügt über folgende benutzerdefinierte Schritte, mit denen Sie Benutzercode schreiben können: 
1. Dateileser*
2. Writer*
3. Spalte hinzufügen
4. Erweiterter Filter
5. Datenfluss transformieren
6. Partition transformieren

*Diese Schritte werden aktuell nicht in einer Spark-Ausführung unterstützt. 

## <a name="code-block-types"></a>Codeblocktypen 
Wir unterstützen für jeden dieser Schritte zwei Codeblocktypen. Zum einen unterstützen wir einen bloßen Python-Ausdruck, der unverändert ausgeführt wird. Und zum anderen unterstützen wir ein Python-Modul, in dem wir eine bestimmte Funktion mit einer bekannten Signatur in dem von Ihnen angegebenen Code aufrufen.

Sie können z.B. eine neue Spalte hinzufügen, die das Protokoll einer anderen Spalte durch die folgenden zwei Arten berechnet: Ausdruck: 

```python    
    math.log(row["Score"])
```

Modul: 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


Bei der Transformation „Spalte hinzufügen“ im Modulmodus wird davon ausgegangen, dass eine Funktion mit dem Namen `newvalue` gefunden wird, die eine Zeilenvariable akzeptiert und den Wert für die Spalte zurückgibt. Dieses Modul kann eine beliebige Anzahl von Python-Code mit anderen Funktionen, Importen usw. enthalten. 

Genauer Informationen zu jedem Erweiterungspunkt werden in den folgenden Abschnitten diskutiert: 

## <a name="imports"></a>Importe 
Wenn Sie den Ausdrucksblocktyp verwenden, können Sie immer noch Importanweisungen zu Ihrem Code hinzufügen, aber sie müssen alle in den oberen Zeilen Ihres Codes gruppiert werden. Richtig: 

```python
import math 
import numpy 
math.log(row["Score"])
```
 

Fehler  

```python
import math  
math.log(row["Score"])  
import numpy
```
 
 
Wenn Sie den Modulblocktyp verwenden, können Sie bei der Verwendung der Importanweisung den normalen Python-Regeln folgen. 

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
  

## <a name="installing-new-packages"></a>Installieren neuer Pakete
Zur Verwendung eines Pakets, dass nicht standardmäßig installiert ist, müssen Sie es zunächst in den Umgebungen installieren, welche die Datenvorbereitung verwenden. Diese Installation muss sowohl auf Ihrem lokalen Computer als auch in allen Computezielen durchgeführt werden, in denen Sie ausführen möchten.

Sie müssen die Datei „conda_dependencies.yml“ im Ordner „aml_config“ im Stamm Ihres Projekts ändern, um Ihre Pakete in einem Computeziel zu installieren.

### <a name="windows"></a>Windows 
Sie finden den Speicherort unter Windows, indem Sie nach der App-spezifischen Installation von Python und dem zugehörigen Skriptverzeichnis suchen. Der Standardspeicherort lautet:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts.` 

Führen Sie anschließend einen der folgenden Befehlen aus: 

`conda install <libraryname>` 

oder 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
Sie finden den Speicherort unter Mac, indem Sie nach der App-spezifischen Installation von Python und dem zugehörigen Skriptverzeichnis suchen. Der Standardspeicherort lautet: 

`/Users/<user>/Library/Caches/AmlWorkbench>/Python/bin` 

Führen Sie anschließend einen der folgenden Befehlen aus: 

`./conda install <libraryname>`

oder 

`./pip install <libraryname>`

## <a name="column-data"></a>Spaltendaten 
Auf Spaltendaten kann aus einer Zeile zugegriffen werden, indem die Punktnotation oder die Schlüssel-Wert-Notation verwendet wird. Auf Spaltennamen, die Leerzeichen oder Sonderzeichen enthalten, kann nicht mithilfe der Punktnotation zugegriffen werden. Die `row`-Variable sollte immer in beiden Modi der Python-Erweiterungen (Modul und Ausdruck) definiert sein. 

Beispiele: 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="file-reader"></a>Dateileser 
### <a name="purpose"></a>Zweck 
Mit diesem Erweiterungspunkt können Sie den Prozess des Lesens einer Datei in einen Datenfluss vollständig steuern. Das System ruft Ihren Code auf und übergibt die Liste der zu verarbeitenden Dateien, und Ihr Code muss einen Pandas-Datenrahmen erstellen und zurückgeben. 

>[!NOTE]
>Dieser Erweiterungspunkt funktioniert nicht in Spark. 


### <a name="how-to-use"></a>Verwendung 
Sie greifen über den Assistenten der offenen Datenquelle auf diesen Erweiterungspunkt zu. Wählen Sie auf der ersten Seite die Datei und anschließend den Dateispeicherort aus. Öffnen Sie auf der Seite „Auswählen der Dateiparameter“ aus der Dropdownliste den Dateityp, und wählen Sie „Benutzerdefinierte Datei (Skript)“ aus. 

Ihrem Code wird ein Pandas-Datenrahmen mit dem Namen „df“ gegeben, der Informationen über die Dateien enthält, die Sie lesen müssen. Wenn Sie ein Verzeichnis mit mehreren Dateien öffnen, enthält der Datenrahmen mehr als eine Zeile.  

Dieser Datenrahmen verfügt über die folgenden Spalten: 
- Path: Die zu lesende Datei
- PathHint: Der Speicherort der Datei Werte: „Local“, „AzureBlobStorage“, „AzureDataLakeStorage“
- AuthenticationType: Der Typ der Authentifizierung, der für den Zugriff auf die Datei verwendet wird. Werte: „None“, „SasToken“, „OAuthToken“
- AuthenticationValue: Enthält „Npne“ oder das Token, welches verwendet werden soll

### <a name="syntax"></a>Syntax 
Ausdruck: 

```python
    paths = df['Path'].tolist()  
    df = pd.read_csv(paths[0])
```


Modul:  
```python
PathHint = Local  
def read(df):  
    paths = df['Path'].tolist()  
    filedf = pd.read_csv(paths[0])  
    return filedf  
```
 

## <a name="writer"></a>Writer 
### <a name="purpose"></a>Zweck 
Mit dem Writer-Erweiterungspunkt können Sie den Prozess des Schreibens von Daten aus einem Datenfluss vollständig steuern. Das System ruft Ihren Code auf und übergibt einen Datenrahmen, und Ihr Code kann diesen Datenrahmen verwenden, um Daten nach Ihren Bedürfnissen zu schreiben. 

>[!NOTE]
>Dieser Writer-Erweiterungspunkt funktioniert nicht in Spark. 


### <a name="how-to-use"></a>Verwendung 
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Schreibdatenfluss (Skript)“ hinzufügen. Er ist im Transformationsmenü der obersten Ebene verfügbar. 

### <a name="syntax"></a>Syntax 
Ausdruck: 

```python
    df.to_csv('c:\\temp\\output.csv')
```

Modul:

```python
def write(df):  
    df.to_csv('c:\\temp\\output.csv')  
    return df
```
 
 
Dieser benutzerdefinierte Schreibblock kann mitten in einer Liste von Schritten vorhanden sein. Wenn Sie also ein Modul verwenden, muss Ihre Schreibfunktion den Datenrahmen zurückgeben, der als Eingabe für den folgenden Schritt gilt. 

## <a name="add-column"></a>Hinzufügen einer Spalte 
### <a name="purpose"></a>Zweck
Mit diesem Erweiterungspunkt können Sie einen Python-Code schreiben, um eine neue Spalte zu berechnen. Der von Ihnen geschriebene Code kann auf die ganze Zeile zugreifen. Er muss einen neuen Spaltenwert für jede Zeile zurückgeben. 

### <a name="how-to-use"></a>Verwendung
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Spalte hinzufügen (Skript)“ hinzufügen. Er ist im Transformationsmenü der obersten Ebene sowie im Spaltenkontextmenü verfügbar. 

### <a name="syntax"></a>Syntax
Ausdruck: 

```python
    math.log(row["Score"])
```

Modul: 

```python
def newvalue(row):  
     return math.log(row["Score"])
```
 

## <a name="advanced-filter"></a>Erweiterter Filter
### <a name="purpose"></a>Zweck 
Mit diesem Erweiterungspunkt können Sie einen benutzerdefinierten Filter schreiben. Sie haben Zugriff auf die gesamte Zeile, und Ihr Code muss TRUE (einschließlich der Zeile) oder FALSE (ausschließlich der Zeile) zurückgeben. 

### <a name="how-to-use"></a>Verwendung
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Erweiterter Filter (Skript)“ hinzufügen. Er ist im Transformationsmenü der obersten Ebene verfügbar. 

### <a name="syntax"></a>Syntax

Ausdruck: 

```python
    row["Score"] > 95
```

Modul:  

```python
def includerow(row):  
    return row["Score"] > 95
```
 

## <a name="transform-dataflow"></a>Transformieren des Datenflusses
### <a name="purpose"></a>Zweck 
Mit dem Erweiterungspunkt können Sie den Datenfluss vollständig transformieren. Sie haben Zugriff auf einen Pandas-Datenrahmen, der alle Spalten und Zeilen enthält, die Sie verarbeiten, und Ihr Code muss einen Pandas-Datenrahmen mit den neuen Daten zurückgeben. 

>[!NOTE]
>In Python müssen alle Daten in einen Arbeitsspeicher in einem Pandas-Datenrahmen geladen werden, wenn diese Erweiterung verwendet wird. 

In Spark werden alle Daten in einem einzelnen Workerknoten gesammelt. Wenn die Daten sehr groß sind, kann dies dazu führen, dass ein Worker nicht genügend Arbeitsspeicher hat. Verwenden Sie dies mit Bedacht.

### <a name="how-to-use"></a>Verwendung 
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Datenfluss transformieren (Skript)“ hinzufügen. Er ist im Transformationsmenü der obersten Ebene verfügbar. 
### <a name="syntax"></a>Syntax 

Ausdruck: 

```python
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Modul: 

```python
def transform(df):  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()  
    return df
```
  

## <a name="transform-partition"></a>Transformieren der Partition  
### <a name="purpose"></a>Zweck 
Mit diesem Erweiterungspunkt können Sie eine Partition des Datenflusses transformieren. Sie haben Zugriff auf einen Pandas-Datenrahmen, der alle Spalten und Zeilen für diese Partition enthält, und Ihr Code muss einen Pandas-Datenrahmen mit den neuen Daten zurückgeben. 

>[!NOTE]
>Abhängig von der Größe Ihrer Daten erhalten Sie in Python möglicherweise eine einzelne Partition oder mehrere Partitionen. In Spark arbeiten Sie mit einem Datenrahmen, der die Daten für eine Partition in einem vorhandenen Workerknoten enthält. Sie können in beiden Fällen nicht davon ausgehen, dass Sie Zugriff auf das komplette Dataset haben. 


### <a name="how-to-use"></a>Verwendung
Sie können diesen Erweiterungspunkt mithilfe des Blocks „Partition transformieren (Skript)“ hinzufügen. Er ist im Transformationsmenü der obersten Ebene verfügbar. 

### <a name="syntax"></a>Syntax 

Ausdruck: 

```python
    df['partition-id'] = index  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Modul: 

```python
def transform(df, index):
    df['partition-id'] = index
    df['index-column'] = range(1, len(df) + 1)
    df = df.reset_index()
    return df
```


## <a name="datapreperror"></a>DataPrepError  
### <a name="error-values"></a>Fehlerwerte  
In der Datenvorbereitung ist das Konzept der Fehlerwerte vorhanden. Deren Erstellung und Existenzgrund wird hier beschrieben: <link to error values doc> 

Im benutzerdefinierten Python-Code können Fehlerwerte auftreten. Sie sind Instanzen einer Python-Klasse mit dem Namen `DataPrepError`. Diese Klasse umschließt eine Python-Ausnahme und besitzt einige Eigenschaften, die Informationen über den Fehler, der bei der Verarbeitung des ursprünglichen Werts aufgetreten ist, und den ursprünglichen Wert enthalten. 


### <a name="datapreperror-class-definition"></a>Klassendefinition von DataPrepError
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
Die Erstellung von DataPrepError im Python-Framework der Datenvorbereitung sieht allgemein folgendermaßen aus: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Verwendung 
Mithilfe der zuvor erwähnten Erstellungsmethode kann Python in einem Erweiterungspunkt ausgeführt werden, um DataPrepErrors als Rückgabewerte zu generieren. Es ist sehr viel wahrscheinlicher, dass DataPrepErrors auftreten, wenn Daten in einem Erweiterungspunkt verarbeitet werden. An diesem Punkt muss der benutzerdefinierte Python-Code DataPrepError als einen gültigen Datentyp behandeln. 

#### <a name="syntax"></a>Syntax 
Ausdruck:  
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
Modul:  
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

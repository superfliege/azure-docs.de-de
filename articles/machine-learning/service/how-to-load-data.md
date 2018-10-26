---
title: Laden von Daten mit dem Azure Machine Learning Data Prep SDK – Python
description: Erfahren Sie mehr über das Laden von Daten mit dem Azure Machine Learning Data Prep SDK. Sie können verschiedene Typen von Eingabedaten laden, Datendateitypen und -parameter angeben oder die intelligente SDK-Lesefunktionalität verwenden, um einen Dateityp automatisch erkennen zu lassen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 91db32b7056a0cf211e6293a891d58e0239ca499
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237584"
---
# <a name="load-and-read-data-with-azure-machine-learning"></a>Laden und Lesen von Daten mit Azure Machine Learning

Verwenden Sie das [Azure Machine Learning Data Prep SDK](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py), um verschiedene Typen von Eingabedaten zu laden. 

Geben Sie den Datendateityp und die Parameter an, um Ihre Daten zu laden.

## <a name="use-text-line-data"></a>Verwenden von Textzeilendaten 
Eine der einfachsten Möglichkeiten zum Laden von Daten besteht darin, diese als Textzeilen zu lesen.

Im Folgenden finden Sie einen Beispielcode:
```python
dataflow = dprep.read_lines(path='./data/text_lines.txt')
dataflow.head(5)
```
||Liniendiagramm|
|----|-----|
|0|Datum \|\| Mindesttemperatur \|\| Maximaltemperatur|
|1|2015-07-1 \|\|  -4.1 \|\|  10.0|
|2|2015-07-2 \|\|  -0.8 \|\|  10.8|
|3|2015-07-3 \|\|  -7.0 \|\|  10.5|
|4|2015-07-4 \|\|  -5.5 \|\|  9.3|

Nachdem die Daten erfasst wurden, können Sie ein Pandas-DataFrame für das gesamte Dataset abrufen.

Im Folgenden finden Sie einen Beispielcode:
```python
df = dataflow.to_pandas_dataframe()
df
```
Beispielausgabe:
||Liniendiagramm|
|----|-----|
|0|Datum \|\| Mindesttemperatur \|\| Maximaltemperatur|
|1|2015-07-1\|\| 4.1\|\| 10.0|
|2|2015-07-2\|\| 0.8\|\| 10.8|
|3|2015-07-3\|\| 7.0\|\| 10.5|
|4|2015-07-4\|\| 5.5\|\| 9.3|

## <a name="use-csv-data"></a>Verwenden von CSV-Daten
Wenn Sie durch Trennzeichen getrennte Dateien lesen, können Sie die zugrunde liegende Laufzeitumgebung die Analyseparameter (z.B. ein Trennzeichen, Codierung, die angibt, ob Header verwendet werden sollen) ableiten lassen, statt sie selbst angeben zu müssen. Versuchen Sie bei diesem Beispiel, eine Datei zu lesen, indem Sie nur den Speicherort angeben. 

Im Folgenden finden Sie einen Beispielcode:
```python
# SAS expires June 16th, 2019
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dataflow.head(5)
```

Beispielausgabe:
| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0||stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Hale County|10171002158| |
|2|ALABAMA|1|101710|Hale County|10171002162| |
|3|ALABAMA|1|101710|Hale County|10171002156| |
|4|ALABAMA|1|101710|Hale County|10171000588|2|

Einer der Parameter, die Sie angeben können, ist eine Anzahl von Zeilen, die in den Dateien, die Sie lesen, übersprungen werden sollen. Verwenden Sie den folgenden Code, um die doppelte Zeile herauszufiltern.
```python
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dataflow.head(5)
```

Beispielausgabe:
| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale County|10171002158|29|
|1|ALABAMA|1|101710|Hale County|10171002162|40 |
|2|ALABAMA|1|101710|Hale County|10171002156| 43|
|3|ALABAMA|1|101710|Hale County|10171000588|2|
|4|ALABAMA|1|101710|Hale County|10171000589|23 |

Als Nächstes können Sie sich die Datentypen der Spalten ansehen.
Im Folgenden finden Sie einen Beispielcode:
```python
dataflow.head(1).dtypes

stnam                     object
fipst                     object
leaid                     object
leanm10                   object
ncessch                   object
schnam10                  object
MAM_MTH00numvalid_1011    object
dtype: object
```

Leider wurden alle unsere Spalten als Zeichenfolgen zurückgegeben. Dies liegt daran, dass das Azure Machine Learning Data Prep SDK standardmäßig nicht den Datentyp ändert. Die Datenquelle, über den der Lesevorgang erfolgt, ist eine Textdatei. Das SDK liest also alle Werte als Zeichenfolgen. In diesem Beispiel möchten wir jedoch die numerischen Spalten als Zahlen analysieren. Zu diesem Zweck können Sie den Parameter „inference_arguments“ auf „current_culture“ festlegen.

```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dataflow.head(1).dtypes

stnam                      object
fipst                     float64
leaid                     float64
leanm10                    object
ncessch                   float64
schnam10                   object
ALL_MTH00numvalid_1011    float64
dtype: object
```

Einige der Spalten wurden ordnungsgemäß als Zahlen erkannt und deren Typ ist auf „float64“ festgelegt. Nach der Erfassung können Sie ein Pandas-DataFrame für das gesamte Dataset abrufen.
Im Folgenden finden Sie einen Beispielcode:
```python
df = dataflow.to_pandas_dataframe()
df
```

Beispielausgabe:
| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale County|1.017100e+10|49.0|
|1|ALABAMA|Hale County|1.017100e+10|40.0|
|2|ALABAMA|Hale County|1.017100e+10|43.0|
|3|ALABAMA|Hale County|1.017100e+10|2.0|
|4|ALABAMA|Hale County|1.017100e+10|23.0|

## <a name="use-excel-data"></a>Verwenden von Excel-Daten
Das Azure Machine Learning Data Prep SDK umfasst eine `read_excel`-Funktion, um Excel-Dateien zu laden. Im Folgenden finden Sie einen Beispielcode:
```python
dataflow = dprep.read_excel(path='./data/excel.xlsx')
dataflow.head(5)
```

Beispielausgabe:
||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|Hoba|Iron, IVB|60000000.0|Gefunden|1920.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-19.58333|17.91667|
|1|Cape York|Iron, IIIAB|58200000.0|Gefunden|1818.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |76.13333|-64.93333|
|2|Campo del Cielo|Iron, IAB-MG|50000000.0|Gefunden|1576.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |-27.46667|-60.58333|
|3|Canyon Diablo|Iron, IAB-MG|30000000.0|Gefunden|1891.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |35.05000|-111.03333|
|4|Armanty|Iron, IIIE|28000000.0|Gefunden|1898.0|http://www.lpi.usra.edu/meteor/metbull.php?cod... |47.00000|88.00000|

Sie haben das erste Arbeitsblatt der Excel-Datei geladen. Sie können das gleiche Ergebnis erzielen, indem Sie explizit den Namen der Tabelle angeben, die geladen werden soll. Wenn Sie stattdessen das zweite Blatt laden möchten, können Sie den Namen als Argument angeben. Beispiel: 
```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dataflow.head(5)
```

Beispielausgabe:
||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|
|1|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|
|2|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|Keine|
|3|RANK|Titel|Studio|Worldwide|Domestic / %|Column1|Overseas / %|Column2|Year^|
|4|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009^|5|

Wie Sie sehen können, enthielt die Tabelle auf dem zweiten Arbeitsblatt Header und drei leere Zeilen. Sie müssen die Argumente der Funktion entsprechend ändern. Beispiel:
```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_header=True, skip_rows=3)
df = dataflow.to_pandas_dataframe()
df
```

Beispielausgabe:
||RANK|Titel|Studio|Worldwide|Domestic / %|Column1|Overseas / %|Column2|Year^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760.5|0.273|2027.5|0.727|2009^|
|1|2|Titanic|Par.|2186.8|658.7|0.301|1528.1|0.699|1997^|
|2|3|Marvel's The Avengers|BV|1518.6|623.4|0,41|895.2|0.59|2012|
|3|4|Harry Potter and the Deathly Hallows Part 2|WB|1341.5|381|0.284|960.5|0.716|2011|
|4|5|Frozen|BV|1274.2|400.7|0.314|873.5|0.686|2013|

## <a name="use-fixed-width-data-files"></a>Verwenden von Datendateien mit fester Breite
Für Dateien mit fester Breite können Sie eine Liste mit Offsets angeben. Bei der ersten Spalte wird immer davon ausgegangen, dass sie beim Offset 0 beginnt. Beispiel:
```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dataflow.head(5)
```

Beispielausgabe:
||010000|99999|BOGUS NORWAY|NO|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|BOGUS NORWAY|NO|NO|ENSO||||
|1|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|
|2|010013|99999|ROST|NO|NO|||||
|3|010014|99999|SOERSTOKKEN|NO|NO|ENSO|+59783|+005350|+00500|
|4|010015|99999|BRINGELAND|NO|NO|ENBL|+61383|+005867|+03270|


Wenn keine Header in den Dateien vorhanden sind, sollten Sie die erste Zeile als Daten behandeln. Durch Übergeben von `PromoteHeadersMode.NONE` an das Argument für den Headerschlüsselwort, können Sie die Headererkennung vermeiden und die richtigen Daten abrufen. Beispiel: 
```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)

df = dataflow.to_pandas_dataframe()
df
```

Beispielausgabe:

||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|BOGUS NORWAY|NO|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|BOGUS NORWAY|NO|NO|ENSO||||
|2|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|
|3|010013|99999|ROST|NO|NO|||||
|4|010014|99999|SOERSTOKKEN|NO|NO|ENSO|+59783|+005350|+00500|
|5|010015|99999|BRINGELAND|NO|NO|ENBL|+61383|+005867|+03270|

## <a name="use-sql-data"></a>Verwenden von SQL-Daten
Das Azure Machine Learning Data Prep SDK kann auch Daten von SQL Server-Instanzen laden. Derzeit wird nur Microsoft SQL Server unterstützt.
Um Daten von SQL Server zu lesen, erstellen Sie ein Datenquellenobjekt, das die Verbindungsinformationen enthält. Beispiel: 
```python
secret = dprep.register_secret("[SECRET-USERNAME]", "[SECRET-PASSWORD]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=[DATABASE-PASSWORD])
```
Wie Sie sehen können, akzeptiert das Kennwortparameter von `MSSQLDataSource` ein geheimes Objekt. Sie können ein geheimes Objekt auf zwei Arten abrufen:
-   Registrieren Sie das Geheimnis und dessen Wert mit der Ausführungs-Engine. 
-   Erstellen Sie das Geheimnis mit nur einer ID. (Dies ist nützlich, wenn der Wert des Geheimnis bereits in der Ausführungsumgebung registriert ist.)

Nachdem Sie ein Datenquellobjekt erstellt haben, können Sie mit dem Lesen von Daten fortfahren. Beispiel: 
```python
dataflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dataflow.head(5)
```

Beispielausgabe:
||ProductID|NAME|ProductNumber|Farbe|StandardCost|ListPrice|Größe|Weight|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|ThumbNailPhoto|ThumbnailPhotoFileName|rowguid|ModifiedDate|
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|0|680|HL Road Frame - Black, 58|FR-R92B-58|Schwarz|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00+00:00|Keine|Keine|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461f-9069-55309d90ea7e|2008-03-11 |0:01:36.827000+00:00|
|1|706|HL Road Frame - Red, 58|FR-R92R-58|Rot|1059.3100|1431.50|58|1016.04|18|6|2002-06-01 00:00:00+00:00|Keine|Keine|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000+00:00|
|2|707|Sport-100 Helmet, Red|HL-U509-R|Rot|13.0863|34.99|Keine|Keine|35|33|2005-07-01 00:00:00+00:00|Keine|Keine|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000+00:00|
|3|708|Sport-100 Helmet, Black|HL-U509|Schwarz|13.0863|34.99|Keine|Keine|35|33|2005-07-01 00:00:00+00:00|Keine|Keine|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|a25a44fb-c2de-4268-958f-110b8d7621e2|2008-03-11 |10:01:36.827000+00:00|
|4|709|Mountain Bike Socks, M|SO-B909-M|Weiß|3.3963|9.50|M|Keine|27|18|2005-07-01 00:00:00+00:00|2006-06-30 00:00:00+00:00|Keine|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|18f95f47-1540-4e02-8f1f-cc1bcb6828d0|2008-03-11 |10:01:36.827000+00:00|

```python
df = dataflow.to_pandas_dataframe()
df.dtypes
```

Beispielausgabe:
```
ProductID                                     int64
Name                                         object
ProductNumber                                object
Color                                        object
StandardCost                                float64
ListPrice                                   float64
Size                                         object
Weight                                      float64
ProductCategoryID                             int64
ProductModelID                                int64
SellStartDate             datetime64[ns, UTC+00:00]
SellEndDate                                  object
DiscontinuedDate                             object
ThumbNailPhoto                               object
ThumbnailPhotoFileName                       object
rowguid                                      object
ModifiedDate              datetime64[ns, UTC+00:00]
dtype: object
```

## <a name="use-azure-data-lake-storage"></a>Verwenden von Azure Data Lake Storage
Es gibt zwei Möglichkeiten, mit dem das SDK das erforderliche OAuth-Token für den Zugriff auf Azure Data Lake Storage abrufen kann:
-   Abrufen des Zugriffstokens von einer aktuellen Anmeldesitzung der Azure CLI-Anmeldung des Benutzers
-   Verwenden eines Dienstprinzipals und eines Zertifikats als Geheimnis

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Verwenden eines Zugriffstokens aus einer aktuellen Azure CLI-Sitzung
Führen Sie auf dem lokalen Computer den folgenden Befehl aus:

> [!NOTE] 
> Wenn Ihr Benutzerkonto Mitglied von mehr als einem Azure-Mandanten ist, müssen Sie den Mandanten in Form des AAD-URL-Hostnamens angeben.


Beispiel: 
```azurecli
az login
az account show --query tenantId
dataflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dataflow.head(5) head
```
### <a name="create-a-service-principal-with-the-azure-cli"></a>Erstellen eines Dienstprinzipals über die Azure CLI
Sie können mit der Azure CLI einen Dienstprinzipal und das entsprechende Zertifikat erstellen. Dieser Dienstprinzipal wird als Leser konfiguriert, wobei der Bereich ausschließlich auf das Azure Data Lake Storage-Konto „dpreptestfiles“ beschränkt ist.  Beispiel: 
```azurecli
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```
Dieser Befehl gibt die `appId` und den Pfad zur Zertifikatdatei (normalerweise im Ordner „home“) aus. Die CRT-Datei enthält das öffentliche Zertifikat sowie den privaten Schlüssel im PEM-Format.

Extrahieren Sie den Fingerabdruck:
```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

Verwenden Sie die ACL für das Azure Data Lake Storage-Dateisystem, die objectId des Benutzers oder wie in diesem Beispiel den Dienstprinzipal. Beispiel: 
```azurecli
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```

Um den Zugriff auf `Read` und `Execute` für das Azure Data Lake Storage-Dateisystem zu konfigurieren, müssen Sie die ACL für Ordner und Dateien separat konfigurieren. Dies ist darauf zurückzuführen, dass das zugrunde liegende HDFS-ACL-Modell keine Vererbung unterstützt. Beispiel: 
```azurecli
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r-x" --path /
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r--" --path /farmers-markets.csv
```
```
certThumbprint = 'C2:08:9D:9E:D1:74:FC:EB:E9:7E:63:96:37:1C:13:88:5E:B9:2C:84'
certificate = ''
with open('./data/adls-dpreptestfiles.crt', 'rt', encoding='utf-8') as crtFile:
    certificate = crtFile.read()

servicePrincipalAppId = "8dd38f34-1fcb-4ff9-accd-7cd60b757174"
```
### <a name="acquire-an-oauth-access-token"></a>Abrufen eines OAuth-Zugriffstokens
Erstellen Sie mithilfe des Pakets `adal` (über `pip install adal`) einen Authentifizierungskontext auf dem MSFT-Mandanten, und rufen Sie ein OAuth-Zugriffstoken ab. Bei ADLS muss die Ressource in der Tokenanforderung für https://datalake.azure.net sein, die sich von den meisten anderen Azure-Ressourcen unterscheidet.

```python
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dataflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dataflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Website|street|city|County|
|----|------|-----|----|----|----|----|
|0|1012063|Caledonia Farmers Market Association - Danville|https://sites.google.com/site/caledoniafarmers... ||Danville|Caledonia|
|1|1011871|Stearns Homestead Farmers' Market|http://Stearnshomestead.com |6975 Ridge Road|Parma|Cuyahoga|
|2|1011878|100 Mile Market|http://www.pfcmarkets.com |507 Harrison St|Kalamazoo|Kalamazoo|
|3|1009364|106 S. Main Street Farmers Market|http://thetownofsixmile.wordpress.com/ |106 S. Main Street|Six Mile|||
|4|1010691|10th Steet Community Farmers Market|http://agrimissouri.com/mo-grown/grodetail.php... |10th Street and Poplar|Lamar|Barton|

## <a name="repeatability-during-copy"></a>Wiederholbarkeit beim Kopieren
Beim Kopieren von Daten mit Azure SQL-SQL-Server von anderen Daten speichert müssen Sie die Wiederholbarkeit unerwünschte Ergebnisse vermeiden Bedenken. 

Beim Kopieren von Daten zu Azure SQL-SQL Server-Datenbank wird kopieraktivität standardmäßig APPEND DataSet der Sink-Tabelle in der Standardeinstellung. Beispielsweise sieht beim Kopieren von Daten aus einer CSV (durch Trennzeichen getrennte Werte) Datei Datenquelle, die zwei Datensätze mit Azure SQL-SQL-Datenbank enthält diese Tabelle:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Angenommen Sie, Sie wurden Fehler gefunden wird, in der Quelldatei, und aktualisiert die Menge der nach-unten-Tube von 2 bis 4 in der Quelldatei. Wenn Sie den Datenslice für diesen Zeitraum erneut ausführen, finden Sie zwei neue Datensätze, die mit Azure SQL-SQL-Datenbank angefügt. Die im folgenden wird davon ausgegangen, die primary Key-Einschränkung in keiner der Spalten in der Tabelle vorkommen.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Um dies zu vermeiden, müssen Sie UPSERT-Semantik angeben, durch die Nutzung eines der folgenden 2 Mechanismen, die unten aufgeführten.

> [!NOTE]
> Ein Slice kann automatisch in Azure Data Factory gemäß der angegebenen wiederholungsrichtlinie erneut ausgeführt werden.
> 
> 

### <a name="mechanism-1"></a>Mechanismus 1
Sie können nutzen **SqlWriterCleanupScript** Eigenschaft Systembereinigungsaktion zuerst ausführen, wenn ein Slice ausgeführt wird. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Das Bereinigungsskript ausgeführt werden würde erste während Kopie für einen bestimmten Slice werden die Daten aus der SQL-Tabelle, die für dieses Segment gelöscht würde. Die Aktivität wird anschließend die Daten in der SQL-Tabelle einfügen. 

Wenn der Slice wird nun erneut ausführen, können Sie sehen, dass die Menge aktualisiert wird, sobald gewünscht.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Nehmen Sie an, dass der Flatfile Washer Datensatz aus der ursprünglichen Csv entfernt wird. Klicken Sie dann den Slice erneut ausführen, erzeugt das folgende Ergebnis: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Keine neuen musste ausgeführt werden. Copy-Aktivität ausgeführt wurde, das Bereinigungsskript aus, um die entsprechenden Daten für dieses Segment zu löschen. Und dann die Eingabe aus dem freigegebenen Clustervolume gelesen werden (die Sie dann nur 1 Datensatz enthalten) und die Tabelle eingefügt. 

### <a name="mechanism-2"></a>Mechanismus 2
> [!IMPORTANT]
> SliceIdentifierColumnName wird für Azure SQL Data Warehouse zu diesem Zeitpunkt nicht unterstützt. 

Ein weiterer Mechanismus Wiederholbarkeit erreicht ist, durch die Verwendung einer dedizierten Spalteninhalts (**SliceIdentifierColumnName**) in der Ziel-Tabelle. Diese Spalte würde von Azure Data Factory verwendet werden, um sicherzustellen, dass die Quelle und Ziel synchronisiert bleiben. Dieser Ansatz funktioniert bei Flexibilität beim Ändern oder das Ziel-SQL-Tabellenschema definieren. 

Diese Spalte wird von Azure Data Factory für Wiederholbarkeit Zwecke verwendet werden, und im Prozess Azure Data Factory wird keine Änderungen Schema der Tabelle. Die Möglichkeit, diesen Ansatz zu verwenden:

1. Definieren Sie eine Spalte des Typs binäre (32) im Ziel-SQL-Tabelle. Es darf keine Einschränkungen für diese Spalte. Nennen Sie diese Spalte als "ColumnForADFuseOnly" wir für dieses Beispiel.
2. Verwenden sie in der kopieraktivität wie folgt:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory füllt diese Spalte gemäß benötigt, stellen Sie sicher, dass die Quelle und Ziel synchronisiert bleiben. Die Werte dieser Spalte sollten nicht vom Benutzer außerhalb dieses Kontexts verwendet werden. 

Ähnlich wie bei den Mechanismus 1 kopieren-Aktivität wird automatisch zunächst bereinigen Sie die Daten für das angegebene Segment aus dem Ziel-SQL-Tabelle, und führen Sie dann die kopieraktivität normalerweise zum Einfügen der Daten aus der Quelle zum Ziel für dieses Segment. 


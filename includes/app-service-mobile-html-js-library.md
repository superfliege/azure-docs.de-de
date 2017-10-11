## <a name="create-client"></a>Erstellen Sie eine Clientverbindung
Erstellen Sie eine Clientverbindung durch das Erstellen einer `WindowsAzure.MobileServiceClient` Objekt.  Ersetzen Sie `appUrl` durch die URL in Ihrer mobilen App.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <a name="table-reference"></a>Arbeiten mit Tabellen
Um den Zugriff oder Aktualisieren von Daten erstellen Sie einen Verweis auf die Back-End-Tabelle. Ersetzen Sie `tableName` durch den Namen der Tabelle

```
var table = client.getTable(tableName);
```

Nachdem Sie einen Tabellenverweis haben, können Sie arbeiten mit der Tabelle weiter:

* [Abfragen einer Tabelle](#querying)
  * [Filtern von Daten](#table-filter)
  * [Paging von Daten](#table-paging)
  * [Sortieren von Daten](#sorting-data)
* [Einfügen von Daten](#inserting)
* [Ändern von Daten](#modifying)
* [Löschen von Daten](#deleting)

### <a name="querying"></a>Vorgehensweise: Abfragen ein Tabellenverweises
Nachdem Sie einen Tabellenverweis haben, können Sie es zur Abfrage von Daten auf dem Server verwenden.  Abfragen werden in einer "LINQ-ähnlichen" Sprache vorgenommen.
Um alle Daten aus der Tabelle zurückzugeben, verwenden Sie den folgenden Code:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Die Success-Funktion wird mit den Ergebnissen.  Verwenden Sie nicht `for (var i in results)` in den Erfolg fungieren als, die Informationen durchlaufen wird, die in den Ergebnissen enthalten ist bei anderen Abfragefunktionen (z. B. `.includeTotalCount()`) verwendet werden.

Weitere Informationen zur Syntax Abfragen finden Sie unter [Abfrage Objekt Dokumentation].

#### <a name="table-filter"></a>Filtern von Daten auf dem server
Sie können eine `where` -Klausel für den Tabellenverweis:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Sie können auch eine Funktion, die das Objekt filtert.  In diesem Fall die `this` Variable zugewiesen wird, mit dem aktuellen Objekt gefiltert wird.  Der folgende Code ist funktionell gleichwertig mit dem vorherigen Beispiel:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <a name="table-paging"></a>Paging von Daten
Nutzen der `take()` und `skip()` Methoden.  Wenn beispielsweise die Tabelle in der Zeile 100 Datensätze geteilt werden soll:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Die `.includeTotalCount()` Methode wird verwendet, um ein Feld TotalCount jedoch stattdessen das Ergebnisse hinzufügen.  Die TotalCount-Feld wird mit der Gesamtanzahl von Datensätzen gefüllt, die zurückgegeben werden würde, wenn keine Auslagerung verwendet wird.

Klicken Sie dann können die Seiten-Variable und einige UI-Schaltflächen Sie eine Seitenliste bereitstellen; Verwenden Sie `loadPage()` neuer Datensätze für jede Seite laden.  Implementieren Sie caching zu Geschwindigkeit Zugriff auf Datensätze, die bereits geladen wurden.

#### <a name="sorting-data"></a>Vorgehensweise: Return sortierte Daten
Verwenden der `.orderBy()` oder `.orderByDescending()` Abfragemethoden:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Weitere Informationen zu Query-Objekt finden Sie unter [Abfrage Objekt Dokumentation].

### <a name="inserting"></a>Vorgehensweise: Einfügen von Daten
Erstellen einer JavaScript-Objekt, das entsprechende Datum und der Aufruf `table.insert()` asynchron:

```javascript
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Für erfolgreiche Einfügung wird das eingefügte Element mit dem zusätzlichen Felder zurückgegeben, die für Synchronisierungsvorgänge erforderlich sind.  Aktualisieren Sie Ihren eigenen Cache mit diesen Informationen für späteren Updates an.

Das Azure Mobile Apps Node.js-Server-SDK unterstützt dynamisches Schema zu Entwicklungszwecken.  Dynamisches Schema können Sie Spalten zur Tabelle hinzufügen, indem Sie sie in einer INSERT- oder update-Operation.  Es wird empfohlen, dass Sie dynamische Schema deaktivieren, bevor Sie Ihre Anwendung produktiv zu fortfahren.

### <a name="modifying"></a>Vorgehensweise: Ändern von Daten
Ähnlich wie die `.insert()` -Methode, sollten Sie ein Update-Objekt erstellen und rufen dann `.update()`.  Das Update-Objekt darf die ID des Datensatzes zu aktualisierenden - die ID wird abgerufen, beim Lesen des Datensatzes oder beim Aufrufen von `.insert()`.

```javascript
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

### <a name="deleting"></a>Vorgehensweise: Löschen von Daten
Um einen Datensatz zu löschen, rufen Sie die `.del()` Methode.  Übergeben Sie die ID ein Objektverweis:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```

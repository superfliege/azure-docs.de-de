---
title: Tutorial zur Indizierung von Azure SQL-Datenbanken in Azure Search | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie eine Azure SQL-Datenbank durchforsten, um durchsuchbare Daten zu extrahieren und einen Azure Search-Index aufzufüllen."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 11/10/2017
ms.author: heidist
ms.openlocfilehash: c5b7d5a5f20a4a70cbbbe43e33fdf65f12cb8e7d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-crawl-an-azure-sql-database-using-azure-search-indexers"></a>Durchforsten einer Azure SQL-Datenbank unter Verwendung von Azure Search-Indexern

In diesem Tutorial erfahren Sie, wie Sie einen Indexer konfigurieren, um durchsuchbare Daten aus einer Azure SQL-Beispieldatenbank zu extrahieren. Ein [Indexer](search-indexer-overview.md) ist eine Komponente von Azure Search, die externe Datenquellen durchforstet und einen [Suchindex](search-what-is-an-index.md) auffüllt. Von allen Indexern wird der Indexer für Azure SQL-Datenbank am häufigsten verwendet. 

Es ist hilfreich, wenn Sie bereits mit dem Konfigurieren von Indexern vertraut sind, da dies das Schreiben und Verwalten von Code vereinfacht. Anstatt ein schemakonformes JSON-Dataset vorzubereiten und mithilfe von Push zu übermitteln, können Sie einen Indexer an eine Datenquelle anfügen und ihn Daten extrahieren und in einen Index einfügen lassen. Außerdem können Sie den Indexer optional auf der Grundlage eines Wiederholungszeitplans ausführen, um Änderungen in der zugrunde liegenden Quelle zu erfassen.

In diesem Tutorial verwenden Sie die [Azure Search-.NET-Clientbibliotheken](https://aka.ms/search-sdk) und eine .NET Core-Konsolenanwendung, um folgende Aufgaben auszuführen:

> [!div class="checklist"]
> * Herunterladen und Konfigurieren der Projektmappe
> * Hinzufügen von Suchdienstinformationen zu Anwendungseinstellungen
> * Vorbereiten eines externen Datasets in Azure SQL-Datenbank 
> * Überprüfen der Index- und Indexerdefinition im Beispielcode
> * Ausführen des Indexercodes zum Importieren von Daten
> * Durchsuchen des Index
> * Anzeigen der Indexerkonfiguration im Portal

## <a name="prerequisites"></a>Voraussetzungen

* Ein aktives Azure-Konto. Falls Sie über kein Azure-Abonnement verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/) registrieren. 

* Ein Azure Search-Dienst. Eine Einrichtungsanleitung finden Sie unter [Erstellen eines Suchdiensts](search-create-service-portal.md).

* Eine Azure SQL-Datenbank, die die externe Datenquelle für einen Indexer bereitstellt. In der Beispielprojektmappe steht eine SQL-Datendatei zum Erstellen der Tabelle zur Verfügung.

* Visual Studio 2017. Sie können die kostenlose [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) verwenden. 

> [!Note]
> Bei Verwendung des kostenlosen Azure Search-Diensts gilt eine Obergrenze von drei Indizes, drei Indexern und drei Datenquellen. In diesem Tutorial wird davon jeweils eine Instanz erstellt. Vergewissern Sie sich, dass Ihr Dienst über genügend freie Kapazität für die neuen Ressourcen verfügt.

## <a name="download-the-solution"></a>Herunterladen der Projektmappe

Die in diesem Tutorial verwendete Indexerprojektmappe stammt aus einer Sammlung von Azure Search-Beispielen, die in einem einzelnen Masterdownload zusammengefasst wurden. Für dieses Tutorial wird die Projektmappe *DotNetHowToIndexers* verwendet.

1. Navigieren Sie im GitHub-Repository mit den Azure-Beispielen zu [**Azure-Samples/search-dotnet-getting-started**](https://github.com/Azure-Samples/search-dotnet-getting-started).

2. Klicken Sie auf **Clone or download** (Klonen oder herunterladen) > **Download ZIP** (ZIP-Datei herunterladen). Standardmäßig wird die Datei in den Ordner „Downloads“ heruntergeladen.

3. Klicken Sie im **Explorer** >  im Ordner **Downloads** mit der rechten Maustaste auf die Datei, und klicken Sie auf **Alle extrahieren**.

4. Deaktivieren Sie den Schreibschutz. Klicken Sie mit der rechten Maustaste auf den Namen des Ordners. Klicken Sie auf **Eigenschaften** > **Allgemein**, und deaktivieren Sie das Attribut **Schreibgeschützt** für den aktuellen Ordner, die Unterordner und die Dateien.

5. Öffnen Sie in **Visual Studio 2017** die Projektmappe *DotNetHowToIndexers.sln*.

6. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die übergeordnete Projektmappe des obersten Knotens, und klicken Sie anschließend auf **NuGet-Pakete wiederherstellen**.

## <a name="set-up-connections"></a>Einrichten von Verbindungen
Verbindungsinformationen für erforderliche Dienste werden in der Datei **appsettings.json** der Projektmappe angegeben. 

Öffnen Sie im Projektmappen-Explorer die Datei **appsettings.json**, um die einzelnen Einstellungen gemäß den Anweisungen in diesem Tutorial auffüllen zu können.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
}
```

### <a name="get-the-search-service-name-and-admin-api-key"></a>Ermitteln von Suchdienstname und Administrator-API-Schlüssel

Den Suchdienstendpunkt und den Schlüssel finden Sie im Portal. Ein Schlüssel ermöglicht den Zugriff auf Dienstvorgänge. Administratorschlüssel ermöglichen Schreibzugriff, der zum Erstellen und Löschen von Objekten wie Indizes und Indexern in Ihrem Dienst erforderlich ist.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und suchen Sie nach den [Suchdiensten für Ihr Abonnement](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

2. Öffnen Sie die Dienstseite.

3. Suchen Sie im oberen Bereich der Hauptseite nach dem Dienstnamen. Im folgenden Screenshot lautet der Name *azs-tutorial*.

   ![Dienstname](./media/search-indexer-tutorial/service-name.png)

4. Kopieren Sie den Namen, und fügen Sie ihn in Visual Studio als ersten Eintrag in **appsettings.json** ein.

  > [!Note]
  > Ein Dienstname ist Teil des Endpunkts, der „search.windows.net“ enthält. Bei Interesse können Sie sich auf der Übersichtsseite unter **Zusammenfassung** die vollständige URL ansehen. Die URL hat folgendes Format: https://Ihr-Dienstname.search.windows.net

5. Kopieren Sie im linken Bereich unter **Einstellungen** > **Schlüssel** einen der Administratorschlüssel, und fügen Sie ihn als zweiten Eintrag in **appsettings.json** ein. Schlüssel sind alphanumerische Zeichenfolgen, die im Rahmen der Bereitstellung für Ihren Dienst generiert werden und für den autorisierten Zugriff auf Dienstvorgänge erforderlich sind. 

  Nach dem Hinzufügen der beiden Einstellungen sollte die Datei in etwa wie folgt aussehen:

  ```json
  {
    "SearchServiceName": "azs-tutorial",
    "SearchServiceAdminApiKey": "A1B2C3D4E5F6G7H8I9J10K11L12M13N14",
    . . .
  }
  ```

## <a name="prepare-an-external-data-source"></a>Vorbereiten einer externen Datenquelle

In diesem Schritt erstellen Sie eine externe Datenquelle, die ein Indexer durchforsten kann. Für dieses Tutorial wird die Datendatei *hotels.sql* aus dem Projektmappenordner „\DotNetHowToIndexers“ verwendet. 

### <a name="azure-sql-database"></a>Azure SQL-Datenbank

Das Dataset in Azure SQL-Datenbank kann über das Azure-Portal unter Verwendung der Datei *hotels.sql* aus dem Beispiel erstellt werden. Azure Search nutzt vereinfachte Rowsets, wie sie beispielsweise auf der Grundlage einer Sicht oder Abfrage generiert werden. Die SQL-Datei in der Beispielprojektmappe erstellt eine einzelne Tabelle und füllt sie auf.

In der folgenden Übung wird davon ausgegangen, dass weder ein Server noch eine Datenbank vorhanden ist. In Schritt 2 werden Sie daher aufgefordert, beides zu erstellen. Optional: Falls Sie bereits über eine Ressource verfügen, können Sie ihr die Hoteltabelle hinzufügen (ab Schritt 4).

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an. 

2. Klicken Sie auf **Neu** > **SQL-Datenbank**, um eine Datenbank, einen Server und eine Ressourcengruppe zu erstellen. Dabei können Sie die Standardwerte und den günstigsten Tarif verwenden. Die Erstellung eines Servers hat unter anderem den Vorteil, dass Sie einen Administratorbenutzernamen und ein Kennwort angeben können. Diese werden in einem späteren Schritt zum Erstellen und Laden von Tabellen benötigt.

   ![Seite für die neue Datenbank](./media/search-indexer-tutorial/indexer-new-sqldb.png)

3. Klicken Sie auf **Erstellen**, um den neuen Server und die Datenbank bereitzustellen. Warten Sie, bis Server und Datenbank bereitgestellt wurden.

4. Öffnen Sie für Ihre neue Datenbank die Seite „SQL-Datenbank“, sofern sie nicht bereits geöffnet ist. Der Ressourcenname sollte *SQL-Datenbank* lauten (nicht *SQL Server*).

  ![SQL-Datenbankseite](./media/search-indexer-tutorial/hotels-db.png)

4. Klicken Sie auf der Befehlsleiste auf **Tools** > **Abfrage-Editor**.

5. Klicken Sie auf **Anmelden**, und geben Sie den Benutzernamen und das Kennwort des Serveradministrators ein.

6. Klicken Sie auf **Abfrage öffnen**, und navigieren Sie zum Speicherort von *hotels.sql*. 

7. Wählen Sie die Datei aus, und klicken Sie auf **Öffnen**. Das Skript sollte in etwa wie im folgenden Screenshot aussehen:

  ![SQL-Skript](./media/search-indexer-tutorial/sql-script.png)

8. Klicken Sie auf **Ausführen**, um die Abfrage auszuführen. Im Ergebnisbereich sollte eine Erfolgsmeldung für drei Zeilen angezeigt werden.

9. Wenn Sie ein Rowset aus dieser Tabelle zurückgeben möchten, können Sie als Überprüfungsschritt die folgende Abfrage ausführen:

   ```sql
   SELECT HotelId, HotelName, Tags FROM Hotels
   ```
   Die Prototypabfrage (`SELECT * FROM Hotels`) funktioniert im Abfrage-Editor nicht. Die Beispieldaten enthalten geografische Koordinaten im Feld „Standort“. Diese werden derzeit vom Editor nicht verarbeitet. Durch Ausführen der folgenden Anweisung erhalten Sie eine Liste mit weiteren abfragbaren Spalten: `SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.Hotels')`

10. Sie verfügen nun über ein externes Dataset. Kopieren Sie als Nächstes die ADO.NET-Verbindungszeichenfolge für die Datenbank. Navigieren Sie auf der Seite „SQL-Datenbank“ Ihrer Datenbank zu **Einstellungen** > **Verbindungszeichenfolgen**, und kopieren Sie die ADO.NET-Verbindungszeichenfolge.
 
  Eine ADO.NET-Verbindungszeichenfolge sieht wie im folgenden Beispiel aus und muss mit einem gültigen Datenbanknamen, Benutzernamen und Kennwort versehen werden:

  ```sql
  Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
  ```
11. Fügen Sie in Visual Studio in der Datei **appsettings.json** als dritten Eintrag die Verbindungszeichenfolge in „AzureSqlConnectionString“ ein.

    ```json
    {
      "SearchServiceName": "azs-tutorial",
      "SearchServiceAdminApiKey": "A1B2C3D4E5F6G7H8I9J10K11L12M13N14",
      "AzureSqlConnectionString": "Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security  Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

## <a name="understand-index-and-indexer-code"></a>Grundlegendes zum Code für Index und Indexer

Der Code kann jetzt erstellt und ausgeführt werden. Nehmen Sie sich zuvor kurz Zeit, um sich die Index- und Indexerdefinition für dieses Beispiel etwas genauer anzusehen. Der relevante Code befindet sich in zwei Dateien:

  + **hotel.cs**: Enthält das Schema, das den Index definiert.
  + **Program.cs**: Enthält die Funktionen zum Erstellen und Verwalten von Strukturen in Ihrem Dienst.

### <a name="in-hotelcs"></a>hotel.cs

Das Indexschema definiert die Sammlung „fields“ einschließlich Attributen zur Angabe zulässiger Vorgänge (beispielsweise, ob in einem Feld eine Volltextsuche möglich ist oder ob es gefiltert oder sortiert werden kann), wie in der folgenden Felddefinition für „HotelName“ zu sehen. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Ein Schema kann auch andere Elemente enthalten. Hierzu zählen beispielsweise Bewertungsprofile zur Erhöhung einer Suchbewertung, benutzerdefinierte Analysen und andere Konstrukte. Für unsere Zwecke reicht jedoch eine einfache Schemadefinition, die lediglich Felder aus den Beispieldatasets enthält.

In diesem Tutorial ruft der Indexer Daten aus einer einzelnen Datenquelle ab. In der Praxis können Sie mehrere Indexer an den gleichen Index anfügen und so einen konsolidierten durchsuchbaren Index erstellen, der auf mehreren Datenquellen und Indexern basiert. Abhängig von Ihren Flexiblitätsanforderungen können Sie das gleiche Index-Indexer-Paar nutzen und nur die Datenquellen variieren oder einen einzelnen Index mit verschiedenen Indexer-/Datenquellenkombinationen verwenden.

### <a name="in-programcs"></a>Program.cs

Das Hauptprogramm enthält Funktionen für alle drei repräsentativen Datenquellen. Für Azure SQL-Datenbank sind besonders folgende Objekte hervorzuheben:

  ```csharp
  private const string IndexName = "hotels";
  private const string AzureSqlHighWaterMarkColumnName = "RowVersion";
  private const string AzureSqlDataSourceName = "azure-sql";
  private const string AzureSqlIndexerName = "azure-sql-indexer";
  ```

Zu den Objekten, die in Azure Search unabhängig voneinander angezeigt, konfiguriert und gelöscht werden können, zählen Indizes, Indexer und Datenquellen (*hotels*, *azure-sql-indexer* bzw. *azure-sql*). 

Die Spalte *AzureSqlHighWaterMarkColumnName* verdient besondere Erwähnung: Sie enthält Informationen zur Erkennung von Änderungen, auf deren Grundlage der Indexer ermittelt, ob sich eine Zeile seit der letzten Indizierungsworkload geändert hat. [Änderungserkennungsrichtlinien](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) werden nur in Indexern unterstützt und variieren je nach Datenquelle. Für Azure SQL-Datenbank können Sie sich abhängig von Ihren Datenbankanforderungen für eine von zwei Richtlinien entscheiden.

Der folgende Code zeigt die Methoden in „Program.cs“ zum Erstellen einer Datenquelle und eines Indexers. Der Code sucht und löscht vorhandene Ressourcen mit dem gleichen Namen (unter der Annahme, dass Sie dieses Programm ggf. mehrmals ausführen).

  ```csharp
  private static string SetupAzureSqlIndexer(SearchServiceClient serviceClient, IConfigurationRoot configuration)
  {
    Console.WriteLine("Deleting Azure SQL data source if it exists...");
    DeleteDataSourceIfExists(serviceClient, AzureSqlDataSourceName);

    Console.WriteLine("Creating Azure SQL data source...");
    DataSource azureSqlDataSource = CreateAzureSqlDataSource(serviceClient, configuration);

    Console.WriteLine("Deleting Azure SQL indexer if it exists...");
    DeleteIndexerIfExists(serviceClient, AzureSqlIndexerName);

    Console.WriteLine("Creating Azure SQL indexer...");
    Indexer azureSqlIndexer = CreateIndexer(serviceClient, AzureSqlDataSourceName, AzureSqlIndexerName);

    return azureSqlIndexer.Name;
  }
  ```

Wie Sie sehen, sind die Indexer-API-Aufrufe plattformunabhängig. (Einzige Ausnahme: [DataSourceType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) zum Angeben der Art des aufzurufenden Crawlers.)

## <a name="run-the-indexer"></a>Ausführen des Indexers

In diesem Schritt wird das Programm kompiliert und ausgeführt. 

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **DotNetHowToIndexers**, und klicken Sie auf **Erstellen**.
2. Klicken Sie erneut mit der rechten Maustaste auf **DotNetHowToIndexers**, und klicken Sie anschließend auf **Debuggen** > **Neue Instanz starten**.

Das Programm wird im Debugmodus ausgeführt. Ein Konsolenfenster gibt Aufschluss über den Status der einzelnen Vorgänge.

  ![SQL-Skript](./media/search-indexer-tutorial/console-output.png)

Der Code wird lokal in Visual Studio ausgeführt und stellt eine Verbindung mit Ihrem Suchdienst in Azure her, der wiederum unter Verwendung der Verbindungszeichenfolge eine Verbindung mit Azure SQL-Datenbank herstellt und das Dataset abruft. Durch die Anzahl von Vorgängen gibt es mehrere potenzielle Fehlerquellen. Prüfen Sie im Falle eines Fehlers allerdings zunächst Folgendes:

+ Die Angabe der Suchdienst-Verbindungsinformationen beschränkt sich in diesem Tutorial auf den Dienstnamen. Wenn Sie die vollständige URL eingegeben haben, werden die Vorgänge bei der Indexerstellung mit einem Verbindungsfehler beendet.

+ Datenbank-Verbindungsinformationen in **appsettings.json**: Hierbei muss es sich um die ADO.NET-Verbindungszeichenfolge aus dem Portal handeln, die mit einem gültigen Benutzernamen und Kennwort für die Datenbank versehen wurde. Das Benutzerkonto muss über Datenabrufberechtigungen verfügen.

+ Ressourceneinschränkungen: Bedenken Sie, dass für den gemeinsam genutzten (kostenlosen) Dienst eine Obergrenze von drei Indizes, Indexern und Datenquellen gilt. Ist die Obergrenze für einen Dienst erreicht, können keine neuen Objekte mehr erstellt werden.

## <a name="search-the-index"></a>Durchsuchen des Index 

Klicken Sie im Azure-Portal im oberen Bereich der Übersichtsseite des Suchdiensts auf **Suchexplorer**, um einige Abfragen für den neuen Index zu übermitteln.

1. Klicken Sie im oberen Bereich auf **Index ändern**, um den Index *hotels* auszuwählen.

2. Klicken Sie auf die Schaltfläche **Suchen**, um eine leere Suchabfrage auszuführen. 

  Die drei Einträge in Ihrem Index werden als JSON-Dokumente zurückgegeben. Der Suchexplorer gibt Dokumente im JSON-Format zurück, sodass Sie sich die gesamte Struktur ansehen können.

3. Geben Sie als Nächstes eine Suchzeichenfolge ein: `search=river&$count=true`. 

  Diese Abfrage führt eine Volltextsuche nach dem Begriff `river` durch, und das Ergebnis enthält die Anzahl passender Dokumente. Die Rückgabe der Anzahl passender Dokumente ist hilfreich in Testszenarien mit einem umfangreichen Index mit tausenden oder Millionen von Dokumenten. In diesem Fall entspricht der Abfrage lediglich ein einzelnes Dokument.

4. Geben Sie abschließend eine Suchzeichenfolge ein, die die JSON-Ausgabe auf relevante Felder beschränkt: `search=river&$count=true&$select=hotelId, baseRate, description`. 

  Die Abfrageantwort wird auf die ausgewählten Felder beschränkt, um eine präzisere Ausgabe zu erhalten.

## <a name="view-indexer-configuration"></a>Anzeigen der Indexerkonfiguration

Alle Indexer werden im Portal angezeigt. Das gilt auch für den, den Sie gerade programmgesteuert erstellt haben. Sie können eine Indexerdefinition öffnen und die dazugehörige Datenquelle anzeigen oder einen Aktualisierungszeitplan zur Erfassung neuer und geänderter Zeilen konfigurieren.

1. Öffnen Sie die Übersichtsseite Ihres Azure Search-Diensts.
2. Scrollen Sie nach unten zu den Kacheln für **Indexer** und **Datenquellen**.
3. Klicken Sie auf eine Kachel, um eine Liste der einzelnen Ressourcen zu öffnen. Sie können einzelne Indexer oder Datenquellen auswählen, um Konfigurationseinstellungen anzuzeigen oder zu ändern.

  ![Kachel „Indexer“ und „Datenquellen“](./media/search-indexer-tutorial/tiles-portal.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Dienste nicht mehr benötigen, gehen Sie im Azure-Portal wie folgt vor, um alle Ressourcen zu löschen, die im Rahmen dieses Tutorials erstellt wurden: 

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Ressourcengruppe löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen und spezifische Aufgaben für andere unterstützte Datenquellen finden Sie in den folgenden Artikeln:

* [Azure SQL-Datenbank oder SQL Server auf einem virtuellen Azure-Computer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Indizieren von CSV-Blobs mit Azure Search-Blobindexer](search-howto-index-csv-blobs.md)
* [Indizieren von JSON-Blobs mit Azure Search-Blobindexer](search-howto-index-json-blobs.md)


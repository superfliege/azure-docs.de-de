---
title: Migration von Datenbankressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Datenbankressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
---

# <a name="migration-of-database-resources-from-azure-germany-to-global-azure"></a>Migration von Datenbankressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure-Datenbankressourcen von Azure Deutschland zu Azure weltweit.

## <a name="azure-sql-database"></a>Azure SQL-Datenbank

Um Azure SQL-Datenbanken zu migrieren, können Sie (für kleinere Workloads) die Exportfunktion verwenden, um eine BACPAC-Datei zu erstellen. Eine BACPAC-Datei ist eine komprimierte Datei (ZIP-Datei) mit Metadaten und Daten aus der SQL Server-Datenbank. Nach der Erstellung können Sie sie in die Zielumgebung kopieren (z.B. mit AzCopy) und die Importfunktion zum erneuten Erstellen der Datenbank verwenden. Beachten Sie die folgenden Überlegungen (weitere Informationen finden Sie in den unten angegebenen Links):

- Damit ein Export transaktionskonsistent ist, stellen Sie sicher, dass entweder
  - während des Exports keine Schreibaktivität auftritt oder
  - der Export aus einer transaktionskonsistenten Kopie der Azure SQL-Datenbank erfolgt.
- Für den Export in Blobspeicher ist die Größe der BACPAC-Datei auf 200 GB beschränkt. Führen Sie für größere BACPAC-Datei einen Export in lokalen Speicher aus.
- Falls der Exportvorgang aus der Azure SQL-Datenbank länger als 20 Stunden dauert, wird er unter Umständen abgebrochen. Suchen Sie in den folgenden Links nach Hinweisen, wie Sie die Leistung erhöhen können.

> [!NOTE]
> Die Verbindungszeichenfolge ändert sich, da sich der DNS-Name des Servers ändert.

### <a name="next-steps"></a>Nächste Schritte

- [Exportieren der Datenbank in eine BACPAC-Datei](../sql-database/sql-database-export.md)
- [Importieren der BACPAC-Datei in eine Datenbank](../sql-database/sql-database-import.md)

### <a name="references"></a>Referenzen

- [Dokumentation zur Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/)







## <a name="sql-data-warehouse"></a>SQL Data Warehouse

Es gibt keine speziellen Migrationsoptionen für SQL Data Warehouse. Befolgen Sie die Anweisungen unter [Azure SQL-Datenbank](#azure-sql-database).







## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Mit dem Azure Cosmos DB-Datenmigrationstool können Sie problemlos Daten zu Azure Cosmos DB migrieren. Das Azure Cosmos DB-Datenmigrationstool ist eine Open-Source-Lösung, mit der Daten aus verschiedenen Quellen in Azure Cosmos DB importiert werden können.

Das Tool steht als GUI-Tool oder als Befehlszeilentool zur Verfügung. Der Quellcode ist im GitHub-Repository für das [Azure Cosmos DB-Datenmigrationstool](https://github.com/azure/azure-documentdb-datamigrationtool) verfügbar, und eine kompilierte Version steht im [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=46436) zur Verfügung.

Die folgenden Schritte werden empfohlen:

- Führen Sie eine Überprüfung der Anforderungen an die Anwendungsbetriebszeit und der Kontokonfigurationen aus, um den richtigen Aktionsplan zu empfehlen.
- Führen Sie die Schritte zum Klonen der Kontokonfigurationen aus Azure Deutschland in die neue Region aus, indem Sie das Tool ausführen.
- Wenn ein Wartungsfenster möglich ist, führen Sie die Schritte zum Kopieren von Daten aus der Quelle in das Ziel aus, indem Sie das Tool ausführen.
- Wenn kein Wartungsfenster möglich ist, führen Sie die Schritte zum Kopieren von Daten aus der Quelle in das Ziel aus, indem Sie das Tool und den von uns empfohlenen Prozess ausführen.
  - Nehmen Sie Änderungen an den Lese-/Schreibvorgängen in der Anwendung mit einem konfigurationsgesteuerten Ansatz vor.
  - Führen Sie die erste Synchronisierung aus.
  - Richten Sie inkrementelle Synchronisierung/Berücksichtigung des Änderungsfeeds ein.
  - Verweisen Sie die Lesevorgänge auf das neue Konto, und überprüfen Sie die Anwendung.
  - Beenden Sie Schreibvorgänge in das alte Konto, überprüfen Sie, ob der Änderungsfeed berücksichtigt wird, und verweisen Sie dann Schreibvorgänge auf die neuen Konten.
  - Beenden Sie das Tool, und löschen Sie das alte Konto.
- Führen Sie die Schritte zum Ausführen des Tools aus, um zu überprüfen, ob die Daten in den alten und neuen Konten konsistent sind.


### <a name="references"></a>Referenzen

- [Azure Cosmos DB](../cosmos-db/introduction.md)
- [Importieren von Daten in Azure Cosmos DB](../cosmos-db/import-data.md)








## <a name="redis-cache"></a>Redis Cache

Es gibt abhängig von Ihren Anforderungen einige Optionen für die Migration zu Azure weltweit.

### <a name="option-1-data-loss-ok-create-new-instance"></a>Option 1: Datenverlust ist OK, Erstellen einer neuen Instanz

Dieser Ansatz ist am sinnvollsten, wenn

- Redis als vorübergehender Datencache verwendet wird und
- Ihre Anwendung die Cachedaten in der neuen Region automatisch erneut auffüllt.

Folgen Sie diesen Schritten:

- Erstellen Sie eine neue Redis-Instanz in der neuen Zielregion.
- Ändern Sie Ihre Anwendung so, dass die neue Instanz in der neuen Region verwendet wird.
- Löschen Sie die alte Redis-Instanz in der Quellregion.

### <a name="option-2-copy-data-from-source-instance-to-target-instance"></a>Option 2: Kopieren von Daten aus der Quellinstanz in die Zielinstanz

Ein Mitglied des Azure Redis-Teams hat ein Open-Source-Tool geschrieben, das Daten aus einer Redis-Instanz in eine andere kopiert, ohne Import- oder Exportfunktionalität zu erfordern.

- Erstellen Sie einen virtuellen Computer in der Quellregion. Wenn Ihr Dataset in Redis groß ist, stellen Sie sicher, dass Sie eine relativ leistungsfähige VM-Größe auswählen, um die für den Kopiervorgang benötigte Zeit zu minimieren.
- Erstellen Sie eine neue Redis-Instanz in der Zielregion.
- Leeren Sie Daten aus der **Ziel**instanz (stellen Sie sicher, dass die Leerung **NICHT** aus der **Quell**instanz erfolgt. Das Leeren ist erforderlich, da das Kopiertool vorhandene Schlüssel am Zielspeicherort **nicht überschreibt**.
- Verwenden Sie ein Tool wie das folgende, um automatisch Daten aus der Redis-Quellinstanz in die Redis-Zielinstanz zu kopieren: [Source](https://github.com/deepakverma/redis-copy), [Download](https://github.com/deepakverma/redis-copy/releases/download/alpha/Release.zip).

> [!NOTE]
> Dieser Vorgang kann abhängig von der Größe des Datasets sehr lange dauern.

### <a name="option-3-export-from-source-instance-import-into-destination-instance"></a>Option 3: Exportieren aus der Quellinstanz, Importieren in die Zielinstanz

Dieser Ansatz nutzt die Vorteile von Features, die nur im Premium-Tarif verfügbar sind.

- Erstellen Sie eine neue Redis-Instanz des Premium-Tarifs in der neuen Zielregion. Verwenden Sie die gleiche Größe wie die Redis-Quellinstanz.
- [Exportieren Sie Daten aus dem Quellcache](../redis-cache/cache-how-to-import-export-data.md), oder verwenden Sie das [PowerShell-Cmdlet Export-AzureRmRedisCache](/powershell/module/azurerm.rediscache/export-azurermrediscache?view=azurermps-6.4.0).

> [!NOTE]
> Das Exportspeicherkonto muss sich in derselben Region wie die Cacheinstanz befinden.

- Kopieren Sie exportierte Blobs in ein Speicherkonto in der Zielregion (z.B. mithilfe von AzCopy).
- [Importieren Sie Daten in den Zielcache](../redis-cache/cache-how-to-import-export-data.md), oder verwenden Sie das [PowerShell-Cmdlet Import-AzureRmRedisCache](/powershell/module/azurerm.rediscache/import-azurermrediscache?view=azurermps-6.4.0).
- Konfigurieren Sie Ihre Anwendung für die Verwendung der Redis-Zielinstanz neu.

### <a name="option-4-write-data-to-two-redis-instances-read-from-one-instance"></a>Option 4: Schreiben von Daten in zwei Redis-Instanzen, Lesen aus einer Instanz

Bei diesem Ansatz müssen Sie Ihre Anwendung ändern. Sie muss Daten in mehrere Cacheinstanzen schreiben, während sie aus einer der Cacheinstanzen liest. Dieser Ansatz ist sinnvoll, wenn die in Redis gespeicherten Daten
- in regelmäßigen Abständen aktualisiert werden, 
- alle Daten in die Redis-Zielinstanz geschrieben werden und
- genügend Zeit für die Aktualisierung aller Daten verfügbar ist.

### <a name="references"></a>Referenzen

- [Azure Redis Cache: Übersicht](../redis-cache/cache-overview.md)

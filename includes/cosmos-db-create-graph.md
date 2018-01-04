Sie können nun mithilfe des Daten-Explorer-Tools im Azure-Portal eine Diagrammdatenbank erstellen. 

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Daten-Explorer (Vorschau)**.

2. Wählen Sie unter **Daten-Explorer (Vorschau)** die Option **New Graph** (Neuer Graph). Geben Sie auf der Seite dann die folgenden Informationen ein:

    ![Daten-Explorer im Azure-Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Einstellung|Empfohlener Wert|BESCHREIBUNG
    ---|---|---
    Datenbank-ID|sample-database|Geben Sie *sample-database* als Namen für die neue Datenbank ein. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/ \ # ?` noch nachgestellte Leerzeichen enthalten.
    Diagramm-ID|sample-graph|Geben Sie *sample-graph* als Namen für die neue Sammlung ein. Für Diagrammnamen gelten dieselben Zeichenanforderungen wie für Datenbank-IDs.
    Speicherkapazität| 10 GB|Behalten Sie den Standardwert bei. Dies ist die Speicherkapazität der Datenbank.
    Durchsatz|400 RUs|Behalten Sie den Standardwert bei. Sie können den Durchsatz später zentral hochskalieren, wenn Sie Latenzen reduzieren möchten.

3. Wählen Sie **OK**, nachdem Sie das Formular ausgefüllt haben.

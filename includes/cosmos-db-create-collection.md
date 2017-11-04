Sie können nun mithilfe des Daten-Explorer-Tools im Azure-Portal eine Datenbank und eine Sammlung erstellen. 

1. Klicken Sie auf **Daten-Explorer** > **Neue Sammlung**. 
    
    Der Bereich **Sammlung hinzufügen** wird ganz rechts angezeigt. Möglicherweise müssen Sie nach rechts scrollen, damit Sie ihn sehen.

    ![Daten-Explorer im Azure-Portal, Blatt „Sammlung hinzufügen“](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. Geben Sie auf der Seite **Sammlung hinzufügen** die Einstellungen für die neue Sammlung ein.

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Datenbank-ID|Aufgaben|Geben Sie *Tasks* als Namen für die neue Datenbank ein. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder /, \\, #, ? noch nachgestellte Leerzeichen enthalten.
    Sammlungs-ID|Items|Geben Sie *Items* als Namen für die neue Sammlung ein. Für Sammlungs-IDs gelten dieselben Zeichenanforderungen wie für Datenbanknamen.
    Speicherkapazität| Fixed (10 GB)|Ändern Sie den Wert in **Fest (10 GB)**. Dieser Wert gibt die Speicherkapazität der Datenbank an.
    Durchsatz|400 RU|Ändern Sie den Durchsatz in 400 Anforderungseinheiten pro Sekunde (RU/s). Sie können den Durchsatz später zentral hochskalieren, wenn Sie Wartezeiten reduzieren möchten.
    Partitionsschlüssel|/category|Ein Partitionsschlüssel, der Daten gleichmäßig auf alle Partitionen verteilt. Die Auswahl des richtigen Partitionsschlüssels ist wichtig für die Erstellung einer leistungsfähigen Sammlung. Weitere Informationen finden Sie unter [Entwerfen für Partitionierung](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

    Klicken Sie auf **OK**.

    Im Daten-Explorer werden die neue Datenbank und die neue Sammlung angezeigt.

    ![Daten-Explorer mit der neuen Datenbank und der neuen Sammlung](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)
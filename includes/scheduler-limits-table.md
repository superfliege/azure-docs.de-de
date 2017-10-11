In der folgenden Tabelle werden die einzelnen von der wichtigsten Kontingente, Limits, Standardwerte und Drosselungen in Azure Scheduler beschrieben.

| Ressource | Beschreibung des Limits |
| --- | --- |
| **Auftragsgröße** |Maximale Auftragsgröße beträgt 16K. Wenn ein Auftrag, der größer als diese Grenzwerte PUT oder Patch-Vorgang ergibt, wird eine 400 Statuscode "Ungültige Anforderung" zurückgegeben. |
| **Länge des Anforderungs-URL** |Maximale Größe der Anforderungs-URL beträgt 2048 Zeichen. |
| **Größe des aggregierten Headers** |Größe des maximalen aggregierten Headers beträgt 4096 Zeichen. |
| **Headeranzahl** |Die maximale headeranzahl beträgt 50 Header. |
| **Textgröße** |Die maximale Textlänge beträgt 8192 Zeichen. |
| **Spanne für Wiederholungen** |Maximale Spanne für Wiederholungen beträgt 18 Monate. |
| **Zeit bis zur Startzeit** |Maximale "Zeit bis zur Startzeit" beträgt 18 Monate. |
| **Auftragsverlauf** |Maximale Größe des Antworttexts im Auftragsverlauf gespeichert beträgt 2048 Bytes. |
| **Häufigkeit** |Das maximale standardmäßige häufigkeitskontingent beträgt 1 Stunde in einer kostenlosen auftragsauflistung und 1 Minute in einer standardauftragsauflistung. Die maximale Häufigkeit kann für eine auftragsauflistung kleiner als der maximale Wert konfiguriert werden. Alle Aufträge in der auftragssammlung sind beschränkt auf die auftragssammlung festgelegte Wert. Wenn Sie, beim Erstellen eines Auftrags mit einer höheren Häufigkeit als die maximale Häufigkeit der auftragsauflistung versuchen schlägt die Anforderung mit einem Statuscode "409 Konflikt" fehl. |
| **Aufträge** |Das maximale standardmäßige auftragskontingent ist 5 Aufträge in einer kostenlosen auftragsauflistung und 50 Aufträge in einer standardauftragsauflistung. Die maximale Anzahl der Aufträge ist für eine auftragsauflistung konfigurierbar. Alle Aufträge in der auftragssammlung sind beschränkt auf die auftragssammlung festgelegte Wert. Wenn Sie versuchen, mehr Aufträgen als das maximale auftragskontingent Aufträge erstellen, schlägt die Anforderung mit einem Statuscode "409 Konflikt". |
| **Auftragssammlungen** |Maximale Anzahl von auftragssammlung pro Abonnement ist 200.000. |
| **Verlaufsbeibehaltung Auftrag** |Auftragsverlauf wird bis zum letzten 1000 Ausführungen oder bis zu zwei Monate beibehalten. |
| **Abgeschlossene und fehlerhafte Auftrag Aufbewahrung** |Abgeschlossene und fehlerhafte Aufträge werden 60 Tage lang aufbewahrt. |
| **Timeout** |Es ist eine statische (nicht konfigurierbares) Anforderungstimeout von 60 Sekunden für HTTP-Aktionen. Folgen Sie für länger ausgeführten Vorgänge asynchrone HTTP-Protokolle. z. B. sofort einen 202 zurückgegeben, aber weiterhin im Hintergrund arbeiten. |


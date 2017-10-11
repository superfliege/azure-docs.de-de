
>[!NOTE]
>Log Analytics hieß früher Operational Insights.
>
>

Die folgenden Beschränkungen gelten für protokollanalyseressourcen pro Abonnement:

| Ressource | Standardlimit | Kommentare
| --- | --- | --- |
| Anzahl der freien Arbeitsbereiche pro Abonnement | 10 | Dieser Grenzwert kann nicht vergrößert werden. |
| Anzahl der bezahlten Arbeitsbereiche pro Abonnement | N/V | Sie sind durch die Anzahl von Ressourcen innerhalb einer Ressourcengruppe und die Anzahl von Ressourcengruppen pro Abonnement beschränkt. | 


Die folgenden Beschränkungen gelten für jeden Arbeitsbereich für Protokollanalyse:

|  | Free | Standard | Premium | Eigenständig | OMS |
| --- | --- | --- | --- | --- | --- |
| Datenvolume pro Tag erfasst |500 MB<sup>1</sup> |Keine |Keine | Keine | Keine
| Beibehaltungsdauer |7 Tage |1 Monat |12 Monate | 1 Monat<sup>2</sup> | 1 Monat <sup>2</sup>|

<sup>1</sup> bei Kunden, die Daten-Übertragung ihrer 500 MB tagesgrenze erreicht, die Datenanalyse beendet und wird zu Beginn des nächsten Tages fortgesetzt. Täglich basiert auf UTC.

<sup>2</sup> Beibehaltungsdauer der Daten für die eigenständige und die OMS-Preisgestaltung Pläne und 730 Tagen erhöht werden kann.

| Kategorie | Limits | Kommentare
| --- | --- | --- |
| Datensammler-API | Maximale Größe einer einzelnen POST-Anforderung ist 30 MB<br>Maximale Größe für Feldwerte beträgt 32 KB | Teilen Sie größere Volumes in mehrere Beiträge<br>Felder, die länger als 32 KB werden abgeschnitten. |
| Search-API | 5000 für nicht-aggregierte Daten zurückgegebenen Datensätze<br>500000 Einträge für die aggregierten Daten | Aggregierte Daten werden eine Suche, die enthält die `measure` Befehl
 

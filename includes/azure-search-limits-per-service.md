Speicher wird eingeschränkt, indem Speicherplatz oder eine harte Grenze auf die *Höchstanzahl* von Indizes oder Dokumenten, welcher Fall zuerst eintritt.

| Ressource | Free | Einfach | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Vereinbarung zum Servicelevel (SLA) |No <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Speicher pro partition |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partitionen pro Dienst |N/V |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitionsgröße |N/V |2 GB |25 GB |100 GB |200 GB |200 GB |
| Replikate |N/V |3 |12 |12 |12 |12 |
| Maximale Indizes |3 |5 |50 |200 |200 |1.000 pro Partition oder 3000 pro Dienst |
| Maximale Indexer |3 |5 |50 |200 |200 |Keine Indexer-Unterstützung |
| Maximale Datenquellen |3 |5 |50 |200 |200 |Keine Indexer-Unterstützung |
| Maximale Dokumente |10,000 |1 million |15 Millionen pro Partition oder 180 Millionen pro Dienst |60 Millionen pro Partition oder 720 Millionen pro Dienst |120 Millionen pro Partition oder 1.4 Milliarden pro Dienst |1 Million pro Index oder 200 Millionen pro partition |
| Geschätzte Abfragen pro Sekunde (Abfragen pro Sekunde) |N/V |~ 3 pro Replikat |~ 15 pro Replikat |~ 60 pro Replikat |~ 60 pro Replikat |> 60 pro Replikat |

<sup>1</sup> freien Ebenen und Vorschau-Funktionen nicht mit Vereinbarungen zum Servicelevel (SLAs) stammen. Für alle abrechenbare Dienstebenen werden SLAs wirksam, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage (gelesen)-SLA erforderlich. Mindestens drei Replikate sind für die Abfrage und Indizierung SLA (Lese-/ Schreibzugriff) erforderlich. Die Anzahl von Partitionen ist keine SLA-Aspekt. 

<sup>2</sup> S3 HD hat einen festen Grenzwert von 3 Partitionen die ist niedriger als der Grenzwert der Partition für S3. Die untere Grenze für die Partition tritt ein, da die Indexanzahl für S3 HD wesentlich höher. Dienstbeschränkungen für Computerressourcen (Speicher- und Verarbeitungsvorgängen) und den Inhalt (Indizes und Dokumente), den Inhalt Grenzwert vorhanden sind, werden zuerst erreicht.

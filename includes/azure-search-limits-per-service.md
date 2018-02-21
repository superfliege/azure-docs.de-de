Der Speicher ist durch den Festplattenspeicher oder einen festen Grenzwert für die *maximale Anzahl* von Indizes oder Dokumenten beschränkt (je nachdem, was zuerst erreicht wird).

| Ressource | Kostenlos | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Vereinbarung zum Servicelevel (SLA) |Nein <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Speicherkapazität pro Partition |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partitionen pro Dienst |N/V |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitionsgröße |N/V |2 GB |25 GB |100 GB |200 GB |200 GB |
| Replikate |N/V |3 |12 |12 |12 |12 |
| Maximale Anzahl von Indizes |3 |5 <sup>3</sup>|50 |200 |200 |1000 pro Partition oder 3000 pro Dienst |
| Maximale Anzahl von Indexern |3 |5 <sup>3</sup>|50 |200 |200 |Keine Indexer-Unterstützung |
| Maximale Datenquellen |3 |5 <sup>3</sup>|50 |200 |200 |Keine Indexer-Unterstützung |
| Maximale Anzahl von Dokumenten <sup>3</sup> |10.000 |1 Mio. |15 Millionen pro Partition oder 180 Millionen pro Dienst |60 Millionen pro Partition oder 720 Millionen pro Dienst |120 Millionen pro Partition oder 1,4 Milliarden pro Dienst |1 Millionen pro Index oder 200 Millionen pro Partition |

<sup>1</sup> Für Funktionen der Free- und Vorschauversion gilt keine Vereinbarung zum Servicelevel (Service Level Agreement, SLA). Für alle abrechenbaren Tarife gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage-SLA (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLA (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für die SLA nicht berücksichtigt. 

<sup>2</sup> Für S3 HD gilt ein festes Limit von drei Partitionen. (Das Limit ist also geringer als bei S3.) Die Untergrenze für Partitionen wurde festgelegt, da die Indexanzahl für S3 HD wesentlich höher ist. Da sowohl für Computeressourcen (Speicher und Verarbeitung) als auch für Inhalte (Indizes und Dokumente) Dienstlimits gelten, wird das Inhaltslimit zuerst erreicht.

>[!Important]
> **<sup>3</sup>**  Ab Ende 2017 werden neu erstellte Azure Search-Dienste mit leistungsfähigeren Hardwarekonfigurationen bereitgestellt, durch die in bestimmten Regionen – Brasilien (Süden), Kanada (Mitte), Indien (Mitte), USA (Osten, Westen, Norden/Mitte, Süden/Mitte), Südostasien, Vereinigtes Königreich (Süden) und Europa (Norden, Westen) – die Änderung einiger Limits ermöglicht wurde:
>
>* Bei Suchdiensten mit den Tarifen „Basic“ und „Standard“, die nach Ende 2017 erstellt wurden, gibt es keine Limits hinsichtlich der Anzahl von Dokumenten; für diese Dienste werden nur Speichergrenzwerte erzwungen. 
>* Bei S3 High Density-Diensten, die nach Ende 2017 erstellt wurden, wurde das Limit „200 Millionen Dokumente pro Partition“ entfernt, das Limit „1 Million Dokumente pro Index“ bleibt jedoch bestehen.
>* Bei Diensten mit dem Tarif „Basic“, die nach Ende 2017 erstellt wurden, wurde das Limit auf 15 Indizes, Datenquellen und Indexer erhöht.
>
>Weitere Informationen zu den einzelnen Limits, die für einen vorhandenen Dienst gelten, erhalten Sie im Azure-Portal, wo Sie auf der Übersichtsseite des Diensts die Informationen zu den Limits anzeigen können.
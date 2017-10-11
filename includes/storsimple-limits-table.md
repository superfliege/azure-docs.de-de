<!--author=alkohli last changed: 12/15/15-->

| Limit-Bezeichner | Grenzwert | Kommentare |
| --- | --- | --- |
| Maximale Anzahl von Anmeldeinformationsobjekten |64 | |
| Maximale Anzahl von volumecontainern |64 | |
| Maximale Anzahl von volumes |255 | |
| Maximale Anzahl von Zeitplänen pro bandbreitenvorlage |168 |Einen Zeitplan für jede Stunde, jeden Tag der Woche (24 * 7). |
| Maximale Größe eines mehrstufigen Volumes auf physischen Geräten |64 TB für 8100 und 8600 |8100 und 8600 sind physische Geräte. |
| Maximale Größe eines mehrstufigen Volumes auf virtuellen Geräten in Azure |30 TB für 8010 <br></br> 64 TB für 8020 |8010 und 8020 sind virtuelle Geräte in Azure, die Standard und Premium-Speicher bzw. verwenden. |
| Maximale Größe für ein lokales Volume auf physischen Geräten |9 TB für 8100 <br></br> 24 TB für 8600 |8100 und 8600 sind physische Geräte. |
| Maximale Anzahl von iSCSI-Verbindungen |512 | |
| Maximale Anzahl von iSCSI-Verbindungen von Initiatoren |512 | |
| Maximale Anzahl von zugriffssteuerungsdatensätzen pro Gerät |64 | |
| Maximale Anzahl von Volumes pro Sicherungsrichtlinie |24 | |
| Maximale Anzahl von Sicherungen, die pro Sicherungsrichtlinie beibehalten |64 | |
| Maximale Anzahl von Zeitplänen pro Sicherungsrichtlinie |10 | |
| Maximale Anzahl von Momentaufnahmen eines beliebigen Typs, die pro Volume beibehalten werden können |256 |Dies beinhaltet lokale Momentaufnahmen und cloud-Momentaufnahmen. |
| Maximale Anzahl von Momentaufnahmen, die in einem beliebigen Gerät vorhanden sein können |10,000 | |
| Maximale Anzahl von Volumes, die parallel für Sicherung, Wiederherstellung verarbeitet werden können, oder Klonen |16 |<ul><li>Wenn mehr als 16 Volumes vorhanden sind, werden diese sequenziell verarbeitet werden, wenn verarbeitungsslots verfügbar werden.</li><li>Neue Sicherungen eines geklonten oder wiederhergestellten mehrstufigen Volumes kann nicht erfolgen, bis der Vorgang abgeschlossen ist. Allerdings sind für ein lokales Volume Sicherungen zulässig, nachdem das Volume online ist.</li></ul> |
| Wiederherstellen und Klonen von Wiederherstellungszeit bei mehrstufigen volumes |< 2 Minuten |<ul><li>Das Volume wird innerhalb von zwei Minuten nach Wiederherstellungs- oder Klonvorgang Vorgang unabhängig von der Größe des Volumes zur Verfügung gestellt.</li><li>Das Volume Leistung anfänglich ist möglicherweise langsamer als normal, da die meisten Daten und Metadaten noch in der Cloud befinden. Als Datenflüsse aus der Cloud zum StorSimple-Gerät kann die Leistung erhöhen.</li><li>Die Gesamtzeit zum Herunterladen der Metadaten hängt von der zugewiesenen Volumegröße ab. Metadaten wird automatisch in das Gerät im Hintergrund mit einer Rate von 5 Minuten pro TB zugewiesener Volumedaten übertragen. Dieser Wert kann durch die Internetbandbreite zur Cloud beeinflusst werden.</li><li>Der Wiederherstellungs- oder Klonvorgang ist abgeschlossen, wenn alle Metadaten auf dem Gerät ist.</li><li>Sicherungsvorgänge können nicht ausgeführt werden, bis die Wiederherstellung oder Klonen vollständig abgeschlossen werden. |
| Stellen Sie die Wiederherstellungszeit bei lokalen Volumes wieder her. |< 2 Minuten |<ul><li>Das Volume wird innerhalb von zwei Minuten des Wiederherstellungsvorgangs, unabhängig von der Größe des Volumes zur Verfügung gestellt.</li><li>Das Volume Leistung anfänglich ist möglicherweise langsamer als normal, da die meisten Daten und Metadaten noch in der Cloud befinden. Als Datenflüsse aus der Cloud zum StorSimple-Gerät kann die Leistung erhöhen.</li><li>Die Gesamtzeit zum Herunterladen der Metadaten hängt von der zugewiesenen Volumegröße ab. Metadaten wird automatisch in das Gerät im Hintergrund mit einer Rate von 5 Minuten pro TB zugewiesener Volumedaten übertragen. Dieser Wert kann durch die Internetbandbreite zur Cloud beeinflusst werden.</li><li>Im Gegensatz zur mehrstufigen Volumes bei lokalen Volumes werden der Volumedaten auch lokal auf dem Gerät heruntergeladen. Der Restore-Vorgang ist abgeschlossen, wenn alle Daten des Datenträgers wurde auf dem Gerät versetzt.</li><li>Die Wiederherstellungsvorgänge länger sein können, und die Gesamtzeit zum Abschließen des Wiederherstellungsvorgangs hängt von der Größe des bereitgestellten lokalen Volumes, die Internet-Bandbreite und die vorhandenen Daten auf dem Gerät. Sicherungsvorgänge für das lokale Volume sind zulässig, während der Wiederherstellungsvorgang ausgeführt wird. |
| Thinwiederherstellungsverfügbarkeit |Letztes failover | |
| Maximaler Client Lese-/Schreibzugriff Durchsatz (wenn von der SSD-Ebene aus bereitgestellt) * |920/720 MB/s mit einer einzelnen 10-GbE-Netzwerkschnittstelle |Bis zu 2 X mit MPIO und zwei Netzwerkschnittstellen. |
| Maximaler Client Lese-/Schreibzugriff Durchsatz (wenn von HDD-Ebene aus bereitgestellt) * |120/250 MB/s | |
| Maximaler Client Lese-/Schreibzugriff Durchsatz (wenn von der cloudebene aus bereitgestellt) * |11/41 MB/s |Lesedurchsatz hängt von Clients generieren und verwalten müssen genügend e/a-Warteschlangentiefe ab. |

&#42; Der maximale Durchsatz pro e/a-Typ wurde mit 100 %-Lese- und 100 %-schreibszenarien gemessen. Tatsächliche Durchsatz kann geringer sein und hängt von e/a-Downmix und netzwerkbedingungen.


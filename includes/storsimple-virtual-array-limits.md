

| **Limit-Bezeichner** | **Grenzwert** | **Kommentare** |
| --- | --- | --- |
| Gesamtkapazität (einschließlich der Cloud) |Bis zu 64 TB pro virtuelles Gerät |Sie können ein Failover eines vollständigen virtuellen StorSimple-Arrays in ein anderes leeres Array. Wenn Sie versuchen, auf das gleiche Gerät wiederherstellen, stellen Sie sicher, dass Sie über genügend Speicherplatz auf dem Gerät zum Abschließen dieses Vorgangs verfügen. Nachdem Sie 32 TB überschritten haben, kann das gleiche Gerät nicht wiederhergestellt. |
| Maximale Anzahl von Anmeldeinformationsobjekten pro Gerät |1 | |
| Maximale Anzahl von Volumes/Freigaben |16 | |
| Minimale Größe des eine mehrstufige Freigabe |500 GB | |
| Mindestgröße eines mehrstufigen Volumes |500 GB | |
| Maximale Größe für eine mehrstufige Freigabe |20 TB | |
| Maximale Größe eines mehrstufigen Volumes |5 TB | |
| Mindestgröße einer lokalen Freigabe |50 GB | |
| Minimale Größe des ein lokales volume |50 GB | |
| Maximale Größe einer lokalen Freigabe |2 TB | |
| Maximale Größe für ein lokales volume |200 GB | |
| Maximale Anzahl von iSCSI-Verbindungen von Initiatoren |512 | |
| Maximale Anzahl von zugriffssteuerungsdatensätzen pro Gerät |64 | |
| Maximale Anzahl von Sicherungen beibehalten, indem Sie das virtuelle Gerät in *.backups* Ordner für Dateiserver |5 |Dies schließt die letzte geplante (generiert durch die Standardrichtlinie backup) und manuelle Sicherungen. |
| Maximale Anzahl von geplanten Sicherungen, die von dem Gerät beibehalten |55 |30 tägliche Sicherungen<br>12 monatliche Sicherungen<br>13 jährliche Sicherungen |
| Maximale Anzahl von manuelle Sicherungen, die von dem Gerät beibehalten |45 | |
| Maximale Anzahl von Dateien pro Aktie für einen Dateiserver |1 million |Bei der Ausführung einer gerätewiederherstellung sind die Wiederherstellungszeit für alle Freigaben auf dem Gerät proportional zur Anzahl der Dateien. |
| Maximale Anzahl von Dateien pro Volume für einen iSCSI-server |1 million | |
| Maximale Anzahl von Dateien pro virtuellen Arrays |4 Millionen | |
| Wiederherstellungszeit wiederherstellen |Schnelle Wiederherstellung |Die Wiederherstellung basiert auf die Heat Map und richtet sich nach der Volumegröße.<br>Sicherungsvorgänge können auftreten, während ein Wiederherstellungsvorgang ausgeführt wird. |


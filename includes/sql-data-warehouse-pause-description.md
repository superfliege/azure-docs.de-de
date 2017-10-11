
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
Um die Kosten zu sparen, können Sie anhalten und Fortsetzen von Compute-Ressourcen bei Bedarf. Z. B. Wenn Sie die Datenbank während der Nacht und für Wochenenden verwenden, können Sie diese Zeiten anhalten und im Laufe des Tages fortgesetzt. Sie wird nicht DWUs in Rechnung gestellt, während die Datenbank angehalten wird.

Wenn Sie eine Datenbank anhalten:

* Server- und Speicherressourcen werden an den Pool der verfügbaren Ressourcen im Rechenzentrum zurückgegeben.
* DWU Kosten sind 0 (null), für die Dauer der Pause an.
* Datenspeicherung ist nicht betroffen, und Ihre Daten bleiben intakt. 
* SQL Data Warehouse bricht alle Vorgänge für ausgeführt werden oder in der Warteschlange ab.


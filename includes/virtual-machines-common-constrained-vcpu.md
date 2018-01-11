

Einige Datenbankworkloads wie SQL Server oder Oracle erfordern hohe Werte für Arbeitsspeicher, Speicherplatz und E/A-Bandbreite, aber keine hohe Anzahl von Kernen. Viele Datenbankworkloads sind nicht CPU-intensiv. Azure bietet bestimmte VM-Größen an, bei denen Sie die Anzahl der VM vCPUs einschränken können, um die Kosten für die Softwarelizenzierung zu reduzieren, während Arbeitsspeicher, Speicherplatz und E/A-Bandbreite gleich bleiben.

Die Anzahl der vCPUs kann auf die Hälfte oder ein Viertel der ursprünglichen VM-Größe beschränkt werden. Diese neuen VM-Größen verfügen über ein Suffix, das die Anzahl der aktiven vCPUs angibt, um Ihnen die Identifizierung zu erleichtern.

Beispielsweise verfügt die aktuelle VM-Größe „Standard_GS5“ über 32 vCPUs, 448 GB RAM, 64 Festplatten (bis zu 256 TB) und 80.000 IOPs oder 2 GB/s für die E/A-Bandbreite. Die neuen VM-Größen „Standard_GS5-16“ und „Standard_GS5-8“ verfügen über 16 bzw. 8 aktive vCPUs, wobei die restlichen Spezifikationen von „Standard_GS5“ für Arbeitsspeicher, Speicherplatz und E/A-Bandbreite beibehalten werden.

Die Lizenzgebühren für SQL Server oder Oracle sind auf die neue vCPU-Anzahl beschränkt, und andere Produkte sollten auf der Grundlage der neuen vCPU-Anzahl berechnet werden. Dies führt zu einer Erhöhung des Verhältnisses zwischen VM-Spezifikationen und aktiven (abrechenbaren) vCPUs um 50 % bis 75 %. Diese neuen VM-Größen, die nur in Azure verfügbar sind, ermöglichen es Workloads, eine höhere CPU-Auslastung zu einem Bruchteil der Lizenzkosten (pro Kern) zu erreichen. Zu diesem Zeitpunkt bleiben die Computekosten (einschließlich der Betriebssystemlizenzierung) gleich wie bei der ursprünglichen Größe. Weitere Informationen finden Sie unter [Azure VM-Größen für kostengünstigere Datenbankworkloads](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Name                | vCPU | Spezifikationen           |
|---------------------|------|-----------------|
| Standard_M64-32ms   | 32   | Identisch mit M64ms   |
| Standard_M64-16ms   | 16   | Identisch mit M64ms   |
| Standard_M128-64ms  | 64   | Identisch mit M128ms  |
| Standard_M128-32ms  | 32   | Identisch mit M128ms  |
| Standard_E32-16_v3  | 16   | Identisch mit E32s_v3 |
| Standard_E32-8s_v3  | 8    | Identisch mit E32s_v3 |
| Standard_E64-32s_v3 | 32   | Identisch mit E64s_v3 |
| Standard_E64-16s_v3 | 16   | Identisch mit E64s_v3 |
| Standard_GS4-8      | 8    | Identisch mit GS4     |
| Standard_GS4-4      | 4    | Identisch mit GS4     |
| Standard_GS5-16     | 16   | Identisch mit GS5     |
| Standard_GS5-8      | 8    | Identisch mit GS5     |
| Standard_DS13-4_v2  | 4    | Identisch mit DS13_v2 |
| Standard_DS13-2_v2  | 2    | Identisch mit DS13_v2 |
| Standard_DS14-8_v2  | 8    | Identisch mit DS14_v2 |
| Standard_DS14-4_v2  | 4    | Identisch mit DS14_v2 |
**Standard verwalteten virtuellen Computer und Datenträger**

| Standard Datenträgertyp  | S4               | A6               | S10              | S20              | S30              | S40              | SOLLTE VORZUGSWEISE S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Größe des Datenträgers           | 32 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPS pro Datenträger       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Durchsatz pro Datenträger | 60 MB/s | 60 MB/s | 60 MB/s | 60 MB/s | 60 MB/s | 60 MB/s | 60 MB/s | 

**Premium verwaltet Datenträger der virtuellen Maschine: pro Datenträger-Grenzwerte**

| Premium-Datenträger-Typ  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Größe des Datenträgers           | 32 GB | 64 GB | 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS pro Datenträger       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Durchsatz pro Datenträger | 25 MB/s | 50 MB/s  | 100 MB/s | 150 MB/s | 200 MB/s | 250 MB/s | 250 MB/s |

**Premium verwaltet Datenträger der virtuellen Maschine: pro VM-Grenzwerte**

| Ressource | Standardlimit |
| --- | --- |
| Max. IOPS pro virtueller Maschine |80.000 IOPS mit GS5 VM<sup>1</sup> |
| Maximaler Durchsatz pro virtueller Maschine |2.000 MB/s mit GS5 VM<sup>1</sup> |

<sup>1</sup>finden Sie unter [Größe des virtuellen Computers](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) für Grenzwerte für andere VM-Größen. 

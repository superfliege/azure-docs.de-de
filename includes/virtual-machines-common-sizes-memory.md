
Arbeitsspeicheroptimierte VM-Größen bieten ein hohes Arbeitsspeicher-zu-CPU-Verhältnis und eignen sich hervorragend für relationale Datenbankserver, mittelgroße bis große Caches und In-Memory-Analysen. Dieser Artikel enthält Informationen zur Anzahl von vCPUs, Datenträgern und NICs sowie zum Speicherdurchsatz und zur Netzwerkbandbreite der einzelnen Größen in dieser Gruppe. 

* Die M-Serie bietet die höchste vCPU-Anzahl (bis zu 128 vCPUs) und den größten Arbeitsspeicher (bis zu 3,8 TiB) für die virtuellen Computer in der Cloud.  Dies ist ideal für extrem große Datenbanken oder andere Anwendungen, für die eine hohe vCPU-Anzahl und große Mengen an Arbeitsspeicher benötigt werden.

* Die Serien Dv2, D, G sowie deren DS/GS-Entsprechungen eignen sich ideal für Clientanwendungen, die schnellere vCPUs und eine höhere Leistung des temporären Speichers erfordern oder einen höheren Arbeitsspeicherbedarf haben.  Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.

* VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie zeichnen sich durch schnellere Prozessoren, ein höheres Verhältnis zwischen Arbeitsspeicher und vCPU und ein SSD (Solid State Drive) als temporärer Speicher aus. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)(Neue VM-Größen der D-Serie, in englischer Sprache).

* Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.


## <a name="esv3-series-sup1sup"></a>Esv3-Serie <sup>1</sup>

ACU: 160 - 190

Die Instanzen der ESv3-Serie basieren auf dem Prozessor vom Typ 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) und können mit Intel Turbo Boost Technology 2.0 3,5 GHz erreichen und Premium-Speicher nutzen. Die Instanzen der ESv3-Serie eignen sich ideal für speicherintensive Unternehmensanwendungen.


| Größe             | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3  | 2      | 16          | 32             | 4              | 4.000/32 (50)                                                       | 3.200/48                                | 2 / 1,000                                   |
| Standard_E4s_v3  | 4      | 32          | 64             | 8              | 8.000/64 (100)                                                      | 6.400/96                                | 2 / 2,000                                   |
| Standard_E8s_v3  | 8      | 64          | 128            | 16             | 16.000/128 (200)                                                    | 12.800/192                              | 4 / 4,000                                       |
| Standard_E16s_v3 | 16     | 128         | 256            | 32             | 32.000/256 (400)                                                    | 25.600/384                              | 8 / 8,000                                       |
| Standard_E32s_v3 <sup>2</sup> | 32     | 256         | 512            | 32             | 64.000/512 (800)                                                    | 51.200/768                              | 8 / 16,000                             |
| Standard_E64s_v3 <sup>2</sup> | 64     | 432         | 864            | 32             | 128.000/1.024 (1.600)                                                   | 80.000/1.200                             | 8 / 30,000                             |

<sup>1</sup> Virtuelle Computer der Esv3-Serie verfügen über Hyper-Threading-Technologie <sup>2</sup> von Intel® Eingeschränkte Kerngrößen verfügbar 

## <a name="ev3-series-sup1sup"></a>Ev3-Serie <sup>1</sup>

ACU: 160 - 190 

Die Instanzen der Ev3-Serie basieren auf dem Prozessor vom Typ 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) und können mit Intel Turbo Boost Technology 2.0 3,5 GHz erreichen. Die Instanzen der ESv3-Serie eignen sich ideal für speicherintensive Unternehmensanwendungen.

Datenträgerspeicher wird separat zu virtuellen Computern abgerechnet. Verwenden Sie die ESv3-Größen, um Datenträger mit Premium-Speicher zu nutzen. Die Preis- und Abrechnungskennzahlen für die ESv3-Größen entsprechen denen der Ev3-Serie. 


| Größe            | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Maximale Anzahl NICs/Netzwerkbandbreite |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1,000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8,000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16,000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |

<sup>1</sup> Virtuelle Computer der Ev3-Serie verfügen über Hyper-Threading-Technologie von Intel®

## <a name="m-series-sup1sup"></a>M-Serie <sup>1</sup>

ACU: 160–180

| Größe            | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M64s  | 64   | 1024        | 2048           | 64             | 80,000 / 800 (6348)       | 40.000/1.000                            | 8/16000          |
| Standard_M64ms  | 64   | 1792        | 2048           | 64             | 80,000 / 800 (6348)       | 40.000/1.000                            | 8/16000          |
| Standard_M128s <sup>2. 3</sup> | 128  | 2048        | 4096           | 64             | 160,000 / 1,600 (12,696) | 80.000/2.000                            | 8 / 30000          |
| Standard_M128ms <sup>2. 3</sup> | 128  | 3800        | 4096           | 64             | 160,000 / 1,600 (12,696) | 80.000/2.000                            | 8 / 30000          |

<sup>1</sup> Virtuelle Computer der M-Serie verfügen über Hyper-Threading-Technologie von Intel®

<sup>2</sup> Für mehr als 64 vCPUs ist eines der folgenden Gastbetriebssysteme erforderlich: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 und Red Hat Enterprise Linux, CentOS 7.3 oder Oracle Linux 7.3 mit LIS 4.2.1 

<sup>3</sup> Eingeschränkte Kerngrößen verfügbar.

<br>

## <a name="gs-series-sup1sup"></a>GS-Serie <sup>1</sup>

ACU: 180 - 240

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10.000/100 (264) |5.000 / 125 |2/2000 |
| Standard_GS2 |4 |56 |112 |16 |20.000/200 (528) |10.000/250 |2/4000 |
| Standard_GS3 |8 |112 |224 |32 |40.000/400 (1.056) |20.000/500 |4/8000 |
| Standard_GS4 <sup>3</sup> |16 |224 |448 |64 |80.000/800 (2.112) |40.000/1.000 |8/16000 |
| Standard_GS5 <sup>2, 3</sup> |32 |448 |896 |64 |160.000/1.600 (4.224) |80.000/2.000 |8/20000 |

<sup>1</sup> Der mit einer VM der GS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md). 

<sup>2</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

<sup>3</sup> Eingeschränkte Kerngrößen verfügbar 

<br>

## <a name="g-series"></a>G-Serie

ACU: 180 - 240

| Größe         | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 8/8 x 500                       | 2/2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 16/16 x 500                       | 2/4000                     |
| Standard_G3  | 8         | 112         | 1.536          | 24000/375/187                                        | 32/32 x 500                     | 4/8000                |
| Standard_G4  | 16        | 224         | 3.072          | 48000/750/375                                        | 64/64 x 500                     | 8/16000          |
| Standard_G5 <sup>1</sup> | 32        | 448         | 6.144          | 96000/1500/750                                       | 64/64 x 500                     | 8/20000           |

<sup>1</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>


## <a name="dsv2-series-sup1sup"></a>DSv2-Serie <sup>1</sup>

ACU: 210 - 250

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |8 |8.000/64 (72) |6.400/96 |2/1500 |
| Standard_DS12_v2 |4 |28 |56 |16 |16.000/128 (144) |12.800/192 |4/3000 |
| Standard_DS13_v2 |8 |56 |112 |32 |32.000/256 (288) |25.600/384 |8/6000 |
| Standard_DS14_v2 |16 |112 |224 |64 |64.000/512 (576) |51.200/768 |8 / 12000 |
| Standard_DS15_v2 <sup>2</sup> |20 |140 |280 |64 |80.000/640 (720) |64.000/960 |8 / 25000 <sup>3</sup>

<sup>1</sup> Der mit einer VM der DSv2-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md).

<sup>2</sup> Die Instanz ist ein isolierter Knoten, mit dem dafür gesorgt wird, dass Ihre VM die einzige VM auf unserem Intel Haswell-Knoten ist.

<sup>3</sup> 25000 Mbps mit beschleunigtem Netzwerkbetrieb.

<br>

## <a name="dv2-series"></a>Dv2-Serie

ACU: 210 - 250

| Größe              | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8 x 500                         | 2/1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16 x 500                         | 4/3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32 x 500                       | 8/6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/64 x 500                       | 8 / 12000          |
| Standard_D15_v2 <sup>1</sup> | 20        | 140         | 1.000          | 60000/937/468                                        | 64/64 x 500                       | 8 / 25000 <sup>2</sup> |

<sup>1</sup> Die Instanz ist ein isolierter Knoten, mit dem dafür gesorgt wird, dass Ihre VM die einzige VM auf unserem Intel Haswell-Knoten ist.

<sup>2</sup> 25000 Mbps mit beschleunigtem Netzwerkbetrieb.

<br>

## <a name="ds-series-sup1sup"></a>DS-Serie <sup>1</sup>

ACU: 160

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8.000/64 (72) |6.400/64 |2/1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000/128 (144) |12.800/128 |4/2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000/256 (288) |25.600/256 |8/4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000/512 (576) |51.200/512 |8 / 8000 |

<sup>1</sup> Der mit einer VM der DS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/virtual-machines/windows/premium-storage.md).


## <a name="d-series"></a>D-Serie

ACU: 160

| Größe         | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 8/8 x 500                         | 2/1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 16/16 x 500                         | 4/2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 32/32 x 500                       | 8/4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 64/64 x 500                       | 8 / 8000                |

<br>


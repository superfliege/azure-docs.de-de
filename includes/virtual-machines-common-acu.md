 



Wir haben das Konzept der Azure-Berechnungseinheit ACU (Azure Compute Unit) erstellt, um eine Möglichkeit zum Vergleichen der Rechenleistung (CPU) zwischen den Azure-SKUs zu ermöglichen. Auf diese Weise können Sie leicht feststellen, welche SKU Ihren Leistungsanforderungen am ehesten entspricht.  Zurzeit ist der ACU-Wert auf einem kleinen virtuellen Computer (Standard_A1) auf den Standardwert 100 festgelegt, und an den übrigen SKUs kann ungefähr abgelesen werden, wie viel schneller die jeweilige SKU einen Standard-Benchmarktest ausführen kann. 

> [!IMPORTANT]
> Die ACU ist nur ein Richtwert.  Die Ergebnisse für Ihre Workload können abweichen. 
> 
> 

<br>

| SKU-Familie | ACU\vCPU | vCPU:Core |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1–A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5–A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2–A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2–A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A8-A11](../articles/virtual-machines/windows/sizes-hpc.md) |225* | 1:1 |
| [D1–D14](../articles/virtual-machines/windows/sizes-general.md) |160 | 1:1 |
| [D1_v2–D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* | 1:1 |
| [DS1–DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 | 1:1 |
| [DS1_v2–DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | 1:1 |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** |
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** |
| [F2s_v2-F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195-210* | 2:1** |
| [F1-F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [F1s-F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [G1–G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* | 1:1 |
| [GS1–GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* | 1:1 |
| [H](../articles/virtual-machines/windows/sizes-hpc.md) |290 - 300* | 1:1 |
| [L4s - L32s](../articles/virtual-machines/windows/sizes-storage.md) |180 - 240* | 1:1 |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160-180 | 2:1** |

Mit * gekennzeichnete ACUs verwenden die Intel® Turbo-Technologie, um die CPU-Frequenz zu erhöhen eine Leistungssteigerung zu erzielen.  Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.

** Mit Hyperthreading. 

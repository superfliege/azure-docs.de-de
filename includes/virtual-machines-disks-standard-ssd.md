---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 75b3934a7329b4e83a0f36f79bbc8365eaf8a086
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572165"
---
# <a name="standard-ssd-managed-disks-for-azure-virtual-machine-workloads"></a>Verwaltete Standard-SSD-Datenträger für Workloads virtueller Azure-Computer

Verwaltete Azure Standard-SSD-Datenträger stellen eine kostengünstige Speichermöglichkeit dar, die für Workloads mit konstanter Leistung bei niedrigen IOPS-Werten optimiert ist. Standard-SSDs eignen sich insbesondere als Einstiegslösung zur Verlagerung von Diensten in die Cloud. Dies gilt vor allem dann, wenn bei der Ausführung vieler Workloads auf lokalen HDD-Lösungen Probleme auftreten. Mit Standard-SSDs können im Vergleich zu HDD-Datenträgern eine höhere Verfügbarkeit, Konsistenz und Zuverlässigkeit sowie eine optimierte Latenz erzielt werden. Standard-SSDs eignen sich für Webserver, Anwendungsserver mit einem niedrigen IOPS-Wert, wenig genutzte Unternehmensanwendungen und Dev/Test-Workloads.

## <a name="standard-ssd-features"></a>Standard-SSD-Features

**Verwalteter Datenträger:** Standard-SSD-Datenträger sind nur als verwaltete Datenträger verfügbar. Nicht verwaltete Datenträger und Seitenblobs werden nicht für die Option „SSD Standard“ unterstützt. Legen Sie beim Erstellen des verwalteten Datenträgers als Datenträgertyp „SSD Standard“ fest, und geben Sie die Größe des Datenträgers an. Azure erstellt und verwaltet daraufhin den Datenträger für Sie.
Standard-SSDs unterstützen alle Vorgänge des klassischen Bereitstellungsmodells, die durch Managed Disks angeboten werden. Beispielsweise können Sie verwaltete SSD-Standard-Datenträger auf die gleiche Weise erstellen, kopieren und Momentaufnahmen für diese anfertigen wie im Fall von verwalteten Datenträgern.

**Virtuelle Computer:** Standard-SSDs können mit allen virtuellen Azure-Computern einschließlich derjenigen, die keine Premium-Datenträger unterstützen, verwendet werden. Wenn Sie beispielsweise einen virtuellen Azure-Computer der Serie A, N oder DS verwenden, können Sie Standard-SSDs mit diesem virtuellen Computer verwenden. Mit der Einführung von Standard-SSDs können zahlreiche Workloads genutzt werden, für die vorher ein Wechsel von HDD- zu SSD-Datenträgern durchgeführt werden musste. Zusätzlich kann eine konstante Leistung, höhere Verfügbarkeit und eine verbesserte Latenz erzielt werden. Des Weiteren lassen sich alle Vorteile nutzen, die SSDs mit sich bringen.

**Hochverfügbar und stabil:** Standard-SSDs werden auf derselben Azure-Datenträgerplattform erstellt, mit der bereits in der Vergangenheit Hochverfügbarkeit und Stabilität für Datenträger gewährleistet werden konnte. Azure-Datenträger sind für eine Verfügbarkeit von 99,999 % ausgelegt. Für Standard-SSDs steht ebenso wie für alle verwalteten Datenträger lokal redundanter Speicher (LRS) zur Verfügung. Mithilfe von LRS verwaltet die Plattform mehrere Datenreplikate für jeden Datenträger. Für IaaS-Datenträger kann eine Stabilität auf Unternehmensniveau und eine auf das Jahr umgerechnete Ausfallrate (Annualized Failure Rate, AFR) von 0 % erzielt werden. Dies sind branchenweite Bestwerte.

**Momentaufnahmen:** Standard-SSDs unterstützen ebenso wie alle verwalteten Datenträger die Erstellung von Momentaufnahmen. Als Momentaufnahmetyp kann entweder „Standard (HDD)“ oder „Premium (SSD)“ festgelegt werden. Zur Einsparung von Kosten wird der Momentaufnahmetyp „Standard (HDD)“ für alle Azure-Datenträgertypen empfohlen. Hintergrund ist, dass Sie beim Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme immer die Möglichkeit haben, einen Tarif mit höheren Gebühren wie „Standard SSD“ oder „Premium SSD“ auszuwählen.

## <a name="scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele

Die folgende Tabelle enthält die Datenträgergrößen, die derzeit für „Standard SSD“ angeboten werden. Größen, die mit einem Sternchen gekennzeichnet sind, befinden sich derzeit in der Vorschau.

|Standard-SSD-Datenträgertyp  |Datenträgergröße  |IOPS pro Datenträger  |Durchsatz pro Datenträger  |
|---------|---------|---------|---------|
|E10     |128 GB         |Bis zu 500         |Bis zu 60 MiB pro Sekunde         |
|E15     |256 GiB         |Bis zu 500         |Bis zu 60 MiB pro Sekunde         |
|E20     |512 GB         |Bis zu 500         |Bis zu 60 MiB pro Sekunde         |
|E30     |1.024 GiB       |Bis zu 500         |Bis zu 60 MiB pro Sekunde         |
|E40     |2.048 GiB       |Bis zu 500         |Bis zu 60 MiB pro Sekunde         |
|E50     |4.095 GiB       |Bis zu 500         |Bis zu 60 MiB pro Sekunde         |
|E60*     |8.192 GiB       |Bis zu 1.300       |Bis zu 300 MiB pro Sekunde        |
|E70*    |16.384 GiB      |Bis zu 2.000       |Bis zu 500 MiB pro Sekunde        |
|E80*    |32767 GiB      |Bis zu 2.000       |Bis zu 500 MiB pro Sekunde        |

Standard-SSDs sind dafür ausgelegt, 99 Prozent der Zeit Wartezeiten im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge zu erzielen sowie IOPS und Durchsätze bis zu den in der obigen Tabelle genannten Grenzwerten bereitzustellen. Die Istwerte für IOPS und Durchsätze können in einigen Fällen je nach Datenverkehrsmuster variieren. Standard-SSDs stellen eine konstantere Leistung und geringere Latenzen als HDD-Datenträger bereit.

Premium-SSDs schneiden wiederum gegenüber Standard-SSDs besser ab, da Erstere niedrigere Latenzen, höhere IOPS-/Durchsatzwerte und eine noch konstantere bereitgestellte Datenträgerleistung aufweisen. SSD Premium wird für wichtige Produktionsworkloads empfohlen. Wenn Ihre Workload Datenträgerunterstützung für hohe Leistung und geringe Latenz erfordert, empfiehlt sich unter Umständen die Verwendung von Storage Premium.

Standard-SSDs verwenden ebenso wie Premium-SSDs eine E/A-Einheitengröße von 256 KiB. Wenn die übertragenen Daten kleiner als 256 KiB sind, werden sie als eine E/A-Einheit angesehen. Sind die E/A-Daten größer, werden sie als mehrere E/A der Größe 256 KiB gezählt. Beispielsweise wird eine E/A mit 1.100 KiB als fünf E/A-Einheiten gezählt.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei Verwendung von Standard-SSDs sind folgende Abrechnungsaspekte zu berücksichtigen:

- Größe der verwalteten Datenträger
- Momentaufnahmen
- Ausgehende Datenübertragungen
- Transaktionen

**Größe der verwalteten Datenträger:** Die Abrechnung für verwaltete Datenträger erfolgt nach bereitgestellter Größe. Azure ordnet die bereitgestellte Größe (aufgerundet) dem Angebot für die nächsthöhere Datenträgergröße zu. Details zu den angebotenen Datenträgergrößen finden Sie in der Tabelle im obigen Abschnitt zu Skalierbarkeits- und Leistungszielen. Jeder Datenträger wird der bereitgestellten Datenträgergröße zugeordnet und entsprechend abgerechnet. Wenn Sie z.B. einen Standard-SSD-Datenträger mit 200 GiB bereitstellen, wird das Datenträgerangebot E15 (256 GiB) zugeordnet. Die Abrechnung für bereitgestellte Datenträger erfolgt anteilig auf Stundenbasis unter Verwendung des monatlichen Preises für das Storage Premium-Angebot. Wenn Sie z.B. einen Datenträger des Typs E10 bereitgestellt und diesen nach 20 Stunden gelöscht haben, erfolgt die Abrechnung für das E10-Angebot anteilig für 20 Stunden. Welche Datenmenge tatsächlich auf den Datenträger geschrieben wurde, spielt keine Rolle.

**Momentaufnahmen:** Momentaufnahmen von verwalteten Datenträgern werden entsprechend der Kapazität abgerechnet, die von den ggf. vorhandenen Momentaufnahmen für Quelle und Ziel verwendet wird. Weitere Informationen zu Momentaufnahmen finden Sie unter [Momentaufnahmen von verwalteten Datenträgern](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#managed-disk-snapshots).

**Ausgehende Datenübertragungen:**[Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/bandwidth/) (Daten, die von den Azure-Rechenzentren ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

**Transaktionen:** Transaktionen auf Standard-SSDs verursachen ähnlich wie bei Standard-HDDs Kosten. Transaktionen umfassen Lese- und Schreibvorgänge auf dem Datenträger. Die Größe der E/A-Einheit für Abrechnungen von Transaktionen auf Standard-SSDs beträgt 256 KiB. Sind die E/A-Daten größer, werden sie als mehrere E/A der Größe 256 KiB gezählt.

Weitere Informationen zu Preisen für virtuelle Computer und verwaltete Datenträger finden Sie unter:

- [Preisgestaltung für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)
- [Preise für Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen von Standard-SSDs finden Sie im Beispiel zum Erstellen eines virtuellen Computers mit mehreren Standard-SSDs.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Computers mit mehreren Standard-SSDs](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)

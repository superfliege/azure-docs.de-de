---
title: Speicherdienste von Azure Deutschland | Microsoft-Dokumentation
description: "Vergleich der Speicherdienste für Azure Deutschland"
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: ralfwi
ms.openlocfilehash: d205c3d30cc862bdb61585972c9e9837772f3804
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="azure-germany-storage-services"></a>Speicherdienste von Azure Deutschland
## <a name="storage"></a>Storage
Einzelheiten zu Azure Storage und seiner Verwendung finden Sie in der [globalen Dokumentation zu Storage](../storage/index.yml).

Die in Azure Storage gespeicherten Daten werden repliziert, um eine hohe Verfügbarkeit sicherzustellen. Bei georedundanten Speichern und georedundanten Speichern mit Lesezugriff repliziert Azure Daten zwischen den *Regionspaaren*. Bei Azure Deutschland sind folgende Regionspaare verfügbar:

| Primäre Region | Sekundäre (zugehörige) Region |
| --- | --- |
| Deutschland, Mitte | Deutschland, Nordosten |
| Deutschland, Nordosten | Deutschland, Mitte |

Bei der Datenreplikation werden die Daten innerhalb deutscher Grenzen gehalten. Die primäre und sekundäre Region werden einander zugeordnet, um sicherzustellen, dass ausreichend Abstand zwischen den Rechenzentren besteht, um bei einem Ausfall eines großen Gebiets oder bei einem Notfall die Verfügbarkeit sicherzustellen. Wählen Sie bei georedundanten Speichern mit hoher Verfügbarkeit beim Erstellen eines Speicherkontos entweder georedundante Speicher oder georedundante Speicher mit Lesezugriff aus.  

Die Speicherdienstverschlüsselung schützt ruhende Daten in Azure Storage-Konten. Wenn Sie diese Funktion aktivieren, verschlüsselt Azure automatisch Daten, bevor sie in den Speicher eingefügt werden. Die Daten werden mithilfe einer 256-Bit-AES-Verschlüsselung verschlüsselt. Die Speicherdienstverschlüsselung unterstützt die Verschlüsselung von Block-, Anfüge- und Seitenblobs.

### <a name="storage-service-availability-by-azure-germany-region"></a>Speicherdienstverfügbarkeit nach Azure Deutschland-Region

| Dienst | Deutschland, Mitte | Deutschland, Nordosten |
| --- | --- | --- | --- |
| [Blob Storage](../storage/common/storage-introduction.md#blob-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Azure Files](../storage/common/storage-introduction.md#azure-files) | Allgemein verfügbar | Allgemein verfügbar |
| [Table Storage](../storage/common/storage-introduction.md#table-storage) |Allgemein verfügbar  |Allgemein verfügbar |
| [Queue Storage](../storage/common/storage-introduction.md#queue-storage) |Allgemein verfügbar | Allgemein verfügbar |
| [Hot/Cold Blob Storage](../storage/blobs/storage-blob-storage-tiers.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Service Encryption](../storage/common/storage-service-encryption.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Premium](../virtual-machines/windows/premium-storage.md) |Allgemein verfügbar |Allgemein verfügbar |
| Import/Export |Nicht verfügbar |Nicht verfügbar |
| StorSimple |Nicht verfügbar |Nicht verfügbar |

### <a name="variations"></a>Abweichungen
Die URLs für Speicherkonten in Azure Deutschland unterscheiden sich von denen in der globalen Azure-Umgebung:

| Dienstart | Globale Azure-Umgebung | Azure Deutschland |
| --- | --- | --- |
| Blob Storage | *.blob.core.windows.net | *.blob.core.cloudapi.de |
| Azure Files | *.file.core.windows.net | *.file.core.cloudapi.de | 
| Queue Storage | *.queue.core.windows.net | *.queue.core.cloudapi.de |
| Table Storage | *.table.core.windows.net | *.table.core.cloudapi.de |

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die passenden Endpunkte berücksichtigen. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../storage/common/storage-configure-connection-string.md). 
>
>

Weitere Informationen zu APIs finden Sie unter [CloudStorageAccount-Konstruktor](https://msdn.microsoft.com/library/azure/mt616540.aspx).

Das zu verwendende Endpunktsuffix für diese Überladungen lautet *core.cloudapi.de*.

> [!NOTE]
> Wenn während der [Einbindung der Dateifreigabe](../storage/files/storage-dotnet-how-to-use-files.md) der Fehler 53 („Der Netzwerkpfad wurde nicht gefunden.“) zurückgegeben wird, blockiert möglicherweise eine Firewall den ausgehenden Port. Versuchen Sie, die Dateifreigabe auf einem virtuellen Computer einzubinden, der sich im selben Azure-Abonnement wie das Speicherkonto befindet.
>
>


## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.

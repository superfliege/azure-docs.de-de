---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 86c301748b58e7642df9a738c70b4fe70be3310b
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39400007"
---
Zonenredundanter Speicher (ZRS) repliziert Ihre Daten synchron über drei Speichercluster in einer Region. Jeder Speichercluster ist physisch unabhängig von den anderen und befindet sich in einer eigenen Verfügbarkeitszone. Jede Verfügbarkeitszone und der ZRS-Cluster darin ist autonom und verfügt über getrennte Hilfsprogramme und Netzwerkfunktionen.

Durch das Speichern Ihrer Daten in einem ZRS-Konto wird sichergestellt, dass Sie auch dann auf Ihre Daten zugreifen und diese verwalten können, wenn eine Zone nicht verfügbar ist. ZRS bietet eine herausragende Leistung bei geringer Latenz. ZRS bietet die gleichen [Skalierbarkeitsziele](../articles/storage/common/storage-scalability-targets.md) wie [lokal redundanter Speicher (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Erwägen Sie ZRS für Szenarien, die eine hohe Konsistenz, Dauerhaftigkeit und Verfügbarkeit erfordern, selbst wenn das Rechenzentrum einer Zone bei einem Ausfall oder einer Naturkatastrophe nicht verfügbar ist. Der ZRS bietet eine Dauerhaftigkeit für Speicherobjekte von mindestens 99,9999999999 % (12-mal die Zahl 9) über einen Zeitraum von einem Jahr.

Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Übersicht über Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Supportabdeckung und regionale Verfügbarkeit
ZRS unterstützt derzeit Standardkonten vom Typ [Allgemein v2 (GPv2)](../articles/storage/common/storage-account-options.md#general-purpose-v2-accounts). ZRS ist für Blockblobs, Seitenblobs ohne Datenträger, Dateien, Tabellen und Warteschlangen verfügbar und darüber hinaus auch für alle Ihre [Storage Analytics](../articles/storage/common/storage-analytics.md)-Protokolle und [Storage-Metriken](../articles/storage/common/storage-enable-and-view-metrics.md).

ZRS ist in den folgenden Regionen verfügbar:

- USA (Ost 2)
- USA, Westen 2
- USA, Mitte
- Nordeuropa
- Europa, Westen
- Frankreich, Mitte
- Asien, Südosten

Microsoft arbeitet daran, ZRS in weiteren Azure-Regionen zu aktivieren. Sehen Sie regelmäßig auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/) nach, um Informationen zu neuen Regionen zu erhalten.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Was geschieht, wenn eine Zone nicht mehr verfügbar ist?

Ihre Daten bleiben erhalten, wenn eine Zone ausfällt. Microsoft empfiehlt, die Vorgehensweisen für vorübergehende Fehler einzuhalten. Dazu gehört z.B. die Implementierung von Wiederholungsrichtlinien mit exponentiellem Backoff. Wenn eine Zone nicht verfügbar ist, führt Azure Netzwerkupdates durch, z.B. durch die Festlegung neuer DNS-Ziele. Diese Updates können sich auf Ihre Anwendung auswirken, wenn Sie auf Ihre Daten zugreifen, bevor sie abgeschlossen wurden.

ZRS kann Ihre Daten nicht vor einem regionalen Notfall schützen, bei dem mehrere Zonen dauerhaft betroffen sind. ZRS bietet stattdessen Resilienz für Ihre Daten bei einem temporären Ausfall. Für den Schutz vor regionalen Ausfällen empfiehlt Microsoft die Verwendung von georedundantem Speicher (GRS). Weitere Informationen zu GRS finden Sie unter [Georedundanter Speicher (GRS): regionsübergreifende Replikation für Azure Storage](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Wechseln zur ZRS-Replikation
Sie können derzeit das Azure-Portal oder die Storage-Ressourcenanbieter-API verwenden, um den Redundanztyp Ihres Kontos zu ändern, sofern Sie eine Migration zu oder von LRS, GRS und RA-GRS durchführen. Mit ZRS ist die Migration jedoch nicht so einfach, da die Daten physisch aus einem einzelnen Speicherstempel in mehrere Stempel innerhalb einer Region verschoben werden müssen. 

Ihnen stehen zwei primäre Optionen für die Migration zu oder von ZRS zur Verfügung. Sie können Daten manuell aus Ihrem vorhandenen Konto in ein neues ZRS-Konto kopieren oder verschieben. Sie können auch eine Livemigration anfordern. Microsoft empfiehlt dringend, Sie eine manuelle Migration auszuführen, da es keine Garantie gibt, wann eine Livemigration abgeschlossen sein wird. Eine manuelle Migrationsroute bietet mehr Flexibilität als eine Livemigration, und Sie haben volle Kontrolle über den zeitlichen Ablauf der Migration.

Für eine manuelle Migration steht Ihnen eine Vielzahl von Optionen zur Verfügung:
- Verwenden Sie vorhandene Tools wie AzCopy, das Storage SDK, Tools von zuverlässigen Drittanbietern usw.
- Wenn Sie mit Hadoop oder HDInsight vertraut sind, können Sie das Quell- und Zielkonto (ZRS) an Ihren Cluster anfügen und beispielsweise DistCp verwenden, um den Kopiervorgang der Daten hochgradig zu parallelisieren.
- Erstellen Sie eigene Tools, die das Storage SDK nutzen.

Wenn eine manuelle Migration allerdings zu Anwendungsausfallzeiten führt und Sie dies auf Ihrer Seite nicht auffangen können, bietet Microsoft als Option eine Livemigration an. Eine Livemigration ist eine direkte Migration, bei der Sie Ihr vorhandenes Speicherkonto weiterverwenden können, während die Daten zwischen Quell- und Zielspeicherstempel migriert werden. Während der Migration verfügen Sie über das gleiche Maß an Dauerhaftigkeit und Verfügbarkeit gemäß SLA, wie Sie es gewohnt sind.

Trotzdem gelten für Livemigrationen gewisse Einschränkungen. Diese werden unten aufgeführt.

- Auch wenn Microsoft auf Ihre Anforderung zur Livemigration sofort reagiert, gibt es keine Garantie, wann die Migration abgeschlossen sein wird. Wenn Sie Ihre Daten zu einem bestimmten Zeitpunkt im ZRS benötigen, sollten Sie eine manuelle Migration durchführen. Grundsätzlich gilt: Je mehr Daten Sie in Ihrem Konto haben, desto länger dauert die Migration dieser Daten. 
- Sie können eine Livemigration nur über ein Konto mit LRS- oder GRS-Replikation ausführen. Wenn Ihr Konto RA-GRS verwendet, müssen Sie zunächst zu einem dieser Replikationstypen migrieren, bevor Sie fortfahren können. Dieser Zwischenschritt stellt sicher, dass der sekundäre schreibgeschützte-Endpunkt, den RA-GRS bereitstellt, vor der Migration entfernt wird.
- Ihr Konto muss Daten enthalten.
- Es werden nur Migrationen innerhalb einer Region unterstützt. Wenn Sie Ihre Daten in ein ZRS-Konto zu einer anderen Region als das Quellkonto migrieren möchten, müssen Sie eine manuelle Migration ausführen.
- Es werden nur Storage Standard-Konten unterstützt. Sie können nicht von einem Storage Premium-Konto migrieren.

Anforderungen zur Livemigration erfolgen über das Azure-Support-Portal. Sie wählen über das Portal das Speicherkonto aus, das Sie in ZRS konvertieren möchten.
1. Klicken Sie auf **Neue Supportanfrage**.
2. Überprüfen Sie die Grundlagen. Klicken Sie auf **Weiter**. 
3. Gehen Sie im Abschnitt **Problem** folgendermaßen vor: 
    - Lassen Sie den Schweregrad unverändert.
    - Problemtyp = **Datenmigration**
    - Kategorie = **Migrieren zu ZRS innerhalb einer Region**
    - Titel = **ZRS-Kontomigration** (oder eine Beschreibung)
    - Details = Ich möchte in der Region ______ von [LRS, GRS] zu ZRS migrieren. 
4. Klicken Sie auf **Weiter**.
5. Überprüfen Sie, ob die Kontaktinformationen auf dem Blatt „Kontaktinformationen“ korrekt sind.
6. Klicken Sie auf **Submit**.

Ein Supportmitarbeiter wird mit Ihnen in Kontakt treten. Diese Person bietet Ihnen ggf. erforderliche Unterstützung. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Classic: eine ältere Option für die Redundanz von Blockblobs
> [!NOTE]
> ZRS Classic-Konten werden am 31. März 2021 als veraltet gekennzeichnet, und für diese ist eine Migration erforderlich. Bevor die Komponenten als veraltet gekennzeichnet werden, sendet Microsoft weitere Informationen an ZRS Classic-Kunden. Microsoft plant die Bereitstellung eines automatisierten Migrationsprozesses von ZRS Classic zu ZRS.

>[!NOTE]
> Sobald der ZRS in einer Region [allgemein verfügbar](#support-coverage-and-regional-availability) ist, kann ein ZRS Classic-Konto nicht mehr über das Portal in dieser Region erstellt werden. Allerdings können Sie es weiterhin über andere Methoden wie Microsoft PowerShell und die Azure-Befehlszeilenschnittstelle erstellen, bis ZRS Classic veraltet ist.

ZRS Classic repliziert Daten asynchron zwischen Rechenzentren in einer oder zwei Regionen. Ein Replikat ist ggf. erst wieder verfügbar, wenn von Microsoft das Failover in der sekundären Region initiiert wird. ZRS Classic ist nur für **Blockblobs** in Speicherkonten vom Typ [Allgemein v1 (GPv1)](../articles/storage/common/storage-account-options.md#general-purpose-v1-accounts) verfügbar. Ein ZRS Classic-Konto kann nicht in bzw. aus LRS oder GRS konvertiert werden und verfügt nicht über Metriken oder Protokollierungsfunktionen.

ZRS Classic-Konten können nicht in oder aus LRS, GRS oder RA-GRS konvertiert werden. ZRS Classic-Konten unterstützen zudem weder Metriken noch Protokollierungen.

Um manuell ZRS-Kontodaten zu oder von einem LRS-, ZRS Classic-, GRS- oder RA-GRS-Konto zu migrieren, verwenden Sie AzCopy, Azure Storage-Explorer, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle. Sie können auch Ihre eigene Migrationslösung mit einer der Azure Storage-Clientbibliotheken erstellen.

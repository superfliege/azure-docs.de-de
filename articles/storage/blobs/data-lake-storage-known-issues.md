---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Einschränkungen und bekannten Probleme mit Azure Data Lake Storage Gen2.
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: normesta
ms.openlocfilehash: 5677649b8f002490900ec32bee954348b2f444e6
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731545"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden die bekannten Probleme und vorübergehenden Einschränkungen für Azure Data Lake Storage Gen2 beschrieben.

## <a name="api-interoperability"></a>API-Interoperabilität

Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs sind nicht interoperabel.

Wenn Sie über benutzerdefinierte Tools, Anwendungen oder Skripts verfügen, die Blob-APIs verwenden, und sie für alle Inhalte einsetzen möchten, die Sie auf Ihr Konto hochladen, aktivieren Sie keine hierarchischen Namespaces für Ihr Blob-Speicherkonto, bis die Interoperabiliät von Blob-APIs mit Azure Data Lake Gen2-APIs sichergestellt ist. Die Verwendung eines Speicherkontos ohne die hierarchischen Namespaces bedeutet, dass Sie keinen Zugriff auf Data Lake Storage Gen2-spezifische Features wie z.B. Zugriffssteuerungslisten (ACLs) für Verzeichnis- und Dateisystem haben.

## <a name="blob-storage-apis"></a>Blob Storage-APIs

Blob Storage-APIs stehen für Azure Data Lake Storage Gen2-Konten noch nicht zur Verfügung.

Diese APIs sind deaktiviert, um unbeabsichtigte Datenzugriffsfehler zu vermeiden, die durch die fehlende Interoperabilität zwischen Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs entstehen könnten.

Falls Sie diese APIs vor der Deaktivierung zum Laden von Daten verwendet haben und produktionsbedingt Zugriff auf diese Daten benötigen, wenden Sie sich mit folgenden Informationen an den Microsoft-Support:

* Abonnement-ID (die GUID, nicht der Name)

* Speicherkontoname(n)

* Ist Ihre Produktion direkt betroffen, und falls ja, für welche Speicherkonten?

* Müssen die Daten in ein anderes Speicherkonto kopiert werden, und falls ja, warum? (Diese Frage ist auch relevant, wenn Ihre Produktion nicht direkt betroffen ist.)

Unter diesen Umständen können wir den Zugriff auf die Blob-API für einen begrenzten Zeitraum wiederherstellen, damit Sie die Daten in ein Speicherkonto kopieren können, für das keine hierarchischen Namespaces aktiviert sind.

Nicht verwaltete VM-Datenträger hängen von den deaktivierten Blob Storage-APIs ab. Wenn Sie also hierarchische Namespaces für ein Speicherkonto aktivieren möchten, platzieren Sie nicht verwaltete VM-Datenträger ggf. in einem Speicherkonto, für das keine hierarchischen Namespaces aktiviert sind.

## <a name="azure-storage-explorer"></a>Azure Storage-Explorer

Wenn Sie Data Lake Storage Gen2-Konten über den Azure Storage-Explorer anzeigen oder verwalten möchten, benötigen Sie mindestens die Version `1.6.0` des Tools. Diese können Sie [hier](https://azure.microsoft.com/features/storage-explorer/) kostenlos herunterladen.

Beachten Sie, dass mit der in das Azure-Portal eingebetteten Version des Storage-Explorers derzeit keine Data Lake Storage Gen2-Konten mit aktivierten hierarchischen Namespaces angezeigt oder verwaltet werden können.

## <a name="blob-viewing-tool"></a>Tool zur Blobanzeige

Das Tool zur Blobanzeige im Azure-Portal bietet nur eingeschränkte Unterstützung für Azure Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Drittanbieteranwendungen

Drittanbieteranwendungen bieten möglicherweise keine Unterstützung für Azure Data Lake Storage Gen2.

Die Unterstützung liegt im Ermessen des jeweiligen Anbieters der Drittanbieteranwendung. Aktuell können Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs nicht zum Verwalten desselben Inhalts verwendet werden. Da wir an der Bereitstellung dieser Interoperabilität arbeiten, ist es möglich, dass viele Drittanbietertools Azure Data Lake Storage Gen2 automatisch unterstützen.

## <a name="azcopy-support"></a>AzCopy-Unterstützung

Version 8 von AzCopy bietet keine Unterstützung für Azure Data Lake Storage Gen2.

Verwenden Sie stattdessen die neueste Vorschauversion von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)), da diese Version Azure Data Lake Storage Gen2-Endpunkte unterstützt.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) empfängt keine Ereignisse von Azure Data Lake Gen2-Konten, weil diese Konten noch keine solchen Ereignisse generieren.  

## <a name="soft-delete-and-snapshots"></a>Vorläufiges Löschen und Momentaufnahmen

Vorläufiges Löschen und Momentaufnahmen stehen für Azure Data Lake Storage Gen2-Konten nicht zur Verfügung.

Für Speicherkonten mit aktivierten hierarchischen Namespaces stehen noch keine der Features für die Versionsverwaltung zur Verfügung – [Momentaufnahmen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) und [vorläufiges Löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) eingeschlossen.

## <a name="object-level-storage-tiers"></a>Objektspeicherebenen

Objektspeicherebenen („Heiß“, „Kalt“, „Archiv“) stehen für Azure Data Lake Storage Gen 2-Konten noch nicht zur Verfügung. Für Speicherkonten ohne aktivierte hierarchische Namespaces sind sie jedoch verfügbar.

## <a name="azure-blob-storage-lifecycle-management-preview-policies"></a>Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung (Vorschau)

Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung (Vorschau) stehen für Azure Data Lake Storage Gen2-Konten noch nicht zur Verfügung.

Für Speicherkonten ohne aktivierte hierarchische Namespaces sind diese Richtlinien verfügbar.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Diagnoseprotokolle stehen für Azure Data Lake Storage Gen2-Konten nicht zur Verfügung.

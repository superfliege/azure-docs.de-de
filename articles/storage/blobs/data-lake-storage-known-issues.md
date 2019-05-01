---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Einschränkungen und bekannten Probleme mit Azure Data Lake Storage Gen2.
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: normesta
ms.openlocfilehash: 51230fe050c67253dd5b2bb3f23d03512df64ef6
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939251"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden die bekannten Probleme und vorübergehenden Einschränkungen für Data Lake Storage Gen2 beschrieben.

## <a name="sdk-support-for-data-lake-storage-gen2-accounts"></a>SDK-Unterstützung für Data Lake Storage Gen2-Konten

Es gibt keine SDKs, die mit Data Lake Storage Gen2-Konten funktionieren.

## <a name="blob-storage-apis"></a>Blob Storage-APIs

Blob Storage-APIs stehen für Data Lake Storage Gen2-Konten noch nicht zur Verfügung.

Diese APIs sind deaktiviert, um unbeabsichtigte Datenzugriffsfehler zu vermeiden, die durch die fehlende Interoperabilität zwischen Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs entstehen könnten.

Falls Sie diese APIs vor der Deaktivierung zum Laden von Daten verwendet haben und produktionsbedingt Zugriff auf diese Daten benötigen, wenden Sie sich mit folgenden Informationen an den Microsoft-Support:

* Abonnement-ID (die GUID, nicht der Name)

* Speicherkontoname(n)

* Ist Ihre Produktion direkt betroffen, und falls ja, für welche Speicherkonten?

* Müssen die Daten in ein anderes Speicherkonto kopiert werden, und falls ja, warum? (Diese Frage ist auch relevant, wenn Ihre Produktion nicht direkt betroffen ist.)

Unter diesen Umständen können wir den Zugriff auf die Blob-API für einen begrenzten Zeitraum wiederherstellen, damit Sie diese Daten in ein Speicherkonto kopieren können, bei dem das Feature „hierarchischer Namespace“ nicht aktiviert ist.

Nicht verwaltete VM-Datenträger hängen von den deaktivierten Blob Storage-APIs ab. Wenn Sie also einen hierarchischen Namespace für ein Speicherkonto aktivieren möchten, platzieren Sie nicht verwaltete VM-Datenträger ggf. in einem Speicherkonto, bei dem das Feature „hierarchischer Namespace“ nicht aktiviert ist.

## <a name="api-interoperability"></a>API-Interoperabilität

Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs sind nicht interoperabel.

Wenn Sie über Tools, Anwendungen, Dienste oder Skripts verfügen, die Blob-APIs verwenden, und sie für alle Inhalte einsetzen möchten, die Sie auf Ihr Konto hochladen, aktivieren Sie so lange keinen hierarchischen Namespace für Ihr Blob-Speicherkonto, bis die Interoperabiliät von Blob-APIs mit Azure Data Lake Gen2-APIs sichergestellt ist. Die Verwendung eines Speicherkontos ohne einen hierarchischen Namespace bedeutet, dass Sie keinen Zugriff auf Data Lake Storage Gen2-spezifische Features wie Zugriffssteuerungslisten (ACLs) für das Verzeichnis- und Dateisystem haben.

## <a name="azure-storage-explorer"></a>Azure Storage-Explorer

Wenn Sie Data Lake Storage Gen2-Konten über den Azure Storage-Explorer anzeigen oder verwalten möchten, benötigen Sie mindestens die Version `1.6.0` des Tools. Diese können Sie [hier](https://azure.microsoft.com/features/storage-explorer/) kostenlos herunterladen.

Beachten Sie, dass mit der in das Azure-Portal eingebetteten Version des Storage-Explorers derzeit keine Data Lake Storage Gen2-Konten mit aktiviertem Feature „hierarchischer Namespace“ angezeigt oder verwaltet werden können.

## <a name="blob-viewing-tool"></a>Tool zur Blobanzeige

Das Tool zur Blobanzeige im Azure-Portal bietet nur eingeschränkte Unterstützung für Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Drittanbieteranwendungen

Drittanbieteranwendungen bieten möglicherweise keine Unterstützung für Data Lake Storage Gen2.

Die Unterstützung liegt im Ermessen des jeweiligen Anbieters der Drittanbieteranwendung. Aktuell können Blob Storage-APIs und Data Lake Storage Gen2-APIs nicht zum Verwalten desselben Inhalts verwendet werden. Da wir an der Bereitstellung dieser Interoperabilität arbeiten, ist es möglich, dass viele Drittanbietertools Data Lake Storage Gen2 automatisch unterstützen.

## <a name="azcopy-support"></a>AzCopy-Unterstützung

Version 8 von AzCopy bietet keine Unterstützung für Data Lake Storage Gen2.

Verwenden Sie stattdessen die neueste Vorschauversion von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)), da diese Version Data Lake Storage Gen2-Endpunkte unterstützt.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) empfängt keine Ereignisse von Azure Data Lake Gen2-Konten, weil diese Konten noch keine solchen Ereignisse generieren.  

## <a name="soft-delete-and-snapshots"></a>Vorläufiges Löschen und Momentaufnahmen

Vorläufiges Löschen und Momentaufnahmen stehen für Data Lake Storage Gen2-Konten nicht zur Verfügung.

Für Storage-Konten mit aktiviertem Feature „hierarchischer Namespace“ stehen noch keine der Features für die Versionsverwaltung zur Verfügung – [Momentaufnahmen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) und [vorläufiges Löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) eingeschlossen.

## <a name="object-level-storage-tiers"></a>Objektspeicherebenen

Objektspeicherebenen („Heiß“, „Kalt“, „Archiv“) stehen für Azure Data Lake Storage Gen 2-Konten noch nicht zur Verfügung. Für Storage-Konten ohne aktiviertes Feature „hierarchischer Namespace“ sind sie jedoch verfügbar.

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung

Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung stehen für Data Lake Storage Gen2-Konten noch nicht zur Verfügung.

Für Storage-Konten ohne aktiviertes Feature „hierarchischer Namespace“ sind diese Richtlinien verfügbar.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Diagnoseprotokolle stehen für Data Lake Storage Gen2-Konten nicht zur Verfügung.

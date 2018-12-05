---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Einschränkungen und bekannten Probleme mit Azure Data Lake Storage Gen2.
services: storage
author: normesta
manager: twooley
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: normesta
ms.openlocfilehash: fa37c23f936173c19f32b76dffab8908176ebd75
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621706"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden die bekannten Probleme und vorübergehenden Einschränkungen für Azure Data Lake Storage Gen2 beschrieben.

## <a name="api-interoperability"></a>API-Interoperabilität

Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs sind nicht interoperabel.

Wenn Sie das gleiche Tool verwenden müssen, um mit allen Inhalten zu arbeiten, die Sie in Ihr Konto hochladen, dann aktivieren Sie keine hierarchischen Namespaces für Ihr Blob Storage-Konto, bis Interoperabilität für diese APIs gewährleistet ist. Die Verwendung eines Speicherkontos ohne die hierarchischen Namespaces bedeutet, dass Sie keinen Zugriff auf Data Lake Storage Gen2-spezifische Features wie z.B. Zugriffssteuerungslisten (ACLs) für Verzeichnis- und Dateisystem haben.

## <a name="blob-storage-apis"></a>Blob Storage-APIs

Blob Storage-APIs stehen für Azure Data Lake Storage Gen2-Konten noch nicht zur Verfügung.

Diese APIs sind deaktiviert, um unbeabsichtigte Datenzugriffsfehler zu vermeiden, die durch die fehlende Interoperabilität zwischen Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs entstehen könnten.

Nicht verwaltete VM-Datenträger hängen von diesen APIs ab. Wenn Sie also hierarchische Namespaces für ein Speicherkonto aktivieren möchten, platzieren Sie nicht verwaltete VM-Datenträger ggf. in einem Speicherkonto, für das keine hierarchischen Namespaces aktiviert sind.

## <a name="azure-storage-explorer"></a>Azure Storage-Explorer

Einige Features im Storage-Explorer arbeiten noch nicht mit Azure Data Lake Storage Gen2-Dateisystemen zusammen. Diese Einschränkungen gelten sowohl für die [eigenständige Version](https://azure.microsoft.com/features/storage-explorer/) von Azure Storage-Explorer als auch für die im Azure-Portal enthaltene Version.

## <a name="blob-viewing-tool"></a>Tool zur Blobanzeige

Das Tool zur Blobanzeige im Azure-Portal bietet nur eingeschränkte Unterstützung für Azure Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Drittanbieteranwendungen

Drittanbieteranwendungen bieten möglicherweise keine Unterstützung für Azure Data Lake Storage Gen2.

Die Unterstützung liegt im Ermessen des jeweiligen Anbieters der Drittanbieteranwendung. Aktuell können Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs nicht zum Verwalten desselben Inhalts verwendet werden. Da wir an der Bereitstellung dieser Interoperabilität arbeiten, ist es möglich, dass viele Drittanbietertools Azure Data Lake Storage Gen2 automatisch unterstützen.

## <a name="azcopy-support"></a>AzCopy-Unterstützung

Version 8 von AzCopy bietet keine Unterstützung für Azure Data Lake Storage Gen2.

Verwenden Sie stattdessen die neueste Vorschauversion von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)), da diese Version Azure Data Lake Storage Gen2-Endpunkte unterstützt.

## <a name="oauth-authentication"></a>OAuth-Authentifizierung

Dienste wie Azure Databricks, HDInsight und Azure Data Factory bieten noch keine Integration für die Azure Active Directory-Authentifizierung über ein OAuth-Bearertoken.

## <a name="access-control-lists-acl"></a>Zugriffssteuerungslisten (ACLs)

Zugriffssteuerungslisten auf Verzeichnis- und Dateiebene sind schwer zu verwalten. Es gibt kein UI-basiertes Tool, mit dem Sie diese Zugriffssteuerungslisten abrufen und festlegen können.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) empfängt keine Ereignisse von Azure Data Lake Gen2-Konten, weil diese Konten noch keine solchen Ereignisse generieren.  

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Sie können in einem Azure Data Lake Storage Gen2-Konto keine rollenbasierte Zugriffssteuerung auf Dateisystemobjekte anwenden.

## <a name="sql-data-warehouse-polybase"></a>SQL Data Warehouse PolyBase

Wenn Speicherfirewalls für ein Azure Storage-Konto aktiviert sind, kann SQL Data Warehouse [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017) nicht auf diese Konten zugreifen.

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

Wenden Sie sich an den Azure-Support, um Diagnoseprotokolle anzufordern. Geben Sie Ihren Kontonamen und den Zeitraum an, für den Sie Protokolle benötigen.

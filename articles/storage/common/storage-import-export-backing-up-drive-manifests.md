---
title: Sichern von Azure Import/Export-Laufwerkmanifesten | Microsoft Docs
description: Erfahren Sie, wie Sie die automatische Sicherung Ihrer Laufwerkmanifeste für den Microsoft Azure Import/Export-Dienst einrichten.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 61881508e18a2c7dbe1bc3be72d34423f862437a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473390"
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Sichern von Manifesten für Azure Import/Export-Aufträge

Laufwerkmanifeste können automatisch in Blobs gesichert werden, indem Sie die `BackupDriveManifest`-Eigenschaft in den REST-API-Vorgängen [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) oder [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) auf `true` festlegen. Die Laufwerkmanifeste werden standardmäßig nicht gesichert. Die Sicherungen der Laufwerkmanifeste werden als Blockblobs in einem Container innerhalb des Speicherkontos gespeichert, das dem Auftrag zugeordnet ist. Der Standardname des Containers ist `waimportexport`, Sie können jedoch beim Aufrufen der Vorgänge `Put Job` oder `Update Job Properties` in der `DiagnosticsPath`-Eigenschaft einen anderen Namen angeben. Die Sicherung des Manifestblobs wird im folgenden Format benannt: `waies/jobname_driveid_timestamp_manifest.xml`.

 Sie können den URI der gesicherten Laufwerkmanifeste für einen Auftrag abrufen, indem Sie den Vorgang [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) aufrufen. Der Blob-URI wird in der `ManifestUri`-Eigenschaft für die einzelnen Laufwerke zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)

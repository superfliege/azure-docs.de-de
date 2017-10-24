---
title: "Hinzufügen/Entfernen eines Serverendpunkts für die Azure-Dateisynchronisierung (Vorschau) | Microsoft-Dokumentation"
description: "Erfahren Sie, was Sie beim Planen einer Azure Files-Bereitstellung berücksichtigen müssen."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 92ac80953623a5a94d3104f30787c9636308c707
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="addremove-an-azure-file-sync-preview-server-endpoint"></a>Hinzufügen/Entfernen eines Serverendpunkts für die Azure-Dateisynchronisierung (Vorschau)
Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers zu verzichten. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

Ein *Serverendpunkt* stellt einen bestimmten Speicherort auf einem *registrierten Server* dar, z.B. einen Ordner auf einem Servervolume oder im Stammverzeichnis des Volumes. Mehrere Serverendpunkte können auf dem gleichen Volume vorhanden sein, wenn sich deren Namespaces nicht überschneiden (z.B. „F:\sync1“ und „F:\sync2“). Sie können Richtlinien für das Cloudtiering für jeden Serverendpunkt einzeln konfigurieren. Wenn Sie einen Serverspeicherort mit einem vorhandenen Satz von Dateien als Serverendpunkt zu einer Synchronisierungsgruppe hinzufügen, werden diese Dateien mit anderen Dateien, die sich bereits auf anderen Endpunkten in der Synchronisierungsgruppe befinden, zusammengeführt.

Informationen über die End-to-End-Bereitstellung der Azure-Dateisynchronisierung finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).

## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen eines Serverendpunkts müssen Sie zunächst sicherstellen, dass die folgenden Kriterien erfüllt sind: 
- Auf dem Server ist der Azure-Dateisynchronisierungs-Agent installiert und registriert. Anweisungen zum Installieren des Azure-Dateisynchronisierungs-Agents finden Sie im Artikel [Registrieren/Aufheben der Registrierung eines Servers mit der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-server-registration.md). 
- Stellen Sie sicher, dass der Speichersynchronisierungsdienst bereitgestellt wurde. Informationen zum Bereitstellen eines Speichersynchronisierungsdiensts finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md). 
- Stellen Sie sicher, dass eine Synchronisierungsgruppe bereitgestellt wurde. Erfahren Sie, wie Sie eine [Synchronisierungsgruppe erstellen](storage-sync-files-deployment-guide.md#create-a-sync-group).
- Stellen Sie sicher, dass der Server mit dem Internet verbunden ist und dass der Zugriff auf Azure möglich ist.

## <a name="add-a-server-endpoint"></a>Hinzufügen eines Serverendpunkts
Um einen Serverendpunkt hinzuzufügen, navigieren Sie zu der gewünschten Synchronisierungsgruppe, und wählen Sie „Serverendpunkt hinzufügen“ aus.

![Hinzufügen eines neuen Serverendpunkts im Bereich „Synchronisierungsgruppe“](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

Unter **Serverendpunkt hinzufügen** sind die folgenden Informationen erforderlich:

- **Registrierter Server**: Der Name des Servers oder Clusters, auf dem der Serverendpunkt erstellt werden soll.
- **Pfad**: Der Pfad auf dem Windows-Server, der als Teil der Synchronisierungsgruppe synchronisiert werden soll.
- **Cloudtiering**: Ein Schalter zum Aktivieren oder Deaktivieren von Cloudtiering, ein Feature, das das Auslagern von nur selten verwendeten oder aufgerufenen Dateien in Azure Files ermöglicht.
- **Freier Speicherplatz auf Volume**: Die Menge des freien Speicherplatzes auf dem Volume, auf dem sich der Serverendpunkt befindet. Wenn z.B. für ein Volume mit einem einzigen Serverendpunkt „Freier Speicherplatz auf Volume“ auf 50 % festgelegt ist, wird ungefähr die Hälfte der Daten in Azure Files ausgelagert. Die Azure-Dateifreigabe enthält immer eine vollständige Kopie der Daten in der Synchronisierungsgruppe, unabhängig davon, ob Cloudtiering aktiviert ist.

Wählen Sie **Erstellen** aus, um den Serverendpunkt hinzuzufügen. Die Dateien im Namespace einer Synchronisierungsgruppe bleiben synchron. 

## <a name="remove-a-server-endpoint"></a>Entfernen eines Serverendpunkts
Wenn für einen Serverendpunkt das Cloudtiering aktiviert ist, werden Dateien auf Ihre Azure-Dateifreigaben *ausgelagert*. Dadurch können lokale Dateifreigaben als Cache statt als vollständige Kopie des Datasets fungieren, um den Speicherplatz auf dem Dateiserver effizient zu nutzen. Wenn jedoch ein Serverendpunkt entfernt wird und ausgelagerte Dateien noch lokal auf dem Server vorhanden sind, kann auf diese Dateien nicht mehr zugegriffen werden. Aus diesem Grund müssen Sie vor dem Aufheben der Registrierung alle ausgelagerten Dateien aus Azure Files abrufen, wenn ein Zugriff auf die Dateien weiterhin möglich sein soll. 

Dies kann mithilfe des folgenden PowerShell-Cmdlets durchgeführt werden:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Wenn das lokale Volume, das den Server hostet, nicht genügend freien Speicherplatz aufweist, um alle ausgelagerten Daten abzurufen, schlägt das Cmdlet `Invoke-StorageSyncFileRecall` fehl.  

So entfernen Sie den Serverendpunkt:

1. Navigieren Sie zu dem Speichersynchronisierungsdienst, bei dem der Server registriert ist.
2. Navigieren Sie zur gewünschten Synchronisierungsgruppe.
3. Entfernen Sie in der Synchronisierungsgruppe im Speichersynchronisierungsdienst den gewünschten Serverendpunkt. Klicken Sie hierfür im Bereich „Synchronisierungsgruppe“ mit der rechten Maustaste auf den entsprechenden Serverendpunkt.

    ![Entfernen eines Serverendpunkts aus einer Synchronisierungsgruppe](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Nächste Schritte
- [Registrieren/Aufheben der Registrierung eines Servers mit der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-server-registration.md)
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
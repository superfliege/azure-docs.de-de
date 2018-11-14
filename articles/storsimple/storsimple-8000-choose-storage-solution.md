---
title: Optionen für die Datenübertragung an Azure mithilfe einer Appliance | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die richtige Appliance zum Übertragen von Daten an Azure auswählen.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 11/01/2018
ms.author: alkohli
ms.openlocfilehash: 0cb1a0bccbb989506988f36c515d59cddb832265
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263395"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>Vergleich der Datenübertragungsoptionen für StorSimple mit denen für Azure-Dateisynchronisierung und Data Box Edge 
 
Dieses Dokument enthält eine Übersicht der Optionen für die lokale Datenübertragung an Azure. Dabei werden Data Box Edge, die Azure-Dateisynchronisierung (AFS) und die StorSimple 8000-Serie verglichen.

- **[Data Box Edge:](/azure/databox-online/data-box-edge-overview.md)** Data Box Edge ist ein lokales Netzwerkgerät, das Daten in und aus Azure verschieben sowie mithilfe von KI-fähigem Edgecomputing auch beim Hochladen verarbeiten kann. Es wurde auf der Ignite 2018 angekündigt und befindet sich in der öffentlichen Vorschau. Data Box Gateway ist eine virtuelle Version des Geräts mit den gleichen Datenübertragungsfunktionen.
- **[Azure-Dateisynchronisierung:](/azure/storage/files/storage-sync-files-deployment-guide.md)** Mit der Azure-Dateisynchronisierung können die Dateifreigaben Ihrer Organisation in Azure Files zentralisiert werden, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit der Azure-Dateisynchronisierung werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Die allgemeine Verfügbarkeit von AFS wurde Anfang des Jahres 2018 bekannt gegeben.
- **[StorSimple:](/azure/storsimple/storsimple-overview.md)** StorSimple ist ein Hybridgerät, mit dem Unternehmen ihre Speicherinfrastruktur für primären Speicher, Datenschutz, Archivierung und Notfallwiederherstellung auf eine einzige Lösung konsolidieren können, da es eng mit Azure Storage integriert ist. Den Produktlebenszyklus von StorSimple finden Sie [hier](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).

## <a name="comparison-summary"></a>Zusammenfassender Vergleich

|                           |StorSimple 8000   |Azure-Dateisynchronisierung   |Data Box Edge (Vorschauversion)           |
|---------------------------|----------------------------------------|-------------------------------|-----------------------------------------|
|Übersicht         |Mehrstufiger Hybridspeicher und Archivierung|Allgemeiner Dateiserverspeicher mit Cloudtiering und Synchronisierung mehrerer Standorte.  |Speicherlösung zum Vorverarbeiten von Daten und Senden dieser über das Netzwerk an Azure.        |
|Szenarien        |Dateiserver, Archivierung, Sicherungsziel |Dateiserver, Archivierung (mehrere Standorte)   |Datenübertragung, Datenvorverarbeitung einschließlich ML-Rückschlüssen, IoT, Archivierung    |
|Edgecomputing     |Nicht verfügbar |Nicht verfügbar |Unterstützt das Ausführen von Containern mit Azure IoT Edge    |
|Formfaktor      |Physisches Gerät   |Unter Windows Server installierter Agent |Physisches Gerät   |
|Hardware         |Physisches Gerät, das von Microsoft im Rahmen des Diensts bereitgestellt wird | Vom Kunden bereitgestellt |Physisches Gerät, das von Microsoft im Rahmen des Diensts bereitgestellt wird  |
|Datenformat      |Benutzerdefiniertes Format   |Dateien         |Blobs oder Dateien    |
|Protokollunterstützung |iSCSI          |SMB, NFS    | SMB oder NFS      |
|Preise          |[StorSimple-Preise](https://azure.microsoft.com/pricing/details/storsimple/) |[AFS-Preise](https://azure.microsoft.com/pricing/details/storage/files/)  |[Data Box Edge-Preise](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie etwas über [Azure Data Box Edge](/azure/databox-online/data-box-edge-overview.md) und [Azure Data Box Gateway](/azure/databox-online/data-box-gateway-overview.md).
- Erfahren Sie etwas über die [Azure-Dateisynchronisierung](/azure/storage/files/storage-sync-files-deployment-guide.md).
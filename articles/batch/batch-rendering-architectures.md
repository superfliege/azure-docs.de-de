---
title: Rendering in Azure – Referenzarchitekturen
description: Architekturen für die Verwendung von Azure Batch und anderen Azure-Diensten zum Erweitern einer lokalen Render-Farm mittels Cloud Bursting in die Cloud
services: batch
author: davefellows
manager: jeconnoc
ms.author: danlep
ms.date: 08/13/2018
ms.topic: conceptual
ms.openlocfilehash: 0fe101ee6eb88094034b90c4d39f06ba509c9512
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "40099684"
---
# <a name="reference-architectures-for-azure-rendering"></a>Referenzarchitekturen für das Rendering in Azure

In diesem Artikel werden Architekturdiagramme auf hoher Ebene für Szenarien zum Erweitern (oder „Bursting“) einer lokalen Render-Farm zu Azure gezeigt. Die Beispiele zeigen verschiedene Optionen für Compute-, Netzwerk- und Speicherdienste in Azure.

## <a name="hybrid-with-nfs-or-cfs"></a>Hybrid mit NFS oder CFS

Das folgende Diagramm zeigt ein Hybridszenario, das die folgenden Azure-Dienste umfasst:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk**: Lokal: Azure Expressroute oder VPN. Azure: Azure VNET.

* **Speicher**: Eingabe- und Ausgabedateien: NFS oder CFS mithilfe von Azure-VMs, synchronisiert mit lokalem Speicher über Azure-Dateisynchronisierung oder RSync.

  ![Cloudbursting – hybrid mit NFS oder CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Hybrid mit Blobfuse

Das folgende Diagramm zeigt ein Hybridszenario, das die folgenden Azure-Dienste umfasst:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk**: Lokal: Azure Expressroute oder VPN. Azure: Azure VNET.

* **Speicher**: Eingabe- und Ausgabedateien: Blobspeicher, eingebunden in Computeressourcen über Azure Blobfuse.

  ![Cloudbursting – hybrid mit Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hybrides Compute und Speichern

Das folgende Diagramm zeigt ein vollständig verbundenes Hybridszenario für Compute und Speichern und umfasst die folgenden Azure-Dienste:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk**: Lokal: Azure Expressroute oder VPN. Azure: Azure VNET.

* **Speicher**: Standortübergreifend: Avere vFXT. Optionale Archivierung von lokalen Dateien über Azure Data Box in Blob Storage.

  ![Cloudbursting – hybrides Compute und Speichern](./media/batch-rendering-architectures/hybrid-compute-storage.png)


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Verwendung von [Render-Managern](batch-rendering-render-managers.md) mit Azure Batch.

* Weitere Informationen zu Optionen für das [Rendering in Azure](batch-rendering-service.md).
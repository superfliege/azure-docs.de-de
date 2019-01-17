---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/02/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b278cce8f885f31f9d59fd389261812e04e58405
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54066989"
---
Azure Storage bietet drei Arten von Speicherkonten. Jeder Typ unterstützt unterschiedliche Features und verfügt über ein eigenes Preismodell. Informieren Sie sich vor dem Erstellen eines Speicherkontos genau über diese Unterschiede, um den Kontotyp zu ermitteln, der sich für Ihre Anwendungen am besten eignet. Folgende Speicherkontotypen stehen zur Verfügung:

* **Konten vom Typ „Allgemein v2“:** Grundlegender Speicherkontotyp für Blobs, Dateien, Warteschlangen und Tabellen. Empfohlen für die meisten Azure Storage-Szenarien.
* **Konten vom Typ „Allgemein v1“:** Legacy-Speicherkontotyp für Blobs, Dateien, Warteschlangen und Tabellen. Verwenden Sie nach Möglichkeit Konten vom Typ „Allgemein v2“.
* **Konten vom Typ „Blobspeicher“:** Reine Blobspeicherkonten. Verwenden Sie nach Möglichkeit Konten vom Typ „Allgemein v2“. 

Die folgende Tabelle beschreibt die Speicherkontotypen und ihre jeweiligen Funktionen:

| Speicherkontotyp | Unterstützte Dienste                       | Unterstützte Leistungsstufen | Unterstützte Zugriffsebenen               | Replikationsoptionen                                                | Bereitstellungsmodell<sup>1</sup>  | Verschlüsselung<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| Allgemein v2   | Blob, Datei, Warteschlange, Tabelle und Datenträger       | Standard, Premium           | Heiß, kalt, Archiv<sup>3</sup> | LRS, ZRS<sup>4</sup>, GRS, RA-GRS | Ressourcen-Manager | Verschlüsselt  |
| Allgemein v1   | Blob, Datei, Warteschlange, Tabelle und Datenträger       | Standard, Premium           | N/V                                  | LRS, GRS, RA-GRS                                                   | Resource Manager, klassisch  | Verschlüsselt  |
| Blob Storage         | Blob (nur Blockblobs und Anfügeblobs) | Standard                    | Heiß, kalt, Archiv<sup>3</sup>                            | LRS, GRS, RA-GRS                                                   | Ressourcen-Manager  | Verschlüsselt  |

<sup>1</sup>Die Verwendung des Azure Resource Manager-Bereitstellungsmodells wird empfohlen. Speicherkonten, die das klassische Bereitstellungsmodell verwenden, können an einigen Standorten weiterhin erstellt werden, und vorhandene klassische Konten werden weiterhin unterstützt. Weitere Informationen finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../articles/azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>Alle Speicherkonten sind per Storage Service Encryption (SSE) für ruhende Daten verschlüsselt. Weitere Informationen finden Sie unter [Azure Storage Service Encryption für ruhende Daten](../articles/storage/common/storage-service-encryption.md).

<sup>3</sup>Die Ebene „Archiv“ ist nur auf Ebene der einzelnen Blobs verfügbar, nicht auf Ebene des Speicherkontos. Nur Blockblobs und Anfügeblobs können archiviert werden. Weitere Informationen finden Sie unter [Azure Blob Storage: Speicherebenen „Premium“ (Vorschauversion), „Heiß“, „Kalt“ und „Archiv“](../articles/storage/blobs/storage-blob-storage-tiers.md).

<sup>4</sup>Zonenredundanter Speicher (ZRS) ist nur für Storage Standard-Konten des Typs „Allgemein v2“ verfügbar. Weitere Informationen zu ZRS finden Sie unter [Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen](../articles/storage/common/storage-redundancy-zrs.md). Weitere Informationen zu weiteren Replikationsoptionen finden Sie unter [Azure Storage-Replikation](../articles/storage/common/storage-redundancy.md).

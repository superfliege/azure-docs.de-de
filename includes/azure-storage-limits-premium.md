---
title: Includedatei
description: Includedatei
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e3d2466f05028a963256dbcc052c46650857836d
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372712"
---
### <a name="premium-performance-block-blob-storage"></a>Blockblobspeicher mit Premium-Leistung

Ein Blockblob-Speicherkonto mit Premium-Leistung ist für Anwendungen optimiert, die kleinere Objekte im Kilobytebereich verwenden. Dies ist ideal für Anwendungen, die sehr hohe Transaktionsraten oder Speicher mit gleichbleibend geringer Latenz erfordern. Der Blockblobspeicher mit Premium-Leistung ist zur Skalierung mit Ihren Anwendungen ausgelegt. Wenn Sie Anwendungen bereitstellen möchten, die Hunderttausende von Anforderungen pro Sekunde oder Speicherkapazität im Petabytebereich erfordern, nehmen Sie Kontakt mit uns auf, indem Sie eine Supportanfrage im [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) übermitteln.

### <a name="premium-performance-page-blob-storage"></a>Seitenblobspeicher mit Premium-Leistung

Speicherkonten des Typs „Premium-Leistung“, „Allgemein v1“ oder „Allgemein v2“ weisen die folgenden Skalierbarkeitsziele auf:

| Gesamtkapazität des Kontos                            | Gesamtbandbreite für ein lokal redundantes Speicherkonto                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Datenträgerkapazität: 35 TB <br>Kapazität für Momentaufnahmen: 10 TB | Bis zu 50 Gigabit pro Sekunde für eingehenden<sup>1</sup> und ausgehenden<sup>2</sup> Datenverkehr |

<sup>1</sup> Alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden

<sup>2</sup> Alle Daten (Antworten), die von einem Speicherkonto empfangen werden

Wenn Sie Speicherkonten mit Premium-Leistung für nicht verwaltete Datenträger verwenden und Ihre Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreitet, sollten Sie die Migration zu verwalteten Datenträgern erwägen. Falls die Migration zu verwalteten Datenträgern für Sie keine Option ist, können Sie Ihre Anwendung für die Nutzung mehrerer Speicherkonten erstellen. Partitionieren Sie Ihre Daten dann basierend auf diesen Speicherkonten. Wenn Sie beispielsweise 51-TB-Datenträger an mehrere VMs anfügen möchten, können Sie sie auf zwei Speicherkonten verteilen. 35 TB ist der Grenzwert pro Storage Premium-Konto. Stellen Sie sicher, dass ein Speicherkonto mit Premium-Leistung stets Datenträger mit einer Größe von maximal 35 TB enthält.
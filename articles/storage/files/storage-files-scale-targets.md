---
title: "Skalierbarkeits- und Leistungsziele für Azure Files | Microsoft-Dokumentation"
description: "Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files, einschließlich Kapazität, Anforderungsrate sowie Grenzwerte für ein- und ausgehende Bandbreite."
services: storage
documentationcenter: na
author: wmgries
manager: klaasl
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/17/2017
ms.author: wgries
ms.openlocfilehash: 35d430b683224d1035cccdfb58b9db415d47ea3b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2017
---
# <a name="azure-files-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Azure Files
[Azure Files](storage-files-introduction.md) bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll SMB zugegriffen werden kann. Dieser Artikel behandelt die Skalierbarkeits- und Leistungsziele für Azure Files und Azure File Sync (Vorschau).

Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckt, können jedoch von anderen Variablen in Ihrer Bereitstellung beeinflusst werden. Beispielsweise kann der Durchsatz für eine Datei auch durch die verfügbare Netzwerkbandbreite und nicht nur durch die Server begrenzt sein, die den Azure Files-Dienst hosten. Wir empfehlen Ihnen ausdrücklich, Ihr Nutzungsverhalten zu testen, um festzustellen, ob Skalierbarkeit und Leistung von Azure Files Ihren Anforderungen entspricht. Wir verpflichten uns auch, diese Grenzwerte mit der Zeit zu erhöhen. Zögern Sie nicht, uns entweder in den Kommentaren unten oder über [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) Feedback dazu zu geben, welche Grenzwerte wir für Sie erhöhen sollen.

## <a name="azure-storage-account-scale-targets"></a>Skalierbarkeitsziele für das Azure-Speicherkonto
Die übergeordnete Ressource einer Azure-Dateifreigabe ist ein Azure-Speicherkonto. Ein Speicherkonto stellt einen Speicherpool in Azure dar, der von mehreren Speicherdiensten, einschließlich Azure Files, zum Speichern von Daten verwendet werden kann. Andere Dienste, die Daten in Speicherkonten speichern, sind Azure Blob Storage, Azure Queue Storage und Azure Table Storage. Die folgenden Ziele gelten für alle Speicherdienste, die Daten in einem Speicherkonto speichern:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Die Speicherbelegung durch andere Speicherdienste wirkt sich auf die Azure-Dateifreigaben in Ihrem Speicherkonto aus. Wenn Sie z.B. mit Azure Blob Storage die maximale Speicherkapazität erreichen, können Sie in Ihrer Azure-Dateifreigabe keine neuen Dateien erstellen, auch wenn Ihre Azure-Dateifreigabe kleiner als die maximale Freigabegröße ist.

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für Azure File Sync
Bei Azure File Sync haben wir als Entwicklungsziel eine unbegrenzte Nutzung vorgesehen, was jedoch nicht immer möglich ist. Die folgende Tabelle zeigt die bei unseren Tests ermittelten Grenzwerte und für welche Ziele tatsächlich feste Grenzen gelten:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="see-also"></a>Weitere Informationen
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
- [Skalierbarkeits- und Leistungsziele für andere Speicherdienste](../common/storage-scalability-targets.md)
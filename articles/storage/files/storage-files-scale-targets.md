---
title: Skalierbarkeits- und Leistungsziele für Azure Files | Microsoft-Dokumentation
description: Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files, einschließlich Kapazität, Anforderungsrate sowie Grenzwerte für ein- und ausgehende Bandbreite.
services: storage
documentationcenter: na
author: wmgries
manager: aungoo
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: beb3e5caf8c8dce9b2ea06bbd0a2ea5a4e05a714
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738073"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Azure Files
[Azure Files](storage-files-introduction.md) bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll SMB zugegriffen werden kann. Dieser Artikel behandelt die Skalierbarkeits- und Leistungsziele für Azure Files und Azure File Sync (Vorschau).

Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckt, können jedoch von anderen Variablen in Ihrer Bereitstellung beeinflusst werden. Beispielsweise kann der Durchsatz für eine Datei auch durch die verfügbare Netzwerkbandbreite und nicht nur durch die Server begrenzt sein, die den Azure Files-Dienst hosten. Wir empfehlen Ihnen ausdrücklich, Ihr Nutzungsverhalten zu testen, um festzustellen, ob Skalierbarkeit und Leistung von Azure Files Ihren Anforderungen entspricht. Wir verpflichten uns auch, diese Grenzwerte mit der Zeit zu erhöhen. Zögern Sie nicht, uns entweder in den Kommentaren unten oder über [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) Feedback dazu zu geben, welche Grenzwerte wir für Sie erhöhen sollen.

## <a name="azure-storage-account-scale-targets"></a>Skalierbarkeitsziele für das Azure-Speicherkonto
Die übergeordnete Ressource einer Azure-Dateifreigabe ist ein Azure-Speicherkonto. Ein Speicherkonto stellt einen Speicherpool in Azure dar, der von mehreren Speicherdiensten, einschließlich Azure Files, zum Speichern von Daten verwendet werden kann. Andere Dienste, die Daten in Speicherkonten speichern, sind Azure Blob Storage, Azure Queue Storage und Azure Table Storage. Die folgenden Ziele gelten für alle Speicherdienste, die Daten in einem Speicherkonto speichern:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Die Belegung von Speicherkonten durch andere Speicherdienste wirkt sich auf die Azure-Dateifreigaben in Ihrem Speicherkonto aus. Wenn Sie z.B. mit Azure Blob Storage die maximale Speicherkapazität erreichen, können Sie in Ihrer Azure-Dateifreigabe keine neuen Dateien erstellen, auch wenn Ihre Azure-Dateifreigabe kleiner als die maximale Freigabegröße ist.

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für Azure File Sync
Bei Azure File Sync haben wir als Entwicklungsziel eine unbegrenzte Nutzung vorgesehen, was jedoch nicht immer möglich ist. Die folgende Tabelle zeigt die bei unseren Tests ermittelten Grenzwerte und für welche Ziele tatsächlich feste Grenzen gelten:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure File Sync-Leistungsmetriken
Da der Azure File Sync-Agent auf einem Windows Server-Computer ausgeführt wird, der mit den Azure-Dateifreigaben verbunden wird, hängt die effektive Synchronisierungsleistung von einer Reihe von Faktoren in Ihrer Infrastruktur ab: von Windows Server und der zugrunde liegenden Datenträgerkonfiguration, der Netzwerkbandbreite zwischen dem Server und Azure Storage, der Dateigröße, der gesamten Datasetgröße und der Aktivität im Dataset. Da Azure File Sync auf Dateiebene ausgeführt wird, werden die Leistungsmerkmale einer Azure File Sync-basierten Lösung besser in der Anzahl von Objekten (Dateien und Verzeichnisse) gemessen, die pro Sekunde verarbeitet werden. 
 
Bei Azure File Sync ist die Leistung in zwei Phasen entscheidend:
1. Bei der **ersten einmaligen Bereitstellung**: Informationen zum Optimieren der Leistung bei der ersten Bereitstellung finden Sie unter [Onboarding bei Azure File Sync](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync).
2. Bei der **fortlaufenden Synchronisierung**: Nachdem zunächst ein Seeding für die Daten in den Azure-Dateifreigaben ausgeführt wird, synchronisiert Azure File Sync fortlaufend mehrere Endpunkte.

Wenn Sie die Bereitstellung für jede der Phasen planen, sehen Sie sich im Folgenden die Ergebnisse an, die bei den internen Tests auf einem System mit einer Konfiguration zustande kamen.
| Systemkonfiguration |  |
|-|-|
| CPU | 64 virtuelle Kerne mit 64-MiB-L3-Cache |
| Arbeitsspeicher | 128 GB |
| Datenträger | SAS-Datenträger mit RAID 10 mit batteriegepuffertem Cache |
| Netzwerk | 1-GBit/s-Netzwerk |
| Workload | Allgemeiner Dateiserver|

| Erste einmalige Bereitstellung  |  |
|-|-|
| Anzahl der Objekte | 10 Millionen Objekte | 
| Datasetgröße| Ca. 4 TiB |
| Durchschnittliche Dateigröße | Ca. 500 KiB (größte Datei: 100 GiB) |
| Uploaddurchsatz | 15 Objekte pro Sekunde |
| Durchsatz beim Download von Namespaces* | 350 Objekte pro Sekunde |
 
*Wenn ein neuer Serverendpunkt erstellt wird, lädt der Azure File Sync-Agent keine Dateiinhalte herunter. Zuerst synchronisiert er den vollständigen Namespace und löst dann im Hintergrund einen Rückruf aus, um die Dateien herunterzuladen, entweder in ihrer Gesamtheit oder bei aktiviertem Cloudtiering in der Cloudtieringrichtliniengruppe für den Serverendpunkt.

| Laufende Synchronisierung  |   |
|-|--|
| Anzahl der synchronisierten Objekte| 125.000 Objekte (Änderungsumfang ca. 1 %) | 
| Datasetgröße| 50 GiB |
| Durchschnittliche Dateigröße | Ca. 500 KiB (größte Datei: 100 GiB) |
| Uploaddurchsatz | 20 Objekte pro Sekunde |
| Durchsatz bei vollständigen Downloads* | 30 Objekte pro Sekunde |
 
Wenn Cloudtiering aktiviert ist, werden Sie wahrscheinlich eine bessere Leistung beobachten, da nur ein Teil der Dateidaten heruntergeladen wird. Azure File Sync lädt die Daten zwischengespeicherter Dateien nur dann herunter, wenn sie auf einem der Endpunkte geändert werden. Bei mehrstufigen oder neu erstellten Dateien lädt der Agent nicht die Dateidaten herunter, sondern synchronisiert lediglich den Namespace auf allen Serverendpunkten. Der Agent unterstützt auch teilweise Downloads von mehrstufigen Dateien, wenn Benutzer auf diese zugreifen. 
 
> [!Note]  
> Die oben genannten Zahlen sind kein Hinweis auf die Leistung, die bei Ihnen zu erwarten ist. Die tatsächliche Leistung hängt von mehreren Faktoren ab, die am Anfang dieses Abschnitts beschrieben werden.

Als allgemeine Richtlinie für Ihre Bereitstellung sollten Sie einige Dinge berücksichtigen:
- Der Objektdurchsatz wird ungefähr relativ zur Anzahl der Synchronisierungsgruppen auf dem Server skaliert. Das Aufteilen von Daten in mehrere Synchronisierungsgruppen auf einem Server führt zu einem besseren Durchsatz, der zudem durch den Server und Netzwerk begrenzt wird.
- Der Objektdurchsatz ist umgekehrt proportional zum Durchsatz in MiB pro Sekunde. Bei kleineren Dateien tritt ein höherer Durchsatz im Hinblick auf die Anzahl der verarbeiteten Objekte pro Sekunde auf, jedoch einen niedrigeren Durchsatz in MiB pro Sekunde aufweist. Im Gegensatz dazu werden bei größeren Dateien weniger Objekte pro Sekunde verarbeitet, dafür jedoch ein höherer Durchsatz in MiB pro Sekunde erzielt. Der Durchsatz in MiB pro Sekunde wird durch die Azure Files-Skalierungsziele beschränkt. 

## <a name="see-also"></a>Weitere Informationen
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
- [Skalierbarkeits- und Leistungsziele für andere Speicherdienste](../common/storage-scalability-targets.md)
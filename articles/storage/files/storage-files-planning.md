---
title: Planung für eine Azure Files-Bereitstellung | Microsoft-Dokumentation
description: Erfahren Sie, was Sie beim Planen einer Azure Files-Bereitstellung berücksichtigen müssen.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: a9c37258d7c9631c6e5fe13007b78c4205a1c249
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473883"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planung für eine Azure Files-Bereitstellung
[Azure Files](storage-files-introduction.md) bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Standardprotokoll SMB zugegriffen werden kann. Da Azure Files vollständig verwaltet ist, ist die Bereitstellung in Produktionsszenarien wesentlich einfacher als das Bereitstellen und Verwalten eines Dateiservers oder NAS-Geräts. In diesem Artikel werden die Aspekte behandelt, die beim Bereitstellen einer Azure-Dateifreigabe in der Produktionsumgebung Ihrer Organisation zu berücksichtigen sind.

## <a name="management-concepts"></a>Verwaltungskonzepte
 Das folgende Diagramm veranschaulicht die Verwaltungskonstrukte von Azure Files:

![Dateistruktur](./media/storage-files-introduction/files-concepts.png)

* **Storage Account** (Speicherkonto): Alle Zugriffe auf den Azure-Speicher erfolgen über ein Speicherkonto. Ausführliche Informationen zur Kapazität von Speicherkonten finden Sie unter [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Skalierbarkeits- und Leistungsziele für Azure Storage).

* **Freigabe:** Eine Datenspeicherfreigabe ist eine SMB-Dateifreigabe in Azure. Alle Verzeichnisse und Dateien müssen in der übergeordneten Freigabe erstellt werden. Ein Konto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze von 5 TiB für die Dateifreigabe erreicht ist.

* **Verzeichnis:** Eine optionale Hierarchie von Verzeichnissen.

* **Datei:** Eine Datei in der Freigabe. Die Datei kann bis zu 1 TiB groß sein.

* **URL-Format:** Für Anforderung an eine Azure-Dateifreigabe mithilfe des REST-Protokolls „File“ sind Dateien mit dem folgenden URL-Format adressierbar:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Datenzugriffsmethode
Azure Files bietet zwei integrierte, praktische Datenzugriffsmethoden, die Sie für den Zugriff auf Ihre Daten getrennt oder gemeinsam verwenden können:

1. **Direkter Cloudzugriff:** Azure-Dateifreigaben können unter [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) und/oder [Linux](storage-how-to-use-files-linux.md) mithilfe des Standardprotokolls Server Message Block (SMB) oder über die REST-API „File“ eingebunden werden. Bei SMB erfolgen Lese- und Schreibvorgänge in Dateien auf der Freigabe direkt auf der Dateifreigabe in Azure. Zur Bereitstellung durch eine VM in Azure muss der SMB-Client des Betriebssystems mindestens SMB 2.1 unterstützen. Für eine lokale Bereitstellung, z.B. auf der Arbeitsstation eines Benutzers, muss der von der Arbeitsstation unterstützte SMB-Client mindestens SMB 3.0 (mit Verschlüsselung) unterstützen. Zusätzlich zu SMB können neue Anwendungen oder Dienste über die REST-API „File“ direkt auf die Dateifreigabe zugreifen. Diese stellt eine einfache und skalierbare Anwendungsprogrammierschnittstelle für die Softwareentwicklung bereit.
2. **Azure-Dateisynchronisierung:** Mit der Azure-Dateisynchronisierung können die Freigaben auf Windows Server-Computern lokal oder in Azure repliziert werden. Ihre Benutzer greifen auf die Dateifreigabe über den Windows-Server zu, z.B. über eine SMB- oder NFS-Freigabe. Dies ist nützlich für Szenarien, in denen Daten weit entfernt von einem Azure-Rechenzentrum abgerufen und geändert werden, z.B. in einer Zweigstelle. Daten können zwischen mehreren Windows Server-Endpunkten repliziert werden, z.B. zwischen mehreren Zweigstellen. Schließlich können Daten in Azure Files mehrstufig gespeichert werden, z.B. so, dass über den Server weiter auf alle Daten zugegriffen werden kann, der Server aber nicht über eine vollständige Kopie der Daten verfügt. Vielmehr werden die Daten beim Öffnen durch den Benutzer nahtlos abgerufen.

Die folgende Tabelle verdeutlicht, wie Ihre Benutzer und Anwendungen auf Ihre Azure-Dateifreigabe zugreifen können:

| | Direkter Cloudzugriff | Azure-Dateisynchronisierung |
|------------------------|------------|-----------------|
| Welche Protokolle müssen Sie verwenden? | Azure Files unterstützt SMB 2.1, SMB 3.0 und die REST-API „File“. | Auf die Azure-Dateifreigabe kann über alle unter Windows Server unterstützten Protokolle (SMB, NFS, FTPS usw.) zugegriffen werden. |  
| Wo wird Ihre Workload ausgeführt? | **In Azure:** Azure Files bietet einen direkten Zugriff auf Ihre Daten. | **Lokal mit langsamem Netzwerk:** Windows-, Linux- und macOS-Clients können eine lokale Windows-Dateifreigabe als schnellen Cache Ihrer Azure-Dateifreigabe einbinden. |
| Welche Art von Zugriffssteuerungslisten benötigen Sie? | Freigabe- und Dateiebene. | Freigabe-, Datei- und Benutzerebene. |

## <a name="data-security"></a>Datensicherheit
Azure Files bietet mehrere integrierte Optionen zum Gewährleisten der Datensicherheit:

* Unterstützung für die Verschlüsselung in beiden drahtgebundenen Protokollen: SMB 3.0-Verschlüsselung und REST-API „File“ über HTTPS. Standardmäßig gilt: 
    * Clients, die die SMB 3.0-Verschlüsselung unterstützen, senden und empfangen Daten über einen verschlüsselten Kanal.
    * Clients, die SMB 3.0 mit Verschlüsselung nicht unterstützen, können innerhalb des Rechenzentrums über SMB 2.1 oder SMB 3.0 ohne Verschlüsselung kommunizieren. SMB-Clients dürfen nicht unverschlüsselt zwischen Rechenzentren über SMB 2.1 oder SMB 3.0 kommunizieren.
    * Clients können über die REST-API „File“ mit HTTP oder HTTPS kommunizieren.
* Verschlüsselung ruhender Daten ([Azure-Speicherdienstverschlüsselung](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): Die Speicherdienstverschlüsselung (SSE) ist für alle Speicherkonten aktiviert. Ruhende Daten werden mit vollständig verwalteten Schlüsseln verschlüsselt. Durch Verschlüsselung ruhender Daten erhöhen sich weder die Speicherkosten, noch wird die Leistung verringert. 
* Optionale Anforderung der Verschlüsselung von Daten während der Übertragung: Bei Auswahl dieser Option lehnt Azure Files den Zugriff auf die Daten über unverschlüsselte Kanäle ab. Konkret sind bei verschlüsselten Verbindungen nur HTTPS und SMB 3.0 zugelassen. 

    > [!Important]  
    > Die Notwendigkeit einer sicheren Datenübertragung führt dazu, dass ältere SMB-Clients, die nicht in der Lage sind, mit SMB 3.0 mit Verschlüsselung zu kommunizieren, nicht funktionieren. Weitere Informationen finden Sie unter [Einbinden unter Windows](storage-how-to-use-files-windows.md), [Einbinden unter Linux](storage-how-to-use-files-linux.md) und [Einbinden unter macOS](storage-how-to-use-files-mac.md).

Für maximale Sicherheit empfehlen wir dringend, stets sowohl die Verschlüsselung ruhender Daten als auch die Verschlüsselung von Daten während der Übertragung zu aktivieren, wenn Sie moderne Clients für den Zugriff auf Ihre Daten verwenden. Wenn Sie z. B. eine Freigabe auf einer VM mit Windows Server 2008 R2 bereitstellen müssen, die nur SMB 2.1 unterstützt, müssen Sie unverschlüsselten Datenverkehr zu Ihrem Speicherkonto zulassen, da SMB 2.1 keine Verschlüsselung unterstützt.

Wenn Sie über die Azure-Dateisynchronisierung auf Ihre Azure-Dateifreigabe zugreifen, verwenden wir stets HTTPS und SMB 3.0 mit Verschlüsselung, um Ihre Daten mit Ihren Windows-Servern zu synchronisieren. Dies gilt unabhängig davon, ob Sie eine Verschlüsselung ruhender Daten benötigen.

## <a name="file-share-performance-tiers"></a>Leistungsstufen von Dateifreigaben
Azure Files unterstützt zwei Leistungsstufen: Standard und Premium.

* **Standard-Dateifreigaben** werden durch rotierende Festplattenlaufwerke (HDDs) gestützt, die eine zuverlässige Leistung für E/A-Workloads bieten, die weniger anfällig für Leistungsschwankungen sind, z.B. Dateifreigaben für allgemeine Zwecke und Dev/Test-Umgebungen. Standard-Dateifreigaben sind nur in einem nutzungsbasierten Abrechnungsmodell verfügbar.
* **Premium-Dateifreigaben (Vorschauversion)** werden durch Solid-State-Laufwerke (SSDs) gestützt, die konsistent hohe Leistung und niedrige Latenz im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge für Workloads mit besonders umfassenden E/A bieten. Dadurch sind sie für eine Vielzahl von Workloads wie Datenbanken, Websitehosting, Entwicklungsumgebungen usw. geeignet. Premium-Dateifreigaben sind nur in einem Abrechnungsmodell nach Bereitstellung verfügbar.

### <a name="provisioned-shares"></a>Bereitgestellte Freigaben
Premium-Dateifreigaben werden basierend auf einem festen Verhältnis aus GiB/IOPS/Durchsatz bereitgestellt. Für jedes bereitgestellte GiB erhält die Freigabe 1 IOPS und einen Durchsatz von 0,1 MiB/s bis zum maximalen Grenzwert pro Freigabe. Die kleinste zulässige Bereitstellung beträgt 100 GiB mit den minimalen Werten für IOPS/Durchsatz. Die Größe der Dateifreigabe kann jederzeit erhöht oder verringert werden, sie kann jedoch nur einmal alle 24 Stunden seit der letzten Erhöhung verringert werden.

Auf einer Best-Effort-Basis können alle Freigaben für 60 Minuten oder länger (je nach Größe der Freigabe) auf bis zu 3 IOPS pro GiB an bereitgestelltem Speicher erhöht werden (Burst). Neue Freigaben beginnen mit dem vollständigen Burstguthaben, basierend auf der bereitgestellten Kapazität.

| Bereitgestellte Kapazität | 100 GB | 500 GiB | 1 TiB | 5 TiB | 
|----------------------|---------|---------|-------|-------|
| IOPS-Grundwert | 100 | 500 | 1024 | 5.120 | 
| Burstgrenzwert | 300 | 1.500 | 3.072 | 15.360 | 
| Throughput | 110 MiB/s | 150 MiB/s | 202 MiB/s | 612 MiB/s |

## <a name="file-share-redundancy"></a>Dateifreigaberedundanz
Azure Files unterstützt drei Optionen für Datenredundanz: lokal redundanter Speicher (LRS), zonenredundanter Speicher (ZRS) und georedundanter Speicher (GRS). In den folgenden Abschnitten werden die Unterschiede zwischen den verschiedenen Redundanzoptionen erläutert:

### <a name="locally-redundant-storage"></a>Lokal redundanter Speicher
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Zonenredundanter Speicher
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Georedundanter Speicher
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Muster des Datenwachstums
Die maximale Größe einer Azure-Dateifreigabe beträgt derzeit 5 TiB. Aufgrund dieser aktuellen Einschränkung müssen Sie das erwartete Wachstum berücksichtigen, wenn Sie eine Azure-Dateifreigabe bereitstellen. 

Mithilfe der Azure-Dateisynchronisierung können mehrere Azure-Dateifreigaben mit einem einzelnen Windows-Dateiserver synchronisiert werden. Dadurch können Sie sicherstellen, dass ältere große Dateifreigaben, über die Sie möglicherweise lokal verfügen, in die Azure-Dateisynchronisierung übertragen werden können. Weitere Informationen finden Sie unter [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-files-planning.md).

## <a name="data-transfer-method"></a>Datenübertragungsmethode
Es gibt viele einfache Optionen, um Daten in einem Massenvorgang aus einer vorhandenen Dateifreigabe (z.B. einer lokalen Dateifreigabe) in Azure Files zu übertragen. Dazu zählen u.a.:

* **Azure-Dateisynchronisierung:** Als Teil einer ersten Synchronisierung zwischen einer Azure-Dateifreigabe (einem „Cloudendpunkt“) und einem Windows-Verzeichnisnamespace (einem „Serverendpunkt“) repliziert die Azure-Dateisynchronisierung alle Daten aus der vorhandenen Dateifreigabe in Azure Files.
* **[Azure Import/Export:](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** Mit dem Azure Import/Export-Dienst können Sie große Datenmengen auf sichere Weise in eine Azure-Dateifreigabe übertragen, indem Sie Festplattenlaufwerke an ein Azure-Rechenzentrum schicken. 
* **[Robocopy:](https://technet.microsoft.com/library/cc733145.aspx)** Robocopy ist ein bekanntes Kopiertool, das in Windows und Windows Server enthalten ist. Robocopy kann zum Übertragen von Daten in Azure Files verwendet werden, indem die Dateifreigabe lokal bereitgestellt wird. Anschließend wird der bereitgestellte Speicherort als Ziel des Robocopy-Befehls verwendet.
* **[AzCopy:](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)** AzCopy ist ein Befehlszeilenprogramm, das zum Kopieren von Daten in und aus Azure Files sowie Azure Blob Storage entwickelt wurde, wobei durch einfache Befehle eine optimale Leistung erzielt wird. AzCopy steht für Windows und Linux zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
* [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
* [Bereitstellen von Azure Files](storage-files-deployment-guide.md)
* [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md)

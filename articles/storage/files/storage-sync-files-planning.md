---
title: Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau) | Microsoft-Dokumentation
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
ms.openlocfilehash: 241b744f5c5e89f53addb4d41d732245d76ef9a3
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)
Verwenden Sie Azure File Sync (Vorschau), um die Dateifreigaben Ihrer Organisation in Azure Files zu zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit Azure File Sync werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen, z.B. SMB, NFS und FTPS. Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel beschreibt wichtige Überlegungen für die Bereitstellung von Azure File Sync. Es wird empfohlen, dass Sie [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md) lesen. 

## <a name="azure-file-sync-terminology"></a>Azure File Sync-Terminologie
Bevor wir in die Details der Planung für eine Azure File Sync-Bereitstellung einsteigen, müssen Sie die Terminologie verstehen.

### <a name="storage-sync-service"></a>Speichersynchronisierungsdienst
Der Speichersynchronisierungsdienst ist die Azure-Ressource der obersten Ebene für Azure File Sync. Die Speichersynchronisierungsdienst-Ressource ist der Speicherkontoressource gleichgeordnet und kann in ähnlicher Weise in Azure-Ressourcengruppen bereitgestellt werden. Eine spezielle Ressource der obersten Ebene der Speicherkontoressource ist erforderlich, da der Speichersynchronisierungsdienst Synchronisierungsbeziehungen mit mehrere Speicherkonten über mehrere Synchronisierungsgruppen erstellen kann. In einem Abonnement können mehrere Speichersynchronisierungsdienst-Ressourcen bereitgestellt werden.

### <a name="sync-group"></a>Synchronisierungsgruppe
Eine Synchronisierungsgruppe definiert die Synchronisierungstopologie für einen Satz von Dateien. Endpunkte innerhalb einer Synchronisierungsgruppe bleiben miteinander synchron. Wenn Sie z.B. über zwei unterschiedliche Sätze von Dateien verfügen, die Sie mit Azure File Sync verwalten möchten, würden Sie zwei Synchronisierungsgruppen erstellen und beiden unterschiedliche Endpunkte hinzufügen. Ein Speichersynchronisierungsdienst kann beliebig viele Synchronisierungsgruppen hosten.  

### <a name="registered-server"></a>Registrierter Server
Das Objekt „Registrierter Server“ stellt eine Vertrauensstellung zwischen Ihrem Server (oder Cluster) und dem Speichersynchronisierungsdienst dar. Sie können beliebig viele Server bei einer Instanz des Speichersynchronisierungsdiensts registrieren. Allerdings kann ein Server (oder Cluster) zu einem beliebigen Zeitpunkt jeweils nur bei einem Speichersynchronisierungsdienst registriert sein.

### <a name="azure-file-sync-agent"></a>Azure-Dateisynchronisierungs-Agent
Der Azure File Sync-Agent ist ein herunterladbares Paket, mit dem ein Windows Server-Computer mit einer Azure-Dateifreigabe synchronisiert werden kann. Der Azure File Sync-Agent besteht aus drei Hauptkomponenten: 
- **FileSyncSvc.exe**: Der Windows-Hintergrunddienst, der für das Überwachen von Änderungen auf Serverendpunkten und das Einleiten von Synchronisierungssitzungen nach Azure zuständig ist.
- **StorageSync.sys**: Der Azure File Sync-Dateisystemfilter, der für das Tiering von Dateien nach Azure Files zuständig ist (wenn Cloudtiering aktiviert ist).
- **PowerShell-Verwaltungs-Cmdlets**: PowerShell-Cmdlets für die Interaktion mit dem Azure-Ressourcenanbieter „Microsoft.StorageSync“. Sie finden diese an folgenden Speicherorten (Standard):
    - C:\Programme\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Programme\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Serverendpunkt
Ein Serverendpunkt stellt einen bestimmten Speicherort auf einem registrierten Server dar, z.B. einen Ordner auf einem Servervolume oder das Stammverzeichnis des Volumes. Mehrere Serverendpunkte können auf dem gleichen Volume vorhanden sein, wenn sich deren Namespaces nicht überschneiden (z.B. „F:\sync1“ und „F:\sync2“). Sie können Richtlinien für das Cloudtiering für jeden Serverendpunkt separat konfigurieren. Wenn Sie einen Serverspeicherort, an dem ein Satz Dateien vorhanden ist, einer Synchronisierungsgruppe als Serverendpunkt hinzufügen, werden diese Dateien mit anderen Dateien, die sich bereits auf anderen Endpunkten in der Synchronisierungsgruppe befinden, zusammengeführt.

### <a name="cloud-endpoint"></a>Cloudendpunkt
Ein Cloudendpunkt ist eine Azure-Dateifreigabe, die Teil einer Synchronisierungsgruppe ist. Die gesamte Azure-Dateifreigabe wird synchronisiert, und eine Azure-Dateifreigabe kann nur einem Cloudendpunkt angehören. Aus diesem Grund kann eine Azure-Dateifreigabe nur einer Synchronisierungsgruppe angehören. Wenn Sie eine Azure-Dateifreigabe, die über einen Satz vorhandener Dateien verfügt, einer Synchronisierungsgruppe als Cloudendpunkt hinzufügen, werden diese Dateien mit anderen Dateien, die sich bereits auf anderen Endpunkten in der Synchronisierungsgruppe befinden, zusammengeführt.

> [!Important]  
> Azure File Sync unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure File Sync-Auftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird nur einmal alle 24 Stunden ausgegeben. Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Cloudtiering 
Cloudtiering ist ein optionales Feature der Azure-Dateisynchronisierung, das ein Tiering von nur selten verwendeten oder aufgerufenen Dateien nach Azure Files ermöglicht. Beim Tiering einer Datei ersetzt der Azure File Sync-Dateisystemfilter (StorageSync.sys) die Datei lokal durch einen Zeiger oder Analysepunkt. Der Analysepunkt stellt eine URL zur Datei in Azure Files dar. Für eine Tieringdatei ist das Attribut „offline“ in NTFS festgelegt, damit Anwendungen von Drittanbietern Tieringdateien identifizieren können. Wenn ein Benutzer eine Tieringdatei öffnet, ruft Azure File Sync nahtlos die Dateidaten aus Azure Files ab, ohne dass der Benutzer wissen muss, dass die Datei nicht lokal auf dem System gespeichert ist. Diese Funktionalität ist auch als hierarchisches Speichermanagement (HSM) bekannt.

## <a name="azure-file-sync-interoperability"></a>Azure File Sync-Interoperabilität 
Dieser Abschnitt behandelt die Interoperabilität von Azure File Sync mit Windows Server-Features und -Rollen sowie Lösungen von Drittanbietern.

### <a name="supported-versions-of-windows-server"></a>Unterstützte Windows Server-Versionen
Aktuell werden die folgenden Versionen von Windows Server von Azure File Sync unterstützt:

| Version | Unterstützte SKUs | Unterstützte Bereitstellungsoptionen |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter und Standard | Vollständig (Server mit einer Benutzeroberfläche) |
| Windows Server 2012 R2 | Datacenter und Standard | Vollständig (Server mit einer Benutzeroberfläche) |

Zukünftige Versionen von Windows Server werden hinzugefügt, sobald sie veröffentlicht werden. Frühere Windows-Versionen werden möglicherweise je nach Benutzerfeedback hinzugefügt.

> [!Important]  
> Es wird empfohlen, alle Servercomputer, die für Azure File Sync verwendet werden, mit den neuesten Updates von Windows Update auf dem neuesten Stand zu halten. 

### <a name="file-system-features"></a>Features des Dateisystems
| Feature | Status der Unterstützung | Hinweise |
|---------|----------------|-------|
| Zugriffssteuerungslisten (ACLs) | Vollständig unterstützt | Windows-Zugriffssteuerungslisten werden von der Azure-Dateisynchronisierung beibehalten und von Windows Server auf Serverendpunkten erzwungen. Windows-Zugriffssteuerungslisten werden (noch) nicht von Azure Files unterstützt, wenn direkt auf Dateien in der Cloud zugegriffen wird. |
| Feste Links | Übersprungen | |
| Symbolische Links | Übersprungen | |
| Bereitstellungspunkte | Teilweise unterstützt | Bereitstellungspunkte können der Stamm eines Serverendpunkts sein, aber sie werden übersprungen, wenn sie im Namespace des Serverendpunkts enthalten sind. |
| Verbindungen | Übersprungen | |
| Analysepunkte | Übersprungen | |
| NTFS-Komprimierung | Vollständig unterstützt | |
| Sparsedateien | Vollständig unterstützt | Sparsedateien werden synchronisiert (werden nicht blockiert), werden jedoch als vollständige Dateien in die Cloud synchronisiert. Wenn sich der Inhalt der Datei in der Cloud (oder auf einem anderen Server) ändert, handelt es sich bei der Datei nicht länger um eine Sparsedatei, sobald die Änderung heruntergeladen wird. |
| Alternative Datenströme (ADS) | Beibehalten, aber nicht synchronisiert | |

> [!Note]  
> Nur NTFS-Volumes werden unterstützt.

### <a name="failover-clustering"></a>Failoverclustering
Windows Server-Failoverclustering wird von der Azure-Dateisynchronisierung für die Bereitstellungsoption „Dateiserver zur allgemeinen Verwendung“ unterstützt. Failoverclustering wird auf einem „Dateiserver mit horizontaler Skalierung für Anwendungsdaten“ (SOFS) oder auf freigegebenen Volumes (CSV) nicht unterstützt.

> [!Note]  
> Der Azure File Sync-Agent muss auf jedem Knoten in einem Failovercluster installiert sein, damit die Synchronisierung ordnungsgemäß funktioniert.

### <a name="data-deduplication"></a>Datendeduplizierung
Für Volumes, bei denen Cloudtiering nicht aktiviert ist, unterstützt Azure File Sync, dass die Windows Server-Datendeduplizierung auf dem Volume aktiviert wird. Derzeit wird Interoperabilität zwischen Azure File Sync mit aktiviertem Cloudtiering und Datendeduplizierung nicht unterstützt.

### <a name="antivirus-solutions"></a>Virenschutzlösungen
Da für den Virenschutz Dateien auf bekannte Schadsoftware überprüft werden müssen, kann ein Virenschutzprodukt den Rückruf von Tieringdateien verursachen. Da für Tieringdateien das Attribut „offline“ festgelegt ist, empfiehlt es sich, beim Softwareanbieter nachzufragen, wie die Lösung so konfiguriert werden kann, dass Offlinedateien nicht gelesen werden. 

Die folgenden Lösungen unterstützen das Überspringen von Offlinedateien:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [McAfee Endpunkt Security](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (Abschnitt „Scan only what you need to“ (Nur die gewünschten Dateien überprüfen) auf Seite 90 der PDF-Datei)
- [Kaspersky Anti-Virus](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Sicherungslösungen
Wie Virenschutzlösungen können auch Sicherungslösungen den Rückruf von Tieringdateien verursachen. Es wird empfohlen, die Azure-Dateifreigabe mithilfe einer Cloudsicherungslösung anstelle eines lokalen Sicherungsprodukts zu sichern.

### <a name="encryption-solutions"></a>Verschlüsselungslösungen
Die Unterstützung von Verschlüsselungslösungen hängt davon ab, wie sie implementiert werden. Die Azure-Dateisynchronisierung funktioniert mit:

- BitLocker-Verschlüsselung
- Azure Rights Management Services (Azure RMS) (und Legacy-Active Directory RMS)

Die Azure-Dateisynchronisierung funktioniert nicht mit:

- NTFS EFS (Encrypted File System)

Im Allgemeinen sollte Azure File Sync Interoperabilität mit Verschlüsselungslösungen unterstützen, die unterhalb des Dateisystems ansetzen, wie etwa BitLocker, sowie mit Lösungen, die im Dateiformat implementiert sind, wie etwa BitLocker. Es wurde keine besondere Interoperabilität für Lösungen implementiert, die oberhalb des Dateisystems ansetzen (wie etwa NTFS EFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Andere Lösungen für hierarchisches Speichermanagement (HSM)
Es sollten keine anderen HSM-Lösungen in Verbindung mit Azure File Sync verwendet werden.

## <a name="region-availability"></a>Regionale Verfügbarkeit
Azure File Sync ist nur in den folgenden Regionen als Vorschau verfügbar:

| Region | Standort des Rechenzentrums |
|--------|---------------------|
| USA (Westen) | Kalifornien, USA |
| Europa, Westen | Niederlande |
| Südostasien | Singapur |
| Australien (Osten) | New South Wales, Australien |

In der Vorschauversion wird nur die Synchronisierung mit einer Azure-Dateifreigabe in der gleichen Region wie der Speichersynchronisierungsdienst unterstützt.

## <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
* [Bereitstellen von Azure Files](storage-files-deployment-guide.md)
* [Bereitstellen von Azure File Sync](storage-sync-files-deployment-guide.md)

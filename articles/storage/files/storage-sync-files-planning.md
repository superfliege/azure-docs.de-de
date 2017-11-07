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
ms.openlocfilehash: d626f71aa21cea562ef6c9554c05e6de027e7f4d
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)
Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

Dieser Leitfaden beschreibt, was Sie beim Bereitstellen der Azure-Dateisynchronisierung berücksichtigen müssen. Es wird empfohlen, dass Sie den Leitfaden [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md) lesen. 

## <a name="understanding-azure-file-sync-terminology"></a>Grundlegendes zur Terminologie der Azure-Dateisynchronisierung
Bevor Sie sich mit den Details der Azure-Dateisynchronisierung beschäftigen, ist es wichtig, die Terminologie zu verstehen.

### <a name="storage-sync-service"></a>Speichersynchronisierungsdienst
Der Speichersynchronisierungsdienst ist die Azure-Ressource der obersten Ebene, die die Azure-Dateisynchronisierung darstellt. Die Speichersynchronisierungsdienst-Ressource ist der Speicherkontoressource gleichgeordnet und kann in ähnlicher Weise in Azure-Ressourcengruppen bereitgestellt werden. Eine spezielle Ressource der obersten Ebene der Speicherkontoressource ist erforderlich, da der Speichersynchronisierungsdienst Synchronisierungsbeziehungen mit mehrere Speicherkonten über mehrere Synchronisierungsgruppen erstellen kann. In einem Abonnement können mehrere Speichersynchronisierungsdienst-Ressourcen bereitgestellt werden.

### <a name="sync-group"></a>Synchronisierungsgruppe
Eine Synchronisierungsgruppe definiert die Synchronisierungstopologie für einen Satz von Dateien. Endpunkte innerhalb einer Synchronisierungsgruppe bleiben miteinander synchron. Wenn Sie z.B. über zwei unterschiedliche Sätze von Dateien verfügen, die Sie mit der Azure-Dateisynchronisierung verwalten möchten, würden Sie zwei Synchronisierungsgruppen erstellen und beiden unterschiedliche Endpunkte hinzufügen. Ein Speichersynchronisierungsdienst kann beliebig viele Synchronisierungsgruppen hosten.  

### <a name="registered-server"></a>Registrierter Server
Das Objekt „Registrierter Server“ stellt eine Vertrauensstellung zwischen Ihrem Server (oder Cluster) und dem Speichersynchronisierungsdienst dar. Sie können beliebig viele Server bei einer Instanz des Speichersynchronisierungsdiensts registrieren. Allerdings kann ein Server (oder Cluster) zu einem beliebigen Zeitpunkt jeweils nur bei einem Speichersynchronisierungsdienst registriert sein.

### <a name="azure-file-sync-agent"></a>Azure-Dateisynchronisierungs-Agent
Der Azure-Dateisynchronisierungs-Agent ist ein herunterladbares Paket, mit dem ein Windows Server-Computer mit einer Azure-Dateifreigabe synchronisiert werden kann. Der Azure-Dateisynchronisierungs-Agent besteht aus drei Hauptkomponenten: 
- **FileSyncSvc.exe**: Der Windows-Hintergrunddienst für das Überwachen von Änderungen auf Serverendpunkten und das Initiieren der Synchronisierungssitzungen in Azure.
- **StorageSync.sys**: Der Azure-Dateisynchronisierungs-Dateisystemfilter für das Tiering von Dateien in Azure Files (wenn Cloudtiering aktiviert ist).
- **PowerShell-Verwaltungs-Cmdlets**: PowerShell-Cmdlets für die Interaktion mit dem Azure-Ressourcenanbieter „Microsoft.StorageSync“. Sie finden sie in den folgenden Speicherorten (Standard):
    - C:\Programme\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Programme\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Serverendpunkt
Ein Serverendpunkt stellt einen bestimmten Speicherort auf einem registrierten Server dar, z.B. einen Ordner auf einem Servervolume oder das Stammverzeichnis des Volumes. Mehrere Serverendpunkte können auf dem gleichen Volume vorhanden sein, wenn sich deren Namespaces nicht überschneiden (z.B. F:\sync1 und F:\sync2). Sie können Richtlinien für das Cloudtiering für jeden Serverendpunkt separat konfigurieren. Wenn Sie einen Serverspeicherort mit einem vorhandenen Satz von Dateien als Serverendpunkt zu einer Synchronisierungsgruppe hinzufügen, werden diese Dateien mit anderen Dateien, die sich bereits auf anderen Endpunkten in der Synchronisierungsgruppe befinden, zusammengeführt.

### <a name="cloud-endpoint"></a>Cloudendpunkt
Ein Cloudendpunkt ist eine Azure-Dateifreigabe, die Teil einer Synchronisierungsgruppe ist. Die gesamte Azure-Dateifreigabe wird synchronisiert, und eine Azure-Dateifreigabe kann nur einem Cloudendpunkt und damit nur einer Synchronisierungsgruppe angehören. Wenn Sie eine Azure-Dateifreigabe mit einem vorhandenen Satz von Dateien als Cloudendpunkt zu einer Synchronisierungsgruppe hinzufügen, werden diese Dateien mit anderen Dateien, die sich bereits auf anderen Endpunkten in der Synchronisierungsgruppe befinden, zusammengeführt.

> [!Important]  
> Azure File Sync unterstützt direkte Änderungen an der Azure Dateifreigabe. Beachten Sie jedoch, dass alle Änderungen, die an der Azure-Dateifreigabe vorgenommen wurden, zuerst von einem Azure File Sync-Änderungserkennungsauftrag ermittelt werden müssen, der nur einmal alle 24 Stunden für einen Cloudendpunkt gestartet wird. Weitere Informationen finden Sie in den [häufig gestellten Fragen zu Azure Files](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Cloudtiering 
Cloudtiering ist ein optionales Feature der Azure-Dateisynchronisierung, das ein Tiering von nur selten verwendeten oder aufgerufenen Dateien in Azure Files ermöglicht. Beim Tiering einer Datei ersetzt der Azure-Dateisynchronisierungs-Dateisystemfilter (StorageSync.sys) die Datei lokal durch einen Zeiger oder Analysepunkt, der eine URL für die Datei in Azure Files darstellt. Für eine Tieringdatei ist das Attribut „offline“ in NTFS festgelegt, damit Anwendungen von Drittanbietern Tieringdateien identifizieren können. Wenn ein Benutzer eine Tieringdatei öffnet, ruft die Azure-Dateisynchronisierung nahtlos die Dateidaten aus Azure Files ab, ohne dass der Benutzer wissen muss, dass die Datei nicht lokal auf dem System gespeichert ist. Diese Funktionalität ist auch als hierarchisches Speichermanagement (HSM) bekannt.

## <a name="azure-file-sync-interoperability"></a>Interoperabilität der Azure-Dateisynchronisierung 
Dieser Abschnitt behandelt die Interoperabilität von Azure File Sync mit Windows Server-Features und -Rollen sowie Lösungen von Drittanbietern.

### <a name="supported-versions-of-windows-server"></a>Unterstützte Windows Server-Versionen
Zum gegenwärtigen Zeitpunkt werden die folgenden Versionen von Windows Server von der Azure-Dateisynchronisierung unterstützt:

| Version | Unterstützte SKUs | Unterstützte Bereitstellungsoptionen |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter und Standard | Vollständig (Server mit einer Benutzeroberfläche) |
| Windows Server 2012 R2 | Datacenter und Standard | Vollständig (Server mit einer Benutzeroberfläche) |

Zukünftige Versionen von Windows Server werden hinzugefügt, sobald sie veröffentlicht werden. Ältere Versionen von Windows können basierend auf Feedbacks der Benutzer hinzugefügt werden.

> [!Important]  
> Es wird empfohlen, alle Windows Server-Computer, die für Azure File Sync verwendet werden, mit den neuesten Updates von Windows Update auf dem neuesten Stand zu halten. 

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
| Sparsedateien | Vollständig unterstützt | Sparsedateien werden synchronisiert (nicht blockiert), werden jedoch als vollständige Dateien in die Cloud synchronisiert. Wenn sich der Inhalt der Datei in der Cloud (oder auf einem anderen Server) ändert, handelt es sich bei der Datei nicht länger um eine Sparsedatei, sobald die Änderung heruntergeladen wird. |
| Alternative Datenströme (ADS) | Beibehalten, aber nicht synchronisiert | |

> [!Note]  
> Nur NTFS-Volumes werden unterstützt.

### <a name="failover-clustering"></a>Failoverclustering
Windows Server-Failoverclustering wird von der Azure-Dateisynchronisierung für die Bereitstellungsoption „Dateiserver zur allgemeinen Verwendung“ unterstützt. Failoverclustering wird auf einem „Dateiserver mit horizontaler Skalierung für Anwendungsdaten“ (SOFS) oder auf freigegebenen Volumes (CSV) nicht unterstützt.

> [!Note]  
> Der Azure-Dateisynchronisierungs-Agent muss auf jedem Knoten in einem Failovercluster installiert sein, damit die Synchronisierung ordnungsgemäß funktioniert.

### <a name="windows-server-data-deduplication"></a>Windows Server-Datendeduplizierung
Für Volumes, bei denen Cloudtiering nicht aktiviert ist, unterstützt die Azure-Dateisynchronisierung, dass die Datendeduplizierung auf dem Volume aktiviert wird. Derzeit wird Interoperabilität zwischen der Azure-Dateisynchronisierung mit aktiviertem Cloudtiering und Datendeduplizierung nicht unterstützt.

### <a name="anti-virus-solutions"></a>Virenschutzlösungen
Da für den Virenschutz Dateien auf bekannte Schadsoftware überprüft werden müssen, kann Virenschutz den Rückruf von Tieringdateien verursachen. Da für Tieringdateien das Attribut „offline“ festgelegt ist, empfiehlt es sich, beim Softwareanbieter nachzufragen, wie die Lösung so konfiguriert werden kann, dass Offlinedateien nicht gelesen werden. 

Die folgenden Lösungen unterstützen das Überspringen von Offlinedateien:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [McAfee Endpunkt Security](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (Abschnitt „Scan only what you need to“ (Nur die gewünschten Dateien überprüfen) auf Seite 90)
- [Kaspersky Anti-Virus](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Sicherungslösungen
Wie Virenschutzlösungen können auch Sicherungslösungen den Rückruf von Tieringdateien verursachen. Es wird empfohlen, mithilfe einer Cloudsicherungslösung die Azure-Dateifreigabe zu sichern, statt ein lokales Sicherungsprodukt zu verwenden.

### <a name="encryption-solutions"></a>Verschlüsselungslösungen
Die Unterstützung von Verschlüsselungslösungen hängt davon ab, wie sie implementiert werden. Die Azure-Dateisynchronisierung funktioniert mit:

- BitLocker-Verschlüsselung
- Azure RMS (und der Legacyversion Active Directory RMS)

Die Azure-Dateisynchronisierung funktioniert nicht mit:

- NTFS EFS (Encrypted File System)

Im Allgemeinen sollte eine Interoperabilität der Azure-Dateisynchronisierung mit Verschlüsselungslösungen gegeben sein, die sich unterhalb des Dateisystems befinden, z.B. BitLocker, sowie mit Lösungen, die in das Dateiformat implementiert sind, z.B. BitLocker. Es wurde jedoch keine spezielle Interoperabilität integriert, damit eine Interoperabilität mit Lösungen möglich ist, die sich oberhalb des Dateisystems befinden (z.B. NTFS EFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Andere Lösungen für hierarchisches Speichermanagement (HSM)
Mit der Azure-Dateisynchronisierung sollten keine anderen Lösungen für hierarchisches Speichermanagement verwendet werden.

## <a name="region-availability"></a>Regionale Verfügbarkeit
Die Azure-Dateisynchronisierung ist nur in den folgenden Regionen als Vorschauversion verfügbar:

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
* [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md)

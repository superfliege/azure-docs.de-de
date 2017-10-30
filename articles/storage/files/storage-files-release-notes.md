---
title: Versionshinweise zum Azure File Sync-Agent | Microsoft-Dokumentation
description: Versionshinweise zu Azure File Sync
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 1db3fef17e24022bff59665558f4a354f8c37d1d
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="azure-file-sync-agent-release-notes"></a>Versionshinweise zum Azure File Sync-Agent
Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

Dieser Artikel enthält die Versionshinweise für unterstützte Versionen des Azure File Sync-Agents.

## <a name="supported-versions"></a>Unterstützte Versionen
Von Azure File Sync werden folgende Versionen unterstützt:

| Agent-Versionsnummer | Herausgabedatum | Ende der Unterstützung |
|----------------------|--------------|------------------|
| 1.1.0.0 | 26.09.2017 | Aktuelle Version |

### <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-1100"></a>Agent-Version 1.1.0.0
Die folgenden Versionshinweise betreffen die Agent-Version 1.1.0.0 vom 9. September 2017. Bei dieser Version handelt es sich um die erste Vorschauversion von Azure File Sync.

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit einem Windows-Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket (MSI) muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Wird nur für Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GB physischen Speicher.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- Verwenden Sie nicht den Ressourcen-Manager für Dateiserver oder andere Dateiprüfungen: Dateiprüfungen können zu endlosen Synchronisierungsfehlern führen, wenn Dateien aufgrund der Dateiprüfung blockiert werden.
- Die Duplizierung eines registrierten Servers (einschließlich Klonen von virtuellen Computern) kann zu unerwarteten Ergebnissen führen (insbesondere dazu, dass die Synchronisierung nicht konvergiert).
- Datendeduplizierung und Cloudtiering werden nicht auf dem gleichen Volume unterstützt.
 
### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, ansonsten ist der Systembetrieb jedoch nicht beeinträchtigt:
- Pfade mit mehr als 2.048 Zeichen
- Der DACL-Teil einer Sicherheitsbeschreibung ab einer Größe von mehr als 2.000. (Dies ist nur ein Problem, wenn Sie über mehr als etwa 40 Zugriffssteuerungseinträge für ein einzelnes Element verfügen.)
- Der SACL-Teil der Sicherheitsbeschreibung (zu Überwachungszwecken)
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) bleibt nicht erhalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die unseren Dienst am Lesen der Daten hindern 
    
    > [!Note]  
    > Azure File Sync verschlüsselt Daten immer während der Übertragung, und ruhende Daten können in Azure verschlüsselt werden.
 
### <a name="server-endpoints"></a>Serverendpunkte
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von Azure File Sync derzeit nicht unterstützt.
- Ein Serverendpunkt darf sich nicht auf dem Systemvolume befinden. („C:\MyFolder“ ist beispielsweise kein zulässiger Pfad – es sei denn, bei „C:\MyFolder“ handelt es sich um einen Bereitstellungspunkt).
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt darf nicht geschachtelt sein, kann sich aber parallel mit anderen Serverendpunkten auf dem gleichen Volume befinden.
- Das gleichzeitige Löschen einer großen Anzahl von Verzeichnissen auf einem Server (mehr als 10.000) kann Synchronisierungsfehler zur Folge haben. Löschen Sie Verzeichnisse in Batches mit weniger als 10.000 Verzeichnissen, und vergewissern Sie sich vor dem Löschen des nächsten Batchs, dass die Löschvorgänge erfolgreich synchronisiert werden.
- Am Stamm eines Volumes nicht unterstützt.
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei innerhalb eines Serverendpunkts.
 
### <a name="cloud-tiering"></a>Cloudtiering
- Um sicherzustellen, dass Dateien ordnungsgemäß abgerufen werden können, wird für neue oder geänderte Dateien unter Umständen bis zu 32 Stunden lang kein Tiering durchgeführt (einschließlich des erstmaligen Tierings nach dem Konfigurieren eines neuen Serverendpunkts). Für bedarfsgerechtes Tiering steht ein PowerShell-Cmdlet zur Verfügung, sodass Sie Tiering effizienter bewerten können, ohne auf den Hintergrundprozess zu warten.
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, handelt es sich bei der Zieldatei nicht um eine Tieringdatei, das Offlineattribut ist aber unter Umständen festgelegt, da Robocopy das Attribut fälschlicherweise in Kopiervorgänge einbezieht.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten möglicherweise so aus, als wäre das Offlineattribut nicht korrekt festgelegt.
---
title: Versionshinweise zum Azure File Sync-Agent (Vorschauversion) | Microsoft-Dokumentation
description: Versionshinweise zum Azure File Sync-Agent (Vorschauversion)
services: storage
author: wmgries
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/12/2018
ms.author: wgries
ms.openlocfilehash: b42287580078b4391ddbc5b8ff2835131c64236d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="release-notes-for-the-azure-file-sync-agent-preview"></a>Versionshinweise zum Azure File Sync-Agent (Vorschauversion)
Mit Azure File Sync können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Ihre Windows Server-Installationen werden in einen schnellen Cache Ihrer Azure-Dateifreigabe transformiert. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen (z.B. SMB, NFS und FTPS). Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält die Versionshinweise für die unterstützten Versionen des Azure File Sync-Agents.

## <a name="supported-versions"></a>Unterstützte Versionen
Für den Azure File Sync-Agent werden die folgenden Versionen unterstützt:

| Meilenstein | Agent-Versionsnummer | Herausgabedatum | Status |
|----|----------------------|--------------|------------------|
| Updaterollup von März | 2.2.0.0 | 12. März 2018 | Unterstützt (empfohlene Version) |
| Updaterollup von Februar | 2.1.0.0 | 28. Februar 2018 | Unterstützt |
| Aktualisierung 1 | 2.0.11.0 | 8. Februar 2018 | Unterstützt |
| Updaterollup von Januar | 1.4.0.0 | 8. Januar 2018 | Ende der Unterstützung: 8. Mai 2018<sup>1</sup> |
| Updaterollup von November | 1.3.0.0 | 30. November 2017 | Ende der Unterstützung: 8. Mai 2018<sup>1</sup> |
| Updaterollup von Oktober | 1.2.0.0 | 31. Oktober 2017 | Ende der Unterstützung: 8. Mai 2018<sup>1</sup> |
| Erste Vorschauversion | 1.1.0.0 | 26. September 2017 | Ende der Unterstützung: 8. Mai 2018<sup>1</sup> |

\[1\]: Versionen des Azure File Sync-Agents während der Vorschau entsprechen absichtlich nicht der Updaterichtlinie. Die Updaterichtlinie wird ab der ersten Version des Agents erzwungen, nachdem Azure File Sync als allgemein verfügbar erklärt wurde.

### <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-2200"></a>Agent-Version 2.2.0.0
Die folgenden Versionshinweise gelten für Version 2.2.0.0 des Azure File Sync-Agents (veröffentlicht am 12. März 2018).  Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für Version 2.1.0.0 und 2.0.11.0 angegeben sind.

Die Installation von Version 2.1.0.0 ist für einige Kunden nicht erfolgreich, da die FileSyncSvc nicht stoppt. Dieses Problem wird mit diesem Update behoben.

## <a name="agent-version-2100"></a>Agent-Version 2.1.0.0
Die folgenden Versionshinweise gelten für Version 2.1.0.0 des Azure File Sync-Agents, der am 28. Februar 2018 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für Version 2.0.11.0 angegeben sind.

Diese Version umfasst die folgenden Änderungen:
- Optimierte Verarbeitung von Clusterfailovern
- Verbesserungen bei der zuverlässigen Verarbeitung von Tieringdateien
- Unterstützung für die Installation des Agents auf Domänencontrollercomputern, die einer Windows Server 2008 R2-Domänenumgebung hinzugefügt werden.
- In dieser Version behoben: Generierung von übermäßig vielen Diagnosedaten auf Servern mit vielen Dateien
- Verbesserungen der Fehlerbehandlung bei Sitzungsfehlern
- Verbesserungen der Fehlerbehandlung bei Problemen mit der Dateiübertragung
- In dieser Version geändert: Das Standardintervall zum Ausführen des Cloudtiering beträgt bei Aktivierung auf einem Serverendpunkt jetzt eine Stunde 
- Problem aufgrund von temporärer Blockierung: Verschiebung von Azure File Sync-Ressourcen (Speichersynchronisierungsdienst) in ein neues Azure-Abonnement

## <a name="agent-version-20110"></a>Agent-Version 2.0.11.0
Die folgenden Versionshinweise gelten für Version 2.0.11.0 des Azure File Sync-Agents (veröffentlicht am 9. Februar 2018). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket (MSI) muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GB physischen Speicher.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- Diese Version verfügt über Unterstützung für DFS-R. Weitere Informationen finden Sie im [Leitfaden zur Planung](storage-sync-files-planning.md#distributed-file-system-dfs).
- Verwenden Sie nicht den Ressourcen-Manager für Dateiserver oder andere Dateiprüfungen. Dateiprüfungen können zu endlosen Synchronisierungsfehlern führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Duplizierung von registrierten Servern (einschließlich VM-Klonen) kann zu unerwarteten Ergebnissen führen. Es kann vor allem passieren, dass Synchronisierungen nicht konvergiert werden können.
- Datendeduplizierung und Cloudtiering auf demselben Volume werden nicht unterstützt.
 
### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern. 
    
    > [!Note]  
    > Bei Azure File Sync werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoints"></a>Serverendpunkte
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von Azure File Sync derzeit nicht unterstützt.
- Ein Serverendpunkt kann sich nicht auf dem Systemvolume befinden. Beispielsweise ist „C:\MyFolder“ nur dann ein akzeptabler Pfad, wenn „C:\MyFolder“ ein Bereitstellungspunkt ist.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Das gleichzeitige Löschen einer großen Zahl von Verzeichnissen (mehr als 10.000) von einem Server kann zu Synchronisierungsfehlern führen. Löschen Sie Verzeichnisse in Gruppen von weniger als 10.000. Stellen Sie sicher, dass die Löschvorgänge erfolgreich synchronisiert werden, bevor Sie die nächste Gruppe löschen.
- Mit dieser Version wird Unterstützung für den Synchronisierungsstamm am Stamm eines Volumes hinzugefügt.
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei, die sich innerhalb eines Serverendpunkts befindet.
- Änderung in dieser Version: Es wurden neue Ereignisse zum Nachverfolgen der Gesamtlaufzeit für Cloudtiering (Ereignis-ID 9016), zum Nachverfolgen des Uploadfortschritts beim Synchronisieren (Ereignis-ID 9302) und zum Nachverfolgen von Dateien hinzugefügt, die nicht synchronisiert wurden (Ereignis-ID 9900).
- Änderung in dieser Version: Die Leistung der schnellen DR-Namespacesynchronisierung wurde erheblich verbessert.
 
### <a name="cloud-tiering"></a>Cloudtiering
- Änderung im Vergleich zur Vorgängerversion: Das Tiering für neue Dateien erfolgt gemäß der Tieringrichtlinieneinstellung innerhalb einer Stunde (vorher: 32 Stunden). Mit dem zur Verfügung gestellten PowerShell-Cmdlet können Sie das Tiering bedarfsgesteuert durchführen. Sie können das Cmdlet verwenden, um das Tiering effizienter zu evaluieren, da Sie nicht auf den Hintergrundprozess warten müssen.
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.
- Änderung im Vergleich zur Vorgängerversion: Dateien werden jetzt als Tieringdateien auf anderen Servern heruntergeladen (vorausgesetzt, die Datei ist neu oder bereits eine Tieringdatei).

## <a name="agent-version-1100"></a>Agent-Version 1.1.0.0
Die folgenden Versionshinweise gelten für Version 1.1.0.0 des Azure File Sync-Agents (veröffentlicht am 9. September 2017, erste Vorschauversion). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket (MSI) muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur für Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GB physischen Speicher.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- Verwenden Sie nicht den Ressourcen-Manager für Dateiserver oder andere Dateiprüfungen. Dateiprüfungen können zu endlosen Synchronisierungsfehlern führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Duplizierung von registrierten Servern (einschließlich VM-Klonen) kann zu unerwarteten Ergebnissen führen. Es kann vor allem passieren, dass Synchronisierungen nicht konvergiert werden können.
- Datendeduplizierung und Cloudtiering auf demselben Volume werden nicht unterstützt.
 
### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil einer Sicherheitsbeschreibung, wenn dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern. 
    
    > [!Note]  
    > Bei Azure File Sync werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoints"></a>Serverendpunkte
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von Azure File Sync derzeit nicht unterstützt.
- Ein Serverendpunkt kann sich nicht auf dem Systemvolume befinden. Beispielsweise ist „C:\MyFolder“ nur dann ein akzeptabler Pfad, wenn „C:\MyFolder“ ein Bereitstellungspunkt ist.
- Das Failoverclustering wird nur mit Clusterdatenträgern und nicht mit CSVs unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Das gleichzeitige Löschen einer großen Zahl von Verzeichnissen (mehr als 10.000) von einem Server kann zu Synchronisierungsfehlern führen. Löschen Sie Verzeichnisse in Gruppen von weniger als 10.000. Stellen Sie sicher, dass die Löschvorgänge erfolgreich synchronisiert werden, bevor Sie die nächste Gruppe löschen.
- Ein Serverendpunkt am Stamm eines Volumes wird derzeit nicht unterstützt.
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei, die sich innerhalb eines Serverendpunkts befindet.
 
### <a name="cloud-tiering"></a>Cloudtiering
- Damit sichergestellt ist, dass Dateien korrekt zurückgerufen werden können, führt das System für neue oder geänderte Dateien ggf. für einen Zeitraum von bis zu 32 Stunden kein automatisches Tiering durch. Dieser Prozess umfasst auch das erstmalige Tiering nach dem Konfigurieren eines neuen Serverendpunkts. Mit dem zur Verfügung gestellten PowerShell-Cmdlet können Sie das Tiering bedarfsgesteuert durchführen. Sie können das Cmdlet verwenden, um das Tiering effizienter zu evaluieren, da Sie nicht auf den Hintergrundprozess warten müssen.
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.
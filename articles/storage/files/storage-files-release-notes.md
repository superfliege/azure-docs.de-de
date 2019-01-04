---
title: Versionshinweise zum Azure-Dateisynchronisierungs-Agent | Microsoft-Dokumentation
description: Versionshinweise zum Azure-Dateisynchronisierungs-Agent
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 12/10/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 5bea4c655e9a8970d8d0d946827cc3e46e7efa7a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255158"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Versionshinweise zum Azure-Dateisynchronisierungs-Agent
Mit der Azure-Dateisynchronisierung können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Ihre Windows Server-Installationen werden in einen schnellen Cache Ihrer Azure-Dateifreigabe transformiert. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen (z.B. SMB, NFS und FTPS). Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält die Versionshinweise für die unterstützten Versionen des Azure-Dateisynchronisierungs-Agents.

## <a name="supported-versions"></a>Unterstützte Versionen
Für den Azure-Dateisynchronisierungs-Agent werden die folgenden Versionen unterstützt:

| Meilenstein | Agent-Versionsnummer | Herausgabedatum | Status |
|----|----------------------|--------------|------------------|
| Updaterollup aus Dezember: [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10. Dezember 2018 | Unterstützt (empfohlene Version) |
| Updaterollup aus Dezember | 4.1.0.0 | 4. Dezember 2018 | Unterstützt |
| Release V4 | 4.0.1.0 | 13. November 2018 | Unterstützt |
| Updaterollup von September | 3.3.0.0 | 24. September 2018 | Unterstützt |
| Updaterollup aus August | 3.2.0.0 | 15. August 2018 | Unterstützt |
| Allgemeine Verfügbarkeit | 3.1.0.0 | 19. Juli 2018 | Unterstützt |
| Updaterollup für Juni | 3.0.13.0 | 29. Juni 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Aktualisieren 2 | 3.0.12.0 | 22. Mai 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Updaterollup von April | 2.3.0.0 | 8. Mai 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Updaterollup von März | 2.2.0.0 | 12. März 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Updaterollup von Februar | 2.1.0.0 | 28. Februar 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Aktualisierung 1 | 2.0.11.0 | 8. Februar 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Updaterollup von Januar | 1.4.0.0 | 8. Januar 2018 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Updaterollup von November | 1.3.0.0 | 30. November 2017 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Updaterollup von Oktober | 1.2.0.0 | 31. Oktober 2017 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |
| Erste Vorschauversion | 1.1.0.0 | 26. September 2017 | Nicht unterstützt – Agent-Version ist am 1. Oktober 2018 abgelaufen |

### <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-4200"></a>Agent-Version 4.2.0.0
Die folgenden Versionshinweise gelten für Version 4.2.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 10. Dezember 2018). Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 4.0.1.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Ein Abbruchfehler vom Typ „0x3B“ oder „0x1E“ kann auftreten, wenn eine VSS-Momentaufnahme erstellt wird.  
- Ein Arbeitsspeicherverlust kann auftreten, wenn Cloudtiering aktiviert ist.  

## <a name="agent-version-4100"></a>Agent-Version 4.1.0.0
Die folgenden Versionshinweise gelten für Version 4.1.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 4. Dezember 2018). Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 4.0.1.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Der Server reagiert ggf. aufgrund eines Cloudtiering-Speicherverlusts nicht mehr.  
- Fehler bei der Agent-Installation: Fehler 1921. Der Dienst „Speichersynchronisierungs-Agent“ (FileSyncSvc) konnte nicht beendet werden.  Überprüfen Sie, ob Sie über ausreichend Rechte verfügen, um Systemdienste zu beenden.  
- Der Speichersynchronisierungs-Agent-Dienst (FileSyncSvc) stürzt möglicherweise ab, wenn die Speicherauslastung hoch ist.  
- Verschiedene Zuverlässigkeitverbesserungen für Cloudtiering und Synchronisierung.

## <a name="agent-version-4010"></a>Agent-Version 4.0.1.0
Die folgenden Versionshinweise gelten für Version 4.0.1.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 13. November 2018).

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein AzureRM-PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder ein nicht unterstütztes Betriebssystem. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.
- Ein Abbruchfehler vom Typ „0x3B“ oder „0x1E“ kann auftreten, wenn eine VSS-Momentaufnahme erstellt wird.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.
- Datendeduplizierung und Cloudtiering auf demselben Volume werden nicht unterstützt.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
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
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert. Heben Sie zur Aktualisierung des Servernamens im Portal die Registrierung des Servers auf, und registrieren Sie ihn neu.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Die Richtlinieneinstellung für das datumsbasierte Cloudtiering wird verwendet, um Dateien anzugeben, die zwischengespeichert werden sollen, wenn innerhalb einer angegebenen Anzahl von Tagen darauf zugegriffen wird. Weitere Informationen finden Sie im Artikel [Übersicht über Cloudtiering](https://docs.microsoft.com/azure/storage/files/storage-sync-cloud-tiering#afs-force-tiering).
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.

## <a name="agent-version-3300"></a>Agent-Version 3.3.0.0
Die folgenden Anmerkungen zu dieser Version gelten für Version 3.3.0.0 des Azure-Dateisynchronisierungs-Agents, die am 24. September 2018 veröffentlicht wurde. Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 3.1.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:
- Status des registrierten Servers ist nach dem Upgrade des Azure-Dateisynchronisierungs-Agents auf Version 3.1 oder 3.2 „Anscheinend offline“.
- Speichersynchronisierungs-Agent-Dienst (FileSyncSvc) stürzt aufgrund von Dateien mit langen Pfaden ab.
- Fehler bei der Serverregistrierung: Datei oder Assembly „Kailani.Afs.StorageSyncProtocol.V3“ konnte nicht geladen werden.

## <a name="agent-version-3200"></a>Agent-Version 3.2.0.0
Die folgenden Anmerkungen zu dieser Version gelten für Version 3.2.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 15. August 2018). Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 3.1.0.0 angegeben sind.

Diese Version umfasst die folgende Fehlerbehebung:
- Fehler bei der Synchronisierung mit Fehler durch ungenügenden Arbeitsspeicher (0x8007000e) aufgrund eines Speicherverlusts

## <a name="agent-version-3100"></a>Agent-Version 3.1.0.0
Die folgenden Versionshinweise gelten für Version 3.1.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 19. Juli 2018).

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein AzureRM-PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder ein nicht unterstütztes Betriebssystem. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GB physischen Speicher.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- Verwenden Sie nicht den Ressourcen-Manager für Dateiserver oder andere Dateiprüfungen. Dateiprüfungen können zu endlosen Synchronisierungsfehlern führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Die Agent-Installation und Serverregistrierung sollte nach der Bereitstellung des Serverimages und nach Abschluss des Mini-Setups für Sysprep erfolgen.
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
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Mehrstufige Dateien werden unbrauchbar, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei, die sich innerhalb eines Serverendpunkts befindet.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert. Heben Sie zur Aktualisierung des Servernamens im Portal die Registrierung des Servers auf, und registrieren Sie ihn neu.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.

---
title: Log Analytics-Datenmodell für Azure Backup
description: In diesem Artikel werden die Details des Log Analytics-Datenmodells für Azure Backup-Daten vorgestellt.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9cdb11bad5d4aa94fdc083a0fc7dc6a2c5787cd
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635151"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics-Datenmodell für Azure Backup-Daten
Verwenden Sie das Log Analytics-Datenmodell zum Erstellen von Berichten. Mit dem Datenmodell können Sie nach Wunsch benutzerdefinierte Abfragen und Dashboards erstellen oder Azure Backup-Daten anpassen.

## <a name="using-azure-backup-data-model"></a>Verwenden des Azure Backup-Datenmodells
Sie können die folgenden im Rahmen des Datenmodells bereitgestellten Felder verwenden, um visuelle Elemente, benutzerdefinierte Abfragen und ein Dashboard gemäß Ihren Anforderungen zu erstellen.

### <a name="alert"></a>Warnung
Diese Tabelle enthält Details zu warnungsbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| AlertUniqueId_s |Text |Eindeutiger Bezeichner der generierten Warnung |
| AlertType_s |Text |Typ der Warnung, z.B. Sicherung |
| AlertStatus_s |Text |Status der Warnung, z.B. „Aktiv“ |
| AlertOccurenceDateTime_s |Datum/Uhrzeit |Datum und Uhrzeit der Warnungserstellung |
| AlertSeverity_s |Text |Schweregrad der Warnung, z.B. „Kritisch“ |
| EventName_s |Text |Name der Veranstaltung. Immer „AzureBackupCentralReport“ |
| BackupItemUniqueId_s |Text |Eindeutiger Bezeichner des Sicherungselements, das der Warnung zugeordnet ist |
| SchemaVersion_s |Text |Aktuelle Version des Schemas, z.B. **V1** |
| State_s |Text |Aktueller Status des Warnungsobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der diese Warnung gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Name des aktuellen Vorgangs, z.B. Warnung |
| Category (Kategorie) |Text |Kategorie der Diagnosedaten, die mithilfe von Push in Log Analytics übertragen werden. Immer „AzureBackupReport“ |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutiger Bezeichner des geschützten Servers, der der Warnung zugeordnet ist |
| VaultUniqueId_s |Text |Eindeutiger Bezeichner des geschützten Tresors, der der Warnung zugeordnet ist |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Eindeutiger Bezeichner der Ressource, zu der Daten gesammelt werden. Beispiel: eine Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="backupitem"></a>BackupItem
Diese Tabelle enthält Details zu Feldern in Bezug auf Sicherungselemente.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Name der Veranstaltung. Immer „AzureBackupCentralReport“ |  
| BackupItemUniqueId_s |Text |Eindeutiger Bezeichner des Sicherungselements |
| BackupItemId_s |Text |Bezeichner des Sicherungselements |
| BackupItemName_s |Text |Name des Sicherungselements |
| BackupItemFriendlyName_s |Text |Anzeigename des Sicherungselements |
| BackupItemType_s |Text |Typ des Sicherungselements, z.B. VM, FileFolder |
| ProtectedServerName_s |Text |Name des geschützten Servers, zu dem das Sicherungselement gehört |
| ProtectionState_s |Text |Aktueller Schutzstatus des Sicherungselements, z.B. „Geschützt“, „Schutz beendet“ |
| SchemaVersion_s |Text |Version des Schemas, z.B. **V1** |
| State_s |Text |Status des Sicherungselementobjekts, z.B. „Aktiv“ oder „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieses Sicherungselement gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Name des Vorgangs, z.B. „BackupItem“ |
| Category (Kategorie) |Text |Kategorie der Diagnosedaten, die mithilfe von Push in Log Analytics übertragen werden. Immer „AzureBackupReport“ |
| Ressource |Text |Ressource, für die Daten erfasst werden; z.B. der Name des Recovery Services-Tresors |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcen-ID, für die Daten erfasst werden; z.B. die Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor) für Daten, die erfasst werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor) für Daten, die erfasst werden |
| ResourceProvider |Text |Ressourcenanbieter für Daten, die erfasst werden; z.B. „Microsoft.RecoveryServices“ |
| ResourceType |Text |Typ der Ressource für Daten, die erfasst werden; z.B. Tresore |

### <a name="backupitemassociation"></a>BackupItemAssociation
Diese Tabelle enthält Details zur Zuordnung von Sicherungselementen zu verschiedenen Entitäten.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |  
| BackupItemUniqueId_s |Text |Eindeutige ID des Sicherungselements |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Zuordnungsobjekts für das Sicherungselement, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: BackupItemAssociation |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| PolicyUniqueId_g |Text |Eindeutiger Bezeichner der Richtlinie, die dem Sicherungselement zugeordnet ist |
| ProtectedServerUniqueId_s |Text |Eindeutiger Bezeichner des geschützten Servers, der dem Sicherungselement zugeordnet ist |
| VaultUniqueId_s |Text |Eindeutiger Bezeichner des Tresors, der das Sicherungselement enthält |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), für die Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), für die Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter für Daten, die erfasst werden; z.B. „Microsoft.RecoveryServices“ |
| ResourceType |Text |Typ der Ressource für Daten, die erfasst werden; z.B. Tresore |

### <a name="job"></a>Auftrag
Diese Tabelle enthält Details zu auftragsbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Name der Veranstaltung. Immer „AzureBackupCentralReport“ |
| BackupItemUniqueId_s |Text |Eindeutiger Bezeichner des Sicherungselements |
| SchemaVersion_s |Text |Version des Schemas, z.B. **V1** |
| State_s |Text |Aktueller Status des Auftragsobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Auftrag |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutiger Bezeichner des geschützten Servers, der dem Job zugeordnet ist |
| VaultUniqueId_s |Text |Eindeutiger Bezeichner des geschützten Tresors |
| JobOperation_s |Text |Vorgang, für den Auftrag ausgeführt wird, z.B. Sicherung, Wiederherstellung, Sicherungskonfiguration |
| JobStatus_s |Text |Status des beendeten Auftrags, z.B. „Abgeschlossen“, „Fehler“ |
| JobFailureCode_s |Text |Zeichenfolge mit dem Fehlercode zum Angeben des Grunds des Auftragsfehlers |
| JobStartDateTime_s |Datum/Uhrzeit |Datum und Uhrzeit des Starts des Auftrags |
| BackupStorageDestination_s |Text |Ziel des Sicherungsspeichers, z.B. Cloud, Datenträger  |
| JobDurationInSecs_s | Number |Gesamtdauer des Auftrags in Sekunden |
| DataTransferredInMB_s | Number |Für diesen Auftrag übertragene Daten in MB|
| JobUniqueId_g |Text |Eindeutige ID zur Bezeichnung des Auftrags |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors|
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="policy"></a>Richtlinie
Diese Tabelle enthält Details zu richtlinienbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Richtlinienobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Richtlinie |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| PolicyUniqueId_g |Text |Eindeutige ID zur Bezeichnung der Richtlinie |
| PolicyName_s |Text |Name der definierten Richtlinie |
| BackupFrequency_s |Text |Häufigkeit, mit der Sicherungen erfolgen, z.B. täglich, wöchentlich |
| BackupTimes_s |Text |Datum und Uhrzeit geplanter Sicherungen |
| BackupDaysOfTheWeek_s |Text |Tag der Woche, für den Sicherungen geplant sind |
| RetentionDuration_s |Ganze Zahl |Beibehaltungsdauer für konfigurierte Sicherungen |
| DailyRetentionDuration_s |Ganze Zahl |Gesamte Beibehaltungsdauer in Tagen für konfigurierte Sicherungen |
| DailyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der täglichen Beibehaltung |
| WeeklyRetentionDuration_s |Dezimalzahl |Gesamte wöchentliche Beibehaltungsdauer in Wochen für konfigurierte Sicherungen |
| WeeklyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der wöchentlichen Beibehaltung |
| WeeklyRetentionDaysOfTheWeek_s |Text |Tage der Woche, die für die wöchentliche Beibehaltung ausgewählt sind |
| MonthlyRetentionDuration_s |Dezimalzahl |Gesamte Beibehaltungsdauer in Monaten für konfigurierte Sicherungen |
| MonthlyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der monatlichen Beibehaltung |
| MonthlyRetentionFormat_s |Text |Typ der Konfiguration für die monatliche Beibehaltung, z.B. „Täglich“ für tagesbasiert, „Wöchentlich“ für wochenbasiert |
| MonthlyRetentionDaysOfTheWeek_s |Text |Tage der Woche, die für die monatliche Beibehaltung ausgewählt sind |
| MonthlyRetentionWeeksOfTheMonth_s |Text |Wochen des Monats, in denen die monatliche Beibehaltung konfiguriert ist, z.B. Erste, Letzte usw. |
| YearlyRetentionDuration_s |Dezimalzahl |Gesamte Beibehaltungsdauer in Jahren für konfigurierte Sicherungen |
| YearlyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der jährlichen Beibehaltung |
| YearlyRetentionMonthsOfTheYear_s |Text |Monate des Jahres, die für die jährliche Beibehaltung ausgewählt sind |
| YearlyRetentionFormat_s |Text |Typ der Konfiguration für die jährliche Beibehaltung, z.B. „Täglich“ für tagesbasiert, „Wöchentlich“ für wochenbasiert |
| YearlyRetentionDaysOfTheMonth_s |Text |Datumsangaben des Monats, die für die jährliche Beibehaltung ausgewählt sind |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="policyassociation"></a>PolicyAssociation
Diese Tabelle enthält Details zur Zuordnung von Richtlinien zu verschiedenen Entitäten.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Richtlinienobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: PolicyAssociation |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| PolicyUniqueId_g |Text |Eindeutige ID zur Bezeichnung der Richtlinie |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, zu dem diese Richtlinie gehört |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="protectedserver"></a>ProtectedServer
Diese Tabelle enthält Details zu Feldern in Bezug auf geschützte Server.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| ProtectedServerName_s |Text |Name des geschützten Servers |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Objekts des geschützten Servers, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: ProtectedServer |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers |
| RegisteredContainerId_s |Text |ID des Containers, der für die Sicherung registriert ist |
| ProtectedServerType_s |Text |Typ des geschützten Servers, z.B. Windows |
| ProtectedServerFriendlyName_s |Text |Anzeigename des geschützten Servers |
| AzureBackupAgentVersion_s |Text |Nummer der Backup-Agent-Version |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Diese Tabelle enthält Details zur Zuordnung von geschützten Servern zu anderen Entitäten.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Objekts der Zuordnung für den geschützten Server, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: ProtectedServerAssociation |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, zu dem dieser geschützte Server gehört |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="storage"></a>Speicher
Diese Tabelle enthält Details zu speicherbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| CloudStorageInBytes_s |Dezimalzahl |Von Sicherungen belegter Sicherungsspeicher in der Cloud, berechnet anhand des letzten Werts |
| ProtectedInstances_s |Dezimalzahl |Anzahl der geschützten Instanzen, die zum Berechnen von Front-End-Speicher in der Abrechnung verwendet werden, berechnet anhand des letzten Werts |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Speicherobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für den Server zur Durchführung des Sicherungsjobs, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Speicher |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers, für den der Speicher berechnet wird |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, für den der Speicher berechnet wird |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

### <a name="vault"></a>Tresor
Diese Tabelle enthält Details zu tresorbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Tresorobjekts, z.B. „Aktiv“, „Gelöscht“ |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Tresor |
| Category (Kategorie) |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors |
| VaultName_s |Text |Name des Tresors |
| AzureDataCenter_s |Text |Rechenzentrum, in dem sich der Tresor befindet |
| StorageReplicationType_s |Text |Typ der Speicherreplikation für den Tresor, z.B. GeoRedundant |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Ressourcenbezeichner der Daten, die erfasst werden. Beispiel: Ressourcen-ID des Recovery Services-Tresors |
| SubscriptionId |Text |Abonnementbezeichner der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceGroup |Text |Ressourcengruppe der Ressource (z.B. Recovery Services-Tresor), zu der Daten gesammelt werden |
| ResourceProvider |Text |Ressourcenanbieter, für den Daten gesammelt werden. Beispiel: Microsoft.RecoveryServices |
| ResourceType |Text |Ressourcentyp, für den Daten gesammelt werden. Beispiel: Tresore |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das Datenmodell zum Erstellen von Azure Backup-Berichten überprüft haben, können Sie mit dem [Erstellen von Dashboards](../azure-monitor/platform/dashboards.md) in Log Analytics beginnen.

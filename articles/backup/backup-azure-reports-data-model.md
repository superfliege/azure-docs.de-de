---
title: "Datenmodell für Azure Backup"
description: "In diesem Artikel dreht Details für die Power BI Modells für Azure Backup-Berichte."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 0767c330-690d-474d-85a6-aa8ddc410bb2
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/26/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: efecbc9f1c410744f49795889c4ec3cc618f07e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="data-model-for-azure-backup-reports"></a>Datenmodell für Azure Backup-Berichte
Dieser Artikel beschreibt die Power BI-Datenmodell, die zum Erstellen von Azure Backup-Berichten verwendet. Mit diesem Datenmodell, können Sie vorhandene Berichte basierend auf den entsprechenden Feldern filtern und weitere einhergehen, eigene Berichte erstellen, mithilfe von Tabellen und Felder im Modell. 

## <a name="creating-new-reports-in-power-bi"></a>Erstellen neue Berichte in Power BI
Power BI bietet Funktionen zur Anpassung können Sie mit [Erstellen von Berichten mithilfe des Datenmodells](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Mithilfe von Azure Backup-Datenmodell
Sie können die folgenden Felder, die als Teil des Datenmodells bereitgestellten Berichte zu erstellen und Anpassen vorhandener Berichte.

### <a name="alert"></a>Warnung
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene verwandte Warnungsfelder an.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Ganze Zahl |Anzahl der Warnungen, die im ausgewählten Zeitraum erstellt |
| % ActiveAlertsCreatedInPeriod |Prozentsatz |Prozentsatz der aktiven Warnungen im ausgewählten Zeitraum |
| % CriticalAlertsCreatedInPeriod |Prozentsatz |Prozentsatz der kritische Warnungen im ausgewählten Zeitraum |
| AlertOccurenceDate |Datum |Datum der Erstellung der Warnung |
| AlertSeverity |Text |Schweregrad der Warnung z. B. kritisch |
| AlertStatus |Text |Status der Warnung z. B. aktiv |
| AlertType |Text |Typ der generierten Warnung z. B. Sicherung |
| AlertUniqueId |Text |Eindeutige Id der generierten Warnung |
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Dezimalzahl |Dies ist die durchschnittliche Zeit (in Minuten) zum Beheben der Warnung für ausgewählten Zeitraum |
| EntityState |Text |Aktuellen Status der Warnung Objekts z. B. aktiv, gelöschte |

### <a name="backup-item"></a>Sichern des Elements
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene backup Element bezogenen Felder.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #BackupItems |Ganze Zahl |Anzahl der Elemente für die Sicherung |
| #UnprotectedBackupItems |Ganze Zahl |Anzahl der Elemente für die Sicherung für den Schutz beendet oder für Sicherungen jedoch nicht gestartet Sicherungen konfiguriert|
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| BackupItemFriendlyName |Text |Anzeigename der backup-Element |
| BackupItemId |Text |ID der backup-Element |
| BackupItemName |Text |Name der backup-Element |
| BackupItemType |Text |Backup Elementtyp z. B. VM, FileFolder |
| EntityState |Text |Aktuellen Status des Objekts Sichern des Elements z. B. aktiv, gelöschte |
| LastBackupDateTime |Datum/Uhrzeit |Zeitpunkt der letzten Sicherung für Sichern des ausgewählten Elements |
| LastBackupState |Text |Status der letzten Sicherung für das Sichern des ausgewählten Elements beispielsweise erfolgreich, Fehler |
| LastSuccessfulBackupDateTime |Datum/Uhrzeit |Zeitpunkt der letzten erfolgreichen Sicherung Sichern des ausgewählten Elements |
| ProtectionState |Text |Aktuelle Schutzstatus des backup Elements z. B. Protected, ProtectionStopped |

### <a name="calendar"></a>Kalender
Diese Tabelle enthält Details zu den Feldern kalenderbezogener.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| Datum |Datum |Datum, die zum Filtern von Daten ausgewählt |
| DateKey |Text |Der eindeutige Schlüssel für jedes Datum-Element |
| DayDiff |Dezimalzahl |Differenz an Tag zum Filtern von Daten, z. B., 0 gibt an, Daten des aktuellen Tages, 1, wenn Daten von einem des vorherigen Tages, 0 und-1 anzugeben, die Daten für den aktuellen und dem vorherigen Tag  |
| Monat |Text |Monat des Jahres, der zum Filtern von Daten ausgewählt und Monat am ersten Tag beginnt und endet am 31. Tag |
| MonthDate | Datum |Datum des Monats, wenn Monat beendet wird, zum Filtern von Daten ausgewählt |
| MonthDiff |Dezimalzahl |Unterschied im Monat zum Filtern von Daten, z. B., 0 gibt an, Daten des aktuellen Monats, 1, wenn Daten des vorherigen Monats, 0 und-1 anzugeben, die Daten für den aktuellen und dem vorherigen Monat |
| Woche |Text |Zum Filtern von Daten, ausgewählte Woche die Woche am Sonntag beginnt, und endet am Samstag |
| WeekDate |Datum |Datum in der Woche aus, wenn Woche beendet wird, zum Filtern von Daten ausgewählt |
| WeekDiff |Dezimalzahl |Unterschied in der Woche zum Filtern von Daten, z. B., 0 gibt Daten der aktuellen Woche, 1, wenn Daten der vorherigen Woche, 0 und-1 Daten anzugeben, für die aktuellen und dem vorherigen Woche |
| Jahr |Text |Kalenderjahr, die zum Filtern von Daten ausgewählt |
| YearDate |Datum |Datum des Jahres, wenn Jahr endet, zum Filtern von Daten ausgewählt |

### <a name="job"></a>Job
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene auftragsbezogene Felder.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #JobsCreatedInPeriod |Ganze Zahl |Anzahl der Aufträge, die in den ausgewählten Zeitraum erstellt |
| % FailuresForJobsCreatedInPeriod |Prozentsatz |Prozentsatz insgesamt schon in den ausgewählten Zeitraum |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Dezimalzahl |80. Perzentil-Wert, der Daten in MB für die übertragenen **backup** in den ausgewählten Zeitraum erstellten Aufträge |
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Dezimalzahl |Durchschnittliche Zeit in Minuten für **abgeschlossene Sicherung** im ausgewählten Zeitraum erstellten Aufträge |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Dezimalzahl |Durchschnittliche Zeit in Minuten für **Wiederherstellung abgeschlossen** im ausgewählten Zeitraum erstellten Aufträge |
| BackupStorageDestination |Text |Ziel des backup-Storage-z. B. Cloud, Datenträger  |
| EntityState |Text |Aktuellen Status der Job-Objekt, z. B. aktiv, gelöschte |
| JobFailureCode |Text |Fehler-Codezeichenfolge aufgrund der Auftragsfehler aufgetreten ist |
| JobOperation |Text |Vorgang für den Auftrag z. B. Sicherung, Wiederherstellung, Konfigurieren der Sicherung ausgeführt wird |
| JobStartDate |Datum |Datum der Auftrag gestartet |
| "Jobstarttime" |Zeit |Zeit bei der Ausführung Auftrags gestartet wurde |
| JobStatus |Text |Status des Auftrags nicht mehr benötigen, z. B. abgeschlossen, fehlgeschlagen |
| JobUniqueId |Text |Eindeutige Id für den Auftrag zu identifizieren |

### <a name="policy"></a>Richtlinie
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene Felder im Zusammenhang mit Gruppenrichtlinien.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #Policies |Ganze Zahl |Anzahl von Sicherungsrichtlinien, die im System vorhanden sind. |
| #PoliciesInUse |Ganze Zahl |Anzahl der Richtlinien, die derzeit für das Konfigurieren von Sicherungen verwendet wird |
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| BackupDaysOfTheWeek |Text |Wenn Sicherungen geplanten Wochentage |
| BackupFrequency |Text |Häufigkeit, mit dem Sicherungen, z. B. täglich, wöchentlich ausgeführt werden |
| BackupTimes |Text |Datum und Uhrzeit, wann die Sicherungen geplant sind |
| DailyRetentionDuration |Ganze Zahl |Insgesamt Beibehaltungsdauer in Tagen für die konfigurierten Sicherungen |
| DailyRetentionTimes |Text |Datum und Uhrzeit, wann die tägliche Beibehaltung konfiguriert wurde |
| EntityState |Text |Aktuellen Status des Objekts Richtlinie z. B. aktiv, gelöschte |
| MonthlyRetentionDaysOfTheMonth |Text |Datumsangaben des Monats für die monatliche Beibehaltung ausgewählt |
| MonthlyRetentionDaysOfTheWeek |Text |Die Wochentage für die monatliche Beibehaltung ausgewählt |
| MonthlyRetentionDuration |Dezimalzahl |Insgesamt Beibehaltungsdauer in Monaten für konfigurierten Sicherungen |
| MonthlyRetentionFormat |Text |Geben Sie der Konfiguration für die monatliche Beibehaltung z. B. täglich für Tag basieren, wöchentlich, für die Woche basierend |
| MonthlyRetentionTimes |Text |Datum und Uhrzeit, wann die monatliche Beibehaltung konfiguriert ist |
| MonthlyRetentionWeeksOfTheMonth |Text |Wochen des Monats, wenn die monatliche Beibehaltung ist, konfiguriert, z. B. First, Last usw.. |
| Richtlinienname |Text |Name der Richtlinie definiert |
| PolicyUniqueId |Text |Eindeutige Id zum Identifizieren der Richtlinie |
| RetentionType |Text |Geben Sie der Aufbewahrungsrichtlinie z. B., täglich, wöchentlich, monatlich, jährlich |
| WeeklyRetentionDaysOfTheWeek |Text |Die Wochentage für die wöchentliche Beibehaltung ausgewählt |
| WeeklyRetentionDuration |Dezimalzahl |Wöchentliche Beibehaltung-Gesamtdauer in Wochen für den konfigurierten Sicherungen |
| WeeklyRetentionTimes |Text |Datum und Uhrzeit, wann die wöchentliche Beibehaltung konfiguriert ist |
| YearlyRetentionDaysOfTheMonth |Text |Datumsangaben des Monats für die jährliche Beibehaltung ausgewählt |
| YearlyRetentionDaysOfTheWeek |Text |Die Wochentage für die jährliche Beibehaltung ausgewählt |
| YearlyRetentionDuration |Dezimalzahl |Insgesamt Beibehaltungsdauer in Jahren für den konfigurierten Sicherungen |
| YearlyRetentionFormat |Text |Geben Sie der Konfiguration für die jährliche Beibehaltung z. B. täglich für Tag basieren, wöchentlich, für die Woche basierend |
| YearlyRetentionMonthsOfTheYear |Text |Monate des Jahres für die jährliche Beibehaltung ausgewählt |
| YearlyRetentionTimes |Text |Datum und Uhrzeit, wann die jährliche Beibehaltung konfiguriert ist |
| YearlyRetentionWeeksOfTheMonth |Text |Wochen des Monats, wenn die jährliche Beibehaltung ist, konfiguriert, z. B. First, Last usw.. |

### <a name="protected-server"></a>Geschützte Server
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene der geschützten serverbezogene Felder.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #ProtectedServers |Ganze Zahl |Anzahl von geschützten Servern |
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| AzureBackupAgentOSType |Text |Der Typ des Azure Backup-Agents |
| AzureBackupAgentOSVersion |Text |BS-Version von Azure Backup-Agents |
| AzureBackupAgentUpdateDate |Text |Datum, Agent-Backup-Agent aktualisiert wurde |
| AzureBackupAgentVersion |Text |Versionsnummer der Backup-Agent |
| BackupManagementType |Text |Anbietertyp für das Backup z. B. IaaSVM, FileFolder |
| EntityState |Text |Aktuellen Status des Objekts geschützten Server z. B. aktiv, gelöschte |
| ProtectedServerFriendlyName |Text |Anzeigename des geschützten Servers |
| ProtectedServerName |Text |Name des geschützten Servers |
| ProtectedServerType |Text |Typ des geschützten Servers gesichert wird, wird z. B. IaaSVMContainer |
| ProtectedServerName |Text |Name der geschützten Server zum Sichern des Elements gehört |
| RegisteredContainerId |Text |ID des Containers für die Sicherung registriert |

### <a name="storage"></a>Speicher
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene speicherbezogene Felder.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #ProtectedInstances |Dezimalzahl |Anzahl der geschützten Instanzen, die zum Berechnen von Front-End-Speicher in Abrechnung, berechneten basierend auf neueste Wert in die ausgewählte Zeit |
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| CloudStorageInMB |Dezimalzahl |Backup-Sicherungen, berechnet verwendete Cloud-Speicher basierend auf neueste Wert in die ausgewählte Zeit |
| EntityState |Text |Aktuellen Status des Objekts, z. B. aktiv, gelöschte |
| LastUpdatedDate |Datum |Datum, die ausgewählte Zeile zuletzt aktualisiert wurde |

### <a name="time"></a>Zeit
Diese Tabelle enthält Details zu den Feldern zeitbezogene.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| Stunde |Zeit |Stunde des Tages z. B. 13:00:00 Uhr |
| HourNumber |Dezimalzahl |Anzahl der Stunde des Tages z. B. 13.00 |
| Minute |Dezimalzahl |Minute einer Stunde |
| PeriodOfTheDay |Text |Zeitraum Zeitfenster des Tages z. B. 12-3-Uhr |
| Zeit |Zeit |Die Tageszeit beispielsweise 12:00:01 Uhr |
| TimeKey |Text |Schlüsselwert zum Zeitpunkt darstellen. |

### <a name="vault"></a>Tresor
Diese Tabelle enthält grundlegende Felder und Aggregationen über verschiedene Tresor-bezogene Felder.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| #Vaults |Ganze Zahl |Anzahl von Tresoren |
| AsOnDateTime |Datum/Uhrzeit |Letzte Aktualisierungszeit für die ausgewählte Zeile |
| AzureDataCenter |Text |Rechenzentrum, in dem Tresor befindet. |
| EntityState |Text |Aktuellen Status des Tresor-Objekts, z. B. aktiv, gelöschte |
| StorageReplicationType |Text |Typ der Speicherreplikation für den Tresor z. B. GeoRedundant |
| "SubscriptionId" |Text |Abonnement-Id des Kunden, die zur Erstellung von Berichten ausgewählt |
| Tresorname |Text |Name des Tresors |
| VaultTags |Text |RFID-Transponder, die den Tresor zugeordnet sind |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das Datenmodell zum Erstellen von Azure Backup-Berichte überprüfen, finden Sie in den folgenden Artikeln, ausführliche Informationen zum Erstellen und Anzeigen von Berichten in Power BI.

* [Erstellen von Berichten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filtern von Berichten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)

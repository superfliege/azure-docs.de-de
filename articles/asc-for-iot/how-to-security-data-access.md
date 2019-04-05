---
title: Zugreifen auf Daten unter Verwendung von ASC für IoT (Vorschauversion) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie bei Verwendung von ASC für IoT auf Ihre Sicherheitswarnungen und Empfehlungsdaten zugreifen.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: e394f6025f7898aad7dde7b1acefd9f95029a554
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541506"
---
# <a name="access-your-security-data"></a>Zugreifen auf Sicherheitsdaten 

> [!IMPORTANT]
> ASC für IoT befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

ASC für IoT speichert Sicherheitswarnungen, Empfehlungen und Sicherheitsrohdaten (falls Sie diese speichern möchten) in Ihrem Log Analytics-Arbeitsbereich.

## <a name="log-analytics"></a>Log Analytics

So konfigurieren Sie, wie der Log Analytics-Arbeitsbereich verwendet wird:

1. Öffnen Sie Ihren IoT Hub.
1. Klicken Sie auf **Sicherheit**.
2. Klicken Sie auf **Einstellungen**, und ändern Sie die Konfiguration für Ihren Log Analytics-Arbeitsbereich.

So greifen Sie nach der Konfiguration auf Ihren Log Analytics-Arbeitsbereich zu:

1. Wählen Sie eine Warnung in ASC für IoT aus. 
2. Klicken Sie auf **Weitere Untersuchungen**, und dann auf **Klicken Sie hier, und zeigen Sie die Spalte „DeviceId“ an, um die Geräte mit dieser Warnung anzuzeigen**.

Ausführliche Informationen zum Abfragen von Daten aus Log Analytics finden Sie unter [Erste Schritte mit Abfragen in Log Analytics](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Sicherheitswarnungen

Sicherheitswarnungen werden in der Tabelle **ASCforIoT.SecurityAlert** in Ihrem konfigurierten Log Analytics-Arbeitsbereich gespeichert.

Verwenden Sie die folgenden grundlegenden kql-Abfragen, um mit der Untersuchung von Sicherheitswarnungen zu beginnen.

### <a name="sample-records-query"></a>Musterdatensätze abfragen

So wählen Sie einige zufällige Datensätze aus: 

```
// Select a few random records
//
SecurityAlert
| project 
    TimeGenerated, 
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity, 
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

#### <a name="sample-query-results"></a>Ergebnisse der Beispielabfrage 

| TimeGenerated           | IoTHubId                                                                                                       | deviceId      | AlertSeverity | DisplayName                           | BESCHREIBUNG                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Hoch          | Erfolgreicher Brute-Force-Angriff           | Ein Brute-Force-Angriff auf dem Gerät war erfolgreich        |    { "Vollständige Quelladresse": "[\"10.165.12.18:\"]", "Benutzernamen": "[\"\"]", "DeviceId": "IoT-Device-Linux" }                                                                       |
| 2018-11-19T12:40:31.000 | /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Hoch          | Erfolgreiche lokale Anmeldung auf dem Gerät      | Eine erfolgreiche lokale Anmeldung auf dem Gerät wurde erkannt     | { "Remoteadresse": "?", "Remoteport": "", "Lokaler Port": "", "Anmeldeshell": "/bin/su", "Anmeldeprozess-ID": "28207", "Benutzername": "Angreifer", "DeviceId": "IoT-Device-Linux" } |
| 2018-11-19T12:40:31.000 | /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Hoch          | Fehler bei lokalem Anmeldeversuch auf dem Gerät  | Ein fehlgeschlagener Anmeldeversuch auf dem Gerät wurde erkannt |  { "Remoteadresse": "?", "Remoteport": "", "Lokaler Port": "", "Anmeldeshell": "/bin/su", "Anmeldeprozess-ID": "22644", "Benutzername": "Angreifer", "DeviceId": "IoT-Device-Linux" } |

### <a name="device-summary-query"></a>Abfrage der Gerätezusammenfassung

Verwenden Sie diese kql-Abfrage, um eine Reihe von unterschiedlichen in der letzten Woche erkannten Sicherheitswarnungen nach IoT Hub, Gerät, Schweregrad der Warnung, Warnungstyp auszuwählen.

```
// Select number of distinct security alerts detected last week by 
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

#### <a name="device-summary-query-results"></a>Ergebnisse der Abfrage der Gerätezusammenfassung

| IoTHubId | deviceId| AlertSeverity| DisplayName | Count |
|----------|---------|------------------|---------|---------|
|/subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Hoch          | Erfolgreicher Brute-Force-Angriff           | 9   |    
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Mittel        | Fehler bei lokalem Anmeldeversuch auf dem Gerät  | 242 |    
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Hoch          | Erfolgreiche lokale Anmeldung auf dem Gerät      | 31  |
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | <Gerätename> | Mittel        | Crypto Coin Miner                     | 4   |
|

### <a name="iot-hub-summary"></a>IoT Hub-Zusammenfassung

Verwenden Sie diese kql-Abfrage, um eine Reihe von unterschiedlichen Geräten, für die in der letzten Woche Warnungen aufgetreten sind, nach IoT Hub, Schweregrad der Warnung, Warnungstyp auszuwählen:

```
// Select number of distinct devices which had alerts in the last week, by 
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId, 
    AlertSeverity,
    DisplayName
```

#### <a name="iot-hub-summary-query-results"></a>Ergebnisse der Abfrage der IoT Hub-Zusammenfassung

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | Hoch          | Erfolgreicher Brute-Force-Angriff           | 1          |    
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | Mittel        | Fehler bei lokalem Anmeldeversuch auf dem Gerät  | 1          | 
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | Hoch          | Erfolgreiche lokale Anmeldung auf dem Gerät      | 1          |
| /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppe>/providers/Microsoft.Devices/IotHubs/<IoT_Hub> | Mittel        | Crypto Coin Miner                     | 1          |



## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht](overview.md) über ASC für IoT.
- Informieren Sie sich über die [Architektur](architecture.md) von ASC für IoT.
- Grundlegende und weiterführende Informationen zu [ASC für IoT-Warnungen](concept-security-alerts.md)

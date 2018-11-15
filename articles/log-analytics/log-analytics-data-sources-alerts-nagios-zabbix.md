---
title: Erfassen von Nagios- und Zabbix-Warnungen in Log Analytics | Microsoft-Dokumentation
description: Nagios und Zabbix sind Open-Source-Überwachungstools. Sie können Warnungen von diesen Tools in Log Analytics erfassen, um diese zusammen mit Warnungen aus anderen Quellen zu analysieren.  In diesem Artikel wird beschrieben, wie der Log Analytics-Agent für Linux für die Erfassung von Warnungen von diesen Systemen konfiguriert wird.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 6050771ee1d129edd4370079c07da721f60d2065
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613544"
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-log-analytics-agent-for-linux"></a>Erfassen von Warnungen von Nagios und Zabbix in Log Analytics durch den Log Analytics-Agent für Linux 
[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]
[Nagios](https://www.nagios.org/) und [Zabbix](http://www.zabbix.com/) sind Open-Source Überwachungstools. Sie können Warnungen in Log Analytics über diese Tools erfassen, um sie zusammen mit [Warnungen aus anderen Quellen](../monitoring-and-diagnostics/monitoring-overview-alerts.md) zu analysieren.  In diesem Artikel wird beschrieben, wie der Log Analytics-Agent für Linux für die Erfassung von Warnungen von diesen Systemen konfiguriert wird.
 
## <a name="prerequisites"></a>Voraussetzungen
Der Log Analytics-Agent für Linux unterstützt das Erfassen von Warnungen von Nagios bis Version 4.2.x und Zabbix bis Version 2.x.

## <a name="configure-alert-collection"></a>Konfigurieren der Erfassung von Warnungen

### <a name="configuring-nagios-alert-collection"></a>Konfigurieren der Erfassung von Nagios-Warnungen
Führen Sie die folgenden Schritte auf dem Nagios-Server aus, um Warnungen zu erfassen.

1. Gewähren Sie dem Benutzer **omsagent** Lesezugriff auf die Nagios-Protokolldatei `/var/log/nagios/nagios.log`. Sie können den Benutzer **omsagent** der **nagios**-Gruppe hinzufügen, vorausgesetzt, die „nagios.log“-Datei gehört der Gruppe `nagios`. 

    sudo usermod -a -G nagios omsagent

2.  Ändern Sie die Konfigurationsdatei unter `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht auskommentiert sind:  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. Starten Sie den omsagent-Daemon neu.

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Konfigurieren der Erfassung von Zabbix-Warnungen
Sie müssen einen Benutzer und ein Kennwort in *Klartext* eingeben, um Warnungen von einem Zabbix-Server zu erfassen.  Auch wenn dies nicht ideal ist, empfehlen wir die Erstellung eines Zabbix-Benutzers nur mit Leseberechtigungen zum Abfangen relevanter Alarme.

Führen Sie die folgenden Schritte aus, um Warnungen auf dem Nagios-Server zu erfassen.

1. Ändern Sie die Konfigurationsdatei unter `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht auskommentiert sind.  Ändern Sie den Benutzernamen und das Kennwort in Werte für Ihre Zabbix-Umgebung.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Starten Sie den omsagent-Daemon neu.

    `sudo sh /opt/microsoft/omsagent/bin/service_control restart`


## <a name="alert-records"></a>Warnungsdatensätze
Sie können mit [Protokollsuchen](log-analytics-queries.md) Warnungsdatensätze von Nagios und Zabbix in Log Analytics abrufen.

### <a name="nagios-alert-records"></a>Nagios-Warnungsdatensätze

Von Nagios erfasste Datensätze haben den **Typ** **Warnung** und das **SourceSystem** **Nagios**.  Sie verfügen über die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*Nagios* |
| AlertName |Name der Warnung. |
| AlertDescription | Beschreibung der Warnung. |
| AlertState | Status des Diensts oder Hosts<br><br>OK<br>WARNING<br>UP<br>DOWN |
| HostName | Der Name des Hosts, der die Warnung erstellt hat |
| PriorityNumber | Prioritätsstufe der Warnung. |
| StateType | Der Statustyp der Warnung<br><br>SOFT: Problem, das nicht erneut geprüft wurde<br>HARD: Problem, das so oft wie angegeben erneut geprüft wurde  |
| TimeGenerated |Datum und Uhrzeit der Warnungserstellung. |


### <a name="zabbix-alert-records"></a>Zabbix-Warnungsdatensätze
Von Zabbix erfasste Datensätze haben den **Typ** **Warnung** und das **SourceSystem** **Zabbix**.  Sie verfügen über die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*Zabbix* |
| AlertName | Name der Warnung. |
| AlertPriority | Schweregrad der Warnung<br><br>not classified<br>Information<br>Warnung<br>average<br>high<br>disaster  |
| AlertState | Status der Warnung<br><br>0: Status ist auf dem neuesten Stand.<br>1: Status ist unbekannt.  |
| AlertTypeNumber | Gibt an, ob Warnungen mehrere Problemereignisse generieren können<br><br>0: Status ist auf dem neuesten Stand.<br>1: Status ist unbekannt.    |
| Kommentare | Zusätzliche Kommentare zur Warnung |
| HostName | Der Name des Hosts, der die Warnung erstellt hat |
| PriorityNumber | Der Wert, der den Schweregrad der Warnung angibt<br><br>0: Nicht klassifiziert<br>1: Information<br>2: Warnung<br>3: Durchschnitt<br>4: Hoch<br>5: Notfall |
| TimeGenerated |Datum und Uhrzeit der Warnungserstellung. |
| TimeLastModified |Datum und Uhrzeit der letzten Änderung des Status der Warnung |


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Warnungen](../monitoring-and-diagnostics/monitoring-overview-alerts.md) finden Sie in Log Analytics.
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-queries.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 

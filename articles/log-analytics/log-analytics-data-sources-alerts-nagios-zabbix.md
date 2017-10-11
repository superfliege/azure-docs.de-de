---
title: Erfassen von Nagios und Zabbix-Warnungen in der OMS-Protokollanalyse | Microsoft Docs
description: "Nagios und Zabbix sind Tools zur Überwachung open-Source. Sie können Warnungen von diesen Tools zu Protokollanalyse zusammenfassen, um diese zusammen mit Warnungen aus anderen Quellen zu analysieren.  Dieser Artikel beschreibt das Konfigurieren der OMS-Agent für Linux für die Warnungen von diesen Systemen zu erfassen."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 0b64c32e1031e704d50aab0b38eaea41e27d134b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Erfassen von Warnungen von Nagios und Zabbix in Protokollanalyse von OMS-Agent für Linux 
[Nagios](https://www.nagios.org/) und [Zabbix](http://www.zabbix.com/) open-Source Überwachungstools sind.  Sie können Warnungen in Protokollanalyse über diese Tools erfassen, um zusammen mit zu analysieren [Warnungen aus anderen Quellen](log-analytics-alerts.md).  Dieser Artikel beschreibt das Konfigurieren der OMS-Agent für Linux für die Warnungen von diesen Systemen zu erfassen.
 
## <a name="configure-alert-collection"></a>Konfigurieren der Erfassung von Warnungen

### <a name="configuring-nagios-alert-collection"></a>Konfigurieren der Erfassung von Nagios-Warnungen
Führen Sie die folgenden Schritte aus, auf die Nagios-Server, um Warnungen zu sammeln.

1. Gewähren Sie dem Benutzer **"omsagent"** Lesezugriff auf die Nagios-Protokolldatei (d. h. `/var/log/nagios/nagios.log`). Vorausgesetzt, die Datei "nagios.log" im Besitz der Gruppe `nagios`, können Sie den Benutzer hinzufügen **"omsagent"** auf die **Nagios** Gruppe. 

    "sudo" Usermod - a -G Nagios "omsagent"

2.  Ändern die Konfigurationsdatei an (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht als Kommentare formatiert sind:  

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

3. Starten Sie den Omsagent-Daemon neu.

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Konfigurieren der Erfassung von Zabbix-Warnungen
Um Warnungen von einem Zabbix-Server zu erfassen, müssen Sie einen Benutzer und Kennwort in angeben *Klartext*. Dies ist nicht ideal, aber es wird empfohlen, erstellen Sie den Benutzer und gewähren von Berechtigungen für Onlu zu überwachen.

Führen Sie die folgenden Schritte aus, auf die Nagios-Server, um Warnungen zu sammeln.

1. Ändern die Konfigurationsdatei an (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht als Kommentare formatiert sind.  Ändern Sie den Benutzernamen und das Kennwort auf Werte für Ihre Umgebung Zabbix an.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Starten Sie den Omsagent-Daemon neu.

    "sudo" sh /opt/microsoft/omsagent/bin/service_control neu starten


## <a name="alert-records"></a>Warnung Datensätze
Sie können Warnungen Datensätze von Nagios und Zabbix Abrufen mit [protokollieren sucht](log-analytics-log-searches.md) in Protokollanalyse.

### <a name="nagios-alert-records"></a>Warnung Nagios-Datensätze

Warnung Nagios erhobene-Datensätze ein **Typ** von **Warnung** und eine **SourceSystem** von **Nagios**.  Sie haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*Nagios* |
| AlertName |Der Name der Warnung. |
| AlertDescription | Beschreibung der Warnung. |
| Warnungsstatus (alertstate) | Status des Diensts oder der Host.<br><br>OK<br>WARNUNG<br>NACH-OBEN<br>NACH-UNTEN |
| HostName | Der Name des Hosts, die die datenwarnung erstellt hat. |
| PriorityNumber | Priorität der Warnung an. |
| StateType | Der Typ der Zustand der Warnung.<br><br>SOFT - Problem, das nicht erneut geprüft werden soll.<br>Festplatten - Problem, die eine angegebene Anzahl von Malen kennwortanforderungen.  |
| TimeGenerated |Datum und Uhrzeit der Erstellung die Warnung. |


### <a name="zabbix-alert-records"></a>Warnung Zabbix-Datensätze
Warnungen von Zabbix gesammelten Datensätze haben eine **Typ** von **Warnung** und ein **SourceSystem** von **Zabbix**.  Sie haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*Zabbix* |
| AlertName | Der Name der Warnung. |
| AlertPriority | Schweregrad der Warnung.<br><br>nicht klassifiziert<br>Informationen<br>Warnung<br>Durchschnitt<br>Hohe<br>im Notfall  |
| Warnungsstatus (alertstate) | Status der Warnung.<br><br>0 - Status wird auf dem neuesten Stand.<br>1 - Status ist unbekannt.  |
| AlertTypeNumber | Gibt an, ob Warnungen mehrere Problem Ereignisse generiert werden können.<br><br>0 - Status wird auf dem neuesten Stand.<br>1 - Status ist unbekannt.    |
| Kommentare | Zusätzliche Kommentare zur Warnung. |
| HostName | Der Name des Hosts, die die datenwarnung erstellt hat. |
| PriorityNumber | Der Wert, der angibt, der Schweregrad der Warnung.<br><br>0 – nicht klassifiziert<br>1 - Informationen<br>2 – Warnung<br>3 – Durchschnitt<br>4 – hoch<br>5 – nach einem Notfall |
| TimeGenerated |Datum und Uhrzeit der Erstellung die Warnung. |
| TimeLastModified |Datum und Uhrzeit der letzten Änderung der Zustand der Warnung. |


## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Warnungen](log-analytics-alerts.md) in Protokollanalyse.
* Erfahren Sie mehr über [protokollieren sucht](log-analytics-log-searches.md) zum Analysieren der Daten in Datenquellen / Lösungen gesammelt. 

---
title: Nachverfolgen von Änderungen mit Azure Automation
description: Mit der Lösung für die Änderungsnachverfolgung können Sie Änderungen an Software und Windows-Diensten in Ihrer Umgebung besser erkennen.
services: automation
ms.service: automation
ms.component: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 717cf6b2abfb529313699836b790bd3f07844a67
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867952"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Nachverfolgen von Änderungen in Ihrer Umgebung mit der Lösung für die Änderungsnachverfolgung

Dieser Artikel unterstützt Sie bei der Verwendung der Lösung für die Änderungsnachverfolgung, damit Sie Änderungen an Ihrer Umgebung einfach erkennen können. Die Lösung verfolgt Änderungen an Windows- und Linux-Software, an Windows- und Linux-Dateien sowie an Windows-Registrierungsschlüsseln, Windows-Diensten und Linux-Daemons. Durch Ermitteln von Konfigurationsänderungen können Sie Betriebsprobleme präzise bestimmen.

Änderungen an installierter Software, an Windows-Diensten, an der Windows-Registrierung und an Windows-Dateien sowie an Linux-Daemons auf den überwachten Servern werden zur Verarbeitung an den Log Analytics-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Mithilfe der Informationen im Change Tracking-Dashboard können Sie ganz leicht die Änderungen erkennen, die in Ihrer Serverinfrastruktur vorgenommen wurden.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

Um mit der Nachverfolgung von Änderungen zu beginnen, müssen Sie die Lösung für die Änderungsnachverfolgung und den Bestand für Ihr Automation-Konto aktivieren.

1. Navigieren Sie im Azure-Portal zu Ihrem Automation-Konto.
1. Wählen Sie **Änderungsnachverfolgung** unter **KONFIGURATION** aus.
1. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich oder **Neuen Arbeitsbereich erstellen** aus, und klicken Sie auf **Aktivieren**.

Dadurch wird die Lösung für Ihr Automation-Konto aktiviert. Das Aktivieren der Lösung kann bis zu 15 Minuten dauern. Das blaue Banner dient zur Benachrichtigung, wenn die Lösung aktiviert ist. Navigieren Sie zurück zur Seite **Änderungsnachverfolgung**, um die Lösung zu verwalten.

## <a name="configuring-change-tracking-and-inventory"></a>Konfigurieren der Änderungs- und Bestandsnachverfolgung

Wie Sie Computer in die Lösung integrieren, erfahren Sie unter [Integrieren von Automation-Lösungen](automation-onboard-solutions-from-automation-account.md). Nachdem ein Computeronboarding mit der Lösung „Änderungsnachverfolgung“ und „Bestand“ vorgenommen wurde, können Sie die nachzuverfolgenden Elemente konfigurieren. Wenn Sie eine neue Datei oder einen Registrierungsschlüssel zum Nachverfolgen aktivieren, wird das entsprechende Element sowohl für die Änderungsnachverfolgung als auch die Bestandsverfolgung aktiviert.

Zum Nachverfolgen von Änderungen unter Windows und Linux werden MD5-Hashes der Dateien verwendet. Diese Hashes werden dann verwendet, um zu ermitteln, ob seit der letzten Bestandsaufnahme eine Änderung vorgenommen wurde.

### <a name="configure-linux-files-to-track"></a>Konfigurieren von Linux-Dateien für die Nachverfolgung

Führen Sie zum Konfigurieren der Dateinachverfolgung auf Linux-Computern die folgenden Schritte aus:

1. Wählen Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** die Option **Änderungsnachverfolgung** aus. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
2. Wählen Sie auf der Seite **Änderungsnachverfolgung** die Option **Linux-Dateien** aus, und klicken Sie dann auf **+ Hinzufügen**, um eine neue nachzuverfolgende Datei hinzuzufügen.
3. Geben Sie unter **Linux-Datei für Änderungsnachverfolgung hinzufügen** die Informationen zu der Datei oder dem Verzeichnis ein, die bzw. das nachverfolgt werden soll, und klicken Sie auf **Speichern**.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Gruppenname für die logische Gruppierung von Dateien        |
|Pfad eingeben     | Der zu überprüfende Pfad für die Datei, Beispiel: „/etc/*.conf“       |
|Pfadtyp     | Typ des nachzuverfolgenden Elements (mögliche Werte sind „Datei“ und „Verzeichnis“)        |
|Rekursion     | Bestimmt, ob beim Suchen nach dem nachzuverfolgenden Element die Rekursion verwendet wird        |
|Sudo verwenden     | Diese Einstellung bestimmt, ob „sudo“ bei der Suche nach dem Element verwendet wird         |
|Links     | Diese Einstellung bestimmt, wie symbolische Verknüpfungen beim Durchlaufen von Verzeichnissen behandelt werden<br> **Ignorieren**: Symbolische Verknüpfungen werden ignoriert, und die referenzierten Dateien/Verzeichnisse werden nicht einbezogen<br>**Folgen**: Folgt den symbolischen Verknüpfungen bei der Rekursion und bindet auch die referenzierten Dateien/Verzeichnisse ein<br>**Verwalten**: Folgt den symbolischen Verknüpfungen und ermöglicht das Ändern von zurückgegebenen Inhalten     |
|Hochladen von Dateiinhalt für alle Einstellungen| Aktiviert oder deaktiviert den Upload des Dateiinhalts für nachverfolgte Änderungen. Verfügbare Optionen: **TRUE** oder **FALSE**.|

> [!NOTE]
> Die Linkoption „Verwalten“ wird nicht empfohlen. Das Abrufen von Dateiinhalten wird nicht unterstützt.

### <a name="configure-windows-files-to-track"></a>Konfigurieren der nachzuverfolgenden Windows-Dateien

Führen Sie zum Konfigurieren der Dateinachverfolgung auf Windows-Computern die folgenden Schritte aus:

1. Wählen Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** die Option **Änderungsnachverfolgung** aus. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
2. Wählen Sie auf der Seite **Änderungsnachverfolgung** die Option **Windows-Dateien** aus, und klicken Sie dann auf **+ Hinzufügen**, um eine neue nachzuverfolgende Datei hinzuzufügen.
3. Geben Sie unter **Windows-Datei für Änderungsnachverfolgung hinzufügen** die Informationen zu der nachzuverfolgenden Datei ein, und klicken Sie auf **Speichern**.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Gruppenname für die logische Gruppierung von Dateien        |
|Pfad eingeben     | Der zu überprüfende Pfad für die Datei, z. B. „c:\temp\meinedatei.txt“       |
|Hochladen von Dateiinhalt für alle Einstellungen| Aktiviert oder deaktiviert den Upload des Dateiinhalts für nachverfolgte Änderungen. Verfügbare Optionen: **TRUE** oder **FALSE**.|

## <a name="configure-file-content-tracking"></a>Konfigurieren der Nachverfolgung von Dateiinhalten

Mithilfe der Nachverfolgung von Dateiinhalten können Sie die Inhalte vor und nach vorgenommenen Änderungen an einer Datei anzeigen. Dies ist für Windows- und Linux-Dateien verfügbar. Bei jeder Dateiänderung werden die Inhalte der Datei in einem Speicherkonto gespeichert, und die Datei wird in den Versionen vor und nach der Änderung angezeigt, wahlweise inline oder nebeneinander. Weitere Informationen finden Sie unter [View the contents of a tracked file](change-tracking-file-contents.md) (Anzeigen der Inhalte einer nachverfolgten Datei).

![Anzeigen von Änderungen in einer Datei](./media/change-tracking-file-contents/view-file-changes.png)

### <a name="configure-windows-registry-keys-to-track"></a>Konfigurieren der nachzuverfolgenden Windows-Registrierungsschlüssel

Führen Sie zum Konfigurieren der Nachverfolgung von Registrierungsschlüsseln auf Windows-Computern die folgenden Schritte aus:

1. Wählen Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** die Option **Änderungsnachverfolgung** aus. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
2. Wählen Sie auf der Seite **Änderungsnachverfolgung** die Option **Windows-Registrierung** aus, und klicken Sie dann auf **+ Hinzufügen**, um einen neuen nachzuverfolgenden Registrierungsschlüssel hinzuzufügen.
3. Geben Sie unter **Windows-Registrierung für Änderungsnachverfolgung hinzufügen** die Informationen zu dem Schlüssel ein, der nachverfolgt werden soll, und klicken Sie auf **Speichern**.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Gruppenname für die logische Gruppierung von Dateien        |
|Windows-Registrierungsschlüssel   | Der zu überprüfende Pfad für die Datei, z. B. „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup“      |

## <a name="limitations"></a>Einschränkungen

Die Lösung für die Änderungsnachverfolgung unterstützt derzeit folgende Elemente nicht:

* Ordner (Verzeichnisse) für die Nachverfolgung von Windows-Dateien
* Rekursion für die Nachverfolgung von Windows-Dateien
* Platzhalter für die Nachverfolgung von Windows-Dateien
* Rekursion für die Nachverfolgung der Windows-Registrierung
* Pfadvariablen
* Netzwerkdateisysteme
* Inhalt der Datei

Weitere Einschränkungen:

* Die Spalte **Maximale Dateigröße** und ihre Werte werden in der aktuellen Implementierung nicht genutzt.
* Wenn Sie im 30-minütigen Sammlungszyklus mehr als 2.500 Dateien erfassen, wird die Leistung der Lösung möglicherweise beeinträchtigt.
* Wenn der Netzwerkverkehr hoch ist, kann es bis zu sechs Stunden dauern, bis Änderungsdatensätze angezeigt werden.
* Wenn Sie die Konfiguration ändern, während ein Computer heruntergefahren ist, stellt der Computer möglicherweise Änderungen bereit, die zur vorherigen Konfiguration gehörten.

## <a name="known-issues"></a>Bekannte Probleme

Die Lösung für die Änderungsnachverfolgung weist derzeit die folgenden Probleme auf:

* Hotfix-Updates werden für Computer mit Windows 10 Creators Update und Windows Server 2016 Core RS3 nicht erfasst.

## <a name="change-tracking-data-collection-details"></a>Details zur Datensammlung für die Änderungsnachverfolgung

Die folgende Tabelle zeigt die Datensammlungshäufigkeit für die Änderungstypen. Für jeden Typ wird die Datenmomentaufnahme des aktuellen Status außerdem mindestens alle 24 Stunden aktualisiert:

| **Änderungstyp** | **Frequency** |
| --- | --- |
| Windows-Registrierung | 50 Minuten |
| Windows-Datei | 30 Minuten |
| Linux-Datei | 15 Minuten |
| Windows-Dienste | 10 Sekunden bis 30 Minuten</br> Standardwert: 30 Minuten |
| Linux-Daemons | 5 Minuten |
| Windows-Software | 30 Minuten |
| Linux-Software | 5 Minuten |

### <a name="windows-service-tracking"></a>Windows-Dienstnachverfolgung

Die Standard-Sammelhäufigkeit für Windows-Dienste beträgt 30 Minuten. Um die Häufigkeit zu konfigurieren, wechseln Sie zu **Änderungsnachverfolgung**. Auf der Registerkarte **Windows-Dienste** befindet sich unter **Bearbeitungseinstellungen** ein Schieberegler, mit dem Sie die Sammelhäufigkeit für Windows-Dienste vom kleinsten Wert von 10 Sekunden bis zum größten von 30 Minuten ändern können. Bewegen Sie den Schieberegler auf die gewünschte Häufigkeit; sie wird automatisch gespeichert.

![Windows-Dienste-Schieberegler](./media/automation-change-tracking/windowservices.png)

Der Agent verfolgt nur Änderungen nach, dies optimiert seine Leistung. Wird ein zu hoher Schwellenwert festgelegt, können Änderungen verpasst werden, wenn der Dienst wieder in seinen ursprünglichen Zustand zurückgekehrt ist. Das Festlegen der Häufigkeit auf einen kleineren Wert ermöglicht es Ihnen, Änderungen zu erfassen, die sonst verpasst würden.

> [!NOTE]
> Zwar kann der Agent Änderungen bis hinab zu einem 10-Sekunden-Intervall nachverfolgen, bis zur Anzeige der Daten im Portal vergehen jedoch einige Minuten. Änderungen, die zwischen der Erfassung und der Anzeige im Portal erfolgen, werden trotzdem nachverfolgt und protokolliert.
  
### <a name="registry-key-change-tracking"></a>Registrierungsschlüssel-Änderungsnachverfolgung

Die Überwachung von Änderungen der Registrierungsschlüssel dient dem Ermitteln von Erweiterungspunkten, an denen Code von Drittanbietern und Schadsoftware aktiv werden können. Die folgende Liste enthält die Liste der vorkonfigurierten Registrierungsschlüssel. Diese Schlüssel sind konfiguriert, aber nicht aktiviert. Um diese Registrierungsschlüssel nachzuverfolgen, müssen Sie jeden einzeln aktivieren.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit „Explorer.exe“ ausgeführt werden.    |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht Skripts, die beim Start ausgeführt werden.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht Skripts, die beim Herunterfahren ausgeführt werden.     |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht Schlüssel, die geladen werden, bevor sich der Benutzer bei seinem Windows-Konto anmeldet. Der Schlüssel wird für 32-Bit-Programme verwendet, die auf 64-Bit-Computern ausgeführt werden.    |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht Änderungen an Anwendungseinstellungen.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit „Explorer.exe“ ausgeführt werden.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit „Explorer.exe“ ausgeführt werden.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht die Symboloverlayhandler-Registrierung.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht die Symboloverlayhandler-Registrierung für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht neue Browserhilfsobjekt-Plug-Ins für Internet Explorer. Wird für den Zugriff auf das Dokumentobjektmodell (DOM) der aktuellen Seite und zum Steuern der Navigation verwendet.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht neue Browserhilfsobjekt-Plug-Ins für Internet Explorer. Wird für den Zugriff auf das Dokumentobjektmodell (DOM) der aktuellen Seite und zum Steuern der Navigation für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden, verwendet.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht, ob neue Internet Explorer-Erweiterungen vorliegen, z. B. benutzerdefinierte Toolmenüs und benutzerdefinierte Symbolleistenschaltflächen.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht, ob neue Internet Explorer-Erweiterungen vorliegen, z. B. benutzerdefinierte Toolmenüs und benutzerdefinierte Symbolleisten-Schaltflächen für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht die mit wavemapper zugeordneten 32-Bit-Treiber, wave1 und wave2, msacm.imaadpcm, .msadpcm, .msgsm610 und vidc. Ähnlich dem Abschnitt „[drivers]“ in der SYSTEM. INI-Datei.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht die mit wavemapper zugeordneten 32-Bit-Treiber, wave1 und wave2, msacm.imaadpcm, .msadpcm, .msgsm610 und vidc für 32-Bit-Programme, die auf 64-Bit-Computern ausgeführt werden. Ähnlich dem Abschnitt „[drivers]“ in der SYSTEM. INI-Datei.|
> |**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht die Liste der bekannten oder häufig verwendete System-DLLs. Dieses System verhindert, dass schwache Anwendungsverzeichnisberechtigungen durch Infiltration mit Trojanerversionen von System-DLLs ausgenutzt werden.|
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;Überwacht die Liste der Pakete, die Ereignisbenachrichtigungen von Winlogon, dem interaktiven Anmeldungsunterstützungsmodell für das Windows-Betriebssystem, empfangen können.|

## <a name="use-change-tracking"></a>Verwenden von Change Tracking

Nachdem die Lösung aktiviert ist, können Sie die Zusammenfassung der Änderungen für Ihre überwachten Computern anzeigen. Wählen Sie dazu in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** die Option **Änderungsnachverfolgung** aus.

Sie können zuerst die Änderungen an Ihren Computern und dann Einzelheiten für jedes Ereignis anzeigen. Dropdownmenüs stehen am oberen Rand des Diagramms zur Verfügung, damit Sie anhand des Änderungstyps und Zeitbereichs das Diagramm und die detaillierten Informationen einschränken können. Sie können auch auf dem Diagramm klicken und ziehen, um einen benutzerdefinierten Zeitbereich auszuwählen.

![Abbildung des Change Tracking-Dashboards](./media/automation-change-tracking/change-tracking-dash01.png)

Detaillierte Informationen zu einer Änderung oder einem Ereignis können Sie durch Klicken auf die Änderung bzw. das Ereignis aufrufen. Wie aus dem Beispiel hervorgeht, wurde der Starttyp des Diensts von „Manuell“ in „Automatisch“ geändert.

![Abbildung mit Details der Änderungsnachverfolgung](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Protokollsuche

Zusätzlich zu den Details, die im Portal bereitgestellt werden, können Sie auch die Protokolle durchsuchen. Öffnen Sie die Seite **Änderungsnachverfolgung**, und klicken Sie auf **Log Analytics**; daraufhin wird die Seite **Protokollsuche** geöffnet.

### <a name="sample-queries"></a>Beispielabfragen

Die folgende Tabelle enthält Beispiele für Protokollsuchen für Änderungsdatensätze, die mit dieser Lösung erfasst wurden:

|Abfragen  |BESCHREIBUNG  |
|---------|---------|
|ConfigurationData<br>&#124; where   ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"<br>&#124; where SvcState == "Stopped"<br>&#124; summarize arg_max(TimeGenerated, *) by SoftwareName, Computer         | Zeigt die aktuellen Bestandsdatensätze für Windows-Dienste an, die auf „Auto“ festgelegt, aber als „Beendet“ gemeldet wurden.<br>Die Ergebnisse werden auf den aktuellen Datensatz für den betreffenden Softwarenamen und Computer beschränkt.      |
|ConfigurationChange<br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Removed"<br>&#124; order by TimeGenerated desc|Zeigt die Änderungsdatensätze für entfernte Software an.|

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das Tutorial zur Änderungsnachverfolgung, um mehr über die Verwendung der Lösung zu erfahren:

> [!div class="nextstepaction"]
> [Problembehandlung für Änderungen in Ihrer Umgebung](automation-tutorial-troubleshoot-changes.md)

* Verwenden Sie die [Protokollsuche in Log Analytics](../log-analytics/log-analytics-log-searches.md) , um ausführliche Daten zur Änderungsnachverfolgung anzuzeigen.

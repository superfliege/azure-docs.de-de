---
title: Durchführen der Problembehandlung für Änderungen auf einem virtuellen Azure-Computer | Microsoft-Dokumentation
description: Verwenden Sie die Änderungsnachverfolgung, um die Problembehandlung für Änderungen auf einem virtuellen Azure-Computer durchzuführen.
services: automation
ms.service: automation
ms.component: change-inventory-management
keywords: Änderung, Nachverfolgung, Automatisierung
author: jennyhunter-msft
ms.author: jehunte
ms.date: 08/27/2018
ms.topic: tutorial
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: fd94fd234067f63eab424c7f757d4adf842e7b46
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43120584"
---
# <a name="troubleshoot-changes-in-your-environment"></a>Problembehandlung für Änderungen in Ihrer Umgebung

In diesem Tutorial wird beschrieben, wie Sie die Problembehandlung für Änderungen auf einem virtuellen Azure-Computer durchführen. Indem Sie die Änderungsnachverfolgung aktivieren, können Sie Änderungen für Software, Dateien, Linux-Daemons, Windows-Dienste und Windows-Registrierungsschlüssel auf Ihren Computern nachverfolgen.
Die Identifizierung dieser Konfigurationsänderungen ermöglicht die Erkennung von betriebsbezogenen Problemen in Ihrer Umgebung.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren einer VM für die Änderungsnachverfolgung und den Bestand
> * Durchsuchen von Änderungsprotokollen nach beendeten Diensten
> * Konfigurieren der Änderungsnachverfolgung
> * Aktivieren der Aktivitätsprotokollverbindung
> * Auslösen eines Ereignisses
> * Anzeigen von Änderungen

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* Ein [Automation-Konto](automation-offering-get-started.md) für die Watcher- und Aktionsrunbooks und den Watchertask.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Änderungsnachverfolgung und Bestandsfunktion

In diesem Tutorial müssen Sie zuerst die Änderungsnachverfolgung und die Bestandsfunktion für Ihre VM aktivieren. Dieser Schritt ist nicht erforderlich, falls Sie zuvor eine andere Automatisierungslösung für eine VM aktiviert haben.

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer**, und wählen Sie in der Liste eine VM aus.
1. Klicken Sie im Menü auf der linken Seite unter dem Abschnitt **VORGÄNGE** auf **Bestand**. Die Seite **Änderungsnachverfolgung** wird geöffnet.

![Aktivieren der Änderung](./media/automation-tutorial-troubleshoot-changes/enableinventory.png) Die Anzeige **Änderungsnachverfolgung** wird geöffnet. Konfigurieren Sie den gewünschten Standort, den Log Analytics-Arbeitsbereich und das Automation-Konto, und klicken Sie auf **Aktivieren**. Wenn die Felder ausgegraut sind, bedeutet dies, dass eine andere Automatisierungslösung für die VM aktiviert ist und derselbe Arbeitsbereich und dasselbe Automation-Konto verwendet werden müssen.

Mit einem [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Bestandsfunktion generiert werden.
Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen.

Beim Onboarding wird die VM mit dem Microsoft Monitoring Agent (MMA) und Hybrid Worker bereitgestellt.
Dieser Agent wird verwendet, um mit dem virtuellen Computer zu kommunizieren und Informationen zur installierten Software abzurufen.

Das Aktivieren der Lösung kann bis zu 15 Minuten dauern. Während dieses Zeitraums sollten Sie das Browserfenster nicht schließen.
Nachdem die Lösung aktiviert wurde, werden Informationen zur installierten Software und Änderungen der VM-Datenflüsse zu Log Analytics angezeigt.
Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

## <a name="using-change-tracking-in-log-analytics"></a>Verwenden der Änderungsnachverfolgung in Log Analytics

Bei der Änderungsnachverfolgung werden Protokolldaten generiert, die an Log Analytics gesendet werden. Um die Protokolle per Ausführung von Abfragen zu durchsuchen, klicken Sie oben im Fenster **Änderungsnachverfolgung** auf **Log Analytics**.
Die Daten der Änderungsnachverfolgung werden unter dem Typ **ConfigurationChange** gespeichert. Mit der folgenden Log Analytics-Beispielabfrage werden alle Windows-Dienste zurückgegeben, die beendet wurden.

```
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Weitere Informationen zur Ausführung von Abfragen und zum Durchsuchen von Protokolldateien in Log Analytics finden Sie unter [Azure Log Analytics](https://docs.loganalytics.io/index).

## <a name="configure-change-tracking"></a>Konfigurieren der Änderungsnachverfolgung

Die Änderungsnachverfolgung ermöglicht Ihnen das Nachverfolgen von Konfigurationsänderungen auf Ihrer VM. In den folgenden Schritten wird veranschaulicht, wie Sie die Nachverfolgung von Registrierungsschlüsseln und Dateien konfigurieren.
 
Wählen Sie oben auf der Seite **Änderungsnachverfolgung** die Option **Einstellungen bearbeiten**, um anzugeben, welche Dateien und Registrierungsschlüssel erfasst und nachverfolgt werden sollen.

> [!NOTE]
> Für die Bestandsfunktion und die Änderungsnachverfolgung werden die gleichen Sammlungseinstellungen verwendet, und die Einstellungen werden auf einer Arbeitsbereichsebene konfiguriert.

Fügen Sie im Fenster **Arbeitsbereichskonfiguration** wie in den nächsten drei Abschnitten erläutert die Windows-Registrierungsschlüssel und die Windows- oder Linux-Dateien hinzu, die nachverfolgt werden sollen.

### <a name="add-a-windows-registry-key"></a>Hinzufügen eines Windows-Registrierungsschlüssels

1. Wählen Sie auf der Registerkarte **Windows-Registrierung** die Option **Hinzufügen**.
    Das Fenster **Windows-Registrierung für Änderungsnachverfolgung hinzufügen** wird geöffnet.

3. Geben Sie unter **Windows-Registrierung für Änderungsnachverfolgung hinzufügen** die Informationen zu dem Schlüssel ein, der nachverfolgt werden soll, und klicken Sie auf **Speichern**.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Ein Gruppenname für die logische Gruppierung von Dateien        |
|Windows Registry Key   | Der Pfad für die Überprüfung auf die Datei, z.B. „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup“      |

### <a name="add-a-windows-file"></a>Hinzufügen einer Windows-Datei

1. Wählen Sie auf der Registerkarte **Windows-Dateien** die Option **Hinzufügen**. Das Fenster **Windows-Datei für Änderungsnachverfolgung hinzufügen** wird geöffnet.

1. Geben Sie unter **Windows-Datei für Änderungsnachverfolgung hinzufügen** die Informationen zu der Datei oder dem Verzeichnis ein, die bzw. das nachverfolgt werden soll, und klicken Sie auf **Speichern**.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Ein Gruppenname für die logische Gruppierung von Dateien        |
|Pfad eingeben     | Der zu überprüfende Pfad für die Datei, z.B. „c:\temp\\\*.txt“<br>Sie können auch Umgebungsvariablen verwenden, beispielsweise „%winDir%\System32\\\*.*“.         |
|Rekursion     | Bestimmt, ob beim Suchen nach dem nachzuverfolgenden Element die Rekursion verwendet wird        |
|Hochladen von Dateiinhalt für alle Einstellungen| Aktiviert oder deaktiviert den Upload des Dateiinhalts für nachverfolgte Änderungen. Verfügbare Optionen: **TRUE** oder **FALSE**.|

### <a name="add-a-linux-file"></a>Hinzufügen einer Linux-Datei

1. Wählen Sie auf der Registerkarte **Linux-Dateien** die Option **Hinzufügen**. Das Fenster **Linux-Datei für Änderungsnachverfolgung hinzufügen** wird geöffnet.

1. Geben Sie unter **Linux-Datei für Änderungsnachverfolgung hinzufügen** die Informationen zu der Datei oder dem Verzeichnis ein, die bzw. das nachverfolgt werden soll, und klicken Sie auf **Speichern**.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|Aktiviert     | Bestimmt, ob die Einstellung angewendet wird        |
|Item Name     | Anzeigename der nachzuverfolgenden Datei        |
|Group     | Ein Gruppenname für die logische Gruppierung von Dateien        |
|Enter Path     | Der zu überprüfende Pfad für die Datei, z.B. „/etc/*.conf“       |
|Path Type     | Typ des nachzuverfolgenden Elements (mögliche Werte sind „File“ und „Directory“)        |
|Recursion     | Bestimmt, ob beim Suchen nach dem nachzuverfolgenden Element die Rekursion verwendet wird        |
|Sudo verwenden     | Diese Einstellung bestimmt, ob „sudo“ bei der Suche nach dem Element verwendet wird         |
|Links     | Diese Einstellung bestimmt, wie symbolische Verknüpfungen beim Durchlaufen von Verzeichnissen behandelt werden<br> **Ignore**: Symbolische Links werden ignoriert, und die referenzierten Dateien/Verzeichnisse werden nicht einbezogen.<br>**Follow**: Folgt den symbolischen Links während der Rekursion und bindet auch die referenzierten Dateien/Verzeichnisse ein.<br>**Manage**: Folgt den symbolischen Links und ermöglicht eine Änderung von zurückgegebenen Inhalten.      |
|Hochladen von Dateiinhalt für alle Einstellungen| Aktiviert oder deaktiviert den Upload des Dateiinhalts für nachverfolgte Änderungen. Verfügbare Optionen: **TRUE** oder **FALSE**.|

   > [!NOTE]
   > Die Linkoption „Verwalten“ wird nicht empfohlen. Das Abrufen von Dateiinhalten wird nicht unterstützt.

## <a name="enable-activity-log-connection"></a>Aktivieren der Aktivitätsprotokollverbindung

Wählen Sie auf Ihrer VM auf der Seite **Änderungsnachverfolgung** die Option **Aktivitätsprotokollverbindung verwalten**. Mit diesem Vorgang wird die Seite **Azure-Aktivitätsprotokoll** geöffnet. Wählen Sie **Verbinden**, um die Änderungsnachverfolgung mit der Azure-Aktivität für Ihre VM zu verbinden.

Navigieren Sie bei aktivierter Einstellung auf die Seite **Übersicht** Ihrer VM, und wählen Sie **Beenden**, um die VM zu beenden. Wählen Sie nach der entsprechenden Aufforderung **Ja**, um die VM zu beenden. Wählen Sie nach der Aufhebung der Zuordnung die Option **Starten**, um die VM neu zu starten.

Beim Beenden und Starten einer VM wird im Aktivitätsprotokoll dazu ein Ereignis protokolliert. Navigieren Sie zurück zur Seite **Änderungsnachverfolgung**. Wählen Sie unten auf der Seite die Registerkarte **Ereignisse**. Nach kurzer Wartezeit werden die Ereignisse im Diagramm und in der Tabelle angezeigt. Wie im vorherigen Schritt auch, können Sie jedes Ereignis auswählen, um ausführliche Informationen dazu anzuzeigen.

![Anzeigen von Details zu den Änderungen im Portal](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Anzeigen von Änderungen

Nachdem die Lösung für die Änderungsnachverfolgung und den Bestand aktiviert wurde, können Sie die Ergebnisse auf der Seite **Änderungsnachverfolgung** anzeigen.

Wählen Sie auf Ihrer VM unter **VORGÄNGE** die Option **Änderungsnachverfolgung**.

![Screenshot mit der Liste der Änderungen an der VM](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

Im Diagramm werden Änderungen angezeigt, die im Laufe der Zeit durchgeführt wurden.
Nachdem Sie eine Aktivitätsprotokollverbindung hinzugefügt haben, werden oben im Liniendiagramm Azure-Aktivitätsprotokollereignisse angezeigt.
Jede Zeile mit Balkendiagrammen steht für einen eigenen Änderungstyp, der nachverfolgt werden kann.
Diese Typen sind Linux-Daemons, Dateien, Windows-Registrierungsschlüssel, Software und Windows-Dienste.
Auf der Registerkarte mit den Änderungen werden die Details für die angezeigten Änderungen der Visualisierung in absteigender Reihenfolge ihres Auftretens aufgeführt (neueste zuerst).
Auf der Registerkarte **Ereignisse** werden in der Tabelle die verbundenen Activity Log-Ereignisse und die dazugehörigen Details angezeigt (neueste zuerst).

Sie können in den Ergebnissen ablesen, dass mehrere Änderungen am System vorgenommen wurden, z.B. an den Diensten und der Software. Sie können die Filter oben auf der Seite nutzen, um die Ergebnisse nach **Änderungstyp** oder Zeitraum zu filtern.

Wenn Sie eine Änderung vom Typ **WindowsServices** wählen, wird das Fenster mit den **Änderungsdetails** geöffnet. In diesem Fenster werden Details zur Änderung und die Werte vor und nach der Änderung angezeigt. Für diese Instanz wurde der Dienst „Software Protection“ beendet.

![Anzeigen von Details zu den Änderungen im Portal](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Integrieren einer VM für die Änderungsnachverfolgung und den Bestand
> * Durchsuchen von Änderungsprotokollen nach beendeten Diensten
> * Konfigurieren der Änderungsnachverfolgung
> * Aktivieren der Aktivitätsprotokollverbindung
> * Auslösen eines Ereignisses
> * Anzeigen von Änderungen

Fahren Sie mit der Übersicht über die Lösung für die Änderungsnachverfolgung und den Bestand fort, um mehr darüber zu erfahren.

> [!div class="nextstepaction"]
> [Lösung für die Änderungsnachverfolgung und den Bestand](automation-change-tracking.md)

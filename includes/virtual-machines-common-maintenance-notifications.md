
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Anzeigen der virtuellen Computer mit geplanter Wartung im Portal

Nachdem eine Welle für eine geplante Wartung geplant und Benachrichtigungen gesendet wurden, können Sie die Liste mit den virtuellen Computern anzeigen, die von der anstehenden geplanten Wartung betroffen sind. 

Sie können im Azure-Portal nach VMs suchen, für die eine Wartung geplant ist.

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie im linken Navigationsbereich auf **Virtuelle Computer**.

3. Klicken Sie im Bereich mit den virtuellen Computern auf die Schaltfläche **Spalten**, um die Liste mit den verfügbaren Spalten zu öffnen.

4. Wählen Sie die folgenden Spalten aus, und fügen Sie sie hinzu:

   **Wartung**: Enthält den Wartungsstatus für den virtuellen Computer. Mögliche Werte:
      
      | Wert | Beschreibung |
      |-------|-------------|
      | Jetzt starten | Der virtuelle Computer befindet sich im Zeitfenster für die Self-Service-Wartung, in dem Sie die Wartung selbst initiieren können. Informationen zum Starten der Wartung für Ihren virtuellen Computer finden Sie weiter unten. | 
      | Geplant | Die Wartung für den virtuellen Computer ist geplant und kann nicht von Ihnen initiiert werden. Informationen zum Wartungszeitfenster erhalten Sie, indem Sie in dieser Ansicht das Fenster „Automatisch geplant“ auswählen oder auf den virtuellen Computer klicken. | 
      | Abgeschlossen | Sie haben die Wartung für Ihren virtuellen Computer erfolgreich initiiert und abgeschlossen. | 
      | Übersprungen| Sie haben erfolglos versucht, die Wartung zu initiieren. Azure hat die Wartung für Ihren virtuellen Computer abgebrochen und plant die Wartung für einen späteren Zeitpunkt. | 
      | Versuchen Sie es später noch mal.| Sie haben versucht, die Wartung zu initiieren, die Anforderung konnte von Azure jedoch nicht erfüllt werden. In diesem Fall können Sie den Vorgang zu einem späteren Zeitpunkt wiederholen. | 
   
   **Wartung – Proaktives Fenster**: Zeigt das Zeitfenster an, in dem Sie die Wartung für Ihre virtuellen Computer selbst starten können.
   
   **Wartung – automatisch geplantes Fenster**: Zeigt das Zeitfenster an, in dem Azure Ihren virtuellen Computer neu startet, um die Wartung abzuschließen. 




## <a name="notification-and-alerts-in-the-portal"></a>Benachrichtigungen und Warnungen im Portal

Azure sendet eine E-Mail an die Gruppe der Abonnementbesitzer und -mitbesitzer, um sie über den Zeitplan für eine geplante Wartung zu informieren. Sie können Azure-Aktivitätsprotokollwarnungen erstellen, um weitere Empfänger und Kanäle hinzuzufügen. Weitere Informationen finden Sie unter [Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll] (../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im Menü auf der linken Seite auf **Überwachen**. 
3. Klicken Sie im Bereich **Monitor - Activity log** (Überwachen – Aktivitätsprotokoll) auf **Warnungen**.
4. Klicken Sie im Bereich **Monitor - Alerts** (Überwachen – Warnungen) auf **+ Aktivitätsprotokollwarnung hinzufügen**.
5. Geben Sie auf der Seite **Aktivitätsprotokollwarnung hinzufügen** die erforderlichen Informationen an, und legen Sie die folgenden **Kriterien** fest: **Typ**: Wartung; **Status**: Alle (Legen Sie den Status nicht auf „Aktiv“ oder „Aufgelöst“ fest.) **Ebene**: Alle
    
Weitere Informationen zum Konfigurieren von Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md).
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Starten der Wartung für Ihren virtuellen Computer über das Portal

In der Ansicht mit den VM-Details können Sie auch weitere wartungsbezogene Details anzeigen.  
Am oberen Rand der VM-Detailansicht wird ein neues Benachrichtigungsmenüband angezeigt, wenn Ihr virtueller Computer von einer Welle für eine geplante Wartung betroffen ist. Darüber hinaus wird eine neue Option zum Starten der Wartung hinzugefügt (sofern möglich). 


Klicken Sie auf die Wartungsbenachrichtigung, um die Wartungsseite mit weiteren Details zur geplanten Wartung anzuzeigen. Von dort aus können Sie die Wartung**** für Ihren virtuellen Computer starten.

Nach dem Start der Wartung wird Ihr virtueller Computer neu gestartet, und der Wartungsstatus wird innerhalb weniger Minuten mit dem entsprechenden Ergebnis aktualisiert.

Sollten Sie das Zeitfenster verpasst haben, in dem Sie die Wartung starten können, wird das Zeitfenster angezeigt, in dem Ihr virtueller Computer von Azure neu gestartet wird. 

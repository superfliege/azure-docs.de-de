Das Bereitstellungsskript wird in Azure Erstellen der virtuellen Umgebung überspringen, wenn er erkennt, dass eine kompatible virtuelle Umgebung bereits vorhanden ist.  Dies können Sie die Bereitstellung erheblich beschleunigen.  Pakete, die bereits installiert sind, werden über die Pip übersprungen.

In bestimmten Situationen müssen Sie eventuell ein erzwingen dieser virtuellen Umgebung löschen.  Sie sollten dies tun, wenn Sie eine virtuelle Umgebung als Teil Ihrer Repository einschließen möchten.  Möglicherweise möchten Sie auch vorgehen, wenn Sie bestimmte Pakete löschen, oder testen Sie Änderungen an requirements.txt müssen.

Es gibt einige Optionen für die Verwaltung der vorhandenen virtuellen Umgebung in Azure:

### <a name="option-1-use-ftp"></a>Option 1: Verwenden von FTP
Mit einem FTP-Client eine Verbindung mit dem Server herstellen, und beim Löschen des Ordners Env können.  Beachten Sie, dass einige FTP-Clients (z. B. Webbrowser) möglicherweise nur-Lese und es nicht Sie zum Löschen von Ordnern gestattet, damit Sie sicherstellen, dass einen FTP-Client mit dieser Funktion verwenden möchten.  Der FTP-Hostname und die Benutzer in Ihrer Web-app Blatt angezeigt, auf die [Azure-Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Option 2: Zum ein-/ausschalten-Laufzeit
Hier ist eine Alternative, die nutzt die Tatsache, dass das Bereitstellungsskript Env Ordner gelöscht werden, wenn die gewünschte Version von Python keine Übereinstimmung vorliegt.  Dies wird effektiv die vorhandene Umgebung löschen und erstellen Sie eine neue.

1. Wechseln Sie zu einer anderen Version von Python (über runtime.txt oder **Anwendungseinstellungen** Blatt im Azure-Portal)
2. Git-push einige Änderungen (Pip Installation Fehler ignorieren, falls vorhanden)
3. Wechseln Sie zurück zur ersten Version von Python
4. Git push einige Änderungen erneut

### <a name="option-3-customize-deployment-script"></a>Option 3: Anpassen von Bereitstellungsskripts
Wenn Sie das Bereitstellungsskript angepasst haben, können Sie den Code in deploy.cmd gezwungen beim Löschen des Ordners Env ändern.


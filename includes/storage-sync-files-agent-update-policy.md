Updates für den Azure-Dateisynchronisierungs-Agent werden in regelmäßigen Abständen veröffentlicht, um neue Funktionen hinzuzufügen und die gefundenen Probleme zu beheben. Es wird empfohlen, Microsoft Update zu aktivieren, damit Sie alle Updates für den Azure-Dateisynchronisierungs-Agent erhalten, sobald wir sie veröffentlichen. Wir wissen allerdings, dass einige Organisationen Updates genau steuern und testen möchten. Für Bereitstellungen mit älteren Versionen des Azure-Dateisynchronisierungs-Agents:

- Der Speichersynchronisierungsdienst berücksichtigt die vorherige Hauptversion für drei Monate nach der Erstveröffentlichung einer neuen Hauptversion. Beispiel: Version 1.\* wird vom Speichersynchronisierungsdienst bis drei Monate nach der Veröffentlichung von Version 2.\* unterstützt.
- Wenn drei Monate vergangen sind, beginnt der Speichersynchronisierungsdienst damit, registrierte Server, die die abgelaufene Version verwenden, für die Synchronisierung mit ihren Synchronisierungsgruppen zu blockieren.
- Innerhalb der drei Monate für eine vorherige Hauptversion werden alle Fehlerbehebungen nur in der aktuellen Hauptversion implementiert.

> [!Note]  
> Sie werden über eine Popupbenachrichtigung im Azure-Portal benachrichtigt, wenn Sie eine Version von Azure File Sync verwenden, die innerhalb der nächsten drei Monate abläuft.
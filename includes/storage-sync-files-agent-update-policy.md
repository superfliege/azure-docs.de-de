Updates für den Azure-Dateisynchronisierungs-Agent werden in regelmäßigen Abständen veröffentlicht, um neue Funktionen hinzuzufügen und die ermittelten Probleme zu beheben. Es wird empfohlen, Microsoft Update zu konfigurieren, damit Sie alle Updates für den Azure-Dateisynchronisierungs-Agent erhalten, sobald wir sie veröffentlichen. Uns ist bewusst, dass einige Organisationen Updates genau steuern und testen möchten. 

Für Bereitstellungen mit älteren Versionen des Azure-Dateisynchronisierungs-Agents:

- Der Speichersynchronisierungsdienst berücksichtigt die vorherige Hauptversion nach der Erstveröffentlichung einer neuen Hauptversion noch über einen Zeitraum von drei Monaten. Der Speichersynchronisierungsdienst unterstützt Version 1.\* nach der Veröffentlichung von 2.\* beispielsweise noch drei Monate lang.
- Wenn drei Monate vergangen sind, beginnt der Speichersynchronisierungsdienst damit, registrierte Server, die die abgelaufene Version verwenden, für die Synchronisierung mit ihren Synchronisierungsgruppen zu blockieren.
- Innerhalb der drei Monate für eine vorherige Hauptversion werden alle Fehlerbehebungen nur in der aktuellen Hauptversion implementiert.

> [!Note]  
> Sie werden über eine Popupbenachrichtigung im Azure-Portal benachrichtigt, wenn Sie eine Version von Azure File Sync verwenden, die innerhalb der nächsten drei Monate abläuft.
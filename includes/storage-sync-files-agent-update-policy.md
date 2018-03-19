Der Azure File Sync-Agent wird regelmäßig aktualisiert, um neue Funktionalität hinzuzufügen und Probleme zu beheben. Es wird empfohlen, Microsoft Update zu konfigurieren, um Updates für den Azure File Sync-Agent zu erhalten, wenn diese verfügbar sind. Uns ist bewusst, dass einige Organisationen Updates genau steuern und testen möchten.

Für Bereitstellungen mit älteren Versionen des Azure File Sync-Agents:

- Der Speichersynchronisierungsdienst berücksichtigt die vorherige Hauptversion nach der Erstveröffentlichung einer neuen Hauptversion noch über einen Zeitraum von drei Monaten. Beispielsweise unterstützt der Speichersynchronisierungsdienst Version 1.\* nach der Veröffentlichung von Version 2.\* noch drei Monate lang.
- Nach Ablauf der drei Monate blockiert der Speichersynchronisierungsdienst für registrierte Server mit der abgelaufenen Version die Synchronisierung mit den entsprechenden Synchronisierungsgruppen.
- Während der drei Monate, in denen die vorherige Version noch unterstützt wird, fließen alle Fehlerbehebungen nur in die aktuelle (neue) Hauptversion ein.

> [!Note]  
> Wenn Ihre Version von Azure File Sync innerhalb von drei Monaten abläuft, erhalten Sie im Azure-Portal eine entsprechende Popupbenachrichtigung.

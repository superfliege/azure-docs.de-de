```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------| 
|routePrefix|api|Das Routenpräfix, das für alle Routen gilt. Verwenden Sie eine leere Zeichenfolge, um das Standardpräfix zu entfernen. |
|maxOutstandingRequests|-1|Die maximale Anzahl ausstehender Anforderungen, die zu einem beliebigen Zeitpunkt (-1 bedeutet „unbegrenzt“) gespeichert werden. Der Grenzwert umfasst Anforderungen in der Warteschlange, deren Ausführung aber noch nicht gestartet ist, sowie alle laufenden Ausführungen. Alle eingehenden Anforderungen über diesem Grenzwert werden mit der Antwort 429 „Ausgelastet“ zurückgewiesen. Aufrufer können diese Antwort verwenden, um zeitbasierte Wiederholungsstrategien einzusetzen. Diese Einstellung steuert nur das Queuing, das innerhalb des Ausführungspfads des Auftragshosts auftritt. Andere Warteschlangen, z. B. die ASP.NET-Anforderungswarteschlange, sind von dieser Einstellung nicht betroffen. |
|maxConcurrentRequests|-1|Die maximale Anzahl von HTTP-Funktionen, die parallel (-1 bedeutet „unbegrenzt“) ausgeführt werden. Sie könnten z. B. einen Grenzwert festlegen, wenn Ihre HTTP-Funktionen bei hoher Parallelität zu viele Systemressourcen verwenden. Oder wenn Funktionen ausgehende Anforderungen an einen Dienst eines Drittanbieters stellen, muss die Rate dieser Aufrufe möglicherweise eingeschränkt werden.|
|dynamicThrottlesEnabled|false|Führt dazu, dass die Pipeline zur Anforderungsverarbeitung regelmäßig die Systemleistungsindikatoren überprüft. Zu Leistungsindikatoren zählen Verbindungen, Threads, Prozesse, Arbeitsspeicher und CPU. Wenn einer dieser Leistungsindikatoren einen integrierten Schwellenwert übersteigt (80 %), werden Anforderungen mit der Antwort 429 „Ausgelastet“ zurückgewiesen, bis die Indikatoren zur normalen Werten zurückkehrt sind.|

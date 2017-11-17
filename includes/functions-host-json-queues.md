```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------| 
|maxPollingInterval|60000|Das maximale Intervall in Millisekunden zwischen Warteschlangenumfragen.| 
|visibilityTimeout|0|Das Zeitintervall zwischen Wiederholungsversuchen, wenn bei der Verarbeitung einer Nachricht ein Fehler auftritt.| 
|batchSize|16|Die Anzahl der Warteschlangennachrichten, die parallel abgerufen und verarbeitet werden. Der maximale Wert beträgt 32.| 
|maxDequeueCount|5|Die Anzahl der Versuche zum Verarbeiten einer Nachricht, bevor diese in die Warteschlange für nicht verarbeitete Nachrichten verschoben wird.| 
|newBatchThreshold|batchSize/2|Der Schwellenwert, ab dem ein neuer Nachrichtenbatch abgerufen wird.| 

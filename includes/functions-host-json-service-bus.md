```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|maxConcurrentCalls|16|Die maximale Anzahl gleichzeitiger Aufrufe für den Rückruf, der vom Nachrichtensystem initiiert werden soll. Die Functions-Runtime verarbeitet standardmäßig mehrere Nachrichten gleichzeitig. Um die Runtime anzuweisen, jeweils nur eine Warteschlangen- oder Themennachricht zu verarbeiten, legen Sie `maxConcurrentCalls` auf „1“ fest. | 
|prefetchCount|–|Das standardmäßige PrefetchCount, das von dem zugrunde liegenden MessageReceiver verwendet wird.| 
|autoRenewTimeout|00:05:00|Die maximale Zeitspanne, in der die Nachrichtensperre automatisch erneuert wird.| 

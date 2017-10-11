Die Auflistung von benutzerdefinierten Messungen. Verwenden Sie diese Auflistung zum Bericht mit dem Namen Messung der Telemetrie-Element zugeordnet. Typische Anwendungsfälle sind:
- die Größe der Abhängigkeit Telemetrie-Nutzlast
- die Anzahl von Warteschlangenelementen von Telemetriedaten Anforderung verarbeitet
- Zeit, die diesen Kunden, die zum Abschließen des Schritts im Assistenten Schritt Abschluss Ereignis Telemetrie gedauert hat.

Sie können Abfragen [benutzerdefinierte Messungen](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Anwendungsanalyse:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Benutzerdefinierte Messungen sind die Telemetrie-Element, dem sie zu gehören zugeordnet. Sie unterliegen Stichproben ein, mit dem Telemetrie-Element, enthält diese Messungen. Um eine Messung nachzuverfolgen, die einen Wert, der unabhängig von anderen Projekttypen Telemetrie aufweist, verwenden [Metrik Telemetrie](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Max. Anzahl von Schlüssellänge: 150

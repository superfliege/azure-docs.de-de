Es gibt einige Einschränkungen auf die Anzahl von Metriken und-Ereignisse pro Anwendung (d. h. pro instrumentierungsschlüssel). Grenzwerte hängen von der [Preise Plan](https://azure.microsoft.com/pricing/details/application-insights/) , die Sie auswählen.

| **Ressource** | **Standardlimit** | **Hinweis**
| --- | --- | --- |
| Gesamtdaten pro Tag | 500 GB | Sie können Daten verringern, indem Sie eine Obergrenze festlegen. Wenn Sie mehr benötigen, e-Mail- AIDataCap@microsoft.com.
| Kostenlose Daten pro Monat<br/> (Grundpreis Plan) | 1 GB | Zusätzliche Daten werden pro GB abgerechnet.
| Einschränkung | 32 k Ereignisse/Sekunde | Der Grenzwert wird über eine Minute gemessen.
| Datenaufbewahrung | 90 Tage | Diese Ressource dient zum [Suche](../articles/application-insights/app-insights-diagnostic-search.md), [Analytics](../articles/application-insights/app-insights-analytics.md), und [Metriken Explorer](../articles/application-insights/app-insights-metrics-explorer.md).
| [Verfügbarkeit mehrstufiges Test](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) detaillierte Ergebnisse Aufbewahrung | 90 Tage | Diese Ressource bietet detaillierte Ergebnisse der einzelnen Schritte.
| Die maximale Ereignisgröße | 64 KB | 
| Namenslänge für den-Eigenschaft und die Metrik | 150 | Finden Sie unter [geben Schemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Zeichenfolgenlänge des Eigenschaftswerts | 8,192 | Finden Sie unter [geben Schemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Ablaufverfolgung und der Ausnahme Nachrichtenlänge | 10 k | Finden Sie unter [geben Schemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| [Verfügbarkeit Tests](../articles/application-insights/app-insights-monitor-web-app-availability.md) Anzahl pro app  | 10 |
| [Profiler](../articles/application-insights/app-insights-profiler.md) Beibehaltung der Daten | 5 Tage |
| [Profiler](../articles/application-insights/app-insights-profiler.md) gesendeten Daten pro Tag | 10GB |

Weitere Informationen finden Sie unter [zu Preisen und Kontingente in Application Insights](../articles/application-insights/app-insights-pricing.md).


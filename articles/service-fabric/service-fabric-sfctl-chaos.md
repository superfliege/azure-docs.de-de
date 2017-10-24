---
title: "Azure Service Fabric CLI – sfctl chaos | Microsoft-Dokumentation"
description: Beschreibt die sfctl chaos-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: 336e74d8f69cb04e6bd0e85fc68ba38b218fabae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-chaos"></a>sfctl chaos
Ermöglicht es, den Chaos-Testdienst zu starten und zu beenden sowie seine Berichte anzuzeigen.

## <a name="commands"></a>Befehle

|Befehl|Beschreibung|
| --- | --- |
|    report| Ruft das jeweils nächste Segment des Chaos-Berichts basierend auf dem übergebenen Fortsetzungstoken oder dem übergebenen Zeitraum ab.|
|    Start | Startet Chaos mit den angegebenen Chaos-Parametern, wenn Chaos noch nicht im Cluster ausgeführt wird.|
|    stop  | Beendet Chaos im Cluster, wenn es bereits ausgeführt wird. Führt andernfalls keine Aktion aus.|


## <a name="sfctl-chaos-report"></a>sfctl chaos report
Ruft das jeweils nächste Segment des Chaos-Berichts basierend auf dem übergebenen Fortsetzungstoken oder dem übergebenen Zeitraum ab.

Sie können entweder das ContinuationToken zum Abrufen des nächsten Segments des Chaos-Berichts angeben oder den Zeitraum mit StartTimeUtc und EndTimeUtc festlegen. Sie können jedoch nicht im gleichen Aufruf das ContinuationToken und den Zeitraum angeben. Wenn mehr als 100 Chaos-Ereignisse vorhanden sind, wird der Chaos-Bericht in Segmenten zurückgegeben, die jeweils nicht mehr als 100 Chaos-Ereignisse enthalten. 

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --continuation-token| Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen. Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein.|
| --end-time-utc   | Die Anzahl von Takten (Ticks), die die Endzeit des Zeitraums angeben, für den ein Chaos-Bericht generiert werden soll. Ausführliche Informationen zu Ticks finden Sie unter [DateTime.Ticks-Eigenschaft](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29).|
| --start-time-utc | Die Anzahl von Takten (Ticks), die die Startzeit des Zeitraums angeben, für den ein Chaos-Bericht generiert werden soll. Ausführliche Informationen zu Ticks finden Sie unter [DateTime.Ticks-Eigenschaft](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29).|
| --timeout -t     | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug          | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h        | Zeigt diese Hilfemeldung an und beendet.|
| --output -o      | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query          | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose        | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-chaos-start"></a>sfctl chaos start
Startet Chaos mit den angegebenen Chaos-Parametern, wenn Chaos noch nicht im Cluster ausgeführt wird.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --app-type-health-policy-map  | JSON-codierte Liste mit maximalem Prozentsatz fehlerhafter Anwendungen für bestimmte Anwendungstypen. Jeder Eintrag gibt als Schlüssel den Namen des Anwendungstyps und als Wert eine ganze Zahl an, die dem „MaxPercentUnhealthyApplications“-Prozentsatz entspricht, der dazu verwendet wird, die Anwendungen des angegebenen Anwendungstyps zu bewerten.|
| --disable-move-replica-faults | Deaktiviert das Verschieben von primären und sekundären Fehlern .|
| --max-cluster-stabilization| Die maximale Zeitdauer für ein Warten, bis alle Clusterentitäten stabil und fehlerfrei sind.  Standardwert: 60.|
| --max-concurrent-faults    | Die maximale Anzahl von gleichzeitigen Fehlern, die pro Iteration ausgelöst werden.           Standardwert: 1.|
| --max-percent-unhealthy-apps  | Der maximal zulässige Prozentsatz fehlerhafter Anwendungen, bevor ein Fehler gemeldet wird, wenn während Chaos die Clusterintegrität ausgewertet wird.|
| --max-percent-unhealthy-nodes | Der maximal zulässige Prozentsatz fehlerhafter Knoten, bevor ein Fehler gemeldet wird, wenn während Chaos die Clusterintegrität ausgewertet wird.|
| --time-to-run              | Die Gesamtzeit (in Sekunden), während der Chaos ausgeführt wird, bevor es automatisch beendet wird. Der maximal zulässige Wert ist „4.294.967.295“ (System.UInt32.MaxValue).  Standardwert: 4294967295.|
| --timeout -t               | Servertimeout in Sekunden.  Standardwert: 60.|
| --wait-time-between-faults | Wartezeit (in Sekunden) zwischen aufeinander folgenden Fehlern innerhalb einer einzelnen Iteration.  Standardwert: 20.|
| --wait-time-between-iterations| Zeitabstand (in Sekunden) zwischen zwei aufeinander folgenden Iterationen von Chaos.  Standardwert: 30.|
| --warning-as-error         | Wird während Chaos die Clusterintegrität ausgewertet, sollen Warnungen mit demselben Schweregrad berücksichtigt werden wie Fehler.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                    | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                  | Zeigt diese Hilfemeldung an und beendet.|
| --output -o                | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.           Standardwert: json.|
| --query                    | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose                  | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-chaos-stop"></a>sfctl chaos stop
Beendet Chaos im Cluster, wenn es bereits ausgeführt wird. Führt andernfalls keine Aktion aus.

Bewirkt, dass Chaos keine weiteren Fehler plant. Wirkt sich nicht auf die aktuell wirksamen Fehler aus.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --timeout -t| Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug  | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h| Zeigt diese Hilfemeldung an und beendet.|
| --output -o | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query  | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose| Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)
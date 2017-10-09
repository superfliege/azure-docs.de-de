---
title: "Azure Service Fabric CLI – sfctl replica | Microsoft-Dokumentation"
description: Beschreibt die sfctl replica-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.translationtype: HT
ms.sourcegitcommit: cb9130243bdc94ce58d6dfec3b96eb963cdaafb0
ms.openlocfilehash: 50d185294131e42aaf8b541ab17495fcb40c89dd
ms.contentlocale: de-de
ms.lasthandoff: 09/26/2017

---
# <a name="sfctl-replica"></a>sfctl replica
Ermöglicht es, Replikate zu verwalten, die zu Servicepartitionen gehören.

## <a name="commands"></a>Befehle

|Befehl|Beschreibung|
| --- | --- |
|    deployed  | Ruft die Details eines Replikats ab, das auf einem Service Fabric-Knoten bereitgestellt wird.|
|    deployed-list| Ruft die Liste der Replikate ab, die auf einem Service Fabric-Knoten bereitgestellt werden.|
|    health    | Ruft die Integrität eines zustandsbehafteten Service Fabric-Dienstreplikats oder einer zustandslosen
                   Service Fabric-Dienstinstanz ab.|
|    info      | Ruft die Informationen zu einem Replikat einer Service Fabric-Partition ab.|
|    list      | Ruft die Informationen zu Replikaten einer Service Fabric-Dienstpartition ab.|
|    remove    | Entfernt ein Dienstreplikat, das auf einem Knoten ausgeführt wird.|
|    report-health| Sendet einen Integritätsbericht zu dem Service Fabric-Replikat.|
|    restart   | Startet ein Dienstreplikat eines permanenten Diensts neu, das auf einem Knoten ausgeführt wird.|


## <a name="sfctl-replica-deployed"></a>sfctl replica deployed
Ruft die Details eines Replikats ab, das auf einem Service Fabric-Knoten bereitgestellt wird.

Ruft die Details des Replikats ab, das auf einem Service Fabric-Knoten bereitgestellt wird. Die Informationen umfassen Dienstart, Dienstname, aktueller Dienstvorgang, Startzeitpunkt des aktuellen Dienstvorgangs, Partitions-ID, Replikat/Instanz-ID, gemeldete Last und weitere Informationen.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --node-name [erforderlich]| Der Name des Knotens.|
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --replica-id [erforderlich]| Der Bezeichner (ID) des Replikats.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-replica-health"></a>sfctl replica health
Ruft die Integrität eines zustandsbehafteten Service Fabric-Dienstreplikats oder einer zustandslosen Service Fabric-Dienstinstanz ab.

Ruft die Integrität eines Service Fabric-Replikats ab. Verwenden Sie „EventsHealthStateFilter“, um die Sammlung der Integritätsereignisse (HealthEvents), die für das Replikat gemeldet wurden, anhand des Integritätsstatus zu filtern. .

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --replica-id [erforderlich]| Der Bezeichner (ID) des Replikats.|
| --events-health-state-filter| Ermöglicht das Filtern der Sammlung zurückgegebener HealthEvent-Objekte anhand des Integritätsstatus. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus. Es werden nur Ereignisse zurückgegeben, die dem Filter entsprechen. Alle Ereignisse werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden alle Ereignisse zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein. Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat. Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“. – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben. Der Wert ist gleich „65535“.|
| --timeout -t             | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                  | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                | Zeigt diese Hilfemeldung an und beendet.|
| --output -o              | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query                  | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose                | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-replica-info"></a>sfctl replica info
Ruft die Informationen zu einem Replikat einer Service Fabric-Partition ab.

Die Antwort enthält die ID, die Rolle, den Status, die Integrität, den Knotennamen, die Betriebszeit und andere Details zu dem Replikat.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --replica-id [erforderlich]| Der Bezeichner (ID) des Replikats.|
| --continuation-token  | Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen. Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-replica-list"></a>sfctl replica list
Ruft die Informationen zu Replikaten einer Service Fabric-Dienstpartition ab.

Der GetReplicas-Endpunkt gibt Informationen zu den Replikaten der angegebenen Partition zurück.
Die Antwort enthält die ID, die Rolle, den Status, die Integrität, den Knotennamen, die Betriebszeit und andere Details zu dem Replikat.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --continuation-token  | Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen. Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-replica-remove"></a>sfctl replica remove
Entfernt ein Dienstreplikat, das auf einem Knoten ausgeführt wird.

Diese API simuliert einen Service Fabric-Replikatfehler durch Entfernen eines Replikats aus einem Service Fabric-Cluster. Beim Entfernen wird das Replikat geschlossen und anschließend in die Rolle „None“ überführt, und dann werden alle Statusinformationen des Replikats aus dem Cluster entfernt. Diese API testet den Pfad für ein Entfernen des Replikatstatus und simuliert den dauerhaften Pfad für Berichten von Fehlern über Client-APIs. Warnung: Es werden keine Sicherheitsüberprüfungen ausgeführt, wenn diese API verwendet wird. Eine falsche Verwendung dieser API kann zu Datenverlusten für zustandsbehaftete Dienste führen. Darüber hinaus wirkt sich das Flag „forceRemove“ auf alle anderen Replikate aus, die im selben Prozess gehostet werden.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --node-name [erforderlich]| Der Name des Knotens.|
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --replica-id [erforderlich]| Der Bezeichner (ID) des Replikats.|
| --force-remove        | Erzwingt ein Entfernen einer Service Fabric-Anwendung oder eines Service Fabric-Diensts, ohne die Sequenz für ordnungsgemäßes Herunterfahren zu durchlaufen. Dieser Parameter kann für ein erzwungenes Löschen einer Anwendung oder eines Diensts verwendet werden, für die oder den beim Löschen ein Timeout aufgetreten ist, weil Probleme im Dienstcode ein ordnungsgemäßes Schließen von Replikaten verhindern.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-replica-restart"></a>sfctl replica restart
Startet ein Dienstreplikat eines permanenten Diensts neu, das auf einem Knoten ausgeführt wird.

Startet ein Dienstreplikat eines permanenten Diensts neu, das auf einem Knoten ausgeführt wird. Warnung: Es werden keine Sicherheitsüberprüfungen ausgeführt, wenn diese API verwendet wird. Eine falsche Verwendung dieser API kann zum Verlust der Verfügbarkeit von zustandsbehafteten Diensten führen.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --node-name [erforderlich]| Der Name des Knotens.|
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --replica-id [erforderlich]| Der Bezeichner (ID) des Replikats.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)

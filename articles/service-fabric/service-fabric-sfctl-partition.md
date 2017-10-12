---
title: "Azure Service Fabric CLI – sfctl partition | Microsoft-Dokumentation"
description: Beschreibt die sfctl partition-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.openlocfilehash: a5d0ff59803212403281063f47e706433cee64b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-partition"></a>sfctl partition
Ermöglicht es, Partitionen für jeden Dienst abzufragen und zu verwalten.

## <a name="commands"></a>Befehle

|Befehl|Beschreibung|
| --- | --- |
|    data-loss      | Diese API verursacht Datenverlust für die angegebene Partition.|
|    data-loss-status  | Ruft den Status eines Partitionsdatenverlustvorgangs ab, der über die StartDataLoss-API gestartet wurde.|
|    health         | Ruft die Integrität der angegebenen Service Fabric-Partition ab.|
|    info           | Ruft die Informationen zu einer Service Fabric-Partition ab.|
|    list           | Ruft die Liste der Partitionen eines Service Fabric-Diensts ab.|
|    load           | Ruft die Last der angegebenen Service Fabric-Partition ab.|
|    load-reset     | Setzt die aktuelle Last einer Service Fabric-Partition zurück.|
|    quorum-loss    | Löst einen Quorumverlust für eine bestimmte zustandsbehaftete Dienstpartition aus.|
|    quorum-loss-status| Ruft für eine Partition den Status eines Quorumverlustvorgangs ab, der über die StartQuorumLoss-API gestartet wurde.|
|    recover        | Veranlasst den Service Fabric-Cluster zu versuchen, eine bestimmte Partition wiederherzustellen, die derzeit in Quorumverlust gebunden ist.|
|    recover-all    | Veranlasst den Service Fabric-Cluster zu versuchen, alle Dienste (einschließlich der Systemdienste) wiederherzustellen, die derzeit in Quorumverlust gebunden sind.|
|    report-health  | Sendet einen Integritätsbericht zu der Service Fabric-Partition.|
|    restart        | Diese API startet einige oder alle Replikate oder Instanzen der angegebenen Partition neu.|
|    restart-status | Ruft den Status eines Partitionsneustartvorgangs ab, der über „StartPartitionRestart“ gestartet wurde.|
|    svc-name       | Ruft den Namen des Service Fabric-Diensts für eine Partition ab.|


## <a name="sfctl-partition-health"></a>sfctl partition health
Ruft die Integrität der angegebenen Service Fabric-Partition ab.

Ruft die Integritätsinformationen der angegebenen Partition ab. Verwenden Sie „EventsHealthStateFilter“, um die Sammlung der Integritätsereignisse (HealthEvents), die für den Dienst gemeldet wurden, anhand des Integritätsstatus zu filtern.
Verwenden Sie „ReplicasHealthStateFilter“, um die Auflistung der ReplicaHealthState-Objekte für die Partition zu filtern. Geben Sie eine Partition an, die im Integritätsspeicher nicht vorhanden ist, gibt dieses Cmdlet einen Fehler zurück. .

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --events-health-state-filter  | Ermöglicht das Filtern der Sammlung zurückgegebener HealthEvent-Objekte anhand des Integritätsstatus. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus.                Es werden nur Ereignisse zurückgegeben, die dem Filter entsprechen. Alle Ereignisse werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden alle Ereignisse zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein. Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat. Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“.                – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben.                Der Wert ist gleich „65535“.|
|--exclude-health-statistics   | Gibt an, ob die Integritätsstatistiken als Bestandteil des Abfrageergebnisses zurückgegeben werden sollen. Der Standardwert ist gleich „False“. Die Statistiken zeigen die Anzahl von untergeordneten Entitäten, die einen der Integritätsstatus „Ok“, „Warning“ oder „Error“ haben.|
| --replicas-health-state-filter| Ermöglicht ein Filtern der Auflistung der ReplicaHealthState-Objekte für die Partition. Der Wert kann aus Membern oder bitweisen Operationen für Member von „HealthStateFilter“ abgerufen werden. Es werden nur Replikate zurückgegeben, die dem Filter entsprechen. Alle Replikate werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden alle Ereignisse zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein. Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat. Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“. – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben. Der Wert ist gleich „65535“.|
| --timeout -t               | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                    | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                  | Zeigt diese Hilfemeldung an und beendet.|
| --output -o                | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.                Standardwert: json.|
| --query                    | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“. |
| --verbose                  | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-partition-info"></a>sfctl partition info
Ruft die Informationen zu einer Service Fabric-Partition ab.

Der Partitionenendpunkt gibt Informationen über die angegebene Partition zurück. Die Antwort enthält die Partitions-ID, die Partitionierungsschemainformationen, die Schlüssel, die von der Partition unterstützt werden, den Status, die Integrität und weitere Details zur Partition.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-partition-list"></a>sfctl partition list
Ruft die Liste der Partitionen eines Service Fabric-Diensts ab.

Ruft die Liste der Partitionen eines Service Fabric-Diensts ab. Die Antwort enthält die Partitions-ID, die Partitionierungsschemainformationen, die Schlüssel, die von der Partition unterstützt werden, den Status, die Integrität und weitere Details zur Partition.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --continuation-token| Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen.         Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein.|
| --timeout -t        | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug             | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h           | Zeigt diese Hilfemeldung an und beendet.|
| --output -o         | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query             | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose           | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-partition-load"></a>sfctl partition load
Ruft die Last der angegebenen Service Fabric-Partition ab.

Gibt Informationen über die angegebene Partition zurück. Die Antwort enthält eine Liste mit Lastinformationen. Jeder Informationseintrag enthält den Namen und den Wert der Lastmetrik sowie die zuletzt gemeldete Zeit in UTC. .

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-partition-recover"></a>sfctl partition recover
Veranlasst den Service Fabric-Cluster zu versuchen, eine bestimmte Partition wiederherzustellen, die derzeit in Quorumverlust gebunden ist.

Veranlasst den Service Fabric-Cluster zu versuchen, eine bestimmte Partition wiederherzustellen, die derzeit in Quorumverlust gebunden ist. Dieser Vorgang sollte nur ausgeführt werden, wenn bekannt ist, dass die Replikate, die ausgefallen sind, nicht wiederhergestellt werden können. Eine falsche Verwendung dieser API kann dazu führen, dass Daten verloren gehen.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-partition-restart"></a>sfctl partition restart
Diese API startet einige oder alle Replikate oder Instanzen der angegebenen Partition neu.

Diese API ist hilfreich zum Testen von Failover. Wird diese API für eine zustandslose Dienstpartition verwendet, muss „RestartPartitionMode“ gleich „AllReplicasOrInstances“ sein. Rufen Sie die GetPartitionRestartProgress-API mit der Vorgangs-ID (OperationId) auf, mit der Sie den Status (Fortschritt) abgerufen haben. .

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --operation-id [erforderlich]| Eine GUID, die einen Aufruf dieser API kennzeichnet.  Diese GUID wird in die entsprechende GetProgress-API übergeben.|
| --partition-id [erforderlich]| Die Identität (ID) der Partition.|
| --restart-partition-mode [erforderlich]| – Invalid: Reserviert.  Übergeben Sie diesen Wert nicht an die API. – AllReplicasOrInstances: Alle Replikate oder Instanzen in der Partition werden gleichzeitig neu gestartet. – OnlyActiveSecondaries: Nur die sekundären Replikate werden neu gestartet. .|
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --timeout -t                    | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                         | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                       | Zeigt diese Hilfemeldung an und beendet.|
| --output -o                     | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.                     Standardwert: json.|
| --query                         | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose                       | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)
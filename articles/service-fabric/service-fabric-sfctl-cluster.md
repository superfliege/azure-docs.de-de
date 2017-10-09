---
title: "Azure Service Fabric CLI – sfctl cluster | Microsoft-Dokumentation"
description: Beschreibt die sfctl cluster-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.openlocfilehash: dc2ed59d6adaca97b23dddcb7ec968d90171b483
ms.contentlocale: de-de
ms.lasthandoff: 09/26/2017

---
# <a name="sfctl-cluster"></a>sfctl cluster
Ermöglicht es, Service Fabric-Cluster auszuwählen, zu verwalten und zu betreiben.

## <a name="commands"></a>Befehle

|Befehl|Beschreibung|
| --- | --- |
|    code-versions| Ruft eine Liste der Fabric-Codeversionen ab, die in einem Service Fabric-Cluster bereitgestellt werden.|
|    config-versions | Ruft eine Liste der Fabric-Konfigurationsversionen ab, die in einem Service Fabric-Cluster bereitgestellt werden.|
|    health       | Ruft die Integrität eines Service Fabric-Clusters ab.|
|    manifest     | Ruft das Service Fabric-Clustermanifest ab.|
|    operation-cancel| Bricht einen von einem Benutzer veranlassten Fehlervorgang ab.|
|    operationgit | Ruft eine Liste der von Benutzern veranlassten Fehlervorgänge ab, die nach der bereitgestellten Eingabe gefiltert sind.|
|    provision     | Stellt die Code- oder Konfigurationspakete eines Service Fabric-Clusters bereit.|
|    recover-system  | Veranlasst den Service Fabric-Cluster zu versuchen, die Systemdienste wiederherzustellen, die derzeit in Quorumverlust gebunden sind.|
|report-health   | Sendet einen Integritätsbericht zu dem Service Fabric-Cluster.|
|    select       | Stellt eine Verbindung mit einem Service Fabric-Clusterendpunkt her.|
| unprovision     | Hebt die Bereitstellung der Code- oder Konfigurationspakete eines Service Fabric-Clusters auf.|
|    Upgrade         | Startet ein Upgraden der Code- oder Konfigurationsversion eines Service Fabric-Clusters.|
|    upgrade-resume  | Bewirkt, dass das Clusterupgrade zur nächsten Upgradedomäne wechselt.|
|    upgrade-rollback| Führt einen Rollback für das Upgrade eines Service Fabric-Cluster aus.|
|    upgrade-status  | Ruft den Status des aktuellen Clusterupgrade ab.|
|upgrade-update  | Aktualisiert die Upgradeparameter eines Service Fabric-Clusterupgrades.|


## <a name="sfctl-cluster-health"></a>sfctl cluster health
Ruft die Integrität eines Service Fabric-Clusters ab.

Ruft die Integrität eines Service Fabric-Clusters ab. Verwenden Sie „EventsHealthStateFilter“, um die Sammlung der Integritätsereignisse (HealthEvents), die für den Cluster gemeldet wurden, anhand des Integritätsstatus zu filtern. In ähnlicher Weise können Sie „NodesHealthStateFilter“ und „ApplicationsHealthStateFilter“ verwenden, um die Auflistung der zurückgegebenen Knoten und Anwendungen anhand ihres aggregierten Integritätsstatus zu filtern. 

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --applications-health-state-filter| Ermöglicht ein Filtern der Integritätsstatus von Anwendungen.
                                                    objects returned in the result of cluster health
                                                    query based on their health state. The possible
                                                    values for this parameter include integer value
                                                    obtained from members or bitwise operations on
                                                    members of HealthStateFilter enumeration. Only
                                                    applications that match the filter are returned.
                                                    All applications are used to evaluate the
                                                    aggregated health state. If not specified, all
                                                    entries are returned. The state values are flag
                                                    based enumeration, so the value could be a
                                                    combination of these values obtained using
                                                    bitwise 'OR' operator. For example, if the
                                                    provided value is 6 then health state of
                                                    applications with HealthState value of OK (2)
                                                    and Warning (4) are returned. - Default -
                                                    Default value. Matches any HealthState. The
                                                    value is zero. - None - Filter that doesn't
                                                    match any HealthState value. Used in order to
                                                    return no results on a given collection of
                                                    states. The value is 1. - Ok - Filter that
                                                    matches input with HealthState value Ok. The
                                                    value is 2. - Warning - Filter that matches
                                                    input with HealthState value Warning. The value
                                                    is 4. - Error - Filter that matches input with
                                                    HealthState value Error. The value is 8. - All -
                                                    Filter that matches input with any HealthState value. The value is 65535.|
| – events-health-state-filter | Ermöglicht das Filtern der Sammlung zurückgegebener HealthEvent-Objekte anhand des Integritätsstatus. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus. Es werden nur Ereignisse zurückgegeben, die dem Filter entsprechen. Alle Ereignisse werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden alle Ereignisse zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein. Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat.
Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“. – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben. Der Wert ist gleich „65535“.| |--exclude-health-statistics | Gibt an, ob die Integritätsstatistiken als Bestandteil des Abfrageergebnisses zurückgegeben werden sollen. Der Standardwert ist gleich „False“. Die Statistiken zeigen die Anzahl von untergeordneten Entitäten, die einen der Integritätsstatus „Ok“, „Warning“ oder „Error“ haben.| | --include-system-application-health-statistics| Gibt an, ob die Integritätsstatistiken die Integritätsstatistiken der Systemanwendung von Fabric (fabric:/System) enthalten sollen. Der Standardwert ist gleich „False“. Ist „IncludeSystemApplicationHealthStatistics“ auf „true“ festgelegt, enthalten die Integritätsstatistiken die Entitäten, die zur Systemanwendung von Fabric gehören. Andernfalls enthält das Abfrageergebnis nur Integritätsstatistiken für Benutzeranwendungen. Die Integritätsstatistiken müssen im Abfrageergebnis enthalten sein, damit dieser Parameter angewendet wird. | | --nodes-health-state-filter | Ermöglicht ein Filtern der Knotenintegritätsstatusobjekte, die im Ergebnis der Clusterintegritätsabfrage zurückgegeben wurden, anhand ihres Integritätsstaus. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus. Es werden nur Knoten zurückgegeben, die dem Filter entsprechen. Alle Knoten werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden Integritätsstatus der Knoten zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein. Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat.
Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“. – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben. Der Wert ist gleich „65535“.| | --timeout -t | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                        | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                      | Zeigt diese Hilfemeldung an und beendet.|
| --output -o                    | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.                    Standardwert: json.|
| --query                        | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose                      | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-cluster-manifest"></a>sfctl cluster manifest
Ruft das Service Fabric-Clustermanifest ab.

Ruft das Service Fabric-Clustermanifest ab. Das Clustermanifest enthält Eigenschaften des Clusters, die unterschiedliche Knotentypen im Cluster, Sicherheitskonfigurationen, Fehler- und Upgradedomänentopologien usw. enthalten. Diese Eigenschaften werden als Teil der Datei „clusterconfig.JSON“ angegeben, während ein eigenständiger Cluster bereitgestellt wird. Die meisten Informationen im Clustermanifest werden jedoch intern von Fabric Service während der Clusterbereitstellung in anderen Bereitstellungsszenarios generiert (z. B. bei Verwendung des [Azure-Portals](https://portal.azure.com)). Der Inhalt des Clustermanifests ist nur zu Informationszwecken vorgesehen, und es wird davon ausgegangen, dass es für Benutzer keinerlei Abhängigkeit vom Format des Dateiinhalts oder dessen Interpretation gibt. 

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

## <a name="sfctl-cluster-provision"></a>sfctl cluster provision
Stellt die Code- oder Konfigurationspakete eines Service Fabric-Clusters bereit.

        Validate and provision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
|--cluster-manifest-file-path| Der Dateipfad des Clustermanifests.|
|    --code-file-path            | Der Dateipfad des Clustercodepakets.|
|    --timeout -t                | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h  | Zeigt diese Hilfemeldung an und beendet.|
| --output -o| Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose  | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-cluster-select"></a>sfctl cluster select
Stellt eine Verbindung mit einem Service Fabric-Clusterendpunkt her.

Wird eine Verbindung mit einem sicheren Cluster hergestellt, geben Sie eine Zertifikat- (CRT) und eine Schlüsseldatei (KEY) oder eine einzelne Datei mit beiden (PEM) an. Geben Sie nicht beides an. Optional können Sie in diesem Fall auch einen Pfad zu einer Bundledatei einer Zertifizierungsstelle oder zu einem Verzeichnis mit Zertifikaten einer vertrauenswürdigen Zertifizierungsstelle angeben.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --endpoint [erforderlich]| Clusterendpunkt-URL einschließlich Port und HTTP- oder HTTPS-Präfix.|
| --aad             | Die Authentifizierung erfolgt über Azure Active Directory.|
| --ca              | Pfad zu dem Verzeichnis mit Zertifikaten einer Zertifizierungsstelle, das als gültige Bundledatei oder Bundledatei einer Zertifizierungsstelle behandelt werden soll.|
| --cert            | Pfad zu einer Clientzertifikatdatei.|
| --key             | Pfad zur Schlüsseldatei eines Clientzertifikats.|
| --no-verify       | Die Überprüfung auf Zertifikate wird deaktiviert, wenn HTTPS verwendet wird. Da dies eine unsichere Option ist, sollte sie nicht für Produktionsumgebungen verwendet werden.|
| --pem             | Pfad zu einem Clientzertifikat, das als PEM-Datei angegeben ist.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug           | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h         | Zeigt diese Hilfemeldung an und beendet.|
| --output -o       | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query           | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose         | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-cluster-unprovision"></a>sfctl cluster unprovision
Hebt die Bereitstellung der Code- oder Konfigurationspakete eines Service Fabric-Clusters auf.

        Unprovision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Argumente
|Argument|Beschreibung|
| --- | --- |
|--code-version  | Die Version des Clustercodepakets.|
|    --config-version| Die Version des Clustermanifests.|
|    --timeout -t    | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente
|Argument|Beschreibung|
| --- | --- |
|--debug         | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
 |   --help -h       | Zeigt diese Hilfemeldung an und beendet.|
 |   --output -o     | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
 |   --query         | JMESPath-Abfragezeichenfolge. Unter „http://jmespath.org/“ finden Sie weitere Informationen und
                      Beispiele.|
 |   --verbose       | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|


## <a name="sfctl-cluster-upgrade"></a>sfctl cluster upgrade
Startet ein Upgraden der Code- oder Konfigurationsversion eines Service Fabric-Clusters.

        Validate the supplied upgrade parameters and start upgrading the code or configuration
        version of a Service Fabric cluster if the parameters are valid.

### <a name="arguments"></a>Argumente
|Argument|Beschreibung|
| --- | --- |
|    --app-health-map                      | JSON-codiertes Wörterbuch mit Paaren aus Anwendungsname und
                                            maximalem Prozentsatz für „fehlerhaft“, bevor ein Fehler ausgelöst wird.|
 |   --app-type-health-map                 | JSON-codiertes Wörterbuch mit Paaren aus Anwendungstyp.
                                            name and maximum percentage unhealthy before raising
                                            error.|
 | --code-version | Die Version des Clustercodes.| | --config-version | Die Version der Clusterkonfiguration.| | --delta-health-evaluation | Ermöglicht Delta-Integritätsbewertung statt absoluter Integritätsbewertung nach Abschluss jeder Upgradedomäne.| | --delta-unhealthy-nodes | Der während Clusterupgrades maximal zulässige Prozentsatz für eine Integritätsminderung von Knoten.  Standardwert: 10
Das Delta wird zwischen den Status der Knoten am Anfang des Upgrades und den Status der Knoten zum Zeitpunkt der Integritätsbewertung gemessen. Die Überprüfung wird ausgeführt, sobald ein Upgrade einer Upgradedomäne abgeschlossen ist. So wird sichergestellt, dass der globale Status des Clusters innerhalb der zulässigen Grenzwerte liegt.| |   --failure-action                      | Folgende Werte sind möglich: „Invalid“, „Rollback“, „Manual“.| |   --force-restart                       | Neustart erzwingen.| |   --health-check-retry                  | Timeout für Wiederholung von Integritätsprüfung, gemessen in Millisekunden.| |   --health-check-stable                 | Stabile Dauer für Integritätsprüfung, gemessen in Millisekunden.| |  --health-check-wait                   | Wartezeit für Integritätsprüfung, gemessen in Millisekunden.| |  --replica-set-check-timeout           | Timeout für Upgradeprüfung von Replikatgruppe, gemessen in Sekunden.| |   --rolling-upgrade-mode                | Folgende Werte sind möglich: „Invalid“, „UnmonitoredAuto“, „UnmonitoredManual“, „Monitored“.  Standardwert: UnmonitoredAuto.| | --timeout -t | Servertimeout in Sekunden.  Standardwert: 60.| |  --unhealthy-applications              | Der maximal zulässige Prozentsatz fehlerhafter Anwendungen, bevor ein Fehler gemeldet wird.
Soll es z. B.zulässig sein, dass 10 % der Anwendungen fehlerhaft sind, muss dieser Wert gleich „10“ sein. Der Prozentsatz entspricht dem maximalen tolerierten Prozentsatz an Anwendungen, die fehlerhaft sein können, bevor der Cluster als fehlerhaft behandelt wird. Wird der Prozentsatz eingehalten, gibt es aber mindestens eine fehlerhafte Anwendung, wird die Integrität als „Warning“ ausgewertet. Dies wird berechnet, indem die Anzahl von fehlerhaften Anwendungen durch die Gesamtanzahl von Anwendungsinstanzen im Cluster dividiert wird, wobei Anwendungen mit Anwendungstypen ausgeschlossen werden, die in „ApplicationTypeHealthPolicyMap“ enthalten sind. Bei der Berechnung wird aufgerundet, um einen Fehler bei einer kleinen Anzahl von Anwendungen zu tolerieren.| |   --unhealthy-nodes                     | Der maximal zulässige Prozentsatz fehlerhafter Knoten, bevor ein Fehler gemeldet.
Soll es z. B.zulässig sein, dass 10 % der Knoten fehlerhaft sind, muss dieser Wert gleich „10“ sein. Der Prozentsatz entspricht dem maximalen tolerierten Prozentsatz an Knoten, die fehlerhaft sein können, bevor der Cluster als fehlerhaft behandelt wird. Wird der Prozentsatz eingehalten, gibt es aber mindestens einen fehlerhaften Knoten, wird die Integrität als „Warning“ ausgewertet. Der Prozentsatz wird berechnet, indem die Anzahl von fehlerhaften Knoten durch die Gesamtanzahl von Knoten im Cluster dividiert wird. Die Berechnung wird aufgerundet, um einen Fehler auf einer kleinen Anzahl von Knoten zu tolerieren. In großen Clustern sind einige Knoten immer heruntergefahren oder für Reparaturen außer Betrieb. Daher sollte dieser Prozentsatz so konfiguriert sein, dass dies berücksichtigt ist.| | --upgrade-domain-delta-unhealthy-nodes| Der während eines Clusterupgrades maximal zulässige Prozentsatz für die Integritätsminderung von Upgradedomänenknoten.
Stadardwert: 15.
Das Delta wird zwischen den Status der Upgradedomänenknoten am Anfang des Upgrades und den Status der Upgradedomänenknoten zum Zeitpunkt der Integritätsbewertung gemessen. Die Überprüfung wird ausgeführt, sobald ein Upgrade einer Upgradedomäne abgeschlossen ist. So wird sichergestellt, dass der globale Status des Clusters innerhalb der zulässigen Grenzwerte liegt.| |   --upgrade-domain-timeout              | Timeout für eine Upgradedomäne, gemessen in Millisekunden.| |   --upgrade-timeout                     | Upgradetimeout, gemessen in Millisekunden.| |   --warning-as-error                    | Warnungen werden mit demselben Schweregrad berücksichtigt wie Fehler.|

### <a name="global-arguments"></a>Globale Argumente
    |Argument|Beschreibung|
| --- | --- |
|--debug                               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
|    --help -h                             | Zeigt diese Hilfemeldung an und beendet.|
|    --output -o                           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.
                                            Standardwert: json.|
|    --query                               | JMESPath-Abfragezeichenfolge. Unter „http://jmespath.org/“ finden Sie weitere
                                            Informationen und Beispiele.|
|    --verbose                             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige
                                            Debugprotokolle wünschen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)

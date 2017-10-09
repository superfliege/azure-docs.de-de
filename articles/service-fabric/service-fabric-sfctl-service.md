---
title: "Azure Service Fabric CLI – sfctl service | Microsoft-Dokumentation"
description: Beschreibt die sfctl service-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.sourcegitcommit: 469246d6cb64d6aaf995ef3b7c4070f8d24372b1
ms.openlocfilehash: 66649bb6ae317eb227dcdf45aa084905967c117f
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="sfctl-service"></a>sfctl service
Ermöglicht es, Dienste, Diensttypen und Dienstpakete zu erstellen, zu löschen und zu verwalten.

## <a name="commands"></a>Befehle

|Befehl|Beschreibung|
| --- | --- |
|    app-name       | Ruft den Namen der Service Fabric-Anwendung für einen Dienst ab.|
|    code-package-list | Ruft die Liste der Codepakete ab, die auf einem Service Fabric-Knoten bereitgestellt werden.|
|    create         | Erstellt den angegebenen Service Fabric-Dienst aus der Beschreibung.|
|    delete         | Löscht einen vorhandene Service Fabric-Dienst.|
|    deployed-type  | Ruft die Informationen zu einem angegebenen Diensttyp der Anwendung ab, die auf einem Knoten in einem Service Fabric-Cluster bereitgestellt wird.|
|    deployed-type-list| Ruft die Liste ab, die die Informationen zu den Diensttypen der Anwendungen enthält, die auf einem Knoten in einem Service Fabric-Cluster bereitgestellt werden.|
|    Beschreibung    | Ruft die Beschreibung eines vorhandenen Service Fabric-Diensts ab.|
|    health         | Ruft die Integrität des angegebenen Service Fabric-Diensts ab.|
|    info           | Ruft die Informationen zu dem speziellen Dienst ab, der zu einer Service Fabric-Anwendung gehört.|
|    list           | Ruft die Informationen zu allen Diensten ab, die zu der Anwendung gehören, die durch die Anwendungs-ID angegeben ist.|
|    manifest       | Ruft das Manifest ab, in dem ein Diensttyp beschrieben ist.|
|    package-deploy | Lädt Pakete, die dem angegebenen Dienstmanifest zugeordnet sind, in den Imagecache auf dem angegebenen Knoten herunter.|
|    package-health | Ruft die Informationen über die Integrität eines Dienstpakets für eine bestimmte Anwendung ab, die für einen Service Fabric-Knoten und eine Service Fabric-Anwendung bereitgestellt wird.|
|    package-info   | Ruft die Liste von Dienstpaketen ab, die auf einem Service Fabric-Knoten bereitgestellt werden und genau mit dem angegebenen Namen übereinstimmen.|
|    package-list   | Ruft die Liste der Dienstpakete ab, die auf einem Service Fabric-Knoten bereitgestellt werden.|
|    recover        | Veranlasst den Service Fabric-Cluster zu versuchen, den angegebenen Dienst wiederherzustellen, der derzeit in Quorumverlust gebunden ist.|
|    report-health  | Sendet einen Integritätsbericht zu dem Service Fabric-Dienst.|
|    resolve        | Löst eine Service Fabric-Partition auf.|
|    type-list      | Ruft die Liste ab, die die Informationen zu den Diensttypen enthält, die von einem bereitgestellten Anwendungstyp in einem Service Fabric-Cluster unterstützt werden.|
|    aktualisieren         | Aktualisiert den angegebenen Dienst mithilfe der angegebenen Updatebeschreibung.|


## <a name="sfctl-service-create"></a>sfctl service create
Erstellt den angegebenen Service Fabric-Dienst aus der Beschreibung.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --app-id [erforderlich]| Die Identität (ID) der übergeordneten Anwendung. Dies ist üblicherweise die vollständige ID der Anwendung ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat eine Anwendung beispielsweise den Namen „fabric://meineapp/app1“, hat die Anwendungsidentität in 6.0+ den Wert „meineapp~app1“ und in früheren Versionen den Wert „meineapp/app1“.|
| --name [erforderlich]| Der Name des Diensts. Dies muss ein untergeordnetes Element der Anwendungs-ID sein.           Dies ist der vollständige Name einschließlich des URI `fabric:`. Beispielsweise ist der Dienst `fabric:/A/B` ein untergeordnetes Element der Anwendung `fabric:/A`.|
| --service-type [erforderlich]| Den Name des Diensttyps.|
| --activation-mode     | Der Aktivierungsmodus für das Dienstpaket.|
| --constraints         | Die Platzierungseinschränkungen als Zeichenfolge. Platzierungseinschränkungen sind boolesche Ausdrücke für Knoteneigenschaften und ermöglichen es, einen Dienst entsprechend den Dienstanforderungen auf bestimmte Knoten einzuschränken. Soll ein Dienst z. B. auf Knoten platziert werden, für die Blau als Knotenfarbe (NodeColor) festgelegt ist, geben Sie Folgendes an: „NodeColor == blue“.|
| --correlated-service  | Der Name des Zieldiensts, dem der Dienst zugeordnet werden soll.|
| --correlation         | Ordnen Sie den Dienst über eine Ausrichtungsaffinität einem vorhandenen Dienst zu.|
| --dns-name            | Der DNS-Name des Diensts, der erstellt werden soll. Für diese Einstellung muss der Service Fabric-DNS-Systemdienst aktiviert sein.|
| --instance-count      | Die Anzahl von Instanzen. Dies gilt für nur zustandslose Dienste.|
| --int-scheme          | Gibt an, dass der Dienst über einen Bereich von Ganzzahlen ohne Vorzeichen gleichmäßig partitioniert werden muss.|
| --int-scheme-count    | Die Anzahl von Partitionen, die im ganzzahligen Schlüsselbereich (für ein einheitliches ganzzahliges Partitionsschema) erstellt werden sollen.|
| --int-scheme-high     | Das Ende des ganzzahligen Schlüsselbereichs, wenn ein einheitliches ganzzahliges Partitionsschema verwendet wird.|
| --int-scheme-low      | Der Anfang des ganzzahligen Schlüsselbereichs, wenn ein einheitliches ganzzahliges Partitionsschema verwendet wird.|
| --load-metrics        | JSON-codierte Liste der Metriken, die zum knotenübergreifenden Lastenausgleich für Dienste verwendet werden.|
| --min-replica-set-size| Die Mindestgröße der Replikatgruppe als Zahl. Dies gilt nur für zustandsbehaftete Dienste.|
| --move-cost           | Gibt die Verschiebungskosten für den Dienst an. Mögliche Werte sind: „Zero“, „Low“, „Medium“, „High“.|
| --named-scheme        | Gibt an, dass der Dienst mehrere benannte Partitionen haben muss.|
| --named-scheme-list   | JSON-codierte Liste mit Namen, über die der Dienst partitioniert werden soll, wenn das benannte Partitionsschema verwendet wird.|
| --no-persisted-state  | Bei „true“ gibt dieses Argument an, dass der Dienst keinen persistenten Zustand (Status) hat, der auf dem lokalen Datenträger gespeichert ist, oder den Zustand nur im Arbeitsspeicher speichert.|
| --placement-policy-list  | JSON-codierte Liste mit Platzierungsrichtlinien für den Dienst und alle zugehörigen Domänennamen. Richtlinien können aus einer oder einer Kombination der folgenden Angaben bestehen: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`.|
| --quorum-loss-wait    | Die maximale Dauer (in Sekunden), für die eine Partition den Zustand für Quorumsverlust haben darf. Dies gilt nur für zustandsbehaftete Dienste.|
| --replica-restart-wait| Die Dauer (in Sekunden) zwischen einem Ausfall eines Replikats und dem Erstellen eines neuen Replikats. Dies gilt nur für zustandsbehaftete Dienste.|
| --singleton-scheme    | Gibt an, dass der Dienst nur eine einzige Partition haben darf oder ein nicht partitionierter Dienst sein muss.|
| --stand-by-replica-keep  | Die maximale Dauer (in Sekunden), für die StandBy-Replikate beibehalten werden, bevor sie entfernt werden. Dies gilt nur für zustandsbehaftete Dienste.|
| --stateful            | Gibt an, dass der Dienst ein zustandsbehafteter Dienst ist.|
| --stateless           | Gibt an, dass der Dienst ein zustandsloser Dienst ist.|
| --target-replica-set-size| Die Zielgröße der Replikatgruppe als Zahl. Dies gilt nur für zustandsbehaftete Dienste.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-delete"></a>sfctl service delete
Löscht einen vorhandene Service Fabric-Dienst.

Löscht einen vorhandene Service Fabric-Dienst. Ein Dienst muss erstellt worden sein, bevor er gelöscht werden kann. Service Fabric versucht standardmäßig, Dienstreplikate auf ordnungsgemäße Weise zu schließen und den Dienst dann zu löschen. Wenn es aber Probleme mit dem ordnungsgemäßen Schließen eines Replikats gibt, kann der Löschvorgang sehr lange dauern oder abstürzen. Verwenden Sie das optionale Flag „ForceRemove“, um die ordnungsgemäße Schließsequenz zu überspringen und das Löschen des Diensts zu erzwingen.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --force-remove      | Erzwingt ein Entfernen einer Service Fabric-Anwendung oder eines Service Fabric-Diensts, ohne die Sequenz für ordnungsgemäßes Herunterfahren zu durchlaufen. Dieser Parameter kann für ein erzwungenes Löschen einer Anwendung oder eines Diensts verwendet werden, für die oder den beim Löschen ein Timeout aufgetreten ist, weil Probleme im Dienstcode ein ordnungsgemäßes Schließen von Replikaten verhindern.|
| --timeout -t        | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug             | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h           | Zeigt diese Hilfemeldung an und beendet.|
| --output -o         | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query             | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose           | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-description"></a>sfctl service description
Ruft die Beschreibung eines vorhandenen Service Fabric-Diensts ab.

Ruft die Beschreibung eines vorhandenen Service Fabric-Diensts ab. Ein Dienst muss erstellt worden sein, bevor seine Beschreibung abgerufen werden kann.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --timeout -t        | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug             | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h           | Zeigt diese Hilfemeldung an und beendet.|
| --output -o         | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query             | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose           | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-health"></a>sfctl service health
Ruft die Integrität des angegebenen Service Fabric-Diensts ab.

Ruft die Integritätsinformationen des angegebenen Diensts ab. Verwenden Sie „EventsHealthStateFilter“, um die Sammlung der Integritätsereignisse (HealthEvents), die für den Dienst gemeldet wurden, anhand des Integritätsstatus zu filtern. Verwenden Sie „PartitionsHealthStateFilter“, um die zurückgegebene Sammlung von Partitionen zu filtern. Geben Sie einen Dienst an, die im Integritätsspeicher nicht vorhanden ist, gibt dieses Cmdlet einen Fehler zurück. .

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --events-health-state-filter | Ermöglicht das Filtern der Sammlung zurückgegebener HealthEvent-Objekte anhand des Integritätsstatus. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus. Es werden nur Ereignisse zurückgegeben, die dem Filter entsprechen. Alle Ereignisse werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden alle Ereignisse zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein. Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat. Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“. – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben. Der Wert ist gleich „65535“.|
|--exclude-health-statistics     | Gibt an, ob die Integritätsstatistiken als Bestandteil des Abfrageergebnisses zurückgegeben werden sollen. Der Standardwert ist gleich „False“. Die Statistiken zeigen die Anzahl von untergeordneten Entitäten, die einen der Integritätsstatus „Ok“, „Warning“ oder „Error“ haben.|
| --partitions-health-state-filter| Ermöglicht es, die Partitionen-Integritätsstatusobjekte, die im Ergebnis einer Dienstintegritätsabfrage zurückgegeben werden, anhand des Integritätsstatus zu filtern. Die möglichen Werte für diesen Parameter entsprechen dem jeweiligen ganzzahligen Wert von einem der folgenden Integritätsstatus. Es werden nur Partitionen zurückgegeben, die dem Filter entsprechen. Alle Partitionen werden verwendet, um den aggregierten Integritätsstatus auszuwerten. Ist kein Filter angegeben, werden alle Einträge zurückgegeben. Ein Statuswert ist eine kennzeichenbasierte Enumeration, sodass der Wert eine Kombination der Werte sein kann, die mit dem bitweisen ODER-Operator abgerufen werden. Ist der angegebene Wert beispielsweise gleich „6“, werden Integritätsstatus der Partitionen zurückgegeben, für die „HealthState“ den Wert für OK (2) oder Warnung (4) hat. – Default: Standardwert. Stimmt mit jedem Integritätsstatus (HealthState) überein.                  Der Wert ist gleich null. – None: Filter, der mit keinem Wert für „HealthState“ übereinstimmt. Wird verwendet, um keine Ergebnisse für eine angegebene Statussammlung zurückzugeben. Der Wert ist gleich „1“. – Ok: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Ok“ hat. Der Wert ist gleich „2“. – Warning: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Warning“ hat. Der Wert ist gleich „4“. – Error: Filter, der mit Eingaben übereinstimmt, für die „HealthState“ den Wert „Error“ hat. Der Wert ist gleich „8“. – All: Filter, der mit Eingaben übereinstimmt, die einen beliebigen Wert für „HealthState“ haben. Der Wert ist gleich „65535“.|
| --timeout -t                 | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                      | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                    | Zeigt diese Hilfemeldung an und beendet.|
| --output -o                  | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.                  Standardwert: json.|
| --query                      | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose                    | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-info"></a>sfctl service info
Ruft die Informationen zu dem speziellen Dienst ab, der zu einer Service Fabric-Anwendung gehört.

Gibt die Informationen zu dem angegebenen Dienst ab, der zu der angegebenen Service Fabric-Anwendung gehört.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --application-id [erforderlich]| Die Identität (ID) der Anwendung. Dies ist üblicherweise der vollständige Name der Anwendung ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat eine Anwendung beispielsweise den Namen „fabric://meineapp/app1“, hat die Anwendungsidentität in 6.0+ den Wert „meineapp~app1“ und in früheren Versionen den Wert „meineapp/app1“.|
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --timeout -t            | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                 | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h               | Zeigt diese Hilfemeldung an und beendet.|
| --output -o             | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query                 | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose               | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-list"></a>sfctl service list
Ruft die Informationen zu allen Diensten ab, die zu der Anwendung gehören, die durch die Anwendungs-ID angegeben ist.

Gibt die Informationen zu allen Diensten zurück, die zu der Anwendung gehören, die durch die Anwendungs-ID angegeben ist.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --application-id [erforderlich]| Die Identität (ID) der Anwendung. Dies ist üblicherweise der vollständige Name der Anwendung ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat eine Anwendung beispielsweise den Namen „fabric://meineapp/app1“, hat die Anwendungsidentität in 6.0+ den Wert „meineapp~app1“ und in früheren Versionen den Wert „meineapp/app1“.|
| --continuation-token    | Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen. Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein.|
| --service-type-name     | Der Diensttypname, der zum Filtern der Dienste verwendet wird, die abgefragt werden sollen.|
| --timeout -t            | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                 | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h               | Zeigt diese Hilfemeldung an und beendet.|
| --output -o             | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query                 | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose               | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-manifest"></a>sfctl service manifest
Ruft das Manifest ab, in dem ein Diensttyp beschrieben ist.

Ruft das Manifest ab, in dem ein Diensttyp beschrieben ist. Die Antwort enthält den XML-Code des Dienstmanifests als Zeichenfolge.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --application-type-name [erforderlich]| Der Name des Anwendungstyps.|
| --application-type-version [erforderlich]| Die Version des Anwendungstyps.|
| --service-manifest-name [erforderlich]| Der Name eines Dienstmanifests, das als Bestandteil eines Anwendungstyps in einem Service Fabric-Cluster registriert ist.|
| --timeout -t                      | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug                           | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h                         | Zeigt diese Hilfemeldung an und beendet.|
| --output -o                       | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.                       Standardwert: json.|
| --query                           | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose                         | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-recover"></a>sfctl service recover
Veranlasst den Service Fabric-Cluster zu versuchen, den angegebenen Dienst wiederherzustellen, der derzeit in Quorumverlust gebunden ist.

Veranlasst den Service Fabric-Cluster zu versuchen, den angegebenen Dienst wiederherzustellen, der derzeit in Quorumverlust gebunden ist. Dieser Vorgang sollte nur ausgeführt werden, wenn die Replikate, die ausgefallen sind, nicht wiederhergestellt werden können. Eine falsche Verwendung dieser API kann dazu führen, dass Daten verloren gehen.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --timeout -t        | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug             | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h           | Zeigt diese Hilfemeldung an und beendet.|
| --output -o         | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query             | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose           | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-resolve"></a>sfctl service resolve
Löst eine Service Fabric-Partition auf.

Lösen Sie eine Service Fabric-Dienstpartition auf, um die Endpunkte der Dienstreplikate abzurufen.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Die Identität (ID) des Diensts. Dies ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --partition-key-type| Schlüsseltyp für die Partition. Dieser Parameter ist erforderlich, wenn das Partitionsschema für den Dienst gleich „Int64Range“ oder „Named“ ist. Die folgenden Werte sind möglich. – None (1): Gibt an, dass der Parameter „PartitionKeyValue“ nicht angegeben ist. Dies ist für die Partitionen gültig, deren Partitionsschema als „Singleton“ festgelegt ist. Dies ist der Standardwert. Der Wert ist gleich „1“. – Int64Range (2): Gibt an, dass der Wert des Parameters „PartitionKeyValue“ ein int64-Partitionsschlüssel ist. Dies ist für die Partitionen gültig, deren Partitionsschema als „Int64Range“ festgelegt ist. Der Wert ist gleich „2“. – Named (3): Gibt an, dass der Wert des Parameters „PartitionKeyValue“ ein Name der Partition ist. Dies ist für die Partitionen gültig, deren Partitionsschema als „Named“ festgelegt ist. Der Wert ist gleich „3“.|
| --partition-key-value  | Der Partitionsschlüssel. Dieses Argument ist erforderlich, wenn das Partitionsschema für den Dienst gleich „Int64Range“ oder „Named“ ist.|
| --previous-rsp-version | Der Wert im „Version“-Feld der Antwort, die zuvor empfangen wurde. Dieses Argument ist erforderlich, wenn der Benutzer weiß, dass das zuvor abgerufene Ergebnis veraltet ist.|
| --timeout -t        | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug             | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h           | Zeigt diese Hilfemeldung an und beendet.|
| --output -o         | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query             | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose           | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-service-update"></a>sfctl service update
Aktualisiert den angegebenen Dienst mithilfe der angegebenen Updatebeschreibung.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --service-id [erforderlich]| Der Zieldienst, der aktualisiert werden soll. Dies ist üblicherweise die vollständige ID des Diensts ohne das URI-Schema „fabric:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric://meineapp/app1/dienst1“, hat die Dienstidentität in 6.0+ den Wert „meineapp~app1~dienst1“ und in früheren Versionen den Wert „meineapp/app1/dienst1“.|
| --constraints         | Die Platzierungseinschränkungen als Zeichenfolge. Platzierungseinschränkungen sind boolesche Ausdrücke für Knoteneigenschaften und ermöglichen es, einen Dienst entsprechend den Dienstanforderungen auf bestimmte Knoten einzuschränken. Soll ein Dienst z. B. auf Knoten platziert werden, für die Blau als Knotenfarbe (NodeColor) festgelegt ist, geben Sie Folgendes an: „NodeColor == blue“.|
| --correlated-service  | Der Name des Zieldiensts, dem der Dienst zugeordnet werden soll.|
| --correlation         | Ordnen Sie den Dienst über eine Ausrichtungsaffinität einem vorhandenen Dienst zu.|
| --instance-count      | Die Anzahl von Instanzen. Dies gilt für nur zustandslose Dienste.|
| --load-metrics        | JSON-codierte Liste der Metriken, die zum knotenübergreifenden Lastenausgleich verwendet werden.|
| --min-replica-set-size| Die Mindestgröße der Replikatgruppe als Zahl. Dies gilt nur für zustandsbehaftete Dienste.|
| --move-cost           | Gibt die Verschiebungskosten für den Dienst an. Mögliche Werte sind: „Zero“, „Low“, „Medium“, „High“.|
| --placement-policy-list  | JSON-codierte Liste mit Platzierungsrichtlinien für den Dienst und alle zugehörigen Domänennamen. Richtlinien können aus einer oder einer Kombination der folgenden Angaben bestehen: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`.|
| --quorum-loss-wait    | Die maximale Dauer (in Sekunden), für die eine Partition den Zustand für Quorumsverlust haben darf. Dies gilt nur für zustandsbehaftete Dienste.|
| --replica-restart-wait| Die Dauer (in Sekunden) zwischen einem Ausfall eines Replikats und dem Erstellen eines neuen Replikats. Dies gilt nur für zustandsbehaftete Dienste.|
| --stand-by-replica-keep  | Die maximale Dauer (in Sekunden), für die StandBy-Replikate beibehalten werden, bevor sie entfernt werden. Dies gilt nur für zustandsbehaftete Dienste.|
| --stateful            | Gibt an, dass der Zieldienst ein zustandsbehafteter Dienst ist.|
| --stateless           | Gibt an, dass der Zieldienst ein zustandsloser Dienst ist.|
| --target-replica-set-size| Die Zielgröße der Replikatgruppe als Zahl. Dies gilt nur für zustandsbehaftete Dienste.|
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

---
title: Azure Service Fabric CLI – sfctl compose | Microsoft-Dokumentation
description: Beschreibt die sfctl compose-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 3ce0b63c579412d9d8d35b835803becab09f7ef4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494151"
---
# <a name="sfctl-compose"></a>sfctl compose
Ermöglicht es, Docker Compose Anwendungen zu erstellen, zu löschen und zu verwalten.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| create | Erstellt eine Service Fabric-Compose-Bereitstellung. |
| list | Ruft die Liste der Compose-Bereitstellungen ab, die in dem Service Fabric-Cluster erstellt wurden. |
| remove | Löscht eine vorhandene Service Fabric-Compose-Bereitstellung aus dem Cluster. |
| status | Ruft Informationen zu einer Service Fabric-Compose-Bereitstellung ab. |
| Upgrade | Startet ein Aktualisieren einer Compose-Bereitstellung im Service Fabric-Cluster. |
| upgrade-status | Ruft Details zu dem neuesten Upgrade ab, das für diese Compose-Bereitstellung von Service Fabric durchgeführt wurde. |

## <a name="sfctl-compose-create"></a>sfctl compose create
Erstellt eine Service Fabric-Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Der Name der Bereitstellung |
| --file-path [erforderlich] | Pfad zur Docker Compose-Zieldatei. |
| --encrypted-pass | Bewirkt, dass anstelle der Aufforderung zur Eingabe eines Containerregistrierungskennworts eine bereits verschlüsselte Passphrase verwendet wird. |
| --has-pass | Fordert zur Eingabe eines Kennworts für die Containerregistrierung auf. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |
| --user | Der Benutzername, für den die Verbindung zur Containerregistrierung hergestellt werden soll. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-compose-list"></a>sfctl compose list
Ruft die Liste der Compose-Bereitstellungen ab, die in dem Service Fabric-Cluster erstellt wurden.

Ruft den Status zu den Compose-Bereitstellungen ab, die im Service Fabric-Cluster erstellt wurden oder momentan erstellt werden. Die Antwort enthält den Namen, den Status und weitere Details über die Compose-Bereitstellungen. Wenn die Liste mit den Bereitstellungen nicht auf eine Seite passt, werden sowohl eine Seite mit Ergebnissen als auch ein Fortsetzungstoken zurückgegeben, über das zur nächsten Seite gewechselt werden kann.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --continuation-token | Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen. Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein. |
| --max-results | Die maximale Anzahl von Ergebnissen, die als Teil der seitenweisen Abfragen zurückgegeben werden sollen. Dieser Parameter definiert die obere Grenze für die Anzahl von zurückgegebenen Ergebnissen. Es können weniger Ergebnisse zurückgegeben werden, als dieser maximalen Anzahl entspricht. Dies ist der Fall, wenn die Ergebnisse wegen der Größenbeschränkungen, die für Meldungen in der Konfiguration definiert sind, nicht in die jeweilige Meldung passen. Ist dieser Parameter gleich null oder nicht angegeben, enthält die seitenweise Abfrage so viele Ergebnisse, wie in die Rückgabemeldung passen. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-compose-remove"></a>sfctl compose remove
Löscht eine vorhandene Service Fabric-Compose-Bereitstellung aus dem Cluster.

Löscht eine vorhandene Service Fabric-Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-compose-status"></a>sfctl compose status
Ruft Informationen zu einer Service Fabric-Compose-Bereitstellung ab.

Gibt den Status der Compose-Bereitstellung zurück, die im Service Fabric-Cluster erstellt wurde oder gerade erstellt wird und deren Name mit dem übereinstimmt, der als Parameter angegebenen wurde. Die Antwort enthält den Namen, den Status und weitere Details über die Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-compose-upgrade"></a>sfctl compose upgrade
Startet ein Aktualisieren einer Compose-Bereitstellung im Service Fabric-Cluster.

Überprüft die für das Upgrade angegebenen Parameter und startet die Durchführung des Upgrades für die Bereitstellung, sofern die Parameter gültig sind.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Der Name der Bereitstellung |
| --file-path [erforderlich] | Pfad zur Docker Compose-Zieldatei. |
| --default-svc-type-health-map | JSON-codiertes Wörterbuch, in dem die Integritätsrichtlinie beschrieben ist, mit der die Integrität von Diensten bewertet wird. |
| --encrypted-pass | Bewirkt, dass anstelle der Aufforderung zur Eingabe eines Containerregistrierungskennworts eine bereits verschlüsselte Passphrase verwendet wird. |
| --failure-action | Folgende Werte sind möglich\: „Invalid“, „Rollback“, „Manual“. |
| --force-restart | Erzwingt einen Neustart. |
| --has-pass | Fordert zur Eingabe eines Kennworts für die Containerregistrierung auf. |
| --health-check-retry | Timeout der Wiederholung einer Integritätsprüfung, gemessen in Millisekunden |
| --health-check-stable | Stabile Dauer für Integritätsprüfung, gemessen in Millisekunden. |
| --health-check-wait | Wartezeit für Integritätsprüfung, gemessen in Millisekunden. |
| --replica-set-check | Timeout der Upgradeprüfung von Replikatgruppe, gemessen in Sekunden |
| --svc-type-health-map | JSON-codierte Liste der Objekte, in denen die Integritätsrichtlinien beschrieben sind, mit denen die Integrität verschiedener Diensttypen bewertet wird. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |
| --unhealthy-app | Der maximal zulässige Prozentsatz fehlerhafter Anwendungen, bevor ein Fehler gemeldet wird. <br><br> Soll es z. B.zulässig sein, dass 10 % der Anwendungen fehlerhaft sind, muss dieser Wert gleich „10“ sein. Der Prozentsatz entspricht dem maximalen tolerierten Prozentsatz an Anwendungen, die fehlerhaft sein können, bevor der Cluster als fehlerhaft behandelt wird. Wird der Prozentsatz eingehalten, gibt es aber mindestens eine fehlerhafte Anwendung, wird die Integrität als „Warning“ ausgewertet. Dieser wird berechnet, indem die Anzahl von fehlerhaften Anwendungen durch die Gesamtanzahl von Anwendungsinstanzen im Cluster dividiert wird. |
| --upgrade-domain-timeout | Timeout der Upgradedomäne, gemessen in Millisekunden |
| --upgrade-kind | Standardwert\: „Rolling“. |
| --upgrade-mode | Folgende Werte sind möglich\: „Invalid“, „UnmonitoredAuto“, „UnmonitoredManual“, „Monitored“.  Standardwert\: „UnmonitoredAuto“. |
| --upgrade-timeout | Timeout des Upgrades, gemessen in Millisekunden |
| --user | Der Benutzername, für den die Verbindung zur Containerregistrierung hergestellt werden soll. |
| --warning-as-error | Warnungen werden mit demselben Schweregrad berücksichtigt wie Fehler. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl compose upgrade-status
Ruft Details zu dem neuesten Upgrade ab, das für diese Compose-Bereitstellung von Service Fabric durchgeführt wurde.

Gibt die Informationen über den Status des Upgrades für Compose-Bereitstellungen zusammen mit Details zurück, die beim Debuggen von Anwendungsfehlern unterstützen.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)
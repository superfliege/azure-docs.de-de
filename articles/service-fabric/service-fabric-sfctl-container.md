---
title: Azure Service Fabric-CLI – sfctl container | Microsoft-Dokumentation
description: Beschreibt die sfctl-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI) für Container.
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
ms.openlocfilehash: 27108d27ee27346e4cba44e6778faff56df70a36
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495127"
---
# <a name="sfctl-container"></a>sfctl container
Führt containerbezogene Befehle auf einem Clusterknoten aus.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| invoke-api | Ruft die Container-REST-API auf. |
| Protokolle | Ruft Containerprotokolle ab. |

## <a name="sfctl-container-invoke-api"></a>sfctl container invoke-api
Ruft die Container-REST-API auf.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --application-id           [erforderlich] | Anwendungsidentität. |
| --code-package-instance-id [erforderlich] | Instanz-ID des Codepakets, die mit „service code-package-list“ abgerufen werden kann. |
| --code-package-name        [erforderlich] | Name des Codepakets. |
| --container-api-uri-path   [erforderlich] | Pfad zum URI der Container-REST-API: Verwenden Sie „{id}“ anstatt des Namens bzw. der ID des Containers. |
| --node-name [erforderlich] | Der Name des Knotens. |
| --service-manifest-name [erforderlich] | Name des Dienstmanifests. |
| --container-api-body | HTTP-Anforderungstext für die Container-REST-API. |
| --container-api-content-type | Inhaltstyp der Container-REST-API, Standardwert ist „application/json“. |
| --container-api-http-verb | HTTP-Verb für die Container-REST-API, Standardwert ist GET. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-container-logs"></a>sfctl container logs
Ruft Containerprotokolle ab.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --application-id           [erforderlich] | Anwendungsidentität. |
| --code-package-instance-id [erforderlich] | Instanz-ID des Codepakets, die mit „service code-package-list“ abgerufen werden kann. |
| --code-package-name        [erforderlich] | Name des Codepakets. |
| --node-name [erforderlich] | Der Name des Knotens. |
| --service-manifest-name [erforderlich] | Name des Dienstmanifests. |
| --tail | Gibt nur diese Anzahl von Protokollzeilen vom Ende der Protokolle zurück. Wird als Ganzzahl angegeben; geben Sie „all“ an, um alle Protokollzeilen auszugeben. Der Standardwert lautet „all“. |
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
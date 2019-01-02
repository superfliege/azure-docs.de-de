---
title: 'Azure Service Fabric CLI: sfctl mesh secretvalue | Microsoft-Dokumentation'
description: Beschreibt die sfctl mesh secretvalue-Befehle der Service Fabric CLI.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: b8172ba50f7fec8f0aa1ba768c05d087ecabf8c8
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53283796"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Ruft die Mesh-secretvalue-Ressourcen ab bzw. löscht sie.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| delete | Löscht den angegebenen Wert der benannten Geheimnisressource. |
| list | Listet die Namen aller Werte der angegebenen Geheimnisressource auf. |
| show | Ruft den Wert der angegebenen Version einer Geheimnisressource ab. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue delete
Löscht den angegebenen Wert der benannten Geheimnisressource.

Löscht die Geheimniswertressource, die anhand des Namens identifiziert wird. Der Name der Ressource ist normalerweise die Version, die diesem Wert zugeordnet ist. Beim Löschen tritt ein Fehler auf, wenn der angegebene Wert verwendet wird.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |
| --version -v     [Erforderlich] | Der Name der Geheimnisversion. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue list
Listet die Namen aller Werte der angegebenen Geheimnisressource auf.

Ruft Informationen zu allen Geheimniswertressourcen der angegebenen Geheimnisressource ab. Die Informationen enthalten den Namen der Geheimniswertressourcen, aber nicht die tatsächlichen Werte.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue show
Ruft den Wert der angegebenen Version einer Geheimnisressource ab.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |
| --version -v     [Erforderlich] | Der Name der Geheimnisversion. |
| --show-value | Zeigt den tatsächlichen Wert der Geheimnisversion an. |

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
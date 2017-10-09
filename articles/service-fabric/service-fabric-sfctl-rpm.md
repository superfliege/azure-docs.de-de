---
title: "Azure Service Fabric CLI – sfctl rpm | Microsoft-Dokumentation"
description: Beschreibt die sfctl rpm-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.date: 09/26/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 469246d6cb64d6aaf995ef3b7c4070f8d24372b1
ms.openlocfilehash: f032af4714ad458fa6ad6fb0741f689d44f4098b
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---

# <a name="sfctl-rpm"></a>sfctl rpm
Ermöglicht es, Befehle an den Reparatur-Manager-Dienst abzufragen und zu senden.

## <a name="commands"></a>Befehle
|Befehl|Beschreibung|
| --- | --- |
|    approve-force| Erzwingt die Genehmigung das angegebenen Reparaturtasks.|
|    delete       | Löscht einen abgeschlossenen Reparaturtask.|
|    list         | Ruft eine Liste der Reparaturtasks ab, die dem angegebenen Filter entsprechen.|

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
Löscht einen abgeschlossenen Reparaturtask.

Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden. 

### <a name="arguments"></a>Argumente
|Argument|Beschreibung|
| --- | --- |
|    --task-id [erforderlich]| Die ID des abgeschlossenen Reparaturtasks, der gelöscht werden soll.|
|    --version           | Die aktuelle Versionsnummer des Reparaturtasks. Ist der Wert ungleich null, ist die Anforderung nur dann erfolgreich, wenn dieser Wert mit der tatsächlichen aktuellen Version des Reparaturtasks übereinstimmt. Ist der Wert gleich null, wird keine Versionsüberprüfung ausgeführt.|

### <a name="global-arguments"></a>Globale Argumente
|Argument|Beschreibung|
| --- | --- |
|    --debug             | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
|    --help -h           | Zeigt diese Hilfemeldung an und beendet.|
|    --output -o         | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.
|    --query             | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
|    --verbose           | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|


## <a name="sfctl-rpm-list"></a>sfctl rpm list
Ruft eine Liste der Reparaturtasks ab, die dem angegebenen Filter entsprechen.

Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden. 

### <a name="arguments"></a>Argumente
|Argument|Beschreibung|
| --- | --- |
|    --executor-filter| Der Name des Reparaturexecutors, dessen beanspruchte Tasks in der Liste enthalten sein sollen.|
|    --state-filter   | Eine bitweise OR-Operation für die folgenden Werte, die angibt, welche Taskstatus in der Ergebnisliste enthalten sein sollen. – 1: Erstellt – 2: Beansprucht – 4: Wird vorbereitet – 8: Genehmigt – 16: Wird ausgeführt – 32: Wird wiederhergestellt – 64: Abgeschlossen.|
|    --task-id-filter | Das Reparaturtask-ID-Präfix, das abgeglichen werden soll.|

### <a name="global-arguments"></a>Globale Argumente
|Argument|Beschreibung|
| --- | --- |
|    --debug          | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
|    --help -h        | Zeigt diese Hilfemeldung an und beendet.|
|    --output -o      | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standard| json.|
|    --query          | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
|    --verbose        | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)

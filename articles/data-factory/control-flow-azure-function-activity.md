---
title: Aktivität „Azure Function“ in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Aktivität „Azure Function“ verwenden, um eine Azure-Funktion in einer Data Factory-Pipeline auszuführen
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: douglasl
ms.openlocfilehash: 4b185236e5925152acb5f8a733e117186a2318cf
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53740891"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Aktivität „Azure Function“ in Azure Data Factory

Mit der Aktivität „Azure Function“ können Sie [Azure-Funktionen](../azure-functions/functions-overview.md) in einer Data Factory-Pipeline ausführen. Um eine Azure-Funktion auszuführen, müssen Sie einen verknüpften Dienst erstellen sowie eine Aktivität, die die auszuführende Azure-Funktion angibt.

## <a name="azure-function-linked-service"></a>Verknüpfter Dienst der Azure-Funktion

Der Rückgabetyp der Azure-Funktion muss ein gültiges JObject sein. Jeder andere Typ schlägt fehl und löst den generischen Benutzerfehler *Fehler beim Aufrufen des Endpunkts* aus.

| **Eigenschaft** | **Beschreibung** | **Erforderlich** |
| --- | --- | --- |
| type   | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **AzureFunction** | Ja |
| Funktions-App-URL | URL für die Azure-Funktions-App. Das Format lautet `https://<accountname>.azurewebsites.net`. Diese URL ist der Wert unter dem Abschnitt **URL**, wenn Sie Ihre Funktions-App im Azure-Portal anzeigen.  | Ja |
| Funktionsschlüssel | Der Zugriffsschlüssel für die Azure-Funktion. Klicken Sie in den Abschnitt **Verwalten** der jeweiligen Funktion, und kopieren Sie entweder den **Funktionsschlüssel** oder den **Hostschlüssel**. Weitere Informationen finden Sie hier: [HTTP-Trigger und -Bindungen in Azure Functions](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | Ja |
|   |   |   |

## <a name="azure-function-activity"></a>Aktivität „Azure Function“

| **Eigenschaft**  | **Beschreibung** | **Zulässige Werte** | **Erforderlich** |
| --- | --- | --- | --- |
| name  | Name der Aktivität in der Pipeline  | Zeichenfolge | Ja |
| type  | Typ der Aktivität ist „AzureFunctionActivity“ | Zeichenfolge | Ja |
| Verknüpfter Dienst | Der mit der Azure-Funktion verknüpfte Dienst für die entsprechende Azure-Funktions-App  | Verweis auf den verknüpften Dienst | Ja |
| Funktionsname  | Name der Funktion in der Azure-Funktions-App, die diese Aktivität aufruft. | Zeichenfolge | Ja |
| method  | REST-API-Methode für den Funktionsaufruf | Unterstützte Zeichenfolgentypen: „GET“, „POST“, „PUT“   | Ja |
| Header  | Header, die in der Anforderung gesendet werden. So legen Sie beispielsweise die Sprache und den Typ für eine Anforderung fest: "headers": { "Accept-Language": "en-us", "Content-Type": "application/json" } | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein  |
| body  | Text, der zusammen mit der Anforderung an die API-Methode der Funktion gesendet wird  | Zeichenfolge (oder Ausdruck mit resultType der Zeichenfolge) oder Objekt.   | Erforderlich für PUT/POST-Methoden |
|   |   |   | |

Weitere Informationen finden Sie in den Details zum Schema der Anforderungsnutzlast im Abschnitt  [Schema der Anforderungsnutzlast](control-flow-web-activity.md#request-payload-schema) .

## <a name="more-info"></a>Weitere Informationen

Die Azure-Funktionsaktivität unterstützt **Routing**. Wenn Ihre App z.B. das Routing `https://functionAPP.azurewebsites.net/api/functionName/{value}?code=<secret>` verwendet, weist `functionName` die Form `functionName/{value}` auf und kann zur Bereitstellung des gewünschten `functionName`-Elements zur Laufzeit parametrisiert werden.

Die Azure-Funktionsaktivität unterstützt auch **Abfragen**. Eine Abfrage muss Teil von `functionName` sein, z.B. `HttpTriggerCSharp2?name=hello`, wobei `HttpTriggerCSharp2` für `function name` steht.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Aktivitäten in Data Factory in [Pipelines und Aktivitäten in Azure Data Factory](concepts-pipelines-activities.md).
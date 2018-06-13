---
title: Azure Resource Manager-Grenzwerte für Anforderungen | Microsoft Docs
description: Beschreibt, wie eine Begrenzung von Azure Resource Manager-Anforderungen genutzt wird, wenn Abonnementgrenzwerte erreicht wurden.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2018
ms.author: tomfitz
ms.openlocfilehash: f3dcb0c5036b2cfc38ef2a6a16269a8697bbd9e6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358862"
---
# <a name="throttling-resource-manager-requests"></a>Begrenzen von Resource Manager-Anforderungen
Für jedes Abonnement und jeden Mandanten begrenzt Resource Manager Leseanforderungen auf 15.000 pro Stunde und Schreibanforderungen auf 1.200 pro Stunde. Diese Grenzwerte gelten für jede Azure Resource Manager-Instanz. In jeder Azure-Region sind mehrere Instanzen vorhanden, und Azure Resource Manager wird in allen Azure-Regionen bereitgestellt.  In der Praxis sind die Grenzwerte also effektiv sehr viel höher als die aufgeführten Grenzwerte, da Benutzeranforderungen in der Regel von vielen verschiedenen Instanzen verarbeitet werden.

Wenn Ihre Anwendung oder Ihr Skript diese Grenzwerte erreicht, müssen Sie die Anforderungen begrenzen. In diesem Artikel erfahren Sie, wie Sie Ihre verbleibenden Anforderungen bestimmen können, bevor der Grenzwert erreicht wird, und wie Sie reagieren können, wenn Sie den Grenzwert erreicht haben.

Wenn Sie den Grenzwert erreichen, erhalten Sie den HTTP-Statuscode **429 Zu viele Anforderungen**.

Die Anzahl von Anforderungen ist für Ihr Abonnement und Ihren Mandanten begrenzt. Wenn mehrere Anwendungen gleichzeitig in Ihrem Abonnement Anforderungen auslösen, werden die Anforderungen dieser Anwendungen addiert, um die Anzahl der verbleibenden Anforderungen zu bestimmen.

Bei abonnementbezogenen Anforderungen wird Ihre Abonnement-ID übergeben, z.B. beim Abrufen der Ressourcengruppen in Ihrem Abonnement. Mandantenbezogene Anforderungen enthalten Ihre Abonnement-ID nicht, etwa beim Abrufen von gültigen Azure-Standorten.

## <a name="remaining-requests"></a>Verbleibende Anforderungen
Sie können die Anzahl der verbleibenden Anforderungen durch Untersuchen der Antwortheader bestimmen. Jede Anforderung enthält Werte für die Anzahl der verbleibenden Lese- und Schreibanforderungen. Die folgende Tabelle beschreibt die Antwortheader, die Sie auf diese Werte untersuchen können:

| Antwortheader | BESCHREIBUNG |
| --- | --- |
| x-ms-ratelimit-remaining-subscription-reads |Verbleibende abonnementbezogene Lesevorgänge. Dieser Wert wird für Lesevorgänge zurückgegeben. |
| x-ms-ratelimit-remaining-subscription-writes |Verbleibende abonnementbezogene Schreibvorgänge. Dieser Wert wird für Schreibvorgänge zurückgegeben. |
| x-ms-ratelimit-remaining-tenant-reads |Verbleibende mandantenbezogene Lesevorgänge |
| x-ms-ratelimit-remaining-tenant-writes |Verbleibende mandantenbezogene Schreibvorgänge |
| x-ms-ratelimit-remaining-subscription-resource-requests |Verbleibende abonnementbezogene Ressourcenanforderungen.<br /><br />Dieser Headerwert wird nur zurückgegeben, wenn ein Dienst den standardmäßigen Grenzwert außer Kraft gesetzt hat. Resource Manager fügt diesen Wert anstelle der Lese- oder Schreibvorgänge des Abonnements hinzu. |
| x-ms-ratelimit-remaining-subscription-resource-entities-read |Verbleibende abonnementbezogene Ressourcensammlungsanforderungen.<br /><br />Dieser Headerwert wird nur zurückgegeben, wenn ein Dienst den standardmäßigen Grenzwert außer Kraft gesetzt hat. Dieser Wert gibt die Anzahl der verbleibenden Sammlungsanforderungen an (Auflisten von Ressourcen). |
| x-ms-ratelimit-remaining-tenant-resource-requests |Verbleibende mandantenbezogene Ressourcenanforderungen.<br /><br />Dieser Header wird nur für Anforderungen auf Mandantenebene hinzugefügt, und nur dann, wenn ein Dienst den standardmäßigen Grenzwert außer Kraft gesetzt hat. Resource Manager fügt diesen Wert anstelle der Lese- oder Schreibvorgänge des Mandanten hinzu. |
| x-ms-ratelimit-remaining-tenant-resource-entities-read |Verbleibende mandantenbezogene Ressourcensammlungsanforderungen.<br /><br />Dieser Header wird nur für Anforderungen auf Mandantenebene hinzugefügt, und nur dann, wenn ein Dienst den standardmäßigen Grenzwert außer Kraft gesetzt hat. |

## <a name="retrieving-the-header-values"></a>Abrufen der Headerwerte
Das Abrufen dieser Headerwerte in Ihrem Code oder Skript unterscheidet sich nicht vom Abrufen anderer Headerwerte. 

Beispiel: In **C#** rufen Sie den Headerwert aus einem **HttpWebResponse**-Objekt mit dem Namen **response** mit dem folgenden Code ab:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

In **PowerShell** rufen Sie den Headerwert aus einem Invoke-WebRequest-Vorgang ab.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Wenn Sie die verbleibenden Anforderungen für das Debuggen anzeigen möchten, können Sie den **-Debug**-Parameter in Ihrem **PowerShell**-Cmdlet bereitstellen.

```powershell
Get-AzureRmResourceGroup -Debug
```

So wird eine Vielzahl von Werten zurückgegeben, einschließlich des folgenden Antwortwerts:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
```

Um den Schreibgrenzwert abzurufen, verwenden Sie einen Schreibvorgang: 

```powershell
New-AzureRmResourceGroup -Name myresourcegroup -Location westus -Debug
```

So wird eine Vielzahl von Werten zurückgegeben, einschließlich der folgenden Werte:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

In der **Azure-CLI** rufen Sie den Headerwert mithilfe der ausführlicheren Option ab.

```azurecli
az group list --verbose --debug
```

So wird eine Vielzahl von Werten zurückgegeben, einschließlich der folgenden Werte:

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '14998'
```

Um den Schreibgrenzwert abzurufen, verwenden Sie einen Schreibvorgang: 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

So wird eine Vielzahl von Werten zurückgegeben, einschließlich der folgenden Werte:

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="waiting-before-sending-next-request"></a>Warten vor dem Senden der nächsten Anforderung
Wenn der Grenzwert für Anforderungen erreicht ist, gibt Resource Manager den HTTP-Statuscode **429** und einen **Retry-After** Wert im Header zurück. Der **Retry-After**-Wert gibt die Anzahl von Sekunden an, die Ihre Anwendung warten muss (oder im Ruhezustand verbleibt), bevor die nächste Anforderung gesendet wird. Wenn Sie eine Anforderung senden, bevor der Retry-Wert verstrichen ist, wird Ihre Anforderung nicht verarbeitet, und es wird ein neuer Retry-Wert zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Grenzwerten und Kontingenten finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
* Informationen zum Arbeiten mit asynchronen REST-Anforderungen finden Sie unter [Nachverfolgen asynchroner Vorgänge in Azure](resource-manager-async-operations.md).

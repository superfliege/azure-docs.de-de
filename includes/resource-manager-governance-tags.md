---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 03/13/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 69c67f437d2f0b7bd6c1f5311eb5ba1d962d889a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38738878"
---
Durch Anwenden von Tags können Sie Ihre Azure-Ressourcen logisch nach Kategorien organisieren. Jedes Tag besteht aus einem Namen und einem Wert. So können Sie beispielsweise den Namen „Umgebung“ und den Wert „Produktion“ auf alle Ressourcen in der Produktion anwenden.

Nach dem Anwenden von Tags können Sie alle Ressourcen in Ihrem Abonnement abrufen, die diesen Tagnamen und -wert besitzen. Mit Tags können Sie verwandte Ressourcen aus verschiedenen Ressourcengruppen abrufen. Das ist hilfreich, wenn Sie Ressourcen für die Abrechnung oder Verwaltung organisieren müssen.

Für Tags gelten folgende Einschränkungen:

* Jede Ressource oder Ressourcengruppe kann maximal 15 Tagname-Wert-Paare besitzen. Diese Einschränkung gilt nur für Tags, die direkt auf die Ressourcengruppe oder die Ressource angewendet werden. Eine Ressourcengruppe kann zahlreiche Ressourcen mit jeweils 15 Tagname-Wert-Paaren enthalten. Wenn Sie einer Ressource mehr als 15 Werte zuordnen müssen, verwenden Sie eine JSON-Zeichenfolge für den Tagwert. Die JSON-Zeichenfolge kann zahlreiche Werte enthalten, die auf einen einzelnen Tagnamen angewendet werden. Dieser Artikel enthält ein Beispiel für die Zuweisung einer JSON-Zeichenfolge zum Tag.
* Der Tagname ist auf 512 Zeichen beschränkt und der Tagwert auf 256 Zeichen. Bei Speicherkonten ist der Tagname auf 128 Zeichen beschränkt und der Tagwert auf 256 Zeichen.
* Auf die Ressourcengruppe angewendete Tags werden nicht von den in dieser Ressourcengruppe enthaltenen Ressourcen geerbt.
* Tags können nicht auf klassische Ressourcen wie Cloud Services angewendet werden.
* Tag-Namen dürfen die folgenden Zeichen nicht enthalten: `<`, `>`, `%`, `&`, `\`, `?`, `/`

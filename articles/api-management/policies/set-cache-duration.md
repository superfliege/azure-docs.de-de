---
title: "Beispiel für eine Azure API Management-Richtlinie – Festlegen der Aufbewahrungsdauer im Cache für Antworten | Microsoft-Dokumentation"
description: "Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie die Aufbewahrungsdauer von Antworten im Cache mit dem maxAge-Wert in dem vom Back-End gesendeten Cache-Control-Header festlegen."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 8471b51999159c8f6233abbbce998b122163efd8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="set-response-cache-duration"></a>Festlegen der Aufbewahrungsdauer von Antworten im Cache

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie die Aufbewahrungsdauer von Antworten im Cache mit dem maxAge-Wert im vom Back-End gesendeten Cache-Control-Header festlegen. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte in [Festlegen oder Bearbeiten von Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [Richtlinienbeispiele](../policy-samples.md)


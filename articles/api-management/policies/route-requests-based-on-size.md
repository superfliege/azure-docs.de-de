---
title: "Beispiel für eine Azure API Management-Richtlinie – Weiterleiten von Anforderungen basierend auf der Größe des Textkörpers | Microsoft-Dokumentation"
description: "Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie Anforderungen basierend auf der Größe des Textkörpers weiterleiten."
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
ms.openlocfilehash: 0558f82a31015af91df135b6a0d4c4050049d11d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>Weiterleiten der Anforderung basierend auf der Größe des Textkörpers

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie Anforderungen basierend auf der Größe des Textkörpers weiterleiten. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte in [Festlegen oder Bearbeiten von Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [Richtlinienbeispiele](../policy-samples.md)


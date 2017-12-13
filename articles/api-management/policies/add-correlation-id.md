---
title: "Beispiel für eine Azure API Management-Richtlinie – Hinzufügen eines Headers mit einer Korrelations-ID | Microsoft-Dokumentation"
description: "Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie der eingehenden Anforderung einen Header mit einer Korrelations-ID hinzufügen."
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
ms.openlocfilehash: a550009b4442bb59b9b9f4b18593a7537213bb78
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="add-a-header-containing-a-correlation-id"></a>Hinzufügen eines Headers mit eine Korrelations-ID

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie der eingehenden Anforderung einen Header mit einer Korrelations-ID hinzufügen. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte in [Festlegen oder Bearbeiten von Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [Richtlinienbeispiele](../policy-samples.md)


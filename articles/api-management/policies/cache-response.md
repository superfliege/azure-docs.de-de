---
title: "Azure API Management-Richtlinienbeispiel – Hinzufügen von Funktionen zu einem Back-End-Dienst | Microsoft-Dokumentation"
description: "Azure API Management-Richtlinienbeispiel: Veranschaulicht, wie Sie einem Back-End-Dienst Funktionen hinzufügen. Beispielsweise können Sie in einer API zur Wettervorhersage anstelle von Längen- und Breitengrad einen Ortsnamen akzeptieren."
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
ms.openlocfilehash: d500df0f0e48134ac9c1397795d65706d2e56ff9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="add-capabilities-to-a-backend-service"></a>Hinzufügen von Funktionen zu einem Back-End-Dienst

Dieser Artikel veranschaulicht mithilfe eines Beispiels für eine Azure API Management-Richtlinie, wie Sie einem Back-End-Dienst Funktionen hinzufügen. Beispielsweise können Sie in einer API zur Wettervorhersage anstelle von Längen- und Breitengrad einen Ortsnamen akzeptieren. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)


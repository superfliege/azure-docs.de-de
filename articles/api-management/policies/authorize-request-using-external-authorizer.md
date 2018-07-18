---
title: 'Beispiel für eine Azure API Management-Richtlinie: Autorisieren von Anforderungen mit einem externen Autorisierer | Microsoft-Dokumentation'
description: 'Beispiel für eine Azure API Management-Richtlinie: Hier erfahren Sie, wie Sie Anforderungen mithilfe eines externen Autorisierers autorisieren, der eine benutzerdefinierte oder ältere Authentifizierungs-/Autorisierungslogik kapselt.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 7d172a40f2aad65b595026fc656634060a1f7193
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284871"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorisieren von Anforderungen mit einem externen Autorisierer

Dieser Artikel veranschaulicht anhand eines Beispiels für eine Azure API Management-Richtlinie, wie Sie den API-Zugriff mithilfe eines externen Autorisierers schützen, der eine benutzerdefinierte Authentifizierungs-/Autorisierungslogik kapselt. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [API Management-Richtlinien für die Zugriffsbeschränkung](../api-management-access-restriction-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)
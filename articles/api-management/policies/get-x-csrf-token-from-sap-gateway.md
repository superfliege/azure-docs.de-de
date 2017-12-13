---
title: "Beispiel für eine Azure API Management-Richtlinie – Implementieren von X-CSRF-Mustern | Microsoft-Dokumentation"
description: "Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie ein von APIs verwendetes X-CSRF-Muster implementieren. Dieses Beispiel bezieht sich speziell auf das SAP-Gateway."
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
ms.openlocfilehash: a99ff25f7f8a3851936ba8d28e76eff98b1b0458
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="implement-x-csrf-pattern"></a>Implementieren von X-CSRF-Mustern

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie ein von APIs verwendetes X-CSRF-Muster implementieren. Dieses Beispiel bezieht sich speziell auf das SAP-Gateway. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte in [Festlegen oder Bearbeiten von Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get X-CSRF token from SAP gateway using send request policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [Richtlinienbeispiele](../policy-samples.md)


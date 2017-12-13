---
title: "Beispiel für eine Azure API Management-Richtlinie –Senden von Anforderungskontextinformationen an den Back-End-Dienst | Microsoft-Dokumentation"
description: "Beispiel für eine Azure API Management-Richtlinie: Veranschaulicht, wie Anforderungskontextinformationen an den Back-End-Dienst gesendet werden."
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
ms.openlocfilehash: af4d0a8aa3705b290fa5e4801eaa25264229696e
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="send-request-context-information-to-the-backend-service"></a>Senden von Anforderungskontextinformationen an den Back-End-Dienst

Dieser Artikel enthält ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Anforderungskontextinformationen an den Back-End-Dienst gesendet werden. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [How to set or edit Azure API Management policies](../set-edit-policies.md) (Festlegen oder Bearbeiten von Azure API Management-Richtlinien) aus. Weitere Beispiele finden Sie unter [API Management policy samples](../policy-samples.md) (API Management-Richtlinienbeispiele).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management policy samples](../policy-samples.md) (API Management-Richtlinienbeispiele)


---
title: "Beispiel für eine Azure API Management-Richtlinie – Generieren von Shared Access Signatures | Microsoft-Dokumentation"
description: "Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie Shared Access Signatures mithilfe von Ausdrücken generieren und die Anforderung mithilfe der rewrite-uri-Richtlinie an Azure Storage weiterleiten."
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
ms.openlocfilehash: e1f17f9f4e17a3eebb55e4ec1905aec19a2165a5
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="generate-shared-access-signature"></a>Generieren von Shared Access Signatures

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie [Shared Access Signatures](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1) mithilfe von Ausdrücken generieren und die Anforderung mithilfe der rewrite-uri-Richtlinie an Azure Storage weiterleiten. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte in [Festlegen oder Bearbeiten von Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [Richtlinienbeispiele](../policy-samples.md)


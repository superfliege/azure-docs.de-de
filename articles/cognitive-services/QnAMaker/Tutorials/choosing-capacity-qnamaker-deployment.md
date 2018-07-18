---
title: Wählen der Kapazität für Ihre QnA Maker-Bereitstellung – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Leitfaden für die Wahl der Kapazität für Ihre QnA Maker-Bereitstellung
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b0219b9f7dbbee52406dab9d808134fa2e2a689d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376314"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Wählen der Kapazität für Ihre QnA Maker-Bereitstellung

Der QnA Maker-Dienst weist Abhängigkeiten von drei Azure-Ressourcen auf:
1.  App Service (für die Laufzeit)
2.  Azure Search (für das Speichern von Fragen und Antworten)
3.  App Insights (optional, zum Speichern von Chatprotokollen und Telemetriedaten)

Bevor Sie Ihren QnA Maker-Dienst erstellen, sollten Sie sich entscheiden, welche Tarife der oben aufgeführten Dienste für Ihre Zwecke passend sind. 

Es gibt in der Regel drei Parameter, die Sie berücksichtigen müssen:
1. **Den Durchsatz, den Sie vom Dienst benötigen**: Wählen Sie den passenden [App-Plan](https://azure.microsoft.com/en-in/pricing/details/app-service/plans/) für Ihren App Service auf der Grundlage Ihrer Anforderungen aus. Sie können die App [zentral hochskalieren](https://docs.microsoft.com/en-us/azure/app-service/web-sites-scale) oder zentral herunterskalieren. Dies sollte auch Einfluss auf Ihre Wahl der Azure Search SKU haben; weitere Details dazu finden Sie [hier](https://docs.microsoft.com/en-us/azure/search/search-sku-tier).

2. **Größe und Anzahl der Wissensdatenbanken**: Wählen Sie die für Ihr Szenario passende [Azure Search-SKU](https://azure.microsoft.com/en-in/pricing/details/search/) aus. Sie können N-1 Wissensdatenbanken in einem bestimmten Tarif veröffentlichen, wobei N die Anzahl der im Tarif maximal zulässigen Indizes ist. Überprüfen Sie außerdem die maximale Größe und Anzahl der in den einzelnen Tarifen zulässigen Dokumente.

3. **Anzahl der Dokumente aus Quellen**: Die kostenlose SKU des QnA Maker-Management-Diensts begrenzt die Anzahl der Dokumente, die über das Portal und die APIs verwaltet werden können, auf 3 (bei einer Größe von jeweils 1 MB). Die Standard-SKU ist in der Anzahl der Dokumente, die Sie verwalten können, nicht begrenzt. Ausführlichere Informationen finden Sie [hier](https://aka.ms/qnamaker-pricing).

Die folgende Tabelle gibt Ihnen einige allgemeine Richtlinien.

|                        | QnA Maker Management | App Service | Azure Search | Einschränkungen                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentieren        | Kostenlose SKU             | Free-Tarif   | Free-Tarif    | Veröffentlichen von bis zu 2 KB bei einer Größe von 50 MB  |
| Entwicklungs-/Testumgebung   | Standard-SKU         | Shared      | Basic        | Veröffentlichen von bis zu 4 KB bei einer Größe von 2 GB    |
| Produktionsumgebung | Standard-SKU         | Basic       | Standard     | Veröffentlichen von bis zu 49 KB bei einer Größe von 25 GB |

Informationen zum Ausführen von Upgrades für Ihren QnA Maker-Stapel finden Sie unter [Upgrade Ihres QnA Maker-Diensts](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Upgrade Ihres QnA Maker-Diensts](../How-To/upgrade-qnamaker-service.md)

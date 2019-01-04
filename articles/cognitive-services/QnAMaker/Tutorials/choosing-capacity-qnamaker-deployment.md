---
title: 'Ressourcenkapazität für die Bereitstellung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Bevor Sie Ihren QnA Maker-Dienst erstellen, sollten Sie sich entscheiden, welcher Tarif der oben aufgeführten Dienste für Ihre Zwecke geeignet ist.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9e197929ce08f4e0c665f96d1c4ddbd382fdfb22
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084453"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Wählen der Kapazität für Ihre QnA Maker-Bereitstellung

Der QnA Maker-Dienst weist Abhängigkeiten von drei Azure-Ressourcen auf:
1.  App Service (für die Laufzeit)
2.  Azure Search (für das Speichern von Fragen und Antworten)
3.  App Insights (optional, zum Speichern von Chatprotokollen und Telemetriedaten)

Bevor Sie Ihren QnA Maker-Dienst erstellen, sollten Sie sich entscheiden, welcher Tarif der oben aufgeführten Dienste für Ihre Zwecke geeignet ist. 

Es gibt in der Regel drei Parameter, die Sie berücksichtigen müssen:
1. **Den Durchsatz, den Sie vom Dienst benötigen:** Wählen Sie den passenden [App-Plan](https://azure.microsoft.com/pricing/details/app-service/plans/) für Ihren App Service auf der Grundlage Ihrer Anforderungen aus. Sie können die App [zentral hochskalieren](https://docs.microsoft.com/azure/app-service/web-sites-scale) oder zentral herunterskalieren. Dies sollte auch Einfluss auf Ihre Wahl der Azure Search SKU haben; weitere Details dazu finden Sie [hier](https://docs.microsoft.com/azure/search/search-sku-tier).

2. **Größe und Anzahl von Wissensdatenbanken:** Wählen Sie eine geeignete [Azure Search-SKU](https://azure.microsoft.com/pricing/details/search/) für Ihr Szenario aus. Sie können N-1 Wissensdatenbanken in einem bestimmten Tarif veröffentlichen, wobei N die Anzahl der im Tarif maximal zulässigen Indizes ist. Überprüfen Sie außerdem die maximale Größe und Anzahl der in den einzelnen Tarifen zulässigen Dokumente.

3. **Anzahl von Dokumenten als Quellen:** Die kostenlose SKU des QnA Maker-Verwaltungsdiensts begrenzt die Anzahl der Dokumente, die über das Portal und die APIs verwaltet werden können, auf 3 (bei einer Größe von jeweils 1 MB). Die Standard-SKU ist in der Anzahl der Dokumente, die Sie verwalten können, nicht begrenzt. Ausführlichere Informationen finden Sie [hier](https://aka.ms/qnamaker-pricing).

Die folgende Tabelle gibt Ihnen einige allgemeine Richtlinien.

|                        | QnA Maker Management | App Service | Azure Search | Einschränkungen                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentieren        | Kostenlose SKU             | Free-Tarif   | Free-Tarif    | Veröffentlichen von bis zu 2 KB bei einer Größe von 50 MB  |
| Entwicklungs-/Testumgebung   | Standard-SKU         | Shared      | Basic        | Veröffentlichen von bis zu 14 KB bei einer Größe von 2 GB    |
| Produktionsumgebung | Standard-SKU         | Basic       | Standard     | Veröffentlichen von bis zu 49 KB bei einer Größe von 25 GB |

Informationen zum Ausführen von Upgrades für Ihren QnA Maker-Stapel finden Sie unter [Upgrade Ihres QnA Maker-Diensts](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Upgrade Ihres QnA Maker-Diensts](../How-To/upgrade-qnamaker-service.md)

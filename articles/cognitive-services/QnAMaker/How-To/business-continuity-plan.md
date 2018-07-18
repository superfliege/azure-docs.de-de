---
title: Erstellen eines Geschäftskontinuitätsplans für Ihren QnA Maker-Dienst – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: 'Gewusst wie: Erstellen eines Geschäftskontinuitätsplans für Ihren QnA Maker-Dienst'
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: ca6e54b8a8ca8b38e8ef6b1a148f8b2c54bd43da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376210"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Erstellen eines Geschäftskontinuitätsplans für Ihren QnA Maker-Dienst

Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.

![QnA Maker-Geschäftskontinuitätsplan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Das oben dargestellte allgemeine Konzept lautet wie folgt:

1. Richten Sie zwei parallele [QnA Maker-Dienste](../How-To/set-up-qnamaker-service-azure.md) in [Azure-Regionspaaren](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) ein.

2. Halten Sie die primären und sekundären Azure-Suchindizes synchron. Verwenden Sie das [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) aufgeführte Github-Beispiel, um zu erfahren, wie Azure-Indizes gesichert und wiederhergestellt werden.

3. Sichern Sie die Application Insights-Daten mithilfe des [fortlaufenden Exports](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry).

4. Verwenden Sie [Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/) nach der Einrichtung der primären und sekundären Stapel, um die zwei Endpunkte zu konfigurieren und eine Routingmethode einzurichten.

5. Sie müssen ein SSL-Zertifikat für Ihren Traffic Manager-Endpunkt erstellen. [Binden Sie das SSL-Zertifikat](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-ssl) an Ihre App-Dienste.

6. Verwenden Sie dann den Traffic Manager-Endpunkt in Ihrem Bot oder in Ihrer App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen der Kapazität für Ihre QnA Maker-Bereitstellung](../Tutorials/choosing-capacity-qnamaker-deployment.md)

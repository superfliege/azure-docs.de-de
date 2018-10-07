---
title: 'Einrichten eines QnA Maker-Diensts: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 87bcd23983c1d973f7e90caf806092c909b7b9e4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032545"
---
# <a name="create-a-qna-maker-service"></a>Erstellen eines QnA Maker-Diensts

Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.

Bei diesem Setup werden einige Azure-Ressourcen bereitgestellt. In Kombination verwalten diese Ressourcen die Inhalte der Wissensdatenbank und stellen die Fähigkeit zum Beantworten von Fragen über einen Endpunkt zur Verfügung.

1. Melden Sie sich beim [Azure-Portal](<https://portal.azure.com>)an.

2.  Klicken Sie auf **Neue Ressource hinzufügen**, geben Sie im Suchfeld „qna maker“ ein, und wählen Sie die QnA Maker-Ressource aus.

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Klicken Sie auf **Erstellen**, nachdem Sie die Geschäftsbedingungen gelesen haben.

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. Wählen Sie in **QnA Maker** die geeigneten Tarife und Regionen aus.

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Setzen Sie in **Name** einen eindeutigen Namen zur Identifizierung dieses QnA Maker-Diensts ein. Dieser Name kennzeichnet darüber hinaus den QnA Maker-Endpunkt, dem Ihre Wissensdatenbanken zugeordnet werden.
    * Wählen Sie das **Abonnement** aus, in dem die QnA Maker-Ressource bereitgestellt werden soll.
    * Wählen Sie den **Tarif für Verwaltung** für die QnA Maker-Verwaltungsdienste (Portal und Verwaltungs-APIs) aus. Details zur den Preisen für SKUs finden Sie [hier](https://aka.ms/qnamaker-pricing).
    * Erstellen Sie eine neue **Ressourcengruppe** (empfohlen), oder verwenden Sie eine vorhandene Ressourcengruppe, in der diese QnA Maker-Ressource bereitgestellt werden soll.
    * Wählen Sie den **Tarif für Suche** des Azure Search-Diensts aus. Wenn die Free-Tarifoption abgeblendet angezeigt wird, bedeutet dies, dass Sie bereits einen kostenlosen Azure Search-Tarif in Ihrem Abonnement bereitgestellt haben. In diesem Fall müssen Sie mit dem Basic Azure Search-Tarif einsteigen. Details zu den Preisen von Azure Search finden Sie [hier](https://azure.microsoft.com/en-us/pricing/details/search/).
    * Wählen Sie den **Suchspeicherort** aus, an dem die Azure Search-Daten bereitgestellt werden sollen. Einschränkungen zu den verpflichtenden Speicherorten für Kundendaten ergeben sich durch Ihre Wahl des Speicherorts für Azure Search.
    * Geben Sie Ihrem App Service unter **App-Name** einen Namen.
    * Standardmäßig wird der App Service nach dem Standardtarif (S1) abgerechnet. Sie können den Plan nach der Erstellung ändern. Weitere Details zu App Service-Preisen finden Sie [hier](https://azure.microsoft.com/en-in/pricing/details/app-service/).
    * Wählen Sie den **Website-Speicherort** aus, an dem der App Service bereitgestellt wird.

        > [!NOTE]
        > Der Suchspeicherort kann vom Website-Speicherort abweichen.

    * Wählen Sie, ob Sie **Application Insights** aktivieren möchten. Wenn **Application Insights** aktiviert ist, sammelt QnA Maker Telemetriedaten zu Datenverkehr, Chatprotokollen und Fehlern.
    * Wählen Sie den **App Insights-Speicherort** aus, an dem die Application Insights-Ressource bereitgestellt werden soll.

5. Nachdem alle Felder überprüft wurden, können Sie auf **Erstellen** klicken, um die Bereitstellung dieser Dienste in Ihrem Abonnement zu starten. Dies nimmt einige Minuten in Anspruch.

6.  Nachdem die Bereitstellung abgeschlossen ist, sehen Sie die folgenden neu erstellten Ressourcen in Ihrem Abonnement.

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen und Veröffentlichen einer Wissensdatenbank](../Quickstarts/create-publish-knowledge-base.md)
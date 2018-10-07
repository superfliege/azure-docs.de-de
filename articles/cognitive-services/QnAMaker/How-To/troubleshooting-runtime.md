---
title: 'Problembehandlung: QnA Maker'
titlesuffix: Azure Cognitive Services
description: QnA Maker besteht aus Komponenten, die im Azure-Konto des Benutzers gehostet werden. Für das Debuggen kann es notwendig sein, dass Benutzer ihre Azure-Ressourcen für QnA Maker bearbeiten oder dem QnA Maker-Supportteam zusätzliche Informationen zu ihrer Installation bereitstellen.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: ad3c01ebf37c8b544830b281144090694eeadfcd
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033725"
---
# <a name="qnamaker-troubleshooting"></a>QnA Maker – Problembehandlung
QnA Maker besteht aus Komponenten, die im Azure-Konto des Benutzers gehostet werden. Für das Debuggen kann es notwendig sein, dass Benutzer ihre Azure-Ressourcen für QnA Maker bearbeiten oder dem QnA Maker-Supportteam zusätzliche Informationen zu ihrer Installation bereitstellen.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Abrufen der neuesten Updates der QnA Maker-Runtime
Die QnA Maker-Runtime ist Teil von Azure App Service, einem beim [Erstellen eines QnA Maker-Diensts](./set-up-qnamaker-service-azure.md) im Azure-Portal bereitgestellten Dienst. Updates für die Runtime werden in regelmäßigen Abständen bereitgestellt. Um die neuesten Updates auf Ihre QnA Maker-Installation anzuwenden, müssen Sie App Service neu starten.
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem QnA Maker-Dienst (Ressourcengruppe).

    ![Azure-Ressourcengruppe von QnA Maker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klicken Sie auf „App Service“, und öffnen Sie den Abschnitt „Übersicht“.

     ![QnA Maker in App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Starten Sie App Service neu. Dieser Vorgang sollte innerhalb weniger Sekunden abgeschlossen sein. Beachten Sie, dass Ihre Downstreamanwendungen/-bots, die auf diesem QnA Maker-Dienst beruhen, während dieses Neustarts nicht für Endbenutzer verfügbar sind.

    ![Neustarten von App Service QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Abrufen des Namens des QnA Maker-Diensthosts
Der Name des QnA Maker-Diensthosts ist beim Debuggen hilfreich, wenn Sie sich an den QnA Maker-Support oder UserVoice wenden. Der Hostname ist eine URL der Form: https://*{Hostname}*.azurewebsites.net.
    
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem QnA Maker-Dienst (Ressourcengruppe).

    ![Azure-Ressourcengruppe von QnA Maker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klicken auf App Service

     ![QnA Maker in App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Sie finden die Hostnamen-URL im Abschnitt „Übersicht“.

    ![QnA Maker-Hostname](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden der QnA Maker-API](./upgrade-qnamaker-service.md)

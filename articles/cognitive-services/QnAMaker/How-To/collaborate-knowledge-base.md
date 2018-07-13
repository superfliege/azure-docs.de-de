---
title: Zusammenarbeiten an einer Wissensdatenbank – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Informationen zum Zusammenarbeiten an einer QnA Maker-Wissensdatenbank
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e18d656236276595fc5186a6656349bf28974ead
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376163"
---
# <a name="collaborate-on-your-knowledge-base"></a>Zusammenarbeiten an einer Wissensdatenbank

QnA Maker ermöglicht mehreren Personen, an einer Wissensdatenbank zusammenzuarbeiten. Diese Funktion wird im Rahmen der [rollenbasierten Zugriffssteuerung](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) in Azure bereitgestellt. 

Führen Sie die folgenden Schritte aus, um Ihren QnA Maker-Dienst für andere freizugeben:

1. Melden Sie sich im Azure-Portal an, und gehen Sie zu Ihrer QnA Maker-Ressource.

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Öffnen Sie die Registerkarte **Zugriffssteuerung (IAM)**.

    ![IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Wählen Sie **Hinzufügen**.

    ![Hinzufügen von IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Wählen Sie die Rolle **Besitzer** oder **Mitwirkender** aus.

    ![Hinzufügen einer IAM-Rolle in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Geben Sie die E-Mail-Adresse der Person ein, für die Sie den Dienst freigeben möchten, und klicken Sie auf „Speichern“.

    ![Hinzufügen einer E-Mail-Adresse für IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Wenn sich eine Person, für die Sie Ihren QnA Maker-Dienst freigegeben haben, im [QnA Maker-Portal](https://qnamaker.ai) anmeldet, sieht sie nun alle Wissensdatenbanken in diesem Dienst.

Es ist nicht möglich, eine bestimmte Wissensdatenbank in einem QnA Maker-Dienst freizugeben. Wenn Sie eine differenzierte Zugriffssteuerung wünschen, verteilen Sie Ihre Wissensdatenbanken auf verschiedene QnA Maker-Dienste.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Testen einer Wissensdatenbank](./test-knowledge-base.md)

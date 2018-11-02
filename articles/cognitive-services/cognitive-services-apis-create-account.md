---
title: Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal
titlesuffix: Azure Cognitive Services
description: Anleitung zum Erstellen eines Microsoft Cognitive Services-APIs-Kontos im Azure-Portal.
services: cognitive-services
author: garyericson
manager: cgronlun
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: garye
ms.openlocfilehash: 37f53303a3b0c224c1286fb488a796fd5cdee0e5
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386415"
---
# <a name="quickstart-create-a-cognitive-services-account-in-the-azure-portal"></a>Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal

Verwenden Sie diese Schnellstartanleitung, um mit der Verwendung von Azure Cognitive Services zu beginnen. Diese Dienste werden durch Azure-[Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) repräsentiert, die es Ihnen ermöglichen, sich mit einer oder mehreren der vielen verfügbaren Cognitive Services-APIs zu verbinden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement. Sie können [ein Konto kostenlos erstellen](https://azure.microsoft.com/free/).

## <a name="create-and-subscribe-to-an-azure-cognitive-services-resource"></a>Erstellen und Abonnieren einer Azure Cognitive Services-Ressource

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an, und klicken Sie auf **+Ressource erstellen**.
    
    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azurePortalScreen.png)

2. Wählen Sie unter Azure Marketplace die Option **KI und Machine Learning** aus. Wenn der für Sie interessante Dienst nicht angezeigt wird, klicken Sie auf **Alle anzeigen**, um den gesamten Katalog der Cognitive Services-APIs anzuzeigen.

    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:

    |    |    |
    |--|--|
    | **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Es wird empfohlen einen aussagekräftigen Namen zu wählen, z. B. *MyNameFaceAPIAccount*. |
    | **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
    | **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
    | **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Ressourcengruppe** | Die [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/azure-resource-access#what-is-an-azure-resource-group), die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie zu einer bereits bestehenden Gruppe hinzufügen. |

    ![Bildschirm „Ressourcenerstellung“](media/cognitive-services-apis-create-account/resource_create_screen.png)

## <a name="access-your-resource"></a>Zugreifen auf Ihre Ressource 

> [!NOTE]
> Abonnementbesitzer können die Erstellung eines Cognitive Services-Kontos für Ressourcengruppen und Abonnements durch Anwenden von [Azure-Richtlinien](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition) deaktivieren, indem die Richtliniendefinition „Nicht zulässige Ressourcentypen“ zugewiesen und **Microsoft.CognitiveServices/accounts** als Zielressourcentyp angegeben wird.

Nachdem Sie Ihre Ressource erstellt haben, können Sie sie über das Azure Dashboard aufrufen, wenn Sie sie angeheftet haben. Andernfalls finden Sie sie in **Ressourcengruppen**.

Innerhalb Ihrer Cognitive Services-Ressource können Sie die Endpunkt-URL und Schlüssel im Abschnitt **Übersicht** verwenden, um API-Aufrufe in Ihren Anwendungen zu starten.

![Bildschirm „Ressourcen“](media/cognitive-services-apis-create-account/resourceScreen.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial zur Maschinelles Sehen-API in C#](https://docs.microsoft.com/azure/cognitive-services/computer-vision/tutorials/csharptutorial)

## <a name="see-also"></a>Weitere Informationen

* [Schnellstart: Extrahieren von handschriftlichem Text aus einem Bild](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/csharp-hand-text)
* [Tutorial: Erstellen einer App zum Erkennen und Umranden von Gesichtern in einem Bild](https://docs.microsoft.com/azure/cognitive-services/Face/Tutorials/FaceAPIinCSharpTutorial)
* [Erstellen einer Webseite für die benutzerdefinierte Suche](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page)
* [Integrate Language Understanding (LUIS) mit einem Bot über das Bot Framework](https://docs.microsoft.com/azure/cognitive-services/luis/luis-nodejs-tutorial-build-bot-framework-sample)

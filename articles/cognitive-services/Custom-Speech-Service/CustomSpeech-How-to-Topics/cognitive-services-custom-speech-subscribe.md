---
title: Abrufen von Abonnementschlüsseln für Custom Speech Service in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Abonnementschlüssel für Aufrufe für Custom Speech Service in Cognitive Services abrufen können.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: e5cac34a9aead209bec90cfd5f89e94a005a1995
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980354"
---
# <a name="obtain-subscription-keys"></a>Erhalten von Abonnementschlüsseln
Zunächst müssen Sie mit Azure Custom Speech Service Ihr Benutzerkonto mit einem Azure-Abonnement verknüpfen. Es stehen Abonnements im Free-Tarif und in zahlungspflichtigen Tarifen zur Verfügung. Informationen zu den Tarifen finden Sie auf der [Seite mit den Preisangaben](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Abrufen eines Abonnementschlüssels
1. Sie können einen Abonnementschlüssel über zwei Arten aus dem Azure-Portal abrufen:

    * Navigieren Sie zum [Azure-Portal](https://ms.portal.azure.com), und fügen Sie eine neue Cognitive Services-API hinzu, indem Sie nach _Cognitive Services_ suchen und anschließend **Cognitive Services-APIs** auswählen.

      ![Suche nach Cognitive Services](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Navigieren Sie direkt zu den [Cognitive Services-APIs](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![Cognitive Services-APIs](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
1. Füllen Sie die erforderlichen Felder wie folgt aus:

      a. **Kontoname**. Wählen Sie den gewünschten Namen aus. Notieren Sie sich diesen Namen, um Ihr Cognitive Services-Abonnement in der Ressourcenliste finden zu können.

      b. **Abonnement**. Wählen Sie eines Ihrer Azure-Abonnements aus.

      c. **API-Typ**. Wählen Sie **Custom Speech Service (Vorschau)** aus.

      d. **Standort**. Dies ist derzeit **USA, Westen**.

      e. **Tarif**. Wählen Sie den gewünschten Tarif aus. **F0** ist der Free-Tarif. Die zulässigen Kontingente werden auf der [Seite mit den Preisen](https://www.microsoft.com/cognitive-services/en-us/pricing) erläutert.

      ![Erstellung eines Cognitive Services-Kontos](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

1. Sie sollten entweder eine Ansicht auf Ihrem Dashboard oder einen Dienst mit dem angegebenen Kontonamen in Ihrer Ressourcenliste vorfinden. Wenn Sie diese Option auswählen, erhalten Sie eine Übersicht über Ihren Dienst. Wählen Sie in der Liste auf der linken Seite unter **Ressourcenverwaltung** die Option **Schlüssel** aus. Kopieren Sie **SCHLÜSSEL 1**.

      Dieser Abonnementschlüssel ist in den nächsten Schritten erforderlich.

      ![SCHLÜSSEL 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Kopieren Sie nicht die **Abonnement-ID** auf der Übersichtsseite. Sie benötigen den Abonnementschlüssel im nächsten Schritt.
      >

      ![Übersicht über die Abonnement-ID](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

1. Wählen Sie zum Eingeben Ihres Abonnementschlüssels auf dem Menüband in der oberen rechten Ecke Ihr Benutzerkonto aus. Wählen Sie im Dropdownmenü die Option **Abonnements** aus.

      ![Menüelement „Abonnements“](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    Eine Tabelle mit Abonnements wird angezeigt, die beim erstmaligen Öffnen leer ist.

    ![Abonnementtabelle](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

1. Wählen Sie **Neue hinzufügen** aus. Geben Sie einen Namen für das Abonnement und den Abonnementschlüssel ein. Dieser kann entweder **SCHLÜSSEL 1** (Primärschlüssel) oder **SCHLÜSSEL 2** (Sekundärschlüssel) aus Ihrem Abonnement lauten.

      ![Name des Abonnementschlüssels](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Um Daten hochzuladen, ein Modell zu trainieren oder eine Bereitstellung durchzuführen, müssen Sie Ihre benutzerdefinierten Custom Speech Service-Aktivitäten mit einem Azure-Abonnement verknüpfen. Hierbei kann es sich entweder um ein Abonnement im Free-Tarif oder in einem kostenpflichtigen Tarif handeln. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-id"></a>Abrufen einer Abonnement-ID
Um eine Abonnement-ID abzurufen, navigieren Sie zum Azure-Portal. Suchen Sie nach *Cognitive Services* und *Custom Speech Service*, und befolgen Sie die Anweisungen.

> [!NOTE]
> Der Abonnementschlüssel wird im Laufe dieses Prozesses benötigt.
>

## <a name="next-steps"></a>Nächste Schritte
* Beginnen Sie mit der Erstellung Ihres [benutzerdefinierten Akustikmodells](cognitive-services-custom-speech-create-acoustic-model.md).
* Beginnen Sie mit der Erstellung Ihres [benutzerdefinierten Sprachmodells](cognitive-services-custom-speech-create-language-model.md).

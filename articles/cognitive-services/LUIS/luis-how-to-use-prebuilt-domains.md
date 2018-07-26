---
title: Verwenden von vordefinierten Domänen in LUIS-Apps in Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über die Verwendung von vordefinierten Domänen in LUIS-Anwendungen (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: diberry
ms.openlocfilehash: df57f9adf5bf7f5f652a77ddeafe950463c6a9fc
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224176"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Verwenden von vordefinierten Domänen in LUIS-Apps  

Language Understanding Intelligent Service (LUIS) bietet *vordefinierte Domänen*. Dabei handelt es sich um vordefinierte Sätze von [Absichten](luis-how-to-add-intents.md) und [Entitäten](luis-concept-entity-types.md), die für Domänen oder allgemeine Kategorien von Clientanwendungen zusammenarbeiten. Die vordefinierten Domänen wurden bereits trainiert und können direkt Ihrer LUIS-App hinzugefügt werden. Die Absichten und Entitäten in einer vordefinierten Domäne sind vollständig anpassbar, sobald Sie sie der App hinzugefügt haben – Sie können sie mit Äußerungen aus Ihrem System trainieren, damit sie für Ihre Benutzer geeignet sind. Sie können eine gesamte vordefinierte Domäne als Ausgangspunkt für die Anpassung verwenden oder nur einige Absichten oder Entitäten aus einer vordefinierten Domäne übernehmen. 

Durchsuchen Sie die Registerkarte **Prebuilt domains** (Vordefinierte Domänen), um andere vordefinierte Domänen anzuzeigen, die Sie in Ihrer App verwenden können. Klicken Sie auf die Kachel für eine Domäne, um diese Ihrer App hinzuzufügen, oder klicken Sie in der Kachel auf **Learn more** (Mehr erfahren), um weitere Informationen zu den Absicht und Entitäten zu erhalten.

> [!TIP]
> Eine vollständige Auflistung der vordefinierten Domänen finden Sie in der [Referenz zu vordefinierten Domänen](./luis-reference-prebuilt-domains.md).

![Hinzufügen einer vordefinierten Domäne](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne
Suchen Sie auf der Registerkarte **Prebuilt domains** (Vordefinierte Domänen) die Domäne RestaurantReservation, und klicken Sie auf **Domäne hinzufügen**. Nachdem die vordefinierte Domäne Ihrer LUIS-App hinzugefügt wurde, öffnen Sie die **Absichten**, und klicken Sie auf die Absicht „RestaurantReservation.Reserve“. Sie sehen, dass viele Beispieläußerungen bereits angegeben und mit Entitäten bezeichnet wurden.

![Absicht „RestaurantReservation.Reserve“](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Entwerfen von LUIS-Apps aus vordefinierten Domänen
Wenn Sie in Ihrer LUIS-App vordefinierte Domänen verwenden, können Sie eine gesamte vordefinierte Domäne anpassen oder nur mit einigen der enthaltenen Absichten und Entitäten beginnen.

## <a name="customizing-an-entire-prebuilt-domain"></a>Anpassen einer gesamten vordefinierten Domäne
Vordefinierte Domänen sind allgemein ausgelegt. Sie enthalten viele Absichten und Entitäten, in denen Sie eine Auswahl treffen können, um eine App an Ihre Anforderungen anzupassen. Wenn Sie eine gesamte vordefinierte Domäne anpassen, löschen Sie die Absichten und Entitäten, die Ihre Anwendung nicht verwenden muss. Sie können auch der Gruppe einige Absichten oder Entitäten hinzufügen, die die vordefinierte Domäne bereits enthält. Wenn Sie beispielsweise die vordefinierte Domäne **Ereignisse** für eine Sportveranstaltungs-App verwenden, können Sie Entitäten für Sportteams hinzufügen. Wenn Sie in LUIS [Äußerungen bereitstellen](luis-how-to-add-example-utterances.md), schließen Sie Begriffe ein, die für Ihre App spezifisch sind. LUIS lernt, diese zu erkennen, und passt die Absichten und Entitäten der vordefinierten Domäne an die Anforderungen Ihrer App an. 

> [!TIP]
> Die Absichten und Entitäten in einer vordefinierten Domäne funktionieren am besten zusammen. Es ist besser, nach Möglichkeit Absichten und Entitäten aus derselben Domäne zu kombinieren.
> * Eine bewährte Methode ist die Verwendung von verwandten Absichten aus derselben Domäne. Wenn Sie beispielsweise die Absicht `MakeReservation` in der Domäne **Places** anpassen, wählen Sie die Absicht `Cancel` aus der Domäne **Places** aus und nicht die Absicht „Cancel“ aus einer der Domänen **Events** oder **Utilities**.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Ändern des Verhaltens der Absicht einer vordefinierten Domäne
Möglicherweise enthält eine vordefinierte Domäne eine Absicht, die einer Absicht, die Sie in Ihrer LUIS-App haben möchten, ähnelt, Sie möchten das Verhalten jedoch ändern. Beispielsweise enthält die vordefinierte Domäne **Places** die Absicht `MakeReservation` zum Durchführen einer Restaurantreservierung, aber Ihre App soll mit dieser Absicht Hotelreservierungen vornehmen. In diesem Fall können Sie das Verhalten dieser Absicht ändern, indem Sie LUIS Äußerungen zu Hotelreservierungen bereitstellen und diese mithilfe der Absicht `MakeReservation` bezeichnen. LUIS kann dann neu trainiert werden, sodass die Absicht `MakeReservation` in einer Anforderung zum Buchen eines Hotelzimmers erkannt wird.

> [!TIP]
> In der Domäne „Utilities“ finden Sie vordefinierte Absichten, die Sie für die Verwendung in beliebigen Domänen anpassen können. Beispielsweise können Sie Ihrer App `Utilities.Repeat` hinzufügen und für die Aktionen trainieren, die Benutzer in Ihrer Anwendung wiederholen möchten.


## <a name="next-step"></a>Nächster Schritt

Anpassen einer vordefinierten Domäne durch Hinzufügen weiterer Beispieläußerungen.

> [!div class="nextstepaction"]
> [Hinzufügen von Beispieläußerungen](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Ausführliche Informationen zu den vordefinierten Domänen finden Sie in der [Referenz zu vordefinierten Domänen](./luis-reference-prebuilt-domains.md).

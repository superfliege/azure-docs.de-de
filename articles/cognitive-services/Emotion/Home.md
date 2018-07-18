---
title: Übersicht über die Emotionen-API | Microsoft-Dokumentation
description: Mit dem innovativen, cloudbasierten Algorithmus von Microsoft für die Emotionserkennung mit der Emotionen-API in Cognitive Services den Personalisierungsgrad Ihrer Apps erhöhen
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 8383370cba3f78060e809f444f4ad3dab7380f4e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374314"
---
# <a name="what-is-emotion-api"></a>Worum handelt es sich bei der Emotionen-API?

> [!IMPORTANT]
> Die Emotionen-API wurde am 30. Oktober 2017 eingestellt. Die Funktionalität ist nun Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/en-us/azure/cognitive-services/face/).

Mit der Emotionen-API können Sie dank dem cloudbasierten Microsoft-Algorithmus für die Erkennung von Emotionen den Personalisierungsgrad Ihrer Apps erhöhen.

### <a name="emotion-recognition"></a>Emotionserkennung

Die Betaversion der Emotionen-API nimmt mithilfe der Gesichtserkennungs-API ein Bild als Eingabe und gibt für jedes Gesicht im Bild den Übereinstimmungsgrad mit verschiedenen Emotionen sowie einen Begrenzungsrahmen für das Gesicht zurück. Folgende Emotionen werden erkannt: Freude, Trauer, Überraschung, Wut, Angst, Verachtung, Ekel und Neutralität. Diese Emotionen werden kulturübergreifend und universell über die gleichen grundlegenden Gesichtsausdrücke kommuniziert, die die Emotion-API identifiziert. 

**Interpretieren der Ergebnisse:** 

Die von der Emotionen-API erkannte Emotion ist diejenige mit der höchsten Punktzahl. Alle Werte werden so normalisiert, dass ihre Summe Eins ergibt. Benutzer können nach Bedarf in ihrer Anwendung einen höheren Übereinstimmungsschwellenwert festlegen. 

Weitere Informationen zur Emotionserkennung finden Sie in der API-Referenz: 
  * Basic: Wenn ein Benutzer die Gesichtserkennungs-API bereits aufgerufen hat, kann er das Gesichtsrechteck als Eingabe übermitteln und den Modus „Basic“ verwenden. [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standard: Wenn ein Benutzer kein Gesichtsrechteck übermittelt, ist der Modus „Standard“ geeignet.  [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Ein Beispiel dafür, wie Sie Streamingvideos mit der Emotionen-API interpretieren, finden Sie unter [Videoanalyse in Echtzeit](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).

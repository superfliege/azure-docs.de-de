---
title: Was ist die Emotionen-API?
titlesuffix: Azure Cognitive Services
description: Mit dem cloudbasierten Algorithmus für die Erkennung von Emotionen können Sie den Personalisierungsgrad Ihrer Apps erhöhen.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: overview
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: abf94e0ab6ebb3df649b1958503c086feb4fa19e
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237125"
---
# <a name="what-is-the-emotion-api"></a>Was ist die Emotionen-API?

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar. 

Mit der Emotionen-API können Sie dank dem cloudbasierten Microsoft-Algorithmus für die Erkennung von Emotionen den Personalisierungsgrad Ihrer Apps erhöhen.

### <a name="emotion-recognition"></a>Emotionserkennung

Die Betaversion der Emotionen-API nimmt mithilfe der Gesichtserkennungs-API ein Bild als Eingabe und gibt für jedes Gesicht im Bild den Übereinstimmungsgrad mit verschiedenen Emotionen sowie einen Begrenzungsrahmen für das Gesicht zurück. Folgende Emotionen werden erkannt: Freude, Trauer, Überraschung, Wut, Angst, Verachtung, Ekel und Neutralität. Diese Emotionen werden kulturübergreifend und universell über die gleichen grundlegenden Gesichtsausdrücke kommuniziert, die die Emotion-API identifiziert.

**Interpretieren der Ergebnisse:**

Die von der Emotionen-API erkannte Emotion ist diejenige mit der höchsten Punktzahl. Alle Werte werden so normalisiert, dass ihre Summe Eins ergibt. Benutzer können nach Bedarf in ihrer Anwendung einen höheren Übereinstimmungsschwellenwert festlegen.

Weitere Informationen zur Emotionserkennung finden Sie in der API-Referenz:
  * Basic: Wenn ein Benutzer die Gesichtserkennungs-API bereits aufgerufen hat, kann er das Gesichtsrechteck als Eingabe übermitteln und den Modus „Basic“ verwenden. [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standard: Wenn ein Benutzer kein Gesichtsrechteck übermittelt, ist der Modus „Standard“ geeignet.  [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Ein Beispiel dafür, wie Sie Streamingvideos mit der Emotionen-API interpretieren, finden Sie unter [Videoanalyse in Echtzeit](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).

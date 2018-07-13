---
title: Tutorial zur Emotionen-API in Python | Microsoft-Dokumentation
description: Lernen Sie mithilfe eines Jupyter Notebooks die Verwendung der Cognitive Services-Emotionen-API mit Python kennen. Zeigen Sie Ihre Ergebnisse mithilfe gängiger Bibliotheken an.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 70c8ca48c651601f3d7cbb3717c32bfe112176fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374091"
---
# <a name="emotion-api-using-python-tutorial"></a>Tutorial zur Emotionen-API in Python

> [!IMPORTANT]
> Die Video-API-Vorschau ist am 30. Oktober 2017 abgelaufen. Testen Sie die neue [Video Indexer-API (Vorschau)](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Extrahieren Sie Erkenntnisse aus Videos, und verbessern Sie die Auffindbarkeit von Inhalten, z.B. mit Suchergebnissen durch die Erkennung von gesprochenen Wörtern, Gesichtern, Zeichen und Emotionen. [Weitere Informationen](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview)

Um Ihnen den Einstieg in die Emotionen-API zu erleichtern, zeigt Ihnen das Jupyter Notebook, das Sie mit dem folgenden Link aufrufen können, wie Sie die API in Python verwenden und Ihre Ergebnisse mit einigen beliebten Bibliotheken anzeigen können. 

[Link zum Notebook in GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Verwenden von Jupyter Notebook

Um das Notebook interaktiv zu verwenden, müssen Sie es klonen und in Jupyter ausführen. Um zu erfahren, wie Sie den Einstieg mit interaktiven Jupyter Notebooks durchführen, befolgen Sie die Anweisungen unter http://jupyter.readthedocs.org/en/latest/install.html. 

Um dieses Notizbuch verwenden zu können, benötigen Sie einen Abonnementschlüssel für die Emotionen-API. Besuchen Sie zur Registrierung die [Abonnementseite](https://azure.microsoft.com/try/cognitive-services/). Melden Sie sich auf der Seite „Anmelden“ mit Ihrem Microsoft-Konto an, und Sie können kostenlose Schlüssel abonnieren und abrufen. Fügen Sie Ihren Schlüssel nach Abschluss des Registrierungsvorgangs in den unten gezeigten Variablenabschnitt ein. Entweder der primäre oder sekundäre Schlüssel funktioniert.

```
Python Example 

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```

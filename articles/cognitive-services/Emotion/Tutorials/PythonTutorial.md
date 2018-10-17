---
title: 'Tutorial: Erkennen von Emotionen auf Gesichtern in einem Bild – Emotionen-API, Python'
titlesuffix: Azure Cognitive Services
description: Lernen Sie mithilfe eines Jupyter Notebooks die Verwendung der Emotionen-API mit Python kennen. Zeigen Sie Ihre Ergebnisse mithilfe gängiger Bibliotheken an.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 31e346cd9a3f43f8181ebee4474ae6c9ee2cc6fc
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237856"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Tutorial: Verwenden der Emotionen-API mit einem Jupyter Notebook und Python

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar. 

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

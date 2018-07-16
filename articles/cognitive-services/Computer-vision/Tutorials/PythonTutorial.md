---
title: Tutorial zur Maschinelles Sehen-API mit Python | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Maschinelles Sehen-API mit Python verwenden, indem Sie Jupyter-Notebooks in Microsoft Cognitive Services nutzen. Visualisieren Sie Ihre Ergebnisse mithilfe gängiger Bibliotheken.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: a093c2d066e70a8daf1fe1cd33ccf794ecb196af
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374066"
---
# <a name="computer-vision-api-python-tutorial"></a>Tutorial: Maschinelles Sehen-API in Python

Im Tutorial wird veranschaulicht, wie Sie die Maschinelles Sehen-API in Python verwenden und Ihre Ergebnisse mithilfe beliebter Bibliotheken visualisieren. Nutzen Sie Jupyter, um das Tutorial durchzuarbeiten. Informationen zum Einstieg in interaktive Jupyter-Notebooks finden Sie in der [Jupyter-Dokumentation](http://jupyter.readthedocs.io/en/latest/index.html). 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>Öffnen des Tutorial-Notebooks in Jupyter 

1. Navigieren Sie zum [Tutorial-Notebook auf GitHub](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Klicken Sie auf die grüne Schaltfläche, um das Tutorial zu klonen oder herunterzuladen. 
3. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner _Cognitive-Vision-Python-master\Jupyter Notebook_. 
4. Führen Sie an der Eingabeaufforderung den Befehl**jupyter notebook** aus. Jupyter wird gestartet.
5. Klicken Sie im Jupyter-Fenster auf _Computer Vision API Example.ipynb_, um das Tutorial-Notebook zu öffnen. 

### <a name="running-the-tutorial"></a>Ausführen des Tutorials

Um dieses Notizbuch verwenden zu können, benötigen Sie einen Abonnementschlüssel für die Maschinelles Sehen-API. Besuchen Sie zur Registrierung die [Abonnementseite](https://azure.microsoft.com/try/cognitive-services/). Melden Sie sich auf der Seite „Anmelden“ mit Ihrem Microsoft-Konto an. Anschließend können Sie kostenlose Schlüssel abonnieren und abrufen. Fügen Sie Ihren Schlüssel nach Abschluss des Registrierungsvorgangs in den Variablenabschnitt des Notebooks ein (unten dargestellt). Entweder der Primär- oder der Sekundärschlüssel funktioniert. Achten Sie darauf, den Schlüssel in Anführungszeichen zu setzen, um ihn zu einer Zeichenfolge zu machen.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```

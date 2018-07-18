---
title: Python-App zur Erkennung von Anomalien – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Lernen Sie ein Python-Notebook kennen, das die API zur Anomalieerkennung in Microsoft Cognitive Services verwendet. Senden Sie originale Datenpunkte an die API, und rufen Sie den erwarteten Wert und Anomaliepunkte ab.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: d35f41ddab21aa155376ad52ff4084298dab8fc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376146"
---
# <a name="anomaly-detection-python-application"></a>Python-Anwendung zur Anomalieerkennung

Das Tutorial veranschaulicht die Verwendung der API zur Anomalieerkennung in Python und die Visualisierung Ihrer Ergebnisse mithilfe beliebter Bibliotheken. Verwenden Sie Jupyter zur Ausführung des Tutorials, und probieren Sie eigene Daten mit Ihrem Abonnementschlüssel aus. Informationen zum Einstieg in interaktive Jupyter-Notebooks finden Sie in der [Jupyter-Dokumentation](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Voraussetzungen

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonnieren der Anomalieerkennung und Abrufen eines Abonnementschlüssels 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Herunterladen des Beispielcodes

1. Navigieren Sie zum [Tutorial-Notebook in Github](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Klicken Sie auf die grüne Schaltfläche, um das Tutorial zu klonen oder herunterzuladen. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Öffnen des Tutorial-Notebooks in Jupyter

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Ordner „python-sample“.
2. Führen Sie den Befehl „Jupyter notebook“ an der Eingabeaufforderung aus, wodurch Jupyter gestartet wird.
3. Klicken Sie im Jupyter-Fenster auf <em>Anomaly Detection API Example.ipynb</em>, um das Tutorial-Notebook zu öffnen.   

## <a name="running-the-tutorial"></a>Ausführen des Tutorials

Um dieses Notebook verwenden zu können, benötigen Sie einen Abonnementschlüssel für die Anomalieerkennungs-API. Suchen Sie die Abonnementseite auf, um sich zu registrieren. Melden Sie sich auf der Seite „Anmelden“ mit Ihrem Microsoft-Konto an. Dort können Sie abonnieren und Ihre Schlüssel abrufen. Fügen Sie Ihren Schlüssel nach Abschluss des Registrierungsvorgangs in den Variablenabschnitt des Notebooks ein (unten dargestellt). Sie können wahlweise den primären oder den sekundären Schlüssel verwenden. Achten Sie darauf, den Schlüssel in Anführungszeichen einzuschließen, um ihn zu einer Zeichenfolge zu machen.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)

---
title: 'Schnellstart: Verwenden von Python zum Aufrufen der Textanalyse-API'
titleSuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für die ersten Schritte mit der Textanalyse-API in Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: 69eb3789586233b824da1ef6a9c338b07281f324
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001387"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>Schnellstart: Verwenden von Python zum Aufrufen der Textanalyse von Cognitive Services 
<a name="HOLTop"></a>

Diese Anleitung veranschaulicht, wie Sie mithilfe der [Textanalyse-APIs](//go.microsoft.com/fwlink/?LinkID=759711) mit Python [Sprachen erkennen](#Detect), [Stimmungen analysieren](#SentimentAnalysis) und [Schlüsselbegriffe extrahieren](#KeyPhraseExtraction).

Sie können dieses Beispiel über die Befehlszeile oder als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf den Badge zum Starten von Binder klicken:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

### <a name="command-line"></a>Befehlszeile

Sie müssen den Kernel für Jupyter ([IPython](https://ipython.org/install.html)) ggf. aktualisieren:
```bash
pip install --upgrade IPython
```

Unter Umständen müssen Sie die [Anforderungsbibliothek](http://docs.python-requests.org/en/master/) aktualisieren:
```bash
pip install requests
```

Die technische Dokumentation für die APIs finden Sie in den [API-Definitionen](//go.microsoft.com/fwlink/?LinkID=759346).

## <a name="prerequisites"></a>Voraussetzungen

* [!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

* [Endpunkt und Zugriffsschlüssel](../How-tos/text-analytics-how-to-access-key.md), die bei der Registrierung für Sie generiert wurden

* Die folgenden Importe, der Abonnementschlüssel und `text_analytics_base_url` werden für alle nachfolgenden Schnellstartanleitungen verwendet. Fügen Sie die Importe hinzu.

    ```python
    import requests
    # pprint is pretty print (formats the JSON)
    from pprint import pprint
    from IPython.display import HTML
    ```
    
    Fügen Sie die folgenden Zeilen hinzu, und ersetzen Sie `subscription_key` durch einen gültigen Abonnementschlüssel, den Sie zuvor erhalten haben.
    
    ```python
    subscription_key = '<ADD KEY HERE>'
    assert subscription_key
    ```
    
    Fügen Sie dann die folgende Zeile hinzu, und überprüfen Sie, ob die Region in `text_analytics_base_url` der Region entspricht, für die Sie den Dienst eingerichtet haben. Wenn Sie einen Schlüssel für die kostenlose Testversion verwenden, müssen Sie keine Änderungen vornehmen.
    
    ```python
    text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/"
    ```

<a name="Detect"></a>

## <a name="detect-languages"></a>Sprachenerkennung

Die Sprachenerkennungs-API erfasst die Sprache eines Textdokuments mithilfe der [Sprachenerkennungsmethode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7). Der Dienstendpunkt der Sprachenerkennungs-API für Ihre Region ist unter der folgenden URL verfügbar:

```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/languages


Die Nutzlast an die API besteht aus einer Liste von `documents`, die jeweils eine `id` und ein `text`-Attribut enthalten. Das `text`-Attribut speichert den Text, der analysiert werden soll. 

Ersetzen Sie das `documents`-Wörterbuch durch einen beliebigen Text zur Sprachenerkennung.

```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

Die nächsten Codezeilen rufen die Sprachenerkennungs-API über die `requests`-Bibliothek in Python auf, um die Sprache der Dokumente zu ermitteln.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

Die folgenden Codezeilen rendern die JSON-Daten als eine HTML-Tabelle.

```python
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

Erfolgreiche JSON-Antwort:

```json
    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
                                           'name': 'English',
                                           'score': 1.0}],
                    'id': '1'},
                   {'detectedLanguages': [{'iso6391Name': 'es',
                                           'name': 'Spanish',
                                           'score': 1.0}],
                    'id': '2'},
                   {'detectedLanguages': [{'iso6391Name': 'zh_chs',
                                           'name': 'Chinese_Simplified',
                                           'score': 1.0}],
                    'id': '3'}],
     'errors': []}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Analysieren von Stimmungen

Die Standpunktanalyse-API erkennt die Stimmung (positiv oder negativ) in einer Gruppe von Textdatensätzen mithilfe der [Sentiment-Methode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9). Im folgenden Beispiel werden zwei Dokumente bewertet, ein englisches und ein spanisches.

Der Dienstendpunkt für die Standpunktanalyse ist für Ihre Region unter der folgenden URL verfügbar:

```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment

Wie im Sprachenerkennungsbeispiel wird der Dienst mit einem Wörterbuch mit einem `documents`-Schlüssel bereitgestellt, der aus einer Dokumentenliste besteht. Jedes Dokument ist ein Tupel aus der `id`, dem zu analysierenden `text` und der `language` des Texts. Sie können dieses Feld mithilfe der Sprachenerkennungs-API aus dem vorherigen Abschnitt auffüllen.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

Mit der Standpunktanalyse-API lässt sich nun die Stimmung von Dokumenten analysieren.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

Erfolgreiche JSON-Antwort:

```json
{'documents': [{'id': '1', 'score': 0.7673527002334595},
                {'id': '2', 'score': 0.18574094772338867},
                {'id': '3', 'score': 0.5}],
    'errors': []}
```

Die Stimmungspunktzahl eines Dokuments liegt zwischen 0,0 und 1,0, dabei gilt: Je höher der Wert, umso positiver die Stimmung.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Extrahieren von Schlüsselbegriffen

Die API zur Schlüsselbegriffserkennung extrahiert Schlüsselbegriffe aus einem Textdokument mithilfe der [Schlüsselbegriffsmethode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6). In diesem Abschnitt der Anleitung werden Schlüsselbegriffe für englische und spanische Dokumente extrahiert.

Der Dienstendpunkt für den Schlüsselbegriffserkennungs-Dienst ist unter der folgenden URL verfügbar:

```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases

Die Sammlung der Dokumente entspricht der Sammlung, die für die Stimmungsanalyse verwendet wurde.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

Das JSON-Objekt kann mit den folgenden Codezeilen als HTML-Tabelle gerendert werden:

```python
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

Die nächsten Codezeilen rufen die Sprachenerkennungs-API über die `requests`-Bibliothek in Python auf, um die Sprache der Dokumente zu ermitteln.
```python
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

Erfolgreiche JSON-Antwort:
```json
{'documents': [
    {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
    {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
    {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
    {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
    'errors': []
}
```

## <a name="identify-entities"></a>Identifizieren von Entitäten

Die Entitäts-API erkennt bekannte Entitäten in einem Textdokument mithilfe der [Entitätsmethode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634). Im folgenden Beispiel werden Entitäten für englische Dokumente erkannt.

Der Dienstendpunkt für den Entitätsverknüpfungsdienst ist unter der folgenden URL verfügbar:

```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/entities

Die Sammlung der Dokumente wird unten angezeigt:

```python
documents = {'documents' : [
  {'id': '1', 'text': 'Microsoft is an It company.'}
]}
```
Jetzt können die Dokumente an die Textanalyse-API gesendet werden, um die Antwort zu erhalten.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```

Erfolgreiche JSON-Antwort:
```json
{  
   "documents":[  
      {  
         "id":"1",
         "entities":[  
            {  
               "name":"Microsoft",
               "matches":[  
                  {  
                     "wikipediaScore":0.20872054383103444,
                     "entityTypeScore":0.99996185302734375,
                     "text":"Microsoft",
                     "offset":0,
                     "length":9
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Microsoft",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Microsoft",
               "bingId":"a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "type":"Organization"
            },
            {  
               "name":"Technology company",
               "matches":[  
                  {  
                     "wikipediaScore":0.82123868042800585,
                     "text":"It company",
                     "offset":16,
                     "length":10
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Technology company",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Technology_company",
               "bingId":"bc30426e-22ae-7a35-f24b-454722a47d8f"
            }
         ]
      }
   ],
    "errors":[]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)

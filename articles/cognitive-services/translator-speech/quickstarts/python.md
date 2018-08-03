---
title: Python-Schnellstart für Azure Cognitive Services, Microsoft-Sprachübersetzungs-API | Microsoft-Dokumentation
description: Hier finden Sie Informationen und Codebeispiele, mit denen Sie schnell erste Schritte mit der Microsoft-Sprachübersetzungs-API in Microsoft Cognitive Services in Azure ausführen können.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 07/17/2018
ms.author: v-jaswel
ms.openlocfilehash: 8bf904b2029790d64a806fcf4a7e4860579a5a2f
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39204849"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-python"></a>Schnellstart für die Microsoft-Sprachübersetzungs-API mit Python 
<a name="HOLTop"></a>

In diesem Artikel wird gezeigt, wie Sie mit der Microsoft-Sprachübersetzungs-API gesprochene Sprache in einer WAV-Datei übersetzen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Codes benötigen Sie [Python 3.x](https://www.python.org/downloads/).

Sie müssen das [Websocket-Clientpaket](https://pypi.python.org/pypi/websocket-client) für Python installieren.

Sie benötigen eine WAV-Datei namens „speak.wav“, die im gleichen Ordner gespeichert ist wie die ausführbare Datei, die Sie aus dem unten angegebenen Code kompilieren. Diese WAV-Datei muss im PCM-Standardformat mit 16 Bit, 16 kHz und in mono vorliegen. Eine solche WAV-Datei können Sie über die [Text-to-Speech-API](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech) abrufen.

Sie müssen über ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Microsoft-Sprachübersetzungs-API** verfügen. Sie benötigen einen kostenpflichtigen Abonnementschlüssel aus Ihrem [Azure-Dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Übersetzen von gesprochener Sprache

Mit dem folgenden Code wird gesprochene Sprache von einer Sprache in eine andere übersetzt.

1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Python-Projekt.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `key`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```python
# -*- coding: utf-8 -*-

# To install this package, run:
#   pip install websocket-client
import websocket

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'wss://dev.microsofttranslator.com'
path = '/speech/translate';
params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa'
uri = host + path + params

input_file = 'speak.wav'
output_file = 'speak2.wav'

output = bytearray ()

def on_open (client):
    print ("Connected.")

# r = read. b = binary.
    with open (input_file, mode='rb') as file:
        data = file.read()

    print ("Sending audio.")
    client.send (data, websocket.ABNF.OPCODE_BINARY)
# Make sure the audio file is followed by silence.
# This lets the service know that the audio input is finished.
    print ("Sending silence.")
    client.send (bytearray (32000), websocket.ABNF.OPCODE_BINARY)

def on_data (client, message, message_type, is_last):
    global output
    if (websocket.ABNF.OPCODE_TEXT == message_type):
        print ("Received text data.")
        print (message)
# For some reason, we receive the data as type websocket.ABNF.OPCODE_CONT.
    elif (websocket.ABNF.OPCODE_BINARY == message_type or websocket.ABNF.OPCODE_CONT == message_type):
        print ("Received binary data.")
        print ("Is last? " + str(is_last))
        output = output + message
        if (True == is_last):
# w = write. b = binary.
            with open (output_file, mode='wb') as file:
                file.write (output)
                print ("Wrote data to output file.")
            client.close ()
    else:
        print ("Received data of type: " + str (message_type))

def on_error (client, error):
    print ("Connection error: " + str (error))

def on_close (client):
    print ("Connection closed.")

client = websocket.WebSocketApp(
    uri,
    header=[
        'Ocp-Apim-Subscription-Key: ' + key
    ],
    on_open=on_open,
    on_data=on_data,
    on_error=on_error,
    on_close=on_close
)

print ("Connecting...")
client.run_forever()
```

**Ergebnis der Sprachübersetzung**

Bei erfolgreicher Ausführung wird eine Datei namens „speak2.wav“ erstellt. Die Datei enthält die Übersetzung der in „speak.wav“ gesprochenen Sprache.

[Nach oben](#HOLTop)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial zur Sprachübersetzung](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Weitere Informationen 

[Übersicht über die Sprachübersetzungs-API](../overview.md)
[API-Referenz](http://docs.microsofttranslator.com/speech-translate.html)

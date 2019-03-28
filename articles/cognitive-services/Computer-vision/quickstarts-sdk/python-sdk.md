---
title: 'Schnellstart: Python SDK'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie das Python SDK für allgemeine Aufgaben verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: pafarley
ms.openlocfilehash: ffecc07c49db8fd1b27cc2dd82192aa31a7fbd19
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194978"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>Azure Cognitive Services: SDK für maschinelles Sehen für Python

Über den Dienst für maschinelles Sehen haben Entwickler Zugriff auf erweiterte Algorithmen für die Bildverarbeitung und die Rückgabe von Informationen. Algorithmen für maschinelles Sehen analysieren den Inhalt eines Bilds auf unterschiedliche Weise – je nachdem, für welche visuellen Merkmale Sie sich interessieren. 

* [Analysieren eines Bilds](#analyze-an-image)
* [Abrufen der Motivdomänenliste](#get-subject-domain-list)
* [Analysieren eines Bilds nach Domäne](#analyze-an-image-by-domain)
* [Generieren einer Bildbeschreibung in Textform](#get-text-description-of-an-image)
* [Extrahieren von handschriftlichem Text aus einem Bild](#get-text-from-image)
* [Generieren einer Miniaturansicht](#generate-thumbnail)

Weitere Informationen zu diesem Dienst finden Sie unter [Worum handelt es sich bei maschinellem Sehen?][computervision_docs].

Weitere Dokumentationen:

* [SDK-Referenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Dokumentation für Maschinelles Sehen-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.6+][python]
* Kostenloser [Schlüssel für maschinelles Sehen][computervision_resource] und die zugeordnete Region. Diese Werte werden beim Erstellen der Instanz des Clientobjekts [ComputerVisionAPI][ref_computervisionclient] benötigt. Die Werte können mit einer der folgenden Methoden ermittelt werden. 

### <a name="if-you-dont-have-an-azure-subscription"></a>Vorgehensweise ohne Azure-Abonnement

Erstellen Sie über **[Jetzt testen!][computervision_resource]** einen kostenlosen Schlüssel für den Dienst für maschinelles Sehen. Dieser ist sieben Tage lang gültig. Wenn der Schlüssel erstellt wurde, kopieren Sie den Schlüssel und den Namen der Region. Diese Angaben sind zum [Erstellen des Clients](#create-client) erforderlich.

Speichern Sie nach der Schlüsselerstellung Folgendes:

* Schlüsselwert: Eine Zeichenfolge mit 32 Zeichen im Format `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`. 
* Schlüsselregion: Die Unterdomäne der Endpunkt-URL (https://**westcentralus**.api.cognitive.microsoft.com).

### <a name="if-you-have-an-azure-subscription"></a>Vorgehensweise mit Azure-Abonnement

Am einfachsten können Sie eine Ressource in Ihrem Abonnement mithilfe des folgenden Befehls über die [Azure-Befehlszeilenschnittstelle][azure_cli] erstellen. Dadurch wird ein Cognitive Services-Schlüssel erstellt, der für viele Cognitive Services verwendet werden kann. Wählen Sie den Namen der _bereits vorhandenen_ Ressourcengruppe (beispielsweise „my-cogserv-group“) und den Namen der neuen Ressource für maschinelles Sehen (beispielsweise „my-computer-vision-resource“) aus. 

```Bash
RES_REGION=westeurope 
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind CognitiveServices \
    --sku S0 \
    --yes
```

<!--
## Installation

Install the Azure Cognitive Services Computer Vision SDK with [pip][pip], optionally within a [virtual environment][venv].

### Configure a virtual environment (optional)

Although not required, you can keep your base system and Azure SDK environments isolated from one another if you use a [virtual environment][virtualenv]. Execute the following commands to configure and then enter a virtual environment with [venv][venv], such as `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```
-->

### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie das [Azure Cognitive Services SDK für maschinelles Sehen für Python][pypi_computervision] mit [pip][pip]:

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Authentication

Nachdem Sie die Ressource für maschinelles Sehen erstellt haben, benötigen Sie ihre **Region** und einen ihrer **Kontoschlüssel**, um das Clientobjekt zu instanziieren.

Verwenden Sie diese Werte, wenn Sie die Instanz des Clientobjekts [ComputerVisionAPI][ref_computervisionclient] erstellen. 

Verwenden Sie beispielsweise das Bash-Terminal, um die Umgebungsvariablen festzulegen:

```Bash
ACCOUNT_REGION=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-region"></a>Abrufen von Anmeldeinformationen für Schlüssel und Region für Azure-Abonnementbenutzer

Sollten Sie Ihre Region und Ihren Schlüssel vergessen haben, können Sie sie wie folgt ermitteln. Falls Sie einen Schlüssel und eine Region erstellen müssen, können Sie die Methode für [Besitzer eines Azure-Abonnements](#if-you-have-an-azure-subscription) oder für [Benutzer ohne Azure-Abonnement](#if-you-dont-have-an-azure-subscription) verwenden.

Verwenden Sie den folgenden Codeausschnitt für die [Azure-Befehlszeilenschnittstelle][cloud_shell], um zwei Umgebungsvariablen mit der **Region** und einem der **Schlüssel** des Kontos für maschinelles Sehen aufzufüllen. (Diese Werte stehen auch im [Azure-Portal][azure_portal] zur Verfügung.) Der Ausschnitt ist für die Bash-Shell formatiert.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_REGION=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query location \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```


### <a name="create-client"></a>Erstellen des Clients

Rufen Sie die Region und den Schlüssel aus Umgebungsvariablen ab, und erstellen Sie anschließend das Clientobjekt [ComputerVisionAPI][ref_computervisionclient].  

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionAPI
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get region and key from environment variables
import os
region = os.environ['ACCOUNT_REGION']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionAPI(region, credentials)
```

## <a name="examples"></a>Beispiele

Für die folgenden Aufgaben wird ein Clientobjekt vom Typ [ComputerVisionAPI][ref_computervisionclient] vorausgesetzt.

### <a name="analyze-an-image"></a>Analysieren von Bildern

Mit [`analyze_image`][ref_computervisionclient_analyze_image] können verschiedene Aspekte eines Bilds analysiert werden. Verwenden Sie die Eigenschaft [`visual_features`][ref_computervision_model_visualfeatures], um festzulegen, welche Analysen für das Bild ausgeführt werden sollen. Gängige Werte sind `VisualFeatureTypes.tags` und `VisualFeatureTypes.description`.

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>Abrufen der Motivdomänenliste

Verwenden Sie [`list_models`][ref_computervisionclient_list_models], um die für die Bildanalyse verwendeten Motivdomänen zu überprüfen. Diese Domänennamen werden bei der [domänenbasierten Bildanalyse](#analyze-an-image-by-domain) verwendet. Ein Beispiel für eine Domäne wäre etwa `landmarks`.

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>Analysieren eines Bilds nach Domäne

Mit [`analyze_image_by_domain`][ref_computervisionclient_analyze_image_by_domain] können Sie eine Bildanalyse auf der Grundlage der Motivdomäne durchführen. Rufen Sie die [Liste unterstützter Motivdomänen](#get-subject-domain-list) ab, um den passenden Domänennamen zu verwenden.  

```Python
# type of prediction
domain = "landmarks"

# Public domain image of Eiffel tower
url = "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg"

# English language response
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>Generieren einer Bildbeschreibung in Textform

Mit [`describe_image`][ref_computervisionclient_describe_image] können Sie eine sprachbasierte Textbeschreibung eines Bilds generieren. Mit der Eigenschaft `max_description` können Sie mehrere Beschreibungen anfordern, wenn Sie eine Textanalyse für Schlüsselwörter im Zusammenhang mit dem Bild durchführen. Textbeschreibungsbeispiele für das folgende Bild wären etwa `a train crossing a bridge over a body of water`, `a large bridge over a body of water` und `a train crossing a bridge over a large body of water`.

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>Extrahieren von Text aus einem Bild

Sie können handschriftlichen oder gedruckten Text aus einem Bild extrahieren. Dazu sind zwei SDK-Aufrufe erforderlich: [`recognize_text`][ref_computervisionclient_recognize_text] und [`get_text_operation_result`][ref_computervisionclient_get_text_operation_result]. Der Aufruf von „recognize_text“ ist asynchron. In den Ergebnissen des Aufrufs von „get_text_operation_result“ müssen Sie anhand von [`TextOperationStatusCodes`][ref_computervision_model_textoperationstatuscodes] überprüfen, ob der erste Aufruf abgeschlossen wurde, bevor Sie die Textdaten extrahieren. Die Ergebnisse enthalten den Text sowie die Koordinaten des umgebenden Rechtecks für den Text. 

```Python
# import models
from azure.cognitiveservices.vision.computervision.models import TextRecognitionMode
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
mode = TextRecognitionMode.handwritten
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.recognize_text(url, mode, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while result.status in ['NotStarted', 'Running']:
    time.sleep(1)
    result = client.get_text_operation_result(operationId)

# Get data
if result.status == TextOperationStatusCodes.succeeded:

    for line in result.recognition_result.lines:
        print(line.text)
        print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Generieren einer Miniaturansicht

Mit [`generate_thumbnail`][ref_computervisionclient_generate_thumbnail] können Sie eine Miniaturansicht (JPG) eines Bilds generieren. Die Proportionen der Miniaturansicht können von den Proportionen des ursprünglichen Bilds abweichen. 

Installieren Sie **Pillow**, um dieses Beispiel zu verwenden:

```bash
pip install Pillow
``` 

Verwenden Sie nach der Installation von Pillow das Paket im folgenden Codebeispiel, um das Miniaturbild zu generieren:

```Python
# Pillow package
from PIL import Image

# IO package to create local image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>Problembehandlung

### <a name="general"></a>Allgemein

Wenn Sie bei Verwendung des Python SDK mit dem Clientobjekt [ComputerVisionAPI][ref_computervisionclient] interagieren, wird für die Rückgabe von Fehlern die Klasse [`ComputerVisionErrorException`][ref_computervision_computervisionerrorexception] verwendet. Die von dem Dienst zurückgegebenen Fehler entsprechen den HTTP-Statuscodes, die für REST-API-Anforderungen zurückgegeben werden.

Wenn Sie also beispielsweise versuchen, ein Bild mit einem ungültigen Schlüssel zu analysieren, wird ein Fehler vom Typ `401` zurückgegeben. Im folgenden Codeausschnitt wird der [Fehler][ref_httpfailure] ordnungsgemäß behandelt, indem die Ausnahme abgefangen wird und zusätzliche Fehlerinformationen angezeigt werden.

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and region are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Behandeln von vorübergehenden Fehlern mit Wiederholungen

Bei der Verwendung des [ComputerVisionAPI][ref_computervisionclient]-Clients kann es zu vorübergehenden Fehlern kommen. Diese können auf durch den Dienst erzwungene [Ratenlimits][computervision_request_units] oder auf andere vorübergehende Probleme (etwa auf Netzwerkausfälle) zurückzuführen sein. Informationen zur Behandlung solcher Fehler finden Sie im Leitfaden für Cloudentwurfsmuster unter [Wiederholungsmuster][azure_pattern_retry] sowie unter dem dazugehörigen [Trennschalter-Muster][azure_pattern_circuit_breaker].

### <a name="more-sample-code"></a>Weiterer Beispielcode

Im GitHub-Repository des SDK stehen zahlreiche Beispiele für das Computer Vision Python SDK zur Verfügung. Diese Beispiele enthalten Beispielcode für weitere gängige Szenarien im Zusammenhang mit maschinellem Sehen:

* [recognize_text][recognize-text]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Anwenden von Inhaltstags auf Bilder](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/en-us/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
[azure_pattern_circuit_breaker]: https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker
[azure_pattern_retry]: https://docs.microsoft.com/azure/architecture/patterns/retry
[azure_portal]: https://portal.azure.com
[azure_sub]: https://azure.microsoft.com/free/

[cloud_shell]: https://docs.microsoft.com/azure/cloud-shell/overview

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://azure.microsoft.com/en-us/try/cognitive-services/?

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python


[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#analyze-image-url--visual-features-none--details-none--language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#list-models-custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#analyze-image-by-domain-model--url--language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#describe-image-url--max-candidates--1---language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_recognize_text]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#recognize-text-url--mode--custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#get-text-operation-result-operation-id--custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#generate-thumbnail-width--height--url--smart-cropping-false--custom-headers-none--raw-false--callback-none----operation-config-


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/

[recognize-text]:https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/vision/computer_vision_samples.py


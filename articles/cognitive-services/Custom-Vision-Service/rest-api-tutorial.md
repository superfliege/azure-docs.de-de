---
title: 'Tutorial: Erstellen, Trainieren und Exportieren eines Modells mit der Custom Vision-REST-API'
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die REST-API zum Erstellen, Trainieren, Testen und Exportieren eines benutzerdefinierten Modells für maschinelles Sehen.
services: cognitive-services
author: blackmist
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: larryfr
ms.openlocfilehash: 54b5f7bb16803adf91a0a8ea60cfa68d1e322d07
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351098"
---
# <a name="tutorial-create-train-and-export-a-model-with-rest"></a>Tutorial: Erstellen, Trainieren und Exportieren eines Modells mit REST

Die Informationen in diesem Dokument veranschaulichen, wie Sie einen REST-Client für die Arbeit mit der REST-API für das Trainieren des Custom Vision-Diensts verwenden. Die Beispiele zeigen, wie Sie die API mithilfe des Hilfsprogramms `curl` aus einer Bash-Umgebung und mithilfe des Hilfsprogramms `Invoke-WebRequest` in Windows PowerShell verwenden.

> [!div class="checklist"]
> * Abrufen der Schlüssel
> * Erstellen eines Projekts
> * Erstellen von Tags
> * Hinzufügen von Bildern
> * Trainieren und Testen des Modells
> * Exportieren des Modells

## <a name="prerequisites"></a>Voraussetzungen

* Grundlegende Kenntnisse von REST (Representational State Transfer). In diesem Dokument wird nicht auf Details wie HTTP-Verben, JSON oder andere Dinge, die häufig mit REST verwendet werden, eingegangen.
* Entweder eine Bash (Bourne Again Shell) mit dem Hilfsprogramm [cURL](https://curl.haxx.se) oder Windows PowerShell 3.0 (oder höher)
* Ein Custom Vision-Konto. Weitere Informationen finden Sie im Dokument [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md).

## <a name="get-keys"></a>Abrufen der Schlüssel

Für die Verwendung der REST-API ist eine Authentifizierung mithilfe eines Schlüssels erforderlich. Verwenden Sie für Verwaltungs- oder Trainingsvorgänge den __Trainingsschlüssel__. Wenn Sie das Modell für Vorhersagen verwenden möchten, verwenden Sie den __Vorhersageschlüssel__.

Bei einer Anforderung wird der Schlüssel als Anforderungsheader gesendet.

Besuchen Sie die [Custom Vision-Webseite](https://customvision.ai), und wählen Sie in der rechten oberen Ecke das __Zahnradsymbol__ aus, um die Schlüssel für Ihr Konto abzurufen. Kopieren Sie aus dem Abschnitt __Accounts__ (Konten) die Werte der Felder __Training Key__ (Trainingsschlüssel) und __Prediction Key__ (Vorhersageschlüssel).

![Abbildung der Schlüsselbenutzeroberfläche](./media/rest-api-tutorial/training-prediction-keys.png)

> [!IMPORTANT]
> Da die Schlüssel für das Authentifizieren sämtlicher Anforderungen verwendet werden, wird in den Beispielen in diesem Dokument davon ausgegangen, dass die Schlüsselwerte in Umgebungsvariablen enthalten sind. Verwenden Sie die folgenden Befehle, um die Schlüssel in Umgebungsvariablen zu speichern, bevor Sie andere Codeausschnitte in diesem Dokument verwenden:
>
> ```bash
> read -p "Enter the training key: " TRAININGKEY
> read -p "Enter the prediction key: " PREDICTIONKEY
> ```
>
> ```powershell
> $trainingKey = Read-Host 'Enter the training key'
> $predictionKey = Read-Host 'Enter the prediction key'
> ```

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

In den folgenden Beispielen erstellen Sie das neue Projekt `myproject` in Ihrer Custom Vision-Dienstinstanz. Dieser Dienst verwendet standardmäßig die Domäne `General`:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
{
  "id":"45d1b19b-69b6-4a22-8e7e-d1ca37504686",
  "name":"myproject",
  "description":"",
  "settings":{
    "domainId":"ee85a72c-405e-4adc-bb47-ffa8ca0c9f31",
    "useNegativeSet":true,
    "classificationType":"Multilabel",
    "detectionParameters":null
  },
  "created":"2018-08-10T17:39:02.5633333",
  "lastModified":"2018-08-10T17:39:02.5633333",
  "thumbnailUri":null
}
```

> [!TIP]
> Der Eintrag `id` in der Antwort ist die ID des neuen Projekts. Diese wird in anderen Beispielen weiter unten in diesem Dokument verwendet.

Weitere Informationen zu dieser Anforderung finden Sie unter [CreateProject](https://go.microsoft.com/fwlink/?linkid=865446).

### <a name="specific-domains"></a>Spezifische Domänen

Um ein Projekt für eine bestimmte Domäne zu erstellen, können Sie die __Domänen-ID__ als optionalen Parameter angeben. Die folgenden Beispiele zeigen, wie Sie eine Liste der verfügbaren Domänen abrufen:

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
[  
  {  
    "id":"ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
    "name":"General",
    "type":"Classification",
    "exportable":false,
    "enabled":true
  },
  {  
    "id":"c151d5b5-dd07-472a-acc8-15d29dea8518",
    "name":"Food",
    "type":"Classification",
    "exportable":false,
    "enabled":true
  },
  {  
    "id":"ca455789-012d-4b50-9fec-5bb63841c793",
    "name":"Landmarks",
    "type":"Classification",
    "exportable":false,
    "enabled":true
  },
  ...
]
```

Weitere Informationen zu dieser Anforderung finden Sie unter [GetDomains](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a827d).

Das folgende Beispiel veranschaulicht das Erstellen eines neuen Projekts, dass die Domäne __Landmarks__ verwendet:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

## <a name="create-tags"></a>Erstellen von Tags

Zum Kennzeichnen von Bildern müssen Sie eine Tag-ID verwenden. Im folgende Beispiel wird veranschaulicht, wie Sie ein neues Tag mit dem Namen `cat` erstellen und eine Tag-ID abrufen. Ersetzen Sie `{projectId}` durch die ID Ihres Projekts. Verwenden Sie den Parameter `name=` zum Angeben des Tagnamens:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus: 

```json
{"id":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43","name":"cat","description":null,"imageCount":0}
```

Speichern Sie den Wert von `id`, da er für das Kennzeichnen von Bildern verwendet wird.

Weitere Informationen zu dieser Anforderung finden Sie unter [CreateTag](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829d).

## <a name="add-images"></a>Hinzufügen von Bildern

Die folgenden Beispiele veranschaulichen das Hinzufügen einer Datei aus der URL. Ersetzen Sie `{projectId}` durch die ID Ihres Projekts. Ersetzen Sie `{tagId}` durch der ID des Tags für das Bild:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
{  
  "isBatchSuccessful":true,
  "images":[  
    {  
      "sourceUrl":"http://myimages/cat.jpg",
      "status":"OK",
      "image":{  
        "id":"081adaee-a76b-4d94-a70e-e4fd0935a28f",
        "created":"2018-08-13T13:24:22.0815638",
        "width":640,
        "height":480,
        "imageUri":"https://linktoimage",
        "thumbnailUri":"https://linktothumbnail",
        "tags":[  
          {  
            "tagId":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "tagName":null,
            "created":"2018-08-13T13:24:22.104936"
          }
        ],
        "regions":[  
          {  
            "regionId":"40f206a1-3f8a-4de7-a6c3-c7b4643117df",
            "tagName":null,
            "created":"2018-08-13T13:24:22.104936",
            "tagId":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "left":119,
            "top":94,
            "width":240,
            "height":140
          }
        ]
      }
    }
  ]
}
```

Weitere Informationen zu dieser Anforderung finden Sie unter [CreateImagesFromUrls](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8287).

## <a name="train-the-model"></a>Modelltraining

Die folgenden Beispiele veranschaulichen das Trainieren des Modells. Ersetzen Sie `{projectId}` durch die ID Ihres Projekts:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } 
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
{
    "id": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "name": "Iteration 1",
    "isDefault": false,
    "status": "Training",
    "created": "2018-08-10T17:39:02.5766667",
    "lastModified": "2018-08-16T17:15:07.5250661",
    "projectId": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "exportable": false,
    "domainId": null
}
```

Speichern Sie den Wert von `id`, da er zum Testen und Exportieren des Modells verwendet wird.

Weitere Informationen finden Sie unter [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8294).

## <a name="test-the-model"></a>Testen des Modells

Die folgenden Beispiele veranschaulichen das Testen des Modells. Ersetzen Sie `{projectId}` durch die ID Ihres Projekts. Ersetzen Sie `{iterationId}` durch die ID, die beim Trainieren des Modells zurückgegeben wurde. Ersetzen Sie `https://linktotestimage` durch den Pfad zum Testbild.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"url":"https://linktotestimage"}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"url":"https://linktotestimage"}'
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
{  
  "id":"369b010b-2a92-4f48-a918-4c1a0af91888",
  "project":"45d1b19b-69b8-4b22-8e7e-d1ca37504686",
  "iteration":"23de09d6-42a1-413e-839e-8db6ee6d3496",
  "created":"2018-08-16T17:39:20.7944508Z",
  "predictions":[  
    {  
      "probability":0.8390652,
      "tagId":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
      "tagName":"cat"
    }
  ]
}
```

Weitere Informationen finden Sie unter [QuickTestImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a828d).

## <a name="export-the-model"></a>Exportieren des Modells

Das Exportieren eines Modell umfasst zwei Schritte. Zuerst müssen Sie das Modellformat angeben, und dann fordern Sie die URL für das exportierte Modell an.

### <a name="request-a-model-export"></a>Anfordern eines Modellexports

Die folgenden Beispiele veranschaulichen das Exportieren eines `coreml`-Modells. Ersetzen Sie `{projectId}` durch die ID Ihres Projekts. Ersetzen Sie `{iterationId}` durch die ID, die beim Trainieren des Modells zurückgegeben wurde.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
{  
  "platform":"CoreML",
  "status":"Exporting",
  "downloadUri":null,
  "flavor":null
}
```

Weitere Informationen finden Sie unter [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829b).

### <a name="download-the-exported-model"></a>Herunterladen des exportierten Modells

Die folgenden Beispiele veranschaulichen das Abrufen der URL des exportierten Modells. Ersetzen Sie `{projectId}` durch die ID Ihres Projekts. Ersetzen Sie `{iterationId}` durch die ID, die beim Trainieren des Modells zurückgegeben wurde.

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

Die Antwort auf die Anforderung sieht in etwa wie das folgende JSON-Dokument aus:

```json
[  
  {  
    "platform":"CoreML",
    "status":"Done",
    "downloadUri":"https://linktoexportedmodel",
    "flavor":null
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [GetExports](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829a).

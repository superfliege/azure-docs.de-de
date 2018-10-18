---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, cURL – Maschinelles Sehen-API'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die Maschinelles Sehen-API mit cURL verwenden.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.openlocfilehash: 51c6a8e5693602cdc839de80f268891c247c63a9
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344063"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-curl-in-computer-vision"></a>Schnellstart: Generieren einer Miniaturansicht mit der REST-API und cURL in der Maschinelles Sehen-API

In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die REST-API von Maschinelles Sehen verwenden. Mit der [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)-Methode können Sie eine Miniaturansicht von einem Bild generieren. Sie geben die Höhe und Breite an. Diese kann sich vom Seitenverhältnis des Eingabebilds unterscheiden. Für das maschinelle Sehen wird die intelligente Zuschneidefunktion verwendet, um die gewünschte Region zu identifizieren und basierend auf dieser Region Zuschneidekoordinaten zu generieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="get-thumbnail-request"></a>Get Thumbnail-Anforderung

Mit der [Get Thumbnail-Methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) können Sie aus einem Bild eine Miniaturansicht generieren. Sie geben die Höhe und Breite an. Diese kann sich vom Seitenverhältnis des Eingabebilds unterscheiden. Für das maschinelle Sehen wird die intelligente Zuschneidefunktion verwendet, um die gewünschte Region zu identifizieren und basierend auf dieser Region Zuschneidekoordinaten zu generieren.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ersetzen Sie `<File>` durch den Pfad und Dateinamen für die Speicherung der Miniaturansicht.
1. Ändern Sie die Anforderungs-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ändern Sie optional das Bild (`{\"url\":\"...`), das Sie analysieren möchten.
1. Öffnen Sie auf einem Computer, auf dem cURL installiert ist, ein Befehlsfenster.
1. Fügen Sie den Code in das Fenster ein, und führen Sie den Befehl aus.

>[!NOTE]
>Sie müssen in Ihrem REST-Aufruf den gleichen Standort verwenden, den Sie zum Erwerb Ihrer Abonnementschlüssel verwendet haben. Wenn Sie Ihre Abonnementschlüssel beispielsweise von „westus“ erhalten haben, ersetzen Sie „westcentralus“ in der URL unten durch „westus“.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen [cURL](https://curl.haxx.se/windows).
- Sie benötigen einen Abonnementschlüssel für Maschinelles Sehen. Informationen zum Beziehen eines Abonnementschlüssels finden Sie unter [Gewusst wie: Beziehen von Abonnementschlüsseln](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-command"></a>Erstellen und Ausführen des Beispielbefehls

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in einen Text-Editor ein.
1. Nehmen Sie die folgenden Änderungen im Befehl vor, falls dies erforderlich ist:
    1. Ersetzen Sie den `<subscriptionKey>`-Wert durch Ihren Abonnementschlüssel.
    1. Ersetzen Sie den Wert von `<thumbnailFile>` durch den Pfad und den Namen der Datei, in der die Miniaturansicht gespeichert werden soll.
    1. Ersetzen Sie die Anforderungs-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail`) durch die Endpunkt-URL für die Methode [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) in der Azure-Region, in der Sie Ihre Abonnementschlüssel bezogen haben, falls erforderlich.
    1. Ändern Sie optional die Bild-URL im Anforderungstext (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) in die URL eines anderen Bilds, von dem eine Miniaturansicht generiert werden soll.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Fügen Sie den Befehl aus dem Text-Editor in das Eingabeaufforderungsfenster ein, und führen Sie den Befehl aus.

```console
curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Bei einer erfolgreichen Antwort wird das Miniaturbild in die unter `<thumbnailFile>` angegebene Datei geschrieben. Wenn die Anforderung nicht erfolgreich ist, enthält die Antwort einen Fehlercode und eine Meldung, damit Sie ermitteln können, wo der Fehler liegt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Eingabeaufforderungsfenster und den Text-Editor nicht mehr benötigen, schließen Sie sie.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-API, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet wird. Wenn Sie schnell mit Ihren Experimenten mit der Maschinelles Sehen-API beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Erkunden der Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

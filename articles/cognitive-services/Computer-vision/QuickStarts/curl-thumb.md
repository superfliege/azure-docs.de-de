---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, cURL – Maschinelles Sehen-API'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung generieren Sie aus einem Bild eine Miniaturansicht, indem Sie die Maschinelles Sehen-API mit cURL in Cognitive Services verwenden.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: ecb973c35a05ca8c246bb10d6fda892b07ab805e
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842514"
---
# <a name="quickstart-generate-a-thumbnail---rest-curl---computer-vision"></a>Schnellstart: Generieren einer Miniaturansicht – REST, cURL – Maschinelles Sehen-API

In dieser Schnellstartanleitung generieren Sie aus einem Bild eine Miniaturansicht, indem Sie die Maschinelles Sehen-API verwenden.

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

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -o <File> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
```

## <a name="get-thumbnail-response"></a>Get Thumbnail-Antwort

Bei einer erfolgreichen Antwort wird die Miniaturansicht in `<File>` geschrieben. Wenn die Anforderung nicht erfolgreich ist, enthält die Antwort einen Fehlercode und eine Meldung, damit Sie ermitteln können, wo der Fehler liegt.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden. Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

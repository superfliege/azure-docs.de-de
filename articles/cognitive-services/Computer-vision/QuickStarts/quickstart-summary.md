---
title: Schnellstart zur Maschinelles Sehen-API – Zusammenfassung
titleSuffix: Azure Cognitive Services
description: In diesen Schnellstartanleitungen analysieren Sie ein Bild, erstellen eine Miniaturansicht und extrahieren gedruckten und handschriftlichen Text, indem Sie die Maschinelles Sehen-API verwenden.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 89502fe49f5eb4b9692d7ee4233c7c46d645b3fb
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576883"
---
# <a name="quickstart-summary"></a>Schnellstart: Zusammenfassung

Diese Schnellstartanleitungen enthalten Informationen und Codebeispiele, um Ihnen den schnellen Einstieg in den Maschinelles Sehen-Dienst zu erleichtern.

In den Beispielen werden direkte HTTP-Aufrufe für die Maschinelles Sehen-API durchgeführt. Der Abschnitt *SDK-Schnellstartanleitungen* enthält Beispiele mit den Clientbibliotheken für maschinelles Sehen, in denen komfortable Methoden zum Umschließen der HTTP-Aufrufe bereitgestellt werden.

Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

Sie können den Dienst für maschinelles Sehen verwenden, um die folgenden Aufgaben durchzuführen:

* Analysieren von Remotebildern
* Analysieren von lokalen Bildern
* Erkennen von Prominenten und Sehenswürdigkeiten (Domänenmodelle)
* Intelligentes Generieren einer Miniaturansicht
* Erkennen und Extrahieren von gedrucktem Text (OCR) aus einem Bild
* Erkennen und Extrahieren von handschriftlichem Text aus einem Bild

Der Code in den einzelnen Beispielen ähnelt sich jeweils. Es geht darin aber jeweils um unterschiedliche Features des maschinellen Sehens und verschiedene Verfahren zum Austauschen von Daten mit dem Dienst, z.B.:

* Beim _Generieren von Miniaturansichten_ wird im Text der Antwort ein Bild als Bytearray zurückgegeben.
* Für das _Analysieren von lokalen Bildern_ muss das Bild als Bytearray in die Anforderung eingefügt werden.
* Für das _Extrahieren von handschriftlichem Text_ sind zwei Aufrufe erforderlich, um den Text abzurufen.

## <a name="summary"></a>Zusammenfassung

| Schnellstart               | Anforderungsparameter                          | response          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Analysieren von Remotebildern   | visualFeatures=Categories,Description,Color | JSON-Zeichenfolge       |
| Analysieren von lokalen Bildern    | data=image_data (Bytearray)                | JSON-Zeichenfolge       |
| Erkennen von Prominenten       | model=celebrities                           | JSON-Zeichenfolge       |
| Generieren von Miniaturansichten     | width=200&height=150&smartCropping=true     | Bytearray        |
| Extrahieren von gedrucktem Text     | language=unk&detectOrientation=true         | JSON-Zeichenfolge       |
| Extrahieren von handschriftlichem Text | handwriting=true                            | URL, JSON-Zeichenfolge  |

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

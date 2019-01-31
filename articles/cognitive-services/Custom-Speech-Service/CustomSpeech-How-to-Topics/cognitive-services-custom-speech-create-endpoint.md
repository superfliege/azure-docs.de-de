---
title: Erstellen eines benutzerdefinierten Spracherkennungsendpunkts mit Custom Speech Service in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen benutzerdefinierten Endpunkt für die Spracherkennung mit Custom Speech Service in Cognitive Services erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: f9aca8529bf0407022ff3683c1cd73b87045f2f2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216664"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Erstellen eines benutzerdefinierten Spracherkennungsendpunkts

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Nachdem Sie benutzerdefinierte Akustik- oder Sprachmodelle erstellt haben, können Sie diese einem benutzerdefinierten Endpunkt für die Spracherkennung bereitstellen. 

## <a name="create-an-endpoint"></a>Erstellen eines Endpunkts
Wählen Sie zum Erstellen eines neuen benutzerdefinierten Endpunkts aus dem Menü **Custom Speech** am oberen Rand der Seite die Option **Bereitstellungen** aus. Durch diese Aktion gelangen Sie zu der Seite **Bereitstellungen**, die eine Tabelle mit aktuellen benutzerdefinierten Endpunkten enthält. Wenn Sie noch keine Endpunkte erstellt haben, ist die Tabelle leer. Das aktuelle Gebietsschema ist im Tabellentitel angegeben. 

Wenn Sie eine Bereitstellung für eine andere Sprache erstellen möchten, müssen Sie auf **Gebietsschema ändern** klicken. Weitere Informationen zu unterstützten Sprachen finden Sie unter [Unterstützte Gebietsschemas in Custom Speech Service](cognitive-services-custom-speech-change-locale.md).

Klicken Sie auf **Neuen erstellen**, um einen neuen Endpunkt zu erstellen. Geben Sie im Bereich **Bereitstellung erstellen** die jeweiligen Informationen in die Felder **Name** und **Beschreibung** Ihrer benutzerdefinierten Bereitstellung ein.

Wählen Sie im Kombinationsfeld **Abonnement** das gewünschte Abonnement aus. Wenn es sich um ein S2-Abonnement handelt, können Sie die Optionen für Skalierungseinheiten und die Inhaltsprotokollierung auswählen. Weitere Informationen über Skalierungseinheiten und die Protokollierung finden Sie unter [Verbrauchseinheiten und Kontingente für Custom Speech Service](../cognitive-services-custom-speech-meters.md).

Die folgende Tabelle enthält die Zuordnung der Skalierungseinheiten zu den verfügbaren gleichzeitigen Anforderungen:

| Skalierungseinheit | Anzahl der gleichzeitigen Anforderungen |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| n | 5 * n |

Außerdem können Sie auswählen, ob die Inhaltsprotokollierung ein- oder ausgeschaltet sein soll. Sie wählen also aus, ob der Endpunktdatenverkehr für die interne Nutzung durch Microsoft gespeichert wird. Wenn es nicht aktiviert wird, wird die Speicherung des Datenverkehrs unterdrückt. Für die Unterdrückung von Inhaltsprotokollierungsergebnissen fallen zusätzliche Kosten an. Details finden Sie auf der Seite mit [Informationen zu Preisen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/).

> [!NOTE]
> Die Inhaltsprotokollierung wird auf der Seite mit Preisdetails als „Ohne Ablaufverfolgung“ bezeichnet.
>


Darüber hinaus bietet Microsoft eine grobe Schätzung der Kosten, sodass Sie erfahren, inwiefern sich dies auf die Kosten von Skalierungseinheiten und der Inhaltsprotokollierung auswirkt. Diese Schätzung stellt eine grobe Schätzung dar und weicht möglicherweise von Ihren tatsächlichen Kosten ab.

> [!NOTE]
> Diese Einstellungen sind nicht bei F0-Abonnements (Free-Tarif) verfügbar.
>

Wählen Sie in der Liste **Akustikmodell** das gewünschte Akustikmodell und in der Liste **Sprachmodell** das gewünschte Sprachmodell aus. Die Optionen für Akustik- und Sprachmodelle umfassen immer die Microsoft-Basismodelle. Die Auswahl des Basismodells schränkt die Kombinationen ein. Konventionelle Basismodelle können nicht mit Such- und Diktatbasismodellen kombiniert werden.

![Die Seite „Bereitstellung erstellen“](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Achten Sie darauf, dass Sie die Nutzungsbedingungen und Preisangaben durch Aktivieren des entsprechenden Kontrollkästchens akzeptieren.
>

Nachdem Sie die Akustik- und Sprachmodelle ausgewählt haben, klicken Sie auf **Erstellen**. Durch diese Aktion kehren Sie zur Seite **Bereitstellungen** zurück. Die Tabelle enthält jetzt einen Eintrag, der Ihrem neuen Endpunkt entspricht. Der Status des Endpunkts stellt den aktuellen Status während seiner Erstellung dar. Es kann bis zu 30 Minuten dauern, bis ein neuer Endpunkt mit Ihren benutzerdefinierten Modellen instanziiert wurde. Wenn als Bereitstellungsstatus *Abgeschlossen* angezeigt wird, kann der Endpunkt verwendet werden.

![Die Seite „Bereitstellungen“](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Wenn die Bereitstellung bereit ist, wird der Name der Bereitstellung zu einem Link. Durch Klicken auf den Link wird die Seite **Bereitstellungsinformationen** angezeigt, die die URLs Ihres benutzerdefinierten Endpunkts für die Verwendung mit einer HTTP-Anforderung oder der Microsoft Cognitive Services-Clientbibliothek für die Spracherkennung anzeigt, die Websockets verwendet.

![Die Seite „Bereitstellungsinformationen“](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Tutorials finden Sie auf den folgenden Seiten:
* [Verwenden eines benutzerdefinierten Spracherkennungsendpunkts](cognitive-services-custom-speech-use-endpoint.md)
* [Erstellen eines benutzerdefinierten Akustikmodells](cognitive-services-custom-speech-create-acoustic-model.md)
* [Erstellen eines benutzerdefinierten Sprachmodells](cognitive-services-custom-speech-create-language-model.md)

---
title: Erstellen eines benutzerdefinierten Spracherkennungsendpunkts mit Custom Speech Service in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen benutzerdefinierten Endpunkt für die Spracherkennung mit dem Custom Speech Service in Cognitive Services erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 03/12/2018
ms.author: panosper
ms.openlocfilehash: d06dbc399a3610982c99a548373482fc5ec21f4d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852869"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Erstellen eines benutzerdefinierten Spracherkennungsendpunkts

Nachdem Sie benutzerdefinierte Akustik- oder Sprachmodelle erstellt haben, können Sie diese einem benutzerdefinierten Endpunkt für die Spracherkennung bereitstellen. 

## <a name="create-an-endpoint"></a>Erstellen eines Endpunkts
Wählen Sie zum Erstellen eines neuen benutzerdefinierten Endpunkts aus dem Menü **Custom Speech** am oberen Rand der Seite die Option **Endpunkte** aus. Durch diese Aktion gelangen Sie zur Seite **Endpunkte**, die eine Tabelle der aktuellen benutzerdefinierten Endpunkte enthält. Wenn Sie noch keine Endpunkte erstellt haben, ist die Tabelle leer. Das aktuelle Gebietsschema ist im Tabellentitel angegeben. 

Wenn Sie eine Bereitstellung für eine andere Sprache erstellen möchten, müssen Sie auf **Gebietsschema ändern** klicken. Weitere Informationen zu unterstützten Sprachen.

Klicken Sie auf **Neuen erstellen**, um einen neuen Endpunkt zu erstellen. Geben Sie im Bereich **Endpunkt erstellen** die jeweiligen Informationen in die Felder **Name** und **Beschreibung** Ihrer benutzerdefinierten Bereitstellung ein.

Wählen Sie im Kombinationsfeld **Abonnement** das gewünschte Abonnement aus. Wenn es sich um ein S0-Abonnement (gebührenpflichtig) handelt, können Sie gleichzeitige Anforderungen auswählen.

Außerdem können Sie auswählen, ob die Inhaltsprotokollierung ein- oder ausgeschaltet sein soll. Sie wählen also aus, ob der Endpunktdatenverkehr gespeichert wird. Wenn es nicht aktiviert wird, wird die Speicherung des Datenverkehrs unterdrückt. Links zum Herunterladen aller protokollierten Inhalte finden Sie unter „Endpunkt“ > „Details“.

Wählen Sie in der Liste **Akustikmodell** das gewünschte Akustikmodell und in der Liste **Sprachmodell** das gewünschte Sprachmodell aus. Die Optionen für Akustik- und Sprachmodelle umfassen immer die Microsoft-Basismodelle. Die Auswahl des Basismodells schränkt die Kombinationen ein. Konventionelle Basismodelle können nicht mit Such- und Diktatbasismodellen kombiniert werden.

> [!NOTE]
> Achten Sie darauf, dass Sie die Nutzungsbedingungen und Preisangaben durch Aktivieren des entsprechenden Kontrollkästchens akzeptieren.
>

Nachdem Sie die Akustik- und Sprachmodelle ausgewählt haben, klicken Sie auf **Erstellen**. Durch diese Aktion kehren Sie zur Seite **Bereitstellungen** zurück. Die Tabelle enthält jetzt einen Eintrag, der Ihrem neuen Endpunkt entspricht. Der Status des Endpunkts stellt den aktuellen Status während seiner Erstellung dar. Es kann bis zu 30 Minuten dauern, bis ein neuer Endpunkt mit Ihren benutzerdefinierten Modellen instanziiert wurde. Wenn als Bereitstellungsstatus *Abgeschlossen* angezeigt wird, kann der Endpunkt verwendet werden.

Wenn die Bereitstellung bereit ist, wird der Name des Endpunkts zu einem Link. Durch Klicken auf den Link wird die Seite **Endpunktinformationen** angezeigt, die die URLs Ihres benutzerdefinierten Endpunkts für die Verwendung mit einer HTTP-Anforderung oder der Microsoft Cognitive Services-Clientbibliothek für die Spracherkennung anzeigt, die Websockets verwendet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Tutorials finden Sie auf den folgenden Seiten:
- [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/)
- [Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)

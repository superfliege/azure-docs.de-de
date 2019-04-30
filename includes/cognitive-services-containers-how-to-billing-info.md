---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2019
ms.openlocfilehash: e92d1c65d9601c23e7e785f07e2de3e43ea6612b
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684649"
---
Abfragen des Containers werden mit dem Tarif der Azure-Ressource in Ressource gestellt, die für den `<ApiKey>` verwendet wird.

Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit dem Abrechnungsendpunkt verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Abrechnungsendpunkt übermitteln können. Cognitive Services-Container senden keine Kundendaten (etwa das analysierte Bild oder den analysierten Text) an Microsoft. 

### <a name="connecting-to-azure"></a>Verbindungsherstellung mit Azure

Der Container benötigt die Abrechnungsargumentwerte, um ausgeführt werden zu können. Diese Werte ermöglichen es dem Container, eine Verbindung mit dem Abrechnungsendpunkt herzustellen. Der Container meldet die Nutzung etwa alle 10 bis 15 Minuten. Wenn der Container sich nicht innerhalb des zulässigen Zeitfensters eine Verbindung mit Azure herstellt, wird der Container weiterhin ausgeführt, verarbeitet aber keine Anfragen, bis der Abrechnungsendpunkt wiederhergestellt wurde. Es erfolgen 10 Verbindungsversuche im gleichen Zeitintervall von 10 bis 15 Minuten. Wenn die Verbindung mit dem Abrechnungsendpunkt nicht innerhalb der 10 Verbindungsversuche hergestellt werden kann, wird die Ausführung des Containers beendet. 

### <a name="billing-arguments"></a>Abrechnungsargumente

Alle drei der folgenden Optionen müssen mit gültigen Werten angegeben werden, damit der Container mit dem Befehl `docker run` gestartet wird:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Cognitive Services-Ressource, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss für die bereitgestellte Ressource, die in `Billing` angegeben wurde, auf einen API-Schlüssel festgelegt werden. |
| `Billing` | Der Endpunkt der Cognitive Services-Ressource, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss auf den Endpunkt-URI einer bereitgestellten Azure-Ressource festgelegt werden.|
| `Eula` | Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf `accept` festgelegt werden. |



---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2019
ms.openlocfilehash: 94e95864d8bac2d6dc0ff690a2a8f53bd2db5a40
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522520"
---
## <a name="validate-container-is-running"></a>Überprüfung auf aktiven Container 

Es gibt mehrere Möglichkeiten, um zu überprüfen, ob der Container aktiv ist: 

|Anforderung|Zweck|
|--|--|
|`http://localhost:5000/`|Der Container stellt eine Homepage bereit.|
|`http://localhost:5000/status`|Anforderung mit GET, um zu überprüfen, ob der Container aktiv ist, ohne eine Endpunktabfrage zu verursachen. Dies kann für [Lebensdauer- und Bereitschaftstests](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) von Kubernetes verwendet werden.|
|`http://localhost:5000/swagger`|Der Container stellt eine umfassende Dokumentation für die Endpunkte sowie die Funktion `Try it now` bereit. Dieses Feature ermöglicht Ihnen die Eingabe Ihrer Einstellungen in einem webbasierten HTML-Formular, sodass Sie die Abfrage ausführen können, ohne Code schreiben zu müssen. Nach der Rückgabe der Abfrage wird ein cURL-Beispielbefehl bereitgestellt, der das erforderliche Format für HTTP-Header und -Text veranschaulicht. |

![Homepage des Containers](./media/cognitive-services-containers-api-documentation/container-webpage.png)

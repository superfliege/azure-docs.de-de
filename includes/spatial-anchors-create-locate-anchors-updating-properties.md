---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110767"
---
## <a name="update-properties"></a>Aktualisieren von Eigenschaften

Um die Eigenschaften eines Ankers zu aktualisieren, verwenden Sie die `UpdateAnchorProperties()`-Methode. Wenn zwei oder mehr Geräte gleichzeitig versuchen, die Eigenschaften für den gleichen Anker zu aktualisieren, verwenden wir ein Modell der optimistischen Nebenläufigkeit. Das bedeutet, dass der erste Schreibvorgang gewinnt.  Bei allen anderen Schreibvorgängen tritt ein Parallelitätsfehler auf: Vor einem erneuten Versuch müssen die Eigenschaften aktualisiert werden.

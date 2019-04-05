---
ms.openlocfilehash: eafe71902ee849c310847f4537214722e129ad42
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57907682"
---
## <a name="update-properties"></a>Aktualisieren von Eigenschaften

Um die Eigenschaften eines Ankers zu aktualisieren, verwenden Sie die UpdateAnchorProperties-Methode. Wenn zwei oder mehr Geräte gleichzeitig versuchen, die Eigenschaften für den gleichen Anker zu aktualisieren, verwenden wir ein Modell der optimistischen Nebenläufigkeit. Das bedeutet, dass der erste Schreibvorgang gewinnt.  Bei allen anderen Schreibvorgängen tritt ein Parallelitätsfehler auf: Vor einem erneuten Versuch müssen die Eigenschaften aktualisiert werden.

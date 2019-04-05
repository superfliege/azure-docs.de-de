---
ms.openlocfilehash: 10eb16dc1757255c77ed8967dc132f42660e070f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57907722"
---
## <a name="provide-frames-to-the-session"></a>Bereitstellen von Rahmen für die Sitzung

Bei einer Sitzung mit Raumankern wird die Umgebung zugeordnet, in der sich der Benutzer gerade befindet. Auf diese Weise lässt sich ermitteln, wo sich Anker befinden. Mobile Plattformen (iOS und Android) erfordern einen nativen Aufruf des Kamerafeeds, um Rahmen aus der AR-Bibliothek Ihrer Plattform abzurufen. Im Gegensatz dazu scannt HoloLens die Umgebung ständig, sodass kein gesonderter Aufruf wie bei mobilen Plattformen erforderlich ist.
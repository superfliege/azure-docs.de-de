---
ms.openlocfilehash: 8e02067b32d9404a5bbdf7362b2cc32712b96250
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57907706"
---
## <a name="pause-reset-or-stop-the-session"></a>Anhalten, Zurücksetzen oder Beenden einer Sitzung

Um eine Sitzung vorübergehend anzuhalten, können Sie „Stop()“ aufrufen. Damit werden alle Überwachungselemente und sämtliche Prozesse der Umgebungsverarbeitung angehalten, auch wenn Sie „ProcessFrame()“ aufrufen. Danach können Sie „Start()“ aufrufen, um die Verarbeitung fortzusetzen. Beim Fortsetzen werden alle in der Sitzung bereits erfassten Umgebungsdaten beibehalten.

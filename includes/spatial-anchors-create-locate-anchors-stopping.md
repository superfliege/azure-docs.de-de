---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631691"
---
## <a name="pause-reset-or-stop-the-session"></a>Anhalten, Zurücksetzen oder Beenden einer Sitzung

Um eine Sitzung vorübergehend anzuhalten, können Sie `Stop()` aufrufen. Damit werden alle Überwachungselemente und sämtliche Prozesse der Umgebungsverarbeitung angehalten, auch wenn Sie „ProcessFrame()“ aufrufen. Danach können Sie `Start()` aufrufen, um die Verarbeitung fortzusetzen. Beim Fortsetzen werden alle in der Sitzung bereits erfassten Umgebungsdaten beibehalten.

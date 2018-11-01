---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9bd2ee708f7d27cff5d07ca7f86d925ca6d2741d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166171"
---
<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>So wechseln Sie in den Wartungsmodus
1. Wählen Sie im Menü der seriellen Konsole Option 1 aus, d.h. die **Anmeldung mit Vollzugriff**.
2. Geben Sie das Kennwort ein. Das Standardkennwort lautet **Password1**.
3. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
     `Enter-HcsMaintenanceMode`
4. Es wird eine Warnmeldung angezeigt, die Sie darüber informiert, dass durch den Wartungsmodus alle E/A-Anforderungen unterbrochen werden und die Verbindung mit dem klassischen Azure-Portal getrennt wird. Sie werden aufgefordert, dies zu bestätigen. Geben Sie **J** ein, um in den Wartungsmodus zu wechseln.
   
    Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine andere Meldung angezeigt, die besagt, dass das Gerät in den Wartungsmodus versetzt wird.


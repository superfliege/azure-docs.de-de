---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 0e8db8779958afe1fd3cf4bf12f2a6fd4a97c61c
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164857"
---
<!--author=alkohli last changed: 9/16/15-->

#### <a name="to-cable-for-power"></a>So verkabeln Sie das Gerät für die Stromversorgung
1. Stellen Sie sicher, dass sich die Netzschalter aller Stromversorgungs- und Kühleinheiten (Power and Cooling Modules, PCMs) in Position OFF befinden.
2. Schließen Sie an jedes PCM im primären Gehäuse ein Netzkabel an.
3. Schließen Sie die Netzkabel an die Rack-PDUs an, wie in der folgenden Abbildung dargestellt. Stellen Sie sicher, dass die beiden PCMs separate Stromquellen verwenden.
   
   > [!IMPORTANT]
   > Damit die Hochverfügbarkeit des Systems sichergestellt ist, empfiehlt es sich dringend, sich strikt an das im folgenden Diagramm dargestellte Stromverkabelungsschema zu halten. 
   > 
   > 
   
    ![Stromverkabelung des 2 HE-Geräts](./media/storsimple-cable-8100-for-power/HCSCableYour2UDeviceforPower.png)
   
    **Stromverkabelung eines 8100-Geräts**
   
   | Bezeichnung | BESCHREIBUNG |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |Controller 1 |
   | 3 |Controller 0 |
   | 4 |PCM 1 |
   | 5 |PDUs |
4. Schalten Sie das System ein, indem Sie die Netzschalter beider PCMs in die Position ON bringen.


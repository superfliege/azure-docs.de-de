---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 36a014fbff30c32a7149a83907c9586bc3b2f01a
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165985"
---
<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>So installieren Sie regelmäßige Updates über Windows PowerShell für StorSimple
1. Öffnen Sie die serielle Konsole des Geräts, und wählen Sie Option 1, **Anmeldung mit Vollzugriff**, aus. Geben Sie das Kennwort ein. Das Standardkennwort lautet *Password1*. 
2. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
     `Get-HcsUpdateAvailability`
   
    Sie werden benachrichtigt, wenn Updates verfügbar sind und ob diese Updates mit oder ohne Unterbrechungen installiert werden können.
3. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
     `Start-HcsUpdate`
   
    Der Updatevorgang wird gestartet.

> [!IMPORTANT]
> * Dieser Befehl gilt nur für regelmäßige Updates. Dieser Befehl muss nur auf einem Controller ausgeführt werden, es werden jedoch beide Controller aktualisiert. 
> * Möglicherweise werden Sie während des Updatevorgangs ein Failover des Controllers bemerken. Dieses Failover wirkt sich jedoch nicht auf die Systemverfügbarkeit oder den Betrieb aus.
> 
> 


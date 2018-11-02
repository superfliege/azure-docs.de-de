---
title: Knotenaktionen für Skalierungseinheiten in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Knotenstatus anzeigen und die Knotenaktionen für das Ein- und Ausschalten, das Entladen und das Fortsetzen in einem in Azure Stack integrierten System verwenden.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: f299cca57e3eaddd2a9d4868f3bae822392e45a0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49944935"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Knotenaktionen für Skalierungseinheiten in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme*

Dieser Artikel beschreibt das Anzeigen des Status einer Skalierungseinheit und der zugehörigen Knoten und das Verwenden der verfügbaren Knotenaktionen. Knotenaktionen sind Ein-/Ausschalten, Entladen, Fortsetzen und Reparieren. Normalerweise verwenden Sie diese Knotenaktionen für den Austausch von Teilen im Betrieb oder bei Wiederherstellungsszenarien für Knoten.

> [!Important]  
> Alle in diesem Artikel beschriebenen Knotenaktionen dürfen immer nur auf einen Knoten gleichzeitig ausgerichtet sein.


## <a name="view-the-node-status"></a>Anzeigen des Knotenstatus

Im Administratorportal können Sie problemlos den Status einer Skalierungseinheit und ihrer zugehörigen Knoten anzeigen.

So zeigen Sie den Status einer Skalierungseinheit an

1. Klicken Sie auf die Kachel **Regionsverwaltung**, und wählen Sie die Region aus.
2. Wählen Sie auf der linken Seite unter **Infrastrukturressourcen** die Option **Skalierungseinheiten** aus.
3. Wählen Sie in den Ergebnissen die Skalierungseinheit aus.
 
Hier werden die folgenden Informationen angezeigt:

- Name der Region. Auf den Regionsnamen wird im PowerShell-Modul mit **-Location** verwiesen.
- Systemtyp
- Gesamtanzahl logischer Kerne
- Gesamter Speicher
- Liste der einzelnen Knoten und ihrer Status, entweder **Wird ausgeführt** oder **Beendet**

![Kachel einer Skalierungseinheit mit Status „Wird ausgeführt“ für jeden Knoten](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-node-information"></a>Anzeigen von Knoteninformationen

Wenn Sie einen Knoten auswählen, können Sie die folgenden Informationen anzeigen:

- Regionsname
- Servermodell
- IP-Adresse des Baseboard-Verwaltungscontrollers (BMC)
- Betriebszustand
- Gesamtanzahl von Kernen
- Gesamtspeichermenge
 
![Kachel einer Skalierungseinheit mit Status „Wird ausgeführt“ für jeden Knoten](media/azure-stack-node-actions/NodeActions.PNG)

Sie können von hier aus auch Knotenaktionen für eine Skalierungseinheit ausführen.

## <a name="scale-unit-node-actions"></a>Knotenaktionen für Skalierungseinheiten

Beim Anzeigen von Informationen zu einem Knoten einer Skalierungseinheit können Sie Knotenaktionen wie die folgenden ausführen:

- Entladen und Fortsetzen
- Reparieren

Der Betriebszustand des Knotens bestimmt, welche Optionen verfügbar sind.

### <a name="power-off"></a>Ausschalten

Die Aktion **Ausschalten** schaltet den Knoten aus. Dies entspricht dem Drücken des Netzschalters. Es wird **kein** Signal zum Herunterfahren an das Betriebssystem gesendet. Stellen Sie bei geplanten Ausschaltvorgängen sicher, dass Sie den Knoten der Skalierungseinheit zunächst entladen.

Diese Aktion wird normalerweise verwendet, wenn ein Knoten nicht mehr reagiert und keine Anforderungen mehr verarbeitet.

> [!Important] 
> Diese Funktionalität steht nur über PowerShell zur Verfügung. Sie wird zu einem späteren Zeitpunkt wieder im Azure Stack-Administratorportal verfügbar sein.


So führen Sie die Ausschaltaktion mithilfe von PowerShell aus

````PowerShell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```` 

Verwenden Sie in dem unwahrscheinlichen Fall, dass die Ausschaltaktion nicht funktioniert, stattdessen die BMC-Webbenutzeroberfläche.

### <a name="power-on"></a>Einschalten

Die Aktion **Einschalten** schaltet den Knoten ein. Dies entspricht dem Drücken des Netzschalters. 

> [!Important] 
> Diese Funktionalität steht nur über PowerShell zur Verfügung. Sie wird zu einem späteren Zeitpunkt wieder im Azure Stack-Administratorportal verfügbar sein.

So führen Sie die Einschaltaktion mithilfe von PowerShell aus

````PowerShell
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
````

Verwenden Sie in dem unwahrscheinlichen Fall, dass die Einschaltaktion nicht funktioniert, stattdessen die BMC-Webbenutzeroberfläche.

### <a name="drain"></a>Entladen

Die Aktion **Entladen** entfernt alle aktiven Workloads, indem diese auf die übrigen Knoten in dieser Skalierungseinheit verteilt werden.

Diese Aktion wird in der Regel für den Austausch von Teilen im Betrieb, z.B. beim Ersetzen eines vollständigen Knotens, verwendet.

> [!IMPORTANT]  
> Stellen Sie sicher, dass Sie Knoten nur während eines geplanten Wartungsfensters entladen, in dem Benutzer benachrichtigt wurden. Unter bestimmten Umständen können bei aktiven Workloads Unterbrechungen auftreten.

So führen Sie die Entladeaktion mithilfe von PowerShell aus

  ````PowerShell
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Fortfahren

Die Aktion **Fortsetzen** setzt einen entladenen Knoten fort und markiert ihn für die Verteilung von Workloads als aktiv. Für frühere Workloads, die auf dem Knoten ausgeführt wurden, wird kein Failback durchgeführt. (Wenn Sie einen Knoten entladen und dann ausschalten, wird er, wenn Sie ihn wieder einschalten, nicht als aktiv für die Workloadverteilung gekennzeichnet. Sie müssen zunächst noch die Aktion „Fortsetzen“ verwenden, um den Knoten als aktiv zu kennzeichnen.)

So führen Sie die Fortsetzungsaktion mithilfe von PowerShell aus

  ````PowerShell
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Reparieren

Die Aktion **Reparieren** repariert einen Knoten. Verwenden Sie diese Aktion nur für eines der folgenden Szenarien:

- Vollständiger Knotenaustausch (mit neuen Datenträgern oder ohne diese)
- Nach dem Ausfall oder dem Austausch einer Hardwarekomponente (sofern in der Dokumentation der FRU (Field Replaceable Unit) empfohlen).

> [!IMPORTANT]  
> Wenn Sie einen Knoten oder einzelne Hardwarekomponenten ersetzen müssen, finden Sie die genauen Schritte in der FRU-Dokumentation Ihres OEM-Hardwareherstellers. In der FRU-Dokumentation ist auch angegeben, ob Sie die Reparaturaktion nach dem Ersetzen einer Hardwarekomponente ausführen müssen.  

Beim Ausführen der Reparaturaktion müssen Sie die BMC-IP-Adresse angeben. 

So führen Sie die Reparaturaktion mithilfe von PowerShell aus

  ````PowerShell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Fabric-Administratormodul von Azure Stack finden Sie unter [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.4.0).
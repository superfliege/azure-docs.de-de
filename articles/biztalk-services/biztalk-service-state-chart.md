---
title: Zulässige Aufgaben in verschiedenen Status in BizTalk Services | Microsoft Docs
description: 'Zulässige Aktionen/Vorgänge in verschiedenen MABS-Status: Beenden, Starten, Neustarten, Anhalten, Wiederaufnehmen, Löschen, Skalieren, Aktualisieren der Konfiguration und Sichern'
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: bbe1288a42db307001ac778394ac410206f1df21
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228197"
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>BizTalk Services: eine Dienststatusübersicht

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Je nach aktuellem Status des BizTalk-Diensts stehen Vorgänge bereit, die Sie in eben diesem BizTalk-Dienst entweder ausführen können oder nicht ausführen können.

Sie stellen beispielsweise einen neuen BizTalk-Dienst bereit. Wenn dieser Vorgang erfolgreich abgeschlossen ist, befindet sich der BizTalk-Dienst im Status `active`. Im aktiven Status können Sie den BizTalk-Dienst beenden, anhalten und löschen. Wenn Sie den BizTalk-Dienst beenden und dabei ein Fehler auftritt, wechselt der BizTalk-Dienst in den Status `StopFailed`. Im Status `StopFailed` können Sie den BizTalk-Dienst neu starten. Wenn Sie versuchen, einen nicht zulässigen Vorgang auszuführen, wie etwa die Wiederaufnahme des Diensts, tritt der folgende Fehler auf:

`Operation not allowed`

## <a name="view-the-possible-states"></a>Anzeigen der möglichen Status

Die folgenden Tabellen enthalten alle Vorgänge oder Aktionen, die ausgeführt werden können, wenn sich der BizTalk-Dienst in einem spezifischen Status befindet. Ein ✔ bedeutet, dass der Vorgang in diesem Zustand zulässig ist. Ein leerer Eintrag bedeutet dagegen, dass der Vorgang nicht ausgeführt werden kann, wenn der betreffende Status zutrifft.

| Dienstzustand | Start | Beenden | Neu starten | Anhalten | Fortfahren | Löschen | Skalieren | Aktualisieren <br/> Konfiguration | Backup |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Aktiv |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Deaktiviert |  |  |  |  |  | ✔ | |  |  | 
| Ausgesetzt |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Beendet | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Dienst-Update fehlgeschlagen |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Siehe auch
* [BizTalk Services: Registerkarten „Dashboard“, „Überwachen“ und „Skalieren“](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Editionsübersicht](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Sichern und Wiederherstellen](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Drosselung](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [BizTalk Services: Name und Schlüssel des Ausstellers](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Wie verwende ich das Azure BizTalk Services SDK?](https://go.microsoft.com/fwlink/p/?LinkID=302335)

